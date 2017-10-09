---
title: "Ansluta hallon Pi (nod) tooAzure IoT - lektionen 1: Hämta verktyg (Windows) | Microsoft Docs"
description: "Hämta och installera nödvändiga hello-verktyg och programvara för hello första exempelprogram för Pi för Windows 7 och senare versioner."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT-utveckling iot programvara, internet saker programvara, installera git för windows, gulp kör, node js windows, installera npm i windows, installera python i windows"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: b3d88e17-97cc-4f23-85fd-a688fc228eb8
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: ea7f77cc79c70c8c7952b63462b926471fcf71cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-windows-7-or-later"></a><span data-ttu-id="32d47-104">Hämta hello verktyg (Windows 7 eller senare)</span><span class="sxs-lookup"><span data-stu-id="32d47-104">Get hello tools (Windows 7 or later)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="32d47-105">Windows 7 eller senare</span><span class="sxs-lookup"><span data-stu-id="32d47-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="32d47-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="32d47-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="32d47-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="32d47-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)


## <a name="what-you-will-do"></a><span data-ttu-id="32d47-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="32d47-108">What you will do</span></span>
<span data-ttu-id="32d47-109">Hämta hello utvecklingsverktyg och hello programvara hello första exempelprogram för hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="32d47-109">Download hello development tools and hello software for hello first sample application for Raspberry Pi 3.</span></span> <span data-ttu-id="32d47-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="32d47-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="32d47-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="32d47-111">What you will learn</span></span>
<span data-ttu-id="32d47-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="32d47-112">In this article, you will learn:</span></span>

* <span data-ttu-id="32d47-113">Hur tooinstall Git och Node.js.</span><span class="sxs-lookup"><span data-stu-id="32d47-113">How tooinstall Git and Node.js.</span></span>
  * <span data-ttu-id="32d47-114">[Git](https://git-scm.com) är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="32d47-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="32d47-115">hello-exempelprogram för den här artikeln är lagrade på Git.</span><span class="sxs-lookup"><span data-stu-id="32d47-115">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="32d47-116">[Node.js](https://nodejs.org/en/) är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="32d47-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="32d47-117">Hur toouse NPM tooinstall-utvecklingsverktyg med ytterligare Node.js.</span><span class="sxs-lookup"><span data-stu-id="32d47-117">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="32d47-118">hello kravet på minimiversion av Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="32d47-118">hello minimum version requirement of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="32d47-119">[NPM](https://www.npmjs.com) är en av hello paketet chefer för Node.js.</span><span class="sxs-lookup"><span data-stu-id="32d47-119">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="32d47-120">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="32d47-120">What you need</span></span>
<span data-ttu-id="32d47-121">toocomplete den här åtgärden behöver du:</span><span class="sxs-lookup"><span data-stu-id="32d47-121">toocomplete this operation, you will need:</span></span>

* <span data-ttu-id="32d47-122">En Internet-anslutning toodownload hello utvecklingsverktyg och hello programvara.</span><span class="sxs-lookup"><span data-stu-id="32d47-122">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="32d47-123">En dator som kör Windows.</span><span class="sxs-lookup"><span data-stu-id="32d47-123">A computer that is running Windows.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="32d47-124">Installera Git och Node.js</span><span class="sxs-lookup"><span data-stu-id="32d47-124">Install Git and Node.js</span></span>
<span data-ttu-id="32d47-125">Klicka på följande länkar toodownload hello och installera Git och Node.js LTS för Windows.</span><span class="sxs-lookup"><span data-stu-id="32d47-125">Click hello following links toodownload and install Git and Node.js LTS for Windows.</span></span>

* [<span data-ttu-id="32d47-126">Hämta Git för Windows</span><span class="sxs-lookup"><span data-stu-id="32d47-126">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
* [<span data-ttu-id="32d47-127">Hämta Node.js LTS för Windows</span><span class="sxs-lookup"><span data-stu-id="32d47-127">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="32d47-128">Installera ytterligare utvecklingsverktyg för Node.js</span><span class="sxs-lookup"><span data-stu-id="32d47-128">Install additional Node.js development tools</span></span>
<span data-ttu-id="32d47-129">Använd [gulp.js](http://gulpjs.com) tooautomate hello distribution av hello exempel programmet tooPi.</span><span class="sxs-lookup"><span data-stu-id="32d47-129">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooPi.</span></span> <span data-ttu-id="32d47-130">Du också använda hello [enhet-discovery-cli](https://github.com/Azure/device-discovery-cli) tooretrieve network information om din IoT-enheter.</span><span class="sxs-lookup"><span data-stu-id="32d47-130">You also use hello [device-discovery-cli](https://github.com/Azure/device-discovery-cli) tooretrieve network information about your IoT devices.</span></span>

<span data-ttu-id="32d47-131">Starta en kommandotolk som administratör.</span><span class="sxs-lookup"><span data-stu-id="32d47-131">Start a command prompt as an administrator.</span></span> <span data-ttu-id="32d47-132">Installera `gulp` och `device-discovery-cli` genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="32d47-132">Install `gulp` and `device-discovery-cli` by running hello following command:</span></span>

```bash
npm install -g device-discovery-cli gulp
```

<span data-ttu-id="32d47-133">Om du får problem med installationen Node.js och dessa ytterligare Node.js-utvecklingsverktyg på datorn läser hello [felsökningsguide för](iot-hub-raspberry-pi-kit-node-troubleshooting.md) för lösningar toocommon problem.</span><span class="sxs-lookup"><span data-stu-id="32d47-133">If you experience issues installing Node.js and these additional Node.js development tools on your computer, see hello [troubleshooting guide](iot-hub-raspberry-pi-kit-node-troubleshooting.md) for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="32d47-134">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="32d47-134">Install Visual Studio Code</span></span>
<span data-ttu-id="32d47-135">[Hämta](https://code.visualstudio.com/docs/setup/windows) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="32d47-135">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span> <span data-ttu-id="32d47-136">Visual Studio-koden är en enkel men kraftfull källa redigerare för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="32d47-136">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="32d47-137">Du kan använda den här redigeraren för senare i hello självstudiekursen tooedit hello exempelkod.</span><span class="sxs-lookup"><span data-stu-id="32d47-137">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="32d47-138">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="32d47-138">Summary</span></span>
<span data-ttu-id="32d47-139">Du har installerat hello krävs utvecklingsverktyg och programvara för hello första exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="32d47-139">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="32d47-140">hello nästa uppgift är toocreate, distribuera och köra hello exempelprogrammet på Pi.</span><span class="sxs-lookup"><span data-stu-id="32d47-140">hello next task is toocreate, deploy, and run hello sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="32d47-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="32d47-141">Next steps</span></span>
[<span data-ttu-id="32d47-142">Skapa och distribuera hello blinka exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="32d47-142">Create and deploy hello blink sample application</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

