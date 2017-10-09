---
title: "Hadoop-aaaCreate kluster med hjälp av Azure REST API - Azure | Microsoft Docs"
description: "Lär dig hur toocreate HDInsight-kluster genom att skicka Azure Resource Manager mallar toohello Azure REST API."
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
ms.openlocfilehash: 87b585e5084eccdc3d7c57483deabb4ad6e32597
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-hadoop-clusters-using-hello-azure-rest-api"></a><span data-ttu-id="980ad-103">Skapa Hadoop-kluster med hello Azure REST API</span><span class="sxs-lookup"><span data-stu-id="980ad-103">Create Hadoop clusters using hello Azure REST API</span></span>

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="980ad-104">Lär dig hur toocreate ett HDInsight-kluster med en Azure Resource Manager-mall och hello Azure REST API.</span><span class="sxs-lookup"><span data-stu-id="980ad-104">Learn how toocreate an HDInsight cluster using an Azure Resource Manager template and hello Azure REST API.</span></span>

<span data-ttu-id="980ad-105">hello Azure REST-API kan du tooperform hanteringsåtgärder på tjänster i hello Azure-plattformen, inklusive hello skapandet av nya resurser, till exempel HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="980ad-105">hello Azure REST API allows you tooperform management operations on services hosted in hello Azure platform, including hello creation of new resources such as HDInsight clusters.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="980ad-106">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="980ad-106">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="980ad-107">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="980ad-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

