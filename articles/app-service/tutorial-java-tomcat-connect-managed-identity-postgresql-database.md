---
title: 'Tutorial: Access data with managed identity in Java'
description: Secure Azure Database for PostgreSQL connectivity with managed identity from a sample Java Tomcat app, and apply it to other Azure services.
ms.devlang: java
ms.topic: tutorial
ms.date: 09/26/2022
author: KarlErickson
ms.author: karler
ms.custom: passwordless-java, service-connector, devx-track-azurecli, devx-track-extended-java
---

# Tutorial: Connect to a PostgreSQL Database from Java Tomcat App Service without secrets using a managed identity

[Azure App Service](overview.md) provides a highly scalable, self-patching web hosting service in Azure. It also provides a [managed identity](overview-managed-identity.md) for your app, which is a turn-key solution for securing access to [Azure Database for PostgreSQL](../postgresql/index.yml) and other Azure services. Managed identities in App Service make your app more secure by eliminating secrets from your app, such as credentials in the environment variables. In this tutorial, you will learn how to:

> [!div class="checklist"]
> * Create a PostgreSQL database.
> * Deploy the sample app to Azure App Service on Tomcat using WAR packaging.
> * Configure a Spring Boot web application to use Azure AD authentication with PostgreSQL Database.
> * Connect to PostgreSQL Database with Managed Identity using Service Connector.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## Prerequisites

