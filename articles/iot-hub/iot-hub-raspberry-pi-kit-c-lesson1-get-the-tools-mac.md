---
title: "Connect Raspberry PI (C) tooAzure IoT - lektionen 1: Hämta verktyg (macOS) | Microsoft Docs"
description: "Hämta och installera nödvändiga hello-verktyg och programvara för hello första exempelprogram för Pi på macOS."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT-utveckling, iot program, internet saker programvara, installera git på mac gulp kör, installera node js mac"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: fc6bd2c8-a847-4bf5-818f-6f7f9a6835ee
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 68ee44945dd69edcdf61ab3da4c80379c0ef955d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-macos-1010"></a><span data-ttu-id="53ebb-104">Hämta hello verktyg (macOS 10.10)</span><span class="sxs-lookup"><span data-stu-id="53ebb-104">Get hello tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="53ebb-105">Windows 7 eller senare</span><span class="sxs-lookup"><span data-stu-id="53ebb-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="53ebb-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="53ebb-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="53ebb-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="53ebb-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="53ebb-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="53ebb-108">What you will do</span></span>
<span data-ttu-id="53ebb-109">Hämta hello utvecklingsverktyg och hello programvara hello första exempelprogram för din hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="53ebb-109">Download hello development tools and hello software for hello first sample application for your Raspberry Pi 3.</span></span> <span data-ttu-id="53ebb-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="53ebb-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

> [!NOTE]
> <span data-ttu-id="53ebb-111">Även om hello programmeringsspråket av hello huvudsakliga logik C, Node.js verktyg som används i hello erfarenheter toodiscover enheter och skapa och distribuera exempelprogram.</span><span class="sxs-lookup"><span data-stu-id="53ebb-111">Although hello programming language of hello main logic is C, Node.js tools are used in hello lessons toodiscover devices, and build and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="53ebb-112">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="53ebb-112">What you will learn</span></span>
<span data-ttu-id="53ebb-113">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="53ebb-113">In this article, you will learn:</span></span>

