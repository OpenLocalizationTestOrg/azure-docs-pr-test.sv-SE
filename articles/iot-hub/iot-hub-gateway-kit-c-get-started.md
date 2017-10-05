---
title: "SensorTag enhet & Azure IoT-Gateway - komma igång | Microsoft Docs"
description: "Kom igång med startpaket för IoT-Gateway, skapa din Azure IoT-hubb och ansluta SensorTag och Gateway för IoT-hubb"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Azure iot-hubb, iot-gateway, komma igång med internet saker, iot toolkit"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 56d05f4e-f2c1-4b22-8701-f01e14deead6
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 624bdc7877d5048da08897f868272fd8e8f3f7b6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-iot-gateway-starter-kit-with-a-sensortag"></a><span data-ttu-id="90d7a-104">Kom igång med IoT Gateway Startpaket med en SensorTag</span><span class="sxs-lookup"><span data-stu-id="90d7a-104">Get started with IoT Gateway Starter Kit with a SensorTag</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="90d7a-105">SensorTag</span><span class="sxs-lookup"><span data-stu-id="90d7a-105">SensorTag</span></span>](iot-hub-gateway-kit-c-get-started.md)
> * [<span data-ttu-id="90d7a-106">Simulerade enheten</span><span class="sxs-lookup"><span data-stu-id="90d7a-106">Simulated Device</span></span>](iot-hub-gateway-kit-c-sim-get-started.md)

