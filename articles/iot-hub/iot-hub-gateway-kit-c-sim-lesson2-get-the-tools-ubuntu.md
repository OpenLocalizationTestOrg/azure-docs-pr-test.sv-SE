---
title: "Simulerade enhet & Azure IoT Gateway - lektionen 2: Hämta verktyg (Ubuntu) | Microsoft Docs"
description: "Installera hello verktyg och hello programvara på din värddator som kör Ubuntu, skapar en IoT-hubb och registrera enheten i hello IoT-hubb."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT-utveckling, iot-programvara, iot-Molntjänsten, internet av saker programvara, azure cli git på ubuntu, gulp kör, installera node js ubuntu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: cf673154-ce67-4ed7-a9f7-2440301c6270
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 38c4d5d9cceec47758f0641cc26b631a7b19d37e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-ubuntu-1604"></a><span data-ttu-id="2d3d9-104">Hämta hello verktyg (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="2d3d9-104">Get hello tools (Ubuntu 16.04)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2d3d9-105">Windows 7 eller senare</span><span class="sxs-lookup"><span data-stu-id="2d3d9-105">Windows 7 or later</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
> * [<span data-ttu-id="2d3d9-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="2d3d9-106">Ubuntu 16.04</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="2d3d9-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="2d3d9-107">macOS 10.10</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="2d3d9-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="2d3d9-108">What you will do</span></span>

- <span data-ttu-id="2d3d9-109">Installera Git, Node.js, Gulp, Python.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-109">Install Git, Node.js, Gulp, Python.</span></span>
- <span data-ttu-id="2d3d9-110">Installera hello Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="2d3d9-110">Install hello Azure command-line interface (Azure CLI).</span></span> 

<span data-ttu-id="2d3d9-111">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="2d3d9-111">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>
## <a name="what-you-will-learn"></a><span data-ttu-id="2d3d9-112">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="2d3d9-112">What you will learn</span></span>

<span data-ttu-id="2d3d9-113">I den här lektionen får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="2d3d9-113">In this lesson, you will learn:</span></span>

- <span data-ttu-id="2d3d9-114">Hur tooinstall Git och Node.js.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-114">How tooinstall Git and Node.js.</span></span>
  - <span data-ttu-id="2d3d9-115">Git är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-115">Git is an open source distributed version control system.</span></span> <span data-ttu-id="2d3d9-116">hello-exempelprogram för den här lektionen lagras på Git.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-116">hello sample application for this lesson is stored on Git.</span></span>
  - <span data-ttu-id="2d3d9-117">Node.js är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-117">Node.js is a JavaScript runtime with a rich package ecosystem.</span></span>
- <span data-ttu-id="2d3d9-118">Hur toouse NPM tooinstall Node.js utvecklingsverktyg.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-118">How toouse NPM tooinstall Node.js development tools.</span></span>
  - <span data-ttu-id="2d3d9-119">hello version som krävs av Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-119">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  - <span data-ttu-id="2d3d9-120">NPM är en av hello paketet chefer för Node.js.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-120">NPM is one of hello package managers for Node.js.</span></span>
- <span data-ttu-id="2d3d9-121">Hur tooinstall Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-121">How tooinstall Visual Studio Code.</span></span>
  - <span data-ttu-id="2d3d9-122">Visual Studio-koden är plattformsoberoende redigerare för enkel men kraftfull källa för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-122">Visual Studio Code is a cross platform, lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="2d3d9-123">Det har bra stöd för felsökning, inbäddad Git-kontroll, syntaxmarkering, intelligent kod slutförande, kodavsnitt och koden eftersom samt.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-123">It has great support for debugging, embedded Git control, syntax highlighting, intelligent code completion, snippets, and code refactoring as well.</span></span>
- <span data-ttu-id="2d3d9-124">Hur tooinstall hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2d3d9-124">How tooinstall hello Azure CLI</span></span>
  - <span data-ttu-id="2d3d9-125">hello Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-125">hello Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="2d3d9-126">Du arbetar direkt från en kommandorad tooprovision och hantera resurser.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-126">You work directly from a command line tooprovision and manage resources.</span></span>
- <span data-ttu-id="2d3d9-127">Hur toouse hello Azure CLI toocreate en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-127">How toouse hello Azure CLI toocreate an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="2d3d9-128">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="2d3d9-128">What you need</span></span>

- <span data-ttu-id="2d3d9-129">En Internet-anslutning toodownload hello verktyg och program.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-129">An Internet connection toodownload hello tools and software.</span></span>
- <span data-ttu-id="2d3d9-130">En dator som kör Ubuntu 16.04 eller senare.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-130">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="2d3d9-131">Installera Git och Node.js</span><span class="sxs-lookup"><span data-stu-id="2d3d9-131">Install Git and Node.js</span></span>

<span data-ttu-id="2d3d9-132">tooinstall Git och Node.js, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="2d3d9-132">tooinstall Git and Node.js, follow these steps:</span></span>

1. <span data-ttu-id="2d3d9-133">Tryck på `Ctrl + Alt + T` tooopen en terminal.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-133">Press `Ctrl + Alt + T` tooopen a terminal.</span></span>
2. <span data-ttu-id="2d3d9-134">Kör följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="2d3d9-134">Run hello following commands:</span></span>

   ```bash
   sudo apt-get update
   curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
   sudo apt-get install -y nodejs
   sudo apt-get install git
   ```

## <a name="install-nodejs-development-tools"></a><span data-ttu-id="2d3d9-135">Installera Node.js utvecklingsverktyg</span><span class="sxs-lookup"><span data-stu-id="2d3d9-135">Install Node.js development tools</span></span>

<span data-ttu-id="2d3d9-136">Du använder [gulp.js](http://gulpjs.com/) tooautomate distribution och körning av skript.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-136">You use [gulp.js](http://gulpjs.com/) tooautomate deployment and execution of scripts.</span></span>

<span data-ttu-id="2d3d9-137">tooinstall gulp kör följande kommando i hello terminal hello:</span><span class="sxs-lookup"><span data-stu-id="2d3d9-137">tooinstall gulp, run hello following command in hello terminal:</span></span>

```bash
sudo npm install -g gulp
```

<span data-ttu-id="2d3d9-138">Om du får problem med hello installation finns hello [felsökningsguide för](iot-hub-gateway-kit-c-sim-troubleshooting.md) för lösningar toocommon problem.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-138">If you experience issues with hello installation, see hello [troubleshooting guide](iot-hub-gateway-kit-c-sim-troubleshooting.md) for solutions toocommon problems.</span></span>

> [!Note]
> <span data-ttu-id="2d3d9-139">Noden, NPM och Gulp är obligatoriska toorun automatiseringsskript utvecklats i Node.js.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-139">Node, NPM and Gulp are required toorun automation scripts developed in Node.js.</span></span>

## <a name="install-hello-azure-cli"></a><span data-ttu-id="2d3d9-140">Installera hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2d3d9-140">Install hello Azure CLI</span></span>

<span data-ttu-id="2d3d9-141">tooinstall hello Azure CLI, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="2d3d9-141">tooinstall hello Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="2d3d9-142">Kör följande kommandon i hello terminal hello:</span><span class="sxs-lookup"><span data-stu-id="2d3d9-142">Run hello following commands in hello terminal:</span></span>

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```

   <span data-ttu-id="2d3d9-143">hello-installationen kan ta 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-143">hello installation might take 5 minutes.</span></span>

2. <span data-ttu-id="2d3d9-144">Kontrollera hello installationen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="2d3d9-144">Verify hello installation by running hello following command:</span></span>

   ```bash
   az iot -h
   ```
<span data-ttu-id="2d3d9-145">Du bör se hello följande utdata om hello-installationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-145">You should see hello following output if hello installation is successful.</span></span>
<span data-ttu-id="2d3d9-146">![Verifiera installation av Azure CLI](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_ubuntu.png)</span><span class="sxs-lookup"><span data-stu-id="2d3d9-146">![Verify Azure CLI installation](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_ubuntu.png)</span></span>

### <a name="install-visual-studio-code"></a><span data-ttu-id="2d3d9-147">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2d3d9-147">Install Visual Studio Code</span></span>

<span data-ttu-id="2d3d9-148">Du kan använda Visual Studio Code senare i hello självstudiekursen tooedit konfigurationsfiler.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-148">You use Visual Studio Code later in hello tutorial tooedit configuration files.</span></span>

<span data-ttu-id="2d3d9-149">[Hämta](https://code.visualstudio.com/docs/setup/linux) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-149">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span>

## <a name="summary"></a><span data-ttu-id="2d3d9-150">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="2d3d9-150">Summary</span></span>

<span data-ttu-id="2d3d9-151">Du har installerat alla hello krävs verktyg och program på värddatorn.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-151">You've installed all hello required tools and software on your host computer.</span></span> <span data-ttu-id="2d3d9-152">Nästa uppgift är toouse hello Azure CLI toocreate en IoT-hubb och registrera enheten i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="2d3d9-152">Your next task is toouse hello Azure CLI toocreate an IoT hub and register your device in your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2d3d9-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2d3d9-153">Next steps</span></span>
[<span data-ttu-id="2d3d9-154">Skapa en IoT-hubb och registrera din enhet</span><span class="sxs-lookup"><span data-stu-id="2d3d9-154">Create an IoT hub and register your device</span></span>](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)
