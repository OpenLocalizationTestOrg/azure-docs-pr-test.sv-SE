---
title: 'SensorTag enhet & Azure IoT Gateway - lektionen 1: Konfigurera Intel NUC | Microsoft Docs'
description: "Ställ in Intel NUC toowork som en IoT-gateway mellan en sensor och en sensorinformation för Azure IoT Hub toocollect och skicka den tooIoT hubb."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: IOT-gateway, intel nuc nuc dator, DE3815TYKE
ms.assetid: 917090d6-35c2-495b-a620-ca6f9c02b317
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 7c3ab3b014713c7facb86b8e8622d70e60a960e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a>Ställ in Intel NUC som en IoT-gateway
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

## <a name="what-you-will-do"></a>Vad du ska göra

- Ställ in Intel NUC som en IoT-gateway.
- Installera hello Azure IoT kanten på hello Intel NUC.
- Kör ett exempelprogram för ”hello_world” på hello Intel NUC tooverify hello gateway-funktioner.

  > Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Vad får du lära dig

I den här lektionen får du lära dig:

- Hur tooconnect Intel NUC med kringutrustning.
- Hur hello tooinstall och uppdatera hello krävs paket på Intel NUC med Smart Package Manager.
- Toorun hello ”hello_world” exempel på hur programmet tooverify hello gateway-funktioner.

## <a name="what-you-need"></a>Vad du behöver

