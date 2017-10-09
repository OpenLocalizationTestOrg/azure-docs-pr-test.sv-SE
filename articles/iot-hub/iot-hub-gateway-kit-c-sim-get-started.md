---
title: "Simulerade enhet & Azure IoT-Gateway - komma igång | Microsoft Docs"
description: "Kom igång med startpaket för IoT-Gateway, skapa din Azure IoT-hubb och Anslut Gateway toohello IoT-hubb"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Azure iot-hubb, iot-gateway, komma igång med hello internet saker, iot toolkit"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 0c110b8b-bee4-4aec-a18a-dfc292aa17a3
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 1a54a5e5f1c1d9b2e657c9e4448274256e2533f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-iot-gateway-starter-kit-with-a-simulated-device"></a><span data-ttu-id="43e30-104">Kom igång med startpaket för IoT-Gateway med en simulerad enhet</span><span class="sxs-lookup"><span data-stu-id="43e30-104">Get started with IoT Gateway Starter Kit with a simulated device</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="43e30-105">SensorTag</span><span class="sxs-lookup"><span data-stu-id="43e30-105">SensorTag</span></span>](iot-hub-gateway-kit-c-get-started.md)
> * [<span data-ttu-id="43e30-106">Simulerade enheten</span><span class="sxs-lookup"><span data-stu-id="43e30-106">Simulated Device</span></span>](iot-hub-gateway-kit-c-sim-get-started.md)

