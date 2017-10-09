---
title: "Connect Intel EDISON (C) tooAzure IoT - lektionen 1: Hämta verktyg (macOS) | Microsoft Docs"
description: "Hämta och installera nödvändiga hello-verktyg och programvara för hello första exempelprogram för modern på macOS."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino utvecklingsverktyg, iot utveckling, iot-programvara, internet saker programvara, installera git på mac installera node js mac"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 3f764f2e-25fa-4dde-9e8d-d278453fccfd
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 4a53331b0dce73c3dd51de91f07df86e28cbb6b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-macos-1010"></a><span data-ttu-id="36f1b-104">Hämta hello verktyg (macOS 10.10)</span><span class="sxs-lookup"><span data-stu-id="36f1b-104">Get hello tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="36f1b-105">[Windows 7 eller senare][windows]</span><span class="sxs-lookup"><span data-stu-id="36f1b-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="36f1b-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="36f1b-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="36f1b-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="36f1b-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="36f1b-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="36f1b-108">What you will do</span></span>
<span data-ttu-id="36f1b-109">Hämta hello utvecklingsverktyg och hello programvara för hello första exempelprogram för Intel-modern.</span><span class="sxs-lookup"><span data-stu-id="36f1b-109">Download hello development tools and hello software for hello first sample application for your Intel Edison.</span></span> <span data-ttu-id="36f1b-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="36f1b-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

> [!NOTE]
> <span data-ttu-id="36f1b-111">Även om hello programmeringsspråket av hello huvudsakliga logik C, Node.js verktyg som används i hello erfarenheter toobuild och distribuera exempelprogram.</span><span class="sxs-lookup"><span data-stu-id="36f1b-111">Although hello programming language of hello main logic is C, Node.js tools are used in hello lessons toobuild and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="36f1b-112">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="36f1b-112">What you will learn</span></span>
<span data-ttu-id="36f1b-113">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="36f1b-113">In this article, you will learn:</span></span>

