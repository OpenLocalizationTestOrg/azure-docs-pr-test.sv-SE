---
title: "aaaSimulated hallon Pi toocloud (Node.js) – ansluta hallon Pi web simulator tooAzure IoT-hubb | Microsoft Docs"
description: "Ansluta hallon Pi web simulator tooAzure IoT-hubb för hallon Pi toosend data toohello Azure-molnet."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Raspberry pi simulatorn, azure iot raspberry pi, raspberry pi iot-hubb, raspberry pi skicka data toocloud, raspberry pi toocloud
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/7/2017
ms.author: xshi
ms.openlocfilehash: 0ebe1004e96ff4e64fdd1b05b7e35192ec42f7d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-raspberry-pi-online-simulator-tooazure-iot-hub-nodejs"></a><span data-ttu-id="51aa0-104">Ansluta hallon Pi online simulator tooAzure IoT-hubb (Node.js)</span><span class="sxs-lookup"><span data-stu-id="51aa0-104">Connect Raspberry Pi online simulator tooAzure IoT Hub (Node.js)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="51aa0-105">I den här självstudiekursen börjar du med learning hello grunderna i att arbeta med hallon Pi online simulatorn.</span><span class="sxs-lookup"><span data-stu-id="51aa0-105">In this tutorial, you begin by learning hello basics of working with Raspberry Pi online simulator.</span></span> <span data-ttu-id="51aa0-106">Du lär dig hur tooseamlessly ansluta hello Pi simulator toohello moln med hjälp av [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="51aa0-106">You then learn how tooseamlessly connect hello Pi simulator toohello cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span> 

<span data-ttu-id="51aa0-107">Om du har fysiska enheter gå [ansluta hallon Pi tooAzure IoT-hubb](iot-hub-raspberry-pi-kit-node-get-started.md) tooget igång.</span><span class="sxs-lookup"><span data-stu-id="51aa0-107">If you have physical devices, visit [Connect Raspberry Pi tooAzure IoT Hub](iot-hub-raspberry-pi-kit-node-get-started.md) tooget started.</span></span> 

<p>
<div id="diag" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#getstarted">
<img src="media/iot-hub-raspberry-pi-web-simulator/3_banner.png" alt="Connect Raspberry Pi web simulator tooAzure IoT Hub" width="400">
</div>
<p>
<div id="button" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted">
<img src="media/iot-hub-raspberry-pi-web-simulator/6_button_default.png" alt="Start Raspberry Pi simulator" width="400" onmouseover="this.src='media/iot-hub-raspberry-pi-web-simulator/5_button_click.png';" onmouseout="this.src='media/iot-hub-raspberry-pi-web-simulator/6_button_default.png';">
</div>

## <a name="what-you-do"></a><span data-ttu-id="51aa0-108">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="51aa0-108">What you do</span></span>

* <span data-ttu-id="51aa0-109">Lär dig grunderna hello av hallon Pi online simulatorn.</span><span class="sxs-lookup"><span data-stu-id="51aa0-109">Learn hello basics of Raspberry Pi online simulator.</span></span>
* <span data-ttu-id="51aa0-110">Skapa en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="51aa0-110">Create an IoT hub.</span></span>
* <span data-ttu-id="51aa0-111">Registrera en enhet för Pi i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="51aa0-111">Register a device for Pi in your IoT hub.</span></span>
* <span data-ttu-id="51aa0-112">Kör ett exempelprogram på Pi toosend simulerade sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="51aa0-112">Run a sample application on Pi toosend simulated sensor data tooyour IoT hub.</span></span>

<span data-ttu-id="51aa0-113">Ansluta simulerade hallon Pi tooan IoT-hubb som du skapar.</span><span class="sxs-lookup"><span data-stu-id="51aa0-113">Connect simulated Raspberry Pi tooan IoT hub that you create.</span></span> <span data-ttu-id="51aa0-114">Sedan kör du ett exempelprogram med hello simulator toogenerate sensordata.</span><span class="sxs-lookup"><span data-stu-id="51aa0-114">Then you run a sample application with hello simulator toogenerate sensor data.</span></span> <span data-ttu-id="51aa0-115">Slutligen kan skicka du hello sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="51aa0-115">Finally, you send hello sensor data tooyour IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="51aa0-116">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="51aa0-116">What you learn</span></span>

