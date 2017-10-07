---
title: "Simulerade enhet & Azure IoT Gateway - lektionen 2: Hämta verktyg (macOS) | Microsoft Docs"
description: "Installera verktyg på Mac-dator, skapar en IoT-hubb och registrera enheten i hello IoT-hubb."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT-utveckling, iot program, iot-Molntjänsten, internet saker programvara, azure cli, installera python mac, installera git på mac gulp kör, installera node js mac"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 42f9d186-e20c-4ef9-98cc-71d39e058b06
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 391d60f3cbb209698cae53098efed360ac0f5fad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-macos"></a><span data-ttu-id="0b5dc-104">Hämta hello verktyg (macOS)</span><span class="sxs-lookup"><span data-stu-id="0b5dc-104">Get hello tools (macOS)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0b5dc-105">Windows 7 eller senare</span><span class="sxs-lookup"><span data-stu-id="0b5dc-105">Windows 7 or later</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
> * [<span data-ttu-id="0b5dc-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="0b5dc-106">Ubuntu 16.04</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="0b5dc-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="0b5dc-107">macOS 10.10</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="0b5dc-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="0b5dc-108">What you will do</span></span>

- <span data-ttu-id="0b5dc-109">Installera Git, Node.js, Gulp, Python.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-109">Install Git, Node.js, Gulp, Python.</span></span>
- <span data-ttu-id="0b5dc-110">Installera hello Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="0b5dc-110">Install hello Azure command-line interface (Azure CLI).</span></span> 

<span data-ttu-id="0b5dc-111">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="0b5dc-111">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="0b5dc-112">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="0b5dc-112">What you will learn</span></span>

<span data-ttu-id="0b5dc-113">I den här lektionen får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="0b5dc-113">In this lesson, you will learn:</span></span>

- <span data-ttu-id="0b5dc-114">Hur tooinstall [Git](https://git-scm.com/) och [Node.js](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="0b5dc-114">How tooinstall [Git](https://git-scm.com/) and [Node.js](https://nodejs.org/en/).</span></span>
  - <span data-ttu-id="0b5dc-115">Git är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-115">Git is an open source distributed version control system.</span></span> <span data-ttu-id="0b5dc-116">hello-exempelprogram för den här lektionen lagras på Git.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-116">hello sample application for this lesson is stored on Git.</span></span>
  - <span data-ttu-id="0b5dc-117">Node.js är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-117">Node.js is a JavaScript runtime with a rich package ecosystem.</span></span>
- <span data-ttu-id="0b5dc-118">Hur toouse [NPM](https://www.npmjs.com/) tooinstall Node.js utvecklingsverktyg.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-118">How toouse [NPM](https://www.npmjs.com/) tooinstall Node.js development tools.</span></span>
  - <span data-ttu-id="0b5dc-119">hello version som krävs av Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-119">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  - <span data-ttu-id="0b5dc-120">NPM är en av hello paketet chefer för Node.js.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-120">NPM is one of hello package managers for Node.js.</span></span>
- <span data-ttu-id="0b5dc-121">Hur tooinstall Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-121">How tooinstall Visual Studio Code.</span></span>
  - <span data-ttu-id="0b5dc-122">Visual Studio-koden är plattformsoberoende redigerare för enkel men kraftfull källa för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-122">Visual Studio Code is a cross platform, lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="0b5dc-123">Det har bra stöd för felsökning, inbäddad Git-kontroll, syntaxmarkering, intelligent kod slutförande, kodavsnitt och koden eftersom samt.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-123">It has great support for debugging, embedded Git control, syntax highlighting, intelligent code completion, snippets, and code refactoring as well.</span></span>
- <span data-ttu-id="0b5dc-124">Hur tooinstall Python.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-124">How tooinstall Python.</span></span>
  - <span data-ttu-id="0b5dc-125">Python är en term som används på hög nivå, allmänna, tolkad och dynamiska programmeringsspråk.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-125">Python is a widely used high-level, general-purpose, interpreted and dynamic programming language.</span></span>
- <span data-ttu-id="0b5dc-126">Hur tooinstall hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-126">How tooinstall hello Azure CLI.</span></span>
  - <span data-ttu-id="0b5dc-127">hello Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-127">hello Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="0b5dc-128">Du arbetar direkt från en kommandorad tooprovision och hantera resurser.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-128">You work directly from a command line tooprovision and manage resources.</span></span>
- <span data-ttu-id="0b5dc-129">Hur toouse hello Azure CLI toocreate en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-129">How toouse hello Azure CLI toocreate an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="0b5dc-130">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="0b5dc-130">What you need</span></span>

- <span data-ttu-id="0b5dc-131">En Internet-anslutning toodownload hello verktyg och program.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-131">An Internet connection toodownload hello tools and software.</span></span>
- <span data-ttu-id="0b5dc-132">En Mac-dator som kör OS X Yosemite (10.10) eller senare.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-132">A Mac computer that’s running OS X Yosemite (10.10) or later.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="0b5dc-133">Installera Git och Node.js</span><span class="sxs-lookup"><span data-stu-id="0b5dc-133">Install Git and Node.js</span></span>

<span data-ttu-id="0b5dc-134">tooinstall Git och Node.js, använder du hanteringsverktyg för hello Homebrew paketet genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="0b5dc-134">tooinstall Git and Node.js, use hello Homebrew package management utility by following these steps:</span></span>

1. <span data-ttu-id="0b5dc-135">[Hämta](http://brew.sh/) och installera Homebrew.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-135">[Download](http://brew.sh/) and install Homebrew.</span></span> <span data-ttu-id="0b5dc-136">Om du redan har installerat Homebrew går toostep 2.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-136">If you’ve already installed Homebrew, go toostep 2.</span></span>
   1. <span data-ttu-id="0b5dc-137">Tryck på `Cmd + Space` och ange `Terminal` tooopen en terminal.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-137">Press `Cmd + Space` and enter `Terminal` tooopen a terminal.</span></span>
   2. <span data-ttu-id="0b5dc-138">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="0b5dc-138">Run hello following command:</span></span>

      ```bash
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
      ```

2. <span data-ttu-id="0b5dc-139">Installera Git och Node.js genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="0b5dc-139">Install Git and Node.js by running hello following command:</span></span>

    ```bash
    brew install node git
    ```

## <a name="install-nodejs-development-tools"></a><span data-ttu-id="0b5dc-140">Installera Node.js utvecklingsverktyg</span><span class="sxs-lookup"><span data-stu-id="0b5dc-140">Install Node.js development tools</span></span>

<span data-ttu-id="0b5dc-141">Du använder [gulp.js](http://gulpjs.com/) tooautomate distribution och körning av skript.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-141">You use [gulp.js](http://gulpjs.com/) tooautomate deployment and execution of scripts.</span></span>

<span data-ttu-id="0b5dc-142">tooinstall gulp kör följande kommando i hello terminal hello:</span><span class="sxs-lookup"><span data-stu-id="0b5dc-142">tooinstall gulp, run hello following command in hello terminal:</span></span>

```bash
npm install -g gulp
```

<span data-ttu-id="0b5dc-143">Om du får problem med hello installation finns hello [felsökningsguide för](iot-hub-gateway-kit-c-sim-troubleshooting.md) för lösningar toocommon problem.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-143">If you experience issues with hello installation, see hello [troubleshooting guide](iot-hub-gateway-kit-c-sim-troubleshooting.md) for solutions toocommon problems.</span></span>

> [!Note]
> <span data-ttu-id="0b5dc-144">Noden, NPM och Gulp är obligatoriska toorun automatiseringsskript utvecklats i Node.js.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-144">Node, NPM and Gulp are required toorun automation scripts developed in Node.js.</span></span>

## <a name="install-python"></a><span data-ttu-id="0b5dc-145">Installera Python</span><span class="sxs-lookup"><span data-stu-id="0b5dc-145">Install Python</span></span>

<span data-ttu-id="0b5dc-146">Även om Mac OS X har Python 2.7, rekommenderar vi att du installerar Python via Homebrew.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-146">Although Mac OS X comes with Python 2.7, we recommend that you install Python through Homebrew.</span></span> <span data-ttu-id="0b5dc-147">Se [installerar Python på Mac OS X](http://docs.python-guide.org/en/latest/starting/install/osx/).</span><span class="sxs-lookup"><span data-stu-id="0b5dc-147">See [Installing Python on Mac OS X](http://docs.python-guide.org/en/latest/starting/install/osx/).</span></span>

<span data-ttu-id="0b5dc-148">Installera Python och pip genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="0b5dc-148">Install Python and pip by running hello following command:</span></span>

```bash
brew install python
```

## <a name="install-hello-azure-cli"></a><span data-ttu-id="0b5dc-149">Installera hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="0b5dc-149">Install hello Azure CLI</span></span>

<span data-ttu-id="0b5dc-150">tooinstall hello Azure CLI, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="0b5dc-150">tooinstall hello Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="0b5dc-151">Kör följande kommandon i hello terminal hello:</span><span class="sxs-lookup"><span data-stu-id="0b5dc-151">Run hello following commands in hello terminal:</span></span>
   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
   <span data-ttu-id="0b5dc-152">hello-installationen kan ta 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-152">hello installation might take 5 minutes.</span></span>

2. <span data-ttu-id="0b5dc-153">Kontrollera hello installationen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="0b5dc-153">Verify hello installation by running hello following command:</span></span>
   ```bash
   az iot -h
   ```
   <span data-ttu-id="0b5dc-154">Du bör se hello följande utdata om hello-installationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-154">You should see hello following output if hello installation is successful.</span></span>

   ![Verifiera installation av Azure CLI](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_osx.png)

## <a name="install-visual-studio-code"></a><span data-ttu-id="0b5dc-156">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0b5dc-156">Install Visual Studio Code</span></span>

<span data-ttu-id="0b5dc-157">Du kan använda Visual Studio Code senare i hello självstudiekursen tooedit konfigurationsfiler.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-157">You use Visual Studio Code later in hello tutorial tooedit configuration files.</span></span>

<span data-ttu-id="0b5dc-158">[Hämta](https://code.visualstudio.com/docs/setup/osx) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-158">[Download](https://code.visualstudio.com/docs/setup/osx) and install Visual Studio Code.</span></span>

## <a name="summary"></a><span data-ttu-id="0b5dc-159">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="0b5dc-159">Summary</span></span>

<span data-ttu-id="0b5dc-160">Du har installerat alla hello krävs verktyg och program på en Mac-dator.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-160">You’ve installed all hello required tools and software on your Mac computer.</span></span> <span data-ttu-id="0b5dc-161">Nästa uppgift är toouse hello Azure CLI toocreate en IoT-hubb och registrera enheten i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="0b5dc-161">Your next task is toouse hello Azure CLI toocreate an IoT hub and register your device in your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0b5dc-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0b5dc-162">Next steps</span></span>
[<span data-ttu-id="0b5dc-163">Skapa en IoT-hubb och registrera enheten</span><span class="sxs-lookup"><span data-stu-id="0b5dc-163">Create an IoT hub and register Device</span></span>](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)
