---
title: aaaSimulate en enhet med Azure IoT kant (Linux) | Microsoft Docs
description: "Hur toouse Azure IoT kanten på Linux toocreate en simulerad enhet som skickar telemetri via en IoT-kant gateway tooan IoT-hubb."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 11e7bf28-ee3d-48d6-a386-eb506c7a31cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: andbuc
ms.openlocfilehash: 168fb8eda8671d02c63073bdf36dfcd88b397fe2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-iot-edge-toosend-device-to-cloud-messages-with-a-simulated-device-linux"></a><span data-ttu-id="81317-103">Använda Azure IoT kant toosend meddelanden från enhet till moln med en simulerad enhet (Linux)</span><span class="sxs-lookup"><span data-stu-id="81317-103">Use Azure IoT Edge toosend device-to-cloud messages with a simulated device (Linux)</span></span>

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-toorun-hello-sample"></a><span data-ttu-id="81317-104">Hur toorun hello exempel</span><span class="sxs-lookup"><span data-stu-id="81317-104">How toorun hello sample</span></span>

<span data-ttu-id="81317-105">Hej **build.sh** skriptet genererar utdata i hello **skapa** mapp i den lokala kopian av hello **iot-edge** databasen.</span><span class="sxs-lookup"><span data-stu-id="81317-105">hello **build.sh** script generates its output in hello **build** folder in your local copy of hello **iot-edge** repository.</span></span> <span data-ttu-id="81317-106">Här ingår hello fyra IoT kant moduler som används i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="81317-106">This output includes hello four IoT Edge modules used in this sample.</span></span>

<span data-ttu-id="81317-107">Hej build skript platser på:</span><span class="sxs-lookup"><span data-stu-id="81317-107">hello build script places the:</span></span>

* <span data-ttu-id="81317-108">**liblogger.so** i hello **build-moduler-logg** mapp.</span><span class="sxs-lookup"><span data-stu-id="81317-108">**liblogger.so** in hello **build/modules/logger** folder.</span></span>
* <span data-ttu-id="81317-109">**libiothub.so** i hello **iothub-build/moduler** mapp.</span><span class="sxs-lookup"><span data-stu-id="81317-109">**libiothub.so** in hello **build/modules/iothub** folder.</span></span>
* <span data-ttu-id="81317-110">**lib\_identitet\_map.so** i hello **identitymap-build/moduler** mapp.</span><span class="sxs-lookup"><span data-stu-id="81317-110">**lib\_identity\_map.so** in hello **build/modules/identitymap** folder.</span></span>
* <span data-ttu-id="81317-111">**libsimulated\_device.so** i hello **build/moduler/simulerade\_enhet** mapp.</span><span class="sxs-lookup"><span data-stu-id="81317-111">**libsimulated\_device.so** in hello **build/modules/simulated\_device** folder.</span></span>

<span data-ttu-id="81317-112">Använd dessa sökvägar för hello **modulen sökvägen** värden som visas i följande JSON-inställningsfilen hello:</span><span class="sxs-lookup"><span data-stu-id="81317-112">Use these paths for hello **module path** values as shown in hello following JSON settings file:</span></span>

<span data-ttu-id="81317-113">hello simulerade\_enhet\_moln\_överför\_exempel processen tar hello sökvägen tooa JSON-konfigurationsfil som kommandoradsargument.</span><span class="sxs-lookup"><span data-stu-id="81317-113">hello simulated\_device\_cloud\_upload\_sample process takes hello path tooa JSON configuration file as a command-line argument.</span></span> <span data-ttu-id="81317-114">hello följande exempel JSON-fil har angetts i hello SDK lagringsplats på **exempel\\simulerade\_enhet\_moln\_överför\_exempel\\src\\ simulerade\_enhet\_moln\_överför\_exempel\_lin.json**.</span><span class="sxs-lookup"><span data-stu-id="81317-114">hello following example JSON file is provided in hello SDK repository at **samples\\simulated\_device\_cloud\_upload\_sample\\src\\simulated\_device\_cloud\_upload\_sample\_lin.json**.</span></span> <span data-ttu-id="81317-115">Den här konfigurationen filen fungerar som om du inte ändrar hello skapa skript tooplace hello IoT kant moduler eller exempel körbara filer i icke-standardplatser.</span><span class="sxs-lookup"><span data-stu-id="81317-115">This configuration file works as is unless you modify hello build script tooplace hello IoT Edge modules or sample executables in non-default locations.</span></span>

