---
title: aaaUse en IoT-gateway tooconnect enhet-tooAzure IoT-hubb | Microsoft Docs
description: "Lär dig hur toouse Intel NUC som en IoT-gateway tooconnect en TI SensorTag och skicka sensor data tooAzure IoT-hubb i hello cloud."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: IOT-gatewayen ansluta enheten toocloud
ms.assetid: cb851648-018c-4a7e-860f-b62ed3b493a5
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2017
ms.author: xshi
ms.openlocfilehash: 418af34bf29992d46b76ae59ef548744808664c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-iot-gateway-tooconnect-things-toohello-cloud---sensortag-tooazure-iot-hub"></a>Använd IoT gateway tooconnect saker toohello cloud - SensorTag tooAzure IoT-hubb

> [!NOTE]
> Innan du börjar den här självstudiekursen, kontrollera att du har slutfört [konfigurera Intel NUC som en IoT-gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md). I [konfigurera Intel NUC som en IoT-gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md), du ställa in hello Intel NUC enhet som en IoT-gateway.

## <a name="what-you-will-learn"></a>Vad får du lära dig

Du lär dig hur toouse en Texas instrument SensorTag (CC2650STK) tooAzure IoT-hubb för tooconnect en IoT-gateway. Hej IoT gateway skickar temperatur och fuktighet data som samlas in från hello SensorTag tooAzure IoT-hubb.

## <a name="what-you-will-do"></a>Vad du ska göra

- Skapa en IoT-hubb.
- Registrera en enhet i hello IoT-hubb för hello SensorTag.
- Aktivera hello-anslutning mellan hello IoT-gateway och hello SensorTag.
- Kör en TIVERA exempel programmet toosend SensorTag data tooyour IoT-hubb.

## <a name="what-you-need"></a>Vad du behöver