* [Git](https://git-scm.com/)
* [Java JDK](/azure/developer/java/fundamentals/java-support-on-azure)
* [Maven](https://maven.apache.org)
* [Azure CLI](/cli/azure/install-azure-cli) version 2.45.0 or higher.

## Clone the sample app and prepare the repo

Run the following commands in your terminal to clone the sample repo and set up the sample app environment.

```bash
git clone https://github.com/Azure-Samples/Passwordless-Connections-for-Java-Apps
cd Passwordless-Connections-for-Java-Apps/Tomcat/
```

## Create an Azure Database for PostgreSQL

Follow these steps to create an Azure Database for Postgres in your subscription. The Spring Boot app will connect to this database and store its data when running, persisting the application state no matter where you run the application.

1. Sign into the Azure CLI, and optionally set your subscription if you have more than one connected to your login credentials.

   ```azurecli-interactive
   az login
   az account set --subscription <subscription-ID>
   ```

1. Create an Azure Resource Group, noting the resource group name.

   ```azurecli-interactive
   RESOURCE_GROUP=<resource-group-name>
   LOCATION=eastus

   az group create --name $RESOURCE_GROUP --location $LOCATION
   ```

1. Create an Azure Database for PostgreSQL server. The server is created with an administrator account, but it won't be used because we'll use the Azure Active Directory (Azure AD) admin account to perform administrative tasks.

   ### [Flexible Server](#tab/flexible)

   ```azurecli-interactive
   POSTGRESQL_ADMIN_USER=azureuser
   # PostgreSQL admin access rights won't be used because Azure AD authentication is leveraged to administer the database.
   POSTGRESQL_ADMIN_PASSWORD=<admin-password>
   POSTGRESQL_HOST=<postgresql-host-name>

   # Create a PostgreSQL server.
   az postgres flexible-server create \
       --resource-group $RESOURCE_GROUP \
       --name $POSTGRESQL_HOST \
       --location $LOCATION \
       --admin-user $POSTGRESQL_ADMIN_USER \
       --admin-password $POSTGRESQL_ADMIN_PASSWORD \
       --public-access 0.0.0.0 \
       --sku-name Standard_D2s_v3 
   ```

   ### [Single Server](#tab/single)

   ```azurecli-interactive
   POSTGRESQL_ADMIN_USER=azureuser
   # PostgreSQL admin access rights won't be used because Azure AD authentication is leveraged to administer the database.
   POSTGRESQL_ADMIN_PASSWORD=<admin-password>
   POSTGRESQL_HOST=<postgresql-host-name>

   # Create a PostgreSQL server.
   az postgres server create \
       --resource-group $RESOURCE_GROUP \
       --name $POSTGRESQL_HOST \
       --location $LOCATION \
       --admin-user $POSTGRESQL_ADMIN_USER \
       --admin-password $POSTGRESQL_ADMIN_PASSWORD \
       --public-network-access 0.0.0.0 \
       --sku-name B_Gen5_1 
   ```

1. Create a database for the application.

   ### [Flexible Server](#tab/flexible)

   ```azurecli-interactive
   DATABASE_NAME=checklist

   az postgres flexible-server db create \
       --resource-group $RESOURCE_GROUP \
       --server-name $POSTGRESQL_HOST \
       --database-name $DATABASE_NAME
   ```

   ### [Single Server](#tab/single)

   ```azurecli-interactive
   DATABASE_NAME=checklist

   az postgres db create \
       --resource-group $RESOURCE_GROUP \
       --server-name $POSTGRESQL_HOST \
       --name $DATABASE_NAME
   ```

## Deploy the application to App Service

Follow these steps to build a WAR file and deploy to Azure App Service on Tomcat using a WAR packaging.

1. The sample app contains a *pom.xml* file that can generate the WAR file. Run the following command to build the app.

   ```bash
   mvn clean package -f pom.xml
   ```

1. Create an Azure App Service resource on Linux using Tomcat 9.0.

   ```azurecli-interactive
   APPSERVICE_PLAN=<app-service-plan>
   APPSERVICE_NAME=<app-service-name>
   # Create an App Service plan
   az appservice plan create \
       --resource-group $RESOURCE_GROUP \
       --name $APPSERVICE_PLAN \
       --location $LOCATION \
       --sku B1 \
       --is-linux

   # Create an App Service resource.
   az webapp create \
       --resource-group $RESOURCE_GROUP \
       --name $APPSERVICE_NAME \
       --plan $APPSERVICE_PLAN \
       --runtime "TOMCAT:9.0-jre8" 
   ```

1. Deploy the WAR package to App Service.

   ```azurecli-interactive
   az webapp deploy \
       --resource-group $RESOURCE_GROUP \
       --name $APPSERVICE_NAME \
       --src-path target/app.war \
       --type war
   ```

## Connect the Postgres database with identity connectivity

Next, connect the database using [Service Connector](../service-connector/overview.md).

Install the Service Connector passwordless extension for the Azure CLI:

```azurecli
az extension add --name serviceconnector-passwordless --upgrade
```

Then, connect your app to a Postgres database with a system-assigned managed identity using Service Connector.

### [Flexible Server](#tab/flexible)

To do this, run the [az webapp connection create](/cli/azure/webapp/connection/create#az-webapp-connection-create-postgres-flexible) command.

```azurecli-interactive
az webapp connection create postgres-flexible \
    --resource-group $RESOURCE_GROUP \
    --name $APPSERVICE_NAME \
    --target-resource-group $RESOURCE_GROUP \
    --server $POSTGRESQL_HOST \
    --database $DATABASE_NAME \
    --system-identity
```

### [Single Server](#tab/single)

To do this, run the [az webapp connection create](/cli/azure/webapp/connection/create#az-webapp-connection-create-postgres) command.

```azurecli-interactive
az webapp connection create postgres \
    --resource-group $RESOURCE_GROUP \
    --name $APPSERVICE_NAME \
    --target-resource-group $RESOURCE_GROUP \
    --server $POSTGRESQL_HOST \
    --database $DATABASE_NAME \
    --system-identity
```

---
This command creates a connection between your web app and your PostgreSQL server, and manages authentication through a system-assigned managed identity.

## View sample web app

Run the following command to open the deployed web app in your browser.

```azurecli-interactive
az webapp browse \
    --resource-group $RESOURCE_GROUP \
    --name MyWebapp \
    --name $APPSERVICE_NAME
```

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## Next steps

Learn more about running Java apps on App Service on Linux in the developer guide.

> [!div class="nextstepaction"]
> [Java in App Service Linux dev guide](configure-language-java.md?pivots=platform-linux)

Learn how to secure your app with a custom domain and certificate.

> [!div class="nextstepaction"]
> [Secure with custom domain and certificate](tutorial-secure-domain-certificate.md)
