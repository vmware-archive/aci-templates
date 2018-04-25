# Create a Bitnami WordPress site on a Container Instance

Create a WordPress site and a MySQL database on Azure Container Instances (ACI)

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fbitnami-labs%2Faci-templates%2Fmaster%2Fwordpress%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Fbitnami-labs%2Faci-templates%2Fmaster%2Fwordpress%2Fazuredeploy.json" target="_blank">
    <img src="http://armviz.io/visualizebutton.png"/>
</a>

This template creates a container group with a WordPress website and a MySQL database on a Container Instance. The WordPress site and MySQL database are persistently stored on an Azure Storage File Share.

`Tags: Azure Container Instance, Bitnami WordPress`

## Solution overview and deployed resources

The following resources are deployed as part of the solution

+ **Azure Container Instance**: Azure Container Instance to host the WordPress site and the MySQL database.
+ **Azure Container Instance**: A [run-once](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-restart-policy#container-restart-policy) Azure Container Instance, where the az-cli is executed to create the file shares.
+ **Storage Account**: Storage account for the file shares to store the WordPress site content and MySQL database.
+ **File share**: Azure File shares to store WordPress site content and the MySQL database.

## Deployment steps

```bash
az group create --name MyResourceGroup --location eastus
az group deployment create -g MyResourceGroup --template-file azuredeploy.json
```

#### Parameters:

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

In order to set the parameters, modify the *azuredeploy.parameters.json* with your desired values and run the commands below:

```bash
PARAMETERS_JSON=$( cat azuredeploy.parameters.json | jq -c '.parameters' )
az group deployment create -g MyResourceGroup --template-file azuredeploy.json --parameters "$PARAMETERS_JSON"
```

#### Output:
+ **containerURL**: The WordPress URL to access your site.

## Usage

Use the browser to access the WordPress site IP from the deployment output once WP is initialized. You can check if it has been initialized by running the command below:

```bash
az container logs --name wordpress-container-instance -g MyResourceGroup --container-name wordpress --follow
```

Wait until the line below appears:

```
INFO  wordpress successfully initialized
```

## Notes

Azure Container Instance is available in selected [locations](https://docs.microsoft.com/en-us/azure/container-instances/container-instances-quotas#region-availability). Please use one of the available location for Azure Container Instance resource.
