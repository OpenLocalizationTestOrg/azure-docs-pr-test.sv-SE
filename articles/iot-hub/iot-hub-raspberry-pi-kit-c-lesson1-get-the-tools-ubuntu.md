---
title: "Connect Raspberry PI (C) till Azure IoT - lektionen 1: Hämta verktyg (Ubuntu) | Microsoft Docs"
description: "Hämta och installera verktyg och programvaran för det första exempelprogrammet för Pi på Ubuntu."
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
ms.openlocfilehash: 28ebba82e90d91470518cd830c96e6da39d8b9b4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-ubuntu-1604"></a><span data-ttu-id="0f3a1-104">Hämta verktygen (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="0f3a1-104">Get the tools (Ubuntu 16.04)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="0f3a1-105">Windows 7 eller senare</span><span class="sxs-lookup"><span data-stu-id="0f3a1-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="0f3a1-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="0f3a1-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="0f3a1-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="0f3a1-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="0f3a1-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="0f3a1-108">What you will do</span></span>
<span data-ttu-id="0f3a1-109">Hämta utvecklingsverktyg och programvaran för det första exempelprogrammet för din hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="0f3a1-109">Download the development tools and the software for the first sample application for your Raspberry Pi 3.</span></span> <span data-ttu-id="0f3a1-110">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="0f3a1-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

> [!NOTE]
> <span data-ttu-id="0f3a1-111">Även om programmeringsspråket huvudsakliga logiken C, används Node.js verktyg i erfarenheter att identifiera enheter, och skapa och distribuera exempelprogram.</span><span class="sxs-lookup"><span data-stu-id="0f3a1-111">Although the programming language of the main logic is C, Node.js tools are used in the lessons to discover devices, and build and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="0f3a1-112">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="0f3a1-112">What you will learn</span></span>
<span data-ttu-id="0f3a1-113">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="0f3a1-113">In this article, you will learn:</span></span>

* <span data-ttu-id="0f3a1-114">Hur du installerar Git och Node.js</span><span class="sxs-lookup"><span data-stu-id="0f3a1-114">How to install Git and Node.js</span></span>
  * <span data-ttu-id="0f3a1-115">[Git](https://git-scm.com) är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="0f3a1-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="0f3a1-116">Exempelprogram för den här artikeln är lagrade på Git.</span><span class="sxs-lookup"><span data-stu-id="0f3a1-116">The sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="0f3a1-117">[Node.js](https://nodejs.org/en/) är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="0f3a1-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="0f3a1-118">Hur du använder NPM för att installera ytterligare Node.js-utvecklingsverktyg.</span><span class="sxs-lookup"><span data-stu-id="0f3a1-118">How to use NPM to install additional Node.js development tools.</span></span>
  * <span data-ttu-id="0f3a1-119">Versionen som krävs för Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="0f3a1-119">The minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="0f3a1-120">[NPM](https://www.npmjs.com) är en av de paket cheferna för Node.js.</span><span class="sxs-lookup"><span data-stu-id="0f3a1-120">[NPM](https://www.npmjs.com) is one of the package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="0f3a1-121">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="0f3a1-121">What you need</span></span>
<span data-ttu-id="0f3a1-122">För att slutföra den här åtgärden behöver du:</span><span class="sxs-lookup"><span data-stu-id="0f3a1-122">To complete this operation, you will need:</span></span>

* <span data-ttu-id="0f3a1-123">En Internet-anslutning att hämta utvecklingsverktyg och programvaran.</span><span class="sxs-lookup"><span data-stu-id="0f3a1-123">An Internet connection to download the development tools and the software.</span></span>
* <span data-ttu-id="0f3a1-124">En dator som kör Ubuntu 16.04 eller senare.</span><span class="sxs-lookup"><span data-stu-id="0f3a1-124">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-nodejs-and-npm"></a><span data-ttu-id="0f3a1-125">Installera Git, Node.js och NPM</span><span class="sxs-lookup"><span data-stu-id="0f3a1-125">Install Git, Node.js, and NPM</span></span>
<span data-ttu-id="0f3a1-126">Använd kortkommandot `Ctrl + Alt + T` att öppna en terminal och kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="0f3a1-126">Use the keyboard shortcut `Ctrl + Alt + T` to open a terminal and run the following commands:</span></span>

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="0f3a1-127">Installera ytterligare utvecklingsverktyg för Node.js</span><span class="sxs-lookup"><span data-stu-id="0f3a1-127">Install additional Node.js development tools</span></span>
<span data-ttu-id="0f3a1-128">Använd [gulp.js](http://gulpjs.com) att automatisera distributionen av exempelprogrammet till Pi.</span><span class="sxs-lookup"><span data-stu-id="0f3a1-128">Use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to Pi.</span></span> <span data-ttu-id="0f3a1-129">Använd den [enhet-discovery-cli](https://github.com/Azure/device-discovery-cli) att hämta nätverksinformation om IoT-enheter.</span><span class="sxs-lookup"><span data-stu-id="0f3a1-129">Use the [device-discovery-cli](https://github.com/Azure/device-discovery-cli) to retrieve network information about your IoT devices.</span></span>

<span data-ttu-id="0f3a1-130">Installera `gulp` och `device-discovery-cli` genom att köra följande kommando i terminalen:</span><span class="sxs-lookup"><span data-stu-id="0f3a1-130">Install `gulp` and `device-discovery-cli` by running the following command in the terminal:</span></span>

```bash
sudo npm install -g device-discovery-cli gulp
```

<span data-ttu-id="0f3a1-131">Om du får problem med installationen Node.js och dessa ytterligare utvecklingsverktyg på Ubuntu finns i [felsökningsguide för](iot-hub-raspberry-pi-kit-c-troubleshooting.md) efter lösningar på vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="0f3a1-131">If you experience issues installing Node.js and these additional development tools on Ubuntu, see the [troubleshooting guide](iot-hub-raspberry-pi-kit-c-troubleshooting.md) for solutions to common problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="0f3a1-132">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0f3a1-132">Install Visual Studio Code</span></span>
<span data-ttu-id="0f3a1-133">[Hämta](https://code.visualstudio.com/docs/setup/linux) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="0f3a1-133">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span> <span data-ttu-id="0f3a1-134">Visual Studio-koden är en enkel men kraftfull källa redigerare för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="0f3a1-134">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="0f3a1-135">Du kan använda den här redigeraren senare under kursen för att redigera exempelkoden.</span><span class="sxs-lookup"><span data-stu-id="0f3a1-135">You use this editor later in the tutorial to edit the sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="0f3a1-136">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="0f3a1-136">Summary</span></span>
<span data-ttu-id="0f3a1-137">Du har installerat nödvändiga utvecklingsverktyg och programvara för första exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="0f3a1-137">You've installed the required development tools and software for the first sample application.</span></span> <span data-ttu-id="0f3a1-138">Nästa uppgift är att skapa, distribuera och köra exempelprogrammet på Pi.</span><span class="sxs-lookup"><span data-stu-id="0f3a1-138">The next task is to create, deploy, and run the sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0f3a1-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0f3a1-139">Next steps</span></span>
[<span data-ttu-id="0f3a1-140">Skapa och distribuera blinkningsprogrammet</span><span class="sxs-lookup"><span data-stu-id="0f3a1-140">Create and deploy the blink application</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md)

