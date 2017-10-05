---
title: "Simulerade enhet & Azure IoT Gateway - lektionen 2: Hämta verktyg (Ubuntu) | Microsoft Docs"
description: "Installera verktygen och programvaran på din värddator som kör Ubuntu, skapar en IoT-hubb och registrera enheten i IoT-hubben."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT-utveckling, iot-programvara, iot-Molntjänsten, internet av saker programvara, azure cli git på ubuntu, gulp kör, installera node js ubuntu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: cf673154-ce67-4ed7-a9f7-2440301c6270
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 349daf5c3206f7fc20662beebd16928624142a56
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-ubuntu-1604"></a><span data-ttu-id="9a024-104">Hämta verktygen (Ubuntu 16.04)</span><span class="sxs-lookup"><span data-stu-id="9a024-104">Get the tools (Ubuntu 16.04)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9a024-105">Windows 7 eller senare</span><span class="sxs-lookup"><span data-stu-id="9a024-105">Windows 7 or later</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
> * [<span data-ttu-id="9a024-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="9a024-106">Ubuntu 16.04</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="9a024-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="9a024-107">macOS 10.10</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="9a024-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="9a024-108">What you will do</span></span>

- <span data-ttu-id="9a024-109">Installera Git, Node.js, Gulp, Python.</span><span class="sxs-lookup"><span data-stu-id="9a024-109">Install Git, Node.js, Gulp, Python.</span></span>
- <span data-ttu-id="9a024-110">Installera Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="9a024-110">Install the Azure command-line interface (Azure CLI).</span></span> 

<span data-ttu-id="9a024-111">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="9a024-111">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>
## <a name="what-you-will-learn"></a><span data-ttu-id="9a024-112">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="9a024-112">What you will learn</span></span>

<span data-ttu-id="9a024-113">I den här lektionen får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="9a024-113">In this lesson, you will learn:</span></span>

- <span data-ttu-id="9a024-114">Hur du installerar Git och Node.js.</span><span class="sxs-lookup"><span data-stu-id="9a024-114">How to install Git and Node.js.</span></span>
  - <span data-ttu-id="9a024-115">Git är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="9a024-115">Git is an open source distributed version control system.</span></span> <span data-ttu-id="9a024-116">Exempelprogram för den här lektionen lagras på Git.</span><span class="sxs-lookup"><span data-stu-id="9a024-116">The sample application for this lesson is stored on Git.</span></span>
  - <span data-ttu-id="9a024-117">Node.js är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="9a024-117">Node.js is a JavaScript runtime with a rich package ecosystem.</span></span>
- <span data-ttu-id="9a024-118">Hur du använder NPM installera Node.js utvecklingsverktyg.</span><span class="sxs-lookup"><span data-stu-id="9a024-118">How to use NPM to install Node.js development tools.</span></span>
  - <span data-ttu-id="9a024-119">Versionen som krävs för Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="9a024-119">The minimum required version of Node.js is 4.5 LTS.</span></span>
  - <span data-ttu-id="9a024-120">NPM är en av de paket cheferna för Node.js.</span><span class="sxs-lookup"><span data-stu-id="9a024-120">NPM is one of the package managers for Node.js.</span></span>
- <span data-ttu-id="9a024-121">Så här installerar Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="9a024-121">How to install Visual Studio Code.</span></span>
  - <span data-ttu-id="9a024-122">Visual Studio-koden är plattformsoberoende redigerare för enkel men kraftfull källa för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="9a024-122">Visual Studio Code is a cross platform, lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="9a024-123">Det har bra stöd för felsökning, inbäddad Git-kontroll, syntaxmarkering, intelligent kod slutförande, kodavsnitt och koden eftersom samt.</span><span class="sxs-lookup"><span data-stu-id="9a024-123">It has great support for debugging, embedded Git control, syntax highlighting, intelligent code completion, snippets, and code refactoring as well.</span></span>
- <span data-ttu-id="9a024-124">Så här installerar du Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9a024-124">How to install the Azure CLI</span></span>
  - <span data-ttu-id="9a024-125">Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure.</span><span class="sxs-lookup"><span data-stu-id="9a024-125">The Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="9a024-126">Du arbetar direkt från en kommandorad för att etablera och hantera resurser.</span><span class="sxs-lookup"><span data-stu-id="9a024-126">You work directly from a command line to provision and manage resources.</span></span>
- <span data-ttu-id="9a024-127">Hur du använder Azure CLI för att skapa en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="9a024-127">How to use the Azure CLI to create an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="9a024-128">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="9a024-128">What you need</span></span>

- <span data-ttu-id="9a024-129">En Internet-anslutning att hämta verktyg och program.</span><span class="sxs-lookup"><span data-stu-id="9a024-129">An Internet connection to download the tools and software.</span></span>
- <span data-ttu-id="9a024-130">En dator som kör Ubuntu 16.04 eller senare.</span><span class="sxs-lookup"><span data-stu-id="9a024-130">A computer that is running Ubuntu 16.04 or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="9a024-131">Installera Git och Node.js</span><span class="sxs-lookup"><span data-stu-id="9a024-131">Install Git and Node.js</span></span>

<span data-ttu-id="9a024-132">Följ dessa steg om du vill installera Git och Node.js:</span><span class="sxs-lookup"><span data-stu-id="9a024-132">To install Git and Node.js, follow these steps:</span></span>

1. <span data-ttu-id="9a024-133">Tryck på `Ctrl + Alt + T` att öppna en terminal.</span><span class="sxs-lookup"><span data-stu-id="9a024-133">Press `Ctrl + Alt + T` to open a terminal.</span></span>
2. <span data-ttu-id="9a024-134">Kör följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="9a024-134">Run the following commands:</span></span>

   ```bash
   sudo apt-get update
   curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
   sudo apt-get install -y nodejs
   sudo apt-get install git
   ```

## <a name="install-nodejs-development-tools"></a><span data-ttu-id="9a024-135">Installera Node.js utvecklingsverktyg</span><span class="sxs-lookup"><span data-stu-id="9a024-135">Install Node.js development tools</span></span>

<span data-ttu-id="9a024-136">Du använder [gulp.js](http://gulpjs.com/) att automatisera distribution och körning av skript.</span><span class="sxs-lookup"><span data-stu-id="9a024-136">You use [gulp.js](http://gulpjs.com/) to automate deployment and execution of scripts.</span></span>

<span data-ttu-id="9a024-137">Om du vill installera gulp, kör du följande kommando i en terminal:</span><span class="sxs-lookup"><span data-stu-id="9a024-137">To install gulp, run the following command in the terminal:</span></span>

```bash
sudo npm install -g gulp
```

<span data-ttu-id="9a024-138">Om du får problem med installationen finns i [felsökningsguide för](iot-hub-gateway-kit-c-sim-troubleshooting.md) efter lösningar på vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="9a024-138">If you experience issues with the installation, see the [troubleshooting guide](iot-hub-gateway-kit-c-sim-troubleshooting.md) for solutions to common problems.</span></span>

> [!Note]
> <span data-ttu-id="9a024-139">Noden, NPM och Gulp krävs för att köra automatiserade skript som utvecklats i Node.js.</span><span class="sxs-lookup"><span data-stu-id="9a024-139">Node, NPM and Gulp are required to run automation scripts developed in Node.js.</span></span>

## <a name="install-the-azure-cli"></a><span data-ttu-id="9a024-140">Installera Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9a024-140">Install the Azure CLI</span></span>

<span data-ttu-id="9a024-141">Så här installerar du Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="9a024-141">To install the Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="9a024-142">Kör följande kommandon i terminalen:</span><span class="sxs-lookup"><span data-stu-id="9a024-142">Run the following commands in the terminal:</span></span>

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```

   <span data-ttu-id="9a024-143">Installationen kan ta 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="9a024-143">The installation might take 5 minutes.</span></span>

2. <span data-ttu-id="9a024-144">Verifiera installationen genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9a024-144">Verify the installation by running the following command:</span></span>

   ```bash
   az iot -h
   ```
<span data-ttu-id="9a024-145">Du bör se följande utdata om installationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="9a024-145">You should see the following output if the installation is successful.</span></span>
<span data-ttu-id="9a024-146">![Verifiera installation av Azure CLI](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_ubuntu.png)</span><span class="sxs-lookup"><span data-stu-id="9a024-146">![Verify Azure CLI installation](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_ubuntu.png)</span></span>

### <a name="install-visual-studio-code"></a><span data-ttu-id="9a024-147">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9a024-147">Install Visual Studio Code</span></span>

<span data-ttu-id="9a024-148">Du kan använda Visual Studio Code senare under kursen för att redigera konfigurationsfiler.</span><span class="sxs-lookup"><span data-stu-id="9a024-148">You use Visual Studio Code later in the tutorial to edit configuration files.</span></span>

<span data-ttu-id="9a024-149">[Hämta](https://code.visualstudio.com/docs/setup/linux) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="9a024-149">[Download](https://code.visualstudio.com/docs/setup/linux) and install Visual Studio Code.</span></span>

## <a name="summary"></a><span data-ttu-id="9a024-150">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="9a024-150">Summary</span></span>

<span data-ttu-id="9a024-151">Du har installerat alla verktyg som krävs och programvara på värddatorn.</span><span class="sxs-lookup"><span data-stu-id="9a024-151">You've installed all the required tools and software on your host computer.</span></span> <span data-ttu-id="9a024-152">Nästa uppgift är att använda Azure CLI för att skapa en IoT-hubb och registrera enheten i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="9a024-152">Your next task is to use the Azure CLI to create an IoT hub and register your device in your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9a024-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9a024-153">Next steps</span></span>
[<span data-ttu-id="9a024-154">Skapa en IoT-hubb och registrera din enhet</span><span class="sxs-lookup"><span data-stu-id="9a024-154">Create an IoT hub and register your device</span></span>](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)
