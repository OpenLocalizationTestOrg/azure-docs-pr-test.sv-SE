---
title: "aaaCreate privata Docker behållare registret - Azure CLI | Microsoft Docs"
description: "Börja skapa och hantera privata Docker behållare register med hello Azure CLI 2.0"
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: na
tags: 
keywords: 
ms.assetid: 29e20d75-bf39-4f7d-815f-a2e47209be7d
ms.service: container-registry
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: nepeters
ms.custom: na
ms.openlocfilehash: 2cadf42db0681a09c95486510f1e65c6f87c5280
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-container-registry-using-hello-azure-cli"></a>Skapa en hanterad behållaren registret med hjälp av hello Azure CLI

Azure Container Registry är en hanterad Docker-behållarregistertjänst som används för att lagra privata Docker-behållaravbildningar. Den här guiden information hur du skapar en hanterad Azure Container registret-instans med hello Azure CLI.

Hanterade Azure-behållarregister finns endast som förhandsversion och är inte tillgängliga i alla regioner.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt denna Snabbstart kräver att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando. En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras. 

hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *westcentralus* plats.

```azurecli-interactive 
az group create --name myResourceGroup --location westcentralus
```

## <a name="create-a-container-registry"></a>Skapa ett behållarregister

Skapa en ACR-instans med hjälp av hello [az acr skapa](/cli/azure/acr#create) kommando.

> [!NOTE]
> När du skapar ett register anger du ett globalt unikt domännamn på den översta nivån, som endast innehåller bokstäver och siffror.

 hello registret i hello exempel heter *myContainerRegistry1*, ersätta en unika namn.

```azurecli
az acr create --name myContainerRegistry1 --resource-group myResourceGroup --sku Managed_Standard
```

När hello register skapas är hello utdata liknande toohello följande:

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-29T04:50:28.607134+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry1",
  "location": "westcentralus",
  "loginServer": "mycontainerregistry1.azurecr.io",
  "name": "myContainerRegistry1",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Managed_Standard",
    "tier": "Managed"
  },
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

## <a name="log-in-tooacr-instance"></a>Logga in tooACR instans

Innan push-installation och dra behållaren bilder, måste du logga in toohello ACR-instans. toodo så Använd hello [az acr inloggning](/cli/azure/acr#login) kommando.

```azurecli-interactive
az acr login --name myAzureContainerRegistry1
```

hello kommando returnerar meddelandet inloggningen lyckades när den har slutförts.

## <a name="use-azure-container-registry"></a>Använda Azure Container Registry

### <a name="list-container-images"></a>Visa lista över behållaravbildningar

Använd hello `az acr` CLI-kommandon tooquery hello bilder och taggar i en databas.

> [!NOTE]
> Behållaren registret stöder för närvarande inte hello `docker search` kommandot tooquery för avbildningar och etiketter.

### <a name="list-repositories"></a>Visa en lista över lagringsplatser

hello visar följande exempel hello databaser i ett register i JSON (JavaScript Object Notation)-format:

```azurecli
az acr repository list -n myContainerRegistry1 -o json
```

### <a name="list-tags"></a>Visa en lista över taggar

hello följande exempel visar hello taggar på hello **exempel/nginx** databasen i JSON-format:

```azurecli
az acr repository show-tags -n myContainerRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a>Nästa steg

Du har skapat en hanterad Azure Container registret-instans med hello Azure CLI i den här snabbstartsguide.

> [!div class="nextstepaction"]
> [Push-en avbildning med hjälp av hello Docker CLI](container-registry-get-started-docker-cli.md)
