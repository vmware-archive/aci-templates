# Create a Bitnami Jenkins site on a Container Instance

Create a Jenkins site on a Container Instance

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fbitnami-labs%2Faci-templates%2Fmaster%2Fjenkins%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fbitnami-labs%2Faci-templates%2Fmaster%2Fjenkins%2Fazuredeploy.json" target="_blank">
    <img src="http://armviz.io/visualizebutton.png"/>
</a>

This template creates a container group with a Jenkins site on a Container Instance. The Jenkins site is persistently stored on an Azure Storage File Share.

`Tags: Azure Container Instance, Bitnami Jenkins`

## Solution overview and deployed resources

The following resources are deployed as part of the solution

+ **Azure Container Instance**: Azure Container Instance to host the WordPress site and the MySQL database.
+ **Azure Container Instance**: A [run-once](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-restart-policy#container-restart-policy) Azure Container Instance, where the az-cli is executed to create the file shares.
+ **Storage Account**: Storage account for the file shares to store the WordPress site content and MySQL database.
+ **File share**: Azure File shares to store Jenkins site content.

## Deployment steps

```bash
az group create --name MyResourceGroup --location eastus
az group deployment create -g MyResourceGroup --template-file azuredeploy.json
```

#### Parameters:
+ **storageAccountType**: Storage Account Type
+ **storageAccountName**: Storage Account Name
+ **jenkinsUsername**: Jenkins Admin User
+ **jenkinsPassword**: Jenkins Admin Password

#### Output:
+ **containerIP**: The Jenkins IP to access your site.
+ **containerPort**: The Jenkins Port to access your site.

## Usage

Use browser to access the site IP from deployment output once Jenkins is initialized. You can check if it's been initialized by running:

```bash
az container logs --name jenkins-container-group -g MyResourceGroup --container-name jenkins --follow
```

Wait until the line below appears:

```
INFO  jenkins successfully initialized
```

## Notes

Azure Container Instance is available in selected [locations](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-quotas#region-availability). Please use one of the available location for Azure Container Instance resource.
