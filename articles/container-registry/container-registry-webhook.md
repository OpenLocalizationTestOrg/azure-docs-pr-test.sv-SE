---
title: "aaaAzure behållaren registret webhooks | Microsoft Docs"
description: "Azure-behållaren registret webhooks"
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acr, azure-container-registry
keywords: "Docker behållare ACR"
ms.assetid: 
ms.service: container-registry
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2017
ms.author: nepeters
ms.openlocfilehash: adc2afec486007e2d54cd689e6f7ef8b1098db06
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-container-registry-webhooks---azure-portal"></a><span data-ttu-id="ceeb0-104">Med hjälp av Azure-behållare registret webhooks - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ceeb0-104">Using Azure Container Registry webhooks - Azure portal</span></span>

<span data-ttu-id="ceeb0-105">En Azure-behållaren registret lagrar och hanterar privata Docker behållare bilder, liknande toohello sätt Docker hubb lagrar offentliga Docker-avbildningar.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-105">An Azure container registry stores and manages private Docker container images, similar toohello way Docker Hub stores public Docker images.</span></span> <span data-ttu-id="ceeb0-106">Du kan använda webhooks tootrigger händelser när vissa åtgärder utföras i en av dina databaser i registret.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-106">You use webhooks tootrigger events when certain actions take place in one of your registry repositories.</span></span> <span data-ttu-id="ceeb0-107">Webhooks kan svara tooevents på hello registret nivå eller vara begränsad ned tooa databas taggen.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-107">Webhooks can respond tooevents at hello registry level or they can be scoped down tooa specific repository tag.</span></span> 

