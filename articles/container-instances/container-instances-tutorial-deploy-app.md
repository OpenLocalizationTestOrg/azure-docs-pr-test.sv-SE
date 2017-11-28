---
title: "Azure Behållarinstanser tutorial – distribuera appen | Microsoft Docs"
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
ms.openlocfilehash: 54151a5c1850ab7120fe666a46dc5dc99c0f5157
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-container-to-azure-container-instances"></a><span data-ttu-id="77ddc-103">Distribuera en behållare till Behållarinstanser som Azure</span><span class="sxs-lookup"><span data-stu-id="77ddc-103">Deploy a container to Azure Container Instances</span></span>

<span data-ttu-id="77ddc-104">Det här är sist av tre delar självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="77ddc-104">This is the last of a three-part tutorial.</span></span> <span data-ttu-id="77ddc-105">I föregående avsnitt [en behållare avbildningen skapades](container-instances-tutorial-prepare-app.md) och [pushas till ett Azure Container registret](container-instances-tutorial-prepare-acr.md).</span><span class="sxs-lookup"><span data-stu-id="77ddc-105">In previous sections, [a container image was created](container-instances-tutorial-prepare-app.md) and [pushed to an Azure Container Registry](container-instances-tutorial-prepare-acr.md).</span></span> <span data-ttu-id="77ddc-106">Det här avsnittet Slutför kursen genom att distribuera behållaren till Azure-Behållarinstanser.</span><span class="sxs-lookup"><span data-stu-id="77ddc-106">This section completes the tutorial by deploying the container to Azure Container Instances.</span></span> <span data-ttu-id="77ddc-107">Slutfört stegen innefattar:</span><span class="sxs-lookup"><span data-stu-id="77ddc-107">Steps completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="77ddc-108">Ange en behållare grupp med en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="77ddc-108">Defining a container group using an Azure Resource Manager template</span></span>
> * <span data-ttu-id="77ddc-109">Distribuera behållargruppen med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="77ddc-109">Deploying the container group using the Azure CLI</span></span>
> * <span data-ttu-id="77ddc-110">Visa behållaren loggar</span><span class="sxs-lookup"><span data-stu-id="77ddc-110">Viewing container logs</span></span>

## <a name="deploy-the-container-using-the-azure-cli"></a><span data-ttu-id="77ddc-111">Distribuera behållare med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="77ddc-111">Deploy the container using the Azure CLI</span></span>

<span data-ttu-id="77ddc-112">Azure CLI gör det möjligt för distribution av en behållare till Azure-Behållarinstanser i ett enda kommando.</span><span class="sxs-lookup"><span data-stu-id="77ddc-112">The Azure CLI enables deployment of a container to Azure Container Instances in a single command.</span></span> <span data-ttu-id="77ddc-113">Eftersom bilden behållaren finns i registret för privat Azure-behållare, måste du inkludera de autentiseringsuppgifter som krävs för att komma åt den.</span><span class="sxs-lookup"><span data-stu-id="77ddc-113">Since the container image is hosted in the private Azure Container Registry, you must include the credentials required to access it.</span></span> <span data-ttu-id="77ddc-114">Om det behövs kan du fråga dem enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="77ddc-114">If necessary, you can query them as shown below.</span></span>

<span data-ttu-id="77ddc-115">Behållaren registret inloggningsserver (uppdatera ditt namn i registret):</span><span class="sxs-lookup"><span data-stu-id="77ddc-115">Container registry login server (update with your registry name):</span></span>

```azurecli-interactive
az acr show --name <acrName> --query loginServer
```

<span data-ttu-id="77ddc-116">Behållaren registret lösenord:</span><span class="sxs-lookup"><span data-stu-id="77ddc-116">Container registry password:</span></span>

```azurecli-interactive
az acr credential show --name <acrName> --query "passwords[0].value"
```

<span data-ttu-id="77ddc-117">För att distribuera avbildningen behållare från behållaren registret med en resursbegäran av 1 processorkärna och 1GB minne, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="77ddc-117">To deploy your container image from the container registry with a resource request of 1 CPU core and 1GB of memory, run the following command:</span></span>

```azurecli-interactive
az container create --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-password <acrPassword> --ip-address public -g myResourceGroup
```

<span data-ttu-id="77ddc-118">Inom några sekunder får du ett första svar från Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="77ddc-118">Within a few seconds, you will receive an initial response from Azure Resource Manager.</span></span> <span data-ttu-id="77ddc-119">Om du vill visa status för distributionen, använder du:</span><span class="sxs-lookup"><span data-stu-id="77ddc-119">To view the state of the deployment, use:</span></span>

```azurecli-interactive
az container show --name aci-tutorial-app --resource-group myResourceGroup --query state
```

<span data-ttu-id="77ddc-120">Vi kan fortsätta att köra det här kommandot förrän tillståndet ändras från *väntande* till *kör*.</span><span class="sxs-lookup"><span data-stu-id="77ddc-120">We can continue running this command until the state changes from *pending* to *running*.</span></span> <span data-ttu-id="77ddc-121">Vi kan sedan fortsätta.</span><span class="sxs-lookup"><span data-stu-id="77ddc-121">Then we can proceed.</span></span>

## <a name="view-the-application-and-container-logs"></a><span data-ttu-id="77ddc-122">Kontrollera händelseloggarna för programmet och en behållare</span><span class="sxs-lookup"><span data-stu-id="77ddc-122">View the application and container logs</span></span>

<span data-ttu-id="77ddc-123">När distributionen lyckas, öppna webbläsaren till IP-adressen som visas i utdata från kommandot:</span><span class="sxs-lookup"><span data-stu-id="77ddc-123">Once the deployment succeeds, open your browser to the IP address shown in the output of the following command:</span></span>

```bash
az container show --name aci-tutorial-app --resource-group myResourceGroup --query ipAddress.ip
```

```json
"13.88.176.27"
```

![Hello world-app i webbläsaren][aci-app-browser]

<span data-ttu-id="77ddc-125">Du kan också visa loggutdata på behållaren:</span><span class="sxs-lookup"><span data-stu-id="77ddc-125">You can also view the log output of the container:</span></span>

```azurecli-interactive
az container logs --name aci-tutorial-app -g myResourceGroup
```

<span data-ttu-id="77ddc-126">Resultat:</span><span class="sxs-lookup"><span data-stu-id="77ddc-126">Output:</span></span>

```bash
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://13.88.176.27/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="next-steps"></a><span data-ttu-id="77ddc-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="77ddc-127">Next steps</span></span>

<span data-ttu-id="77ddc-128">I kursen får slutfört du processen för att distribuera din behållare till Behållarinstanser som Azure.</span><span class="sxs-lookup"><span data-stu-id="77ddc-128">In this tutorial, you completed the process of deploying your containers to Azure Container Instances.</span></span> <span data-ttu-id="77ddc-129">Följande steg har slutförts:</span><span class="sxs-lookup"><span data-stu-id="77ddc-129">The following steps were completed:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="77ddc-130">Distribuera behållare från Azure-behållare registret med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="77ddc-130">Deploying the container from the Azure Container Registry using the Azure CLI</span></span>
> * <span data-ttu-id="77ddc-131">Visar programmet i webbläsaren</span><span class="sxs-lookup"><span data-stu-id="77ddc-131">Viewing the application in the browser</span></span>
> * <span data-ttu-id="77ddc-132">Visa loggar för behållaren</span><span class="sxs-lookup"><span data-stu-id="77ddc-132">Viewing the container logs</span></span>

<!-- LINKS -->
[prepare-app]: ./container-instances-tutorial-prepare-app.md

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png
