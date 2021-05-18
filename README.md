# Automated AKS Upgrade

## 1.0 - Reference

- https://docs.microsoft.com/en-us/azure/aks/upgrade-cluster

## 2.0 - Possible Upgrade issues

- Incompatible APIs in the new version (review the documentations)
- Incompatible custom object

> **Note:** test the upgrade

## 3.0 - Yaml Pipeline

### 3.1 - Service connection

- Create a service connection with access to both resource groups (AKS and MC_*)

### 3.2 - Pipeline

```yaml
# Starter pipeline
# Start with a minimal pipeline that you can customize to build and deploy your code.
# Add steps that build, run tests, deploy, and more:
# https://aka.ms/yaml

trigger:
- main

variables:
  rgName: 'rg-aks-eus-demo'
  aksName: 'alemoraks'
  connection: 'superconnection'
  nextAKSversion: '1.20.5'

pool:
  vmImage: ubuntu-latest
  
steps:

- task: AzureCLI@2
  displayName: 'Upgrade AKS'
  inputs:
    azureSubscription: $(connection)
    scriptType: bash
    scriptLocation: inlineScript
    inlineScript: |
      az aks upgrade --resource-group $(rgName) --name $(aksName) --kubernetes-version $(nextAKSversion) -y
```