* <span data-ttu-id="53ebb-114">Hur tooinstall Git och Node.js.</span><span class="sxs-lookup"><span data-stu-id="53ebb-114">How tooinstall Git and Node.js.</span></span>
  * <span data-ttu-id="53ebb-115">[Git](https://git-scm.com) är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="53ebb-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="53ebb-116">hello-exempelprogram för den här artikeln är lagrade på Git.</span><span class="sxs-lookup"><span data-stu-id="53ebb-116">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="53ebb-117">[Node.js](https://nodejs.org/en/) är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="53ebb-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="53ebb-118">Hur toouse NPM tooinstall-utvecklingsverktyg med ytterligare Node.js.</span><span class="sxs-lookup"><span data-stu-id="53ebb-118">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="53ebb-119">hello version som krävs av Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="53ebb-119">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="53ebb-120">[NPM](https://www.npmjs.com) är en av hello paketet chefer för Node.js.</span><span class="sxs-lookup"><span data-stu-id="53ebb-120">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="53ebb-121">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="53ebb-121">What you need</span></span>
<span data-ttu-id="53ebb-122">toocomplete den här åtgärden behöver du:</span><span class="sxs-lookup"><span data-stu-id="53ebb-122">toocomplete this operation, you will need:</span></span>

* <span data-ttu-id="53ebb-123">En Internet-anslutning toodownload hello utvecklingsverktyg och hello programvara.</span><span class="sxs-lookup"><span data-stu-id="53ebb-123">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="53ebb-124">En Mac som kör macOS Yosemite (10.10) eller senare.</span><span class="sxs-lookup"><span data-stu-id="53ebb-124">A Mac that is running macOS Yosemite (10.10) or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="53ebb-125">Installera Git och Node.js</span><span class="sxs-lookup"><span data-stu-id="53ebb-125">Install Git and Node.js</span></span>
<span data-ttu-id="53ebb-126">tooinstall Git och Node.js, använda hello [Homebrew](http://brew.sh) paketet hanteringsverktyg genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="53ebb-126">tooinstall Git and Node.js, use hello [Homebrew](http://brew.sh) package management utility by following these steps:</span></span>

1. <span data-ttu-id="53ebb-127">Installera Homebrew.</span><span class="sxs-lookup"><span data-stu-id="53ebb-127">Install Homebrew.</span></span> <span data-ttu-id="53ebb-128">Om du redan har installerat Homebrew går toostep 2.</span><span class="sxs-lookup"><span data-stu-id="53ebb-128">If you've already installed Homebrew, go toostep 2.</span></span>
   
   1. <span data-ttu-id="53ebb-129">Tryck på `Cmd + Space` och ange `Terminal` tooopen en terminal.</span><span class="sxs-lookup"><span data-stu-id="53ebb-129">Press `Cmd + Space` and enter `Terminal` tooopen a terminal.</span></span>
   2. <span data-ttu-id="53ebb-130">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="53ebb-130">Run hello following command:</span></span>
      
      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```
2. <span data-ttu-id="53ebb-131">Installera Git och Node.js genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="53ebb-131">Install Git and Node.js by running hello following command:</span></span>
   
   ```bash
   brew install node git
   ```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="53ebb-132">Installera ytterligare utvecklingsverktyg för Node.js</span><span class="sxs-lookup"><span data-stu-id="53ebb-132">Install additional Node.js development tools</span></span>
<span data-ttu-id="53ebb-133">Använd [gulp.js](http://gulpjs.com) tooautomate hello distribution av hello exempel programmet tooyour Pi.</span><span class="sxs-lookup"><span data-stu-id="53ebb-133">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooyour Pi.</span></span> <span data-ttu-id="53ebb-134">Använd hello [enhet-discovery-cli](https://github.com/Azure/device-discovery-cli) tooretrieve network information om din IoT-enheter.</span><span class="sxs-lookup"><span data-stu-id="53ebb-134">Use hello [device-discovery-cli](https://github.com/Azure/device-discovery-cli) tooretrieve network information about your IoT devices.</span></span>

<span data-ttu-id="53ebb-135">Installera `gulp` och `device-discovery-cli` genom att köra följande kommando i hello terminal hello:</span><span class="sxs-lookup"><span data-stu-id="53ebb-135">Install `gulp` and `device-discovery-cli` by running hello following command in hello terminal:</span></span>

```bash
sudo npm install -g device-discovery-cli gulp
```

<span data-ttu-id="53ebb-136">Om du får problem med installationen Node.js och dessa ytterligare utvecklingsverktyg på macOS finns hello [felsökningsguide för](iot-hub-raspberry-pi-kit-c-troubleshooting.md) för lösningar toocommon problem.</span><span class="sxs-lookup"><span data-stu-id="53ebb-136">If you experience issues installing Node.js and these additional development tools on macOS, see hello [troubleshooting guide](iot-hub-raspberry-pi-kit-c-troubleshooting.md) for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="53ebb-137">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="53ebb-137">Install Visual Studio Code</span></span>
<span data-ttu-id="53ebb-138">[Hämta](https://code.visualstudio.com/docs/setup/osx) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="53ebb-138">[Download](https://code.visualstudio.com/docs/setup/osx) and install Visual Studio Code.</span></span> <span data-ttu-id="53ebb-139">Visual Studio-koden är en enkel men kraftfull källa redigerare för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="53ebb-139">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="53ebb-140">Du kan använda den här redigeraren för senare i hello självstudiekursen tooedit hello exempelkod.</span><span class="sxs-lookup"><span data-stu-id="53ebb-140">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="53ebb-141">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="53ebb-141">Summary</span></span>
<span data-ttu-id="53ebb-142">Du har installerat hello krävs utvecklingsverktyg och programvara för hello första exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="53ebb-142">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="53ebb-143">hello nästa uppgift är toocreate, distribuera och köra hello exempelprogrammet på Pi.</span><span class="sxs-lookup"><span data-stu-id="53ebb-143">hello next task is toocreate, deploy, and run hello sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="53ebb-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="53ebb-144">Next steps</span></span>
[<span data-ttu-id="53ebb-145">Skapa och distribuera hello blinka program</span><span class="sxs-lookup"><span data-stu-id="53ebb-145">Create and deploy hello blink application</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md)

