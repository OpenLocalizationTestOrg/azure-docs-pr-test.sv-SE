---
title: "Connect Raspberry PI (C) tooAzure IoT - lektionen 1: Hämta verktyg (Windows) | Microsoft Docs"
description: "Hämta och installera nödvändiga hello-verktyg och programvara för hello första exempelprogram för Pi för Windows 7 och senare versioner."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT-utveckling iot programvara, internet saker programvara, installera git för windows, node js windows, installera npm i windows"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: bd765ddd-65b7-4241-a391-dc77cb3af1c0
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 70ae6d15f9d6af116ff065a79a30d99afc67bffd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-windows-7-or-later"></a><span data-ttu-id="b91bb-104">Hämta hello verktyg (Windows 7 eller senare)</span><span class="sxs-lookup"><span data-stu-id="b91bb-104">Get hello tools (Windows 7 or later)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b91bb-105">Windows 7 eller senare</span><span class="sxs-lookup"><span data-stu-id="b91bb-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="b91bb-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="b91bb-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="b91bb-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="b91bb-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="b91bb-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="b91bb-108">What you will do</span></span>
<span data-ttu-id="b91bb-109">Hämta hello utvecklingsverktyg och hello programvara hello första exempelprogram för hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="b91bb-109">Download hello development tools and hello software for hello first sample application for Raspberry Pi 3.</span></span> <span data-ttu-id="b91bb-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="b91bb-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

> [!NOTE]
> <span data-ttu-id="b91bb-111">Även om hello programmeringsspråket av hello huvudsakliga logik C, Node.js verktyg som används i hello erfarenheter toodiscover enheter och skapa och distribuera exempelprogram.</span><span class="sxs-lookup"><span data-stu-id="b91bb-111">Although hello programming language of hello main logic is C, Node.js tools are used in hello lessons toodiscover devices, and build and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="b91bb-112">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="b91bb-112">What you will learn</span></span>
<span data-ttu-id="b91bb-113">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="b91bb-113">In this article, you will learn:</span></span>

* <span data-ttu-id="b91bb-114">Hur tooinstall Git och Node.js.</span><span class="sxs-lookup"><span data-stu-id="b91bb-114">How tooinstall Git and Node.js.</span></span>
  * <span data-ttu-id="b91bb-115">[Git](https://git-scm.com) är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="b91bb-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="b91bb-116">hello-exempelprogram för den här artikeln är lagrade på Git.</span><span class="sxs-lookup"><span data-stu-id="b91bb-116">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="b91bb-117">[Node.js](https://nodejs.org/en/) är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="b91bb-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="b91bb-118">Hur toouse NPM tooinstall-utvecklingsverktyg med ytterligare Node.js.</span><span class="sxs-lookup"><span data-stu-id="b91bb-118">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="b91bb-119">hello kravet på minimiversion av Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="b91bb-119">hello minimum version requirement of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="b91bb-120">[NPM](https://www.npmjs.com) är en av hello paketet chefer för Node.js.</span><span class="sxs-lookup"><span data-stu-id="b91bb-120">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="b91bb-121">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="b91bb-121">What you need</span></span>

<span data-ttu-id="b91bb-122">toocomplete den här åtgärden behöver du:</span><span class="sxs-lookup"><span data-stu-id="b91bb-122">toocomplete this operation, you will need:</span></span>

* <span data-ttu-id="b91bb-123">En Internet-anslutning toodownload hello utvecklingsverktyg och hello programvara.</span><span class="sxs-lookup"><span data-stu-id="b91bb-123">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="b91bb-124">En dator som kör Windows.</span><span class="sxs-lookup"><span data-stu-id="b91bb-124">A computer that is running Windows.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="b91bb-125">Installera Git och Node.js</span><span class="sxs-lookup"><span data-stu-id="b91bb-125">Install Git and Node.js</span></span>

<span data-ttu-id="b91bb-126">Klicka på hello länkarna nedan toodownload och installera Git och Node.js LTS för Windows.</span><span class="sxs-lookup"><span data-stu-id="b91bb-126">Click hello links below toodownload and install Git and Node.js LTS for Windows.</span></span>

* [<span data-ttu-id="b91bb-127">Hämta Git för Windows</span><span class="sxs-lookup"><span data-stu-id="b91bb-127">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
* [<span data-ttu-id="b91bb-128">Hämta Node.js LTS för Windows</span><span class="sxs-lookup"><span data-stu-id="b91bb-128">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="b91bb-129">Installera ytterligare utvecklingsverktyg för Node.js</span><span class="sxs-lookup"><span data-stu-id="b91bb-129">Install additional Node.js development tools</span></span>

<span data-ttu-id="b91bb-130">Använd [gulp.js](http://gulpjs.com) tooautomate hello distribution av hello exempel programmet tooPi.</span><span class="sxs-lookup"><span data-stu-id="b91bb-130">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooPi.</span></span> <span data-ttu-id="b91bb-131">Använd hello [enhet-discovery-cli](https://github.com/Azure/device-discovery-cli) tooretrieve network information om din IoT-enheter.</span><span class="sxs-lookup"><span data-stu-id="b91bb-131">Use hello [device-discovery-cli](https://github.com/Azure/device-discovery-cli) tooretrieve network information about your IoT devices.</span></span>

<span data-ttu-id="b91bb-132">Starta en kommandotolk som administratör.</span><span class="sxs-lookup"><span data-stu-id="b91bb-132">Start a command prompt as an administrator.</span></span> <span data-ttu-id="b91bb-133">Installera `gulp` och `device-discovery-cli` genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="b91bb-133">Install `gulp` and `device-discovery-cli` by running hello following command:</span></span>

```bash
npm install -g device-discovery-cli gulp
```

<span data-ttu-id="b91bb-134">Om du får problem med installationen Node.js och dessa ytterligare Node.js-utvecklingsverktyg på datorn läser hello [felsökningsguide för](iot-hub-raspberry-pi-kit-c-troubleshooting.md) för lösningar toocommon problem.</span><span class="sxs-lookup"><span data-stu-id="b91bb-134">If you experience issues installing Node.js and these additional Node.js development tools on your computer, see hello [troubleshooting guide](iot-hub-raspberry-pi-kit-c-troubleshooting.md) for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="b91bb-135">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b91bb-135">Install Visual Studio Code</span></span>

<span data-ttu-id="b91bb-136">[Hämta](https://code.visualstudio.com/docs/setup/windows) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="b91bb-136">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span> <span data-ttu-id="b91bb-137">Visual Studio-koden är en enkel men kraftfull källa redigerare för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="b91bb-137">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="b91bb-138">Du kan använda den här redigeraren för senare i hello självstudiekursen tooedit hello exempelkod.</span><span class="sxs-lookup"><span data-stu-id="b91bb-138">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="b91bb-139">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="b91bb-139">Summary</span></span>

<span data-ttu-id="b91bb-140">Du har installerat hello krävs utvecklingsverktyg och programvara för hello första exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="b91bb-140">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="b91bb-141">hello nästa uppgift är toocreate, distribuera och köra hello exempelprogrammet på Pi.</span><span class="sxs-lookup"><span data-stu-id="b91bb-141">hello next task is toocreate, deploy, and run hello sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b91bb-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b91bb-142">Next steps</span></span>

[<span data-ttu-id="b91bb-143">Skapa och distribuera hello blinka program</span><span class="sxs-lookup"><span data-stu-id="b91bb-143">Create and deploy hello blink application</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md)
