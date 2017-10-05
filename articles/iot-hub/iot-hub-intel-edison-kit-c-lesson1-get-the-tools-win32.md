---
title: "Connect Intel EDISON (C) till Azure IoT - lektionen 1: Hämta verktyg (Windows) | Microsoft Docs"
description: "Hämta och installera verktyg och programvaran för det första exempelprogrammet för modern för Windows 7 och senare versioner."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino utvecklingsverktyg, iot utveckling, iot-programvara, internet saker programvara, installera git för windows, installera node js windows"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 7d29a358-544d-4657-a504-5ed9b79c2925
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: f9d614d17f262b81a75d6128cbc5898dc18ab906
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-windows-7-or-later"></a><span data-ttu-id="7cf37-104">Skaffa dig verktyg (Windows 7 eller senare)</span><span class="sxs-lookup"><span data-stu-id="7cf37-104">Get the tools (Windows 7 or later)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="7cf37-105">[Windows 7 eller senare][windows]</span><span class="sxs-lookup"><span data-stu-id="7cf37-105">[Windows 7 or later][windows]</span></span>
> * <span data-ttu-id="7cf37-106">[Ubuntu 16.04][ubuntu]</span><span class="sxs-lookup"><span data-stu-id="7cf37-106">[Ubuntu 16.04][ubuntu]</span></span>
> * <span data-ttu-id="7cf37-107">[macOS 10.10][macos]</span><span class="sxs-lookup"><span data-stu-id="7cf37-107">[macOS 10.10][macos]</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="7cf37-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="7cf37-108">What you will do</span></span>
<span data-ttu-id="7cf37-109">Hämta utvecklingsverktyg och programvaran för det första exempelprogrammet för Intel modern.</span><span class="sxs-lookup"><span data-stu-id="7cf37-109">Download the development tools and the software for the first sample application for Intel Edison.</span></span> <span data-ttu-id="7cf37-110">Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="7cf37-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

> [!NOTE]
> <span data-ttu-id="7cf37-111">Även om programmeringsspråket huvudsakliga logiken C, används Node.js verktyg i erfarenheter att skapa och distribuera exempelprogram.</span><span class="sxs-lookup"><span data-stu-id="7cf37-111">Although the programming language of the main logic is C, Node.js tools are used in the lessons to build and deploy sample applications.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="7cf37-112">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="7cf37-112">What you will learn</span></span>
<span data-ttu-id="7cf37-113">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="7cf37-113">In this article, you will learn:</span></span>

