---
title: "Connect Raspberry PI (C) till Azure IoT - lektionen 1: Hämta verktyg (Windows) | Microsoft Docs"
description: "Hämta och installera verktyg och programvaran för det första exempelprogrammet för Pi för Windows 7 och senare versioner."
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
ms.openlocfilehash: 0e58975f4411f97223b2c4374bdd746fe6628c42
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-windows-7-or-later"></a><span data-ttu-id="4a8fa-104">Skaffa dig verktyg (Windows 7 eller senare)</span><span class="sxs-lookup"><span data-stu-id="4a8fa-104">Get the tools (Windows 7 or later)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="4a8fa-105">Windows 7 eller senare</span><span class="sxs-lookup"><span data-stu-id="4a8fa-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="4a8fa-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="4a8fa-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="4a8fa-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="4a8fa-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="4a8fa-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="4a8fa-108">What you will do</span></span>
<span data-ttu-id="4a8fa-109">Hämta utvecklingsverktyg och programvaran för det första exempelprogrammet för hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="4a8fa-109">Download the development tools and the software for the first sample application for Raspberry Pi 3.</span></span> <span data-ttu-id="4a8fa-110">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="4a8fa-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

> [!NOTE]
> <span data-ttu-id="4a8fa-111">Även om programmeringsspråket huvudsakliga logiken C, används Node.js verktyg i erfarenheter att identifiera enheter, och skapa och distribuera exempelprogram.</span><span class="sxs-lookup"><span data-stu-id="4a8fa-111">Although the programming language of the main logic is C, Node.js tools are used in the lessons to discover devices, and build and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="4a8fa-112">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="4a8fa-112">What you will learn</span></span>
<span data-ttu-id="4a8fa-113">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="4a8fa-113">In this article, you will learn:</span></span>

* <span data-ttu-id="4a8fa-114">Hur du installerar Git och Node.js.</span><span class="sxs-lookup"><span data-stu-id="4a8fa-114">How to install Git and Node.js.</span></span>
  * <span data-ttu-id="4a8fa-115">[Git](https://git-scm.com) är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="4a8fa-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="4a8fa-116">Exempelprogram för den här artikeln är lagrade på Git.</span><span class="sxs-lookup"><span data-stu-id="4a8fa-116">The sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="4a8fa-117">[Node.js](https://nodejs.org/en/) är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="4a8fa-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="4a8fa-118">Hur du använder NPM för att installera ytterligare Node.js-utvecklingsverktyg.</span><span class="sxs-lookup"><span data-stu-id="4a8fa-118">How to use NPM to install additional Node.js development tools.</span></span>
  * <span data-ttu-id="4a8fa-119">Kravet på minimiversion av Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="4a8fa-119">The minimum version requirement of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="4a8fa-120">[NPM](https://www.npmjs.com) är en av de paket cheferna för Node.js.</span><span class="sxs-lookup"><span data-stu-id="4a8fa-120">[NPM](https://www.npmjs.com) is one of the package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="4a8fa-121">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="4a8fa-121">What you need</span></span>

<span data-ttu-id="4a8fa-122">För att slutföra den här åtgärden behöver du:</span><span class="sxs-lookup"><span data-stu-id="4a8fa-122">To complete this operation, you will need:</span></span>

* <span data-ttu-id="4a8fa-123">En Internet-anslutning att hämta utvecklingsverktyg och programvaran.</span><span class="sxs-lookup"><span data-stu-id="4a8fa-123">An Internet connection to download the development tools and the software.</span></span>
* <span data-ttu-id="4a8fa-124">En dator som kör Windows.</span><span class="sxs-lookup"><span data-stu-id="4a8fa-124">A computer that is running Windows.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="4a8fa-125">Installera Git och Node.js</span><span class="sxs-lookup"><span data-stu-id="4a8fa-125">Install Git and Node.js</span></span>

<span data-ttu-id="4a8fa-126">Klicka på länkarna nedan för att hämta och installera Git och Node.js LTS för Windows.</span><span class="sxs-lookup"><span data-stu-id="4a8fa-126">Click the links below to download and install Git and Node.js LTS for Windows.</span></span>

* [<span data-ttu-id="4a8fa-127">Hämta Git för Windows</span><span class="sxs-lookup"><span data-stu-id="4a8fa-127">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
* [<span data-ttu-id="4a8fa-128">Hämta Node.js LTS för Windows</span><span class="sxs-lookup"><span data-stu-id="4a8fa-128">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="4a8fa-129">Installera ytterligare utvecklingsverktyg för Node.js</span><span class="sxs-lookup"><span data-stu-id="4a8fa-129">Install additional Node.js development tools</span></span>

<span data-ttu-id="4a8fa-130">Använd [gulp.js](http://gulpjs.com) att automatisera distributionen av exempelprogrammet till Pi.</span><span class="sxs-lookup"><span data-stu-id="4a8fa-130">Use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to Pi.</span></span> <span data-ttu-id="4a8fa-131">Använd den [enhet-discovery-cli](https://github.com/Azure/device-discovery-cli) att hämta nätverksinformation om IoT-enheter.</span><span class="sxs-lookup"><span data-stu-id="4a8fa-131">Use the [device-discovery-cli](https://github.com/Azure/device-discovery-cli) to retrieve network information about your IoT devices.</span></span>

<span data-ttu-id="4a8fa-132">Starta en kommandotolk som administratör.</span><span class="sxs-lookup"><span data-stu-id="4a8fa-132">Start a command prompt as an administrator.</span></span> <span data-ttu-id="4a8fa-133">Installera `gulp` och `device-discovery-cli` genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4a8fa-133">Install `gulp` and `device-discovery-cli` by running the following command:</span></span>

```bash
npm install -g device-discovery-cli gulp
```

<span data-ttu-id="4a8fa-134">Om du får problem med installationen Node.js och verktygen för utveckling av ytterligare Node.js på datorn finns på [felsökningsguide för](iot-hub-raspberry-pi-kit-c-troubleshooting.md) efter lösningar på vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="4a8fa-134">If you experience issues installing Node.js and these additional Node.js development tools on your computer, see the [troubleshooting guide](iot-hub-raspberry-pi-kit-c-troubleshooting.md) for solutions to common problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="4a8fa-135">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4a8fa-135">Install Visual Studio Code</span></span>

<span data-ttu-id="4a8fa-136">[Hämta](https://code.visualstudio.com/docs/setup/windows) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="4a8fa-136">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span> <span data-ttu-id="4a8fa-137">Visual Studio-koden är en enkel men kraftfull källa redigerare för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="4a8fa-137">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="4a8fa-138">Du kan använda den här redigeraren senare under kursen för att redigera exempelkoden.</span><span class="sxs-lookup"><span data-stu-id="4a8fa-138">You use this editor later in the tutorial to edit the sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="4a8fa-139">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="4a8fa-139">Summary</span></span>

<span data-ttu-id="4a8fa-140">Du har installerat nödvändiga utvecklingsverktyg och programvara för första exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="4a8fa-140">You've installed the required development tools and software for the first sample application.</span></span> <span data-ttu-id="4a8fa-141">Nästa uppgift är att skapa, distribuera och köra exempelprogrammet på Pi.</span><span class="sxs-lookup"><span data-stu-id="4a8fa-141">The next task is to create, deploy, and run the sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4a8fa-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4a8fa-142">Next steps</span></span>

[<span data-ttu-id="4a8fa-143">Skapa och distribuera blinkningsprogrammet</span><span class="sxs-lookup"><span data-stu-id="4a8fa-143">Create and deploy the blink application</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md)
