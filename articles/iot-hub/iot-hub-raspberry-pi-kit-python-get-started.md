---
title: aaaRaspberry Pi toocloud (Python) - ansluta hallon Pi tooAzure IoT-hubb | Microsoft Docs
description: "Lär dig hur toosetup och ansluta hallon Pi tooAzure IoT-hubb för hallon Pi toosend data toohello Azure-molnplattform i den här självstudiekursen."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Azure iot raspberry pi, raspberry pi iot hub, raspberry pi skicka data toocloud, raspberry pi toocloud
ms.service: iot-hub
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/31/2017
ms.author: xshi
ms.openlocfilehash: 86f5c91ab9dd4e23c563437827fb7d2d06916d2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-raspberry-pi-tooazure-iot-hub-python"></a>Ansluta hallon Pi tooAzure IoT-hubb (Python)

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

![Vad du behöver](media/iot-hub-raspberry-pi-kit-c-get-started/0_starter_kit.jpg)

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

## <a name="set-up-raspberry-pi"></a>Ställ in hallon Pi

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

   ![Hej Raspbian Inställningar-menyn](media/iot-hub-raspberry-pi-kit-c-get-started/1_raspbian-preferences-menu.png)

1. På hello **gränssnitt** ställer du in **I2C** och **SSH** för**aktivera**, och klicka sedan på **OK**. Om du inte har fysisk sensorer och vill toouse simulerade sensordata, är det här steget valfritt.

   ![Aktivera I2C och SSH på Raspberry Pi](media/iot-hub-raspberry-pi-kit-c-get-started/2_enable-spi-ssh-on-raspberry-pi.png)

> [!NOTE] 
tooenable SSH och I2C hittar du mer referensdokument på [raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) och [RASPI-CONFIG](https://www.raspberrypi.org/documentation/configuration/raspi-config.md).

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

### <a name="install-hello-prerequisite-packages"></a>Installera nödvändiga hello-paket

Använd någon av följande SSH-klienter från din värd datorn tooconnect tooyour hallon Pi hello.
   
   **Windows-användare**
   1. Hämta och installera [PuTTY](http://www.putty.org/) för Windows. 
   1. Kopiera hello IP-adressen för ditt avsnitt Pi till hello värden namn (eller IP-adress) och välj SSH som hello anslutningstypen.
   
   
   **Mac- och Ubuntu användare**
   
   Använd hello inbyggda SSH-klienten på Ubuntu eller macOS. Du kan behöva toorun `ssh pi@<ip address of pi>` tooconnect Pi via SSH.
   > [!NOTE] 
   hello Standardanvändarnamnet är `pi` , och hello lösenordet är `raspberry`.


### <a name="configure-hello-sample-application"></a>Konfigurera hello exempelprogrammet

1. Klona hello exempelprogrammet genom att köra följande kommando hello:

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-python-raspberrypi-client-app.git
   ```
1. Öppna hello config-fil genom att köra följande kommandon hello:

   ```bash
   cd iot-hub-python-raspberrypi-client-app
   nano config.py
   ```

   Det finns 5 makron i den här filen kan du configurate. hello först en är `MESSAGE_TIMESPAN`, som definierar hello tidsintervall (i millisekunder) mellan två meddelanden som skickar toocloud. Hej andra `SIMULATED_DATA`, vilket är ett booleskt värde för om toouse simulerade sensordata eller inte. `I2C_ADDRESS`är hello I2C-adress som BME280-sensor är ansluten. `GPIO_PIN_ADDRESS`är hello GPIO adressen för din Indikator. hello senast en är `BLINK_TIMESPAN`, som definierats hello timespan när din Indikator är aktiverat i millisekunder.

   Om du **saknar hello sensor**, Ställ in hello `SIMULATED_DATA` värdet för`True` toomake hello exempelprogrammet skapa och använda simulerade sensordata.

1. Spara och avsluta genom att trycka på CTRL-O > Ange > kontrollen X.

### <a name="build-and-run-hello-sample-application"></a>Skapa och köra hello exempelprogrammet

1. Skapa hello exempelprogrammet genom att köra följande kommando hello. Eftersom hello Azure IoT-SDK för Python omslutningar ovanpå hello Azure IoT-enhet C SDK kan behöver du toocompile hello C bibliotek om du vill eller behöver toogenerate hello Python-bibliotek från källkoden.

   ```bash
   sudo chmod u+x setup.sh
   sudo ./setup.sh
   ```
   > [!NOTE] 
   Du kan också ange hello-version som du vill använda genom att köra `sudo ./setup.sh [--python-version|-p] [2.7|3.4|3.5]`. Om du kör skriptet utan parametern hello skriptet identifierar hello-versionen av python som installeras automatiskt (Sökordning 2.7 -> 3.4 3.5 ->). Kontrollera att din version av Python håller konsekvent under Skapa och köra. 
   
   > [!NOTE] 
   På bygger hello Python-klientbiblioteket (iothub_client.so) på Linux-enheter som har mindre än 1GB RAM-minne, kan du se Skapa fastnar 98% när du skapar iothub_client_python.cpp enligt nedan `[ 98%] Building CXX object python/src/CMakeFiles/iothub_client_python.dir/iothub_client_python.cpp.o`. Om du stöter på problemet Kontrollera hello minnesförbrukning hello enheter med hjälp av `free -m command` i en annan terminalfönster under den tiden. Om du kör slut på minne vid kompilering iothub_client_python.cpp filen kanske tootemporarily öka hello växlingen utrymme tooget mer tillgängligt minne toosuccessfully skapa hello Python klientsidan enheten SDK-biblioteket.
   
1. Kör exempelprogrammet hello genom att köra följande kommando hello:

   ```bash
   python app.py '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   Kontrollera att du kopiera / klistra in anslutningssträngen för hello enhet till hello enkla citattecken. Och om du använder hello python 3 och du kan använda hello kommandot `python3 app.py '<your Azure IoT hub device connection string>'`.


   Du bör se hello följande utdata som visar hello sensor data och hello meddelanden som skickas tooyour IoT-hubb.

   ![Utdata - sensordata som skickas från hallon Pi tooyour IoT-hubb](media/iot-hub-raspberry-pi-kit-c-get-started/success.png
)

## <a name="next-steps"></a>Nästa steg

Du har kört ett program toocollect sensor exempeldata och skicka den tooyour IoT-hubb. toosee hälsningsmeddelande som din hallon Pi skickats tooyour IoT-hubb eller skicka meddelanden tooyour hallon Pi i ett kommandoradsgränssnitt finns hello [hantera moln enhet meddelanden med iothub explorer kursen](https://docs.microsoft.com/en-gb/azure/iot-hub/iot-hub-explorer-cloud-device-messaging).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
