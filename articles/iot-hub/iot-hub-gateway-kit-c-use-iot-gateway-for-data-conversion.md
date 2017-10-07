---
title: "aaaData konvertering på IoT-gateway med Azure IoT kant | Microsoft Docs"
description: "Använd IoT gateway tooconvert hello format för sensordata via en anpassad modul från Azure IoT kant."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT-gateway datakonvertering, DTS för iot-gateway"
ms.assetid: 75f2573d-500b-4405-bff7-61021c4c3500
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2017
ms.author: xshi
ms.openlocfilehash: ae94b1f96f36dfcb4f77fadc0ece3cff3d0bba91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-iot-gateway-for-sensor-data-transformation-with-azure-iot-edge"></a><span data-ttu-id="22d36-104">Använd IoT-gateway för omvandling av sensor data med Azure IoT kant</span><span class="sxs-lookup"><span data-stu-id="22d36-104">Use IoT gateway for sensor data transformation with Azure IoT Edge</span></span>

> [!NOTE]
> <span data-ttu-id="22d36-105">Innan du börjar den här självstudiekursen, se har till att du slutfört hello följande erfarenheter i ordning:</span><span class="sxs-lookup"><span data-stu-id="22d36-105">Before you start this tutorial, make sure you’ve completed hello following lessons in sequence:</span></span>
> * [<span data-ttu-id="22d36-106">Konfigurera Intel NUC som en IoT-gateway</span><span class="sxs-lookup"><span data-stu-id="22d36-106">Set up Intel NUC as an IoT gateway</span></span>](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
> * [<span data-ttu-id="22d36-107">Använd IoT gateway tooconnect saker toohello cloud - SensorTag tooAzure IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="22d36-107">Use IoT gateway tooconnect things toohello cloud - SensorTag tooAzure IoT Hub</span></span>](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

<span data-ttu-id="22d36-108">En syftet med en Iot-gateway är tooprocess insamlade data innan du skickar den toohello moln.</span><span class="sxs-lookup"><span data-stu-id="22d36-108">One purpose of an Iot gateway is tooprocess collected data before sending it toohello cloud.</span></span> <span data-ttu-id="22d36-109">Azure IoT-Edge introducerar moduler som skapats och monterade tooform hello databehandling i arbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="22d36-109">Azure IoT Edge introduces modules which can be created and assembled tooform hello data processing workflow.</span></span> <span data-ttu-id="22d36-110">En modul tar emot ett meddelande, utför en åtgärd på den och flytta den på för andra moduler tooprocess.</span><span class="sxs-lookup"><span data-stu-id="22d36-110">A module receives a message, performs some action on it, and then move it on for other modules tooprocess.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="22d36-111">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="22d36-111">What you learn</span></span>

<span data-ttu-id="22d36-112">Du lär dig hur toocreate en modul tooconvert meddelanden från hello SensorTag till ett annat format.</span><span class="sxs-lookup"><span data-stu-id="22d36-112">You learn how toocreate a module tooconvert messages from hello SensorTag into a different format.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="22d36-113">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="22d36-113">What you do</span></span>

* <span data-ttu-id="22d36-114">Skapa en modul tooconvert ett mottaget meddelande till hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="22d36-114">Create a module tooconvert a received message into hello .json format.</span></span>
* <span data-ttu-id="22d36-115">Kompilera hello-modulen.</span><span class="sxs-lookup"><span data-stu-id="22d36-115">Compile hello module.</span></span>
* <span data-ttu-id="22d36-116">Lägg till hello modulen toohello TIVERA exempelprogrammet från Azure IoT kant.</span><span class="sxs-lookup"><span data-stu-id="22d36-116">Add hello module toohello BLE sample application from Azure IoT Edge.</span></span>
* <span data-ttu-id="22d36-117">Köra hello exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="22d36-117">Run hello sample application.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="22d36-118">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="22d36-118">What you need</span></span>

* <span data-ttu-id="22d36-119">följande kurser som slutförts i sekvens hello:</span><span class="sxs-lookup"><span data-stu-id="22d36-119">hello following tutorials completed in sequence:</span></span>
  * [<span data-ttu-id="22d36-120">Konfigurera Intel NUC som en IoT-gateway</span><span class="sxs-lookup"><span data-stu-id="22d36-120">Set up Intel NUC as an IoT gateway</span></span>](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
  * [<span data-ttu-id="22d36-121">Använd IoT gateway tooconnect saker toohello cloud - SensorTag tooAzure IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="22d36-121">Use IoT gateway tooconnect things toohello cloud - SensorTag tooAzure IoT Hub</span></span>](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)
* <span data-ttu-id="22d36-122">En SSH-klient som körs på värddatorn.</span><span class="sxs-lookup"><span data-stu-id="22d36-122">An SSH client that runs on your host computer.</span></span> <span data-ttu-id="22d36-123">PuTTY rekommenderas i Windows.</span><span class="sxs-lookup"><span data-stu-id="22d36-123">PuTTY is recommended on Windows.</span></span> <span data-ttu-id="22d36-124">Linux- och macOS har redan en SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="22d36-124">Linux and macOS already come with an SSH client.</span></span>
* <span data-ttu-id="22d36-125">hello IP-adress och hello användarnamn och lösenord tooaccess hello gateway från hello SSH-klienten.</span><span class="sxs-lookup"><span data-stu-id="22d36-125">hello IP address and hello username and password tooaccess hello gateway from hello SSH client.</span></span>
* <span data-ttu-id="22d36-126">En Internetanslutning.</span><span class="sxs-lookup"><span data-stu-id="22d36-126">An Internet connection.</span></span>

## <a name="create-a-module"></a><span data-ttu-id="22d36-127">Skapa en modul</span><span class="sxs-lookup"><span data-stu-id="22d36-127">Create a module</span></span>

1. <span data-ttu-id="22d36-128">Kör hello SSH-klienten på värddatorn för hello och ansluta toohello IoT gateway.</span><span class="sxs-lookup"><span data-stu-id="22d36-128">On hello host computer, run hello SSH client and connect toohello IoT gateway.</span></span>
1. <span data-ttu-id="22d36-129">Klona hello källfiler för hello konvertering modul från GitHub toohello-hemkataloger hello IoT gateway genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="22d36-129">Clone hello source files of hello conversion module from GitHub toohello home directory of hello IoT gateway by running hello following commands:</span></span>

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-c-intel-nuc-gateway-customized-module.git
   ```

   <span data-ttu-id="22d36-130">Det här är en inbyggd Azure Edge-modul som skrivits i hello programmeringsspråket C.</span><span class="sxs-lookup"><span data-stu-id="22d36-130">This is a native Azure Edge module written in hello C programming language.</span></span> <span data-ttu-id="22d36-131">hello modulen konverterar hello format för mottagna meddelanden till hello efter:</span><span class="sxs-lookup"><span data-stu-id="22d36-131">hello module converts hello format of received messages into hello following one:</span></span>

   ```json
   {"deviceId": "Intel NUC Gateway", "messageId": 0, "temperature": 0.0}
   ```

## <a name="compile-hello-module"></a><span data-ttu-id="22d36-132">Kompilera hello-modul</span><span class="sxs-lookup"><span data-stu-id="22d36-132">Compile hello module</span></span>

<span data-ttu-id="22d36-133">toocompile hello modul kör hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="22d36-133">toocompile hello module, run hello following commands:</span></span>

```bash
cd iot-hub-c-intel-nuc-gateway-customized-module/my_module
# change hello build script runnable
chmod 777 build.sh
# remove hello invalid windows character
sed -i -e "s/\r$//" build.sh
# run hello build shell script
./build.sh
```

<span data-ttu-id="22d36-134">Du får en `libmy_module.so` filen när hello kompilera har slutförts.</span><span class="sxs-lookup"><span data-stu-id="22d36-134">You get a `libmy_module.so` file after hello compile is completed.</span></span> <span data-ttu-id="22d36-135">Anteckna hello absoluta sökvägen till den här filen.</span><span class="sxs-lookup"><span data-stu-id="22d36-135">Make a note of hello absolute path of this file.</span></span>

## <a name="add-hello-module-toohello-ble-sample-application"></a><span data-ttu-id="22d36-136">Lägg till hello modulen toohello TIVERA exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="22d36-136">Add hello module toohello BLE sample application</span></span>

1. <span data-ttu-id="22d36-137">Gå toohello exempelmapp genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="22d36-137">Go toohello samples folder by running hello following command:</span></span>

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples
   ```

1. <span data-ttu-id="22d36-138">Öppna hello konfigurationsfilen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="22d36-138">Open hello configuration file by running hello following command:</span></span>

   ```bash
   vi ble_gateway.json
   ```

1. <span data-ttu-id="22d36-139">Lägga till en modul genom att lägga till följande kod toohello hello `modules` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="22d36-139">Add a module by inserting hello following code toohello `modules` section.</span></span>

   ```json
   {
       "name": "MyModule",
       "loader": {
           "name": "native",
           "entrypoint":{
               "module.path": "[Your libmy_module.so path]"
               }
            },
        "args": null
    },
    ```

1. <span data-ttu-id="22d36-140">Ersätt `[Your libmy_module.so path]` i hello kod med hello absoluta sökvägen till hello libmy_module.so-filen.</span><span class="sxs-lookup"><span data-stu-id="22d36-140">Replace `[Your libmy_module.so path]` in hello code with hello absolute path of hello libmy_module.so\` file.</span></span>
1. <span data-ttu-id="22d36-141">Ersätt hello koden i hello `links` avsnitt med hello efter:</span><span class="sxs-lookup"><span data-stu-id="22d36-141">Replace hello code in hello `links` section with hello following one:</span></span>

   ```json
   {
       "source": "SensorTag",
       "sink": "MyModule"
   },
   {
       "source": "MyModule",
       "sink": "mapping"
   }
   ```

1. <span data-ttu-id="22d36-142">Tryck på `ESC`, och skriv sedan `:wq` toosave hello-filen.</span><span class="sxs-lookup"><span data-stu-id="22d36-142">Press `ESC`, and then type `:wq` toosave hello file.</span></span>

## <a name="run-hello-sample-application"></a><span data-ttu-id="22d36-143">Köra hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="22d36-143">Run hello sample application</span></span>

1. <span data-ttu-id="22d36-144">Starta hello SensorTag.</span><span class="sxs-lookup"><span data-stu-id="22d36-144">Power on hello SensorTag.</span></span>
1. <span data-ttu-id="22d36-145">Ange hello SSL_CERT_FILE miljövariabeln genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="22d36-145">Set hello SSL_CERT_FILE environment variable by running hello following command:</span></span>

   ```bash
   export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
   ```

1. <span data-ttu-id="22d36-146">Kör exempelprogrammet hello med hello tillagda modulen genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="22d36-146">Run hello sample application with hello added module by running hello following command:</span></span>

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a><span data-ttu-id="22d36-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="22d36-147">Next steps</span></span>

<span data-ttu-id="22d36-148">Nu har du Använd hello IoT gateway tooconvert hello-meddelande från SensorTag till hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="22d36-148">You’ve successfully use hello IoT gateway tooconvert hello message from SensorTag into hello .json format.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