* <span data-ttu-id="51aa0-117">Hur toocreate en Azure IoT-hubb och få nya enheten anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="51aa0-117">How toocreate an Azure IoT hub and get your new device connection string.</span></span> <span data-ttu-id="51aa0-118">Om du inte har ett Azure-konto [skapa ett kostnadsfritt Azure konto](https://azure.microsoft.com/free/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="51aa0-118">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="51aa0-119">Hur toowork med hallon Pi online simulatorn.</span><span class="sxs-lookup"><span data-stu-id="51aa0-119">How toowork with Raspberry Pi online simulator.</span></span>
* <span data-ttu-id="51aa0-120">Hur toosend sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="51aa0-120">How toosend sensor data tooyour IoT hub.</span></span>

## <a name="overview-of-raspberry-pi-web-simulator"></a><span data-ttu-id="51aa0-121">Översikt över hallon Pi web simulator</span><span class="sxs-lookup"><span data-stu-id="51aa0-121">Overview of Raspberry Pi web simulator</span></span>

<span data-ttu-id="51aa0-122">Klicka på hello knappen toolaunch hallon Pi online simulatorn.</span><span class="sxs-lookup"><span data-stu-id="51aa0-122">Click hello button toolaunch Raspberry Pi online simulator.</span></span>

> [!div class="button"]
[<span data-ttu-id="51aa0-123">Starta hallon Pi simulator</span><span class="sxs-lookup"><span data-stu-id="51aa0-123">Start Raspberry Pi simulator</span></span>](https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted)

<span data-ttu-id="51aa0-124">Det finns tre områden i hello web-simulatorn.</span><span class="sxs-lookup"><span data-stu-id="51aa0-124">There are three areas in hello web simulator.</span></span>
* <span data-ttu-id="51aa0-125">Sammansättningen område - hello standard kretsen är att en Pi ansluter med en BME280 sensor och en Indikator.</span><span class="sxs-lookup"><span data-stu-id="51aa0-125">Assembly area - hello default circuit is that a Pi connects with a BME280 sensor and an LED.</span></span> <span data-ttu-id="51aa0-126">hello området är låst i förhandsversionen så för närvarande kan du inte göra anpassning.</span><span class="sxs-lookup"><span data-stu-id="51aa0-126">hello area is locked in preview version so currently you cannot do customization.</span></span>
* <span data-ttu-id="51aa0-127">Kodning område - Kodredigerare online för toocode med hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="51aa0-127">Coding area - An online code editor for you toocode with Raspberry Pi.</span></span> <span data-ttu-id="51aa0-128">hello standard exempelprogrammet hjälper toocollect sensordata från BME280 sensor och skickar tooyour Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="51aa0-128">hello default sample application helps toocollect sensor data from BME280 sensor and sends tooyour Azure IoT Hub.</span></span> <span data-ttu-id="51aa0-129">hello programmet är helt kompatibel med verkliga Pi-enheter.</span><span class="sxs-lookup"><span data-stu-id="51aa0-129">hello application is fully compatible with real Pi devices.</span></span> 
* <span data-ttu-id="51aa0-130">Integrerad konsolfönstret - visar hello utdata från din kod.</span><span class="sxs-lookup"><span data-stu-id="51aa0-130">Integrated console window - It shows hello output of your code.</span></span> <span data-ttu-id="51aa0-131">Det finns tre knappar överst hello i det här fönstret.</span><span class="sxs-lookup"><span data-stu-id="51aa0-131">At hello top of this window, there are three buttons.</span></span>
   * <span data-ttu-id="51aa0-132">**Kör** -körs programmet hello i hello kodning område.</span><span class="sxs-lookup"><span data-stu-id="51aa0-132">**Run** - Run hello application in hello coding area.</span></span>
   * <span data-ttu-id="51aa0-133">**Återställ** -återställning hello kodning området toohello standard exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="51aa0-133">**Reset** - Reset hello coding area toohello default sample application.</span></span>
   * <span data-ttu-id="51aa0-134">**Vikning/Expandera** – på höger sida det är en knapp för du toofold/Expandera hello konsolfönstret hello.</span><span class="sxs-lookup"><span data-stu-id="51aa0-134">**Fold/Expand** - On hello right side there is a button for you toofold/expand hello console window.</span></span>

> [!NOTE] 
<span data-ttu-id="51aa0-135">hello hallon Pi web simulator är nu tillgängliga i förhandsversionen.</span><span class="sxs-lookup"><span data-stu-id="51aa0-135">hello Raspberry Pi web simulator is now available in preview version.</span></span> <span data-ttu-id="51aa0-136">Vi vill gärna toohear rösten i hello [Gitter Chatroom](https://gitter.im/Microsoft/raspberry-pi-web-simulator).</span><span class="sxs-lookup"><span data-stu-id="51aa0-136">We'd like toohear your voice in hello [Gitter Chatroom](https://gitter.im/Microsoft/raspberry-pi-web-simulator).</span></span> <span data-ttu-id="51aa0-137">hello källkoden är offentlig på [Github](https://github.com/Azure-Samples/raspberry-pi-web-simulator).</span><span class="sxs-lookup"><span data-stu-id="51aa0-137">hello source code is public on [Github](https://github.com/Azure-Samples/raspberry-pi-web-simulator).</span></span>

![Översikt över Pi online simulator](media/iot-hub-raspberry-pi-web-simulator/0_overview.png)

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]


## <a name="run-a-sample-application-on-pi-web-simulator"></a><span data-ttu-id="51aa0-139">Kör ett exempelprogram på Pi web simulator</span><span class="sxs-lookup"><span data-stu-id="51aa0-139">Run a sample application on Pi web simulator</span></span>

1. <span data-ttu-id="51aa0-140">Kontrollera att du arbetar med hello standard exempelprogrammet i kodning område.</span><span class="sxs-lookup"><span data-stu-id="51aa0-140">In coding area, make sure you are working on hello default sample application.</span></span> <span data-ttu-id="51aa0-141">Ersätt hello platshållare i rad 15 med hello anslutningssträngen för Azure IoT hub-enhet.</span><span class="sxs-lookup"><span data-stu-id="51aa0-141">Replace hello placeholder in Line 15 with hello Azure IoT hub device connection string.</span></span>
   <span data-ttu-id="51aa0-142">![Ersätt hello anslutningssträngen för enhet](media/iot-hub-raspberry-pi-web-simulator/1_connectionstring.png)</span><span class="sxs-lookup"><span data-stu-id="51aa0-142">![Replace hello device connection string](media/iot-hub-raspberry-pi-web-simulator/1_connectionstring.png)</span></span>

2. <span data-ttu-id="51aa0-143">Klicka på **kör** eller typ `npm start` toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="51aa0-143">Click **Run** or type `npm start` toorun hello application.</span></span>


<span data-ttu-id="51aa0-144">Du bör se följande hello utdata som visar hello sensordata och hello-meddelanden som skickas tooyour IoT-hubb ![utdata - sensordata som skickas från hallon Pi tooyour IoT-hubb](media/iot-hub-raspberry-pi-web-simulator/2_run_application.png)</span><span class="sxs-lookup"><span data-stu-id="51aa0-144">You should see hello following output that shows hello sensor data and hello messages that are sent tooyour IoT hub ![Output - sensor data sent from Raspberry Pi tooyour IoT hub](media/iot-hub-raspberry-pi-web-simulator/2_run_application.png)</span></span>


## <a name="next-steps"></a><span data-ttu-id="51aa0-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="51aa0-145">Next steps</span></span>

<span data-ttu-id="51aa0-146">Du har kört ett program toocollect sensor exempeldata och skicka den tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="51aa0-146">You’ve run a sample application toocollect sensor data and send it tooyour IoT hub.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