> [!NOTE]
> <span data-ttu-id="81317-116">hello modulen sökvägar är relativ toohello katalog från där du kör hello simulerade\_enhet\_moln\_överför\_exempel körbar fil, inte hello katalogen där hello körbara filen finns.</span><span class="sxs-lookup"><span data-stu-id="81317-116">hello module paths are relative toohello directory from where you run hello simulated\_device\_cloud\_upload\_sample executable, not hello directory where hello executable is located.</span></span> <span data-ttu-id="81317-117">hello exempel JSON-konfigurationsfil som standard toowriting too'deviceCloudUploadGatewaylog.log' i den aktuella arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="81317-117">hello sample JSON configuration file defaults toowriting too'deviceCloudUploadGatewaylog.log' in your current working directory.</span></span>

<span data-ttu-id="81317-118">Öppna hello-filen i en textredigerare, **exempel/simulerade\_enhet\_moln\_överför\_exempel/src/simulerade\_enhet\_moln\_överför\_lin.json** i den lokala kopian av hello **iot-edge** databasen.</span><span class="sxs-lookup"><span data-stu-id="81317-118">In a text editor, open hello file **samples/simulated\_device\_cloud\_upload\_sample/src/simulated\_device\_cloud\_upload\_lin.json** in your local copy of hello **iot-edge** repository.</span></span> <span data-ttu-id="81317-119">Den här filen konfigurerar hello IoT kant moduler i hello exempel gateway:</span><span class="sxs-lookup"><span data-stu-id="81317-119">This file configures hello IoT Edge modules in hello sample gateway:</span></span>

* <span data-ttu-id="81317-120">Hej **IoTHub** modulen ansluter tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="81317-120">hello **IoTHub** module connects tooyour IoT hub.</span></span> <span data-ttu-id="81317-121">Du konfigurerar den toosend data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="81317-121">You configure it toosend data tooyour IoT hub.</span></span> <span data-ttu-id="81317-122">Mer specifikt ange hello **IoTHubName** värdet toohello namnet på din IoT-hubb och ange hello **IoTHubSuffix** värdet för**azure devices.net**.</span><span class="sxs-lookup"><span data-stu-id="81317-122">Specifically, set hello **IoTHubName** value toohello name of your IoT hub and set hello **IoTHubSuffix** value too**azure-devices.net**.</span></span> <span data-ttu-id="81317-123">Ange hello **Transport** värdet tooone av: **HTTP**, **AMQP**, eller **MQTT**.</span><span class="sxs-lookup"><span data-stu-id="81317-123">Set hello **Transport** value tooone of: **HTTP**, **AMQP**, or **MQTT**.</span></span> <span data-ttu-id="81317-124">För närvarande endast **HTTP** delar en TCP-anslutning för alla meddelanden på enheten.</span><span class="sxs-lookup"><span data-stu-id="81317-124">Currently, only **HTTP** shares one TCP connection for all device messages.</span></span> <span data-ttu-id="81317-125">Om du anger hello värde för**AMQP**, eller **MQTT**, hello gateway upprätthåller en separat TCP-anslutning tooIoT hubb för varje enhet.</span><span class="sxs-lookup"><span data-stu-id="81317-125">If you set hello value too**AMQP**, or **MQTT**, hello gateway maintains a separate TCP connection tooIoT Hub for each device.</span></span>
* <span data-ttu-id="81317-126">Hej **mappning** modulen mappar hello MAC-adresserna för dina simulerade enheterna tooyour IoT-hubb enhets-ID.</span><span class="sxs-lookup"><span data-stu-id="81317-126">hello **mapping** module maps hello MAC addresses of your simulated devices tooyour IoT Hub device ids.</span></span> <span data-ttu-id="81317-127">Se till att **deviceId** värden matchar hello-ID för hello två enheter som du lagt till tooyour IoT-hubb och den hello **deviceKey** värden innehåller hello nycklar två enheter.</span><span class="sxs-lookup"><span data-stu-id="81317-127">Make sure that **deviceId** values match hello ids of hello two devices you added tooyour IoT hub, and that hello **deviceKey** values contain hello keys of your two devices.</span></span>
* <span data-ttu-id="81317-128">Hej **BLE1** och **BLE2** moduler är hello simulerade enheter.</span><span class="sxs-lookup"><span data-stu-id="81317-128">hello **BLE1** and **BLE2** modules are hello simulated devices.</span></span> <span data-ttu-id="81317-129">Observera hur sina MAC-adresser matchar hello-adresser i hello **mappning** modul.</span><span class="sxs-lookup"><span data-stu-id="81317-129">Note how their MAC addresses match hello addresses in hello **mapping** module.</span></span>
* <span data-ttu-id="81317-130">Hej **loggaren** modulen loggar din gateway aktiviteten tooa-fil.</span><span class="sxs-lookup"><span data-stu-id="81317-130">hello **Logger** module logs your gateway activity tooa file.</span></span>
* <span data-ttu-id="81317-131">Hej **modulen sökvägen** värden som visas i hello exemplet förutsätter att du kör hello exempel från hello **skapa** mapp i den lokala kopian av hello **iot-edge** databasen.</span><span class="sxs-lookup"><span data-stu-id="81317-131">hello **module path** values shown in hello example assume that you run hello sample from hello **build** folder in your local copy of hello **iot-edge** repository.</span></span>
* <span data-ttu-id="81317-132">Hej **länkar** matris längst hello hello JSON-filen ansluter hello **BLE1** och **BLE2** moduler toohello **mappning** modulen och hello **mappning** modulen toohello **IoTHub** modul.</span><span class="sxs-lookup"><span data-stu-id="81317-132">hello **links** array at hello bottom of hello JSON file connects hello **BLE1** and **BLE2** modules toohello **mapping** module, and hello **mapping** module toohello **IoTHub** module.</span></span> <span data-ttu-id="81317-133">Det innebär också att alla meddelanden loggas av hello **loggaren** modul.</span><span class="sxs-lookup"><span data-stu-id="81317-133">It also ensures that all messages are logged by hello **Logger** module.</span></span>

