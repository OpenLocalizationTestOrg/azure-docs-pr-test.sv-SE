---
title: "aaaRaspberry Pi toocloud (Node.js) – ansluta hallon Pi tooAzure IoT-hubb | Microsoft Docs"
description: "Lär dig hur toosetup och ansluta hallon Pi tooAzure IoT-hubb för hallon Pi toosend data toohello Azure-molnplattform i den här självstudiekursen."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Azure iot raspberry pi, raspberry pi iot hub, raspberry pi skicka data toocloud, raspberry pi toocloud
ms.assetid: b0e14bfa-8e64-440a-a6ec-e507ca0f76ba
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/27/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 57a15ab3984021a9c18ff0aa1316a4d4c6ebdec1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-raspberry-pi-tooazure-iot-hub-nodejs"></a>Ansluta hallon Pi tooAzure IoT-hubb (Node.js)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

I den här kursen kan du börja genom att lära dig hello grunderna i att arbeta med hallon Pi med Raspbian. Du lär dig hur tooseamlessly ansluta enheter toohello molnet med hjälp av [Azure IoT Hub](iot-hub-what-is-iot-hub.md). För Windows 10 IoT Core prover gå toohello [Windows Dev Center](http://www.windowsondevices.com/).

Har du en kit ännu? Försök [hallon Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md). Eller köp en ny kit [här](https://azure.microsoft.com/develop/iot/starter-kits).


## <a name="what-you-do"></a>Vad du gör

* Skapa en IoT-hubb.
* Registrera en enhet för Pi i din IoT-hubb.
* Konfigurera Raspberry Pi.
* Kör ett exempelprogram på Pi toosend sensor data tooyour IoT-hubb.

Ansluta hallon Pi tooan IoT-hubb som du skapar. Sedan kör du ett exempelprogram på Pi toocollect temperatur- och fuktighetskonsekvens data från en BME280 sensor. Slutligen kan skicka du hello sensor data tooyour IoT-hubb.

## <a name="what-you-learn"></a>Detta får du får lära dig

* Hur toocreate en Azure IoT-hubb och få nya enheten anslutningssträngen.
* Hur tooconnect Pi med en BME280 sensor.
* Hur toocollect sensordata genom att köra ett exempelprogram på Pi.
* Hur toosend sensor data tooyour IoT-hubb.

## <a name="what-you-need"></a>Vad du behöver

![Vad du behöver](media/iot-hub-raspberry-pi-kit-node-get-started/0_starter_kit.jpg)

* hello hallon Pi 2 eller 3 för hallon Pi.
* En aktiv Azure-prenumeration. Om du inte har ett Azure-konto [skapa ett kostnadsfritt Azure konto](https://azure.microsoft.com/free/) i bara några minuter.
* En Övervakare, en USB-tangentbord och mus som ansluter tooPi.
* En Mac- eller en dator som kör Windows eller Linux.
* En Internetanslutning.
* 16 GB eller högre microSD-kort.
* En USB-SD-kort eller microSD-kort tooburn hello operativsystemavbildning på hello microSD-kort.
* En v 5-2-amp elkälla med hello 6 fotavtryck micro USB-kabel.

hello följande objekt är valfria:

* En monterad Adafruit BME280 temperatur, tryck och fuktighet sensorn.
* En breadboard.
* 6-F/M omkopplare kablar.
* En indirekt Indikator för 10 mm.


> [!NOTE] 
Objekten är valfritt eftersom hello kod exempel support simulerade sensordata.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-raspberry-pi"></a>Konfigurera Raspberry Pi

### <a name="install-hello-raspbian-operating-system-for-pi"></a>Installera operativsystemet för hello Raspbian för Pi

Förbered hello microSD-kort för installation av hello Raspbian bild.

1. Hämta Raspbian.
   1. [Hämta Raspbian Jessie med skrivbordet](https://www.raspberrypi.org/downloads/raspbian/) (hello ZIP-fil).
   1. Extrahera hello Raspbian bild tooa mapp på datorn.
1. Installera Raspbian toohello microSD-kort.
   1. [Hämta och installera hello Etcher SD-kort brännare verktyget](https://etcher.io/).
   1. Kör Etcher och välj hello Raspbian avbildning som du extraherade i steg 1.
   1. Välj enhet för hello microSD-kort. Observera att Etcher kanske redan har valt hello rätt enhet.
   1. Klicka på Flash tooinstall Raspbian toohello microSD-kort.
   1. Ta bort hello microSD-kort från datorn när installationen är klar. Det är säker tooremove hello microSD-kort direkt eftersom Etcher automatiskt matar ut eller demonterar hello microSD-kort när åtgärden har slutförts.
   1. Infoga hello microSD-kort i Pi.

### <a name="enable-ssh-and-i2c"></a>Aktivera SSH och I2C

1. Ansluta Pi toohello bildskärm, tangentbord och mus, starta Pi och logga sedan in Raspbian med hjälp av `pi` som hello användarnamn och `raspberry` som hello lösenord.
1. Klicka på hello Raspberry ikonen > **inställningar** > **hallon Pi Configuration**.

   ![Hej Raspbian Inställningar-menyn](media/iot-hub-raspberry-pi-kit-node-get-started/1_raspbian-preferences-menu.png)

1. På hello **gränssnitt** ställer du in **I2C** och **SSH** för**aktivera**, och klicka sedan på **OK**. Om du inte har fysisk sensorer och vill toouse simulerade sensordata, är det här steget valfritt.

   ![Aktivera I2C och SSH på Raspberry Pi](media/iot-hub-raspberry-pi-kit-node-get-started/2_enable-i2c-ssh-on-raspberry-pi.png)

> [!NOTE] 
tooenable SSH och I2C hittar du mer referensdokument på [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) och [Adafruit.com](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).

### <a name="connect-hello-sensor-toopi"></a>Ansluta hello sensor tooPi

Använd hello breadboard och omkopplare kablar tooconnect en Indikator och en BME280 tooPi på följande sätt. Om du inte har hello sensor [hoppa över det här avsnittet](#connect-pi-to-the-network).

![hello hallon Pi och sensor-anslutning](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)

Hej BME280 sensor kan samla in data för temperatur- och fuktighetskonsekvens. Och hello Indikator blinkar om det finns en kommunikation mellan enheten och hello moln. 

För sensor PIN-koder, använder du följande kablar hello:

| Starta (Sensor & Indikator)     | END (moderkort)            | Kabel färg   |
| -----------------------  | ---------------------- | ------------: |
| VDD (PIN-kod 5G)             | 3, 3v PWR (PIN-kod 1)       | Vita kabeln   |
| JORD (PIN-kod 7G)             | JORD (PIN-kod 6)            | Bruna kabel   |
| SDI (PIN-kod 10G)            | I2C1 SDA (PIN-kod 3)       | Röd kabel     |
| SCK (PIN-kod 8G)             | I2C1 SCL (PIN-kod 5)       | Orange kabel  |
| Indikator VDD (PIN-kod 18F)        | GPIO 24 (PIN-kod 18)       | Vita kabeln   |
| Indikator jord (PIN-kod 17F)        | JORD (PIN-kod 20)           | Svart kabel   |

Klicka på tooview [hallon Pi 2 och 3 PIN-kod mappningar](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) referens.

När du har anslutit BME280 tooyour hallon Pi, ska den som under bilden.

![Anslutna Pi och BME280](media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-toohello-network"></a>Ansluta Pi toohello nätverk

Aktivera Pi med hjälp av hello micro USB-kabel och hello strömförsörjning. Använd hello Ethernet-kabel tooconnect Pi tooyour kabelanslutna nätverk eller följa hello [instruktioner från hello hallon Pi Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/) tooconnect Pi tooyour trådlöst nätverk. När din Pi har varit ansluten toohello nätverk, måste tootake anteckna hello [IP-adressen för din Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address).

![Anslutna toowired nätverk](media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> Se till att Pi är anslutna toohello samma nätverk som datorn. Till exempel om datorn är ansluten tooa trådlösa nätverk medan Pi är anslutna tooa kabelanslutet nätverk kan du kanske inte se hello IP-adress i hello devdisco utdata.

## <a name="run-a-sample-application-on-pi"></a>Kör ett exempelprogram på Pi

### <a name="clone-sample-application-and-install-hello-prerequisite-packages"></a>Klona exempelprogrammet och installera nödvändiga hello-paket

1. Använd någon av följande SSH-klienter från din värd datorn tooconnect tooyour hallon Pi hello.
   
   **Windows-användare**
   1. Hämta och installera [PuTTY](http://www.putty.org/) för Windows. 
   1. Kopiera hello IP-adressen för ditt avsnitt Pi till hello värden namn (eller IP-adress) och välj SSH som hello anslutningstypen.
   
   ![PuTTy](media/iot-hub-raspberry-pi-kit-node-get-started/7_putty-windows.png)
   
   **Mac- och Ubuntu användare**
   
   Använd hello inbyggda SSH-klienten på Ubuntu eller macOS. Du kan behöva toorun `ssh pi@<ip address of pi>` tooconnect Pi via SSH.
   > [!NOTE] 
   hello Standardanvändarnamnet är `pi` , och hello lösenordet är `raspberry`.

1. Installera Node.js och NPM tooyour Pi.
   
   Först bör du kontrollera din Node.js-version med hello följande kommando. 
   
   ```bash
   node -v
   ```

   Om hello-versionen är lägre än 4.x eller om det finns inga Node.js på din Pi, kör följande kommando tooinstall hello eller uppdatera Node.js.

   ```bash
   curl -sL http://deb.nodesource.com/setup_4.x | sudo -E bash
   sudo apt-get -y install nodejs
   ```

1. Klona hello exempelprogrammet genom att köra följande kommando hello:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-client-app
   ```

1. Installera alla paket med följande kommando hello. Det innehåller Azure IoT-enhet SDK, BME280 Sensor bibliotek och kabeldragning Pi-biblioteket.

   ```bash
   cd iot-hub-node-raspberrypi-client-app
   sudo npm install
   ```
   > [!NOTE] 
   Det kan ta flera minuter toofinish denna installationsprocess beroende på nätverksanslutningen.

### <a name="configure-hello-sample-application"></a>Konfigurera hello exempelprogrammet

1. Öppna hello config-fil genom att köra följande kommandon hello:

   ```bash
   nano config.json
   ```

   ![Config-fil](media/iot-hub-raspberry-pi-kit-node-get-started/6_config-file.png)

   Det finns två objekt i den här filen kan du configurate. hello först en är `interval`, som definierar hello tidsintervall (i millisekunder) mellan två meddelanden som skickar toocloud. Hej andra `simulatedData`, vilket är ett booleskt värde för om toouse simulerade sensordata eller inte.

   Om du **saknar hello sensor**, Ställ in hello `simulatedData` värdet för`true` toomake hello exempelprogrammet skapa och använda simulerade sensordata.

1. Spara och avsluta genom att trycka på CTRL-O > Ange > kontrollen X.

### <a name="run-hello-sample-application"></a>Köra hello exempelprogrammet

Kör exempelprogrammet hello genom att köra följande kommando hello:

   ```bash
   sudo node index.js '<YOUR AZURE IOT HUB DEVICE CONNECTION STRING>'
   ```

   > [!NOTE] 
   Kontrollera att du kopiera / klistra in anslutningssträngen för hello enhet till hello enkla citattecken.


Du bör se hello följande utdata som visar hello sensor data och hello meddelanden som skickas tooyour IoT-hubb.

![Utdata - sensordata som skickas från hallon Pi tooyour IoT-hubb](media/iot-hub-raspberry-pi-kit-node-get-started/8_run-output.png)

## <a name="next-steps"></a>Nästa steg

Du har kört ett program toocollect sensor exempeldata och skicka den tooyour IoT-hubb. toosee hälsningsmeddelande som din hallon Pi skickats tooyour IoT-hubb eller skicka meddelanden tooyour hallon Pi i ett kommandoradsgränssnitt finns hello [hantera moln enhet meddelanden med iothub explorer kursen](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
