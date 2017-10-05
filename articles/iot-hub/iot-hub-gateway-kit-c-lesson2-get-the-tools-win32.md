---
title: "SensorTag enhet & Azure IoT-Gateway - lektion 2: Hämta verktyg (Windows) | Microsoft Docs"
description: "Installera verktygen och programvaran på din värddator som kör Windows, skapar en IoT-hubb och registrera enheten i IoT-hubben."
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
ms.openlocfilehash: 0d8ba03df63d0b8657a9e275fc636e806c66b683
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-tools-windows-7-and-later"></a><span data-ttu-id="37113-104">Skaffa dig verktyg (Windows 7 och senare)</span><span class="sxs-lookup"><span data-stu-id="37113-104">Get the tools (Windows 7 and later)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="37113-105">Windows 7 eller senare</span><span class="sxs-lookup"><span data-stu-id="37113-105">Windows 7 or later</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
> * [<span data-ttu-id="37113-106">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="37113-106">Ubuntu 16.04</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-ubuntu.md)
> * [<span data-ttu-id="37113-107">macOS 10.10</span><span class="sxs-lookup"><span data-stu-id="37113-107">macOS 10.10</span></span>](iot-hub-gateway-kit-c-lesson2-get-the-tools-mac.md)

## <a name="what-you-will-do"></a><span data-ttu-id="37113-108">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="37113-108">What you will do</span></span>

