---
title: "Ansluta Intel modern (nod) tooAzure IoT - lektionen 1: Hämta verktyg (Ubuntu) | Microsoft Docs"
description: "Hämta och installera nödvändiga hello-verktyg och programvara för hello första exempelprogram för modern på Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino utvecklingsverktyg, iot utveckling, iot-programvara, internet saker programvara, installera git på ubuntu, installera node js ubuntu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 9ab5b161-7ec5-41a6-9c5f-4456e4882752
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: ad1a48708bd74bcc07d09f105f597f18c3f9d2b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-ubuntu-1604"></a><span data-ttu-id="77e69-104">Hämta hello verktyg (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="77e69-104">Get hello tools (Ubuntu 16.04)</span></span>

> [!div class="op_single_selector"]
> * <span data-ttu-id="77e69-105">[Windows 7 eller senare][windows]</span><span class="sxs-lookup"><span data-stu-id="77e69-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="77e69-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="77e69-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="77e69-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="77e69-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="77e69-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="77e69-108">What you will do</span></span>
<span data-ttu-id="77e69-109">Hämta hello utvecklingsverktyg och hello programvara för hello första exempelprogram för Intel-modern.</span><span class="sxs-lookup"><span data-stu-id="77e69-109">Download hello development tools and hello software for hello first sample application for your Intel Edison.</span></span> <span data-ttu-id="77e69-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="77e69-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="77e69-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="77e69-111">What you will learn</span></span>
<span data-ttu-id="77e69-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="77e69-112">In this article, you will learn:</span></span>

* <span data-ttu-id="77e69-113">Hur tooinstall Git och Node.js</span><span class="sxs-lookup"><span data-stu-id="77e69-113">How tooinstall Git and Node.js</span></span>
  * <span data-ttu-id="77e69-114">[Git](https://git-scm.com) är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="77e69-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="77e69-115">hello-exempelprogram för den här artikeln är lagrade på Git.</span><span class="sxs-lookup"><span data-stu-id="77e69-115">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="77e69-116">[Node.js](https://nodejs.org/en/) är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="77e69-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="77e69-117">Hur toouse NPM tooinstall-utvecklingsverktyg med ytterligare Node.js.</span><span class="sxs-lookup"><span data-stu-id="77e69-117">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="77e69-118">hello version som krävs av Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="77e69-118">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="77e69-119">[NPM](https://www.npmjs.com) är en av hello paketet chefer för Node.js.</span><span class="sxs-lookup"><span data-stu-id="77e69-119">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="77e69-120">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="77e69-120">What you need</span></span>
<span data-ttu-id="77e69-121">toocomplete den här åtgärden behöver du:</span><span class="sxs-lookup"><span data-stu-id="77e69-121">toocomplete this operation, you will need:</span></span>
* <span data-ttu-id="77e69-122">En Internet-anslutning toodownload hello utvecklingsverktyg och hello programvara.</span><span class="sxs-lookup"><span data-stu-id="77e69-122">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="77e69-123">En dator som kör Ubuntu 16.04 eller senare.</span><span class="sxs-lookup"><span data-stu-id="77e69-123">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-nodejs-and-npm"></a><span data-ttu-id="77e69-124">Installera Git, Node.js och NPM</span><span class="sxs-lookup"><span data-stu-id="77e69-124">Install Git, Node.js, and NPM</span></span>
<span data-ttu-id="77e69-125">Använd hello kortkommandot `Ctrl + Alt + T` tooopen en hello för terminal och kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="77e69-125">Use hello keyboard shortcut `Ctrl + Alt + T` tooopen a terminal and run hello following commands:</span></span>

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="77e69-126">Installera ytterligare utvecklingsverktyg för Node.js</span><span class="sxs-lookup"><span data-stu-id="77e69-126">Install additional Node.js development tools</span></span>
<span data-ttu-id="77e69-127">Använd [gulp.js](http://gulpjs.com) tooautomate hello distribution av hello exempel programmet tooEdison.</span><span class="sxs-lookup"><span data-stu-id="77e69-127">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooEdison.</span></span>

<span data-ttu-id="77e69-128">Installera `gulp` genom att köra följande kommando i hello terminal hello:</span><span class="sxs-lookup"><span data-stu-id="77e69-128">Install `gulp` by running hello following command in hello terminal:</span></span>

```bash
sudo npm install -g gulp
```

<span data-ttu-id="77e69-129">Om du får problem med installationen Node.js och dessa ytterligare utvecklingsverktyg på Ubuntu finns hello [felsökningsguide för] [ troubleshooting] för lösningar toocommon problem.</span><span class="sxs-lookup"><span data-stu-id="77e69-129">If you experience issues installing Node.js and these additional development tools on Ubuntu, see hello [troubleshooting guide][troubleshooting] for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="77e69-130">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="77e69-130">Install Visual Studio Code</span></span>
<span data-ttu-id="77e69-131">[Hämta](https://code.visualstudio.com/docs/setup/linux) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="77e69-131">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span> <span data-ttu-id="77e69-132">Visual Studio-koden är en enkel men kraftfull källa redigerare för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="77e69-132">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="77e69-133">Du kan använda den här redigeraren för senare i hello självstudiekursen tooedit hello exempelkod.</span><span class="sxs-lookup"><span data-stu-id="77e69-133">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="77e69-134">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="77e69-134">Summary</span></span>
<span data-ttu-id="77e69-135">Du har installerat hello krävs utvecklingsverktyg och programvara för hello första exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="77e69-135">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="77e69-136">hello nästa uppgift är toocreate, distribuera och köra hello exempelprogrammet på modern.</span><span class="sxs-lookup"><span data-stu-id="77e69-136">hello next task is toocreate, deploy, and run hello sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="77e69-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="77e69-137">Next steps</span></span>
<span data-ttu-id="77e69-138">[Skapa och distribuera hello blinka exempelprogrammet][create-and-deploy-the-blink-application]</span><span class="sxs-lookup"><span data-stu-id="77e69-138">[Create and deploy hello blink sample application][create-and-deploy-the-blink-application]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-node-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-mac.md