* <span data-ttu-id="36f1b-114">Hur tooinstall Git och Node.js.</span><span class="sxs-lookup"><span data-stu-id="36f1b-114">How tooinstall Git and Node.js.</span></span>
  * <span data-ttu-id="36f1b-115">[Git](https://git-scm.com) är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="36f1b-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="36f1b-116">hello-exempelprogram för den här artikeln är lagrade på Git.</span><span class="sxs-lookup"><span data-stu-id="36f1b-116">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="36f1b-117">[Node.js](https://nodejs.org/en/) är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="36f1b-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="36f1b-118">Hur toouse NPM tooinstall-utvecklingsverktyg med ytterligare Node.js.</span><span class="sxs-lookup"><span data-stu-id="36f1b-118">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="36f1b-119">hello version som krävs av Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="36f1b-119">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="36f1b-120">[NPM](https://www.npmjs.com) är en av hello paketet chefer för Node.js.</span><span class="sxs-lookup"><span data-stu-id="36f1b-120">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="36f1b-121">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="36f1b-121">What you need</span></span>
<span data-ttu-id="36f1b-122">toocomplete den här åtgärden behöver du:</span><span class="sxs-lookup"><span data-stu-id="36f1b-122">toocomplete this operation, you will need:</span></span>
* <span data-ttu-id="36f1b-123">En Internet-anslutning toodownload hello utvecklingsverktyg och hello programvara.</span><span class="sxs-lookup"><span data-stu-id="36f1b-123">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="36f1b-124">En Mac som kör macOS Yosemite (10.10) eller senare.</span><span class="sxs-lookup"><span data-stu-id="36f1b-124">A Mac that is running macOS Yosemite (10.10) or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="36f1b-125">Installera Git och Node.js</span><span class="sxs-lookup"><span data-stu-id="36f1b-125">Install Git and Node.js</span></span>
<span data-ttu-id="36f1b-126">tooinstall Git och Node.js, använda hello [Homebrew](http://brew.sh) paketet hanteringsverktyg genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="36f1b-126">tooinstall Git and Node.js, use hello [Homebrew](http://brew.sh) package management utility by following these steps:</span></span>

1. <span data-ttu-id="36f1b-127">Installera Homebrew.</span><span class="sxs-lookup"><span data-stu-id="36f1b-127">Install Homebrew.</span></span> <span data-ttu-id="36f1b-128">Om du redan har installerat Homebrew går toostep 2.</span><span class="sxs-lookup"><span data-stu-id="36f1b-128">If you've already installed Homebrew, go toostep 2.</span></span>

   1. <span data-ttu-id="36f1b-129">Tryck på `Cmd + Space` och ange `Terminal` tooopen en terminal.</span><span class="sxs-lookup"><span data-stu-id="36f1b-129">Press `Cmd + Space` and enter `Terminal` tooopen a terminal.</span></span>
   2. <span data-ttu-id="36f1b-130">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="36f1b-130">Run hello following command:</span></span>

      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```
2. <span data-ttu-id="36f1b-131">Installera Git och Node.js genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="36f1b-131">Install Git and Node.js by running hello following command:</span></span>

   ```bash
   brew install node git
   ```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="36f1b-132">Installera ytterligare utvecklingsverktyg för Node.js</span><span class="sxs-lookup"><span data-stu-id="36f1b-132">Install additional Node.js development tools</span></span>
<span data-ttu-id="36f1b-133">Använd [gulp.js](http://gulpjs.com) tooautomate hello distribution av hello exempel programmet tooyour modern.</span><span class="sxs-lookup"><span data-stu-id="36f1b-133">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooyour Edison.</span></span>

<span data-ttu-id="36f1b-134">Installera `gulp` genom att köra följande kommando i hello terminal hello:</span><span class="sxs-lookup"><span data-stu-id="36f1b-134">Install `gulp` by running hello following command in hello terminal:</span></span>

```bash
sudo npm install -g gulp
```

<span data-ttu-id="36f1b-135">Om du får problem med installationen Node.js och dessa ytterligare utvecklingsverktyg på macOS finns hello [felsökningsguide för] [ troubleshooting] för lösningar toocommon problem.</span><span class="sxs-lookup"><span data-stu-id="36f1b-135">If you experience issues installing Node.js and these additional development tools on macOS, see hello [troubleshooting guide][troubleshooting] for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="36f1b-136">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="36f1b-136">Install Visual Studio Code</span></span>
<span data-ttu-id="36f1b-137">[Hämta](https://code.visualstudio.com/docs/setup/osx) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="36f1b-137">[Download](https://code.visualstudio.com/docs/setup/osx) and install Visual Studio Code.</span></span> <span data-ttu-id="36f1b-138">Visual Studio-koden är en enkel men kraftfull källa redigerare för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="36f1b-138">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="36f1b-139">Du kan använda den här redigeraren för senare i hello självstudiekursen tooedit hello exempelkod.</span><span class="sxs-lookup"><span data-stu-id="36f1b-139">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="36f1b-140">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="36f1b-140">Summary</span></span>
<span data-ttu-id="36f1b-141">Du har installerat hello krävs utvecklingsverktyg och programvara för hello första exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="36f1b-141">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="36f1b-142">hello nästa uppgift är toocreate, distribuera och köra hello exempelprogrammet på modern.</span><span class="sxs-lookup"><span data-stu-id="36f1b-142">hello next task is toocreate, deploy, and run hello sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="36f1b-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="36f1b-143">Next steps</span></span>
<span data-ttu-id="36f1b-144">[Skapa och distribuera hello blinka program][create-and-deploy-the-blink-application]</span><span class="sxs-lookup"><span data-stu-id="36f1b-144">[Create and deploy hello blink application][create-and-deploy-the-blink-application]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-c-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-mac.md
