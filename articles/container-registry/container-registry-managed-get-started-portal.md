---
title: aaaCreate privata Docker register - Azure-portalen | Microsoft Docs
description: "Börja skapa och hantera privata Docker behållare register med hello Azure-portalen"
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: na
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: nepeters
ms.custom: na
ms.openlocfilehash: cf3ce0dcf3036d0e9cd1eaf01721deccb00248d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-container-registry-using-hello-azure-portal"></a><span data-ttu-id="9015e-103">Skapa en hanterad behållaren registret med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9015e-103">Create a managed container registry using hello Azure portal</span></span>

<span data-ttu-id="9015e-104">Azure Container Registry är en hanterad Docker-behållarregistertjänst som används för att lagra privata Docker-behållaravbildningar.</span><span class="sxs-lookup"><span data-stu-id="9015e-104">Azure Container Registry is a managed Docker container registry service used for storing private Docker container images.</span></span> <span data-ttu-id="9015e-105">Den här guiden information hur du skapar en hanterad Azure Container registret-instans med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9015e-105">This guide details creating a managed Azure Container Registry instance using hello Azure portal.</span></span>

<span data-ttu-id="9015e-106">Hanterade Azure-behållarregister finns endast som förhandsversion och är inte tillgängliga i alla regioner.</span><span class="sxs-lookup"><span data-stu-id="9015e-106">Managed Azure container registries are in preview and not available in all regions.</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="9015e-107">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="9015e-107">Log in tooAzure</span></span>

<span data-ttu-id="9015e-108">Logga in toohello Azure-portalen på http://portal.azure.com.</span><span class="sxs-lookup"><span data-stu-id="9015e-108">Log in toohello Azure portal at http://portal.azure.com.</span></span>

## <a name="create-a-container-registry"></a><span data-ttu-id="9015e-109">Skapa ett behållarregister</span><span class="sxs-lookup"><span data-stu-id="9015e-109">Create a container registry</span></span>

1. <span data-ttu-id="9015e-110">Klicka på hello **ny** knappen hittades på hello övre vänstra hörnet av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9015e-110">Click hello **New** button found on hello upper left-hand corner of hello Azure portal.</span></span>

2. <span data-ttu-id="9015e-111">Sök hello marketplace för **Azure-behållaren registret** och markera den.</span><span class="sxs-lookup"><span data-stu-id="9015e-111">Search hello marketplace for **Azure container registry** and select it.</span></span>

3. <span data-ttu-id="9015e-112">Klicka på **skapa** som öppnas hello ACR skapa blad.</span><span class="sxs-lookup"><span data-stu-id="9015e-112">Click **Create** which will open hello ACR creation blade.</span></span>

    ![Inställningar för behållarregister](./media/container-registry-get-started-portal/managed-container-registry-settings.png)