<span data-ttu-id="43e30-107">I den här självstudiekursen börjar du med att lära sig hello grunderna i att arbeta med [IoT Gateway startpaket](https://aka.ms/gateway-kit).</span><span class="sxs-lookup"><span data-stu-id="43e30-107">In this tutorial, you begin by learning hello basics of working with [IoT Gateway Starter Kit](https://aka.ms/gateway-kit).</span></span> <span data-ttu-id="43e30-108">Du kommer att arbeta med Intel NUC som kör Linux vind floden.</span><span class="sxs-lookup"><span data-stu-id="43e30-108">You will be working with Intel NUC that's running Wind River Linux.</span></span> <span data-ttu-id="43e30-109">Får du lära dig hur tooseamleesly ansluta enheter toohello molnet med hjälp av Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="43e30-109">You will learn how tooseamleesly connect your devices toohello cloud by using Azure IoT Hub.</span></span>

***
<span data-ttu-id="43e30-110">**Har ett kit än?:** klickar du på [här](https://aka.ms/gateway-kit).</span><span class="sxs-lookup"><span data-stu-id="43e30-110">**Don't have a kit yet?:** Click [here](https://aka.ms/gateway-kit).</span></span>
***

## <a name="lesson-1-configure-your-nuc"></a><span data-ttu-id="43e30-111">Lektion 1: Konfigurera din NUC</span><span class="sxs-lookup"><span data-stu-id="43e30-111">Lesson 1: Configure your NUC</span></span>
![Diagram för Lesson1 slutpunkt till slutpunkt](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson1.png)

<span data-ttu-id="43e30-113">Installerar hello Azure IoT kanten på NUC nu bör du ställa in Intel NUC (nästa enhet för datorer) i hello Kit som en Azure IoT-gateway och kör ett exempel app tooverify hello gateway-funktioner.</span><span class="sxs-lookup"><span data-stu-id="43e30-113">In this lesson, you set up Intel NUC (Next Unit of Computing) in hello Kit as an Azure IoT gateway, install hello Azure IoT Edge package on NUC, and run a sample app tooverify hello gateway functionality.</span></span>

<span data-ttu-id="43e30-114">*Uppskattad tid toocomplete: 15 minuter*</span><span class="sxs-lookup"><span data-stu-id="43e30-114">*Estimated time toocomplete: 15 minutes*</span></span>

<span data-ttu-id="43e30-115">Gå för[konfigurera Intel NUC som en IoT-gateway](iot-hub-gateway-kit-c-sim-lesson1-set-up-nuc.md)</span><span class="sxs-lookup"><span data-stu-id="43e30-115">Go too[Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-sim-lesson1-set-up-nuc.md)</span></span>

## <a name="lesson-2-create-your-iot-hub"></a><span data-ttu-id="43e30-116">Lektion 2: Skapa din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="43e30-116">Lesson 2: Create your IoT Hub</span></span>
![Diagram för Lesson2 slutpunkt till slutpunkt](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson2.png)

<span data-ttu-id="43e30-118">Nu bör installera du hello verktyg och program på värddatorn.</span><span class="sxs-lookup"><span data-stu-id="43e30-118">In this lesson, you install hello tools and software on your host computer.</span></span> <span data-ttu-id="43e30-119">Sedan du skapar din kostnadsfria Azure-konto, etablera Azure IoT-hubb och skapa din första enhet i hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="43e30-119">Then you create your free Azure account, provision your Azure IoT hub and create your first device in hello IoT hub.</span></span>

<span data-ttu-id="43e30-120">Slutför lektionen 1 innan du börjar övningen.</span><span class="sxs-lookup"><span data-stu-id="43e30-120">Complete Lesson 1 before you start this lesson.</span></span>

### <a name="get-hello-tools"></a><span data-ttu-id="43e30-121">Hämta hello-verktyg</span><span class="sxs-lookup"><span data-stu-id="43e30-121">Get hello tools</span></span>
<span data-ttu-id="43e30-122">Installera hello verktyg och programvara på värddatorn.</span><span class="sxs-lookup"><span data-stu-id="43e30-122">Install hello tools and software on your host computer.</span></span>

<span data-ttu-id="43e30-123">*Uppskattad tid toocomplete: 20 minuter*</span><span class="sxs-lookup"><span data-stu-id="43e30-123">*Estimated time toocomplete: 20 minutes*</span></span>

<span data-ttu-id="43e30-124">Gå för[hämta hello-verktyg](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)</span><span class="sxs-lookup"><span data-stu-id="43e30-124">Go too[Get hello tools](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)</span></span>

### <a name="create-an-iot-hub-and-register-your-device"></a><span data-ttu-id="43e30-125">Skapa en IoT-hubb och registrera din enhet</span><span class="sxs-lookup"><span data-stu-id="43e30-125">Create an IoT hub and register your device</span></span>
<span data-ttu-id="43e30-126">Skapa en resursgrupp, etablera din första Azure IoT-hubb och Lägg till din första enhet toohello IoT-hubb med hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="43e30-126">Create your resource group, provision your first Azure IoT hub, and add your first device toohello IoT hub using hello Azure CLI.</span></span>

<span data-ttu-id="43e30-127">*Uppskattad tid toocomplete: 10 minuter*</span><span class="sxs-lookup"><span data-stu-id="43e30-127">*Estimated time toocomplete: 10 minutes*</span></span>

<span data-ttu-id="43e30-128">Gå för[skapar en IoT-hubb och registrerar din enhet](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)</span><span class="sxs-lookup"><span data-stu-id="43e30-128">Go too[Create an IoT hub and register your device](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)</span></span>

## <a name="lesson-3-receive-messages-from-hello-simulated-device-and-read-messages-from-your-iot-hub"></a><span data-ttu-id="43e30-129">Lektionen 3: Ta emot meddelanden från hello simulerade enheten och läsa meddelanden från din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="43e30-129">Lesson 3: Receive messages from hello simulated device and read messages from your IoT hub</span></span>
<span data-ttu-id="43e30-130">Nu bör ska du använda skript tooautomate hello konfiguration och körning av en simulerad enhetsapp i din gateway.</span><span class="sxs-lookup"><span data-stu-id="43e30-130">In this lesson, you will use scripts tooautomate hello configuration and execution of a simulated device app in your gateway.</span></span> <span data-ttu-id="43e30-131">hello simulerade enhetsapp genererar temperatur exempeldata och skickar den tooan IoT hub-modulen.</span><span class="sxs-lookup"><span data-stu-id="43e30-131">hello simulated device app generates sample temperature data and sends it tooan IoT hub module.</span></span> <span data-ttu-id="43e30-132">Hej IoT-hubb modulen paket hello data tas emot och skickar den tooyour IoT-hubb via hello gateway ramverk i Azure IoT kant.</span><span class="sxs-lookup"><span data-stu-id="43e30-132">hello IoT hub module packages hello data received and sends it tooyour IoT hub through hello gateway framework provided in Azure IoT Edge.</span></span>

![Diagram över lektionen 3-slutpunkt till slutpunkt](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson3.png)

### <a name="configure-and-run-a-simulated-device"></a><span data-ttu-id="43e30-134">Konfigurera och köra en simulerad enhet</span><span class="sxs-lookup"><span data-stu-id="43e30-134">Configure and run a simulated device</span></span>
<span data-ttu-id="43e30-135">Förbered hello exempel koder.</span><span class="sxs-lookup"><span data-stu-id="43e30-135">Prepare hello sample codes.</span></span> <span data-ttu-id="43e30-136">Konfigurera och köra hello simulerade enheten exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="43e30-136">Then configure and run hello simulated device sample application.</span></span>

<span data-ttu-id="43e30-137">*Uppskattad tid toocomplete: 15 minuter*</span><span class="sxs-lookup"><span data-stu-id="43e30-137">*Estimated time toocomplete: 15 minutes*</span></span>

<span data-ttu-id="43e30-138">Gå för[konfigurera och köra en simulerad enhet](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)</span><span class="sxs-lookup"><span data-stu-id="43e30-138">Go too[Configure and run a simulated device](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)</span></span>

### <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="43e30-139">Läsa meddelanden från din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="43e30-139">Read messages from your IoT hub</span></span>
<span data-ttu-id="43e30-140">Köra en exempelkod på värden för datorn tooread hälsningsmeddelande från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="43e30-140">Run a sample code on your host computer tooread hello messages from your IoT hub.</span></span>

<span data-ttu-id="43e30-141">*Uppskattad tid toocomplete: 15 minuter*</span><span class="sxs-lookup"><span data-stu-id="43e30-141">*Estimated time toocomplete: 15 minutes*</span></span>

<span data-ttu-id="43e30-142">Gå för[läsa meddelanden från din IoT-hubb](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)</span><span class="sxs-lookup"><span data-stu-id="43e30-142">Go too[Read messages from your IoT hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)</span></span>

## <a name="lesson-4-save-messages-tooazure-table-storage"></a><span data-ttu-id="43e30-143">Lektionen 4: Spara meddelanden tooAzure Table storage</span><span class="sxs-lookup"><span data-stu-id="43e30-143">Lesson 4: Save messages tooAzure Table storage</span></span>
<span data-ttu-id="43e30-144">Skapa en funktionsapp i Azure-som hämtar inkommande meddelanden från din IoT-hubb och skriver dem tooAzure Table storage.</span><span class="sxs-lookup"><span data-stu-id="43e30-144">Create an Azure function app that gets incoming messages from your IoT hub and writes them tooAzure Table storage.</span></span>

![Lektionen 4 slutpunkt till slutpunkt-diagram](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson4.png)

### <a name="create-an-azure-function-app-and-azure-storage-account"></a><span data-ttu-id="43e30-146">Skapa en Azure funktionsapp och Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="43e30-146">Create an Azure function app and Azure Storage account</span></span>
<span data-ttu-id="43e30-147">Använda en Azure Resource Manager-mall toocreate en funktionsapp i Azure-och ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="43e30-147">Use an Azure Resource Manager template toocreate an Azure function app and an Azure Storage account.</span></span>

<span data-ttu-id="43e30-148">*Uppskattad tid toocomplete: 10 minuter*</span><span class="sxs-lookup"><span data-stu-id="43e30-148">*Estimated time toocomplete: 10 minutes*</span></span>

<span data-ttu-id="43e30-149">Gå för[skapa en Azure funktionsapp och Azure Storage-konto](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)</span><span class="sxs-lookup"><span data-stu-id="43e30-149">Go too[Create an Azure function app and Azure Storage account](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)</span></span>

### <a name="read-messages-persisted-in-azure-table-storage"></a><span data-ttu-id="43e30-150">Läs meddelandena kvar i Azure Table storage</span><span class="sxs-lookup"><span data-stu-id="43e30-150">Read messages persisted in Azure Table storage</span></span>
<span data-ttu-id="43e30-151">Övervaka hälsningsmeddelande för gateway-to-cloud medan de skrivs tooAzure Table storage.</span><span class="sxs-lookup"><span data-stu-id="43e30-151">Monitor hello gateway-to-cloud messages as they are written tooAzure Table storage.</span></span>

<span data-ttu-id="43e30-152">*Uppskattad tid toocomplete: 5 minuter*</span><span class="sxs-lookup"><span data-stu-id="43e30-152">*Estimated time toocomplete: 5 minutes*</span></span>

<span data-ttu-id="43e30-153">Gå för[lästa meddelanden kvar i Azure Table storage](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).</span><span class="sxs-lookup"><span data-stu-id="43e30-153">Go too[Read messages persisted in Azure Table storage](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="43e30-154">Felsökning</span><span class="sxs-lookup"><span data-stu-id="43e30-154">Troubleshooting</span></span>
<span data-ttu-id="43e30-155">Om det uppstår problem under hello erfarenheter söka efter lösningar i hello [felsökning](iot-hub-gateway-kit-c-sim-troubleshooting.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="43e30-155">If you have any problems during hello lessons, look for solutions in hello [Troubleshooting](iot-hub-gateway-kit-c-sim-troubleshooting.md) article.</span></span>

## <a name="explore-more"></a><span data-ttu-id="43e30-156">Utforska mer</span><span class="sxs-lookup"><span data-stu-id="43e30-156">Explore more</span></span>
<span data-ttu-id="43e30-157">Besök hello [Intel IoT Gateway Kit developer zonen](https://software.intel.com/en-us/iot/hardware/gateways/dev-kit) toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="43e30-157">Visit hello [Intel IoT Gateway Kit developer zone](https://software.intel.com/en-us/iot/hardware/gateways/dev-kit) toolearn more.</span></span>