```json
{
    "modules": [
        {
            "name": "IotHub",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/iothub/libiothub.so"
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
              "module.path": "./modules/identitymap/libidentity_map.so"
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
              "module.path": "./modules/simulated_device/libsimulated_device.so"
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
              "module.path": "./modules/simulated_device/libsimulated_device.so"
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
              "module.path": "./modules/logger/liblogger.so"
            }
            },
            "args": {
              "filename": "deviceCloudUploadGatewaylog.log"
            }
          }
    ],
    "links": [
        {
            "source": "*",
            "sink": "Logger"
        },
        {
            "source": "BLE1",
            "sink": "mapping"
        },
        {
            "source": "BLE2",
            "sink": "mapping"
        },
        {
            "source": "mapping",
            "sink": "IotHub"
        }
    ]
}
```

<span data-ttu-id="81317-134">Spara hello ändringar gjort du toohello konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="81317-134">Save hello changes you made toohello configuration file.</span></span>

<span data-ttu-id="81317-135">toorun hello-exempel:</span><span class="sxs-lookup"><span data-stu-id="81317-135">toorun hello sample:</span></span>

1. <span data-ttu-id="81317-136">Navigera i gränssnittet, toohello **build-iot-edge** mapp.</span><span class="sxs-lookup"><span data-stu-id="81317-136">In your shell, navigate toohello **iot-edge/build** folder.</span></span>
2. <span data-ttu-id="81317-137">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="81317-137">Run hello following command:</span></span>
   
    ```sh
    ./samples/simulated_device_cloud_upload/simulated_device_cloud_upload_sample ../samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json
    ```
3. <span data-ttu-id="81317-138">Du kan använda hello [enheten explorer] [ lnk-device-explorer] eller [iothub explorer] [ lnk-iothub-explorer] toomonitor hälsningsmeddelande som IoT-hubb som tar emot från hello-verktyget gateway.</span><span class="sxs-lookup"><span data-stu-id="81317-138">You can use hello [device explorer][lnk-device-explorer] or [iothub-explorer][lnk-iothub-explorer] tool toomonitor hello messages that IoT hub receives from hello gateway.</span></span> <span data-ttu-id="81317-139">Till exempel kan Utforskaren iothub du övervaka meddelanden från enhet till moln med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="81317-139">For example, using iothub-explorer you can monitor device-to-cloud messages using hello following command:</span></span>

    ```sh
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a><span data-ttu-id="81317-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="81317-140">Next steps</span></span>

<span data-ttu-id="81317-141">toogain en större kunskap om Azure IoT kant och experiment med vissa kodexempel finns hello följande developer självstudier och resurser:</span><span class="sxs-lookup"><span data-stu-id="81317-141">toogain a more advanced understanding of Azure IoT Edge and experiment with some code examples, visit hello following developer tutorials and resources:</span></span>

* <span data-ttu-id="81317-142">[Skicka meddelanden från enhet till moln från en fysisk enhet med Azure IoT kant][lnk-physical-device]</span><span class="sxs-lookup"><span data-stu-id="81317-142">[Send device-to-cloud messages from a physical device with Azure IoT Edge][lnk-physical-device]</span></span>
* <span data-ttu-id="81317-143">[Azure IoT kant][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="81317-143">[Azure IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="81317-144">toofurther utforska hello funktionerna i IoT Hub, se:</span><span class="sxs-lookup"><span data-stu-id="81317-144">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="81317-145">[Utvecklarhandbok för IoT-hubb][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="81317-145">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="81317-146">[Skydda din IoT-lösning från hello bakgrund][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="81317-146">[Secure your IoT solution from hello ground up][lnk-securing]</span></span>

<!-- Links -->
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-physical-device]: iot-hub-iot-edge-physical-device.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