4. <span data-ttu-id="9015e-114">I hello **Azure Container registret** bladet ange hello följande information.</span><span class="sxs-lookup"><span data-stu-id="9015e-114">In hello **Azure Container Registry** blade, enter hello following information.</span></span> <span data-ttu-id="9015e-115">Klicka på **Skapa** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="9015e-115">Click **Create** when you are done.</span></span>

    <span data-ttu-id="9015e-116">a.</span><span class="sxs-lookup"><span data-stu-id="9015e-116">a.</span></span> <span data-ttu-id="9015e-117">**Registernamn**: Ett globalt unikt domännamn på den översta nivån för ditt specifika register.</span><span class="sxs-lookup"><span data-stu-id="9015e-117">**Registry name**: A globally unique top-level domain name for your specific registry.</span></span> <span data-ttu-id="9015e-118">I det här exemplet är hello registret namnet *myAzureContainerRegistry1*, men i stället använda ett unikt namn för din egen.</span><span class="sxs-lookup"><span data-stu-id="9015e-118">In this example, hello registry name is *myAzureContainerRegistry1*, but substitute a unique name of your own.</span></span> <span data-ttu-id="9015e-119">hello namn får bara innehålla bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="9015e-119">hello name can contain only letters and numbers.</span></span>

    <span data-ttu-id="9015e-120">b.</span><span class="sxs-lookup"><span data-stu-id="9015e-120">b.</span></span> <span data-ttu-id="9015e-121">**Resursgruppen**: Välj en befintlig [resursgruppen](../azure-resource-manager/resource-group-overview.md#resource-groups) eller hello-typnamn för en ny.</span><span class="sxs-lookup"><span data-stu-id="9015e-121">**Resource group**: Select an existing [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) or type hello name for a new one.</span></span>

    <span data-ttu-id="9015e-122">c.</span><span class="sxs-lookup"><span data-stu-id="9015e-122">c.</span></span> <span data-ttu-id="9015e-123">**Plats**: Välj en plats för Azure-datacenter där hello-tjänsten är [tillgängliga](https://azure.microsoft.com/regions/services/), som **södra centrala USA**.</span><span class="sxs-lookup"><span data-stu-id="9015e-123">**Location**: Select an Azure datacenter location where hello service is [available](https://azure.microsoft.com/regions/services/), such as **South Central US**.</span></span>

    <span data-ttu-id="9015e-124">d.</span><span class="sxs-lookup"><span data-stu-id="9015e-124">d.</span></span> <span data-ttu-id="9015e-125">**Administratören**: Om du vill aktivera en admin tooaccess hello registerinställningar.</span><span class="sxs-lookup"><span data-stu-id="9015e-125">**Admin user**: If you want, enable an admin user tooaccess hello registry.</span></span> <span data-ttu-id="9015e-126">Du kan ändra den här inställningen när du har skapat hello registret.</span><span class="sxs-lookup"><span data-stu-id="9015e-126">You can change this setting after creating hello registry.</span></span>

    <span data-ttu-id="9015e-127">e.</span><span class="sxs-lookup"><span data-stu-id="9015e-127">e.</span></span> <span data-ttu-id="9015e-128">**Använd hanterade registret**: väljer du Ja toohave ACR automatiskt hantera hello registret lagring, använder webhooks och Använd AAD-autentisering.</span><span class="sxs-lookup"><span data-stu-id="9015e-128">**Use managed registry**: Select yes toohave ACR automatically manage hello registry storage, use webhooks, and use AAD authentication.</span></span>

    <span data-ttu-id="9015e-129">f.</span><span class="sxs-lookup"><span data-stu-id="9015e-129">f.</span></span> <span data-ttu-id="9015e-130">**Prisnivå**: Välj en prisnivå. Här visas mer information om ACR-priser.</span><span class="sxs-lookup"><span data-stu-id="9015e-130">**Pricing Tier**: Select a pricing tier, see here ACR pricing for more information.</span></span>

## <a name="log-in-tooacr-instance"></a><span data-ttu-id="9015e-131">Logga in tooACR instans</span><span class="sxs-lookup"><span data-stu-id="9015e-131">Log in tooACR instance</span></span>

<span data-ttu-id="9015e-132">Innan push-installation och dra behållaren bilder, måste du logga in toohello ACR-instans.</span><span class="sxs-lookup"><span data-stu-id="9015e-132">Before pushing and pulling container images, you must log in toohello ACR instance.</span></span> 

<span data-ttu-id="9015e-133">toodo så Använd hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="9015e-133">toodo so, use hello Azure CLI 2.0.</span></span> <span data-ttu-id="9015e-134">Först om det behövs, logga in på Azure med hjälp av hello [az inloggningen](/cli/azure/#login) kommando.</span><span class="sxs-lookup"><span data-stu-id="9015e-134">First, if needed, log into Azure using hello [az login](/cli/azure/#login) command.</span></span> 

```azurecli
az login
```

<span data-ttu-id="9015e-135">Använd sedan hello [az acr inloggning](/cli/azure/acr#login) kommandot toolog i toohello Azure Container registret.</span><span class="sxs-lookup"><span data-stu-id="9015e-135">Next, use hello [az acr login](/cli/azure/acr#login) command toolog in toohello Azure Container Registry.</span></span>

```azurecli-interactive
az acr login --name myAzureContainerRegistry1
```

## <a name="use-azure-container-registry"></a><span data-ttu-id="9015e-136">Använda Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="9015e-136">Use Azure Container Registry</span></span>

### <a name="list-container-images"></a><span data-ttu-id="9015e-137">Visa lista över behållaravbildningar</span><span class="sxs-lookup"><span data-stu-id="9015e-137">List container images</span></span>

<span data-ttu-id="9015e-138">Använd hello `az acr` CLI-kommandon tooquery hello bilder och taggar i en databas.</span><span class="sxs-lookup"><span data-stu-id="9015e-138">Use hello `az acr` CLI commands tooquery hello images and tags in a repository.</span></span>

> [!NOTE]
> <span data-ttu-id="9015e-139">Behållaren registret stöder för närvarande inte hello `docker search` kommandot tooquery för avbildningar och etiketter.</span><span class="sxs-lookup"><span data-stu-id="9015e-139">Currently, Container Registry does not support hello `docker search` command tooquery for images and tags.</span></span>

### <a name="list-repositories"></a><span data-ttu-id="9015e-140">Visa en lista över lagringsplatser</span><span class="sxs-lookup"><span data-stu-id="9015e-140">List repositories</span></span>

<span data-ttu-id="9015e-141">hello visar följande exempel hello databaser i ett register i JSON (JavaScript Object Notation)-format:</span><span class="sxs-lookup"><span data-stu-id="9015e-141">hello following example lists hello repositories in a registry, in JSON (JavaScript Object Notation) format:</span></span>

```azurecli
az acr repository list -n myContainerRegistry1 -o json
```

### <a name="list-tags"></a><span data-ttu-id="9015e-142">Visa en lista över taggar</span><span class="sxs-lookup"><span data-stu-id="9015e-142">List tags</span></span>

<span data-ttu-id="9015e-143">hello följande exempel visar hello taggar på hello **exempel/nginx** databasen i JSON-format:</span><span class="sxs-lookup"><span data-stu-id="9015e-143">hello following example lists hello tags on hello **samples/nginx** repository, in JSON format:</span></span>

```azurecli
az acr repository show-tags -n myContainerRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a><span data-ttu-id="9015e-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9015e-144">Next steps</span></span>

<span data-ttu-id="9015e-145">Du har skapat en hanterad Azure Container registret-instans med hello Azure-portalen i den här snabbstartsguide.</span><span class="sxs-lookup"><span data-stu-id="9015e-145">In this quick start, you've created a managed Azure Container Registry instance using hello Azure portal.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="9015e-146">Push-en avbildning med hjälp av hello Docker CLI</span><span class="sxs-lookup"><span data-stu-id="9015e-146">Push your first image using hello Docker CLI</span></span>](container-registry-get-started-docker-cli.md)
