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
# <a name="use-azure-iot-edge-to-send-device-to-cloud-messages-with-a-simulated-device-windows"></a>Använd Azure IoT Edge att skicka meddelanden från enhet till moln med en simulerad enhet (Windows)

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="how-to-run-the-sample"></a>Köra exemplet

Den **build.cmd** skriptet genererar utdata i den **skapa** mapp i den lokala kopian av den **iot-edge** databasen. Här ingår fyra IoT kant moduler som används i det här exemplet.

Skapa skript platser på:

* **Logger.dll** i den **skapa\\moduler\\loggaren\\felsöka** mapp.
* **iothub.dll** i den **skapa\\moduler\\iothub\\felsöka** mapp.
* **Identity\_map.dll** i den **skapa\\moduler\\identitymap\\felsöka** mapp.
* **simulerade\_device.dll** i den **skapa\\moduler\\simulerade\_enhet\\felsöka** mapp.

Använd dessa sökvägar för den **modulen sökvägen** värden som visas i JSON-filen för följande inställningar:

Den simulerade\_enhet\_moln\_överför\_exempel processen tar sökvägen till en JSON-konfigurationsfil som kommandoradsargument. Följande exempel JSON-filen finns i SDK-databasen på **exempel\\simulerade\_enhet\_moln\_överför\_exempel\\src\\ simulerade\_enhet\_moln\_överför\_exempel\_win.json**. Den här konfigurationsfilen fungerar som om du inte ändrar build-skript för att placera IoT kant moduler eller exempel körbara filer i icke-standardplatser.

> [!NOTE]
> Modul-sökvägar är i förhållande till katalogen där den simulerade\_enhet\_moln\_överför\_sample.exe finns. Exempel JSON-konfigurationsfil som standard skriver till 'deviceCloudUploadGatewaylog.log' i den aktuella arbetskatalogen.

Öppna filen i en textredigerare, **exempel\\simulerade\_enhet\_moln\_överför\_exempel\\src\\simulerade\_enhet\_moln\_överför\_win.json** i den lokala kopian av den **iot-edge** databasen. Den här filen konfigurerar IoT kant-moduler i exempel-gateway:

* Den **IoTHub** modulen ansluter till din IoT-hubb. Du konfigurerar den för att skicka data till din IoT-hubb. Mer specifikt kan ange den **IoTHubName** värde till namnet på din IoT-hubb och ange den **IoTHubSuffix** värde till **azure devices.net**. Ange den **Transport** något av: **HTTP**, **AMQP**, eller **MQTT**. För närvarande endast **HTTP** delar en TCP-anslutning för alla meddelanden på enheten. Om du anger värdet till **AMQP**, eller **MQTT**, gatewayen underhåller en separat TCP-anslutning till IoT-hubb för varje enhet.
* Den **mappning** modulen mappar MAC-adresserna för dina simulerade enheter till din IoT-hubb enhets-ID. Se till att **deviceId** värdena matchar ID-numren för de två enheterna som du lagt till din IoT-hubb och att den **deviceKey** värden innehåller nycklar två enheter.
* Den **BLE1** och **BLE2** moduler är simulerade enheterna. Observera hur modulen MAC-adresserna matchar adresserna i den **mappning** modul.
* Den **loggaren** modulen loggar gateway-aktiviteten till en fil.
* Den **modulen sökvägen** värden som visas i följande exempel är i förhållande till katalogen där den simulerade\_enhet\_moln\_överför\_sample.exe finns.
* Den **länkar** matris längst ned i JSON-filen ansluter den **BLE1** och **BLE2** -moduler i **mappning** modulen, och **mappning** modulen till den **IoTHub** modulen. Det innebär också att alla meddelanden loggas av den **loggaren** modul.

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

Spara ändringarna i konfigurationsfilen.

Att köra exemplet:

1. Vid en kommandotolk, navigerar du till den **skapa** mapp i den lokala kopian av den **iot-edge** databasen.
2. Kör följande kommando:
   
    ```cmd
    samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe ..\samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```
3. Du kan använda den [enheten explorer] [ lnk-device-explorer] eller [iothub explorer] [ lnk-iothub-explorer] verktyg för att övervaka de meddelanden som IoT-hubb som tar emot från gatewayen. Till exempel kan Utforskaren iothub du övervaka meddelanden från enhet till moln med följande kommando:

    ```cmd
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a>Nästa steg

För att få större kunskap om IoT kant och experimentera med vissa kodexempel, finns följande kurser för utvecklare och resurser:

* [Skicka meddelanden från enhet till moln från en fysisk enhet med IoT kant][lnk-physical-device]
* [Azure IoT kant][lnk-iot-edge]

Om du vill utforska ytterligare funktionerna i IoT-hubb, se:

* [Utvecklarhandbok för IoT-hubb][lnk-devguide]
* [Skydda din IoT-lösning från grunden upp][lnk-securing]

<!-- Links -->
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-physical-device]: iot-hub-iot-edge-physical-device.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md