<span data-ttu-id="ceeb0-108">Mer bakgrund och begrepp finns hello [översikt](./container-registry-intro.md).</span><span class="sxs-lookup"><span data-stu-id="ceeb0-108">For more background and concepts, see hello [overview](./container-registry-intro.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ceeb0-109">Krav</span><span class="sxs-lookup"><span data-stu-id="ceeb0-109">Prerequisites</span></span> 

- <span data-ttu-id="ceeb0-110">Azure-behållaren hanteras registret - skapa en hanterad behållaren registret i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-110">Azure container managed registry - Create a managed container registry in your Azure subscription.</span></span> <span data-ttu-id="ceeb0-111">Till exempel använda hello Azure-portalen eller hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-111">For example, use hello Azure portal or hello Azure CLI 2.0.</span></span> 
- <span data-ttu-id="ceeb0-112">Docker CLI - tooset din lokala dator som en Docker-värden och åtkomst hello Docker CLI-kommandon, installera Docker-motorn.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-112">Docker CLI - tooset up your local computer as a Docker host and access hello Docker CLI commands, install Docker Engine.</span></span> 

## <a name="create-webhook-azure-portal"></a><span data-ttu-id="ceeb0-113">Skapa webhook Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ceeb0-113">Create webhook Azure portal</span></span>

1. <span data-ttu-id="ceeb0-114">Logga in toohello Azure-portalen och navigera toohello registret som du vill toocreate webhooks.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-114">Log in toohello Azure portal and navigate toohello registry in which you want toocreate webhooks.</span></span> 

2. <span data-ttu-id="ceeb0-115">Välj hello ”Webhooks” flik i hello behållaren bladet.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-115">In hello container blade, select hello "Webhooks" tab.</span></span> 

3. <span data-ttu-id="ceeb0-116">Välj ”Lägg till” Hej webhook bladet verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-116">Select "Add" from hello webhook blade toolbar.</span></span> 

4. <span data-ttu-id="ceeb0-117">Fullständig hello *skapa Webhook* formulär med hello följande information:</span><span class="sxs-lookup"><span data-stu-id="ceeb0-117">Complete hello *Create Webhook* form with hello following information:</span></span>

| <span data-ttu-id="ceeb0-118">Värde</span><span class="sxs-lookup"><span data-stu-id="ceeb0-118">Value</span></span> | <span data-ttu-id="ceeb0-119">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ceeb0-119">Description</span></span> |
|---|---|
| <span data-ttu-id="ceeb0-120">Namn</span><span class="sxs-lookup"><span data-stu-id="ceeb0-120">Name</span></span> | <span data-ttu-id="ceeb0-121">hello-namn som du vill toogive toohello webhooken.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-121">hello name you want toogive toohello webhook.</span></span> <span data-ttu-id="ceeb0-122">Det kan bara innehålla gemena bokstäver och siffror och mellan 5 och 50 tecken.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-122">It can only contain lowercase letters and numbers and between 5-50 characters.</span></span> |
| <span data-ttu-id="ceeb0-123">URI för tjänsten</span><span class="sxs-lookup"><span data-stu-id="ceeb0-123">Service URI</span></span> | <span data-ttu-id="ceeb0-124">hello URI där hello webhook ska skicka efter meddelanden.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-124">hello URI where hello webhook should send POST notifications.</span></span> |
| <span data-ttu-id="ceeb0-125">Egna rubriker</span><span class="sxs-lookup"><span data-stu-id="ceeb0-125">Custom headers</span></span> | <span data-ttu-id="ceeb0-126">Huvuden som du vill använda toopass tillsammans med hello POST-begäran.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-126">Headers you want toopass along with hello POST request.</span></span> <span data-ttu-id="ceeb0-127">De måste vara i ”nyckel: värde” format.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-127">They should be in "key: value" format.</span></span> |
| <span data-ttu-id="ceeb0-128">Utlösaråtgärder</span><span class="sxs-lookup"><span data-stu-id="ceeb0-128">Trigger actions</span></span> | <span data-ttu-id="ceeb0-129">Åtgärder som utlöser hello webhooken.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-129">Actions that trigger hello webhook.</span></span> <span data-ttu-id="ceeb0-130">För närvarande webhooks kan utlösas av push och/eller ta bort åtgärder tooan avbildningen.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-130">Currently webhooks can be triggered by push and/or delete actions tooan image.</span></span> |
| <span data-ttu-id="ceeb0-131">Status</span><span class="sxs-lookup"><span data-stu-id="ceeb0-131">Status</span></span> | <span data-ttu-id="ceeb0-132">hello status för hello webhook när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-132">hello status for hello webhook after it's created.</span></span> <span data-ttu-id="ceeb0-133">Den är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-133">It's enabled by default.</span></span> |
| <span data-ttu-id="ceeb0-134">Omfång</span><span class="sxs-lookup"><span data-stu-id="ceeb0-134">Scope</span></span> | <span data-ttu-id="ceeb0-135">hello scope på som hello webhook fungerar.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-135">hello scope at which hello webhook works.</span></span> <span data-ttu-id="ceeb0-136">Som standard är hello omfång för alla händelser i hello registret.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-136">By default hello scope is for all events in hello registry.</span></span> <span data-ttu-id="ceeb0-137">Det kan anges för en databas eller en tagg hello formatet ”databas: taggen”.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-137">It can be specified for a repository or a tag by using hello format "repository: tag".</span></span> |

<span data-ttu-id="ceeb0-138">Exempelformulär för webhook:</span><span class="sxs-lookup"><span data-stu-id="ceeb0-138">Example webhook form:</span></span>

![DC/OS-GRÄNSSNITTET](./media/container-registry-webhook/webhook.png)

## <a name="create-webhook-azure-cli"></a><span data-ttu-id="ceeb0-140">Skapa webhook Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ceeb0-140">Create webhook Azure CLI</span></span>

<span data-ttu-id="ceeb0-141">en webhook med hjälp av toocreate hello Azure CLI, Använd hello [az acr webhook skapa](/cli/azure/acr/webhook#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-141">toocreate a webhook using hello Azure CLI, use hello [az acr webhook create](/cli/azure/acr/webhook#create) command.</span></span>

```azurecli-interactive
az acr webhook create --registry mycontainerregistry --name myacrwebhook01 --actions delete --uri http://webhookuri.com
```

## <a name="test-webhook"></a><span data-ttu-id="ceeb0-142">Test-webhook</span><span class="sxs-lookup"><span data-stu-id="ceeb0-142">Test webhook</span></span>

### <a name="azure-portal"></a><span data-ttu-id="ceeb0-143">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ceeb0-143">Azure portal</span></span>

<span data-ttu-id="ceeb0-144">Tidigare toousing hello webhook behållaren image push- och borttagningsåtgärder, kan testas med hello **Ping** knappen.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-144">Prior toousing hello webhook on container image push and delete actions, it can be tested using hello **Ping** button.</span></span> <span data-ttu-id="ceeb0-145">När du skickar hello Ping toohello en allmän post-begäran anges slutpunkt och loggar hello svar.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-145">When used, hello Ping sends a generic post request toohello specified endpoint and logs hello response.</span></span> <span data-ttu-id="ceeb0-146">Det här är användbart tooverify som hello webhook har ställts in korrekt.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-146">This is helpful tooverify that hello webhook has been set up correctly.</span></span>

1. <span data-ttu-id="ceeb0-147">Välj hello webhook som du vill tootest.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-147">Select hello webhook you want tootest.</span></span> 
2. <span data-ttu-id="ceeb0-148">Välj hello ”pinga” åtgärd i hello översta verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-148">In hello top toolbar, select hello "Ping" action.</span></span> 
3. <span data-ttu-id="ceeb0-149">Kontrollera hello förfrågan och svar.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-149">Check hello request and response.</span></span>

### <a name="azure-cli"></a><span data-ttu-id="ceeb0-150">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ceeb0-150">Azure CLI</span></span>

<span data-ttu-id="ceeb0-151">tootest en ACR webhook med hello Azure CLI, använder hello [az acr webhook ping](/cli/azure/acr/webhook#ping) kommando.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-151">tootest an ACR webhook with hello Azure CLI, use hello [az acr webhook ping](/cli/azure/acr/webhook#ping) command.</span></span>

```azurecli-interactive
az acr webhook ping --registry mycontainerregistry --name myacrwebhook01
```

<span data-ttu-id="ceeb0-152">toosee hello resultat kan du använda hello [az acr webhook lista-händelser](/cli/azure/acr/webhook#list-events) kommando.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-152">toosee hello results, use hello [az acr webhook list-events](/cli/azure/acr/webhook#list-events) command.</span></span> 

```azurecli-interactive
az acr webhook list-events --registry mycontainerregistry08 --name myacrwebhook01
```

## <a name="delete-webhook"></a><span data-ttu-id="ceeb0-153">Ta bort webhooken</span><span class="sxs-lookup"><span data-stu-id="ceeb0-153">Delete webhook</span></span>

### <a name="azure-portal"></a><span data-ttu-id="ceeb0-154">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ceeb0-154">Azure portal</span></span>

<span data-ttu-id="ceeb0-155">Varje webhook kan tas bort genom att välja hello webhook och hello ta borttagningsknappen hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ceeb0-155">Each webhook can be deleted by selecting hello webhook and then hello delete button on hello Azure portal.</span></span>

### <a name="azure-cli"></a><span data-ttu-id="ceeb0-156">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ceeb0-156">Azure CLI</span></span>

```azurecli-interactive
az acr webhook delete --registry mycontainerregistry --name myacrwebhook01
```