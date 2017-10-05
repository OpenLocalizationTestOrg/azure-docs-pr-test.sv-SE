---
title: "Connect Raspberry PI (C) till Azure IoT - lektionen 1: Hämta verktyg (macOS) | Microsoft Docs"
description: "Hämta och installera verktyg och programvaran för det första exempelprogrammet för Pi på macOS."
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
ms.openlocfilehash: 64db77040ef3482f213bd622320fdb5df243fa93
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-macos-1010"></a><span data-ttu-id="fa74b-104">Hämta verktygen (macOS 10.10)</span><span class="sxs-lookup"><span data-stu-id="fa74b-104">Get the tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fa74b-105">Windows 7 eller senare</span><span class="sxs-lookup"><span data-stu-id="fa74b-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="fa74b-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="fa74b-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="fa74b-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="fa74b-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="fa74b-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="fa74b-108">What you will do</span></span>
<span data-ttu-id="fa74b-109">Hämta utvecklingsverktyg och programvaran för det första exempelprogrammet för din hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="fa74b-109">Download the development tools and the software for the first sample application for your Raspberry Pi 3.</span></span> <span data-ttu-id="fa74b-110">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="fa74b-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

> [!NOTE]
> <span data-ttu-id="fa74b-111">Även om programmeringsspråket huvudsakliga logiken C, används Node.js verktyg i erfarenheter att identifiera enheter, och skapa och distribuera exempelprogram.</span><span class="sxs-lookup"><span data-stu-id="fa74b-111">Although the programming language of the main logic is C, Node.js tools are used in the lessons to discover devices, and build and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="fa74b-112">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="fa74b-112">What you will learn</span></span>
<span data-ttu-id="fa74b-113">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="fa74b-113">In this article, you will learn:</span></span>

