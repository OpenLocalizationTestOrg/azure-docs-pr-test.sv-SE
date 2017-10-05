---
title: "SensorTag enhet & Azure IoT-Gateway - lektion 2: Hämta verktyg (macOS) | Microsoft Docs"
description: "Installera verktyg på Mac-dator, skapa en IoT-hubb och registrera enheten i IoT-hubben."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT-utveckling, iot program, iot-Molntjänsten, internet saker programvara, azure cli, installera python mac, installera git på mac gulp kör, installera node js mac"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 94e538ef-9857-4023-9c3c-e92a0be7c395
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 07bc5f2cf6542273c334371b77520c674c5d2f00
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-macos"></a><span data-ttu-id="c12eb-104">Skaffa dig verktyg (MacOS)</span><span class="sxs-lookup"><span data-stu-id="c12eb-104">Get the tools (MacOS)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c12eb-105">Windows 7 eller senare</span><span class="sxs-lookup"><span data-stu-id="c12eb-105">Windows 7 or later</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
> * [<span data-ttu-id="c12eb-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="c12eb-106">Ubuntu 16.04</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="c12eb-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="c12eb-107">macOS 10.10</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="c12eb-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="c12eb-108">What you will do</span></span>

- <span data-ttu-id="c12eb-109">Installera Git, Node.js, Gulp, Python.</span><span class="sxs-lookup"><span data-stu-id="c12eb-109">Install Git, Node.js, Gulp, Python.</span></span>
- <span data-ttu-id="c12eb-110">Installera Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="c12eb-110">Install the Azure command-line interface (Azure CLI).</span></span> 

<span data-ttu-id="c12eb-111">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="c12eb-111">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="c12eb-112">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="c12eb-112">What you will learn</span></span>

<span data-ttu-id="c12eb-113">I den här lektionen får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="c12eb-113">In this lesson, you will learn:</span></span>

- <span data-ttu-id="c12eb-114">Så här installerar du [Git](https://git-scm.com/) och [Node.js](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="c12eb-114">How to install [Git](https://git-scm.com/) and [Node.js](https://nodejs.org/en/).</span></span>
  - <span data-ttu-id="c12eb-115">Git är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="c12eb-115">Git is an open source distributed version control system.</span></span> <span data-ttu-id="c12eb-116">Exempelprogram för den här lektionen lagras på Git.</span><span class="sxs-lookup"><span data-stu-id="c12eb-116">The sample application for this lesson is stored on Git.</span></span>
  - <span data-ttu-id="c12eb-117">Node.js är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="c12eb-117">Node.js is a JavaScript runtime with a rich package ecosystem.</span></span>
- <span data-ttu-id="c12eb-118">Hur du använder [NPM](https://www.npmjs.com/) installera Node.js utvecklingsverktyg.</span><span class="sxs-lookup"><span data-stu-id="c12eb-118">How to use [NPM](https://www.npmjs.com/) to install Node.js development tools.</span></span>
  - <span data-ttu-id="c12eb-119">Versionen som krävs för Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="c12eb-119">The minimum required version of Node.js is 4.5 LTS.</span></span>
  - <span data-ttu-id="c12eb-120">NPM är en av de paket cheferna för Node.js.</span><span class="sxs-lookup"><span data-stu-id="c12eb-120">NPM is one of the package managers for Node.js.</span></span>
- <span data-ttu-id="c12eb-121">Så här installerar Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="c12eb-121">How to install Visual Studio Code.</span></span>
  - <span data-ttu-id="c12eb-122">Visual Studio-koden är plattformsoberoende redigerare för enkel men kraftfull källa för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="c12eb-122">Visual Studio Code is a cross platform, lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="c12eb-123">Det har bra stöd för felsökning, inbäddad Git-kontroll, syntaxmarkering, intelligent kod slutförande, kodavsnitt och koden eftersom samt.</span><span class="sxs-lookup"><span data-stu-id="c12eb-123">It has great support for debugging, embedded Git control, syntax highlighting, intelligent code completion, snippets, and code refactoring as well.</span></span>
- <span data-ttu-id="c12eb-124">Hur du installerar Python.</span><span class="sxs-lookup"><span data-stu-id="c12eb-124">How to install Python.</span></span>
  - <span data-ttu-id="c12eb-125">Python är en term som används på hög nivå, allmänna, tolkad och dynamiska programmeringsspråk.</span><span class="sxs-lookup"><span data-stu-id="c12eb-125">Python is a widely used high-level, general-purpose, interpreted and dynamic programming language.</span></span>
- <span data-ttu-id="c12eb-126">Så här installerar du Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="c12eb-126">How to install the Azure CLI.</span></span>
  - <span data-ttu-id="c12eb-127">Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure.</span><span class="sxs-lookup"><span data-stu-id="c12eb-127">The Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="c12eb-128">Du arbetar direkt från en kommandorad för att etablera och hantera resurser.</span><span class="sxs-lookup"><span data-stu-id="c12eb-128">You work directly from a command line to provision and manage resources.</span></span>
- <span data-ttu-id="c12eb-129">Hur du använder Azure CLI för att skapa en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c12eb-129">How to use the Azure CLI to create an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="c12eb-130">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="c12eb-130">What you need</span></span>

- <span data-ttu-id="c12eb-131">En Internet-anslutning att hämta verktyg och program.</span><span class="sxs-lookup"><span data-stu-id="c12eb-131">An Internet connection to download the tools and software.</span></span>
- <span data-ttu-id="c12eb-132">En Mac-dator som kör OS X Yosemite (10.10) eller senare.</span><span class="sxs-lookup"><span data-stu-id="c12eb-132">A Mac computer that’s running OS X Yosemite (10.10) or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="c12eb-133">Installera Git och Node.js</span><span class="sxs-lookup"><span data-stu-id="c12eb-133">Install Git and Node.js</span></span>

<span data-ttu-id="c12eb-134">Installera Git och Node.js med verktyget Homebrew paketet för hantering genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="c12eb-134">To install Git and Node.js, use the Homebrew package management utility by following these steps:</span></span>

1. <span data-ttu-id="c12eb-135">[Hämta](http://brew.sh/) och installera Homebrew.</span><span class="sxs-lookup"><span data-stu-id="c12eb-135">[Download](http://brew.sh/) and install Homebrew.</span></span> <span data-ttu-id="c12eb-136">Om du redan har installerat Homebrew går du till steg 2.</span><span class="sxs-lookup"><span data-stu-id="c12eb-136">If you’ve already installed Homebrew, go to step 2.</span></span>
   1. <span data-ttu-id="c12eb-137">Tryck på `Cmd + Space` och ange `Terminal` att öppna en terminal.</span><span class="sxs-lookup"><span data-stu-id="c12eb-137">Press `Cmd + Space` and enter `Terminal` to open a terminal.</span></span>
   2. <span data-ttu-id="c12eb-138">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c12eb-138">Run the following command:</span></span>

      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```

2. <span data-ttu-id="c12eb-139">Installera Git och Node.js genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c12eb-139">Install Git and Node.js by running the following command:</span></span>

    ```bash
    brew install node git
    ```

## <a name="install-nodejs-development-tools"></a><span data-ttu-id="c12eb-140">Installera Node.js utvecklingsverktyg</span><span class="sxs-lookup"><span data-stu-id="c12eb-140">Install Node.js development tools</span></span>

<span data-ttu-id="c12eb-141">Du använder [gulp.js](http://gulpjs.com/) att automatisera distribution och körning av skript.</span><span class="sxs-lookup"><span data-stu-id="c12eb-141">You use [gulp.js](http://gulpjs.com/) to automate deployment and execution of scripts.</span></span>

<span data-ttu-id="c12eb-142">Om du vill installera gulp, kör du följande kommando i en terminal:</span><span class="sxs-lookup"><span data-stu-id="c12eb-142">To install gulp, run the following command in the terminal:</span></span>

```bash
npm install -g gulp
```

<span data-ttu-id="c12eb-143">Om du får problem med installationen finns i [felsökningsguide för](iot-hub-gateway-kit-c-troubleshooting.md) efter lösningar på vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="c12eb-143">If you experience issues with the installation, see the [troubleshooting guide](iot-hub-gateway-kit-c-troubleshooting.md) for solutions to common problems.</span></span>

> [!Note]
> <span data-ttu-id="c12eb-144">Noden, NPM och Gulp krävs för att köra automatiserade skript som utvecklats i Node.js.</span><span class="sxs-lookup"><span data-stu-id="c12eb-144">Node, NPM and Gulp are required to run automation scripts developed in Node.js.</span></span>

## <a name="install-python"></a><span data-ttu-id="c12eb-145">Installera Python</span><span class="sxs-lookup"><span data-stu-id="c12eb-145">Install Python</span></span>

<span data-ttu-id="c12eb-146">Även om Mac OS X har Python 2.7, rekommenderar vi att du installerar Python via Homebrew.</span><span class="sxs-lookup"><span data-stu-id="c12eb-146">Although Mac OS X comes with Python 2.7, we recommend that you install Python through Homebrew.</span></span> <span data-ttu-id="c12eb-147">Se [installerar Python på Mac OS X](http://docs.python-guide.org/en/latest/starting/install/osx/).</span><span class="sxs-lookup"><span data-stu-id="c12eb-147">See [Installing Python on Mac OS X](http://docs.python-guide.org/en/latest/starting/install/osx/).</span></span>

<span data-ttu-id="c12eb-148">Installera Python och pip genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c12eb-148">Install Python and pip by running the following command:</span></span>

```bash
brew install python
```

## <a name="install-the-azure-cli"></a><span data-ttu-id="c12eb-149">Installera Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c12eb-149">Install the Azure CLI</span></span>

<span data-ttu-id="c12eb-150">Så här installerar du Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="c12eb-150">To install the Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="c12eb-151">Kör följande kommandon i terminalen:</span><span class="sxs-lookup"><span data-stu-id="c12eb-151">Run the following commands in the terminal:</span></span>
   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
   <span data-ttu-id="c12eb-152">Installationen kan ta 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="c12eb-152">The installation might take 5 minutes.</span></span>

2. <span data-ttu-id="c12eb-153">Verifiera installationen genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c12eb-153">Verify the installation by running the following command:</span></span>
   ```bash
   az iot -h
   ```
   <span data-ttu-id="c12eb-154">Du bör se följande utdata om installationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="c12eb-154">You should see the following output if the installation is successful.</span></span>

   ![Verifiera installation av Azure CLI](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_osx.png)

## <a name="install-visual-studio-code"></a><span data-ttu-id="c12eb-156">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c12eb-156">Install Visual Studio Code</span></span>

<span data-ttu-id="c12eb-157">Du kan använda Visual Studio Code senare under kursen för att redigera konfigurationsfiler.</span><span class="sxs-lookup"><span data-stu-id="c12eb-157">You use Visual Studio Code later in the tutorial to edit configuration files.</span></span>

<span data-ttu-id="c12eb-158">[Hämta](https://code.visualstudio.com/docs/setup/osx) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="c12eb-158">[Download](https://code.visualstudio.com/docs/setup/osx) and install Visual Studio Code.</span></span>

## <a name="summary"></a><span data-ttu-id="c12eb-159">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="c12eb-159">Summary</span></span>

<span data-ttu-id="c12eb-160">Du har installerat alla verktyg som krävs och programvara på en Mac-dator.</span><span class="sxs-lookup"><span data-stu-id="c12eb-160">You’ve installed all the required tools and software on your Mac computer.</span></span> <span data-ttu-id="c12eb-161">Nästa uppgift är att använda Azure CLI för att skapa en IoT-hubb och registrera enheten i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c12eb-161">Your next task is to use the Azure CLI to create an IoT hub and register your device in your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c12eb-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c12eb-162">Next steps</span></span>
[<span data-ttu-id="c12eb-163">Skapa en IoT-hubb och registrera enheten</span><span class="sxs-lookup"><span data-stu-id="c12eb-163">Create an IoT hub and register Device</span></span>](iot-hub-gateway-kit-c-lesson2-register-device.md)
