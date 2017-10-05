---
title: "Ansluta Intel modern (nod) till Azure IoT - lektionen 1: Hämta verktyg (macOS) | Microsoft Docs"
description: "Hämta och installera verktyg och programvaran för det första exempelprogrammet för modern på macOS."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino utvecklingsverktyg, iot utveckling, iot-programvara, internet saker programvara, installera git på mac installera node js mac"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: fb6742be-2825-4524-89f7-8ccb7e7f1de1
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: a5e406f49379e9f2192ee93334ab1dcf9f3e53d4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-macos-1010"></a><span data-ttu-id="5c3aa-104">Hämta verktygen (macOS 10.10)</span><span class="sxs-lookup"><span data-stu-id="5c3aa-104">Get the tools (macOS 10.10)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="5c3aa-105">[Windows 7 eller senare][windows]</span><span class="sxs-lookup"><span data-stu-id="5c3aa-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="5c3aa-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="5c3aa-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="5c3aa-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="5c3aa-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="5c3aa-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="5c3aa-108">What you will do</span></span>
<span data-ttu-id="5c3aa-109">Hämta utvecklingsverktyg och programvaran för det första exempelprogrammet för Intel-modern.</span><span class="sxs-lookup"><span data-stu-id="5c3aa-109">Download the development tools and the software for the first sample application for your Intel Edison.</span></span> <span data-ttu-id="5c3aa-110">Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="5c3aa-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="5c3aa-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="5c3aa-111">What you will learn</span></span>
<span data-ttu-id="5c3aa-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="5c3aa-112">In this article, you will learn:</span></span>

* <span data-ttu-id="5c3aa-113">Hur du installerar Git och Node.js.</span><span class="sxs-lookup"><span data-stu-id="5c3aa-113">How to install Git and Node.js.</span></span>
  * <span data-ttu-id="5c3aa-114">[Git](https://git-scm.com) är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="5c3aa-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="5c3aa-115">Exempelprogram för den här artikeln är lagrade på Git.</span><span class="sxs-lookup"><span data-stu-id="5c3aa-115">The sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="5c3aa-116">[Node.js](https://nodejs.org/en/) är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="5c3aa-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="5c3aa-117">Hur du använder NPM för att installera ytterligare Node.js-utvecklingsverktyg.</span><span class="sxs-lookup"><span data-stu-id="5c3aa-117">How to use NPM to install additional Node.js development tools.</span></span>
  * <span data-ttu-id="5c3aa-118">Versionen som krävs för Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="5c3aa-118">The minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="5c3aa-119">[NPM](https://www.npmjs.com) är en av de paket cheferna för Node.js.</span><span class="sxs-lookup"><span data-stu-id="5c3aa-119">[NPM](https://www.npmjs.com) is one of the package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="5c3aa-120">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="5c3aa-120">What you need</span></span>
<span data-ttu-id="5c3aa-121">För att slutföra den här åtgärden behöver du:</span><span class="sxs-lookup"><span data-stu-id="5c3aa-121">To complete this operation, you will need:</span></span>
* <span data-ttu-id="5c3aa-122">En Internet-anslutning att hämta utvecklingsverktyg och programvaran.</span><span class="sxs-lookup"><span data-stu-id="5c3aa-122">An Internet connection to download the development tools and the software.</span></span>
* <span data-ttu-id="5c3aa-123">En Mac som kör macOS Yosemite (10.10) eller senare.</span><span class="sxs-lookup"><span data-stu-id="5c3aa-123">A Mac that is running macOS Yosemite (10.10) or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="5c3aa-124">Installera Git och Node.js</span><span class="sxs-lookup"><span data-stu-id="5c3aa-124">Install Git and Node.js</span></span>
<span data-ttu-id="5c3aa-125">Installera Git och Node.js med den [Homebrew](http://brew.sh) paketet hanteringsverktyg genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="5c3aa-125">To install Git and Node.js, use the [Homebrew](http://brew.sh) package management utility by following these steps:</span></span>

1. <span data-ttu-id="5c3aa-126">Installera Homebrew.</span><span class="sxs-lookup"><span data-stu-id="5c3aa-126">Install Homebrew.</span></span> <span data-ttu-id="5c3aa-127">Om du redan har installerat Homebrew går du till steg 2.</span><span class="sxs-lookup"><span data-stu-id="5c3aa-127">If you've already installed Homebrew, go to step 2.</span></span>

   1. <span data-ttu-id="5c3aa-128">Tryck på `Cmd + Space` och ange `Terminal` att öppna en terminal.</span><span class="sxs-lookup"><span data-stu-id="5c3aa-128">Press `Cmd + Space` and enter `Terminal` to open a terminal.</span></span>
   2. <span data-ttu-id="5c3aa-129">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5c3aa-129">Run the following command:</span></span>

      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```
2. <span data-ttu-id="5c3aa-130">Installera Git och Node.js genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5c3aa-130">Install Git and Node.js by running the following command:</span></span>

   ```bash
   brew install node git
   ```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="5c3aa-131">Installera ytterligare utvecklingsverktyg för Node.js</span><span class="sxs-lookup"><span data-stu-id="5c3aa-131">Install additional Node.js development tools</span></span>
<span data-ttu-id="5c3aa-132">Använd [gulp.js](http://gulpjs.com) att automatisera distributionen av exempelprogrammet till din modern.</span><span class="sxs-lookup"><span data-stu-id="5c3aa-132">Use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to your Edison.</span></span>

<span data-ttu-id="5c3aa-133">Installera `gulp` genom att köra följande kommando i terminalen:</span><span class="sxs-lookup"><span data-stu-id="5c3aa-133">Install `gulp` by running the following command in the terminal:</span></span>

```bash
sudo npm install -g gulp
```

<span data-ttu-id="5c3aa-134">Om du får problem med installationen Node.js och dessa ytterligare utvecklingsverktyg på macOS finns i [felsökningsguide för] [ troubleshooting] efter lösningar på vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="5c3aa-134">If you experience issues installing Node.js and these additional development tools on macOS, see the [troubleshooting guide][troubleshooting] for solutions to common problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="5c3aa-135">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5c3aa-135">Install Visual Studio Code</span></span>
<span data-ttu-id="5c3aa-136">[Hämta](https://code.visualstudio.com/docs/setup/osx) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="5c3aa-136">[Download](https://code.visualstudio.com/docs/setup/osx) and install Visual Studio Code.</span></span> <span data-ttu-id="5c3aa-137">Visual Studio-koden är en enkel men kraftfull källa redigerare för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="5c3aa-137">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="5c3aa-138">Du kan använda den här redigeraren senare under kursen för att redigera exempelkoden.</span><span class="sxs-lookup"><span data-stu-id="5c3aa-138">You use this editor later in the tutorial to edit the sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="5c3aa-139">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="5c3aa-139">Summary</span></span>
<span data-ttu-id="5c3aa-140">Du har installerat nödvändiga utvecklingsverktyg och programvara för första exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="5c3aa-140">You've installed the required development tools and software for the first sample application.</span></span> <span data-ttu-id="5c3aa-141">Nästa uppgift är att skapa, distribuera och köra exempelprogrammet på modern.</span><span class="sxs-lookup"><span data-stu-id="5c3aa-141">The next task is to create, deploy, and run the sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5c3aa-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5c3aa-142">Next steps</span></span>
<span data-ttu-id="5c3aa-143">[Skapa och distribuera programmet blinka][create-and-deploy-the-blink-application]</span><span class="sxs-lookup"><span data-stu-id="5c3aa-143">[Create and deploy the blink application][create-and-deploy-the-blink-application]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-node-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-mac.md
