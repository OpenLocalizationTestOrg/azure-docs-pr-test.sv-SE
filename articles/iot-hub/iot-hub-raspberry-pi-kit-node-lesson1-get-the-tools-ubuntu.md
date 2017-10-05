---
title: "Ansluta hallon Pi (nod) till Azure IoT - lektionen 1: Hämta verktyg (Ubuntu) | Microsoft Docs"
description: "Hämta och installera verktyg och programvaran för det första exempelprogrammet för Pi på Ubuntu."
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
ms.openlocfilehash: de583be0cdce058c83091f421376812e8013d76e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-ubuntu-1604"></a><span data-ttu-id="89e74-104">Hämta verktygen (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="89e74-104">Get the tools (Ubuntu 16.04)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="89e74-105">Windows 7 eller senare</span><span class="sxs-lookup"><span data-stu-id="89e74-105">Windows 7 or later</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
> * [<span data-ttu-id="89e74-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="89e74-106">Ubuntu 16.04</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="89e74-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="89e74-107">macOS 10.10</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)


## <a name="what-you-will-do"></a><span data-ttu-id="89e74-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="89e74-108">What you will do</span></span>
<span data-ttu-id="89e74-109">Hämta utvecklingsverktyg och programvaran för det första exempelprogrammet för hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="89e74-109">Download the development tools and the software for the first sample application for Raspberry Pi 3.</span></span> <span data-ttu-id="89e74-110">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="89e74-110">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="89e74-111">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="89e74-111">What you will learn</span></span>
<span data-ttu-id="89e74-112">I den här artikeln får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="89e74-112">In this article, you will learn:</span></span>

* <span data-ttu-id="89e74-113">Hur du installerar Git och Node.js.</span><span class="sxs-lookup"><span data-stu-id="89e74-113">How to install Git and Node.js.</span></span>
  * <span data-ttu-id="89e74-114">[Git](https://git-scm.com) är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="89e74-114">[Git](https://git-scm.com) is an open source distributed version control system.</span></span> <span data-ttu-id="89e74-115">Exempelprogram för den här artikeln är lagrade på Git.</span><span class="sxs-lookup"><span data-stu-id="89e74-115">The sample application for this article is stored on Git.</span></span>
  * <span data-ttu-id="89e74-116">[Node.js](https://nodejs.org/en/) är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="89e74-116">[Node.js](https://nodejs.org/en/) is a JavaScript runtime with a rich package ecosystem.</span></span>
* <span data-ttu-id="89e74-117">Hur du använder NPM för att installera ytterligare Node.js-utvecklingsverktyg.</span><span class="sxs-lookup"><span data-stu-id="89e74-117">How to use NPM to install additional Node.js development tools.</span></span>
  * <span data-ttu-id="89e74-118">Versionen som krävs för Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="89e74-118">The minimum required version of Node.js is 4.5 LTS.</span></span>
  * <span data-ttu-id="89e74-119">[NPM](https://www.npmjs.com) är en av de paket cheferna för Node.js.</span><span class="sxs-lookup"><span data-stu-id="89e74-119">[NPM](https://www.npmjs.com) is one of the package managers for Node.js.</span></span>

## <a name="what-do-you-need"></a><span data-ttu-id="89e74-120">Vad behöver du</span><span class="sxs-lookup"><span data-stu-id="89e74-120">What do you need</span></span>
<span data-ttu-id="89e74-121">För att slutföra den här åtgärden behöver du:</span><span class="sxs-lookup"><span data-stu-id="89e74-121">To complete this operation, you will need:</span></span>

* <span data-ttu-id="89e74-122">En Internet-anslutning att hämta utvecklingsverktyg och programvaran.</span><span class="sxs-lookup"><span data-stu-id="89e74-122">An Internet connection to download the development tools and the software.</span></span>
* <span data-ttu-id="89e74-123">En dator som kör Ubuntu 16.04 eller senare.</span><span class="sxs-lookup"><span data-stu-id="89e74-123">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-nodejs-and-npm"></a><span data-ttu-id="89e74-124">Installera Git, Node.js och NPM</span><span class="sxs-lookup"><span data-stu-id="89e74-124">Install Git, Node.js, and NPM</span></span>
<span data-ttu-id="89e74-125">Använd kortkommandot `Ctrl + Alt + T` att öppna en terminal och kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="89e74-125">Use the keyboard shortcut `Ctrl + Alt + T` to open a terminal and run the following commands:</span></span>

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="install-additional-nodejs-development-tools"></a><span data-ttu-id="89e74-126">Installera ytterligare utvecklingsverktyg för Node.js</span><span class="sxs-lookup"><span data-stu-id="89e74-126">Install additional Node.js development tools</span></span>
<span data-ttu-id="89e74-127">Du använder [gulp.js](http://gulpjs.com) att automatisera distributionen av exempelprogrammet till Pi.</span><span class="sxs-lookup"><span data-stu-id="89e74-127">You use [gulp.js](http://gulpjs.com) to automate the deployment of the sample application to Pi.</span></span> <span data-ttu-id="89e74-128">Du också använda den [enhet-discovery-cli](https://github.com/Azure/device-discovery-cli) att hämta nätverksinformation om IoT-enheter.</span><span class="sxs-lookup"><span data-stu-id="89e74-128">You also use the [device-discovery-cli](https://github.com/Azure/device-discovery-cli) to retrieve network information about your IoT devices.</span></span>

<span data-ttu-id="89e74-129">Installera `gulp` och `device-discovery-cli` genom att köra följande kommando i terminalen:</span><span class="sxs-lookup"><span data-stu-id="89e74-129">Install `gulp` and `device-discovery-cli` by running the following command in the terminal:</span></span>

```bash
sudo npm install -g device-discovery-cli gulp
```

<span data-ttu-id="89e74-130">Om du får problem med installationen Node.js och dessa ytterligare utvecklingsverktyg på Ubuntu finns i [felsökningsguide för](iot-hub-raspberry-pi-kit-node-troubleshooting.md) efter lösningar på vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="89e74-130">If you experience issues installing Node.js and these additional development tools on Ubuntu, see the [troubleshooting guide](iot-hub-raspberry-pi-kit-node-troubleshooting.md) for solutions to common problems.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="89e74-131">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="89e74-131">Install Visual Studio Code</span></span>
<span data-ttu-id="89e74-132">[Hämta](https://code.visualstudio.com/docs/setup/linux) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="89e74-132">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span> <span data-ttu-id="89e74-133">Visual Studio-koden är en enkel men kraftfull källa redigerare för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="89e74-133">Visual Studio Code is a lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="89e74-134">Du kan använda den här redigeraren senare under kursen för att redigera exempelkoden.</span><span class="sxs-lookup"><span data-stu-id="89e74-134">You use this editor later in the tutorial to edit the sample code.</span></span>

## <a name="summary"></a><span data-ttu-id="89e74-135">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="89e74-135">Summary</span></span>
<span data-ttu-id="89e74-136">Du har installerat nödvändiga utvecklingsverktyg och programvara för första exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="89e74-136">You've installed the required development tools and software for the first sample application.</span></span> <span data-ttu-id="89e74-137">Nästa uppgift är att skapa, distribuera och köra exempelprogrammet på Pi.</span><span class="sxs-lookup"><span data-stu-id="89e74-137">The next task is to create, deploy, and run the sample application on Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="89e74-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="89e74-138">Next steps</span></span>
[<span data-ttu-id="89e74-139">Skapa och distribuera blinka exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="89e74-139">Create and deploy the blink sample application</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