* <span data-ttu-id="7cf37-114">Hur du installerar Git och Node.js.</span><span class="sxs-lookup"><span data-stu-id="7cf37-114">How to install Git and Node.js.</span></span>
  * <span data-ttu-id="7cf37-115">[Git](https://git-scm.com) är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="7cf37-115">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="7cf37-116">Exempelprogram för den här artikeln är lagrade på Git.</span><span class="sxs-lookup"><span data-stu-id="7cf37-116">The sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="7cf37-117">[Node.js](https://nodejs.org/en/) är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="7cf37-117">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="7cf37-118">Hur du använder NPM för att installera ytterligare Node.js-utvecklingsverktyg.</span><span class="sxs-lookup"><span data-stu-id="7cf37-118">How to use NPM to install additional Node.js development tools.</span></span>
  * <span data-ttu-id="7cf37-119">Kravet på minimiversion av Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="7cf37-119">The minimum version requirement of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="7cf37-120">[NPM](https://www.npmjs.com) är en av de paket cheferna för Node.js.</span><span class="sxs-lookup"><span data-stu-id="7cf37-120">[NPM](https://www.npmjs.com) is one of the package managers for Node.js.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="7cf37-121">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="7cf37-121">What you need</span></span>

<span data-ttu-id="7cf37-122">För att slutföra den här åtgärden behöver du:</span><span class="sxs-lookup"><span data-stu-id="7cf37-122">To complete this operation, you will need:</span></span>

* <span data-ttu-id="7cf37-123">En Internet-anslutning att hämta utvecklingsverktyg och programvaran.</span><span class="sxs-lookup"><span data-stu-id="7cf37-123">An Internet connection to download the development tools and the software.</span></span>
* <span data-ttu-id="7cf37-124">En dator som kör Windows.</span><span class="sxs-lookup"><span data-stu-id="7cf37-124">A computer that is running Windows.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="7cf37-125">Installera Git och Node.js</span><span class="sxs-lookup"><span data-stu-id="7cf37-125">Install Git and Node.js</span></span>

<span data-ttu-id="7cf37-126">Klicka på länkarna nedan för att hämta och installera Git och Node.js LTS för Windows.</span><span class="sxs-lookup"><span data-stu-id="7cf37-126">Click the links below to download and install Git and Node.js LTS for Windows.</span></span>

* [<span data-ttu-id="7cf37-127">Hämta Git för Windows</span><span class="sxs-lookup"><span data-stu-id="7cf37-127">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
* [<span data-ttu-id="7cf37-128">Hämta Node.js LTS för Windows</span><span class="sxs-lookup"><span data-stu-id="7cf37-128">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="7cf37-129">Installera ytterligare utvecklingsverktyg för Node.js</span><span class="sxs-lookup"><span data-stu-id="7cf37-129">Install additional Node.js development tools</span></span>

<span data-ttu-id="7cf37-130">Använd [gulp.js](http://gulpjs.com) att automatisera distributionen av exempelprogrammet till modern.</span><span class="sxs-lookup"><span data-stu-id="7cf37-130">Use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to Edison.</span></span>

<span data-ttu-id="7cf37-131">Starta en kommandotolk som administratör.</span><span class="sxs-lookup"><span data-stu-id="7cf37-131">Start a command prompt as an administrator.</span></span> <span data-ttu-id="7cf37-132">Installera `gulp` genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7cf37-132">Install `gulp` by running the following command:</span></span>

```cmd
npm install -g gulp
```

<span data-ttu-id="7cf37-133">Om du får problem med installationen Node.js och verktygen för utveckling av ytterligare Node.js på datorn finns på [felsökningsguide för] [ troubleshooting] efter lösningar på vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="7cf37-133">If you experience issues installing Node.js and these additional Node.js development tools on your computer, see the [troubleshooting guide][troubleshooting] for solutions to common problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="7cf37-134">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7cf37-134">Install Visual Studio Code</span></span>

<span data-ttu-id="7cf37-135">[Hämta](https://code.visualstudio.com/docs/setup/windows) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="7cf37-135">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span> <span data-ttu-id="7cf37-136">Visual Studio-koden är en enkel men kraftfull källa redigerare för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="7cf37-136">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="7cf37-137">Du kan använda den här redigeraren senare under kursen för att redigera exempelkoden.</span><span class="sxs-lookup"><span data-stu-id="7cf37-137">You use this editor later in the tutorial to edit the sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="7cf37-138">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="7cf37-138">Summary</span></span>

<span data-ttu-id="7cf37-139">Du har installerat nödvändiga utvecklingsverktyg och programvara för första exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="7cf37-139">You've installed the required development tools and software for the first sample application.</span></span> <span data-ttu-id="7cf37-140">Nästa uppgift är att skapa, distribuera och köra exempelprogrammet på modern.</span><span class="sxs-lookup"><span data-stu-id="7cf37-140">The next task is to create, deploy, and run the sample application on Edison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7cf37-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7cf37-141">Next steps</span></span>

<span data-ttu-id="7cf37-142">[Skapa och distribuera programmet blinka][create-and-deploy-the-blink-application]</span><span class="sxs-lookup"><span data-stu-id="7cf37-142">[Create and deploy the blink application][create-and-deploy-the-blink-application]</span></span>

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-c-lesson1-deploy-blink-app.md
[windows]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-mac.md