- Kursen [konfigurera Intel NUC som en IoT-gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) slutförts i som du konfigurerar Intel NUC som en IoT-gateway.
- * En aktiv Azure-prenumeration. Om du inte har ett Azure-konto [skapa ett kostnadsfritt Azure konto](https://azure.microsoft.com/free/) i bara några minuter.
- En SSH-klient som körs på värddatorn. PuTTY rekommenderas i Windows. Linux- och macOS har redan en SSH-klient.
- hello IP-adress och hello användarnamn och lösenord tooaccess hello gateway från hello SSH-klienten.
- En Internetanslutning.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

> [!NOTE]
> Här registrerar du enheten för dina SensorTag

## <a name="enable-hello-connection-between-hello-iot-gateway-and-hello-sensortag"></a>Aktivera hello anslutning mellan hello IoT-gateway och hello SensorTag

I det här avsnittet kan du utföra följande uppgifter hello:

- Hämta hello MAC-adressen för hello SensorTag för Bluetooth-anslutning.
- Initiera en Bluetooth-anslutning från hello IoT gateway toohello SensorTag.

### <a name="get-hello-mac-address-of-hello-sensortag-for-bluetooth-connection"></a>Hämta hello MAC-adressen för hello SensorTag för Bluetooth-anslutning

1. Kör hello SSH-klienten på värddatorn för hello och ansluta toohello IoT gateway.
1. Tillåt Bluetooth genom att köra följande kommando hello:

   ```bash
   sudo rfkill unblock bluetooth
   ```

1. Starta hello Bluetooth-tjänsten på hello IoT gateway och ange en Bluetooth shell tooconfigure Bluetooth genom att köra hello följande kommandon:

   ```bash
   sudo systemctl start bluetooth
   bluetoothctl
   ```

1. Slå på hello Bluetooth domänkontrollant genom att köra hello följande kommando på hello Bluetooth shell:

   ```bash
   power on
   ```

   ![Slå på hello Bluetooth-styrenheten på hello IoT-gateway med bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/8_power-on-bluetooth-controller-at-bluetooth-shell-bluetoothctl.png)

1. Starta sökning efter närliggande Bluetooth-enheter genom att köra följande kommando hello:

   ```bash
   scan on
   ```

   ![Skanna Närliggande Bluetooth-enheter med bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/9_start-scan-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

1. Tryck på hello länkning hello SensorTag-knappen. hello grön LETT på hello SensorTag gånger.
1. Du bör se hello SensorTag hittas på hello Bluetooth-gränssnittet. Anteckna hello hello SensorTag MAC-adress. I det här exemplet hello MAC-adressen för hello SensorTag är `24:71:89:C0:7F:82`.
1. Inaktivera hello genomsökning genom att köra följande kommando hello:

   ```bash
   scan off
   ```

   ![Stoppa inläsning Närliggande Bluetooth-enheter med bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/10_stop-scanning-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

### <a name="initiate-a-bluetooth-connection-from-hello-iot-gateway-toohello-sensortag"></a>Initiera en bluetoothanslutning från hello IoT gateway toohello SensorTag

1. Anslut toohello SensorTag genom att köra följande kommando hello:

   ```bash
   connect <MAC address>
   ```

   ![Anslut toohello SensorTag med bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/11_connect-to-sensortag-at-bluetooth-shell-bluetoothctl.png)

1. Koppla från hello SensorTag och avsluta hello Bluetooth-gränssnittet genom att köra följande kommandon hello:

   ```bash
   disconnect
   exit
   ```

   ![Koppla från hello SensorTag med bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/12_disconnect-from-sensortag-at-bluetooth-shell-bluetoothctl.png)

Du har aktiverat hello anslutning mellan hello SensorTag och hello IoT-gateway.

## <a name="run-a-ble-sample-application-toosend-sensortag-data-tooyour-iot-hub"></a>Kör en TIVERA exempel programmet toosend SensorTag data tooyour IoT-hubb

hello exempelprogrammet Bluetooth låg energiförbrukning (TIVERA) tillhandahålls av Azure IoT kant. hello exempelprogrammet samlar in data från TIVERA anslutning och skicka hello data tooyou IoT-hubb. toorun hello exempelprogrammet, måste du:

1. Konfigurera hello exempelprogrammet.
1. Kör exempelprogrammet hello på hello IoT gateway.

### <a name="configure-hello-sample-application"></a>Konfigurera hello exempelprogrammet

1. Gå toohello mapp hello exempelprogrammet genom att köra följande kommando hello:

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples/ble_gateway
   ```

1. Öppna hello konfigurationsfilen genom att köra följande kommando hello:

   ```bash
   vi ble_gateway.json
   ```

1. Fyll i hello följande värden i hello konfigurationsfil:

   **IoTHubName**: hello namnet på din IoT-hubb.

   **IoTHubSuffix**: hämta IoTHubSuffix från hello primärnyckeln i anslutningssträngen för hello enheten som du antecknade ned. Se till att du hämta hello primär nyckel för anslutningssträngen för hello enheten inte hello primärnyckel för IoT-hubb anslutningssträngen. hello primärnyckeln för anslutningssträngen för hello enheten är i hello-format för `HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY`.

   **Transport**: hello standardvärdet är `amqp`. Det här värdet visar hello protokollet under transpotation. Det kan vara `http`, `amqp`, eller `mqtt`.

   **macAddress**: hello hello SensorTag som du antecknade ned MAC-adress.

   **deviceID**: ID för hello-enhet som du skapade i din IoT-hubb.

   **deviceKey**: hello primärnyckeln i anslutningssträngen för hello enhet.

   ![Fullständig hello konfigurationsfilen för hello TIVERA exempelprogrammet](./media/iot-hub-iot-gateway-connect-device-to-cloud/13_edit-config-file-of-ble-sample.png)

1. Tryck på `ESC` och skriv `:wq` toosave hello-filen.

### <a name="run-hello-sample-application"></a>Köra hello exempelprogrammet

1. Se till att hello SensorTag är påslagen.
1. Kör följande kommando hello:

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a>Nästa steg

[Använd IoT-gateway för omvandling av sensor data med Azure IoT kant](iot-hub-gateway-kit-c-use-iot-gateway-for-data-conversion.md)
