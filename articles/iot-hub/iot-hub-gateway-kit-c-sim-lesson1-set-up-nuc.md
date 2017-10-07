---
title: 'Simulerade enhet & Azure IoT Gateway - lektionen 1: Konfigurera NUC | Microsoft Docs'
description: "Ställ in Intel NUC toowork som en IoT-gateway mellan en sensor och en sensorinformation för Azure IoT Hub toocollect och skicka den tooIoT hubb."
services: iot-hub
documentationcenter: 
author: shizn
manager: yjianfeng
tags: 
keywords: IOT-gateway, intel nuc nuc dator, DE3815TYKE
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: f41d6b2e-9b00-40df-90eb-17d824bea883
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c2146c7cd8ca54adbd0af279364ec8484be1b45b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a>Ställ in Intel NUC som en IoT-gateway

## <a name="what-you-will-do"></a>Vad du ska göra

- Ställ in Intel NUC som en IoT-gateway.
- Installera hello Azure IoT kanten på Intel NUC.
- Kör ett exempelprogram för ”hello_world” på Intel NUC tooverify hello gateway-funktioner.
Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig

I den här lektionen får du lära dig:

- Hur tooconnect Intel NUC med kringutrustning.
- Hur hello tooinstall och uppdatera hello krävs paket på Intel NUC med Smart Package Manager.
- Toorun hello ”hello_world” exempel på hur programmet tooverify hello gateway-funktioner.

## <a name="what-you-need"></a>Vad du behöver

- En Intel NUC Kit DE3815TYKE med hello Intel IoT Gateway-programsviten (vind floden Linux * 7.0.0.13) förinstallerat.
- En Ethernet-kabel.
- Ett tangentbord.
- HDMI eller VGA-kabel.
- En bildskärm med en HDMI eller VGA-port.

![Gateway-Kit](media/iot-hub-gateway-kit-lessons/lesson1/kit_without_sensortag.png)

## <a name="connect-intel-nuc-with-hello-peripherals"></a>Anslut Intel NUC med hello kringutrustning

hello bilden nedan är ett exempel på Intel NUC som är kopplad till olika kringutrustning:

1. Anslutna tooa tangentbordet.
2. Ansluten toohello Övervakare av en VGA- eller HDMI kabel.
3. Anslutna tooa kabelanslutna nätverket av en Ethernet-kabel.
4. Anslutna toohello strömförsörjning med en power-kabel.

![Intel NUC anslutna tooperipherals](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-toohello-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a>Ansluta toohello Intel NUC system från värddatorn via SSH (Secure Shell)

Du måste här tangentbord och bildskärm tooget hello IP-adressen till enheten NUC. Om du redan vet hello IP-adress, kan du hoppa över toostep 3 i det här avsnittet.

1. Aktivera Intel NUC genom att trycka på strömknappen hello och loggfiler i hello system.

   hello standardanvändarnamn och lösenord är båda `root`.

2. Hämta hello IP-adressen för NUC genom att köra hello `ifconfig` kommando. Detta görs på hello NUC enhet.

   Här är ett exempel på utdata från kommandot hello.

   ![ifconfig utdata visar NUC IP](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   I det här exemplet hello värdet som följer `inet addr:` hello IP-adress som du behöver när du planerar tooconnect via en fjärranslutning från en värd datorn tooIntel NUC.

3. Använd någon av följande SSH-klienter från din värd datorn tooconnect tooIntel NUC hello.

   - [PuTTY](http://www.putty.org/) för Windows.
   - hello inbyggda SSH-klienten på Ubuntu eller macOS.

   Det är mer effektivt och effektiv toooperate på Intel NUC från en värddator. Du behöver hello hello IP-adress, användarnamn och lösenord tooconnect hello NUC via SSH-klienten. Här är hello exempel Använd SSH-klienten på macOS.
   ![SSH-klienten körs på macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)

## <a name="install-hello-azure-iot-edge-package"></a>Installera hello Azure IoT kant paketet

hello Azure IoT kant paketet innehåller hello före kompilerade binärfilerna för hello SDK och dess beroenden. Dessa binärfiler är Azure IoT kant, hello Azure IoT-SDK och hello motsvarande verktyg. hello-paketet innehåller också ett exempelprogram för ”hello_world” som har använt toovalidate hello gateway-funktioner. IoT-Edge tillhör hello core hello gateway. tooinstall hello paketet, gör du följande:

1. Lägg till hello IoT moln databasen genom att köra följande kommandon i ett terminalfönster hello:

   ```bash
   rpm --import http://iotdk.intel.com/misc/iot_pub.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   ```

   Hej `rpm` kommandot import hello rpm-nyckel. Hej `smart channel` kommando lägger till hello rpm kanaler toohello Smart Package Manager. Innan du kör hello `smart update` kommando, se utdata som liknar nedan.

   ![rpm och smarta kanal kommandon utdata](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

   ```bash
   smart update
   ```

2. Installera hello paketet genom att köra följande kommando hello:

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   `packagegroup-cloud-azure`är hello namn hello-paketet. Hej `smart install` kommandot är används tooinstall hello paketet.

   När hello-paket installeras är Intel NUC förväntade toowork som en gateway.

## <a name="run-hello-azure-iot-edge-helloworld-sample-application"></a>Köra hello Azure IoT kant ”hello_world” exempelprogrammet

Gå för`azureiotgatewaysdk/samples` och köra hello exempelprogrammet ”hello_world” exempel. Det här exempelprogrammet skapar en gateway från hello `hello_world.json` filen och använder hello grundläggande komponenter för hello Azure IoT kant arkitektur toolog en hello world tooa meddelandefilen 5 sekunder.

Du kan köra hello exempelprogrammet ”hello_world” exempel genom att köra följande kommando hello:

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

hello exempelprogrammet ger hello följande utdata om hello gateway fungerar korrekt:

![programutdata](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)

Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="summary"></a>Sammanfattning

Grattis! Du har slutfört konfigurationen Intel NUC som en gateway. Nu är du redo toomove på toohello nästa lektionen tooset datorn värden skapar en Azure IoT-hubb och registrera dina Azure IoT hub-logisk enhet.

## <a name="next-steps"></a>Nästa steg
[Förbereda din värddatorn och Azure IoT-hubb](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
