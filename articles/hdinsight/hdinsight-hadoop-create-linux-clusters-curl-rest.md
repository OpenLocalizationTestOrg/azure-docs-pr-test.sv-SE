---
title: "Skapa Hadoop-kluster med hjälp av Azure REST API - Azure | Microsoft Docs"
description: "Lär dig hur du skapar HDInsight-kluster genom att skicka Azure Resource Manager-mallar REST-API: et för Azure."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 98be5893-2c6f-4dfa-95ec-d4d8b5b7dcb5
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/10/2017
ms.author: larryfr
ms.openlocfilehash: a36a41c231472ceeeb46d02ddb65549b1c79728a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-hadoop-clusters-using-the-azure-rest-api"></a><span data-ttu-id="b9481-103">Skapa Hadoop-kluster med hjälp av Azure REST API</span><span class="sxs-lookup"><span data-stu-id="b9481-103">Create Hadoop clusters using the Azure REST API</span></span>

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="b9481-104">Lär dig hur du skapar ett HDInsight-kluster med hjälp av en Azure Resource Manager-mall och Azure REST API.</span><span class="sxs-lookup"><span data-stu-id="b9481-104">Learn how to create an HDInsight cluster using an Azure Resource Manager template and the Azure REST API.</span></span>

<span data-ttu-id="b9481-105">Azure REST-API kan du utföra hanteringsåtgärder på tjänster i Azure-plattformen, inklusive skapandet av nya resurser, till exempel HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="b9481-105">The Azure REST API allows you to perform management operations on services hosted in the Azure platform, including the creation of new resources such as HDInsight clusters.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b9481-106">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="b9481-106">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="b9481-107">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="b9481-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

