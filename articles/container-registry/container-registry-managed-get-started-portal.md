---
title: "Skapa privat Docker-register – Azure Portal | Microsoft-dokument"
description: "Kom igång med att skapa och hantera privata Docker-behållarregister med Azure Portal"
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
ms.openlocfilehash: 560aee42b0c5a61c37c594d7937f833ab7183d49
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="create-a-managed-container-registry-using-the-azure-portal"></a><span data-ttu-id="d3252-103">Skapa ett hanterat behållarregister med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d3252-103">Create a managed container registry using the Azure portal</span></span>

<span data-ttu-id="d3252-104">Azure Container Registry är en hanterad Docker-behållarregistertjänst som används för att lagra privata Docker-behållaravbildningar.</span><span class="sxs-lookup"><span data-stu-id="d3252-104">Azure Container Registry is a managed Docker container registry service used for storing private Docker container images.</span></span> <span data-ttu-id="d3252-105">Den här guiden beskriver hur du skapar en hanterad Azure Container Registry-instans med hjälp av Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="d3252-105">This guide details creating a managed Azure Container Registry instance using the Azure portal.</span></span>

<span data-ttu-id="d3252-106">Hanterade Azure-behållarregister finns endast som förhandsversion och är inte tillgängliga i alla regioner.</span><span class="sxs-lookup"><span data-stu-id="d3252-106">Managed Azure container registries are in preview and not available in all regions.</span></span>

## <a name="log-in-to-azure"></a><span data-ttu-id="d3252-107">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="d3252-107">Log in to Azure</span></span>

<span data-ttu-id="d3252-108">Logga in på Azure Portal på http://portal.azure.com.</span><span class="sxs-lookup"><span data-stu-id="d3252-108">Log in to the Azure portal at http://portal.azure.com.</span></span>

## <a name="create-a-container-registry"></a><span data-ttu-id="d3252-109">Skapa ett behållarregister</span><span class="sxs-lookup"><span data-stu-id="d3252-109">Create a container registry</span></span>

1. <span data-ttu-id="d3252-110">Klicka på knappen **New** (Nytt) i det övre vänstra hörnet i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="d3252-110">Click the **New** button found on the upper left-hand corner of the Azure portal.</span></span>

2. <span data-ttu-id="d3252-111">Sök efter och välj **Azure Container Registry** på Marketplace.</span><span class="sxs-lookup"><span data-stu-id="d3252-111">Search the marketplace for **Azure container registry** and select it.</span></span>

3. <span data-ttu-id="d3252-112">Klicka på **Skapa** så att bladet för att skapa ACR öppnas.</span><span class="sxs-lookup"><span data-stu-id="d3252-112">Click **Create** which will open the ACR creation blade.</span></span>

    ![Inställningar för behållarregister](./media/container-registry-get-started-portal/managed-container-registry-settings.png)

