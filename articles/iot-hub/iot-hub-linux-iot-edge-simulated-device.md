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
# <a name="use-azure-iot-edge-toosend-device-to-cloud-messages-with-a-simulated-device-linux"></a>Använda Azure IoT kant toosend meddelanden från enhet till moln med en simulerad enhet (Linux)

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-toorun-hello-sample"></a>Hur toorun hello exempel

Hej **build.sh** skriptet genererar utdata i hello **skapa** mapp i den lokala kopian av hello **iot-edge** databasen. Här ingår hello fyra IoT kant moduler som används i det här exemplet.

Hej build skript platser på:

* **liblogger.so** i hello **build-moduler-logg** mapp.
* **libiothub.so** i hello **iothub-build/moduler** mapp.
* **lib\_identitet\_map.so** i hello **identitymap-build/moduler** mapp.
* **libsimulated\_device.so** i hello **build/moduler/simulerade\_enhet** mapp.

Använd dessa sökvägar för hello **modulen sökvägen** värden som visas i följande JSON-inställningsfilen hello:

hello simulerade\_enhet\_moln\_överför\_exempel processen tar hello sökvägen tooa JSON-konfigurationsfil som kommandoradsargument. hello följande exempel JSON-fil har angetts i hello SDK lagringsplats på **exempel\\simulerade\_enhet\_moln\_överför\_exempel\\src\\ simulerade\_enhet\_moln\_överför\_exempel\_lin.json**. Den här konfigurationen filen fungerar som om du inte ändrar hello skapa skript tooplace hello IoT kant moduler eller exempel körbara filer i icke-standardplatser.

> [!NOTE]
> hello modulen sökvägar är relativ toohello katalog från där du kör hello simulerade\_enhet\_moln\_överför\_exempel körbar fil, inte hello katalogen där hello körbara filen finns. hello exempel JSON-konfigurationsfil som standard toowriting too'deviceCloudUploadGatewaylog.log' i den aktuella arbetskatalogen.

Öppna hello-filen i en textredigerare, **exempel/simulerade\_enhet\_moln\_överför\_exempel/src/simulerade\_enhet\_moln\_överför\_lin.json** i den lokala kopian av hello **iot-edge** databasen. Den här filen konfigurerar hello IoT kant moduler i hello exempel gateway:

* Hej **IoTHub** modulen ansluter tooyour IoT-hubb. Du konfigurerar den toosend data tooyour IoT-hubb. Mer specifikt ange hello **IoTHubName** värdet toohello namnet på din IoT-hubb och ange hello **IoTHubSuffix** värdet för**azure devices.net**. Ange hello **Transport** värdet tooone av: **HTTP**, **AMQP**, eller **MQTT**. För närvarande endast **HTTP** delar en TCP-anslutning för alla meddelanden på enheten. Om du anger hello värde för**AMQP**, eller **MQTT**, hello gateway upprätthåller en separat TCP-anslutning tooIoT hubb för varje enhet.
* Hej **mappning** modulen mappar hello MAC-adresserna för dina simulerade enheterna tooyour IoT-hubb enhets-ID. Se till att **deviceId** värden matchar hello-ID för hello två enheter som du lagt till tooyour IoT-hubb och den hello **deviceKey** värden innehåller hello nycklar två enheter.
* Hej **BLE1** och **BLE2** moduler är hello simulerade enheter. Observera hur sina MAC-adresser matchar hello-adresser i hello **mappning** modul.
* Hej **loggaren** modulen loggar din gateway aktiviteten tooa-fil.
* Hej **modulen sökvägen** värden som visas i hello exemplet förutsätter att du kör hello exempel från hello **skapa** mapp i den lokala kopian av hello **iot-edge** databasen.
* Hej **länkar** matris längst hello hello JSON-filen ansluter hello **BLE1** och **BLE2** moduler toohello **mappning** modulen och hello **mappning** modulen toohello **IoTHub** modul. Det innebär också att alla meddelanden loggas av hello **loggaren** modul.

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

Spara hello ändringar gjort du toohello konfigurationsfilen.

toorun hello-exempel:

1. Navigera i gränssnittet, toohello **build-iot-edge** mapp.
2. Kör följande kommando hello:
   
    ```sh
    ./samples/simulated_device_cloud_upload/simulated_device_cloud_upload_sample ../samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json
    ```
3. Du kan använda hello [enheten explorer] [ lnk-device-explorer] eller [iothub explorer] [ lnk-iothub-explorer] toomonitor hälsningsmeddelande som IoT-hubb som tar emot från hello-verktyget gateway. Till exempel kan Utforskaren iothub du övervaka meddelanden från enhet till moln med hello följande kommando:

    ```sh
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a>Nästa steg

toogain en större kunskap om Azure IoT kant och experiment med vissa kodexempel finns hello följande developer självstudier och resurser:

* [Skicka meddelanden från enhet till moln från en fysisk enhet med Azure IoT kant][lnk-physical-device]
* [Azure IoT kant][lnk-iot-edge]

toofurther utforska hello funktionerna i IoT Hub, se:

* [Utvecklarhandbok för IoT-hubb][lnk-devguide]
* [Skydda din IoT-lösning från hello bakgrund][lnk-securing]

<!-- Links -->
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-physical-device]: iot-hub-iot-edge-physical-device.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
