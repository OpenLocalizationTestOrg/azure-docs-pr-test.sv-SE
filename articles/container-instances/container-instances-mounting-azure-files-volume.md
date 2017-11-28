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
# <a name="mounting-an-azure-file-share-with-azure-container-instances"></a><span data-ttu-id="cdb3a-103">Montera en filresurs som Azure med Azure Container instanser</span><span class="sxs-lookup"><span data-stu-id="cdb3a-103">Mounting an Azure file share with Azure Container Instances</span></span>

<span data-ttu-id="cdb3a-104">Som standard är Azure Behållarinstanser tillståndslösa.</span><span class="sxs-lookup"><span data-stu-id="cdb3a-104">By default, Azure Container Instances are stateless.</span></span> <span data-ttu-id="cdb3a-105">Om hello behållaren kraschar eller stoppar, förloras alla dess tillstånd.</span><span class="sxs-lookup"><span data-stu-id="cdb3a-105">If hello container crashes or stops, all of its state is lost.</span></span> <span data-ttu-id="cdb3a-106">toopersist tillstånd utöver hello livstid hello-behållare, måste du ansluta en volym från en extern butik.</span><span class="sxs-lookup"><span data-stu-id="cdb3a-106">toopersist state beyond hello lifetime of hello container, you must mount a volume from an external store.</span></span> <span data-ttu-id="cdb3a-107">Den här artikeln visar hur toomount en Azure-filresurs för användning med Azure Container instanser.</span><span class="sxs-lookup"><span data-stu-id="cdb3a-107">This article shows how toomount an Azure file share for use with Azure Container Instances.</span></span>

## <a name="create-an-azure-file-share"></a><span data-ttu-id="cdb3a-108">Skapa en Azure-filresurs</span><span class="sxs-lookup"><span data-stu-id="cdb3a-108">Create an Azure file share</span></span>

<span data-ttu-id="cdb3a-109">Innan du använder en Azure-filresursen med Azure Container instanser, måste du skapa den.</span><span class="sxs-lookup"><span data-stu-id="cdb3a-109">Before using an Azure file share with Azure Container Instances, you must create it.</span></span> <span data-ttu-id="cdb3a-110">Kör följande skript toocreate hello dela ett konto toohost hello lagringsfilresurs och hello sig själv.</span><span class="sxs-lookup"><span data-stu-id="cdb3a-110">Run hello following script toocreate a storage account toohost hello file share and hello share itself.</span></span> <span data-ttu-id="cdb3a-111">Observera att hello lagringskontonamn måste vara globalt unika, så hello skriptet lägger till en slumpmässigt värde toohello bas-sträng.</span><span class="sxs-lookup"><span data-stu-id="cdb3a-111">Note that hello storage account name must be globally unique, so hello script adds a random value toohello base string.</span></span>

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

## <a name="acquire-storage-account-access-details"></a><span data-ttu-id="cdb3a-112">Hämta information om lagringskonto åtkomst</span><span class="sxs-lookup"><span data-stu-id="cdb3a-112">Acquire storage account access details</span></span>

<span data-ttu-id="cdb3a-113">toomount en Azure-filresurs som en volym i Azure Container instanser, du behöver tre värden: hello lagringskontonamnet och hello resursnamn hello lagringsåtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="cdb3a-113">toomount an Azure file share as a volume in Azure Container Instances, you need three values: hello storage account name, hello share name, and hello storage access key.</span></span> 

