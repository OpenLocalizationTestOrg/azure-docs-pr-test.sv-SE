---
title: "Simulerade hallon Pi till molnet (Node.js) – ansluta hallon Pi web simulator som Azure IoT Hub | Microsoft Docs"
description: "Ansluta hallon Pi web simulator till Azure IoT-hubb för hallon Pi att skicka data till Azure-molnet."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Raspberry pi simulatorn, azure iot raspberry pi, raspberry pi iot-hubb, raspberry pi skickar data till molnet, raspberry pi till molnet
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/28/2017
ms.author: xshi
ms.openlocfilehash: 3b80bf35d6af91d5bdb196d97668dc0f837b92cc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="connect-raspberry-pi-online-simulator-to-azure-iot-hub-nodejs"></a><span data-ttu-id="e7384-104">Ansluta hallon Pi online simulator till Azure IoT Hub (Node.js)</span><span class="sxs-lookup"><span data-stu-id="e7384-104">Connect Raspberry Pi online simulator to Azure IoT Hub (Node.js)</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="e7384-105">I den här kursen kan du börja genom att lära dig grunderna i att arbeta med hallon Pi online simulatorn.</span><span class="sxs-lookup"><span data-stu-id="e7384-105">In this tutorial, you begin by learning the basics of working with Raspberry Pi online simulator.</span></span> <span data-ttu-id="e7384-106">Du lär dig sedan sömlöst ansluter Pi simulatorn till molnet med hjälp av [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span><span class="sxs-lookup"><span data-stu-id="e7384-106">You then learn how to seamlessly connect the Pi simulator to the cloud by using [Azure IoT Hub](iot-hub-what-is-iot-hub.md).</span></span> 

<span data-ttu-id="e7384-107">Om du har fysiska enheter gå [ansluta hallon Pi till Azure IoT Hub](iot-hub-raspberry-pi-kit-node-get-started.md) att komma igång.</span><span class="sxs-lookup"><span data-stu-id="e7384-107">If you have physical devices, visit [Connect Raspberry Pi to Azure IoT Hub](iot-hub-raspberry-pi-kit-node-get-started.md) to get started.</span></span> 

<p>
<div id="diag" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#getstarted" target="_blank">
<img src="media/iot-hub-raspberry-pi-web-simulator/3_banner.png" alt="Connect Raspberry Pi web simulator to Azure IoT Hub" width="400">
</div>
<p>
<div id="button" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted" target="_blank">
<img src="media/iot-hub-raspberry-pi-web-simulator/6_button_default.png" alt="Start Raspberry Pi simulator" width="400" onmouseover="this.src='media/iot-hub-raspberry-pi-web-simulator/5_button_click.png';" onmouseout="this.src='media/iot-hub-raspberry-pi-web-simulator/6_button_default.png';">
</div>

## <a name="what-you-do"></a><span data-ttu-id="e7384-108">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="e7384-108">What you do</span></span>

* <span data-ttu-id="e7384-109">Lär dig grunderna i hallon Pi online simulatorn.</span><span class="sxs-lookup"><span data-stu-id="e7384-109">Learn the basics of Raspberry Pi online simulator.</span></span>
* <span data-ttu-id="e7384-110">Skapa en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e7384-110">Create an IoT hub.</span></span>
* <span data-ttu-id="e7384-111">Registrera en enhet för Pi i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e7384-111">Register a device for Pi in your IoT hub.</span></span>
* <span data-ttu-id="e7384-112">Kör ett exempelprogram på Pi simulerade sensordata ska skickas till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e7384-112">Run a sample application on Pi to send simulated sensor data to your IoT hub.</span></span>

<span data-ttu-id="e7384-113">Simulerade hallon Pi att ansluta till en IoT-hubb som du skapar.</span><span class="sxs-lookup"><span data-stu-id="e7384-113">Connect simulated Raspberry Pi to an IoT hub that you create.</span></span> <span data-ttu-id="e7384-114">Sedan kör du ett exempelprogram med simulatorn att generera sensordata.</span><span class="sxs-lookup"><span data-stu-id="e7384-114">Then you run a sample application with the simulator to generate sensor data.</span></span> <span data-ttu-id="e7384-115">Slutligen kan du skicka dessa sensordata för din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e7384-115">Finally, you send the sensor data to your IoT hub.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="e7384-116">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="e7384-116">What you learn</span></span>

* <span data-ttu-id="e7384-117">Hur du skapar en Azure IoT-hubb och hämta din nya anslutningssträngen för enheten.</span><span class="sxs-lookup"><span data-stu-id="e7384-117">How to create an Azure IoT hub and get your new device connection string.</span></span> <span data-ttu-id="e7384-118">Om du inte har ett Azure-konto [skapa ett kostnadsfritt Azure konto](https://azure.microsoft.com/free/) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="e7384-118">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
* <span data-ttu-id="e7384-119">Hur du arbetar med hallon Pi online simulatorn.</span><span class="sxs-lookup"><span data-stu-id="e7384-119">How to work with Raspberry Pi online simulator.</span></span>
* <span data-ttu-id="e7384-120">Hur du skickar sensordata till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e7384-120">How to send sensor data to your IoT hub.</span></span>

## <a name="overview-of-raspberry-pi-web-simulator"></a><span data-ttu-id="e7384-121">Översikt över hallon Pi web simulator</span><span class="sxs-lookup"><span data-stu-id="e7384-121">Overview of Raspberry Pi web simulator</span></span>

<span data-ttu-id="e7384-122">Klicka på knappen för att starta hallon Pi online simulatorn.</span><span class="sxs-lookup"><span data-stu-id="e7384-122">Click the button to launch Raspberry Pi online simulator.</span></span>

> [!div class="button"]
<span data-ttu-id="e7384-123"><a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted" target="_blank">Starta Raspberry Pi Simulator</a></span><span class="sxs-lookup"><span data-stu-id="e7384-123"><a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted" target="_blank">Start Raspberry Pi Simulator</a></span></span>

<span data-ttu-id="e7384-124">Det finns tre områden i web-simulatorn.</span><span class="sxs-lookup"><span data-stu-id="e7384-124">There are three areas in the web simulator.</span></span>
1. <span data-ttu-id="e7384-125">Sammansättningen område - standard kretsen är att en Pi ansluter med en BME280 sensor och en Indikator.</span><span class="sxs-lookup"><span data-stu-id="e7384-125">Assembly area - The default circuit is that a Pi connects with a BME280 sensor and an LED.</span></span> <span data-ttu-id="e7384-126">Området är låst i förhandsversionen så för närvarande kan du inte göra anpassning.</span><span class="sxs-lookup"><span data-stu-id="e7384-126">The area is locked in preview version so currently you cannot do customization.</span></span>
2. <span data-ttu-id="e7384-127">Kodning område - redigerare online koden du till kod med hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="e7384-127">Coding area - An online code editor for you to code with Raspberry Pi.</span></span> <span data-ttu-id="e7384-128">Exempelprogrammet standard hjälper dig att samla in sensordata från BME280 sensor och skickar till din Azure IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e7384-128">The default sample application helps to collect sensor data from BME280 sensor and sends to your Azure IoT Hub.</span></span> <span data-ttu-id="e7384-129">Programmet är helt kompatibel med verkliga Pi-enheter.</span><span class="sxs-lookup"><span data-stu-id="e7384-129">The application is fully compatible with real Pi devices.</span></span> 
3. <span data-ttu-id="e7384-130">Integrerad konsolfönstret - visar utdata från din kod.</span><span class="sxs-lookup"><span data-stu-id="e7384-130">Integrated console window - It shows the output of your code.</span></span> <span data-ttu-id="e7384-131">Det finns tre knappar längst upp i det här fönstret.</span><span class="sxs-lookup"><span data-stu-id="e7384-131">At the top of this window, there are three buttons.</span></span>
   * <span data-ttu-id="e7384-132">**Kör** -köra programmet i området för kodning.</span><span class="sxs-lookup"><span data-stu-id="e7384-132">**Run** - Run the application in the coding area.</span></span>
   * <span data-ttu-id="e7384-133">**Återställ** -återställa området kodning till exempelprogrammet standard.</span><span class="sxs-lookup"><span data-stu-id="e7384-133">**Reset** - Reset the coding area to the default sample application.</span></span>
   * <span data-ttu-id="e7384-134">**Vikning/Expandera** -det finns en knapp för du vikning/Expandera konsolfönstret på höger sida.</span><span class="sxs-lookup"><span data-stu-id="e7384-134">**Fold/Expand** - On the right side there is a button for you to fold/expand the console window.</span></span>

> [!NOTE] 
<span data-ttu-id="e7384-135">Web-simulatorn hallon Pi är nu tillgängliga i förhandsversionen.</span><span class="sxs-lookup"><span data-stu-id="e7384-135">The Raspberry Pi web simulator is now available in preview version.</span></span> <span data-ttu-id="e7384-136">Vi hoppas att du hör din röst i den [Gitter Chatroom](https://gitter.im/Microsoft/raspberry-pi-web-simulator).</span><span class="sxs-lookup"><span data-stu-id="e7384-136">We'd like to hear your voice in the [Gitter Chatroom](https://gitter.im/Microsoft/raspberry-pi-web-simulator).</span></span> <span data-ttu-id="e7384-137">Källkoden är offentlig på [Github](https://github.com/Azure-Samples/raspberry-pi-web-simulator).</span><span class="sxs-lookup"><span data-stu-id="e7384-137">The source code is public on [Github](https://github.com/Azure-Samples/raspberry-pi-web-simulator).</span></span>

![Översikt över Pi online simulator](media/iot-hub-raspberry-pi-web-simulator/0_overview.png)

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]


## <a name="run-a-sample-application-on-pi-web-simulator"></a><span data-ttu-id="e7384-139">Kör ett exempelprogram på Pi web simulator</span><span class="sxs-lookup"><span data-stu-id="e7384-139">Run a sample application on Pi web simulator</span></span>

1. <span data-ttu-id="e7384-140">Kontrollera att du arbetar med exempelprogrammet som standard i kodning område.</span><span class="sxs-lookup"><span data-stu-id="e7384-140">In coding area, make sure you are working on the default sample application.</span></span> <span data-ttu-id="e7384-141">Ersätt platshållaren i rad 15 med anslutningssträngen för Azure IoT hub-enhet.</span><span class="sxs-lookup"><span data-stu-id="e7384-141">Replace the placeholder in Line 15 with the Azure IoT hub device connection string.</span></span>
   <span data-ttu-id="e7384-142">![Ersätt anslutningssträngen enhet](media/iot-hub-raspberry-pi-web-simulator/1_connectionstring.png)</span><span class="sxs-lookup"><span data-stu-id="e7384-142">![Replace the device connection string](media/iot-hub-raspberry-pi-web-simulator/1_connectionstring.png)</span></span>

2. <span data-ttu-id="e7384-143">Klicka på **kör** eller typ `npm start` att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="e7384-143">Click **Run** or type `npm start` to run the application.</span></span>


<span data-ttu-id="e7384-144">Du bör se följande utdata som visar dessa sensordata och meddelanden som skickas till din IoT-hubb ![utdata - sensordata som skickats från hallon Pi till din IoT-hubb](media/iot-hub-raspberry-pi-web-simulator/2_run_application.png)</span><span class="sxs-lookup"><span data-stu-id="e7384-144">You should see the following output that shows the sensor data and the messages that are sent to your IoT hub ![Output - sensor data sent from Raspberry Pi to your IoT hub](media/iot-hub-raspberry-pi-web-simulator/2_run_application.png)</span></span>


## <a name="next-steps"></a><span data-ttu-id="e7384-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e7384-145">Next steps</span></span>

<span data-ttu-id="e7384-146">Du har kört ett exempelprogram för att samla in sensordata och skicka den till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e7384-146">You’ve run a sample application to collect sensor data and send it to your IoT hub.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
