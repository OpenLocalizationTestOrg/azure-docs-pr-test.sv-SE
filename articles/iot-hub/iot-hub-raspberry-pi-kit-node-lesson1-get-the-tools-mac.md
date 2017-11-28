---
title: "Ansluta hallon Pi (nod) tooAzure IoT - lektionen 1: Hämta verktyg (macOS) | Microsoft Docs"
description: "Hämta och installera nödvändiga hello-verktyg och programvara för hello första exempelprogram för Pi på macOS."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT-utveckling iot programvara, internet saker programvara, python mac, installera git på mac, gulp kör, installera node js mac"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 2ea6d211-c0e8-4ade-ac69-d1c2261d78c4
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 382b066cb7ece7ffdeb22b162b725727b22e5fac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-macos-1010"></a><span data-ttu-id="185ce-104">Hämta hello verktyg (macOS 10.10)</span><span class="sxs-lookup"><span data-stu-id="185ce-104">Get hello tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="185ce-105">Windows 7 eller senare</span><span class="sxs-lookup"><span data-stu-id="185ce-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="185ce-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="185ce-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="185ce-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="185ce-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="185ce-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="185ce-108">What you will do</span></span>
<span data-ttu-id="185ce-109">Hämta hello utvecklingsverktyg och hello programvara hello första exempelprogram för din hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="185ce-109">Download hello development tools and hello software for hello first sample application for your Raspberry Pi 3.</span></span> <span data-ttu-id="185ce-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="185ce-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="185ce-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="185ce-111">What you will learn</span></span>
<span data-ttu-id="185ce-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="185ce-112">In this article, you will learn:</span></span>

* <span data-ttu-id="185ce-113">Hur tooinstall Git och Node.js.</span><span class="sxs-lookup"><span data-stu-id="185ce-113">How tooinstall Git and Node.js.</span></span>
  * <span data-ttu-id="185ce-114">[Git](https://git-scm.com) är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="185ce-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="185ce-115">hello-exempelprogram för den här artikeln är lagrade på Git.</span><span class="sxs-lookup"><span data-stu-id="185ce-115">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="185ce-116">[Node.js](https://nodejs.org/en/) är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="185ce-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="185ce-117">Hur toouse NPM tooinstall-utvecklingsverktyg med ytterligare Node.js.</span><span class="sxs-lookup"><span data-stu-id="185ce-117">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="185ce-118">hello version som krävs av Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="185ce-118">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="185ce-119">[NPM](https://www.npmjs.com) är en av hello paketet chefer för Node.js.</span><span class="sxs-lookup"><span data-stu-id="185ce-119">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="185ce-120">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="185ce-120">What you need</span></span>
<span data-ttu-id="185ce-121">toocomplete den här åtgärden behöver du:</span><span class="sxs-lookup"><span data-stu-id="185ce-121">toocomplete this operation, you will need:</span></span>

* <span data-ttu-id="185ce-122">En Internet-anslutning toodownload hello utvecklingsverktyg och hello programvara.</span><span class="sxs-lookup"><span data-stu-id="185ce-122">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="185ce-123">En Mac som kör macOS Yosemite (10.10) eller senare.</span><span class="sxs-lookup"><span data-stu-id="185ce-123">A Mac that is running macOS Yosemite (10.10) or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="185ce-124">Installera Git och Node.js</span><span class="sxs-lookup"><span data-stu-id="185ce-124">Install Git and Node.js</span></span>
<span data-ttu-id="185ce-125">tooinstall Git och Node.js, använda hello [Homebrew](http://brew.sh) paketet hanteringsverktyg genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="185ce-125">tooinstall Git and Node.js, use hello [Homebrew](http://brew.sh) package management utility by following these steps:</span></span>

1. <span data-ttu-id="185ce-126">Installera Homebrew.</span><span class="sxs-lookup"><span data-stu-id="185ce-126">Install Homebrew.</span></span> <span data-ttu-id="185ce-127">Om du redan har installerat Homebrew går toostep 2.</span><span class="sxs-lookup"><span data-stu-id="185ce-127">If you've already installed Homebrew, go toostep 2.</span></span>
   
   1. <span data-ttu-id="185ce-128">Tryck på `Cmd + Space` och ange `Terminal` tooopen en terminal.</span><span class="sxs-lookup"><span data-stu-id="185ce-128">Press `Cmd + Space` and enter `Terminal` tooopen a terminal.</span></span>
   2. <span data-ttu-id="185ce-129">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="185ce-129">Run hello following command:</span></span>
      
      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```
2. <span data-ttu-id="185ce-130">Installera Git och Node.js genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="185ce-130">Install Git and Node.js by running hello following command:</span></span>
   
   ```bash
   brew install node git
   ```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="185ce-131">Installera ytterligare utvecklingsverktyg för Node.js</span><span class="sxs-lookup"><span data-stu-id="185ce-131">Install additional Node.js development tools</span></span>
<span data-ttu-id="185ce-132">Använd [gulp.js](http://gulpjs.com) tooautomate hello distribution av hello exempel programmet tooPi.</span><span class="sxs-lookup"><span data-stu-id="185ce-132">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooPi.</span></span> <span data-ttu-id="185ce-133">Använd hello [enhet-discovery-cli](https://github.com/Azure/device-discovery-cli) tooretrieve network information om din IoT-enheter.</span><span class="sxs-lookup"><span data-stu-id="185ce-133">Use hello [device-discovery-cli](https://github.com/Azure/device-discovery-cli) tooretrieve network information about your IoT devices.</span></span>

<span data-ttu-id="185ce-134">Installera `gulp` och `device-discovery-cli` genom att köra följande kommando i hello terminal hello:</span><span class="sxs-lookup"><span data-stu-id="185ce-134">Install `gulp` and `device-discovery-cli` by running hello following command in hello terminal:</span></span>

```bash
npm install -g device-discovery-cli gulp
```

<span data-ttu-id="185ce-135">Om du får problem med installationen Node.js och dessa ytterligare utvecklingsverktyg på macOS finns hello [felsökningsguide för](iot-hub-raspberry-pi-kit-node-troubleshooting.md) för lösningar toocommon problem.</span><span class="sxs-lookup"><span data-stu-id="185ce-135">If you experience issues installing Node.js and these additional development tools on macOS, see hello [troubleshooting guide](iot-hub-raspberry-pi-kit-node-troubleshooting.md) for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="185ce-136">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="185ce-136">Install Visual Studio Code</span></span>
<span data-ttu-id="185ce-137">[Hämta](https://code.visualstudio.com/docs/setup/osx) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="185ce-137">[Download](https://code.visualstudio.com/docs/setup/osx) and install Visual Studio Code.</span></span> <span data-ttu-id="185ce-138">Visual Studio-koden är en enkel men kraftfull källa redigerare för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="185ce-138">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="185ce-139">Du kan använda den här redigeraren för senare i hello självstudiekursen tooedit hello exempelkod.</span><span class="sxs-lookup"><span data-stu-id="185ce-139">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="185ce-140">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="185ce-140">Summary</span></span>
<span data-ttu-id="185ce-141">Du har installerat hello krävs utvecklingsverktyg och programvara för hello första exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="185ce-141">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="185ce-142">hello nästa uppgift är toocreate, distribuera och köra hello exempelprogrammet på Pi.</span><span class="sxs-lookup"><span data-stu-id="185ce-142">hello next task is toocreate, deploy, and run hello sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="185ce-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="185ce-143">Next steps</span></span>
[<span data-ttu-id="185ce-144">Skapa och distribuera hello blinka exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="185ce-144">Create and deploy hello blink sample application</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