4. <span data-ttu-id="d3252-114">Ange informationen nedan på bladet **Azure Container Registry**.</span><span class="sxs-lookup"><span data-stu-id="d3252-114">In the **Azure Container Registry** blade, enter the following information.</span></span> <span data-ttu-id="d3252-115">Klicka på **Skapa** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="d3252-115">Click **Create** when you are done.</span></span>

    <span data-ttu-id="d3252-116">a.</span><span class="sxs-lookup"><span data-stu-id="d3252-116">a.</span></span> <span data-ttu-id="d3252-117">**Registernamn**: Ett globalt unikt domännamn på den översta nivån för ditt specifika register.</span><span class="sxs-lookup"><span data-stu-id="d3252-117">**Registry name**: A globally unique top-level domain name for your specific registry.</span></span> <span data-ttu-id="d3252-118">I det här exemplet är registernamnet *myAzureContainerRegistry1*, men du kan ersätta namnet med ett eget unikt namn.</span><span class="sxs-lookup"><span data-stu-id="d3252-118">In this example, the registry name is *myAzureContainerRegistry1*, but substitute a unique name of your own.</span></span> <span data-ttu-id="d3252-119">Namnet får bara innehålla bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="d3252-119">The name can contain only letters and numbers.</span></span>

    <span data-ttu-id="d3252-120">b.</span><span class="sxs-lookup"><span data-stu-id="d3252-120">b.</span></span> <span data-ttu-id="d3252-121">**Resursgrupp**: Välj en befintlig [resursgrupp](../azure-resource-manager/resource-group-overview.md#resource-groups) eller skriv namnet på en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d3252-121">**Resource group**: Select an existing [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) or type the name for a new one.</span></span>

    <span data-ttu-id="d3252-122">c.</span><span class="sxs-lookup"><span data-stu-id="d3252-122">c.</span></span> <span data-ttu-id="d3252-123">**Plats**: Välj en plats för ett Azure-datacenter där tjänsten är [tillgänglig](https://azure.microsoft.com/regions/services/), t.ex. **USA, södra centrala**.</span><span class="sxs-lookup"><span data-stu-id="d3252-123">**Location**: Select an Azure datacenter location where the service is [available](https://azure.microsoft.com/regions/services/), such as **South Central US**.</span></span>

    <span data-ttu-id="d3252-124">d.</span><span class="sxs-lookup"><span data-stu-id="d3252-124">d.</span></span> <span data-ttu-id="d3252-125">**Administratörsanvändarnamn**: Om du vill ger du en administratörsanvändare åtkomst till registret.</span><span class="sxs-lookup"><span data-stu-id="d3252-125">**Admin user**: If you want, enable an admin user to access the registry.</span></span> <span data-ttu-id="d3252-126">Du kan ändra den här inställningen när du har skapat registret.</span><span class="sxs-lookup"><span data-stu-id="d3252-126">You can change this setting after creating the registry.</span></span>

    <span data-ttu-id="d3252-127">e.</span><span class="sxs-lookup"><span data-stu-id="d3252-127">e.</span></span> <span data-ttu-id="d3252-128">**Använd hanterat register**: Välj Ja om du vill hantera registerlagring automatiskt med ACR, använda webhooks och använda AAD-autentisering.</span><span class="sxs-lookup"><span data-stu-id="d3252-128">**Use managed registry**: Select yes to have ACR automatically manage the registry storage, use webhooks, and use AAD authentication.</span></span>

    <span data-ttu-id="d3252-129">f.</span><span class="sxs-lookup"><span data-stu-id="d3252-129">f.</span></span> <span data-ttu-id="d3252-130">**Prisnivå**: Välj en prisnivå. Här visas mer information om ACR-priser.</span><span class="sxs-lookup"><span data-stu-id="d3252-130">**Pricing Tier**: Select a pricing tier, see here ACR pricing for more information.</span></span>

## <a name="log-in-to-acr-instance"></a><span data-ttu-id="d3252-131">Logga in på ACR-instansen</span><span class="sxs-lookup"><span data-stu-id="d3252-131">Log in to ACR instance</span></span>

<span data-ttu-id="d3252-132">Innan du skickar och hämtar behållaravbildningar måste du logga in på ACR-instansen.</span><span class="sxs-lookup"><span data-stu-id="d3252-132">Before pushing and pulling container images, you must log in to the ACR instance.</span></span> 

<span data-ttu-id="d3252-133">Det gör du med Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="d3252-133">To do so, use the Azure CLI 2.0.</span></span> <span data-ttu-id="d3252-134">Först loggar du in på Azure med kommandot [az login](/cli/azure/#login), om det behövs.</span><span class="sxs-lookup"><span data-stu-id="d3252-134">First, if needed, log into Azure using the [az login](/cli/azure/#login) command.</span></span> 

```azurecli
az login
```

<span data-ttu-id="d3252-135">Sedan använder du kommandot [az acr login](/cli/azure/acr#login) för att logga in på Azure Container Registry.</span><span class="sxs-lookup"><span data-stu-id="d3252-135">Next, use the [az acr login](/cli/azure/acr#login) command to log in to the Azure Container Registry.</span></span>

```azurecli-interactive
az acr login --name myAzureContainerRegistry1
```

## <a name="use-azure-container-registry"></a><span data-ttu-id="d3252-136">Använda Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="d3252-136">Use Azure Container Registry</span></span>

### <a name="list-container-images"></a><span data-ttu-id="d3252-137">Visa lista över behållaravbildningar</span><span class="sxs-lookup"><span data-stu-id="d3252-137">List container images</span></span>

<span data-ttu-id="d3252-138">Använd `az acr` CLI-kommandona för att skicka frågor mot avbildningarna och taggarna på en lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="d3252-138">Use the `az acr` CLI commands to query the images and tags in a repository.</span></span>

> [!NOTE]
> <span data-ttu-id="d3252-139">För närvarande stöder inte Container Registry `docker search`-kommandot för att hämta information om avbildningar och taggar.</span><span class="sxs-lookup"><span data-stu-id="d3252-139">Currently, Container Registry does not support the `docker search` command to query for images and tags.</span></span>

### <a name="list-repositories"></a><span data-ttu-id="d3252-140">Visa en lista över lagringsplatser</span><span class="sxs-lookup"><span data-stu-id="d3252-140">List repositories</span></span>

<span data-ttu-id="d3252-141">Följande exempel visar en lista över lagringsplatser i ett register, i JSON-format (JavaScript Object Notation):</span><span class="sxs-lookup"><span data-stu-id="d3252-141">The following example lists the repositories in a registry, in JSON (JavaScript Object Notation) format:</span></span>

```azurecli
az acr repository list -n myContainerRegistry1 -o json
```

### <a name="list-tags"></a><span data-ttu-id="d3252-142">Visa en lista över taggar</span><span class="sxs-lookup"><span data-stu-id="d3252-142">List tags</span></span>

<span data-ttu-id="d3252-143">Följande exempel visar en lista med taggarna som används för **samples/nginx**-lagringsplatsen, i JSON-format:</span><span class="sxs-lookup"><span data-stu-id="d3252-143">The following example lists the tags on the **samples/nginx** repository, in JSON format:</span></span>

```azurecli
az acr repository show-tags -n myContainerRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a><span data-ttu-id="d3252-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d3252-144">Next steps</span></span>

<span data-ttu-id="d3252-145">I den här snabbstarten har du skapat en hanterad Azure Container Registry-instans med hjälp av Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="d3252-145">In this quick start, you've created a managed Azure Container Registry instance using the Azure portal.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d3252-146">Skicka din första avbildning med hjälp av Docker CLI</span><span class="sxs-lookup"><span data-stu-id="d3252-146">Push your first image using the Docker CLI</span></span>](container-registry-get-started-docker-cli.md)