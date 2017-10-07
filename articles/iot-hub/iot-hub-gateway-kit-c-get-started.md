---
title: "SensorTag enhet & Azure IoT-Gateway - komma igång | Microsoft Docs"
description: "Kom igång med startpaket för IoT-Gateway, skapa din Azure IoT-hubb och ansluta SensorTag och Gateway toohello IoT-hubb"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Azure iot-hubb, iot-gateway, komma igång med hello internet saker, iot toolkit"
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
ms.openlocfilehash: d8e4057e7774e43c069dd3f2f2e03f098c1ac844
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-iot-gateway-starter-kit-with-a-sensortag"></a><span data-ttu-id="cefdf-104">Kom igång med IoT Gateway Startpaket med en SensorTag</span><span class="sxs-lookup"><span data-stu-id="cefdf-104">Get started with IoT Gateway Starter Kit with a SensorTag</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="cefdf-105">SensorTag</span><span class="sxs-lookup"><span data-stu-id="cefdf-105">SensorTag</span></span>](iot-hub-gateway-kit-c-get-started.md)
> * [<span data-ttu-id="cefdf-106">Simulerade enheten</span><span class="sxs-lookup"><span data-stu-id="cefdf-106">Simulated Device</span></span>](iot-hub-gateway-kit-c-sim-get-started.md)