- En Intel NUC Kit DE3815TYKE med hello Intel IoT Gateway-programsviten (vind floden Linux * 7.0.0.13) förinstallerat. [Klicka här toopurchase Groove IoT kommersiella Gateway Kit](https://www.seeedstudio.com/Grove-IoT-Commercial-Gateway-Kit-p-2724.html).
- En Ethernet-kabel.
- Ett tangentbord.
- HDMI eller VGA-kabel.
- En bildskärm med en HDMI eller VGA-port.
- Valfritt: [Texas Instruments Sensor Tag (CC2650STK)](http://www.ti.com/tool/cc2650stk)

![Gateway-Kit](media/iot-hub-gateway-kit-lessons/lesson1/kit.png)

## <a name="connect-intel-nuc-with-hello-peripherals"></a>Anslut Intel NUC med hello kringutrustning

hello bilden nedan är ett exempel på Intel NUC som är kopplad till olika kringutrustning:

1. Anslutna tooa tangentbordet.
2. Tooa bildskärm är ansluten till en VGA- eller HDMI kabel.
3. Anslutna tooa kabelanslutet nätverk med en Ethernet-kabel.
4. Anslutna tooa strömförsörjning med en power-kabel.

![Intel NUC anslutna tooperipherals](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-toohello-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a>Ansluta toohello Intel NUC system från värddatorn via SSH (Secure Shell)

Du behöver ett tangentbord och en bildskärm tooget hello IP-adress av Intel NUC enheten. Om du redan vet hello IP-adress, kan du hoppa över ahead toostep 3 i det här avsnittet.

1. Aktivera hello Intel NUC genom att trycka på strömknappen hello och logga sedan in.

   hello standardanvändarnamn och lösenord är båda `root`.

       > Hit hello enter key on your keyboard if you see either of hello following errors when you boot: 'A TPM error (7) occurred attempting tooread a pcr value.' or 'Timeout, No TPM chip found, activating TPM-bypass!'

2. Hämta hello IP-adressen för hello Intel NUC genom att köra hello `ifconfig` på hello Intel NUC enhet.

   Här är ett exempel på utdata från kommandot hello.

   ![ifconfig utdata visar Intel NUC IP](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   I det här exemplet hello värdet som följer `inet addr:` hello IP-adress som du behöver när ansluter toohello Intel NUC från en värddator.

3. Använd någon av följande SSH-klienter från din värd datorn tooconnect tooIntel NUC hello.

    - [PuTTY](http://www.putty.org/) för Windows.
    - hello inbyggda SSH-klienten på Ubuntu eller macOS.

   Det är mer effektivt och effektiv toooperate en Intel NUC från en värddator. Intel NUC IP-adress, kommer måste hello tooit för användare och lösenord tooconnect via en SSH-klient. Här är ett exempel som använder en SSH-klient på macOS.
   ![SSH-klienten körs på macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)

## <a name="install-hello-azure-iot-edge-package"></a>Installera hello Azure IoT kant paketet

hello Azure IoT kant paketet innehåller hello före kompilerade binärfilerna för IoT kant och dess beroenden. Dessa binärfiler är Azure IoT kant, hello Azure IoT-SDK och hello motsvarande verktyg. hello paketet innehåller också en ”hello_world” exempelprogrammet är används toovalidate hello gateway-funktioner. IoT-Edge tillhör hello core hello gateway. 

Följ dessa steg tooinstall hello-paketet.

1. Lägg till hello IoT moln databasen genom att köra följande kommandon i ett terminalfönster hello:

   ```bash
   rpm --import https://iotdk.intel.com/misc/iot_pub2.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
   ```

   > Ange ”y” när du uppmanas du too'Include den här kanalen ”?
   
   Om du får ett `import read failed(-1)` fel, Använd hello följande kommandon tooresolve hello problemet:
   ```bash
   wget http://iotdk.intel.com/misc/iot_pub2.key 
   rpm --import iot_pub2.key  
   ```

   Hej `rpm` kommandot import hello rpm-nyckel. Hej `smart channel` kommando lägger till hello rpm kanaler toohello Smart Package Manager. Innan du kör hello `smart update` kommandot visas utdata som nedan.

   ![rpm och smarta kanal kommandon utdata](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

2. Kör hello smart update-kommandot:

   ```bash
   smart update
   ```

3. Installera hello Azure IoT Gateway-paketet genom att köra följande kommando hello:

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   `packagegroup-cloud-azure`är hello namn hello-paketet. Hej `smart install` kommandot är används tooinstall hello paketet.

    > Hello kör följande kommando om du ser felet: 'offentlig nyckel som är inte tillgänglig ”

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```
    > Starta om hello Intel NUC om du ser felet: 'inga erbjuder util-linux-dev'

   När hello-paket installeras är Intel NUC klar toofunction som en gateway.

## <a name="run-hello-azure-iot-edge-helloworld-sample-application"></a>Köra hello Azure IoT kant ”hello_world” exempelprogrammet

hello följande exempelprogrammet skapar en gateway från en `hello_world.json` filen och använder hello grundläggande komponenter för Azure IoT kant arkitektur toolog en hello world tooa meddelandefil (log.txt) var femte sekund.

Du kan köra hello Hello World-exempel genom att köra följande kommandon hello:

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

Låt Hello World hello programmet kör några minuter och sedan trycker hello RETUR viktiga toostop den.
![programutdata](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)

> Du kan ignorera eventuella: Ogiltigt argument handle(NULL)'-fel som visas när du klickar på RETUR.

Du kan verifiera att hello-gateway har körts genom att öppna hello log.txt som är nu i mappen hello_world ![log.txt directory vy](media/iot-hub-gateway-kit-lessons/lesson1/logtxtdir.png)

Öppna log.txt med hello följande kommando:

```bash
vim log.txt
```

Sedan visas hello innehållet i log.txt som ska vara en JSON-formaterade utdata från hello loggningsmeddelanden som skrivits var femte sekund av Hello World hello gateway-modulen.
![log.txt directory vy](media/iot-hub-gateway-kit-lessons/lesson1/logtxtview.png)

Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="summary"></a>Sammanfattning

Grattis! Du har slutfört konfigurationen Intel NUC som en gateway. Nu är du redo toomove på toohello nästa lektionen tooset in värddatorn, skapa en Azure IoT-hubb och registrera din logiska Azure IoT Hub-enhet.

## <a name="next-steps"></a>Nästa steg
[Använda en enhet tooAzure IoT-hubb för tooconnect en IoT-gateway](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

