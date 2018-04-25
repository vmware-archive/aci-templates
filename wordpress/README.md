# Bitnami WordPress Azure Container Instance

## Quickstart

Create a WordPress website and a MySQL database on Azure Container Instances (ACI):

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fbitnami-labs%2Faci-templates%2Fmaster%2Fwordpress%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fbitnami-labs%2Faci-templates%2Fmaster%2Fwordpress%2Fazuredeploy.json" target="_blank">
    <img src="http://armviz.io/visualizebutton.png"/>
</a>

## Solution overview

This template creates a container group with a WordPress website and a MySQL database on an Azure Container Instance. The WordPress website and MySQL database are persistently stored on Azure File Storage.

`Tags: Azure Container Instance, Bitnami WordPress`

## Deployed resources

The following resources are deployed as part of the solution:

+ **Azure Container Instance**: Azure Container Instance to host the WordPress site and the MySQL database.
+ **Azure Container Instance**: A [run-once](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-restart-policy#container-restart-policy) Azure Container Instance, where the `az-cli` is executed to create the file shares.
+ **Storage Account**: A storage account for the Azure File shares to store the WordPress website content and MySQL database.
+ **File share**: Azure File shares to store the WordPress website content and the MySQL database.

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
+ **wordpressBlogName**: WordPress Blog Name
+ **wordpressUsername**: WordPress Admin User Name
+ **wordpressPassword**: WordPress Admin Password
+ **wordpressEmail**: WordPress admin e-mail
+ **wordpressFirstName**: WordPress Admin First Name
+ **wordpressLastName**: WordPress Admin Last Name
+ **databaseUser**: WordPress' Database User
+ **databaseUser**: WordPress' Database Name

In order to set the parameters, modify the `azuredeploy.parameters.json` file with your desired values and run the commands below:

```bash
PARAMETERS_JSON=$( cat azuredeploy.parameters.json | jq -c '.parameters' )
az group deployment create -g MyResourceGroup --template-file azuredeploy.json --parameters "$PARAMETERS_JSON"
```

The output will contain:

+ **containerURL**: The WordPress URL to access your site.

### Validate deployment and access WordPress

Check if WordPress has been initialized by running the command below:

```bash
az container logs --name wordpress-container-instance -g MyResourceGroup --container-name wordpress --follow
```

Wait until the line below appears:

```
INFO  wordpress successfully initialized
```

Use the browser to access the WordPress website IP address from the deployment output once WordPress is initialized. 

## Notes

Azure Container Instances are available in selected [locations](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-quotas#region-availability). Please use one of the available locations for Azure Container Instance resources.