> [!NOTE]
> <span data-ttu-id="b9481-108">Stegen i det här dokumentet används de [curl (https://curl.haxx.se/)](https://curl.haxx.se/) verktyg för att kommunicera med Azure REST API.</span><span class="sxs-lookup"><span data-stu-id="b9481-108">The steps in this document use the [curl (https://curl.haxx.se/)](https://curl.haxx.se/) utility to communicate with the Azure REST API.</span></span>

## <a name="create-a-template"></a><span data-ttu-id="b9481-109">Skapa en mall</span><span class="sxs-lookup"><span data-stu-id="b9481-109">Create a template</span></span>

<span data-ttu-id="b9481-110">Azure Resource Manager-mallarna är JSON-dokument som beskriver en **resursgruppen** och alla resurser i den (till exempel HDInsight.) Den här metoden mallbaserade kan du definiera de resurser som du behöver för HDInsight i en mall.</span><span class="sxs-lookup"><span data-stu-id="b9481-110">Azure Resource Manager templates are JSON documents that describe a **resource group** and all resources in it (such as HDInsight.) This template-based approach allows you to define the resources that you need for HDInsight in one template.</span></span>

<span data-ttu-id="b9481-111">Följande JSON-dokumentet är en fusion mall och parametrar filer från [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), vilket skapar ett Linux-baserade kluster med ett lösenord för att skydda det SSH-kontot.</span><span class="sxs-lookup"><span data-stu-id="b9481-111">The following JSON document is a merger of the template and parameters files from [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), which creates a Linux-based cluster using a password to secure the SSH user account.</span></span>

   ```json
   {
       "properties": {
           "template": {
               "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
               "contentVersion": "1.0.0.0",
               "parameters": {
                   "clusterType": {
                       "type": "string",
                       "allowedValues": ["hadoop",
                       "hbase",
                       "storm",
                       "spark"],
                       "metadata": {
                           "description": "The type of the HDInsight cluster to create."
                       }
                   },
                   "clusterName": {
                       "type": "string",
                       "metadata": {
                           "description": "The name of the HDInsight cluster to create."
                       }
                   },
                   "clusterLoginUserName": {
                       "type": "string",
                       "metadata": {
                           "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
                       }
                   },
                   "clusterLoginPassword": {
                       "type": "securestring",
                       "metadata": {
                           "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                       }
                   },
                   "sshUserName": {
                       "type": "string",
                       "metadata": {
                           "description": "These credentials can be used to remotely access the cluster."
                       }
                   },
                   "sshPassword": {
                       "type": "securestring",
                       "metadata": {
                           "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                       }
                   },
                   "clusterStorageAccountName": {
                       "type": "string",
                       "metadata": {
                           "description": "The name of the storage account to be created and be used as the cluster's storage."
                       }
                   },
                   "clusterWorkerNodeCount": {
                       "type": "int",
                       "defaultValue": 4,
                       "metadata": {
                           "description": "The number of nodes in the HDInsight cluster."
                       }
                   }
               },
               "variables": {
                   "defaultApiVersion": "2015-05-01-preview",
                   "clusterApiVersion": "2015-03-01-preview"
               },
               "resources": [{
                   "name": "[parameters('clusterStorageAccountName')]",
                   "type": "Microsoft.Storage/storageAccounts",
                   "location": "[resourceGroup().location]",
                   "apiVersion": "[variables('defaultApiVersion')]",
                   "dependsOn": [],
                   "tags": {

                   },
                   "properties": {
                       "accountType": "Standard_LRS"
                   }
               },
               {
                   "name": "[parameters('clusterName')]",
                   "type": "Microsoft.HDInsight/clusters",
                   "location": "[resourceGroup().location]",
                   "apiVersion": "[variables('clusterApiVersion')]",
                   "dependsOn": ["[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"],
                   "tags": {

                   },
                   "properties": {
                       "clusterVersion": "3.5",
                       "osType": "Linux",
                       "clusterDefinition": {
                           "kind": "[parameters('clusterType')]",
                           "configurations": {
                               "gateway": {
                                   "restAuthCredential.isEnabled": true,
                                   "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                                   "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                               }
                           }
                       },
                       "storageProfile": {
                           "storageaccounts": [{
                               "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                               "isDefault": true,
                               "container": "[parameters('clusterName')]",
                               "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                           }]
                       },
                       "computeProfile": {
                           "roles": [{
                               "name": "headnode",
                               "targetInstanceCount": "2",
                               "hardwareProfile": {
                                   "vmSize": "Standard_D3"
                               },
                               "osProfile": {
                                   "linuxOperatingSystemProfile": {
                                       "username": "[parameters('sshUserName')]",
                                       "password": "[parameters('sshPassword')]"
                                   }
                               }
                           },
                           {
                               "name": "workernode",
                               "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                               "hardwareProfile": {
                                   "vmSize": "Standard_D3"
                               },
                               "osProfile": {
                                   "linuxOperatingSystemProfile": {
                                       "username": "[parameters('sshUserName')]",
                                       "password": "[parameters('sshPassword')]"
                                   }
                               }
                           }]
                       }
                   }
               }],
               "outputs": {
                   "cluster": {
                       "type": "object",
                       "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                   }
               }
           },
           "mode": "incremental",
           "Parameters": {
               "clusterName": {
                   "value": "newclustername"
               },
               "clusterType": {
                   "value": "hadoop"
               },
               "clusterStorageAccountName": {
                   "value": "newstoragename"
               },
               "clusterLoginUserName": {
                   "value": "admin"
               },
               "clusterLoginPassword": {
                   "value": "changeme"
               },
               "sshUserName": {
                   "value": "sshuser"
               },
               "sshPassword": {
                   "value": "changeme"
               }
           }
       }
   }
   ```

<span data-ttu-id="b9481-112">Det här exemplet används i stegen i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="b9481-112">This example is used in the steps in this document.</span></span> <span data-ttu-id="b9481-113">Ersätta exemplet *värden* i den **parametrar** avsnitt med värden för klustret.</span><span class="sxs-lookup"><span data-stu-id="b9481-113">Replace the example *values* in the **Parameters** section with the values for your cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b9481-114">Mallen använder standardvärdet för antalet arbetarnoder (4) för ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="b9481-114">The template uses the default number of worker nodes (4) for an HDInsight cluster.</span></span> <span data-ttu-id="b9481-115">Om du planerar på mer än 32 arbetarnoder, måste du välja en huvudnod storlek med minst 8 kärnor och 14 GB RAM-minne.</span><span class="sxs-lookup"><span data-stu-id="b9481-115">If you plan on more than 32 worker nodes, then you must select a head node size with at least 8 cores and 14 GB ram.</span></span>
>
> <span data-ttu-id="b9481-116">Mer information om noden storlekar och relaterade kostnader finns [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="b9481-116">For more information on node sizes and associated costs, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

## <a name="log-in-to-your-azure-subscription"></a><span data-ttu-id="b9481-117">Logga in till din Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b9481-117">Log in to your Azure subscription</span></span>

<span data-ttu-id="b9481-118">Följ stegen i [Kom igång med Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) och ansluta till din prenumeration med hjälp av den `az login` kommando.</span><span class="sxs-lookup"><span data-stu-id="b9481-118">Follow the steps documented in [Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) and connect to your subscription using the `az login` command.</span></span>

## <a name="create-a-service-principal"></a><span data-ttu-id="b9481-119">Skapa ett huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="b9481-119">Create a service principal</span></span>

> [!NOTE]
> <span data-ttu-id="b9481-120">Här är en förkortad version av den *skapa tjänstens huvudnamn med lösenord* avsnitt i den [Använd Azure CLI för att skapa ett huvudnamn för tjänsten att komma åt resurser](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md#create-service-principal-with-password) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="b9481-120">These steps are an abridged version of the *Create service principal with password* section of the [Use Azure CLI to create a service principal to access resources](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md#create-service-principal-with-password) document.</span></span> <span data-ttu-id="b9481-121">Dessa steg att skapa ett huvudnamn för tjänsten som används för att autentisera till Azure REST API.</span><span class="sxs-lookup"><span data-stu-id="b9481-121">These steps create a service principal that is used to authenticate to the Azure REST API.</span></span>

1. <span data-ttu-id="b9481-122">Använd följande kommando från en kommandorad för att visa dina Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="b9481-122">From a command line, use the following command to list your Azure subscriptions.</span></span>

   ```bash
   az account list --query '[].{Subscription_ID:id,Tenant_ID:tenantId,Name:name}'  --output table
   ```

    <span data-ttu-id="b9481-123">I listan väljer du den prenumeration som du vill använda och notera den **PRENUMERATIONSID** och __Tenant_ID__ kolumner.</span><span class="sxs-lookup"><span data-stu-id="b9481-123">In the list, select the subscription that you want to use and note the **Subscription_ID** and __Tenant_ID__ columns.</span></span> <span data-ttu-id="b9481-124">Spara dessa värden.</span><span class="sxs-lookup"><span data-stu-id="b9481-124">Save these values.</span></span>

2. <span data-ttu-id="b9481-125">Använd följande kommando för att skapa ett program i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b9481-125">Use the following command to create an application in Azure Active Directory.</span></span>

   ```bash
   az ad app create --display-name "exampleapp" --homepage "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your password> --query 'appId'
   ```

    <span data-ttu-id="b9481-126">Ersätt värdena för den `--display-name`, `--homepage`, och `--identifier-uris` med egna värden.</span><span class="sxs-lookup"><span data-stu-id="b9481-126">Replace the values for the `--display-name`, `--homepage`, and `--identifier-uris` with your own values.</span></span> <span data-ttu-id="b9481-127">Ange ett lösenord för den nya Active Directory-posten.</span><span class="sxs-lookup"><span data-stu-id="b9481-127">Provide a password for the new Active Directory entry.</span></span>

   > [!NOTE]
   > <span data-ttu-id="b9481-128">Den `--home-page` och `--identifier-uris` värden behöver inte referera till en webbsida som finns på internet.</span><span class="sxs-lookup"><span data-stu-id="b9481-128">The `--home-page` and `--identifier-uris` values don't need to reference an actual web page hosted on the internet.</span></span> <span data-ttu-id="b9481-129">De måste vara unikt URI: er.</span><span class="sxs-lookup"><span data-stu-id="b9481-129">They must be unique URIs.</span></span>

   <span data-ttu-id="b9481-130">Värdet som returneras från det här kommandot är den __App-ID__ för det nya programmet.</span><span class="sxs-lookup"><span data-stu-id="b9481-130">The value returned from this command is the __App ID__ for the new application.</span></span> <span data-ttu-id="b9481-131">Spara det här värdet.</span><span class="sxs-lookup"><span data-stu-id="b9481-131">Save this value.</span></span>

3. <span data-ttu-id="b9481-132">Använd följande kommando för att skapa ett huvudnamn för tjänsten med hjälp av den **App-ID**.</span><span class="sxs-lookup"><span data-stu-id="b9481-132">Use the following command to create a service principal using the **App ID**.</span></span>

   ```bash
   az ad sp create --id <App ID> --query 'objectId'
   ```

     <span data-ttu-id="b9481-133">Värdet som returneras från det här kommandot är den __objekt-ID__.</span><span class="sxs-lookup"><span data-stu-id="b9481-133">The value returned from this command is the __Object ID__.</span></span> <span data-ttu-id="b9481-134">Spara det här värdet.</span><span class="sxs-lookup"><span data-stu-id="b9481-134">Save this value.</span></span>

4. <span data-ttu-id="b9481-135">Tilldela den **ägare** roll till tjänstens huvudnamn med den **objekt-ID** värde.</span><span class="sxs-lookup"><span data-stu-id="b9481-135">Assign the **Owner** role to the service principal using the **Object ID** value.</span></span> <span data-ttu-id="b9481-136">Använd den **prenumerations-ID** du fick tidigare.</span><span class="sxs-lookup"><span data-stu-id="b9481-136">Use the **subscription ID** you obtained earlier.</span></span>

   ```bash
   az role assignment create --assignee <Object ID> --role Owner --scope /subscriptions/<Subscription ID>/
   ```

## <a name="get-an-authentication-token"></a><span data-ttu-id="b9481-137">Hämta en token för autentisering</span><span class="sxs-lookup"><span data-stu-id="b9481-137">Get an authentication token</span></span>

<span data-ttu-id="b9481-138">Använd följande kommando för att hämta någon autentiseringstoken:</span><span class="sxs-lookup"><span data-stu-id="b9481-138">Use the following command to retrieve an authentication token:</span></span>

```bash
curl -X "POST" "https://login.microsoftonline.com/$TENANTID/oauth2/token" \
-H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
-H "Content-Type: application/x-www-form-urlencoded" \
--data-urlencode "client_id=$APPID" \
--data-urlencode "grant_type=client_credentials" \
--data-urlencode "client_secret=$PASSWORD" \
--data-urlencode "resource=https://management.azure.com/"
```

<span data-ttu-id="b9481-139">Ange `$TENANTID`, `$APPID`, och `$PASSWORD` till värdena som anskaffats eller använts tidigare.</span><span class="sxs-lookup"><span data-stu-id="b9481-139">Set `$TENANTID`, `$APPID`, and `$PASSWORD` to the values obtained or used previously.</span></span>

<span data-ttu-id="b9481-140">Om denna begäran lyckades, du får svar 200 serien och svarstexten innehåller ett JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="b9481-140">If this request is successful, you receive a 200 series response and the response body contains a JSON document.</span></span>

<span data-ttu-id="b9481-141">JSON-dokument som returneras av denna begäran innehåller ett element med namnet **access_token**.</span><span class="sxs-lookup"><span data-stu-id="b9481-141">The JSON document returned by this request contains an element named **access_token**.</span></span> <span data-ttu-id="b9481-142">Värdet för **access_token** används för att autentiseringsbegäranden till REST API.</span><span class="sxs-lookup"><span data-stu-id="b9481-142">The value of **access_token** is used to authentication requests to the REST API.</span></span>

```json
{
    "token_type":"Bearer",
    "expires_in":"3599",
    "expires_on":"1463409994",
    "not_before":"1463406094",
    "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
}
```

## <a name="create-a-resource-group"></a><span data-ttu-id="b9481-143">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="b9481-143">Create a resource group</span></span>

<span data-ttu-id="b9481-144">Använd följande för att skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="b9481-144">Use the following to create a resource group.</span></span>

* <span data-ttu-id="b9481-145">Ange `$SUBSCRIPTIONID` till prenumerationen ID togs emot när tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="b9481-145">Set `$SUBSCRIPTIONID` to the subscription ID received while creating the service principal.</span></span>
* <span data-ttu-id="b9481-146">Ange `$ACCESSTOKEN` till åtkomsttoken som togs emot i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="b9481-146">Set `$ACCESSTOKEN` to the access token received in the previous step.</span></span>
* <span data-ttu-id="b9481-147">Ersätt `DATACENTERLOCATION` med du vill skapa resursgrupp och resurser i datacentret.</span><span class="sxs-lookup"><span data-stu-id="b9481-147">Replace `DATACENTERLOCATION` with the data center you wish to create the resource group, and resources, in.</span></span> <span data-ttu-id="b9481-148">Till exempel 'södra centrala USA'.</span><span class="sxs-lookup"><span data-stu-id="b9481-148">For example, 'South Central US'.</span></span>
* <span data-ttu-id="b9481-149">Ange `$RESOURCEGROUPNAME` till namn som du vill använda för den här gruppen:</span><span class="sxs-lookup"><span data-stu-id="b9481-149">Set `$RESOURCEGROUPNAME` to the name you wish to use for this group:</span></span>

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME?api-version=2015-01-01" \
    -H "Authorization: Bearer $ACCESSTOKEN" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DATACENTERLOCATION"
}'
```

<span data-ttu-id="b9481-150">Om denna begäran lyckades, du får svar 200 serien och svarstexten innehåller ett JSON-dokument som innehåller information om gruppen.</span><span class="sxs-lookup"><span data-stu-id="b9481-150">If this request is successful, you receive a 200 series response and the response body contains a JSON document containing information about the group.</span></span> <span data-ttu-id="b9481-151">Den `"provisioningState"` elementet innehåller ett värde för `"Succeeded"`.</span><span class="sxs-lookup"><span data-stu-id="b9481-151">The `"provisioningState"` element contains a value of `"Succeeded"`.</span></span>

## <a name="create-a-deployment"></a><span data-ttu-id="b9481-152">Skapa en distribution</span><span class="sxs-lookup"><span data-stu-id="b9481-152">Create a deployment</span></span>

<span data-ttu-id="b9481-153">Använd följande kommando för att distribuera mallen till resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="b9481-153">Use the following command to deploy the template to the resource group.</span></span>

* <span data-ttu-id="b9481-154">Ange `$DEPLOYMENTNAME` till namn som du vill använda för den här distributionen.</span><span class="sxs-lookup"><span data-stu-id="b9481-154">Set `$DEPLOYMENTNAME` to the name you wish to use for this deployment.</span></span>

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json" \
-d "{set your body string to the template and parameters}"
```

> [!NOTE]
> <span data-ttu-id="b9481-155">Om du har sparat mallen till en fil kan du använda följande kommando i stället för `-d "{ template and parameters}"`:</span><span class="sxs-lookup"><span data-stu-id="b9481-155">If you saved the template to a file, you can use the following command instead of `-d "{ template and parameters}"`:</span></span>
>
> `--data-binary "@/path/to/file.json"`

<span data-ttu-id="b9481-156">Om denna begäran lyckades, du får svar 200 serien och svarstexten innehåller ett JSON-dokument som innehåller information om distributionen.</span><span class="sxs-lookup"><span data-stu-id="b9481-156">If this request is successful, you receive a 200 series response and the response body contains a JSON document containing information about the deployment operation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b9481-157">Distributionen har skickats, men har inte slutförts.</span><span class="sxs-lookup"><span data-stu-id="b9481-157">The deployment has been submitted, but has not completed.</span></span> <span data-ttu-id="b9481-158">Det kan ta flera minuter, vanligtvis cirka 15, för att distributionen ska slutföras.</span><span class="sxs-lookup"><span data-stu-id="b9481-158">It can take several minutes, usually around 15, for the deployment to complete.</span></span>

## <a name="check-the-status-of-a-deployment"></a><span data-ttu-id="b9481-159">Kontrollera status för en distribution</span><span class="sxs-lookup"><span data-stu-id="b9481-159">Check the status of a deployment</span></span>

<span data-ttu-id="b9481-160">Om du vill kontrollera status för distributionen, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="b9481-160">To check the status of the deployment, use the following command:</span></span>

```bash
curl -X "GET" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json"
```

<span data-ttu-id="b9481-161">Det här kommandot returnerar ett JSON-dokument som innehåller information om distributionen.</span><span class="sxs-lookup"><span data-stu-id="b9481-161">This command returns a JSON document containing information about the deployment operation.</span></span> <span data-ttu-id="b9481-162">Den `"provisioningState"` elementet innehåller statusen för distributionen.</span><span class="sxs-lookup"><span data-stu-id="b9481-162">The `"provisioningState"` element contains the status of the deployment.</span></span> <span data-ttu-id="b9481-163">Om det här elementet innehåller ett värde av `"Succeeded"`, och sedan distributionen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="b9481-163">If this element contains a value of `"Succeeded"`, then the deployment has completed successfully.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="b9481-164">Felsöka</span><span class="sxs-lookup"><span data-stu-id="b9481-164">Troubleshoot</span></span>

<span data-ttu-id="b9481-165">Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="b9481-165">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9481-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b9481-166">Next steps</span></span>

<span data-ttu-id="b9481-167">Nu när du har skapat ett HDInsight-kluster, kan du använda följande för att lära dig hur du arbetar med ditt kluster.</span><span class="sxs-lookup"><span data-stu-id="b9481-167">Now that you have successfully created an HDInsight cluster, use the following to learn how to work with your cluster.</span></span>

### <a name="hadoop-clusters"></a><span data-ttu-id="b9481-168">Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="b9481-168">Hadoop clusters</span></span>

* [<span data-ttu-id="b9481-169">Använda Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="b9481-169">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="b9481-170">Använda Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="b9481-170">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="b9481-171">Använda MapReduce med HDInsight</span><span class="sxs-lookup"><span data-stu-id="b9481-171">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a><span data-ttu-id="b9481-172">HBase-kluster</span><span class="sxs-lookup"><span data-stu-id="b9481-172">HBase clusters</span></span>

* [<span data-ttu-id="b9481-173">Kom igång med HBase på HDInsight</span><span class="sxs-lookup"><span data-stu-id="b9481-173">Get started with HBase on HDInsight</span></span>](hdinsight-hbase-tutorial-get-started-linux.md)
* [<span data-ttu-id="b9481-174">Utveckla Java-program för HBase i HDInsight</span><span class="sxs-lookup"><span data-stu-id="b9481-174">Develop Java applications for HBase on HDInsight</span></span>](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a><span data-ttu-id="b9481-175">Storm-kluster</span><span class="sxs-lookup"><span data-stu-id="b9481-175">Storm clusters</span></span>

* [<span data-ttu-id="b9481-176">Utveckla Java-topologier för Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="b9481-176">Develop Java topologies for Storm on HDInsight</span></span>](hdinsight-storm-develop-java-topology.md)
* [<span data-ttu-id="b9481-177">Använda Python komponenter i Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="b9481-177">Use Python components in Storm on HDInsight</span></span>](hdinsight-storm-develop-python-topology.md)
* [<span data-ttu-id="b9481-178">Distribuera och övervaka topologier med Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="b9481-178">Deploy and monitor topologies with Storm on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology-linux.md)
