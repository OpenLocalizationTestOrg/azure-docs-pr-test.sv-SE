---
title: "aaaMounting en volym för Azure-filer i Azure Container instanser"
description: "Lär dig hur toomount en Azure filer volym toopersist tillstånd med Azure Container instanser"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: d87215e06d5e5af40bfebcad17768ee45ccabbb2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="mounting-an-azure-file-share-with-azure-container-instances"></a>Montera en filresurs som Azure med Azure Container instanser

Som standard är Azure Behållarinstanser tillståndslösa. Om hello behållaren kraschar eller stoppar, förloras alla dess tillstånd. toopersist tillstånd utöver hello livstid hello-behållare, måste du ansluta en volym från en extern butik. Den här artikeln visar hur toomount en Azure-filresurs för användning med Azure Container instanser.

## <a name="create-an-azure-file-share"></a>Skapa en Azure-filresurs

Innan du använder en Azure-filresursen med Azure Container instanser, måste du skapa den. Kör följande skript toocreate hello dela ett konto toohost hello lagringsfilresurs och hello sig själv. Observera att hello lagringskontonamn måste vara globalt unika, så hello skriptet lägger till en slumpmässigt värde toohello bas-sträng.

```azurecli-interactive
# Change these four parameters
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create hello storage account with hello parameters
az storage account create -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -l $ACI_PERS_LOCATION --sku Standard_LRS

# Export hello connection string as an environment variable, this is used when creating hello Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -o tsv`

# Create hello share
az storage share create -n $ACI_PERS_SHARE_NAME
```

## <a name="acquire-storage-account-access-details"></a>Hämta information om lagringskonto åtkomst

toomount en Azure-filresurs som en volym i Azure Container instanser, du behöver tre värden: hello lagringskontonamnet och hello resursnamn hello lagringsåtkomstnyckel. 

Om du har använt hello skriptet ovan har hello lagringskontonamnet skapats med ett slumpmässigt värde hello slutet. tooquery hello sista sträng (inklusive hello slumpmässiga delen), Använd hello följande kommandon:

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'mystorageaccount')].[name]" -o tsv)
echo $STORAGE_ACCOUNT
```

hello resursnamnet är redan känd (det är *acishare* i hello skriptet ovan), så att allt som återstår är hello lagringskontonyckel, som finns med hello följande kommando:

```azurecli-interactive
$STORAGE_KEY=$(az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCOUNT --query "[0].value" -o tsv)
echo $STORAGE_KEY
```

## <a name="store-storage-account-access-details-with-azure-key-vault"></a>Lagra information om lagringskonto åtkomst med Azure key vault

Lagringskontonycklar skydda åtkomst till tooyour data, så vi rekommenderar att du lagrar dem i en Azure key vault. 

Skapa ett nyckelvalv med hello Azure CLI:

```azurecli-interactive
KEYVAULT_NAME=aci-keyvault
az keyvault create -n $KEYVAULT_NAME --enabled-for-template-deployment -g myResourceGroup
```

Hej `enabled-for-template-deployment` växeln kan Azure Resource Manager toopull hemligheter från nyckelvalvet vid tidpunkten för distribution.

Lagra hello lagringskontonyckel som en ny hemlighet i nyckelvalvet hello:

```azurecli-interactive
KEYVAULT_SECRET_NAME=azurefilesstoragekey
az keyvault secret set --vault-name $KEYVAULT_NAME --name $KEYVAULT_SECRET_NAME --value $STORAGE_KEY
```

## <a name="mount-hello-volume"></a>Montera hello volymen

Montera en Azure-filresurs som en volym i en behållare är en tvåstegsprocess. Hello information hello-resursen som en del av definierar hello behållargruppen Ange först och sedan hur du vill hello volym monterade i en eller flera av hello behållare i hello-gruppen.