<span data-ttu-id="cefdf-107">I den här självstudiekursen börjar du med att lära sig hello grunderna i att arbeta med [IoT Gateway startpaket](https://aka.ms/gateway-kit).</span><span class="sxs-lookup"><span data-stu-id="cefdf-107">In this tutorial, you begin by learning hello basics of working with [IoT Gateway Starter Kit](https://aka.ms/gateway-kit).</span></span> <span data-ttu-id="cefdf-108">Du kommer att arbeta med Intel NUC som kör Linux vind floden och hello [TI SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html#main).</span><span class="sxs-lookup"><span data-stu-id="cefdf-108">You will be working with Intel NUC that's running Wind River Linux and hello [TI SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html#main).</span></span> <span data-ttu-id="cefdf-109">Får du lära dig hur tooseamleesly ansluta enheter toohello molnet med hjälp av Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="cefdf-109">You will learn how tooseamleesly connect your devices toohello cloud by using Azure IoT Hub.</span></span>

***
<span data-ttu-id="cefdf-110">**Har ett kit än?:** klickar du på [här](https://aka.ms/gateway-kit).</span><span class="sxs-lookup"><span data-stu-id="cefdf-110">**Don't have a kit yet?:** Click [here](https://aka.ms/gateway-kit).</span></span> <span data-ttu-id="cefdf-111">**Har inte en SensorTag?:** [börja med en simulerad enhet](iot-hub-gateway-kit-c-sim-get-started.md) eller [köpa en SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/?INTC=SensorTag&HQS=sensortag)</span><span class="sxs-lookup"><span data-stu-id="cefdf-111">**Don't have a SensorTag?:** [Start with a simulated device](iot-hub-gateway-kit-c-sim-get-started.md) or [buy a SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/?INTC=SensorTag&HQS=sensortag)</span></span>
***

## <a name="lesson-1-configure-your-nuc"></a><span data-ttu-id="cefdf-112">Lektion 1: Konfigurera din NUC</span><span class="sxs-lookup"><span data-stu-id="cefdf-112">Lesson 1: Configure your NUC</span></span>
![Diagram för Lesson1 slutpunkt till slutpunkt](media/iot-hub-gateway-kit-lessons/e2e-lesson1.png)

<span data-ttu-id="cefdf-114">Installerar hello Azure IoT kanten på NUC nu bör du ställa in Intel NUC (nästa enhet för datorer) i hello Kit som en Azure IoT-gateway och kör ett exempel app tooverify hello gateway-funktioner.</span><span class="sxs-lookup"><span data-stu-id="cefdf-114">In this lesson, you set up Intel NUC (Next Unit of Computing) in hello Kit as an Azure IoT gateway, install hello Azure IoT Edge package on NUC, and run a sample app tooverify hello gateway functionality.</span></span>

<span data-ttu-id="cefdf-115">*Uppskattad tid toocomplete: 15 minuter*</span><span class="sxs-lookup"><span data-stu-id="cefdf-115">*Estimated time toocomplete: 15 minutes*</span></span>

<span data-ttu-id="cefdf-116">Gå för[konfigurera Intel NUC som en IoT-gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)</span><span class="sxs-lookup"><span data-stu-id="cefdf-116">Go too[Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)</span></span>

## <a name="lesson-2-create-your-iot-hub"></a><span data-ttu-id="cefdf-117">Lektion 2: Skapa din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="cefdf-117">Lesson 2: Create your IoT Hub</span></span>
![Diagram för Lesson2 slutpunkt till slutpunkt](media/iot-hub-gateway-kit-lessons/e2e-lesson2.png)

<span data-ttu-id="cefdf-119">Nu bör installera du hello verktyg och program på värddatorn.</span><span class="sxs-lookup"><span data-stu-id="cefdf-119">In this lesson, you install hello tools and software on your host computer.</span></span> <span data-ttu-id="cefdf-120">Sedan du skapar din kostnadsfria Azure-konto, etablera Azure IoT-hubb och skapa din första enhet i hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="cefdf-120">Then you create your free Azure account, provision your Azure IoT hub and create your first device in hello IoT hub.</span></span>

<span data-ttu-id="cefdf-121">Slutför lektionen 1 innan du börjar övningen.</span><span class="sxs-lookup"><span data-stu-id="cefdf-121">Complete Lesson 1 before you start this lesson.</span></span>

### <a name="get-hello-tools"></a><span data-ttu-id="cefdf-122">Hämta hello-verktyg</span><span class="sxs-lookup"><span data-stu-id="cefdf-122">Get hello tools</span></span>
<span data-ttu-id="cefdf-123">Installera hello verktyg och programvara på värddatorn.</span><span class="sxs-lookup"><span data-stu-id="cefdf-123">Install hello tools and software on your host computer.</span></span>

<span data-ttu-id="cefdf-124">*Uppskattad tid toocomplete: 20 minuter*</span><span class="sxs-lookup"><span data-stu-id="cefdf-124">*Estimated time toocomplete: 20 minutes*</span></span>

<span data-ttu-id="cefdf-125">Gå för[hämta hello-verktyg](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)</span><span class="sxs-lookup"><span data-stu-id="cefdf-125">Go too[Get hello tools](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)</span></span>

### <a name="create-an-iot-hub-and-register-your-device"></a><span data-ttu-id="cefdf-126">Skapa en IoT-hubb och registrera din enhet</span><span class="sxs-lookup"><span data-stu-id="cefdf-126">Create an IoT hub and register your device</span></span>
<span data-ttu-id="cefdf-127">Skapa en resursgrupp, etablera din första Azure IoT-hubb och Lägg till din första enhet toohello IoT-hubb med hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="cefdf-127">Create your resource group, provision your first Azure IoT hub, and add your first device toohello IoT hub using hello Azure CLI.</span></span>

<span data-ttu-id="cefdf-128">*Uppskattad tid toocomplete: 10 minuter*</span><span class="sxs-lookup"><span data-stu-id="cefdf-128">*Estimated time toocomplete: 10 minutes*</span></span>

<span data-ttu-id="cefdf-129">Gå för[skapar en IoT-hubb och registrerar din enhet](iot-hub-gateway-kit-c-lesson2-register-device.md)</span><span class="sxs-lookup"><span data-stu-id="cefdf-129">Go too[Create an IoT hub and register your device](iot-hub-gateway-kit-c-lesson2-register-device.md)</span></span>

## <a name="lesson-3-receive-messages-from-sensortag-and-read-messages-from-your-iot-hub"></a><span data-ttu-id="cefdf-130">Lektionen 3: Ta emot meddelanden från SensorTag och läsa meddelanden från din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="cefdf-130">Lesson 3: Receive messages from SensorTag and read messages from your IoT hub</span></span>
<span data-ttu-id="cefdf-131">Nu bör ska du använda skript tooautomate hello konfiguration och körningen av ett exempelprogram TIVERA i din gateway.</span><span class="sxs-lookup"><span data-stu-id="cefdf-131">In this lesson, you will use scripts tooautomate hello configuration and execution of a BLE sample application in your gateway.</span></span> <span data-ttu-id="cefdf-132">Sådana program använder en samling moduler tooaggregate och transformera data, bearbeta kommandon eller utföra ett antal olika relaterade uppgifter.</span><span class="sxs-lookup"><span data-stu-id="cefdf-132">Such applications use a collection of modules tooaggregate and transform data, process commands, or perform any number of related tasks.</span></span> <span data-ttu-id="cefdf-133">Moduler som kommunicerar med varandra via en koordinator för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="cefdf-133">Modules communicate with one another via a message broker.</span></span> <span data-ttu-id="cefdf-134">hello exempelprogrammet har en tabell modul och en IoT-hubb-modul.</span><span class="sxs-lookup"><span data-stu-id="cefdf-134">hello sample application has a BLE module and an IoT hub module.</span></span> <span data-ttu-id="cefdf-135">hello TIVERA modulen tar emot data från TIVERA SensorTag.</span><span class="sxs-lookup"><span data-stu-id="cefdf-135">hello BLE module receives data from BLE SensorTag.</span></span> <span data-ttu-id="cefdf-136">Hej IoT-hubb modulen paket hello data tas emot och skickar den tooyour IoT-hubb via hello gateway ramverk i Azure IoT kant.</span><span class="sxs-lookup"><span data-stu-id="cefdf-136">hello IoT hub module packages hello data received and sends it tooyour IoT hub through hello gateway framework provided in Azure IoT Edge.</span></span>

![Diagram över lektionen 3-slutpunkt till slutpunkt](media/iot-hub-gateway-kit-lessons/e2e-lesson3.png)

### <a name="configure-and-run-hello-ble-sample-app"></a><span data-ttu-id="cefdf-138">Konfigurera och köra hello TIVERA sample-appen</span><span class="sxs-lookup"><span data-stu-id="cefdf-138">Configure and run hello BLE sample app</span></span>
<span data-ttu-id="cefdf-139">Ställ in hello anslutningen mellan SensorTag och din gateway.</span><span class="sxs-lookup"><span data-stu-id="cefdf-139">Set up hello connectivity between SensorTag and your gateway.</span></span> <span data-ttu-id="cefdf-140">Sedan slutföra hello konfigurationen och köra hello TIVERA exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="cefdf-140">Then finish hello configuration and run hello BLE sample application.</span></span>

<span data-ttu-id="cefdf-141">*Uppskattad tid toocomplete: 15 minuter*</span><span class="sxs-lookup"><span data-stu-id="cefdf-141">*Estimated time toocomplete: 15 minutes*</span></span>

<span data-ttu-id="cefdf-142">Gå för[konfigurera och köra hello TIVERA sample-appen](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)</span><span class="sxs-lookup"><span data-stu-id="cefdf-142">Go too[Configure and run hello BLE sample app](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)</span></span>

### <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="cefdf-143">Läsa meddelanden från din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="cefdf-143">Read messages from your IoT hub</span></span>
<span data-ttu-id="cefdf-144">Köra exempelkod på din värd datorn tooread meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="cefdf-144">Run sample code on your host computer tooread messages from your IoT hub.</span></span>

<span data-ttu-id="cefdf-145">*Uppskattad tid toocomplete: 15 minuter*</span><span class="sxs-lookup"><span data-stu-id="cefdf-145">*Estimated time toocomplete: 15 minutes*</span></span>

<span data-ttu-id="cefdf-146">Gå för[läsa meddelanden från din IoT-hubb](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)</span><span class="sxs-lookup"><span data-stu-id="cefdf-146">Go too[Read messages from your IoT hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)</span></span>

## <a name="lesson-4-save-messages-tooazure-table-storage"></a><span data-ttu-id="cefdf-147">Lektionen 4: Spara meddelanden tooAzure Table storage</span><span class="sxs-lookup"><span data-stu-id="cefdf-147">Lesson 4: Save messages tooAzure Table storage</span></span>
<span data-ttu-id="cefdf-148">Skapa en funktionsapp i Azure-som hämtar inkommande meddelanden från din IoT-hubb och skriver dem tooAzure Table storage.</span><span class="sxs-lookup"><span data-stu-id="cefdf-148">Create an Azure function app that gets incoming messages from your IoT hub and writes them tooAzure Table storage.</span></span>

![Lektionen 4 slutpunkt till slutpunkt-diagram](media/iot-hub-gateway-kit-lessons/e2e-lesson4.png)

### <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="cefdf-150">Skapa en Azure funktionsapp och Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="cefdf-150">Create an Azure function app and Azure Storage account</span></span>
<span data-ttu-id="cefdf-151">Använda en Azure Resource Manager-mall toocreate en funktionsapp i Azure-och ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="cefdf-151">Use an Azure Resource Manager template toocreate an Azure function app and an Azure Storage account.</span></span>

<span data-ttu-id="cefdf-152">*Uppskattad tid toocomplete: 10 minuter*</span><span class="sxs-lookup"><span data-stu-id="cefdf-152">*Estimated time toocomplete: 10 minutes*</span></span>

<span data-ttu-id="cefdf-153">Gå för[skapa en Azure funktionsapp och Azure Storage-konto](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)</span><span class="sxs-lookup"><span data-stu-id="cefdf-153">Go too[Create an Azure function app and Azure Storage account](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)</span></span>

### <a name="read-messages-persisted-in-azure-table-storage"></a><span data-ttu-id="cefdf-154">Läs meddelandena kvar i Azure Table storage</span><span class="sxs-lookup"><span data-stu-id="cefdf-154">Read messages persisted in Azure Table storage</span></span>
<span data-ttu-id="cefdf-155">Övervaka hälsningsmeddelande för gateway-to-cloud medan de skrivs tooAzure Table storage.</span><span class="sxs-lookup"><span data-stu-id="cefdf-155">Monitor hello gateway-to-cloud messages as they are written tooAzure Table storage.</span></span>

<span data-ttu-id="cefdf-156">*Uppskattad tid toocomplete: 5 minuter*</span><span class="sxs-lookup"><span data-stu-id="cefdf-156">*Estimated time toocomplete: 5 minutes*</span></span>

<span data-ttu-id="cefdf-157">Gå för[lästa meddelanden kvar i Azure Table storage](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).</span><span class="sxs-lookup"><span data-stu-id="cefdf-157">Go too[Read messages persisted in Azure Table storage](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="cefdf-158">Felsökning</span><span class="sxs-lookup"><span data-stu-id="cefdf-158">Troubleshooting</span></span>
<span data-ttu-id="cefdf-159">Om det uppstår problem under hello erfarenheter söka efter lösningar i hello [felsökning](iot-hub-gateway-kit-c-troubleshooting.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="cefdf-159">If you have any problems during hello lessons, look for solutions in hello [Troubleshooting](iot-hub-gateway-kit-c-troubleshooting.md) article.</span></span>

## <a name="explore-more"></a><span data-ttu-id="cefdf-160">Utforska mer</span><span class="sxs-lookup"><span data-stu-id="cefdf-160">Explore more</span></span>
<span data-ttu-id="cefdf-161">Besök hello [Intel IoT Gateway Kit developer zonen](http://software.intel.com/iot/microsoft-azure) toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="cefdf-161">Visit hello [Intel IoT Gateway Kit developer zone](http://software.intel.com/iot/microsoft-azure) toolearn more.</span></span>
