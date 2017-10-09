---
title: "Connect Raspberry PI (C) tooAzure IoT - lektionen 1: Hämta verktyg (Ubuntu) | Microsoft Docs"
description: "Hämta och installera nödvändiga hello-verktyg och programvara för hello första exempelprogram för Pi på Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT-utveckling iot programvara, internet saker programvara, git på ubuntu, gulp kör, installera node js ubuntu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 32cfea00-c254-4cef-8f6f-bbf807eca6b6
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 794928b5da63521cb0a72cb54256f2ad9724ec84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-ubuntu-1604"></a><span data-ttu-id="720ff-104">Hämta hello verktyg (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="720ff-104">Get hello tools (Ubuntu 16.04)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="720ff-105">Windows 7 eller senare</span><span class="sxs-lookup"><span data-stu-id="720ff-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="720ff-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="720ff-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="720ff-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="720ff-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="720ff-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="720ff-108">What you will do</span></span>
<span data-ttu-id="720ff-109">Hämta hello utvecklingsverktyg och hello programvara hello första exempelprogram för din hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="720ff-109">Download hello development tools and hello software for hello first sample application for your Raspberry Pi 3.</span></span> <span data-ttu-id="720ff-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="720ff-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

> [!NOTE]
> <span data-ttu-id="720ff-111">Även om hello programmeringsspråket av hello huvudsakliga logik C, Node.js verktyg som används i hello erfarenheter toodiscover enheter och skapa och distribuera exempelprogram.</span><span class="sxs-lookup"><span data-stu-id="720ff-111">Although hello programming language of hello main logic is C, Node.js tools are used in hello lessons toodiscover devices, and build and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="720ff-112">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="720ff-112">What you will learn</span></span>
<span data-ttu-id="720ff-113">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="720ff-113">In this article, you will learn:</span></span>

* <span data-ttu-id="720ff-114">Hur tooinstall Git och Node.js</span><span class="sxs-lookup"><span data-stu-id="720ff-114">How tooinstall Git and Node.js</span></span>
  * <span data-ttu-id="720ff-115">[Git](https://git-scm.com) är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="720ff-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="720ff-116">hello-exempelprogram för den här artikeln är lagrade på Git.</span><span class="sxs-lookup"><span data-stu-id="720ff-116">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="720ff-117">[Node.js](https://nodejs.org/en/) är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="720ff-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="720ff-118">Hur toouse NPM tooinstall-utvecklingsverktyg med ytterligare Node.js.</span><span class="sxs-lookup"><span data-stu-id="720ff-118">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="720ff-119">hello version som krävs av Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="720ff-119">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="720ff-120">[NPM](https://www.npmjs.com) är en av hello paketet chefer för Node.js.</span><span class="sxs-lookup"><span data-stu-id="720ff-120">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="720ff-121">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="720ff-121">What you need</span></span>
<span data-ttu-id="720ff-122">toocomplete den här åtgärden behöver du:</span><span class="sxs-lookup"><span data-stu-id="720ff-122">toocomplete this operation, you will need:</span></span>

* <span data-ttu-id="720ff-123">En Internet-anslutning toodownload hello utvecklingsverktyg och hello programvara.</span><span class="sxs-lookup"><span data-stu-id="720ff-123">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="720ff-124">En dator som kör Ubuntu 16.04 eller senare.</span><span class="sxs-lookup"><span data-stu-id="720ff-124">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-nodejs-and-npm"></a><span data-ttu-id="720ff-125">Installera Git, Node.js och NPM</span><span class="sxs-lookup"><span data-stu-id="720ff-125">Install Git, Node.js, and NPM</span></span>
<span data-ttu-id="720ff-126">Använd hello kortkommandot `Ctrl + Alt + T` tooopen en hello för terminal och kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="720ff-126">Use hello keyboard shortcut `Ctrl + Alt + T` tooopen a terminal and run hello following commands:</span></span>

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="720ff-127">Installera ytterligare utvecklingsverktyg för Node.js</span><span class="sxs-lookup"><span data-stu-id="720ff-127">Install additional Node.js development tools</span></span>
<span data-ttu-id="720ff-128">Använd [gulp.js](http://gulpjs.com) tooautomate hello distribution av hello exempel programmet tooPi.</span><span class="sxs-lookup"><span data-stu-id="720ff-128">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooPi.</span></span> <span data-ttu-id="720ff-129">Använd hello [enhet-discovery-cli](https://github.com/Azure/device-discovery-cli) tooretrieve network information om din IoT-enheter.</span><span class="sxs-lookup"><span data-stu-id="720ff-129">Use hello [device-discovery-cli](https://github.com/Azure/device-discovery-cli) tooretrieve network information about your IoT devices.</span></span>

<span data-ttu-id="720ff-130">Installera `gulp` och `device-discovery-cli` genom att köra följande kommando i hello terminal hello:</span><span class="sxs-lookup"><span data-stu-id="720ff-130">Install `gulp` and `device-discovery-cli` by running hello following command in hello terminal:</span></span>

```bash
sudo npm install -g device-discovery-cli gulp
```

<span data-ttu-id="720ff-131">Om du får problem med installationen Node.js och dessa ytterligare utvecklingsverktyg på Ubuntu finns hello [felsökningsguide för](iot-hub-raspberry-pi-kit-c-troubleshooting.md) för lösningar toocommon problem.</span><span class="sxs-lookup"><span data-stu-id="720ff-131">If you experience issues installing Node.js and these additional development tools on Ubuntu, see hello [troubleshooting guide](iot-hub-raspberry-pi-kit-c-troubleshooting.md) for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="720ff-132">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="720ff-132">Install Visual Studio Code</span></span>
<span data-ttu-id="720ff-133">[Hämta](https://code.visualstudio.com/docs/setup/linux) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="720ff-133">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span> <span data-ttu-id="720ff-134">Visual Studio-koden är en enkel men kraftfull källa redigerare för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="720ff-134">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="720ff-135">Du kan använda den här redigeraren för senare i hello självstudiekursen tooedit hello exempelkod.</span><span class="sxs-lookup"><span data-stu-id="720ff-135">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="720ff-136">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="720ff-136">Summary</span></span>
<span data-ttu-id="720ff-137">Du har installerat hello krävs utvecklingsverktyg och programvara för hello första exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="720ff-137">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="720ff-138">hello nästa uppgift är toocreate, distribuera och köra hello exempelprogrammet på Pi.</span><span class="sxs-lookup"><span data-stu-id="720ff-138">hello next task is toocreate, deploy, and run hello sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="720ff-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="720ff-139">Next steps</span></span>
[<span data-ttu-id="720ff-140">Skapa och distribuera hello blinka program</span><span class="sxs-lookup"><span data-stu-id="720ff-140">Create and deploy hello blink application</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md)