<span data-ttu-id="cdb3a-114">Om du har använt hello skriptet ovan har hello lagringskontonamnet skapats med ett slumpmässigt värde hello slutet.</span><span class="sxs-lookup"><span data-stu-id="cdb3a-114">If you used hello script above, hello storage account name was created with a random value at hello end.</span></span> <span data-ttu-id="cdb3a-115">tooquery hello sista sträng (inklusive hello slumpmässiga delen), Använd hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="cdb3a-115">tooquery hello final string (including hello random portion), use hello following commands:</span></span>

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'mystorageaccount')].[name]" -o tsv)
echo $STORAGE_ACCOUNT
```

<span data-ttu-id="cdb3a-116">hello resursnamnet är redan känd (det är *acishare* i hello skriptet ovan), så att allt som återstår är hello lagringskontonyckel, som finns med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="cdb3a-116">hello share name is already known (it is *acishare* in hello script above), so all that remains is hello storage account key, which can be found using hello following command:</span></span>

```azurecli-interactive
$STORAGE_KEY=$(az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCOUNT --query "[0].value" -o tsv)
echo $STORAGE_KEY
```

## <a name="store-storage-account-access-details-with-azure-key-vault"></a><span data-ttu-id="cdb3a-117">Lagra information om lagringskonto åtkomst med Azure key vault</span><span class="sxs-lookup"><span data-stu-id="cdb3a-117">Store storage account access details with Azure key vault</span></span>

<span data-ttu-id="cdb3a-118">Lagringskontonycklar skydda åtkomst till tooyour data, så vi rekommenderar att du lagrar dem i en Azure key vault.</span><span class="sxs-lookup"><span data-stu-id="cdb3a-118">Storage account keys protect access tooyour data, so we recommend storing them in an Azure key vault.</span></span> 

<span data-ttu-id="cdb3a-119">Skapa ett nyckelvalv med hello Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="cdb3a-119">Create a key vault with hello Azure CLI:</span></span>

```azurecli-interactive
KEYVAULT_NAME=aci-keyvault
az keyvault create -n $KEYVAULT_NAME --enabled-for-template-deployment -g myResourceGroup
```

<span data-ttu-id="cdb3a-120">Hej `enabled-for-template-deployment` växeln kan Azure Resource Manager toopull hemligheter från nyckelvalvet vid tidpunkten för distribution.</span><span class="sxs-lookup"><span data-stu-id="cdb3a-120">hello `enabled-for-template-deployment` switch allows Azure Resource Manager toopull secrets from your key vault at deployment time.</span></span>

<span data-ttu-id="cdb3a-121">Lagra hello lagringskontonyckel som en ny hemlighet i nyckelvalvet hello:</span><span class="sxs-lookup"><span data-stu-id="cdb3a-121">Store hello storage account key as a new secret in hello key vault:</span></span>

```azurecli-interactive
KEYVAULT_SECRET_NAME=azurefilesstoragekey
az keyvault secret set --vault-name $KEYVAULT_NAME --name $KEYVAULT_SECRET_NAME --value $STORAGE_KEY
```

## <a name="mount-hello-volume"></a><span data-ttu-id="cdb3a-122">Montera hello volymen</span><span class="sxs-lookup"><span data-stu-id="cdb3a-122">Mount hello volume</span></span>

<span data-ttu-id="cdb3a-123">Montera en Azure-filresurs som en volym i en behållare är en tvåstegsprocess.</span><span class="sxs-lookup"><span data-stu-id="cdb3a-123">Mounting an Azure file share as a volume in a container is a two-step process.</span></span> <span data-ttu-id="cdb3a-124">Hello information hello-resursen som en del av definierar hello behållargruppen Ange först och sedan hur du vill hello volym monterade i en eller flera av hello behållare i hello-gruppen.</span><span class="sxs-lookup"><span data-stu-id="cdb3a-124">First, you provide hello details of hello share as part of defining hello container group, then you specify how you want hello volume mounted within one or more of hello containers in hello group.</span></span>

<span data-ttu-id="cdb3a-125">toodefine hello volymer som du vill använda toomake som är tillgängliga för montering, lägga till en `volumes` matrisen toohello behållare Gruppdefinition i hello Azure Resource Manager-mall och sedan refererar till dem i hello definition av enskilda hello-behållare.</span><span class="sxs-lookup"><span data-stu-id="cdb3a-125">toodefine hello volumes you want toomake available for mounting, add a `volumes` array toohello container group definition in hello Azure Resource Manager template, then reference them in hello definition of hello individual containers.</span></span>

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

<span data-ttu-id="cdb3a-126">hello mallen innehåller hello lagringskontonamn och nyckel som parametrar som kan finnas i en separat parameterfil.</span><span class="sxs-lookup"><span data-stu-id="cdb3a-126">hello template includes hello storage account name and key as parameters, which can be provided in a separate parameters file.</span></span> <span data-ttu-id="cdb3a-127">toopopulate hello parameterfilen, behöver du tre värden: hello lagringskontonamnet hello resurs-ID för din Azure key vault och hello nyckelvalv hemliga namnet som du använde toostore hello-lagringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="cdb3a-127">toopopulate hello parameters file, you will need three values: hello storage account name, hello resource ID of your Azure key vault, and hello key vault secret name that you used toostore hello storage key.</span></span> <span data-ttu-id="cdb3a-128">Om du har följt stegen kan kan du hämta dessa värden med hello följande skript:</span><span class="sxs-lookup"><span data-stu-id="cdb3a-128">If you have followed previous steps, you can get these values with hello following script:</span></span>

```azurecli-interactive
echo $STORAGE_ACCOUNT
echo $KEYVAULT_SECRET_NAME
az keyvault show --name $KEYVAULT_NAME --query [id] -o tsv
```

<span data-ttu-id="cdb3a-129">Infoga hello värden i parameterfilen hello:</span><span class="sxs-lookup"><span data-stu-id="cdb3a-129">Insert hello values into hello parameters file:</span></span>

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

## <a name="deploy-hello-container-and-manage-files"></a><span data-ttu-id="cdb3a-130">Distribuera hello behållare och hantera filer</span><span class="sxs-lookup"><span data-stu-id="cdb3a-130">Deploy hello container and manage files</span></span>

<span data-ttu-id="cdb3a-131">Du kan skapa hello behållare och montera dess volymen med hjälp av hello Azure CLI med hello-mall som har definierats.</span><span class="sxs-lookup"><span data-stu-id="cdb3a-131">With hello template defined, you can create hello container and mount its volume using hello Azure CLI.</span></span> <span data-ttu-id="cdb3a-132">Förutsatt att hello mallfilen heter *azuredeploy.json* och hello parametrar filen heter *azuredeploy.parameters.json*, och sedan hello kommandoraden är:</span><span class="sxs-lookup"><span data-stu-id="cdb3a-132">Assuming that hello template file is named *azuredeploy.json* and that hello parameters file is named *azuredeploy.parameters.json*, then hello command line is:</span></span>

```azurecli-interactive
az group deployment create --name hellofilesdeployment --template-file azuredeploy.json --parameters @azuredeploy.parameters.json --resource-group myResourceGroup
```

<span data-ttu-id="cdb3a-133">När hello behållare startar, kan du använda enkla hello webbprogram som distribuerats via hello **aci/seanmckenna-hellofiles** bilden, toohello hantera filer i hello Azure-filresursen på hello monteringssökväg som du angav.</span><span class="sxs-lookup"><span data-stu-id="cdb3a-133">Once hello container starts up, you can use hello simple web app deployed via hello **seanmckenna/aci-hellofiles** image, toohello manage files in hello Azure file share at hello mount path that you specified.</span></span> <span data-ttu-id="cdb3a-134">Hämta hello ip-adress för hello webbprogram via hello följande:</span><span class="sxs-lookup"><span data-stu-id="cdb3a-134">Obtain hello ip address for hello web app via hello following:</span></span>

```azurecli-interactive
az container show --resource-group myResourceGroup --name hellofiles -o table
```

<span data-ttu-id="cdb3a-135">Du kan använda ett verktyg som hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com) tooretrieve och inspektera hello writen toohello filen filresurs.</span><span class="sxs-lookup"><span data-stu-id="cdb3a-135">You can use a tool like hello [Microsoft Azure Storage Explorer](http://storageexplorer.com) tooretrieve and inspect hello file writen toohello file share.</span></span>

>[!NOTE]
> <span data-ttu-id="cdb3a-136">toolearn mer om med hjälp av Azure Resource Manager-mallar, parametern filer, och distribuerar med hello Azure CLI, se [distribuera resurser med Resource Manager-mallar och Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="cdb3a-136">toolearn more about using Azure Resource Manager templates, parameter files, and deploying with hello Azure CLI, see [Deploy resources with Resource Manager templates and Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cdb3a-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cdb3a-137">Next steps</span></span>

- <span data-ttu-id="cdb3a-138">Distribuera för din första behållare med hello Azure Container instanser [Snabbstart](container-instances-quickstart.md)</span><span class="sxs-lookup"><span data-stu-id="cdb3a-138">Deploy for your first container using hello Azure Container Instances [quick start](container-instances-quickstart.md)</span></span>
- <span data-ttu-id="cdb3a-139">Lär dig mer om hello [förhållandet mellan Azure Behållarinstanser och behållare orchestrators](container-instances-orchestrator-relationship.md)</span><span class="sxs-lookup"><span data-stu-id="cdb3a-139">Learn about hello [relationship between Azure Container Instances and container orchestrators](container-instances-orchestrator-relationship.md)</span></span>
