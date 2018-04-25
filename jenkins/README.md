# Bitnami Jenkins Azure Container Instance

## Quickstart

Create a Jenkins site on an Azure Container Instance:

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fbitnami-labs%2Faci-templates%2Fmaster%2Fjenkins%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fbitnami-labs%2Faci-templates%2Fmaster%2Fjenkins%2Fazuredeploy.json" target="_blank">
    <img src="http://armviz.io/visualizebutton.png"/>
</a>

## Solution overview

This template creates a container group with a Jenkins site on an Azure Container Instance. The Jenkins site is persistently stored on an Azure Storage File Share.

`Tags: Azure Container Instance, Bitnami Jenkins`

## Deployed resources

The following resources are deployed as part of the solution:

+ **Azure Container Instance**: Azure Container Instance to host the Jenkins site.
+ **Azure Container Instance**: A [run-once](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-restart-policy#container-restart-policy) Azure Container Instance, where the `az` CLI is executed to create the file shares.
+ **Storage Account**: A storage account for the file shares to store Jenkins site content.
+ **File share**: Azure File shares to store Jenkins site content.

## Deployment steps

### Create a resource group and deployment

```bash
az group create --name MyResourceGroup --location eastus
az group deployment create -g MyResourceGroup --template-file azuredeploy.json
```
### Set deployment parameters

The available parameters are:

+ **storageAccountType**: Storage Account Type
+ **storageAccountName**: Storage Account Name
+ **jenkinsUsername**: Jenkins Admin User
+ **jenkinsPassword**: Jenkins Admin Password

In order to set the parameters, modify the `azuredeploy.parameters.json` file with your desired values and run the commands below:

```bash
PARAMETERS_JSON=$( cat azuredeploy.parameters.json | jq -c '.parameters' )
az group deployment create -g MyResourceGroup --template-file azuredeploy.json --parameters "$PARAMETERS_JSON"
```

The output will contain:

+ **containerURL**: The Jenkins URL to access your site.

### Validate deployment and access WordPress

Check if Jenkins has been initialized by running the command below:

```bash
az container logs --name jenkins-container-instance -g MyResourceGroup --container-name jenkins --follow
```

Wait until the line below appears:

```
INFO  jenkins successfully initialized
```

Use your browser to access the site IP address from the deployment output once Jenkins initializes.

## Notes

Azure Container Instances are available in selected [locations](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-quotas#region-availability). Please use one of the available locations for Azure Container Instance resources.
