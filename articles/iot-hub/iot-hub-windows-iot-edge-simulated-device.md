---
title: Simulera en enhet med Azure IoT kant (Windows) | Microsoft Docs
description: "Hur du använder Azure IoT kant i Windows för att skapa en simulerad enhet som skickar telemetri via en gateway för Azure IoT kant till en IoT-hubb."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 6a2aeda0-696a-4732-90e1-595d2e2fadc6
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: andbuc
ms.openlocfilehash: e7eb2931993daf3f0aecbd4a43d27ebd5adc10b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-iot-edge-to-send-device-to-cloud-messages-with-a-simulated-device-windows"></a><span data-ttu-id="5ebd4-103">Använd Azure IoT Edge att skicka meddelanden från enhet till moln med en simulerad enhet (Windows)</span><span class="sxs-lookup"><span data-stu-id="5ebd4-103">Use Azure IoT Edge to send device-to-cloud messages with a simulated device (Windows)</span></span>

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="how-to-run-the-sample"></a><span data-ttu-id="5ebd4-104">Köra exemplet</span><span class="sxs-lookup"><span data-stu-id="5ebd4-104">How to run the sample</span></span>

<span data-ttu-id="5ebd4-105">Den **build.cmd** skriptet genererar utdata i den **skapa** mapp i den lokala kopian av den **iot-edge** databasen.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-105">The **build.cmd** script generates its output in the **build** folder in your local copy of the **iot-edge** repository.</span></span> <span data-ttu-id="5ebd4-106">Här ingår fyra IoT kant moduler som används i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-106">This output includes the four IoT Edge modules used in this sample.</span></span>

<span data-ttu-id="5ebd4-107">Skapa skript platser på:</span><span class="sxs-lookup"><span data-stu-id="5ebd4-107">The build script places the:</span></span>

* <span data-ttu-id="5ebd4-108">**Logger.dll** i den **skapa\\moduler\\loggaren\\felsöka** mapp.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-108">**logger.dll** in the **build\\modules\\logger\\Debug** folder.</span></span>
* <span data-ttu-id="5ebd4-109">**iothub.dll** i den **skapa\\moduler\\iothub\\felsöka** mapp.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-109">**iothub.dll** in the **build\\modules\\iothub\\Debug** folder.</span></span>
* <span data-ttu-id="5ebd4-110">**Identity\_map.dll** i den **skapa\\moduler\\identitymap\\felsöka** mapp.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-110">**identity\_map.dll** in the **build\\modules\\identitymap\\Debug** folder.</span></span>
* <span data-ttu-id="5ebd4-111">**simulerade\_device.dll** i den **skapa\\moduler\\simulerade\_enhet\\felsöka** mapp.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-111">**simulated\_device.dll** in the **build\\modules\\simulated\_device\\Debug** folder.</span></span>

<span data-ttu-id="5ebd4-112">Använd dessa sökvägar för den **modulen sökvägen** värden som visas i JSON-filen för följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="5ebd4-112">Use these paths for the **module path** values as shown in the following JSON settings file:</span></span>

<span data-ttu-id="5ebd4-113">Den simulerade\_enhet\_moln\_överför\_exempel processen tar sökvägen till en JSON-konfigurationsfil som kommandoradsargument.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-113">The simulated\_device\_cloud\_upload\_sample process takes the path to a JSON configuration file as a command-line argument.</span></span> <span data-ttu-id="5ebd4-114">Följande exempel JSON-filen finns i SDK-databasen på **exempel\\simulerade\_enhet\_moln\_överför\_exempel\\src\\ simulerade\_enhet\_moln\_överför\_exempel\_win.json**.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-114">The following example JSON file is provided in the SDK repository at **samples\\simulated\_device\_cloud\_upload\_sample\\src\\simulated\_device\_cloud\_upload\_sample\_win.json**.</span></span> <span data-ttu-id="5ebd4-115">Den här konfigurationsfilen fungerar som om du inte ändrar build-skript för att placera IoT kant moduler eller exempel körbara filer i icke-standardplatser.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-115">This configuration file works as is unless you modify the build script to place the IoT Edge modules or sample executables in non-default locations.</span></span>

> [!NOTE]
> <span data-ttu-id="5ebd4-116">Modul-sökvägar är i förhållande till katalogen där den simulerade\_enhet\_moln\_överför\_sample.exe finns.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-116">The module paths are relative to the directory where the simulated\_device\_cloud\_upload\_sample.exe is located.</span></span> <span data-ttu-id="5ebd4-117">Exempel JSON-konfigurationsfil som standard skriver till 'deviceCloudUploadGatewaylog.log' i den aktuella arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-117">The sample JSON configuration file defaults to writing to 'deviceCloudUploadGatewaylog.log' in your current working directory.</span></span>

