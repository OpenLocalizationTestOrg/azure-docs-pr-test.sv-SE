---
title: "Ansluta Intel modern (nod) tooAzure IoT - lektionen 1: Hämta verktyg (Windows) | Microsoft Docs"
description: "Hämta och installera nödvändiga hello-verktyg och programvara för hello första exempelprogram för modern för Windows 7 och senare versioner."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino utvecklingsverktyg, iot utveckling, iot-programvara, internet saker programvara, installera git för windows, installera node js windows"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 4164b5a1-5a42-4d8a-9ff6-441e79fcc936
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 933cc585d1b8b0236d76452f5c449ae9f2f3987b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-windows-7-or-later"></a><span data-ttu-id="64d42-104">Hämta hello verktyg (Windows 7 eller senare)</span><span class="sxs-lookup"><span data-stu-id="64d42-104">Get hello tools (Windows 7 or later)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="64d42-105">[Windows 7 eller senare][windows]</span><span class="sxs-lookup"><span data-stu-id="64d42-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="64d42-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="64d42-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="64d42-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="64d42-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="64d42-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="64d42-108">What you will do</span></span>
<span data-ttu-id="64d42-109">Hämta hello utvecklingsverktyg och hello programvara för hello första exempelprogram för Intel modern.</span><span class="sxs-lookup"><span data-stu-id="64d42-109">Download hello development tools and hello software for hello first sample application for Intel Edison.</span></span> <span data-ttu-id="64d42-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="64d42-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="64d42-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="64d42-111">What you will learn</span></span>
<span data-ttu-id="64d42-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="64d42-112">In this article, you will learn:</span></span>

* <span data-ttu-id="64d42-113">Hur tooinstall Git och Node.js.</span><span class="sxs-lookup"><span data-stu-id="64d42-113">How tooinstall Git and Node.js.</span></span>
  * <span data-ttu-id="64d42-114">[Git](https://git-scm.com) är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="64d42-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="64d42-115">hello-exempelprogram för den här artikeln är lagrade på Git.</span><span class="sxs-lookup"><span data-stu-id="64d42-115">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="64d42-116">[Node.js](https://nodejs.org/en/) är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="64d42-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="64d42-117">Hur toouse NPM tooinstall-utvecklingsverktyg med ytterligare Node.js.</span><span class="sxs-lookup"><span data-stu-id="64d42-117">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="64d42-118">hello kravet på minimiversion av Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="64d42-118">hello minimum version requirement of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="64d42-119">[NPM](https://www.npmjs.com) är en av hello paketet chefer för Node.js.</span><span class="sxs-lookup"><span data-stu-id="64d42-119">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="64d42-120">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="64d42-120">What you need</span></span>

<span data-ttu-id="64d42-121">toocomplete den här åtgärden behöver du:</span><span class="sxs-lookup"><span data-stu-id="64d42-121">toocomplete this operation, you will need:</span></span>

* <span data-ttu-id="64d42-122">En Internet-anslutning toodownload hello utvecklingsverktyg och hello programvara.</span><span class="sxs-lookup"><span data-stu-id="64d42-122">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="64d42-123">En dator som kör Windows.</span><span class="sxs-lookup"><span data-stu-id="64d42-123">A computer that is running Windows.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="64d42-124">Installera Git och Node.js</span><span class="sxs-lookup"><span data-stu-id="64d42-124">Install Git and Node.js</span></span>

<span data-ttu-id="64d42-125">Klicka på hello länkarna nedan toodownload och installera Git och Node.js LTS för Windows.</span><span class="sxs-lookup"><span data-stu-id="64d42-125">Click hello links below toodownload and install Git and Node.js LTS for Windows.</span></span>

* [<span data-ttu-id="64d42-126">Hämta Git för Windows</span><span class="sxs-lookup"><span data-stu-id="64d42-126">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
* [<span data-ttu-id="64d42-127">Hämta Node.js LTS för Windows</span><span class="sxs-lookup"><span data-stu-id="64d42-127">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="64d42-128">Installera ytterligare utvecklingsverktyg för Node.js</span><span class="sxs-lookup"><span data-stu-id="64d42-128">Install additional Node.js development tools</span></span>

<span data-ttu-id="64d42-129">Använd [gulp.js](http://gulpjs.com) tooautomate hello distribution av hello exempel programmet tooEdison.</span><span class="sxs-lookup"><span data-stu-id="64d42-129">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooEdison.</span></span>

<span data-ttu-id="64d42-130">Starta en kommandotolk som administratör.</span><span class="sxs-lookup"><span data-stu-id="64d42-130">Start a command prompt as an administrator.</span></span> <span data-ttu-id="64d42-131">Installera `gulp` genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="64d42-131">Install `gulp` by running hello following command:</span></span>

```cmd
npm install -g gulp
```

<span data-ttu-id="64d42-132">Om du får problem med installationen Node.js och dessa ytterligare Node.js-utvecklingsverktyg på datorn läser hello [felsökningsguide för] [ troubleshooting] för lösningar toocommon problem.</span><span class="sxs-lookup"><span data-stu-id="64d42-132">If you experience issues installing Node.js and these additional Node.js development tools on your computer, see hello [troubleshooting guide][troubleshooting] for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="64d42-133">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="64d42-133">Install Visual Studio Code</span></span>

<span data-ttu-id="64d42-134">[Hämta](https://code.visualstudio.com/docs/setup/windows) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="64d42-134">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span> <span data-ttu-id="64d42-135">Visual Studio-koden är en enkel men kraftfull källa redigerare för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="64d42-135">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="64d42-136">Du kan använda den här redigeraren för senare i hello självstudiekursen tooedit hello exempelkod.</span><span class="sxs-lookup"><span data-stu-id="64d42-136">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="64d42-137">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="64d42-137">Summary</span></span>

<span data-ttu-id="64d42-138">Du har installerat hello krävs utvecklingsverktyg och programvara för hello första exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="64d42-138">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="64d42-139">hello nästa uppgift är toocreate, distribuera och köra hello exempelprogrammet på modern.</span><span class="sxs-lookup"><span data-stu-id="64d42-139">hello next task is toocreate, deploy, and run hello sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="64d42-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="64d42-140">Next steps</span></span>

<span data-ttu-id="64d42-141">[Skapa och distribuera hello blinka program][create-and-deploy-the-blink-application]</span><span class="sxs-lookup"><span data-stu-id="64d42-141">[Create and deploy hello blink application][create-and-deploy-the-blink-application]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-node-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-mac.md