> [!NOTE]
> <span data-ttu-id="980ad-108">hello stegen i det här dokumentet används hello [curl (https://curl.haxx.se/)](https://curl.haxx.se/) verktyget toocommunicate med hello Azure REST API.</span><span class="sxs-lookup"><span data-stu-id="980ad-108">hello steps in this document use hello [curl (https://curl.haxx.se/)](https://curl.haxx.se/) utility toocommunicate with hello Azure REST API.</span></span>

## <a name="create-a-template"></a><span data-ttu-id="980ad-109">Skapa en mall</span><span class="sxs-lookup"><span data-stu-id="980ad-109">Create a template</span></span>

<span data-ttu-id="980ad-110">Azure Resource Manager-mallarna är JSON-dokument som beskriver en **resursgruppen** och alla resurser i den (till exempel HDInsight.) Den här metoden mallbaserade kan toodefine hello resurser som behövs för HDInsight i en mall.</span><span class="sxs-lookup"><span data-stu-id="980ad-110">Azure Resource Manager templates are JSON documents that describe a **resource group** and all resources in it (such as HDInsight.) This template-based approach allows you toodefine hello resources that you need for HDInsight in one template.</span></span>

<span data-ttu-id="980ad-111">hello följande JSON-dokumentet är en sammanslagning av hello mall och parametrar filer från [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), vilket skapar en Linux-baserade kluster med hjälp av ett lösenord toosecure hello SSH-användarkontot.</span><span class="sxs-lookup"><span data-stu-id="980ad-111">hello following JSON document is a merger of hello template and parameters files from [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), which creates a Linux-based cluster using a password toosecure hello SSH user account.</span></span>

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
                           "description": "hello type of hello HDInsight cluster toocreate."
                       }
                   },
                   "clusterName": {
                       "type": "string",
                       "metadata": {
                           "description": "hello name of hello HDInsight cluster toocreate."
                       }
                   },
                   "clusterLoginUserName": {
                       "type": "string",
                       "metadata": {
                           "description": "These credentials can be used toosubmit jobs toohello cluster and toolog into cluster dashboards."
                       }
                   },
                   "clusterLoginPassword": {
                       "type": "securestring",
                       "metadata": {
                           "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                       }
                   },
                   "sshUserName": {
                       "type": "string",
                       "metadata": {
                           "description": "These credentials can be used tooremotely access hello cluster."
                       }
                   },
                   "sshPassword": {
                       "type": "securestring",
                       "metadata": {
                           "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                       }
                   },
                   "clusterStorageAccountName": {
                       "type": "string",
                       "metadata": {
                           "description": "hello name of hello storage account toobe created and be used as hello cluster's storage."
                       }
                   },
                   "clusterWorkerNodeCount": {
                       "type": "int",
                       "defaultValue": 4,
                       "metadata": {
                           "description": "hello number of nodes in hello HDInsight cluster."
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

<span data-ttu-id="980ad-112">Det här exemplet används i hello stegen i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="980ad-112">This example is used in hello steps in this document.</span></span> <span data-ttu-id="980ad-113">Ersätt hello exempel *värden* i hello **parametrar** avsnitt med hello värden för klustret.</span><span class="sxs-lookup"><span data-stu-id="980ad-113">Replace hello example *values* in hello **Parameters** section with hello values for your cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="980ad-114">hello-mallen använder hello standardantalet arbetarnoder (4) för ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="980ad-114">hello template uses hello default number of worker nodes (4) for an HDInsight cluster.</span></span> <span data-ttu-id="980ad-115">Om du planerar på mer än 32 arbetarnoder, måste du välja en huvudnod storlek med minst 8 kärnor och 14 GB RAM-minne.</span><span class="sxs-lookup"><span data-stu-id="980ad-115">If you plan on more than 32 worker nodes, then you must select a head node size with at least 8 cores and 14 GB ram.</span></span>
>
> <span data-ttu-id="980ad-116">Mer information om noden storlekar och relaterade kostnader finns [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="980ad-116">For more information on node sizes and associated costs, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

## <a name="log-in-tooyour-azure-subscription"></a><span data-ttu-id="980ad-117">Logga in tooyour Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="980ad-117">Log in tooyour Azure subscription</span></span>

<span data-ttu-id="980ad-118">Följ stegen i hello [Kom igång med Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) och ansluta tooyour prenumeration med hjälp av hello `az login` kommando.</span><span class="sxs-lookup"><span data-stu-id="980ad-118">Follow hello steps documented in [Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) and connect tooyour subscription using hello `az login` command.</span></span>

## <a name="create-a-service-principal"></a><span data-ttu-id="980ad-119">Skapa ett huvudnamn för tjänsten</span><span class="sxs-lookup"><span data-stu-id="980ad-119">Create a service principal</span></span>

> [!NOTE]
> <span data-ttu-id="980ad-120">Här är en förkortad version av hello *skapa tjänstens huvudnamn med lösenord* avsnitt i hello [Använd Azure CLI toocreate ett huvudnamn för tjänsten tooaccess resurser](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md#create-service-principal-with-password) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="980ad-120">These steps are an abridged version of hello *Create service principal with password* section of hello [Use Azure CLI toocreate a service principal tooaccess resources](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md#create-service-principal-with-password) document.</span></span> <span data-ttu-id="980ad-121">Dessa steg att skapa ett huvudnamn för tjänsten som används tooauthenticate toohello Azure REST API.</span><span class="sxs-lookup"><span data-stu-id="980ad-121">These steps create a service principal that is used tooauthenticate toohello Azure REST API.</span></span>

1. <span data-ttu-id="980ad-122">Använd följande kommando toolist hello dina Azure-prenumerationer från en kommandorad.</span><span class="sxs-lookup"><span data-stu-id="980ad-122">From a command line, use hello following command toolist your Azure subscriptions.</span></span>

   ```bash
   az account list --query '[].{Subscription_ID:id,Tenant_ID:tenantId,Name:name}'  --output table
   ```

    <span data-ttu-id="980ad-123">Markera i hello hello prenumeration som du vill använda toouse och Observera hello **PRENUMERATIONSID** och __Tenant_ID__ kolumner.</span><span class="sxs-lookup"><span data-stu-id="980ad-123">In hello list, select hello subscription that you want toouse and note hello **Subscription_ID** and __Tenant_ID__ columns.</span></span> <span data-ttu-id="980ad-124">Spara dessa värden.</span><span class="sxs-lookup"><span data-stu-id="980ad-124">Save these values.</span></span>

2. <span data-ttu-id="980ad-125">Använd följande kommando toocreate ett program i Azure Active Directory hello.</span><span class="sxs-lookup"><span data-stu-id="980ad-125">Use hello following command toocreate an application in Azure Active Directory.</span></span>

   ```bash
   az ad app create --display-name "exampleapp" --homepage "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your password> --query 'appId'
   ```

    <span data-ttu-id="980ad-126">Ersätt hello värden för hello `--display-name`, `--homepage`, och `--identifier-uris` med egna värden.</span><span class="sxs-lookup"><span data-stu-id="980ad-126">Replace hello values for hello `--display-name`, `--homepage`, and `--identifier-uris` with your own values.</span></span> <span data-ttu-id="980ad-127">Ange ett lösenord för hello nya Active Directory-posten.</span><span class="sxs-lookup"><span data-stu-id="980ad-127">Provide a password for hello new Active Directory entry.</span></span>

   > [!NOTE]
   > <span data-ttu-id="980ad-128">Hej `--home-page` och `--identifier-uris` värden behöver inte tooreference en webbsida som finns på hello internet.</span><span class="sxs-lookup"><span data-stu-id="980ad-128">hello `--home-page` and `--identifier-uris` values don't need tooreference an actual web page hosted on hello internet.</span></span> <span data-ttu-id="980ad-129">De måste vara unikt URI: er.</span><span class="sxs-lookup"><span data-stu-id="980ad-129">They must be unique URIs.</span></span>

   <span data-ttu-id="980ad-130">hello värdet som returneras från det här kommandot är hello __App-ID__ hello nya programmet.</span><span class="sxs-lookup"><span data-stu-id="980ad-130">hello value returned from this command is hello __App ID__ for hello new application.</span></span> <span data-ttu-id="980ad-131">Spara det här värdet.</span><span class="sxs-lookup"><span data-stu-id="980ad-131">Save this value.</span></span>

3. <span data-ttu-id="980ad-132">Använd hello följande kommando toocreate ett huvudnamn för tjänsten med hjälp av hello **App-ID**.</span><span class="sxs-lookup"><span data-stu-id="980ad-132">Use hello following command toocreate a service principal using hello **App ID**.</span></span>

   ```bash
   az ad sp create --id <App ID> --query 'objectId'
   ```

     <span data-ttu-id="980ad-133">hello värdet som returneras från det här kommandot är hello __objekt-ID__.</span><span class="sxs-lookup"><span data-stu-id="980ad-133">hello value returned from this command is hello __Object ID__.</span></span> <span data-ttu-id="980ad-134">Spara det här värdet.</span><span class="sxs-lookup"><span data-stu-id="980ad-134">Save this value.</span></span>

4. <span data-ttu-id="980ad-135">Tilldela hello **ägare** rollen toohello tjänstens huvudnamn med hjälp av hello **objekt-ID** värde.</span><span class="sxs-lookup"><span data-stu-id="980ad-135">Assign hello **Owner** role toohello service principal using hello **Object ID** value.</span></span> <span data-ttu-id="980ad-136">Använd hello **prenumerations-ID** du fick tidigare.</span><span class="sxs-lookup"><span data-stu-id="980ad-136">Use hello **subscription ID** you obtained earlier.</span></span>

   ```bash
   az role assignment create --assignee <Object ID> --role Owner --scope /subscriptions/<Subscription ID>/
   ```

## <a name="get-an-authentication-token"></a><span data-ttu-id="980ad-137">Hämta en token för autentisering</span><span class="sxs-lookup"><span data-stu-id="980ad-137">Get an authentication token</span></span>

<span data-ttu-id="980ad-138">Använd följande kommando tooretrieve någon autentiseringstoken hello:</span><span class="sxs-lookup"><span data-stu-id="980ad-138">Use hello following command tooretrieve an authentication token:</span></span>

```bash
curl -X "POST" "https://login.microsoftonline.com/$TENANTID/oauth2/token" \
-H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
-H "Content-Type: application/x-www-form-urlencoded" \
--data-urlencode "client_id=$APPID" \
--data-urlencode "grant_type=client_credentials" \
--data-urlencode "client_secret=$PASSWORD" \
--data-urlencode "resource=https://management.azure.com/"
```

<span data-ttu-id="980ad-139">Ange `$TENANTID`, `$APPID`, och `$PASSWORD` toohello värden hämtas eller använt tidigare.</span><span class="sxs-lookup"><span data-stu-id="980ad-139">Set `$TENANTID`, `$APPID`, and `$PASSWORD` toohello values obtained or used previously.</span></span>

<span data-ttu-id="980ad-140">Om denna begäran lyckades, du får svar 200 serien och hello svarstexten innehåller ett JSON-dokument.</span><span class="sxs-lookup"><span data-stu-id="980ad-140">If this request is successful, you receive a 200 series response and hello response body contains a JSON document.</span></span>

<span data-ttu-id="980ad-141">hello JSON-dokument som returneras av denna begäran innehåller ett element med namnet **access_token**.</span><span class="sxs-lookup"><span data-stu-id="980ad-141">hello JSON document returned by this request contains an element named **access_token**.</span></span> <span data-ttu-id="980ad-142">Hej värdet för **access_token** är används tooauthentication begäranden toohello REST API.</span><span class="sxs-lookup"><span data-stu-id="980ad-142">hello value of **access_token** is used tooauthentication requests toohello REST API.</span></span>

```json
{
    "token_type":"Bearer",
    "expires_in":"3599",
    "expires_on":"1463409994",
    "not_before":"1463406094",
    "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
}
```

## <a name="create-a-resource-group"></a><span data-ttu-id="980ad-143">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="980ad-143">Create a resource group</span></span>

<span data-ttu-id="980ad-144">Använd hello följande toocreate en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="980ad-144">Use hello following toocreate a resource group.</span></span>

* <span data-ttu-id="980ad-145">Ange `$SUBSCRIPTIONID` toohello prenumerations-ID togs emot när hello tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="980ad-145">Set `$SUBSCRIPTIONID` toohello subscription ID received while creating hello service principal.</span></span>
* <span data-ttu-id="980ad-146">Ange `$ACCESSTOKEN` toohello åtkomst-token togs emot i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="980ad-146">Set `$ACCESSTOKEN` toohello access token received in hello previous step.</span></span>
* <span data-ttu-id="980ad-147">Ersätt `DATACENTERLOCATION` med hello Datacenter gärna toocreate hello resursgrupp och resurser i.</span><span class="sxs-lookup"><span data-stu-id="980ad-147">Replace `DATACENTERLOCATION` with hello data center you wish toocreate hello resource group, and resources, in.</span></span> <span data-ttu-id="980ad-148">Till exempel 'södra centrala USA'.</span><span class="sxs-lookup"><span data-stu-id="980ad-148">For example, 'South Central US'.</span></span>
* <span data-ttu-id="980ad-149">Ange `$RESOURCEGROUPNAME` toohello namn som du vill toouse för den här gruppen:</span><span class="sxs-lookup"><span data-stu-id="980ad-149">Set `$RESOURCEGROUPNAME` toohello name you wish toouse for this group:</span></span>

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME?api-version=2015-01-01" \
    -H "Authorization: Bearer $ACCESSTOKEN" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DATACENTERLOCATION"
}'
```

<span data-ttu-id="980ad-150">Om denna begäran lyckades, du får svar 200 serien och hello svarstexten innehåller ett JSON-dokument som innehåller information om hello grupp.</span><span class="sxs-lookup"><span data-stu-id="980ad-150">If this request is successful, you receive a 200 series response and hello response body contains a JSON document containing information about hello group.</span></span> <span data-ttu-id="980ad-151">Hej `"provisioningState"` elementet innehåller ett värde för `"Succeeded"`.</span><span class="sxs-lookup"><span data-stu-id="980ad-151">hello `"provisioningState"` element contains a value of `"Succeeded"`.</span></span>

## <a name="create-a-deployment"></a><span data-ttu-id="980ad-152">Skapa en distribution</span><span class="sxs-lookup"><span data-stu-id="980ad-152">Create a deployment</span></span>

<span data-ttu-id="980ad-153">Använd hello följande kommando toodeploy hello mallen toohello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="980ad-153">Use hello following command toodeploy hello template toohello resource group.</span></span>

* <span data-ttu-id="980ad-154">Ange `$DEPLOYMENTNAME` toohello namn som du vill toouse för den här distributionen.</span><span class="sxs-lookup"><span data-stu-id="980ad-154">Set `$DEPLOYMENTNAME` toohello name you wish toouse for this deployment.</span></span>

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json" \
-d "{set your body string toohello template and parameters}"
```

> [!NOTE]
> <span data-ttu-id="980ad-155">Om du har sparat hello mallfilen tooa du kan använda följande kommando i stället för hello `-d "{ template and parameters}"`:</span><span class="sxs-lookup"><span data-stu-id="980ad-155">If you saved hello template tooa file, you can use hello following command instead of `-d "{ template and parameters}"`:</span></span>
>
> `--data-binary "@/path/to/file.json"`

<span data-ttu-id="980ad-156">Om begäran lyckades, du får svar 200 serien och hello svarstexten innehåller ett JSON-dokument som innehåller information om hello distributionsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="980ad-156">If this request is successful, you receive a 200 series response and hello response body contains a JSON document containing information about hello deployment operation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="980ad-157">hello distributionen har skickats, men har inte slutförts.</span><span class="sxs-lookup"><span data-stu-id="980ad-157">hello deployment has been submitted, but has not completed.</span></span> <span data-ttu-id="980ad-158">Det kan ta flera minuter, vanligtvis cirka 15 för hello distribution toocomplete.</span><span class="sxs-lookup"><span data-stu-id="980ad-158">It can take several minutes, usually around 15, for hello deployment toocomplete.</span></span>

## <a name="check-hello-status-of-a-deployment"></a><span data-ttu-id="980ad-159">Hello statusen för en distribution</span><span class="sxs-lookup"><span data-stu-id="980ad-159">Check hello status of a deployment</span></span>

<span data-ttu-id="980ad-160">toocheck hello status för hello-distributionen, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="980ad-160">toocheck hello status of hello deployment, use hello following command:</span></span>

```bash
curl -X "GET" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json"
```

<span data-ttu-id="980ad-161">Det här kommandot returnerar ett JSON-dokument som innehåller information om hello distributionsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="980ad-161">This command returns a JSON document containing information about hello deployment operation.</span></span> <span data-ttu-id="980ad-162">Hej `"provisioningState"` elementet innehåller hello status för hello-distributionen.</span><span class="sxs-lookup"><span data-stu-id="980ad-162">hello `"provisioningState"` element contains hello status of hello deployment.</span></span> <span data-ttu-id="980ad-163">Om det här elementet innehåller ett värde av `"Succeeded"`, och sedan hello distributionen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="980ad-163">If this element contains a value of `"Succeeded"`, then hello deployment has completed successfully.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="980ad-164">Felsöka</span><span class="sxs-lookup"><span data-stu-id="980ad-164">Troubleshoot</span></span>

<span data-ttu-id="980ad-165">Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="980ad-165">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="980ad-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="980ad-166">Next steps</span></span>

<span data-ttu-id="980ad-167">Nu när du har skapat ett HDInsight-kluster använder du följande toolearn hur hello toowork med ditt kluster.</span><span class="sxs-lookup"><span data-stu-id="980ad-167">Now that you have successfully created an HDInsight cluster, use hello following toolearn how toowork with your cluster.</span></span>

### <a name="hadoop-clusters"></a><span data-ttu-id="980ad-168">Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="980ad-168">Hadoop clusters</span></span>

* [<span data-ttu-id="980ad-169">Använda Hive med HDInsight</span><span class="sxs-lookup"><span data-stu-id="980ad-169">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="980ad-170">Använda Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="980ad-170">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="980ad-171">Använda MapReduce med HDInsight</span><span class="sxs-lookup"><span data-stu-id="980ad-171">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a><span data-ttu-id="980ad-172">HBase-kluster</span><span class="sxs-lookup"><span data-stu-id="980ad-172">HBase clusters</span></span>

* [<span data-ttu-id="980ad-173">Kom igång med HBase på HDInsight</span><span class="sxs-lookup"><span data-stu-id="980ad-173">Get started with HBase on HDInsight</span></span>](hdinsight-hbase-tutorial-get-started-linux.md)
* [<span data-ttu-id="980ad-174">Utveckla Java-program för HBase i HDInsight</span><span class="sxs-lookup"><span data-stu-id="980ad-174">Develop Java applications for HBase on HDInsight</span></span>](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a><span data-ttu-id="980ad-175">Storm-kluster</span><span class="sxs-lookup"><span data-stu-id="980ad-175">Storm clusters</span></span>

* [<span data-ttu-id="980ad-176">Utveckla Java-topologier för Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="980ad-176">Develop Java topologies for Storm on HDInsight</span></span>](hdinsight-storm-develop-java-topology.md)
* [<span data-ttu-id="980ad-177">Använda Python komponenter i Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="980ad-177">Use Python components in Storm on HDInsight</span></span>](hdinsight-storm-develop-python-topology.md)
* [<span data-ttu-id="980ad-178">Distribuera och övervaka topologier med Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="980ad-178">Deploy and monitor topologies with Storm on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology-linux.md)