<span data-ttu-id="5ebd4-118">Öppna filen i en textredigerare, **exempel\\simulerade\_enhet\_moln\_överför\_exempel\\src\\simulerade\_enhet\_moln\_överför\_win.json** i den lokala kopian av den **iot-edge** databasen.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-118">In a text editor, open the file **samples\\simulated\_device\_cloud\_upload\_sample\\src\\simulated\_device\_cloud\_upload\_win.json** in your local copy of the **iot-edge** repository.</span></span> <span data-ttu-id="5ebd4-119">Den här filen konfigurerar IoT kant-moduler i exempel-gateway:</span><span class="sxs-lookup"><span data-stu-id="5ebd4-119">This file configures the IoT Edge modules in the sample gateway:</span></span>

* <span data-ttu-id="5ebd4-120">Den **IoTHub** modulen ansluter till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-120">The **IoTHub** module connects to your IoT hub.</span></span> <span data-ttu-id="5ebd4-121">Du konfigurerar den för att skicka data till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-121">You configure it to send data to your IoT hub.</span></span> <span data-ttu-id="5ebd4-122">Mer specifikt kan ange den **IoTHubName** värde till namnet på din IoT-hubb och ange den **IoTHubSuffix** värde till **azure devices.net**.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-122">Specifically, set the **IoTHubName** value to the name of your IoT hub and set the **IoTHubSuffix** value to **azure-devices.net**.</span></span> <span data-ttu-id="5ebd4-123">Ange den **Transport** något av: **HTTP**, **AMQP**, eller **MQTT**.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-123">Set the **Transport** value to one of: **HTTP**, **AMQP**, or **MQTT**.</span></span> <span data-ttu-id="5ebd4-124">För närvarande endast **HTTP** delar en TCP-anslutning för alla meddelanden på enheten.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-124">Currently, only **HTTP** shares one TCP connection for all device messages.</span></span> <span data-ttu-id="5ebd4-125">Om du anger värdet till **AMQP**, eller **MQTT**, gatewayen underhåller en separat TCP-anslutning till IoT-hubb för varje enhet.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-125">If you set the value to **AMQP**, or **MQTT**, the gateway maintains a separate TCP connection to IoT Hub for each device.</span></span>
* <span data-ttu-id="5ebd4-126">Den **mappning** modulen mappar MAC-adresserna för dina simulerade enheter till din IoT-hubb enhets-ID.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-126">The **mapping** module maps the MAC addresses of your simulated devices to your IoT Hub device ids.</span></span> <span data-ttu-id="5ebd4-127">Se till att **deviceId** värdena matchar ID-numren för de två enheterna som du lagt till din IoT-hubb och att den **deviceKey** värden innehåller nycklar två enheter.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-127">Make sure that **deviceId** values match the ids of the two devices you added to your IoT hub, and that the **deviceKey** values contain the keys of your two devices.</span></span>
* <span data-ttu-id="5ebd4-128">Den **BLE1** och **BLE2** moduler är simulerade enheterna.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-128">The **BLE1** and **BLE2** modules are the simulated devices.</span></span> <span data-ttu-id="5ebd4-129">Observera hur modulen MAC-adresserna matchar adresserna i den **mappning** modul.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-129">Note how the module MAC addresses match the addresses in the **mapping** module.</span></span>
* <span data-ttu-id="5ebd4-130">Den **loggaren** modulen loggar gateway-aktiviteten till en fil.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-130">The **Logger** module logs your gateway activity to a file.</span></span>
* <span data-ttu-id="5ebd4-131">Den **modulen sökvägen** värden som visas i följande exempel är i förhållande till katalogen där den simulerade\_enhet\_moln\_överför\_sample.exe finns.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-131">The **module path** values shown in the following example are relative to the directory where the simulated\_device\_cloud\_upload\_sample.exe is located.</span></span>
* <span data-ttu-id="5ebd4-132">Den **länkar** matris längst ned i JSON-filen ansluter den **BLE1** och **BLE2** -moduler i **mappning** modulen, och **mappning** modulen till den **IoTHub** modulen.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-132">The **links** array at the bottom of the JSON file connects the **BLE1** and **BLE2** modules to the **mapping** module, and the **mapping** module to the **IoTHub** module.</span></span> <span data-ttu-id="5ebd4-133">Det innebär också att alla meddelanden loggas av den **loggaren** modul.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-133">It also ensures that all messages are logged by the **Logger** module.</span></span>