<span data-ttu-id="90d7a-107">I den här självstudiekursen, börjar du med att lära dig grunderna i att arbeta med [IoT Gateway startpaket](https://aka.ms/gateway-kit).</span><span class="sxs-lookup"><span data-stu-id="90d7a-107">In this tutorial, you begin by learning the basics of working with [IoT Gateway Starter Kit](https://aka.ms/gateway-kit).</span></span> <span data-ttu-id="90d7a-108">Du kommer att arbeta med Intel NUC som kör Linux vind floden och [TI SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html#main).</span><span class="sxs-lookup"><span data-stu-id="90d7a-108">You will be working with Intel NUC that's running Wind River Linux and the [TI SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html#main).</span></span> <span data-ttu-id="90d7a-109">Du får lära dig hur seamleesly ansluta enheter till molnet med hjälp av Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="90d7a-109">You will learn how to seamleesly connect your devices to the cloud by using Azure IoT Hub.</span></span>

***
<span data-ttu-id="90d7a-110">**Har ett kit än?:** klickar du på [här](https://aka.ms/gateway-kit).</span><span class="sxs-lookup"><span data-stu-id="90d7a-110">**Don't have a kit yet?:** Click [here](https://aka.ms/gateway-kit).</span></span> <span data-ttu-id="90d7a-111">**Har inte en SensorTag?:** [börja med en simulerad enhet](iot-hub-gateway-kit-c-sim-get-started.md) eller [köpa en SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/?INTC=SensorTag&HQS=sensortag)</span><span class="sxs-lookup"><span data-stu-id="90d7a-111">**Don't have a SensorTag?:** [Start with a simulated device](iot-hub-gateway-kit-c-sim-get-started.md) or [buy a SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/?INTC=SensorTag&HQS=sensortag)</span></span>
***

## <a name="lesson-1-configure-your-nuc"></a><span data-ttu-id="90d7a-112">Lektion 1: Konfigurera din NUC</span><span class="sxs-lookup"><span data-stu-id="90d7a-112">Lesson 1: Configure your NUC</span></span>
![Diagram för Lesson1 slutpunkt till slutpunkt](media/iot-hub-gateway-kit-lessons/e2e-lesson1.png)

<span data-ttu-id="90d7a-114">Installerar Azure IoT kanten på NUC nu bör du ställa in Intel NUC (nästa enhet för datorer) i Kit som en Azure IoT-gateway och kör en exempelapp för att verifiera gateway-funktionerna.</span><span class="sxs-lookup"><span data-stu-id="90d7a-114">In this lesson, you set up Intel NUC (Next Unit of Computing) in the Kit as an Azure IoT gateway, install the Azure IoT Edge package on NUC, and run a sample app to verify the gateway functionality.</span></span>

<span data-ttu-id="90d7a-115">*Uppskattad tidsåtgång: 15 minuter*</span><span class="sxs-lookup"><span data-stu-id="90d7a-115">*Estimated time to complete: 15 minutes*</span></span>

<span data-ttu-id="90d7a-116">Gå till [konfigurera Intel NUC som en IoT-gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)</span><span class="sxs-lookup"><span data-stu-id="90d7a-116">Go to [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)</span></span>

## <a name="lesson-2-create-your-iot-hub"></a><span data-ttu-id="90d7a-117">Lektion 2: Skapa din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="90d7a-117">Lesson 2: Create your IoT Hub</span></span>
![Diagram för Lesson2 slutpunkt till slutpunkt](media/iot-hub-gateway-kit-lessons/e2e-lesson2.png)

<span data-ttu-id="90d7a-119">Nu bör installera du de verktyg och program på värddatorn.</span><span class="sxs-lookup"><span data-stu-id="90d7a-119">In this lesson, you install the tools and software on your host computer.</span></span> <span data-ttu-id="90d7a-120">Sedan du skapar din kostnadsfria Azure-konto, etablera Azure IoT-hubb och skapa din första enhet i IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="90d7a-120">Then you create your free Azure account, provision your Azure IoT hub and create your first device in the IoT hub.</span></span>

<span data-ttu-id="90d7a-121">Slutför lektionen 1 innan du börjar övningen.</span><span class="sxs-lookup"><span data-stu-id="90d7a-121">Complete Lesson 1 before you start this lesson.</span></span>

### <a name="get-the-tools"></a><span data-ttu-id="90d7a-122">Hämta verktygen</span><span class="sxs-lookup"><span data-stu-id="90d7a-122">Get the tools</span></span>
<span data-ttu-id="90d7a-123">Installera de verktyg och program på värddatorn.</span><span class="sxs-lookup"><span data-stu-id="90d7a-123">Install the tools and software on your host computer.</span></span>

<span data-ttu-id="90d7a-124">*Uppskattad tidsåtgång: 20 minuter*</span><span class="sxs-lookup"><span data-stu-id="90d7a-124">*Estimated time to complete: 20 minutes*</span></span>

<span data-ttu-id="90d7a-125">Gå till [skaffa verktygen](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)</span><span class="sxs-lookup"><span data-stu-id="90d7a-125">Go to [Get the tools](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)</span></span>

### <a name="create-an-iot-hub-and-register-your-device"></a><span data-ttu-id="90d7a-126">Skapa en IoT-hubb och registrera din enhet</span><span class="sxs-lookup"><span data-stu-id="90d7a-126">Create an IoT hub and register your device</span></span>
<span data-ttu-id="90d7a-127">Skapa en resursgrupp, etablera din första Azure IoT-hubb och Lägg till din första enhet IoT-hubben med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="90d7a-127">Create your resource group, provision your first Azure IoT hub, and add your first device to the IoT hub using the Azure CLI.</span></span>

<span data-ttu-id="90d7a-128">*Uppskattad tidsåtgång: 10 minuter*</span><span class="sxs-lookup"><span data-stu-id="90d7a-128">*Estimated time to complete: 10 minutes*</span></span>

<span data-ttu-id="90d7a-129">Gå till [skapar en IoT-hubb och registrerar din enhet](iot-hub-gateway-kit-c-lesson2-register-device.md)</span><span class="sxs-lookup"><span data-stu-id="90d7a-129">Go to [Create an IoT hub and register your device](iot-hub-gateway-kit-c-lesson2-register-device.md)</span></span>

## <a name="lesson-3-receive-messages-from-sensortag-and-read-messages-from-your-iot-hub"></a><span data-ttu-id="90d7a-130">Lektionen 3: Ta emot meddelanden från SensorTag och läsa meddelanden från din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="90d7a-130">Lesson 3: Receive messages from SensorTag and read messages from your IoT hub</span></span>
<span data-ttu-id="90d7a-131">I kursen ska du använda skript för att automatisera konfigurationen och körningen av ett exempelprogram TIVERA i din gateway.</span><span class="sxs-lookup"><span data-stu-id="90d7a-131">In this lesson, you will use scripts to automate the configuration and execution of a BLE sample application in your gateway.</span></span> <span data-ttu-id="90d7a-132">Sådana program använder en samling av moduler för aggregering och transformera data, bearbeta kommandon eller utföra ett antal olika relaterade uppgifter.</span><span class="sxs-lookup"><span data-stu-id="90d7a-132">Such applications use a collection of modules to aggregate and transform data, process commands, or perform any number of related tasks.</span></span> <span data-ttu-id="90d7a-133">Moduler som kommunicerar med varandra via en koordinator för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="90d7a-133">Modules communicate with one another via a message broker.</span></span> <span data-ttu-id="90d7a-134">Exempelprogrammet har en tabell modul och en IoT-hubb-modul.</span><span class="sxs-lookup"><span data-stu-id="90d7a-134">The sample application has a BLE module and an IoT hub module.</span></span> <span data-ttu-id="90d7a-135">Modulen TIVERA tar emot data från TIVERA SensorTag.</span><span class="sxs-lookup"><span data-stu-id="90d7a-135">The BLE module receives data from BLE SensorTag.</span></span> <span data-ttu-id="90d7a-136">Modulen IoT-hubb paket mottagna data och skickar den till din IoT-hubb via gateway-ramverk i Azure IoT kant.</span><span class="sxs-lookup"><span data-stu-id="90d7a-136">The IoT hub module packages the data received and sends it to your IoT hub through the gateway framework provided in Azure IoT Edge.</span></span>

![Diagram över lektionen 3-slutpunkt till slutpunkt](media/iot-hub-gateway-kit-lessons/e2e-lesson3.png)

### <a name="configure-and-run-the-ble-sample-app"></a><span data-ttu-id="90d7a-138">Konfigurera och köra TIVERA sample-appen</span><span class="sxs-lookup"><span data-stu-id="90d7a-138">Configure and run the BLE sample app</span></span>
<span data-ttu-id="90d7a-139">Konfigurera anslutningen mellan SensorTag och din gateway.</span><span class="sxs-lookup"><span data-stu-id="90d7a-139">Set up the connectivity between SensorTag and your gateway.</span></span> <span data-ttu-id="90d7a-140">Sedan slutföra konfigurationen och köra exempelprogrammet tabell.</span><span class="sxs-lookup"><span data-stu-id="90d7a-140">Then finish the configuration and run the BLE sample application.</span></span>

<span data-ttu-id="90d7a-141">*Uppskattad tidsåtgång: 15 minuter*</span><span class="sxs-lookup"><span data-stu-id="90d7a-141">*Estimated time to complete: 15 minutes*</span></span>

<span data-ttu-id="90d7a-142">Gå till [konfigurera och köra TIVERA sample-appen](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)</span><span class="sxs-lookup"><span data-stu-id="90d7a-142">Go to [Configure and run the BLE sample app](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)</span></span>

### <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="90d7a-143">Läsa meddelanden från din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="90d7a-143">Read messages from your IoT hub</span></span>
<span data-ttu-id="90d7a-144">Köra exempelkod på värddatorn för att läsa meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="90d7a-144">Run sample code on your host computer to read messages from your IoT hub.</span></span>

<span data-ttu-id="90d7a-145">*Uppskattad tidsåtgång: 15 minuter*</span><span class="sxs-lookup"><span data-stu-id="90d7a-145">*Estimated time to complete: 15 minutes*</span></span>

<span data-ttu-id="90d7a-146">Gå till [läsa meddelanden från din IoT-hubb](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)</span><span class="sxs-lookup"><span data-stu-id="90d7a-146">Go to [Read messages from your IoT hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)</span></span>

## <a name="lesson-4-save-messages-to-azure-table-storage"></a><span data-ttu-id="90d7a-147">Lektion 4: Spara meddelanden i Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="90d7a-147">Lesson 4: Save messages to Azure Table storage</span></span>
<span data-ttu-id="90d7a-148">Skapa en funktionsapp i Azure-som hämtar inkommande meddelanden från din IoT-hubb och skriver dem till Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="90d7a-148">Create an Azure function app that gets incoming messages from your IoT hub and writes them to Azure Table storage.</span></span>

![Lektionen 4 slutpunkt till slutpunkt-diagram](media/iot-hub-gateway-kit-lessons/e2e-lesson4.png)

### <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="90d7a-150">Skapa en Azure funktionsapp och Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="90d7a-150">Create an Azure function app and Azure Storage account</span></span>
<span data-ttu-id="90d7a-151">Använd en mall för Azure Resource Manager för att skapa en funktionsapp i Azure-och ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="90d7a-151">Use an Azure Resource Manager template to create an Azure function app and an Azure Storage account.</span></span>

<span data-ttu-id="90d7a-152">*Uppskattad tidsåtgång: 10 minuter*</span><span class="sxs-lookup"><span data-stu-id="90d7a-152">*Estimated time to complete: 10 minutes*</span></span>

<span data-ttu-id="90d7a-153">Gå till [skapa en Azure funktionsapp och Azure Storage-konto](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)</span><span class="sxs-lookup"><span data-stu-id="90d7a-153">Go to [Create an Azure function app and Azure Storage account](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)</span></span>

### <a name="read-messages-persisted-in-azure-table-storage"></a><span data-ttu-id="90d7a-154">Läs meddelandena kvar i Azure Table storage</span><span class="sxs-lookup"><span data-stu-id="90d7a-154">Read messages persisted in Azure Table storage</span></span>
<span data-ttu-id="90d7a-155">Övervaka gateway-to-cloud-meddelanden som de skrivs till Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="90d7a-155">Monitor the gateway-to-cloud messages as they are written to Azure Table storage.</span></span>

<span data-ttu-id="90d7a-156">*Uppskattad tidsåtgång: 5 minuter*</span><span class="sxs-lookup"><span data-stu-id="90d7a-156">*Estimated time to complete: 5 minutes*</span></span>

<span data-ttu-id="90d7a-157">Gå till [lästa meddelanden kvar i Azure Table storage](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).</span><span class="sxs-lookup"><span data-stu-id="90d7a-157">Go to [Read messages persisted in Azure Table storage](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="90d7a-158">Felsökning</span><span class="sxs-lookup"><span data-stu-id="90d7a-158">Troubleshooting</span></span>
<span data-ttu-id="90d7a-159">Om det uppstår problem under erfarenheter söka efter lösningar i den [felsökning](iot-hub-gateway-kit-c-troubleshooting.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="90d7a-159">If you have any problems during the lessons, look for solutions in the [Troubleshooting](iot-hub-gateway-kit-c-troubleshooting.md) article.</span></span>

## <a name="explore-more"></a><span data-ttu-id="90d7a-160">Utforska mer</span><span class="sxs-lookup"><span data-stu-id="90d7a-160">Explore more</span></span>
<span data-ttu-id="90d7a-161">Besök den [Intel IoT Gateway Kit developer zonen](http://software.intel.com/iot/microsoft-azure) vill veta mer.</span><span class="sxs-lookup"><span data-stu-id="90d7a-161">Visit the [Intel IoT Gateway Kit developer zone](http://software.intel.com/iot/microsoft-azure) to learn more.</span></span>