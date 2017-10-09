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
# <a name="create-hadoop-clusters-using-hello-azure-rest-api"></a>Skapa Hadoop-kluster med hello Azure REST API

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Lär dig hur toocreate ett HDInsight-kluster med en Azure Resource Manager-mall och hello Azure REST API.

hello Azure REST-API kan du tooperform hanteringsåtgärder på tjänster i hello Azure-plattformen, inklusive hello skapandet av nya resurser, till exempel HDInsight-kluster.

> [!IMPORTANT]
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

> [!NOTE]
> hello stegen i det här dokumentet används hello [curl (https://curl.haxx.se/)](https://curl.haxx.se/) verktyget toocommunicate med hello Azure REST API.

## <a name="create-a-template"></a>Skapa en mall

Azure Resource Manager-mallarna är JSON-dokument som beskriver en **resursgruppen** och alla resurser i den (till exempel HDInsight.) Den här metoden mallbaserade kan toodefine hello resurser som behövs för HDInsight i en mall.

hello följande JSON-dokumentet är en sammanslagning av hello mall och parametrar filer från [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), vilket skapar en Linux-baserade kluster med hjälp av ett lösenord toosecure hello SSH-användarkontot.

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

Det här exemplet används i hello stegen i det här dokumentet. Ersätt hello exempel *värden* i hello **parametrar** avsnitt med hello värden för klustret.

> [!IMPORTANT]
> hello-mallen använder hello standardantalet arbetarnoder (4) för ett HDInsight-kluster. Om du planerar på mer än 32 arbetarnoder, måste du välja en huvudnod storlek med minst 8 kärnor och 14 GB RAM-minne.
>
> Mer information om noden storlekar och relaterade kostnader finns [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="log-in-tooyour-azure-subscription"></a>Logga in tooyour Azure-prenumeration

Följ stegen i hello [Kom igång med Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) och ansluta tooyour prenumeration med hjälp av hello `az login` kommando.

## <a name="create-a-service-principal"></a>Skapa ett huvudnamn för tjänsten

> [!NOTE]
> Här är en förkortad version av hello *skapa tjänstens huvudnamn med lösenord* avsnitt i hello [Använd Azure CLI toocreate ett huvudnamn för tjänsten tooaccess resurser](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md#create-service-principal-with-password) dokumentet. Dessa steg att skapa ett huvudnamn för tjänsten som används tooauthenticate toohello Azure REST API.

1. Använd följande kommando toolist hello dina Azure-prenumerationer från en kommandorad.

   ```bash
   az account list --query '[].{Subscription_ID:id,Tenant_ID:tenantId,Name:name}'  --output table
   ```

    Markera i hello hello prenumeration som du vill använda toouse och Observera hello **PRENUMERATIONSID** och __Tenant_ID__ kolumner. Spara dessa värden.

2. Använd följande kommando toocreate ett program i Azure Active Directory hello.

   ```bash
   az ad app create --display-name "exampleapp" --homepage "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your password> --query 'appId'
   ```

    Ersätt hello värden för hello `--display-name`, `--homepage`, och `--identifier-uris` med egna värden. Ange ett lösenord för hello nya Active Directory-posten.

   > [!NOTE]
   > Hej `--home-page` och `--identifier-uris` värden behöver inte tooreference en webbsida som finns på hello internet. De måste vara unikt URI: er.

   hello värdet som returneras från det här kommandot är hello __App-ID__ hello nya programmet. Spara det här värdet.

3. Använd hello följande kommando toocreate ett huvudnamn för tjänsten med hjälp av hello **App-ID**.

   ```bash
   az ad sp create --id <App ID> --query 'objectId'
   ```

     hello värdet som returneras från det här kommandot är hello __objekt-ID__. Spara det här värdet.

4. Tilldela hello **ägare** rollen toohello tjänstens huvudnamn med hjälp av hello **objekt-ID** värde. Använd hello **prenumerations-ID** du fick tidigare.

   ```bash
   az role assignment create --assignee <Object ID> --role Owner --scope /subscriptions/<Subscription ID>/
   ```

## <a name="get-an-authentication-token"></a>Hämta en token för autentisering

Använd följande kommando tooretrieve någon autentiseringstoken hello:

```bash
curl -X "POST" "https://login.microsoftonline.com/$TENANTID/oauth2/token" \
-H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
-H "Content-Type: application/x-www-form-urlencoded" \
--data-urlencode "client_id=$APPID" \
--data-urlencode "grant_type=client_credentials" \
--data-urlencode "client_secret=$PASSWORD" \
--data-urlencode "resource=https://management.azure.com/"
```

Ange `$TENANTID`, `$APPID`, och `$PASSWORD` toohello värden hämtas eller använt tidigare.

Om denna begäran lyckades, du får svar 200 serien och hello svarstexten innehåller ett JSON-dokument.

hello JSON-dokument som returneras av denna begäran innehåller ett element med namnet **access_token**. Hej värdet för **access_token** är används tooauthentication begäranden toohello REST API.

```json
{
    "token_type":"Bearer",
    "expires_in":"3599",
    "expires_on":"1463409994",
    "not_before":"1463406094",
    "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
}
```

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Använd hello följande toocreate en resursgrupp.

* Ange `$SUBSCRIPTIONID` toohello prenumerations-ID togs emot när hello tjänstens huvudnamn.
* Ange `$ACCESSTOKEN` toohello åtkomst-token togs emot i hello föregående steg.
* Ersätt `DATACENTERLOCATION` med hello Datacenter gärna toocreate hello resursgrupp och resurser i. Till exempel 'södra centrala USA'.
* Ange `$RESOURCEGROUPNAME` toohello namn som du vill toouse för den här gruppen:

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME?api-version=2015-01-01" \
    -H "Authorization: Bearer $ACCESSTOKEN" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DATACENTERLOCATION"
}'
```

Om denna begäran lyckades, du får svar 200 serien och hello svarstexten innehåller ett JSON-dokument som innehåller information om hello grupp. Hej `"provisioningState"` elementet innehåller ett värde för `"Succeeded"`.

## <a name="create-a-deployment"></a>Skapa en distribution

Använd hello följande kommando toodeploy hello mallen toohello resursgruppen.

* Ange `$DEPLOYMENTNAME` toohello namn som du vill toouse för den här distributionen.

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json" \
-d "{set your body string toohello template and parameters}"
```

> [!NOTE]
> Om du har sparat hello mallfilen tooa du kan använda följande kommando i stället för hello `-d "{ template and parameters}"`:
>
> `--data-binary "@/path/to/file.json"`

Om begäran lyckades, du får svar 200 serien och hello svarstexten innehåller ett JSON-dokument som innehåller information om hello distributionsåtgärder.

> [!IMPORTANT]
> hello distributionen har skickats, men har inte slutförts. Det kan ta flera minuter, vanligtvis cirka 15 för hello distribution toocomplete.

## <a name="check-hello-status-of-a-deployment"></a>Hello statusen för en distribution

toocheck hello status för hello-distributionen, Använd hello följande kommando:

```bash
curl -X "GET" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json"
```

Det här kommandot returnerar ett JSON-dokument som innehåller information om hello distributionsåtgärder. Hej `"provisioningState"` elementet innehåller hello status för hello-distributionen. Om det här elementet innehåller ett värde av `"Succeeded"`, och sedan hello distributionen har slutförts.

## <a name="troubleshoot"></a>Felsöka

Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Nästa steg

Nu när du har skapat ett HDInsight-kluster använder du följande toolearn hur hello toowork med ditt kluster.

### <a name="hadoop-clusters"></a>Hadoop-kluster

* [Använda Hive med HDInsight](hdinsight-use-hive.md)
* [Använda Pig med HDInsight](hdinsight-use-pig.md)
* [Använda MapReduce med HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase-kluster

* [Kom igång med HBase på HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Utveckla Java-program för HBase i HDInsight](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm-kluster

* [Utveckla Java-topologier för Storm på HDInsight](hdinsight-storm-develop-java-topology.md)
* [Använda Python komponenter i Storm på HDInsight](hdinsight-storm-develop-python-topology.md)
* [Distribuera och övervaka topologier med Storm på HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)