* <span data-ttu-id="fa74b-114">Hur du installerar Git och Node.js.</span><span class="sxs-lookup"><span data-stu-id="fa74b-114">How to install Git and Node.js.</span></span>
  * <span data-ttu-id="fa74b-115">[Git](https://git-scm.com) är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="fa74b-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="fa74b-116">Exempelprogram för den här artikeln är lagrade på Git.</span><span class="sxs-lookup"><span data-stu-id="fa74b-116">The sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="fa74b-117">[Node.js](https://nodejs.org/en/) är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="fa74b-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="fa74b-118">Hur du använder NPM för att installera ytterligare Node.js-utvecklingsverktyg.</span><span class="sxs-lookup"><span data-stu-id="fa74b-118">How to use NPM to install additional Node.js development tools.</span></span>
  * <span data-ttu-id="fa74b-119">Versionen som krävs för Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="fa74b-119">The minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="fa74b-120">[NPM](https://www.npmjs.com) är en av de paket cheferna för Node.js.</span><span class="sxs-lookup"><span data-stu-id="fa74b-120">[NPM](https://www.npmjs.com) is one of the package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="fa74b-121">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="fa74b-121">What you need</span></span>
<span data-ttu-id="fa74b-122">För att slutföra den här åtgärden behöver du:</span><span class="sxs-lookup"><span data-stu-id="fa74b-122">To complete this operation, you will need:</span></span>

* <span data-ttu-id="fa74b-123">En Internet-anslutning att hämta utvecklingsverktyg och programvaran.</span><span class="sxs-lookup"><span data-stu-id="fa74b-123">An Internet connection to download the development tools and the software.</span></span>
* <span data-ttu-id="fa74b-124">En Mac som kör macOS Yosemite (10.10) eller senare.</span><span class="sxs-lookup"><span data-stu-id="fa74b-124">A Mac that is running macOS Yosemite (10.10) or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="fa74b-125">Installera Git och Node.js</span><span class="sxs-lookup"><span data-stu-id="fa74b-125">Install Git and Node.js</span></span>
<span data-ttu-id="fa74b-126">Installera Git och Node.js med den [Homebrew](http://brew.sh) paketet hanteringsverktyg genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="fa74b-126">To install Git and Node.js, use the [Homebrew](http://brew.sh) package management utility by following these steps:</span></span>

1. <span data-ttu-id="fa74b-127">Installera Homebrew.</span><span class="sxs-lookup"><span data-stu-id="fa74b-127">Install Homebrew.</span></span> <span data-ttu-id="fa74b-128">Om du redan har installerat Homebrew går du till steg 2.</span><span class="sxs-lookup"><span data-stu-id="fa74b-128">If you've already installed Homebrew, go to step 2.</span></span>
   
   1. <span data-ttu-id="fa74b-129">Tryck på `Cmd + Space` och ange `Terminal` att öppna en terminal.</span><span class="sxs-lookup"><span data-stu-id="fa74b-129">Press `Cmd + Space` and enter `Terminal` to open a terminal.</span></span>
   2. <span data-ttu-id="fa74b-130">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="fa74b-130">Run the following command:</span></span>
      
      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```
2. <span data-ttu-id="fa74b-131">Installera Git och Node.js genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="fa74b-131">Install Git and Node.js by running the following command:</span></span>
   
   ```bash
   brew install node git
   ```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="fa74b-132">Installera ytterligare utvecklingsverktyg för Node.js</span><span class="sxs-lookup"><span data-stu-id="fa74b-132">Install additional Node.js development tools</span></span>
<span data-ttu-id="fa74b-133">Använd [gulp.js](http://gulpjs.com) att automatisera distributionen av exempelprogrammet till din Pi.</span><span class="sxs-lookup"><span data-stu-id="fa74b-133">Use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to your Pi.</span></span> <span data-ttu-id="fa74b-134">Använd den [enhet-discovery-cli](https://github.com/Azure/device-discovery-cli) att hämta nätverksinformation om IoT-enheter.</span><span class="sxs-lookup"><span data-stu-id="fa74b-134">Use the [device-discovery-cli](https://github.com/Azure/device-discovery-cli) to retrieve network information about your IoT devices.</span></span>

<span data-ttu-id="fa74b-135">Installera `gulp` och `device-discovery-cli` genom att köra följande kommando i terminalen:</span><span class="sxs-lookup"><span data-stu-id="fa74b-135">Install `gulp` and `device-discovery-cli` by running the following command in the terminal:</span></span>

```bash
sudo npm install -g device-discovery-cli gulp
```

<span data-ttu-id="fa74b-136">Om du får problem med installationen Node.js och dessa ytterligare utvecklingsverktyg på macOS finns i [felsökningsguide för](iot-hub-raspberry-pi-kit-c-troubleshooting.md) efter lösningar på vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="fa74b-136">If you experience issues installing Node.js and these additional development tools on macOS, see the [troubleshooting guide](iot-hub-raspberry-pi-kit-c-troubleshooting.md) for solutions to common problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="fa74b-137">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fa74b-137">Install Visual Studio Code</span></span>
<span data-ttu-id="fa74b-138">[Hämta](https://code.visualstudio.com/docs/setup/osx) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="fa74b-138">[Download](https://code.visualstudio.com/docs/setup/osx) and install Visual Studio Code.</span></span> <span data-ttu-id="fa74b-139">Visual Studio-koden är en enkel men kraftfull källa redigerare för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="fa74b-139">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="fa74b-140">Du kan använda den här redigeraren senare under kursen för att redigera exempelkoden.</span><span class="sxs-lookup"><span data-stu-id="fa74b-140">You use this editor later in the tutorial to edit the sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="fa74b-141">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="fa74b-141">Summary</span></span>
<span data-ttu-id="fa74b-142">Du har installerat nödvändiga utvecklingsverktyg och programvara för första exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="fa74b-142">You've installed the required development tools and software for the first sample application.</span></span> <span data-ttu-id="fa74b-143">Nästa uppgift är att skapa, distribuera och köra exempelprogrammet på Pi.</span><span class="sxs-lookup"><span data-stu-id="fa74b-143">The next task is to create, deploy, and run the sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fa74b-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fa74b-144">Next steps</span></span>
[<span data-ttu-id="fa74b-145">Skapa och distribuera blinkningsprogrammet</span><span class="sxs-lookup"><span data-stu-id="fa74b-145">Create and deploy the blink application</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md)