```json
{
    "modules" :
    [
      {
        "name": "IotHub",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\iothub\\Debug\\iothub.dll"
          }
          },
          "args": {
            "IoTHubName": "<<insert here IoTHubName>>",
            "IoTHubSuffix": "<<insert here IoTHubSuffix>>",
            "Transport": "HTTP"
          }
        },
      {
        "name": "mapping",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\identitymap\\Debug\\identity_map.dll"
          }
          },
          "args": [
            {
              "macAddress": "01:01:01:01:01:01",
              "deviceId": "<<insert here deviceId>>",
              "deviceKey": "<<insert here deviceKey>>"
            },
            {
              "macAddress": "02:02:02:02:02:02",
              "deviceId": "<<insert here deviceId>>",
              "deviceKey": "<<insert here deviceKey>>"
            }
          ]
        },
      {
        "name": "BLE1",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "01:01:01:01:01:01"
          }
        },
      {
        "name": "BLE2",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "02:02:02:02:02:02"
          }
        },
      {
        "name": "Logger",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\logger\\Debug\\logger.dll"
          }
        },
        "args": {
          "filename": "deviceCloudUploadGatewaylog.log"
        }
      }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IotHub" }
    ]
}
```

<span data-ttu-id="5ebd4-134">Spara ändringarna i konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-134">Save the changes you made to the configuration file.</span></span>

<span data-ttu-id="5ebd4-135">Att köra exemplet:</span><span class="sxs-lookup"><span data-stu-id="5ebd4-135">To run the sample:</span></span>

1. <span data-ttu-id="5ebd4-136">Vid en kommandotolk, navigerar du till den **skapa** mapp i den lokala kopian av den **iot-edge** databasen.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-136">At a command prompt, navigate to the **build** folder in your local copy of the **iot-edge** repository.</span></span>
2. <span data-ttu-id="5ebd4-137">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5ebd4-137">Run the following command:</span></span>
   
    ```cmd
    samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe ..\samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```
3. <span data-ttu-id="5ebd4-138">Du kan använda den [enheten explorer] [ lnk-device-explorer] eller [iothub explorer] [ lnk-iothub-explorer] verktyg för att övervaka de meddelanden som IoT-hubb som tar emot från gatewayen.</span><span class="sxs-lookup"><span data-stu-id="5ebd4-138">You can use the [device explorer][lnk-device-explorer] or [iothub-explorer][lnk-iothub-explorer] tool to monitor the messages that IoT hub receives from the gateway.</span></span> <span data-ttu-id="5ebd4-139">Till exempel kan Utforskaren iothub du övervaka meddelanden från enhet till moln med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5ebd4-139">For example, using iothub-explorer you can monitor device-to-cloud messages using the following command:</span></span>

    ```cmd
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a><span data-ttu-id="5ebd4-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5ebd4-140">Next steps</span></span>

<span data-ttu-id="5ebd4-141">För att få större kunskap om IoT kant och experimentera med vissa kodexempel, finns följande kurser för utvecklare och resurser:</span><span class="sxs-lookup"><span data-stu-id="5ebd4-141">To gain a more advanced understanding of IoT Edge and experiment with some code examples, visit the following developer tutorials and resources:</span></span>

* <span data-ttu-id="5ebd4-142">[Skicka meddelanden från enhet till moln från en fysisk enhet med IoT kant][lnk-physical-device]</span><span class="sxs-lookup"><span data-stu-id="5ebd4-142">[Send device-to-cloud messages from a physical device with IoT Edge][lnk-physical-device]</span></span>
* <span data-ttu-id="5ebd4-143">[Azure IoT kant][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="5ebd4-143">[Azure IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="5ebd4-144">Om du vill utforska ytterligare funktionerna i IoT-hubb, se:</span><span class="sxs-lookup"><span data-stu-id="5ebd4-144">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="5ebd4-145">[Utvecklarhandbok för IoT-hubb][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="5ebd4-145">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="5ebd4-146">[Skydda din IoT-lösning från grunden upp][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="5ebd4-146">[Secure your IoT solution from the ground up][lnk-securing]</span></span>

<!-- Links -->
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-physical-device]: iot-hub-iot-edge-physical-device.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md