---
title: "SensorTag enhet & Azure IoT-Gateway - lektion 2: Hämta verktyg (Windows) | Microsoft Docs"
description: "Installera hello verktyg och hello programvara på din värddator som kör Windows, skapar en IoT-hubb och registrera enheten i hello IoT-hubb."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT-utveckling, iot-programvara, iot-Molntjänsten, internet av saker programvara, azure cli, installera git för windows, gulp kör, node js windows, installera npm i windows, installera python i windows"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 18ae6ee4-574a-4d5f-9838-ca2a78165628
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 3b30b60a0115413394992061a88dde4cd442ac19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-tools-windows-7-and-later"></a><span data-ttu-id="fab7e-104">Hämta hello verktyg (Windows 7 och senare)</span><span class="sxs-lookup"><span data-stu-id="fab7e-104">Get hello tools (Windows 7 and later)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fab7e-105">Windows 7 eller senare</span><span class="sxs-lookup"><span data-stu-id="fab7e-105">Windows 7 or later</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
> * [<span data-ttu-id="fab7e-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="fab7e-106">Ubuntu 16.04</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="fab7e-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="fab7e-107">macOS 10.10</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="fab7e-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="fab7e-108">What you will do</span></span>

- <span data-ttu-id="fab7e-109">Installera Git, Node.js, Gulp, Python.</span><span class="sxs-lookup"><span data-stu-id="fab7e-109">Install Git, Node.js, Gulp, Python.</span></span>
- <span data-ttu-id="fab7e-110">Installera hello Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="fab7e-110">Install hello Azure command-line interface (Azure CLI).</span></span> 

<span data-ttu-id="fab7e-111">Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="fab7e-111">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="fab7e-112">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="fab7e-112">What you will learn</span></span>

<span data-ttu-id="fab7e-113">I den här lektionen får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="fab7e-113">In this lesson, you will learn:</span></span>

- <span data-ttu-id="fab7e-114">Hur tooinstall [Git](https://git-scm.com/) och [Node.js](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="fab7e-114">How tooinstall [Git](https://git-scm.com/) and [Node.js](https://nodejs.org/en/).</span></span>
  - <span data-ttu-id="fab7e-115">Git är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="fab7e-115">Git is an open source distributed version control system.</span></span> <span data-ttu-id="fab7e-116">hello-exempelprogram för den här lektionen lagras på Git.</span><span class="sxs-lookup"><span data-stu-id="fab7e-116">hello sample application for this lesson is stored on Git.</span></span>
  - <span data-ttu-id="fab7e-117">Node.js är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="fab7e-117">Node.js is a JavaScript runtime with a rich package ecosystem.</span></span>
- <span data-ttu-id="fab7e-118">Hur toouse [NPM](https://www.npmjs.com/) tooinstall Node.js utvecklingsverktyg.</span><span class="sxs-lookup"><span data-stu-id="fab7e-118">How toouse [NPM](https://www.npmjs.com/) tooinstall Node.js development tools.</span></span>
  - <span data-ttu-id="fab7e-119">hello version som krävs av Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="fab7e-119">hello minimum required version of Node.js is 4.5 LTS.</span></span>
  - <span data-ttu-id="fab7e-120">NPM är en av hello paketet chefer för Node.js.</span><span class="sxs-lookup"><span data-stu-id="fab7e-120">NPM is one of hello package managers for Node.js.</span></span>
- <span data-ttu-id="fab7e-121">Hur tooinstall Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="fab7e-121">How tooinstall Visual Studio Code.</span></span>
  - <span data-ttu-id="fab7e-122">Visual Studio-koden är plattformsoberoende redigerare för enkel men kraftfull källa för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="fab7e-122">Visual Studio Code is a cross platform, lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="fab7e-123">Det har bra stöd för felsökning, inbäddad Git-kontroll, syntaxmarkering, intelligent kod slutförande, kodavsnitt och koden eftersom samt.</span><span class="sxs-lookup"><span data-stu-id="fab7e-123">It has great support for debugging, embedded Git control, syntax highlighting, intelligent code completion, snippets, and code refactoring as well.</span></span>
- <span data-ttu-id="fab7e-124">Hur tooinstall Python.</span><span class="sxs-lookup"><span data-stu-id="fab7e-124">How tooinstall Python.</span></span>
  - <span data-ttu-id="fab7e-125">Python är en term som används på hög nivå, allmänna, tolkad och dynamiska programmeringsspråk.</span><span class="sxs-lookup"><span data-stu-id="fab7e-125">Python is a widely used high-level, general-purpose, interpreted and dynamic programming language.</span></span>
- <span data-ttu-id="fab7e-126">Hur tooinstall hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="fab7e-126">How tooinstall hello Azure CLI.</span></span>
  - <span data-ttu-id="fab7e-127">hello Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure.</span><span class="sxs-lookup"><span data-stu-id="fab7e-127">hello Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="fab7e-128">Du arbetar direkt från en kommandorad tooprovision och hantera resurser.</span><span class="sxs-lookup"><span data-stu-id="fab7e-128">You work directly from a command line tooprovision and manage resources.</span></span>
- <span data-ttu-id="fab7e-129">Hur toouse hello Azure CLI toocreate en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="fab7e-129">How toouse hello Azure CLI toocreate an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="fab7e-130">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="fab7e-130">What you need</span></span>

- <span data-ttu-id="fab7e-131">En Internet-anslutning toodownload hello verktyg och program.</span><span class="sxs-lookup"><span data-stu-id="fab7e-131">An Internet connection toodownload hello tools and software.</span></span>
- <span data-ttu-id="fab7e-132">En Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="fab7e-132">A Windows computer.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="fab7e-133">Installera Git och Node.js</span><span class="sxs-lookup"><span data-stu-id="fab7e-133">Install Git and Node.js</span></span>

<span data-ttu-id="fab7e-134">Klicka på följande länkar toodownload hello och installera Git och Node.js LTS för Windows.</span><span class="sxs-lookup"><span data-stu-id="fab7e-134">Click hello following links toodownload and install Git and Node.js LTS for Windows.</span></span>

- [<span data-ttu-id="fab7e-135">Hämta Git för Windows</span><span class="sxs-lookup"><span data-stu-id="fab7e-135">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
- [<span data-ttu-id="fab7e-136">Hämta Node.js LTS för Windows</span><span class="sxs-lookup"><span data-stu-id="fab7e-136">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-nodejs-development-tools"></a><span data-ttu-id="fab7e-137">Installera Node.js utvecklingsverktyg</span><span class="sxs-lookup"><span data-stu-id="fab7e-137">Install Node.js development tools</span></span>

<span data-ttu-id="fab7e-138">Du använder [gulp.js](http://gulpjs.com/) tooautomate distribution och körning av skript.</span><span class="sxs-lookup"><span data-stu-id="fab7e-138">You use [gulp.js](http://gulpjs.com/) tooautomate deployment and execution of scripts.</span></span>

<span data-ttu-id="fab7e-139">Tryck på `Windows + R`, typen `cmd` och tryck på `Enter` tooopen ett kommandotolksfönster och sedan kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="fab7e-139">Press `Windows + R`, type `cmd` and press `Enter` tooopen a Command Prompt window, and then run hello following command:</span></span>

```cmd
npm install -g gulp
```

<span data-ttu-id="fab7e-140">Om du får problem med hello installation finns hello [felsökningsguide för](iot-hub-gateway-kit-c-troubleshooting.md) för lösningar toocommon problem.</span><span class="sxs-lookup"><span data-stu-id="fab7e-140">If you experience issues with hello installation, see hello [troubleshooting guide](iot-hub-gateway-kit-c-troubleshooting.md) for solutions toocommon problems.</span></span>

> [!Note]
> <span data-ttu-id="fab7e-141">Noden, NPM och Gulp är obligatoriska toorun automatiseringsskript utvecklats i Node.js.</span><span class="sxs-lookup"><span data-stu-id="fab7e-141">Node, NPM and Gulp are required toorun automation scripts developed in Node.js.</span></span>

## <a name="install-python"></a><span data-ttu-id="fab7e-142">Installera Python</span><span class="sxs-lookup"><span data-stu-id="fab7e-142">Install Python</span></span>

<span data-ttu-id="fab7e-143">Du kan välja mellan Python 2.7, 3.4 eller 3.5.</span><span class="sxs-lookup"><span data-stu-id="fab7e-143">You can choose from Python 2.7, 3.4 or 3.5.</span></span> <span data-ttu-id="fab7e-144">I den här självstudiekursen kommer använder vi Python 2.7.</span><span class="sxs-lookup"><span data-stu-id="fab7e-144">In this tutorial, we use Python 2.7.</span></span> <span data-ttu-id="fab7e-145">Om du redan har installerat python går toohello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="fab7e-145">If you've already installed python, go toohello next section.</span></span>

[<span data-ttu-id="fab7e-146">Hämta Python för Windows</span><span class="sxs-lookup"><span data-stu-id="fab7e-146">Get Python for Windows</span></span>](https://www.python.org/downloads/)

<span data-ttu-id="fab7e-147">Du måste också tooadd hello sökvägen hello mappar där Python.exe och pip.exe är installerade toohello system `PATH` miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="fab7e-147">You also need tooadd hello path of hello folders where Python.exe and pip.exe are installed toohello system `PATH` environment variable.</span></span> <span data-ttu-id="fab7e-148">Som standard installeras python.exe i `C:\Python27` och pip.exe installeras i `C:\Python27\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="fab7e-148">By default, python.exe is installed in `C:\Python27` and pip.exe is installed in `C:\Python27\Scripts`.</span></span>

## <a name="install-hello-azure-cli"></a><span data-ttu-id="fab7e-149">Installera hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="fab7e-149">Install hello Azure CLI</span></span>

<span data-ttu-id="fab7e-150">tooinstall hello Azure CLI, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="fab7e-150">tooinstall hello Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="fab7e-151">Öppna ett kommandotolksfönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="fab7e-151">Open a Command Prompt window as an administrator.</span></span>

2. <span data-ttu-id="fab7e-152">Installera hello Azure CLI genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="fab7e-152">Install hello Azure CLI by running hello following commands:</span></span>

   ```cmd
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```

   <span data-ttu-id="fab7e-153">hello-installationen kan ta 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="fab7e-153">hello installation might take 5 minutes.</span></span>

3. <span data-ttu-id="fab7e-154">Kontrollera hello installationen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="fab7e-154">Verify hello installation by running hello following command:</span></span>

   ```cmd
   az iot -h
   ```

   <span data-ttu-id="fab7e-155">Du bör se hello följande utdata om hello-installationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="fab7e-155">You should see hello following output if hello installation is successful.</span></span>

   ![Verifiera installation av Azure CLI](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_win.png)

## <a name="install-visual-studio-code"></a><span data-ttu-id="fab7e-157">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fab7e-157">Install Visual Studio Code</span></span>

<span data-ttu-id="fab7e-158">Du kan använda Visual Studio Code senare i hello självstudiekursen tooedit konfigurationsfiler.</span><span class="sxs-lookup"><span data-stu-id="fab7e-158">You use Visual Studio Code later in hello tutorial tooedit configuration files.</span></span>

<span data-ttu-id="fab7e-159">[Hämta](https://code.visualstudio.com/docs/setup/windows) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="fab7e-159">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span>

## <a name="summary"></a><span data-ttu-id="fab7e-160">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="fab7e-160">Summary</span></span>

<span data-ttu-id="fab7e-161">Du har installerat alla hello krävs verktyg och program på värddatorn.</span><span class="sxs-lookup"><span data-stu-id="fab7e-161">You've installed all hello required tools and software on your host computer.</span></span> <span data-ttu-id="fab7e-162">Nästa uppgift är toouse hello Azure CLI toocreate en IoT-hubb och registrera enheten i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="fab7e-162">Your next task is toouse hello Azure CLI toocreate an IoT hub and register your device in your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fab7e-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fab7e-163">Next steps</span></span>
[<span data-ttu-id="fab7e-164">Skapa en IoT-hubb och registrera din enhet</span><span class="sxs-lookup"><span data-stu-id="fab7e-164">Create an IoT hub and register your device</span></span>](iot-hub-gateway-kit-c-lesson2-register-device.md)
