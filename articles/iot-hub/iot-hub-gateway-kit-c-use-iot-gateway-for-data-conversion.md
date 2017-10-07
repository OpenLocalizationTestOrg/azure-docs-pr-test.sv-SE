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
# <a name="use-iot-gateway-for-sensor-data-transformation-with-azure-iot-edge"></a>Använd IoT-gateway för omvandling av sensor data med Azure IoT kant

> [!NOTE]
> Innan du börjar den här självstudiekursen, se har till att du slutfört hello följande erfarenheter i ordning:
> * [Konfigurera Intel NUC som en IoT-gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
> * [Använd IoT gateway tooconnect saker toohello cloud - SensorTag tooAzure IoT-hubb](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

En syftet med en Iot-gateway är tooprocess insamlade data innan du skickar den toohello moln. Azure IoT-Edge introducerar moduler som skapats och monterade tooform hello databehandling i arbetsflöden. En modul tar emot ett meddelande, utför en åtgärd på den och flytta den på för andra moduler tooprocess.

## <a name="what-you-learn"></a>Detta får du får lära dig

Du lär dig hur toocreate en modul tooconvert meddelanden från hello SensorTag till ett annat format.

## <a name="what-you-do"></a>Vad du gör

* Skapa en modul tooconvert ett mottaget meddelande till hello JSON-format.
* Kompilera hello-modulen.
* Lägg till hello modulen toohello TIVERA exempelprogrammet från Azure IoT kant.
* Köra hello exempelprogrammet.

## <a name="what-you-need"></a>Vad du behöver

* följande kurser som slutförts i sekvens hello:
  * [Konfigurera Intel NUC som en IoT-gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
  * [Använd IoT gateway tooconnect saker toohello cloud - SensorTag tooAzure IoT-hubb](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)
* En SSH-klient som körs på värddatorn. PuTTY rekommenderas i Windows. Linux- och macOS har redan en SSH-klient.
* hello IP-adress och hello användarnamn och lösenord tooaccess hello gateway från hello SSH-klienten.
* En Internetanslutning.

## <a name="create-a-module"></a>Skapa en modul

1. Kör hello SSH-klienten på värddatorn för hello och ansluta toohello IoT gateway.
1. Klona hello källfiler för hello konvertering modul från GitHub toohello-hemkataloger hello IoT gateway genom att köra följande kommandon hello:

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-c-intel-nuc-gateway-customized-module.git
   ```

   Det här är en inbyggd Azure Edge-modul som skrivits i hello programmeringsspråket C. hello modulen konverterar hello format för mottagna meddelanden till hello efter:

   ```json
   {"deviceId": "Intel NUC Gateway", "messageId": 0, "temperature": 0.0}
   ```

## <a name="compile-hello-module"></a>Kompilera hello-modul

toocompile hello modul kör hello följande kommandon:

```bash
cd iot-hub-c-intel-nuc-gateway-customized-module/my_module
# change hello build script runnable
chmod 777 build.sh
# remove hello invalid windows character
sed -i -e "s/\r$//" build.sh
# run hello build shell script
./build.sh
```

Du får en `libmy_module.so` filen när hello kompilera har slutförts. Anteckna hello absoluta sökvägen till den här filen.

## <a name="add-hello-module-toohello-ble-sample-application"></a>Lägg till hello modulen toohello TIVERA exempelprogrammet

1. Gå toohello exempelmapp genom att köra följande kommando hello:

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples
   ```

1. Öppna hello konfigurationsfilen genom att köra följande kommando hello:

   ```bash
   vi ble_gateway.json
   ```

1. Lägga till en modul genom att lägga till följande kod toohello hello `modules` avsnitt.

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

1. Ersätt `[Your libmy_module.so path]` i hello kod med hello absoluta sökvägen till hello libmy_module.so-filen.
1. Ersätt hello koden i hello `links` avsnitt med hello efter:

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

1. Tryck på `ESC`, och skriv sedan `:wq` toosave hello-filen.

## <a name="run-hello-sample-application"></a>Köra hello exempelprogrammet

1. Starta hello SensorTag.
1. Ange hello SSL_CERT_FILE miljövariabeln genom att köra följande kommando hello:

   ```bash
   export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
   ```

1. Kör exempelprogrammet hello med hello tillagda modulen genom att köra följande kommando hello:

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a>Nästa steg

Nu har du Använd hello IoT gateway tooconvert hello-meddelande från SensorTag till hello JSON-format.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