- <span data-ttu-id="37113-109">Installera Git, Node.js, Gulp, Python.</span><span class="sxs-lookup"><span data-stu-id="37113-109">Install Git, Node.js, Gulp, Python.</span></span>
- <span data-ttu-id="37113-110">Installera Azure-kommandoradsgränssnittet (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="37113-110">Install the Azure command-line interface (Azure CLI).</span></span> 

<span data-ttu-id="37113-111">Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="37113-111">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="37113-112">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="37113-112">What you will learn</span></span>

<span data-ttu-id="37113-113">I den här lektionen får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="37113-113">In this lesson, you will learn:</span></span>

- <span data-ttu-id="37113-114">Så här installerar du [Git](https://git-scm.com/) och [Node.js](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="37113-114">How to install [Git](https://git-scm.com/) and [Node.js](https://nodejs.org/en/).</span></span>
  - <span data-ttu-id="37113-115">Git är ett system för öppen källkod distribuerade versionen.</span><span class="sxs-lookup"><span data-stu-id="37113-115">Git is an open source distributed version control system.</span></span> <span data-ttu-id="37113-116">Exempelprogram för den här lektionen lagras på Git.</span><span class="sxs-lookup"><span data-stu-id="37113-116">The sample application for this lesson is stored on Git.</span></span>
  - <span data-ttu-id="37113-117">Node.js är JavaScript-körning med ett omfattande paketet ekosystem.</span><span class="sxs-lookup"><span data-stu-id="37113-117">Node.js is a JavaScript runtime with a rich package ecosystem.</span></span>
- <span data-ttu-id="37113-118">Hur du använder [NPM](https://www.npmjs.com/) installera Node.js utvecklingsverktyg.</span><span class="sxs-lookup"><span data-stu-id="37113-118">How to use [NPM](https://www.npmjs.com/) to install Node.js development tools.</span></span>
  - <span data-ttu-id="37113-119">Versionen som krävs för Node.js är 4.5 LTS.</span><span class="sxs-lookup"><span data-stu-id="37113-119">The minimum required version of Node.js is 4.5 LTS.</span></span>
  - <span data-ttu-id="37113-120">NPM är en av de paket cheferna för Node.js.</span><span class="sxs-lookup"><span data-stu-id="37113-120">NPM is one of the package managers for Node.js.</span></span>
- <span data-ttu-id="37113-121">Så här installerar Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="37113-121">How to install Visual Studio Code.</span></span>
  - <span data-ttu-id="37113-122">Visual Studio-koden är plattformsoberoende redigerare för enkel men kraftfull källa för Windows, Linux och macOS.</span><span class="sxs-lookup"><span data-stu-id="37113-122">Visual Studio Code is a cross platform, lightweight but powerful source code editor for Windows, Linux, and macOS.</span></span> <span data-ttu-id="37113-123">Det har bra stöd för felsökning, inbäddad Git-kontroll, syntaxmarkering, intelligent kod slutförande, kodavsnitt och koden eftersom samt.</span><span class="sxs-lookup"><span data-stu-id="37113-123">It has great support for debugging, embedded Git control, syntax highlighting, intelligent code completion, snippets, and code refactoring as well.</span></span>
- <span data-ttu-id="37113-124">Hur du installerar Python.</span><span class="sxs-lookup"><span data-stu-id="37113-124">How to install Python.</span></span>
  - <span data-ttu-id="37113-125">Python är en term som används på hög nivå, allmänna, tolkad och dynamiska programmeringsspråk.</span><span class="sxs-lookup"><span data-stu-id="37113-125">Python is a widely used high-level, general-purpose, interpreted and dynamic programming language.</span></span>
- <span data-ttu-id="37113-126">Så här installerar du Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="37113-126">How to install the Azure CLI.</span></span>
  - <span data-ttu-id="37113-127">Azure CLI tillhandahåller en flera plattformar kommandoraden för Azure.</span><span class="sxs-lookup"><span data-stu-id="37113-127">The Azure CLI provides a multiplatform command-line experience for Azure.</span></span> <span data-ttu-id="37113-128">Du arbetar direkt från en kommandorad för att etablera och hantera resurser.</span><span class="sxs-lookup"><span data-stu-id="37113-128">You work directly from a command line to provision and manage resources.</span></span>
- <span data-ttu-id="37113-129">Hur du använder Azure CLI för att skapa en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="37113-129">How to use the Azure CLI to create an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="37113-130">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="37113-130">What you need</span></span>

- <span data-ttu-id="37113-131">En Internet-anslutning att hämta verktyg och program.</span><span class="sxs-lookup"><span data-stu-id="37113-131">An Internet connection to download the tools and software.</span></span>
- <span data-ttu-id="37113-132">En Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="37113-132">A Windows computer.</span></span>

## <a name="install-git-and-nodejs"></a><span data-ttu-id="37113-133">Installera Git och Node.js</span><span class="sxs-lookup"><span data-stu-id="37113-133">Install Git and Node.js</span></span>

<span data-ttu-id="37113-134">Klicka på länken nedan om du vill hämta och installera Git och Node.js LTS för Windows.</span><span class="sxs-lookup"><span data-stu-id="37113-134">Click the following links to download and install Git and Node.js LTS for Windows.</span></span>

- [<span data-ttu-id="37113-135">Hämta Git för Windows</span><span class="sxs-lookup"><span data-stu-id="37113-135">Get Git for Windows</span></span>](https://git-scm.com/download/win/)
- [<span data-ttu-id="37113-136">Hämta Node.js LTS för Windows</span><span class="sxs-lookup"><span data-stu-id="37113-136">Get Node.js LTS for Windows</span></span>](https://nodejs.org/en/)

## <a name="install-nodejs-development-tools"></a><span data-ttu-id="37113-137">Installera Node.js utvecklingsverktyg</span><span class="sxs-lookup"><span data-stu-id="37113-137">Install Node.js development tools</span></span>

<span data-ttu-id="37113-138">Du använder [gulp.js](http://gulpjs.com/) att automatisera distribution och körning av skript.</span><span class="sxs-lookup"><span data-stu-id="37113-138">You use [gulp.js](http://gulpjs.com/) to automate deployment and execution of scripts.</span></span>

<span data-ttu-id="37113-139">Tryck på `Windows + R`, typen `cmd` och tryck på `Enter` att öppna en kommandotolk och kör sedan följande kommando:</span><span class="sxs-lookup"><span data-stu-id="37113-139">Press `Windows + R`, type `cmd` and press `Enter` to open a Command Prompt window, and then run the following command:</span></span>

```cmd
npm install -g gulp
```

<span data-ttu-id="37113-140">Om du får problem med installationen finns i [felsökningsguide för](iot-hub-gateway-kit-c-troubleshooting.md) efter lösningar på vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="37113-140">If you experience issues with the installation, see the [troubleshooting guide](iot-hub-gateway-kit-c-troubleshooting.md) for solutions to common problems.</span></span>

> [!Note]
> <span data-ttu-id="37113-141">Noden, NPM och Gulp krävs för att köra automatiserade skript som utvecklats i Node.js.</span><span class="sxs-lookup"><span data-stu-id="37113-141">Node, NPM and Gulp are required to run automation scripts developed in Node.js.</span></span>

## <a name="install-python"></a><span data-ttu-id="37113-142">Installera Python</span><span class="sxs-lookup"><span data-stu-id="37113-142">Install Python</span></span>

<span data-ttu-id="37113-143">Du kan välja mellan Python 2.7, 3.4 eller 3.5.</span><span class="sxs-lookup"><span data-stu-id="37113-143">You can choose from Python 2.7, 3.4 or 3.5.</span></span> <span data-ttu-id="37113-144">I den här självstudiekursen kommer använder vi Python 2.7.</span><span class="sxs-lookup"><span data-stu-id="37113-144">In this tutorial, we use Python 2.7.</span></span> <span data-ttu-id="37113-145">Om du redan har installerat python, gå till nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="37113-145">If you've already installed python, go to the next section.</span></span>

[<span data-ttu-id="37113-146">Hämta Python för Windows</span><span class="sxs-lookup"><span data-stu-id="37113-146">Get Python for Windows</span></span>](https://www.python.org/downloads/)

<span data-ttu-id="37113-147">Du måste också lägga till sökvägen till de mappar där Python.exe och pip.exe är installerade i systemet `PATH` miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="37113-147">You also need to add the path of the folders where Python.exe and pip.exe are installed to the system `PATH` environment variable.</span></span> <span data-ttu-id="37113-148">Som standard installeras python.exe i `C:\Python27` och pip.exe installeras i `C:\Python27\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="37113-148">By default, python.exe is installed in `C:\Python27` and pip.exe is installed in `C:\Python27\Scripts`.</span></span>

## <a name="install-the-azure-cli"></a><span data-ttu-id="37113-149">Installera Azure CLI</span><span class="sxs-lookup"><span data-stu-id="37113-149">Install the Azure CLI</span></span>

<span data-ttu-id="37113-150">Så här installerar du Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="37113-150">To install the Azure CLI, follow these steps:</span></span>

1. <span data-ttu-id="37113-151">Öppna ett kommandotolksfönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="37113-151">Open a Command Prompt window as an administrator.</span></span>

2. <span data-ttu-id="37113-152">Installera Azure CLI genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="37113-152">Install the Azure CLI by running the following commands:</span></span>

   ```cmd
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```

   <span data-ttu-id="37113-153">Installationen kan ta 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="37113-153">The installation might take 5 minutes.</span></span>

3. <span data-ttu-id="37113-154">Verifiera installationen genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="37113-154">Verify the installation by running the following command:</span></span>

   ```cmd
   az iot -h
   ```

   <span data-ttu-id="37113-155">Du bör se följande utdata om installationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="37113-155">You should see the following output if the installation is successful.</span></span>

   ![Verifiera installation av Azure CLI](media/iot-hub-gateway-kit-lessons/lesson2/az_iot_help_win.png)

## <a name="install-visual-studio-code"></a><span data-ttu-id="37113-157">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="37113-157">Install Visual Studio Code</span></span>

<span data-ttu-id="37113-158">Du kan använda Visual Studio Code senare under kursen för att redigera konfigurationsfiler.</span><span class="sxs-lookup"><span data-stu-id="37113-158">You use Visual Studio Code later in the tutorial to edit configuration files.</span></span>

<span data-ttu-id="37113-159">[Hämta](https://code.visualstudio.com/docs/setup/windows) och installera Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="37113-159">[Download](https://code.visualstudio.com/docs/setup/windows) and install Visual Studio Code.</span></span>

## <a name="summary"></a><span data-ttu-id="37113-160">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="37113-160">Summary</span></span>

<span data-ttu-id="37113-161">Du har installerat alla verktyg som krävs och programvara på värddatorn.</span><span class="sxs-lookup"><span data-stu-id="37113-161">You've installed all the required tools and software on your host computer.</span></span> <span data-ttu-id="37113-162">Nästa uppgift är att använda Azure CLI för att skapa en IoT-hubb och registrera enheten i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="37113-162">Your next task is to use the Azure CLI to create an IoT hub and register your device in your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="37113-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="37113-163">Next steps</span></span>
[<span data-ttu-id="37113-164">Skapa en IoT-hubb och registrera din enhet</span><span class="sxs-lookup"><span data-stu-id="37113-164">Create an IoT hub and register your device</span></span>](iot-hub-gateway-kit-c-lesson2-register-device.md)