toodefine hello volymer som du vill använda toomake som är tillgängliga för montering, lägga till en `volumes` matrisen toohello behållare Gruppdefinition i hello Azure Resource Manager-mall och sedan refererar till dem i hello definition av enskilda hello-behållare.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageaccountname": {
      "type": "string"
    },
    "storageaccountkey": {
      "type": "securestring"
    }
  },
  "resources":[{
    "name": "hellofiles",
    "type": "Microsoft.ContainerInstance/containerGroups",
    "apiVersion": "2017-08-01-preview",
    "location": "[resourceGroup().location]",
    "properties": {
      "containers": [{
        "name": "hellofiles",
        "properties": {
          "image": "seanmckenna/aci-hellofiles",
          "resources": {
            "requests": {
              "cpu": 1,
              "memoryInGb": 1.5
            }
          },
          "ports": [{
            "port": 80
          }],
          "volumeMounts": [{
            "name": "myvolume",
            "mountPath": "/aci/logs/"
          }]
        }  
      }],
      "osType": "Linux",
      "ipAddress": {
        "type": "Public",
        "ports": [{
          "protocol": "tcp",
          "port": "80"
        }]
      },
      "volumes": [{
        "name": "myvolume",
        "azureFile": {
          "shareName": "acishare",
          "storageAccountName": "[parameters('storageaccountname')]",
          "storageAccountKey": "[parameters('storageaccountkey')]"
        }
      }]
    }
  }]
}
```

hello mallen innehåller hello lagringskontonamn och nyckel som parametrar som kan finnas i en separat parameterfil. toopopulate hello parameterfilen, behöver du tre värden: hello lagringskontonamnet hello resurs-ID för din Azure key vault och hello nyckelvalv hemliga namnet som du använde toostore hello-lagringsnyckel. Om du har följt stegen kan kan du hämta dessa värden med hello följande skript:

```azurecli-interactive
echo $STORAGE_ACCOUNT
echo $KEYVAULT_SECRET_NAME
az keyvault show --name $KEYVAULT_NAME --query [id] -o tsv
```

Infoga hello värden i parameterfilen hello:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageaccountname": {
      "value": "<my_storage_account_name>"
    },    
   "storageaccountkey": {
      "reference": {
        "keyVault": {
          "id": "<my_keyvault_id>"
        },
        "secretName": "<my_storage_account_key_secret_name>"
      }
    }
  }
}
```

## <a name="deploy-hello-container-and-manage-files"></a>Distribuera hello behållare och hantera filer

Du kan skapa hello behållare och montera dess volymen med hjälp av hello Azure CLI med hello-mall som har definierats. Förutsatt att hello mallfilen heter *azuredeploy.json* och hello parametrar filen heter *azuredeploy.parameters.json*, och sedan hello kommandoraden är:

```azurecli-interactive
az group deployment create --name hellofilesdeployment --template-file azuredeploy.json --parameters @azuredeploy.parameters.json --resource-group myResourceGroup
```

När hello behållare startar, kan du använda enkla hello webbprogram som distribuerats via hello **aci/seanmckenna-hellofiles** bilden, toohello hantera filer i hello Azure-filresursen på hello monteringssökväg som du angav. Hämta hello ip-adress för hello webbprogram via hello följande:

```azurecli-interactive
az container show --resource-group myResourceGroup --name hellofiles -o table
```

Du kan använda ett verktyg som hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com) tooretrieve och inspektera hello writen toohello filen filresurs.

>[!NOTE]
> toolearn mer om med hjälp av Azure Resource Manager-mallar, parametern filer, och distribuerar med hello Azure CLI, se [distribuera resurser med Resource Manager-mallar och Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md).

## <a name="next-steps"></a>Nästa steg

- Distribuera för din första behållare med hello Azure Container instanser [Snabbstart](container-instances-quickstart.md)
- Lär dig mer om hello [förhållandet mellan Azure Behållarinstanser och behållare orchestrators](container-instances-orchestrator-relationship.md)
