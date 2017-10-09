---
title: "aaaAzure Behållarinstanser tutorial – distribuera appen | Microsoft Docs"
description: "Azure Behållarinstanser tutorial – distribuera appen"
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
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: b9fb098d9491e1073f0be4b14a0b9b1a18f16095
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-container-tooazure-container-instances"></a><span data-ttu-id="00b86-103">Distribuera en behållare tooAzure Behållarinstanser</span><span class="sxs-lookup"><span data-stu-id="00b86-103">Deploy a container tooAzure Container Instances</span></span>

<span data-ttu-id="00b86-104">Detta är hello senaste i tre delar självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="00b86-104">This is hello last of a three-part tutorial.</span></span> <span data-ttu-id="00b86-105">I föregående avsnitt [en behållare avbildningen skapades](container-instances-tutorial-prepare-app.md) och [pushas tooan Azure Container registret](container-instances-tutorial-prepare-acr.md).</span><span class="sxs-lookup"><span data-stu-id="00b86-105">In previous sections, [a container image was created](container-instances-tutorial-prepare-app.md) and [pushed tooan Azure Container Registry](container-instances-tutorial-prepare-acr.md).</span></span> <span data-ttu-id="00b86-106">Det här avsnittet är klar hello kursen genom att distribuera hello behållaren tooAzure Behållarinstanser.</span><span class="sxs-lookup"><span data-stu-id="00b86-106">This section completes hello tutorial by deploying hello container tooAzure Container Instances.</span></span> <span data-ttu-id="00b86-107">Slutfört stegen innefattar:</span><span class="sxs-lookup"><span data-stu-id="00b86-107">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="00b86-108">Ange en behållare grupp med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="00b86-108">Defining a container group using an Azure Resource Manager template</span></span>
> * <span data-ttu-id="00b86-109">Distribuera hello behållargruppen med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="00b86-109">Deploying hello container group using hello Azure CLI</span></span>
> * <span data-ttu-id="00b86-110">Visa behållaren loggar</span><span class="sxs-lookup"><span data-stu-id="00b86-110">Viewing container logs</span></span>

## <a name="deploy-hello-container-using-hello-azure-cli"></a><span data-ttu-id="00b86-111">Distribuera hello behållare med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="00b86-111">Deploy hello container using hello Azure CLI</span></span>

<span data-ttu-id="00b86-112">hello Azure CLI gör det möjligt för distribution av en behållare tooAzure Behållarinstanser i ett enda kommando.</span><span class="sxs-lookup"><span data-stu-id="00b86-112">hello Azure CLI enables deployment of a container tooAzure Container Instances in a single command.</span></span> <span data-ttu-id="00b86-113">Eftersom hello behållaren bilden finns i Hej registret för privat Azure-behållare, måste du inkludera hello autentiseringsuppgifter krävs tooaccess den.</span><span class="sxs-lookup"><span data-stu-id="00b86-113">Since hello container image is hosted in hello private Azure Container Registry, you must include hello credentials required tooaccess it.</span></span> <span data-ttu-id="00b86-114">Om det behövs kan du fråga dem enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="00b86-114">If necessary, you can query them as shown below.</span></span>

<span data-ttu-id="00b86-115">Behållaren registret inloggningsserver (uppdatera ditt namn i registret):</span><span class="sxs-lookup"><span data-stu-id="00b86-115">Container registry login server (update with your registry name):</span></span>

```azurecli-interactive
az acr show --name <acrName> --query loginServer
```

<span data-ttu-id="00b86-116">Behållaren registret lösenord:</span><span class="sxs-lookup"><span data-stu-id="00b86-116">Container registry password:</span></span>

```azurecli-interactive
az acr credential show --name <acrName> --query "passwords[0].value"
```

<span data-ttu-id="00b86-117">toodeploy behållaren avbildningen från hello behållaren registret med en resurs begär 1 processorkärna och 1GB minne, kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="00b86-117">toodeploy your container image from hello container registry with a resource request of 1 CPU core and 1GB of memory, run hello following command:</span></span>

```azurecli-interactive
az container create --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-password <acrPassword> --ip-address public -g myResourceGroup
```

<span data-ttu-id="00b86-118">Inom några sekunder får du ett första svar från Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="00b86-118">Within a few seconds, you will receive an initial response from Azure Resource Manager.</span></span> <span data-ttu-id="00b86-119">tooview hello tillstånd för hello distribution, Använd:</span><span class="sxs-lookup"><span data-stu-id="00b86-119">tooview hello state of hello deployment, use:</span></span>

```azurecli-interactive
az container show --name aci-tutorial-app --resource-group myResourceGroup --query state
```

<span data-ttu-id="00b86-120">Vi kan fortsätta att köra det här kommandot förrän hello tillstånd ändras från *väntande* för*kör*.</span><span class="sxs-lookup"><span data-stu-id="00b86-120">We can continue running this command until hello state changes from *pending* too*running*.</span></span> <span data-ttu-id="00b86-121">Vi kan sedan fortsätta.</span><span class="sxs-lookup"><span data-stu-id="00b86-121">Then we can proceed.</span></span>

## <a name="view-hello-application-and-container-logs"></a><span data-ttu-id="00b86-122">Visa hello program- och loggfiler</span><span class="sxs-lookup"><span data-stu-id="00b86-122">View hello application and container logs</span></span>

<span data-ttu-id="00b86-123">När hello distribution lyckas, öppna din webbläsare toohello IP-adress som visas i hello utdata från hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="00b86-123">Once hello deployment succeeds, open your browser toohello IP address shown in hello output of hello following command:</span></span>

```bash
az container show --name aci-tutorial-app --resource-group myResourceGroup --query ipAddress.ip
```

```json
"13.88.176.27"
```

![Hello world-app i hello webbläsare][aci-app-browser]

<span data-ttu-id="00b86-125">Du kan också visa hello loggutdata för hello behållare:</span><span class="sxs-lookup"><span data-stu-id="00b86-125">You can also view hello log output of hello container:</span></span>

```azurecli-interactive
az container logs --name aci-tutorial-app -g myResourceGroup
```

<span data-ttu-id="00b86-126">Resultat:</span><span class="sxs-lookup"><span data-stu-id="00b86-126">Output:</span></span>

```bash
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://13.88.176.27/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="next-steps"></a><span data-ttu-id="00b86-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="00b86-127">Next steps</span></span>

<span data-ttu-id="00b86-128">I kursen får slutfört du hello processen för att distribuera din behållare tooAzure Behållarinstanser.</span><span class="sxs-lookup"><span data-stu-id="00b86-128">In this tutorial, you completed hello process of deploying your containers tooAzure Container Instances.</span></span> <span data-ttu-id="00b86-129">hello följande steg har slutförts:</span><span class="sxs-lookup"><span data-stu-id="00b86-129">hello following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="00b86-130">Distribuera hello behållare från hello Azure Container registret med hjälp av hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="00b86-130">Deploying hello container from hello Azure Container Registry using hello Azure CLI</span></span>
> * <span data-ttu-id="00b86-131">Visar hello program i hello webbläsare</span><span class="sxs-lookup"><span data-stu-id="00b86-131">Viewing hello application in hello browser</span></span>
> * <span data-ttu-id="00b86-132">Visa hello behållaren loggar</span><span class="sxs-lookup"><span data-stu-id="00b86-132">Viewing hello container logs</span></span>

<!-- LINKS -->
[prepare-app]: ./container-instances-tutorial-prepare-app.md

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png
