---
title: "Datakonvertering på IoT-gateway med Azure IoT kant | Microsoft Docs"
description: "Använd IoT-gateway för att konvertera formatet sensordata via en anpassad modul från Azure IoT kant."
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
ms.openlocfilehash: d5c735a4adbc59e9526ec4fd40720c5ec136d63d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="use-iot-gateway-for-sensor-data-transformation-with-azure-iot-edge"></a><span data-ttu-id="c6506-104">Använd IoT-gateway för omvandling av sensor data med Azure IoT kant</span><span class="sxs-lookup"><span data-stu-id="c6506-104">Use IoT gateway for sensor data transformation with Azure IoT Edge</span></span>

> [!NOTE]
> <span data-ttu-id="c6506-105">Innan du börjar den här självstudiekursen, kontrollera att du har slutfört följande erfarenheter i följd:</span><span class="sxs-lookup"><span data-stu-id="c6506-105">Before you start this tutorial, make sure you’ve completed the following lessons in sequence:</span></span>
> * [<span data-ttu-id="c6506-106">Konfigurera Intel NUC som en IoT-gateway</span><span class="sxs-lookup"><span data-stu-id="c6506-106">Set up Intel NUC as an IoT gateway</span></span>](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
> * [<span data-ttu-id="c6506-107">Använd IoT-gateway för att ansluta saker till molnet - SensorTag till Azure IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="c6506-107">Use IoT gateway to connect things to the cloud - SensorTag to Azure IoT Hub</span></span>](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

<span data-ttu-id="c6506-108">En syftar Iot-gateway till att bearbeta insamlade data innan du skickar den till molnet.</span><span class="sxs-lookup"><span data-stu-id="c6506-108">One purpose of an Iot gateway is to process collected data before sending it to the cloud.</span></span> <span data-ttu-id="c6506-109">Azure IoT-Edge introducerar moduler som kan skapas och monteras så att arbetsflödet för databearbetning.</span><span class="sxs-lookup"><span data-stu-id="c6506-109">Azure IoT Edge introduces modules which can be created and assembled to form the data processing workflow.</span></span> <span data-ttu-id="c6506-110">En modul tar emot ett meddelande, utför en åtgärd på den och flytta den på för andra moduler för att bearbeta.</span><span class="sxs-lookup"><span data-stu-id="c6506-110">A module receives a message, performs some action on it, and then move it on for other modules to process.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="c6506-111">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="c6506-111">What you learn</span></span>

<span data-ttu-id="c6506-112">Du lär dig hur du skapar en modul för att konvertera meddelanden från SensorTag till ett annat format.</span><span class="sxs-lookup"><span data-stu-id="c6506-112">You learn how to create a module to convert messages from the SensorTag into a different format.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="c6506-113">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="c6506-113">What you do</span></span>

* <span data-ttu-id="c6506-114">Skapa en modul för att konvertera ett mottaget meddelande till JSON-format.</span><span class="sxs-lookup"><span data-stu-id="c6506-114">Create a module to convert a received message into the .json format.</span></span>
* <span data-ttu-id="c6506-115">Kompilera modulen.</span><span class="sxs-lookup"><span data-stu-id="c6506-115">Compile the module.</span></span>
* <span data-ttu-id="c6506-116">Lägg till modulen exempelprogrammet tabell från Azure IoT kant.</span><span class="sxs-lookup"><span data-stu-id="c6506-116">Add the module to the BLE sample application from Azure IoT Edge.</span></span>
* <span data-ttu-id="c6506-117">Kör exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="c6506-117">Run the sample application.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="c6506-118">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="c6506-118">What you need</span></span>

* <span data-ttu-id="c6506-119">Följande kurser slutförts i ordning:</span><span class="sxs-lookup"><span data-stu-id="c6506-119">The following tutorials completed in sequence:</span></span>
  * [<span data-ttu-id="c6506-120">Konfigurera Intel NUC som en IoT-gateway</span><span class="sxs-lookup"><span data-stu-id="c6506-120">Set up Intel NUC as an IoT gateway</span></span>](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
  * [<span data-ttu-id="c6506-121">Använd IoT-gateway för att ansluta saker till molnet - SensorTag till Azure IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="c6506-121">Use IoT gateway to connect things to the cloud - SensorTag to Azure IoT Hub</span></span>](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)
* <span data-ttu-id="c6506-122">En SSH-klient som körs på värddatorn.</span><span class="sxs-lookup"><span data-stu-id="c6506-122">An SSH client that runs on your host computer.</span></span> <span data-ttu-id="c6506-123">PuTTY rekommenderas i Windows.</span><span class="sxs-lookup"><span data-stu-id="c6506-123">PuTTY is recommended on Windows.</span></span> <span data-ttu-id="c6506-124">Linux- och macOS har redan en SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="c6506-124">Linux and macOS already come with an SSH client.</span></span>
* <span data-ttu-id="c6506-125">IP-adressen och användarnamn och lösenord för åtkomst till gatewayen från SSH-klienten.</span><span class="sxs-lookup"><span data-stu-id="c6506-125">The IP address and the username and password to access the gateway from the SSH client.</span></span>
* <span data-ttu-id="c6506-126">En Internetanslutning.</span><span class="sxs-lookup"><span data-stu-id="c6506-126">An Internet connection.</span></span>

## <a name="create-a-module"></a><span data-ttu-id="c6506-127">Skapa en modul</span><span class="sxs-lookup"><span data-stu-id="c6506-127">Create a module</span></span>

1. <span data-ttu-id="c6506-128">Kör SSH-klienten på värddatorn och ansluta till IoT-gateway.</span><span class="sxs-lookup"><span data-stu-id="c6506-128">On the host computer, run the SSH client and connect to the IoT gateway.</span></span>
1. <span data-ttu-id="c6506-129">Klona källfiler för modulen konvertering från GitHub till arbetskatalogen för IoT-gateway genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="c6506-129">Clone the source files of the conversion module from GitHub to the home directory of the IoT gateway by running the following commands:</span></span>

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-c-intel-nuc-gateway-customized-module.git
   ```

   <span data-ttu-id="c6506-130">Det här är en inbyggd Azure Edge-modul som skrivits i programmeringsspråket C.</span><span class="sxs-lookup"><span data-stu-id="c6506-130">This is a native Azure Edge module written in the C programming language.</span></span> <span data-ttu-id="c6506-131">Modulen konverterar formatet för mottagna meddelanden till följande:</span><span class="sxs-lookup"><span data-stu-id="c6506-131">The module converts the format of received messages into the following one:</span></span>

   ```json
   {"deviceId": "Intel NUC Gateway", "messageId": 0, "temperature": 0.0}
   ```

## <a name="compile-the-module"></a><span data-ttu-id="c6506-132">Kompilera modulen</span><span class="sxs-lookup"><span data-stu-id="c6506-132">Compile the module</span></span>

<span data-ttu-id="c6506-133">Kompilera modulen genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="c6506-133">To compile the module, run the following commands:</span></span>

```bash
cd iot-hub-c-intel-nuc-gateway-customized-module/my_module
# change the build script runnable
chmod 777 build.sh
# remove the invalid windows character
sed -i -e "s/\r$//" build.sh
# run the build shell script
./build.sh
```

<span data-ttu-id="c6506-134">Du får en `libmy_module.so` filen när kompileringen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="c6506-134">You get a `libmy_module.so` file after the compile is completed.</span></span> <span data-ttu-id="c6506-135">Anteckna den absoluta sökvägen till den här filen.</span><span class="sxs-lookup"><span data-stu-id="c6506-135">Make a note of the absolute path of this file.</span></span>

## <a name="add-the-module-to-the-ble-sample-application"></a><span data-ttu-id="c6506-136">Lägga till modulen TIVERA exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="c6506-136">Add the module to the BLE sample application</span></span>

1. <span data-ttu-id="c6506-137">Gå till mappen exempel genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c6506-137">Go to the samples folder by running the following command:</span></span>

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples
   ```

1. <span data-ttu-id="c6506-138">Öppna konfigurationsfilen genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c6506-138">Open the configuration file by running the following command:</span></span>

   ```bash
   vi ble_gateway.json
   ```

1. <span data-ttu-id="c6506-139">Lägga till en modul genom att lägga till följande kod i `modules` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c6506-139">Add a module by inserting the following code to the `modules` section.</span></span>

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

1. <span data-ttu-id="c6506-140">Ersätt `[Your libmy_module.so path]` i koden med den absoluta sökvägen till libmy_module.so-fil.</span><span class="sxs-lookup"><span data-stu-id="c6506-140">Replace `[Your libmy_module.so path]` in the code with the absolute path of the libmy_module.so\` file.</span></span>
1. <span data-ttu-id="c6506-141">Ersätt Koden i den `links` avsnitt med följande:</span><span class="sxs-lookup"><span data-stu-id="c6506-141">Replace the code in the `links` section with the following one:</span></span>

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

1. <span data-ttu-id="c6506-142">Tryck på `ESC`, och skriv sedan `:wq` att spara filen.</span><span class="sxs-lookup"><span data-stu-id="c6506-142">Press `ESC`, and then type `:wq` to save the file.</span></span>

## <a name="run-the-sample-application"></a><span data-ttu-id="c6506-143">Kör exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="c6506-143">Run the sample application</span></span>

1. <span data-ttu-id="c6506-144">Starta SensorTag.</span><span class="sxs-lookup"><span data-stu-id="c6506-144">Power on the SensorTag.</span></span>
1. <span data-ttu-id="c6506-145">Ange miljövariabeln SSL_CERT_FILE genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c6506-145">Set the SSL_CERT_FILE environment variable by running the following command:</span></span>

   ```bash
   export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
   ```

1. <span data-ttu-id="c6506-146">Kör exempelprogrammet med modulen lagts till genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c6506-146">Run the sample application with the added module by running the following command:</span></span>

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a><span data-ttu-id="c6506-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c6506-147">Next steps</span></span>

<span data-ttu-id="c6506-148">Du har kunna använda IoT-gateway för att konvertera meddelandet från SensorTag till JSON-format.</span><span class="sxs-lookup"><span data-stu-id="c6506-148">You’ve successfully use the IoT gateway to convert the message from SensorTag into the .json format.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
