---
title: "Ansluta hallon Pi (nod) tooAzure IoT - lektionen 1: Hämta verktyg (Ubuntu) | Microsoft Docs"
description: "Hämta och installera nödvändiga hello-verktyg och programvara för hello första exempelprogram för Pi på Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT-utveckling iot programvara, internet saker programvara, git på ubuntu, gulp kör, installera node js ubuntu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 4d5e45c0-1db9-4662-a039-99ba26333085
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b4f566fa0d1faf8b2321707145f675e3d87f0bef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-ubuntu-1604"></a><span data-ttu-id="5a4ba-104">Hämta hello verktyg (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="5a4ba-104">Get hello tools (Ubuntu 16.04)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="5a4ba-105">Windows 7 eller senare</span><span class="sxs-lookup"><span data-stu-id="5a4ba-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="5a4ba-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="5a4ba-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="5a4ba-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="5a4ba-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)


## <a name="what-you-will-do"></a><span data-ttu-id="5a4ba-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="5a4ba-108">What you will do</span></span>
<span data-ttu-id="5a4ba-109">Hämta hello utvecklingsverktyg och hello programvara hello första exempelprogram för hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="5a4ba-109">Download hello development tools and hello software for hello first sample application for Raspberry Pi 3.</span></span> <span data-ttu-id="5a4ba-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="5a4ba-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="5a4ba-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="5a4ba-111">What you will learn</span></span>
<span data-ttu-id="5a4ba-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="5a4ba-112">In this article, you will learn:</span></span>

* <span data-ttu-id="5a4ba-113">Hur tooinstall Git och Node.js.</span><span class="sxs-lookup"><span data-stu-id="5a4ba-113">How tooinstall Git and Node.js.</span></span>
  * <span data-ttu-id="5a4ba-114">[Git](https://git-scm.com) är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="5a4ba-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="5a4ba-115">hello-exempelprogram för den här artikeln är lagrade på Git.</span><span class="sxs-lookup"><span data-stu-id="5a4ba-115">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="5a4ba-116">[Node.js](https://nodejs.org/en/) är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="5a4ba-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="5a4ba-117">Hur toouse NPM tooinstall-utvecklingsverktyg med ytterligare Node.js.</span><span class="sxs-lookup"><span data-stu-id="5a4ba-117">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="5a4ba-118">hello version som krävs av Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="5a4ba-118">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="5a4ba-119">[NPM](https://www.npmjs.com) är en av hello paketet chefer för Node.js.</span><span class="sxs-lookup"><span data-stu-id="5a4ba-119">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="5a4ba-120">Vad behöver du</span><span class="sxs-lookup"><span data-stu-id="5a4ba-120">What do you need</span></span>
<span data-ttu-id="5a4ba-121">toocomplete den här åtgärden behöver du:</span><span class="sxs-lookup"><span data-stu-id="5a4ba-121">toocomplete this operation, you will need:</span></span>

* <span data-ttu-id="5a4ba-122">En Internet-anslutning toodownload hello utvecklingsverktyg och hello programvara.</span><span class="sxs-lookup"><span data-stu-id="5a4ba-122">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="5a4ba-123">En dator som kör Ubuntu 16.04 eller senare.</span><span class="sxs-lookup"><span data-stu-id="5a4ba-123">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-nodejs-and-npm"></a><span data-ttu-id="5a4ba-124">Installera Git, Node.js och NPM</span><span class="sxs-lookup"><span data-stu-id="5a4ba-124">Install Git, Node.js, and NPM</span></span>
<span data-ttu-id="5a4ba-125">Använd hello kortkommandot `Ctrl + Alt + T` tooopen en hello för terminal och kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="5a4ba-125">Use hello keyboard shortcut `Ctrl + Alt + T` tooopen a terminal and run hello following commands:</span></span>

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="5a4ba-126">Installera ytterligare utvecklingsverktyg för Node.js</span><span class="sxs-lookup"><span data-stu-id="5a4ba-126">Install additional Node.js development tools</span></span>
<span data-ttu-id="5a4ba-127">Du använder [gulp.js](http://gulpjs.com) tooautomate hello distribution av hello exempel programmet tooPi.</span><span class="sxs-lookup"><span data-stu-id="5a4ba-127">You use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooPi.</span></span> <span data-ttu-id="5a4ba-128">Du också använda hello [enhet-discovery-cli](https://github.com/Azure/device-discovery-cli) tooretrieve network information om din IoT-enheter.</span><span class="sxs-lookup"><span data-stu-id="5a4ba-128">You also use hello [device-discovery-cli](https://github.com/Azure/device-discovery-cli) tooretrieve network information about your IoT devices.</span></span>

<span data-ttu-id="5a4ba-129">Installera `gulp` och `device-discovery-cli` genom att köra följande kommando i hello terminal hello:</span><span class="sxs-lookup"><span data-stu-id="5a4ba-129">Install `gulp` and `device-discovery-cli` by running hello following command in hello terminal:</span></span>

```bash
sudo npm install -g device-discovery-cli gulp
```

<span data-ttu-id="5a4ba-130">Om du får problem med installationen Node.js och dessa ytterligare utvecklingsverktyg på Ubuntu finns hello [felsökningsguide för](iot-hub-raspberry-pi-kit-node-troubleshooting.md) för lösningar toocommon problem.</span><span class="sxs-lookup"><span data-stu-id="5a4ba-130">If you experience issues installing Node.js and these additional development tools on Ubuntu, see hello [troubleshooting guide](iot-hub-raspberry-pi-kit-node-troubleshooting.md) for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="5a4ba-131">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5a4ba-131">Install Visual Studio Code</span></span>
<span data-ttu-id="5a4ba-132">[Hämta](https://code.visualstudio.com/docs/setup/linux) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="5a4ba-132">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span> <span data-ttu-id="5a4ba-133">Visual Studio-koden är en enkel men kraftfull källa redigerare för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="5a4ba-133">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="5a4ba-134">Du kan använda den här redigeraren för senare i hello självstudiekursen tooedit hello exempelkod.</span><span class="sxs-lookup"><span data-stu-id="5a4ba-134">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="5a4ba-135">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="5a4ba-135">Summary</span></span>
<span data-ttu-id="5a4ba-136">Du har installerat hello krävs utvecklingsverktyg och programvara för hello första exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="5a4ba-136">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="5a4ba-137">hello nästa uppgift är toocreate, distribuera och köra hello exempelprogrammet på Pi.</span><span class="sxs-lookup"><span data-stu-id="5a4ba-137">hello next task is toocreate, deploy, and run hello sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a4ba-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5a4ba-138">Next steps</span></span>
[<span data-ttu-id="5a4ba-139">Skapa och distribuera hello blinka exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="5a4ba-139">Create and deploy hello blink sample application</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

