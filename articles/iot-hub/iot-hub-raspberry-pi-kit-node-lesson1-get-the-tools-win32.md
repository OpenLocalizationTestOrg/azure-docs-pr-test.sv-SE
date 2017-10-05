---
title: "Ansluta hallon Pi (nod) till Azure IoT - lektionen 1: Hämta verktyg (Windows) | Microsoft Docs"
description: "Hämta och installera verktyg och programvaran för det första exempelprogrammet för Pi för Windows 7 och senare versioner."
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
ms.openlocfilehash: 24c58e006bbef9bbc1fcd626a0f8f6bcac063f05
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-windows-7-or-later"></a><span data-ttu-id="b4125-104">Skaffa dig verktyg (Windows 7 eller senare)</span><span class="sxs-lookup"><span data-stu-id="b4125-104">Get the tools (Windows 7 or later)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b4125-105">Windows 7 eller senare</span><span class="sxs-lookup"><span data-stu-id="b4125-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="b4125-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="b4125-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="b4125-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="b4125-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)


## <a name="what-you-will-do"></a><span data-ttu-id="b4125-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="b4125-108">What you will do</span></span>
<span data-ttu-id="b4125-109">Hämta utvecklingsverktyg och programvaran för det första exempelprogrammet för hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="b4125-109">Download the development tools and the software for the first sample application for Raspberry Pi 3.</span></span> <span data-ttu-id="b4125-110">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="b4125-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="b4125-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="b4125-111">What you will learn</span></span>
<span data-ttu-id="b4125-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="b4125-112">In this article, you will learn:</span></span>

* <span data-ttu-id="b4125-113">Hur du installerar Git och Node.js.</span><span class="sxs-lookup"><span data-stu-id="b4125-113">How to install Git and Node.js.</span></span>
  * <span data-ttu-id="b4125-114">[Git](https://git-scm.com) är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="b4125-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="b4125-115">Exempelprogram för den här artikeln är lagrade på Git.</span><span class="sxs-lookup"><span data-stu-id="b4125-115">The sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="b4125-116">[Node.js](https://nodejs.org/en/) är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="b4125-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="b4125-117">Hur du använder NPM för att installera ytterligare Node.js-utvecklingsverktyg.</span><span class="sxs-lookup"><span data-stu-id="b4125-117">How to use NPM to install additional Node.js development tools.</span></span>
  * <span data-ttu-id="b4125-118">Kravet på minimiversion av Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="b4125-118">The minimum version requirement of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="b4125-119">[NPM](https://www.npmjs.com) är en av de paket cheferna för Node.js.</span><span class="sxs-lookup"><span data-stu-id="b4125-119">[NPM](https://www.npmjs.com) is one of the package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="b4125-120">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="b4125-120">What you need</span></span>
<span data-ttu-id="b4125-121">För att slutföra den här åtgärden behöver du:</span><span class="sxs-lookup"><span data-stu-id="b4125-121">To complete this operation, you will need:</span></span>

* <span data-ttu-id="b4125-122">En Internet-anslutning att hämta utvecklingsverktyg och programvaran.</span><span class="sxs-lookup"><span data-stu-id="b4125-122">An Internet connection to download the development tools and the software.</span></span>
* <span data-ttu-id="b4125-123">En dator som kör Windows.</span><span class="sxs-lookup"><span data-stu-id="b4125-123">A computer that is running Windows.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="b4125-124">Installera Git och Node.js</span><span class="sxs-lookup"><span data-stu-id="b4125-124">Install Git and Node.js</span></span>
<span data-ttu-id="b4125-125">Klicka på länken nedan om du vill hämta och installera Git och Node.js LTS för Windows.</span><span class="sxs-lookup"><span data-stu-id="b4125-125">Click the following links to download and install Git and Node.js LTS for Windows.</span></span>

* [<span data-ttu-id="b4125-126">Hämta Git för Windows</span><span class="sxs-lookup"><span data-stu-id="b4125-126">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
* [<span data-ttu-id="b4125-127">Hämta Node.js LTS för Windows</span><span class="sxs-lookup"><span data-stu-id="b4125-127">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="b4125-128">Installera ytterligare utvecklingsverktyg för Node.js</span><span class="sxs-lookup"><span data-stu-id="b4125-128">Install additional Node.js development tools</span></span>
<span data-ttu-id="b4125-129">Använd [gulp.js](http://gulpjs.com) att automatisera distributionen av exempelprogrammet till Pi.</span><span class="sxs-lookup"><span data-stu-id="b4125-129">Use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to Pi.</span></span> <span data-ttu-id="b4125-130">Du också använda den [enhet-discovery-cli](https://github.com/Azure/device-discovery-cli) att hämta nätverksinformation om IoT-enheter.</span><span class="sxs-lookup"><span data-stu-id="b4125-130">You also use the [device-discovery-cli](https://github.com/Azure/device-discovery-cli) to retrieve network information about your IoT devices.</span></span>

<span data-ttu-id="b4125-131">Starta en kommandotolk som administratör.</span><span class="sxs-lookup"><span data-stu-id="b4125-131">Start a command prompt as an administrator.</span></span> <span data-ttu-id="b4125-132">Installera `gulp` och `device-discovery-cli` genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="b4125-132">Install `gulp` and `device-discovery-cli` by running the following command:</span></span>

```bash
npm install -g device-discovery-cli gulp
```

<span data-ttu-id="b4125-133">Om du får problem med installationen Node.js och verktygen för utveckling av ytterligare Node.js på datorn finns på [felsökningsguide för](iot-hub-raspberry-pi-kit-node-troubleshooting.md) efter lösningar på vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="b4125-133">If you experience issues installing Node.js and these additional Node.js development tools on your computer, see the [troubleshooting guide](iot-hub-raspberry-pi-kit-node-troubleshooting.md) for solutions to common problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="b4125-134">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b4125-134">Install Visual Studio Code</span></span>
<span data-ttu-id="b4125-135">[Hämta](https://code.visualstudio.com/docs/setup/windows) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="b4125-135">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span> <span data-ttu-id="b4125-136">Visual Studio-koden är en enkel men kraftfull källa redigerare för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="b4125-136">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="b4125-137">Du kan använda den här redigeraren senare under kursen för att redigera exempelkoden.</span><span class="sxs-lookup"><span data-stu-id="b4125-137">You use this editor later in the tutorial to edit the sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="b4125-138">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="b4125-138">Summary</span></span>
<span data-ttu-id="b4125-139">Du har installerat nödvändiga utvecklingsverktyg och programvara för första exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="b4125-139">You've installed the required development tools and software for the first sample application.</span></span> <span data-ttu-id="b4125-140">Nästa uppgift är att skapa, distribuera och köra exempelprogrammet på Pi.</span><span class="sxs-lookup"><span data-stu-id="b4125-140">The next task is to create, deploy, and run the sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b4125-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b4125-141">Next steps</span></span>
[<span data-ttu-id="b4125-142">Skapa och distribuera blinka exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="b4125-142">Create and deploy the blink sample application</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

