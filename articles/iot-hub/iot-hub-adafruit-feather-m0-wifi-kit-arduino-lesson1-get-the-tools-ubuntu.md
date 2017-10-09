---
title: "Ansluta Arduino tooAzure IoT - lektionen 1: Hämta verktyg (Ubuntu) | Microsoft Docs"
description: "Hämta och installera nödvändiga hello-verktyg och programvara för hello första exempelprogram för Adafruit ludd M0 WiFi på Ubuntu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino utvecklingsverktyg, iot utveckling, iot-programvara, internet saker programvara, installera git på ubuntu, installera node js ubuntu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 7572f191-420d-41f0-923b-7ea86c0bfa73
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 586f89025d2fa11a31cb782e3789d306ade018a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-ubuntu-1604"></a><span data-ttu-id="8aa68-104">Hämta hello verktyg (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="8aa68-104">Get hello tools (Ubuntu 16.04)</span></span>

> [!div class="op_single_selector"]
> * <span data-ttu-id="8aa68-105">[Windows 7 eller senare][windows]</span><span class="sxs-lookup"><span data-stu-id="8aa68-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="8aa68-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="8aa68-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="8aa68-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="8aa68-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="8aa68-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="8aa68-108">What you will do</span></span>

<span data-ttu-id="8aa68-109">Hämta hello utvecklingsverktyg och hello programvara hello första exempelprogram för Adafruit ludd M0 WiFi Arduino-skiva.</span><span class="sxs-lookup"><span data-stu-id="8aa68-109">Download hello development tools and hello software for hello first sample application for your Adafruit Feather M0 WiFi Arduino board.</span></span> 

<span data-ttu-id="8aa68-110">Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="8aa68-110">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

> [!NOTE]
> <span data-ttu-id="8aa68-111">Även om hello programmeringsspråket av hello huvudsakliga logik Arduino, Node.js-verktyg som används i hello erfarenheter toobuild och distribuera exempelprogram.</span><span class="sxs-lookup"><span data-stu-id="8aa68-111">Although hello programming language of hello main logic is Arduino, Node.js tools are used in hello lessons toobuild and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="8aa68-112">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="8aa68-112">What you will learn</span></span>
<span data-ttu-id="8aa68-113">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="8aa68-113">In this article, you will learn:</span></span>

* <span data-ttu-id="8aa68-114">Hur tooinstall Git och Node.js</span><span class="sxs-lookup"><span data-stu-id="8aa68-114">How tooinstall Git and Node.js</span></span>
  * <span data-ttu-id="8aa68-115">[Git](https://git-scm.com) är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="8aa68-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="8aa68-116">hello-exempelprogram för den här artikeln är lagrade på Git.</span><span class="sxs-lookup"><span data-stu-id="8aa68-116">hello sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="8aa68-117">[Node.js](https://nodejs.org/en/) är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="8aa68-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="8aa68-118">Hur toouse NPM tooinstall-utvecklingsverktyg med ytterligare Node.js.</span><span class="sxs-lookup"><span data-stu-id="8aa68-118">How toouse NPM tooinstall additional Node.js development tools.</span></span>
  * <span data-ttu-id="8aa68-119">hello version som krävs av Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="8aa68-119">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="8aa68-120">[NPM](https://www.npmjs.com) är en av hello paketet chefer för Node.js.</span><span class="sxs-lookup"><span data-stu-id="8aa68-120">[NPM](https://www.npmjs.com) is one of hello package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="8aa68-121">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="8aa68-121">What you need</span></span>
<span data-ttu-id="8aa68-122">toocomplete den här åtgärden behöver du:</span><span class="sxs-lookup"><span data-stu-id="8aa68-122">toocomplete this operation, you will need:</span></span>
* <span data-ttu-id="8aa68-123">En Internet-anslutning toodownload hello utvecklingsverktyg och hello programvara.</span><span class="sxs-lookup"><span data-stu-id="8aa68-123">An Internet connection toodownload hello development tools and hello software.</span></span>
* <span data-ttu-id="8aa68-124">En dator som kör Ubuntu 16.04 eller senare.</span><span class="sxs-lookup"><span data-stu-id="8aa68-124">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-nodejs-and-npm"></a><span data-ttu-id="8aa68-125">Installera Git, Node.js och NPM</span><span class="sxs-lookup"><span data-stu-id="8aa68-125">Install Git, Node.js, and NPM</span></span>
<span data-ttu-id="8aa68-126">Använd hello kortkommandot `Ctrl + Alt + T` tooopen en hello för terminal och kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="8aa68-126">Use hello keyboard shortcut `Ctrl + Alt + T` tooopen a terminal and run hello following commands:</span></span>

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="8aa68-127">Installera ytterligare utvecklingsverktyg för Node.js</span><span class="sxs-lookup"><span data-stu-id="8aa68-127">Install additional Node.js development tools</span></span>
<span data-ttu-id="8aa68-128">Använd [gulp.js](http://gulpjs.com) tooautomate hello distribution av hello exempel programmet tooyour Arduino-kort.</span><span class="sxs-lookup"><span data-stu-id="8aa68-128">Use [gulp.js](http://gulpjs.com) tooautomate hello deployment of hello sample application tooyour Arduino board.</span></span>

<span data-ttu-id="8aa68-129">Installera `gulp`, `device-discovery-cli` genom att köra följande kommando i hello terminal hello:</span><span class="sxs-lookup"><span data-stu-id="8aa68-129">Install `gulp`, `device-discovery-cli` by running hello following command in hello terminal:</span></span>

```bash
sudo npm install -g gulp device-discovery-cli
```

<span data-ttu-id="8aa68-130">Om du får problem med installationen Node.js och dessa ytterligare utvecklingsverktyg på Ubuntu finns hello [felsökningsguide för] [ troubleshooting] för lösningar toocommon problem.</span><span class="sxs-lookup"><span data-stu-id="8aa68-130">If you experience issues installing Node.js and these additional development tools on Ubuntu, see hello [troubleshooting guide][troubleshooting] for solutions toocommon problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="8aa68-131">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8aa68-131">Install Visual Studio Code</span></span>
<span data-ttu-id="8aa68-132">[Hämta](https://code.visualstudio.com/docs/setup/linux) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="8aa68-132">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span> <span data-ttu-id="8aa68-133">Visual Studio-koden är en enkel men kraftfull källa redigerare för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="8aa68-133">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="8aa68-134">Du kan använda den här redigeraren för senare i hello självstudiekursen tooedit hello exempelkod.</span><span class="sxs-lookup"><span data-stu-id="8aa68-134">You use this editor later in hello tutorial tooedit hello sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="8aa68-135">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="8aa68-135">Summary</span></span>
<span data-ttu-id="8aa68-136">Du har installerat hello krävs utvecklingsverktyg och programvara för hello första exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="8aa68-136">You've installed hello required development tools and software for hello first sample application.</span></span> <span data-ttu-id="8aa68-137">hello nästa uppgift är toocreate, distribuera och köra hello exempelprogrammet på Arduino-kort.</span><span class="sxs-lookup"><span data-stu-id="8aa68-137">hello next task is toocreate, deploy, and run hello sample application on your Arduino board.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8aa68-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8aa68-138">Next steps</span></span>
<span data-ttu-id="8aa68-139">[Skapa och distribuera hello blinka exempelprogrammet][create-and-deploy-the-blink-sample-application]</span><span class="sxs-lookup"><span data-stu-id="8aa68-139">[Create and deploy hello blink sample application][create-and-deploy-the-blink-sample-application]</span></span>

<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-mac.md
[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[create-and-deploy-the-blink-sample-application]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-deploy-blink-app.md