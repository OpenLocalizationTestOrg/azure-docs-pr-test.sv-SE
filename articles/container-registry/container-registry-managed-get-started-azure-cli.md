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
# <a name="create-a-managed-container-registry-using-hello-azure-cli"></a><span data-ttu-id="ae43d-103">Skapa en hanterad behållaren registret med hjälp av hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ae43d-103">Create a managed container registry using hello Azure CLI</span></span>

<span data-ttu-id="ae43d-104">Azure Container Registry är en hanterad Docker-behållarregistertjänst som används för att lagra privata Docker-behållaravbildningar.</span><span class="sxs-lookup"><span data-stu-id="ae43d-104">Azure Container Registry is a managed Docker container registry service used for storing private Docker container images.</span></span> <span data-ttu-id="ae43d-105">Den här guiden information hur du skapar en hanterad Azure Container registret-instans med hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="ae43d-105">This guide details creating a managed Azure Container Registry instance using hello Azure CLI.</span></span>

<span data-ttu-id="ae43d-106">Hanterade Azure-behållarregister finns endast som förhandsversion och är inte tillgängliga i alla regioner.</span><span class="sxs-lookup"><span data-stu-id="ae43d-106">Managed Azure container registries are in preview and not available in all regions.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="ae43d-107">Om du väljer tooinstall och använda hello CLI lokalt denna Snabbstart kräver att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="ae43d-107">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="ae43d-108">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="ae43d-108">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="ae43d-109">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ae43d-109">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-a-resource-group"></a><span data-ttu-id="ae43d-110">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="ae43d-110">Create a resource group</span></span>

<span data-ttu-id="ae43d-111">Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="ae43d-111">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="ae43d-112">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="ae43d-112">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="ae43d-113">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *westcentralus* plats.</span><span class="sxs-lookup"><span data-stu-id="ae43d-113">hello following example creates a resource group named *myResourceGroup* in hello *westcentralus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location westcentralus
```

## <a name="create-a-container-registry"></a><span data-ttu-id="ae43d-114">Skapa ett behållarregister</span><span class="sxs-lookup"><span data-stu-id="ae43d-114">Create a container registry</span></span>

<span data-ttu-id="ae43d-115">Skapa en ACR-instans med hjälp av hello [az acr skapa](/cli/azure/acr#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="ae43d-115">Create an ACR instance using hello [az acr create](/cli/azure/acr#create) command.</span></span>

> [!NOTE]
> <span data-ttu-id="ae43d-116">När du skapar ett register anger du ett globalt unikt domännamn på den översta nivån, som endast innehåller bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="ae43d-116">When you create a registry, specify a globally unique top-level domain name, containing only letters and numbers.</span></span>

 <span data-ttu-id="ae43d-117">hello registret i hello exempel heter *myContainerRegistry1*, ersätta en unika namn.</span><span class="sxs-lookup"><span data-stu-id="ae43d-117">hello registry name in hello example is *myContainerRegistry1*, substitute a unique name of your own.</span></span>

```azurecli
az acr create --name myContainerRegistry1 --resource-group myResourceGroup --sku Managed_Standard
```

<span data-ttu-id="ae43d-118">När hello register skapas är hello utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="ae43d-118">When hello registry is created, hello output is similar toohello following:</span></span>

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

## <a name="log-in-tooacr-instance"></a><span data-ttu-id="ae43d-119">Logga in tooACR instans</span><span class="sxs-lookup"><span data-stu-id="ae43d-119">Log in tooACR instance</span></span>

<span data-ttu-id="ae43d-120">Innan push-installation och dra behållaren bilder, måste du logga in toohello ACR-instans.</span><span class="sxs-lookup"><span data-stu-id="ae43d-120">Before pushing and pulling container images, you must log in toohello ACR instance.</span></span> <span data-ttu-id="ae43d-121">toodo så Använd hello [az acr inloggning](/cli/azure/acr#login) kommando.</span><span class="sxs-lookup"><span data-stu-id="ae43d-121">toodo so, use hello [az acr login](/cli/azure/acr#login) command.</span></span>

```azurecli-interactive
az acr login --name myAzureContainerRegistry1
```

<span data-ttu-id="ae43d-122">hello kommando returnerar meddelandet inloggningen lyckades när den har slutförts.</span><span class="sxs-lookup"><span data-stu-id="ae43d-122">hello command returns a 'Login Succeeded' message once completed.</span></span>

## <a name="use-azure-container-registry"></a><span data-ttu-id="ae43d-123">Använda Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="ae43d-123">Use Azure Container Registry</span></span>

### <a name="list-container-images"></a><span data-ttu-id="ae43d-124">Visa lista över behållaravbildningar</span><span class="sxs-lookup"><span data-stu-id="ae43d-124">List container images</span></span>

<span data-ttu-id="ae43d-125">Använd hello `az acr` CLI-kommandon tooquery hello bilder och taggar i en databas.</span><span class="sxs-lookup"><span data-stu-id="ae43d-125">Use hello `az acr` CLI commands tooquery hello images and tags in a repository.</span></span>

> [!NOTE]
> <span data-ttu-id="ae43d-126">Behållaren registret stöder för närvarande inte hello `docker search` kommandot tooquery för avbildningar och etiketter.</span><span class="sxs-lookup"><span data-stu-id="ae43d-126">Currently, Container Registry does not support hello `docker search` command tooquery for images and tags.</span></span>

### <a name="list-repositories"></a><span data-ttu-id="ae43d-127">Visa en lista över lagringsplatser</span><span class="sxs-lookup"><span data-stu-id="ae43d-127">List repositories</span></span>

<span data-ttu-id="ae43d-128">hello visar följande exempel hello databaser i ett register i JSON (JavaScript Object Notation)-format:</span><span class="sxs-lookup"><span data-stu-id="ae43d-128">hello following example lists hello repositories in a registry, in JSON (JavaScript Object Notation) format:</span></span>

```azurecli
az acr repository list -n myContainerRegistry1 -o json
```

### <a name="list-tags"></a><span data-ttu-id="ae43d-129">Visa en lista över taggar</span><span class="sxs-lookup"><span data-stu-id="ae43d-129">List tags</span></span>

<span data-ttu-id="ae43d-130">hello följande exempel visar hello taggar på hello **exempel/nginx** databasen i JSON-format:</span><span class="sxs-lookup"><span data-stu-id="ae43d-130">hello following example lists hello tags on hello **samples/nginx** repository, in JSON format:</span></span>

```azurecli
az acr repository show-tags -n myContainerRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a><span data-ttu-id="ae43d-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ae43d-131">Next steps</span></span>

<span data-ttu-id="ae43d-132">Du har skapat en hanterad Azure Container registret-instans med hello Azure CLI i den här snabbstartsguide.</span><span class="sxs-lookup"><span data-stu-id="ae43d-132">In this quick start, you've created a managed Azure Container Registry instance using hello Azure CLI.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ae43d-133">Push-en avbildning med hjälp av hello Docker CLI</span><span class="sxs-lookup"><span data-stu-id="ae43d-133">Push your first image using hello Docker CLI</span></span>](container-registry-get-started-docker-cli.md)
