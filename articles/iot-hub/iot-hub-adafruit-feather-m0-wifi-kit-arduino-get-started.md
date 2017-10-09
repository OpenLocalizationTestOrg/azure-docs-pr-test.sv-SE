---
title: 'M0 toocloud: ansluta ludd M0 WiFi tooAzure IoT-hubb | Microsoft Docs'
description: "Lär dig hur tooset upp och ansluta Adafruit ludd M0 WiFi tooAzure IoT-hubb toosend data toohello Azure cloud-plattformen i den här självstudiekursen."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.assetid: 51befcdb-332b-416f-a6a1-8aabdb67f283
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/16/2017
ms.author: xshi
ms.openlocfilehash: 6aabeb961a50ba5d3934f77eb1ccda4af1bf64c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-adafruit-feather-m0-wifi-tooazure-iot-hub-in-hello-cloud"></a>Ansluta Adafruit ludd M0 WiFi tooAzure IoT-hubb i hello moln
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Anslutning mellan en BME280 och ludd M0 WiFi IoT-hubb](media/iot-hub-adafruit-feather-m0-wifi-get-started/1_connection-m0-feather-m0-iot-hub.png)

I den här självstudiekursen börjar du med learning hello grunderna i att arbeta med Arduino-kort. Du lär dig hur tooseamlessly ansluta enheter toohello molnet med hjälp av [Azure IoT Hub](iot-hub-what-is-iot-hub.md).

## <a name="what-you-do"></a>Vad du gör

Ansluta Adafruit ludd M0 WiFi tooan IoT-hubb som du skapar. Sedan kör du ett exempelprogram på M0 WiFi toocollect hello temperatur- och fuktighetskonsekvens data från en BME280. Slutligen kan skicka du hello sensor data tooyour IoT-hubb.


## <a name="what-you-learn"></a>Detta får du får lära dig

* Hur toocreate en IoT-hubb och registrera en enhet för Ludd M0 WiFi
* Hur tooconnect ludd M0 WiFi med hello sensor och datorn
* Hur toocollect sensordata genom att köra ett exempelprogram på ludd M0 WiFi
* Hur toosend hello sensor data tooyour IoT-hubb

## <a name="what-you-need"></a>Vad du behöver

![delar som behövs för hello självstudiekursen](media/iot-hub-adafruit-feather-m0-wifi-get-started/2_parts-needed-for-the-tutorial.png)

toocomplete den här åtgärden måste hello följande delar från din startpaket för Ludd M0 Wi-Fi:

* hello ludd M0 WiFi-kort
* Micro USB-tooType en USB-kabel

Du måste också följande saker för din utvecklingsmiljö hello:

* En aktiv Azure-prenumeration. Om du inte har ett Azure-konto [skapa ett kostnadsfritt Azure konto](https://azure.microsoft.com/free/) i bara några minuter.
* En Mac eller en dator som kör Windows eller Ubuntu.
* Ett trådlöst nätverk för Ludd M0 WiFi tooconnect till.
* En Internet-anslutning toodownload hello-konfigurationsverktyget.
* [Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 eller senare. Tidigare versioner fungerar inte med hello Azure IoT Hub-biblioteket.

Om du inte har en sensor är hello följande objekt valfria. Du kan också ha hello möjlighet att använda simulerade sensordata:

* En BME280 temperatur- och fuktighetskonsekvens sensor
* En breadboard
* M/M omkopplare kablar

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-m0-wifi-with-hello-sensor-and-your-computer"></a>Anslut ludd M0 WiFi med hello sensor och din dator
I det här avsnittet kan du ansluta hello sensorer tooyour kort. Sedan ansluter du enheten tooyour datorn för framtida användning.

### <a name="connect-a-dht22-temperature-and-humidity-sensor-toofeather-m0-wifi"></a>Ansluta en DHT22 temperatur- och fuktighetskonsekvens sensor tooFeather M0 WiFi

Använd hello breadboard och omkopplare kablar toomake hello-anslutning. Om du inte har en sensor hoppa över det här avsnittet, eftersom du kan använda simulerade sensordata i stället.

![Referens för anslutningar](media/iot-hub-adafruit-feather-m0-wifi-get-started/3_connections_on_breadboard.png)


För sensor PIN-koder, använder du följande kablar hello:


| Start (sensor)           | END (moderkort)            | Kabel färg   |
| -----------------------  | ---------------------- | ------------: |
| VDD (PIN-kod 27A)            | 3v (3A PIN-kod)            | Röd kabel     |
| JORD (PIN-kod 29 a)            | JORD (6A PIN-kod)           | Svart kabel   |
| SCK (PIN-kod 30A)            | SCK (PIN-kod 12A)          | Gult kabel  |
| SDO (PIN-kod 31A)            | MI (PIN-kod 14A)           | Vita kabeln   |
| SDI (PIN-kod 32 a)            | M0 (PIN-kod 13 a)           | Blå kabeln    |
| CS (PIN-kod 33A)             | GPIO 5 (PIN-kod 15J)       | Orange kabel  |

Mer information finns i [Adafruit BME280 fuktighet barometertrycket + temperatur Sensor nedbrytning](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout/wiring-and-test?view=all) och [Adafruit ludd M0 WiFi pinouts](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500/pinouts).



Ludd M0 Wi-Fi bör nu vara ansluten med en fungerar sensor.

![Anslut DHT22 med ludd Huzzah](media/iot-hub-adafruit-feather-m0-wifi-get-started/4_connect-bme280-feather-m0-wifi.png)

### <a name="connect-feather-m0-wifi-tooyour-computer"></a>Ansluta ludd M0 WiFi tooyour dator

Använd hello Micro USB tooType en USB-kabel tooconnect ludd M0 WiFi tooyour dator, som visas:

![Ansluta ludd Huzzah tooyour dator](media/iot-hub-adafruit-feather-m0-wifi-get-started/5_connect-feather-m0-wifi-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a>Lägg till behörigheter för seriell port (Ubuntu)

Om du använder Ubuntu, kontrollera att du har hello behörigheter toooperate på hello USB-port för Ludd M0 WiFi. tooadd serieport behörigheter så här:


1. Kör hello följande kommandon i en terminal:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   Du får ett av hello följande utdata:

   * crw-rw---1 rot uucp xxxxxxxx
   * crw-rw---1 rot antingen xxxxxxxx

   Hello utdata och notera att `uucp` eller `dialout` är hello ägare gruppnamn hello USB-port.

2. tooadd hello toohello användargruppen, kör hello följande kommando:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   I föregående steg hello du fick hello gruppnamn ägare `<group-owner-name>`. Användarnamnet Ubuntu är `<username>`.

3. Logga ut från Ubuntu för hello ändra tooappear och loggar sedan in igen.

## <a name="collect-sensor-data-and-send-it-tooyour-iot-hub"></a>Samla in sensordata och skicka den tooyour IoT-hubb

I det här avsnittet, distribuera och köra ett exempelprogram på ludd M0 WiFi. hello exempelprogrammet gör hello Indikator blinka på ludd M0 WiFi. Skickar sedan hello temperatur och fuktighet data som samlas in från hello BME280 sensor tooyour IoT-hubb.

### <a name="get-hello-sample-application-from-github-and-prepare-hello-arduino-ide"></a>Hämta exempelprogrammet hello från GitHub och förbereda hello Arduino IDE

hello exempelprogrammet finns på GitHub. Klona hello exempel lagringsplats som innehåller hello exempelprogrammet från GitHub. tooclone hello exempel lagringsplatsen, gör du följande:

1. Öppna en kommandotolk eller ett terminalfönster.

2. Gå tooa mappen där du vill att hello exempel programmet toobe lagras.
3. Kör följande kommando hello:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-Feather-M0-WiFi-client-app.git
   ```

### <a name="install-hello-package-for-feather-m0-wifi-in-hello-arduino-ide"></a>Installera hello paketet för Ludd M0 WiFi i hello Arduino IDE

1. Öppna hello mappen där hello exempelprogrammet lagras.

2. Öppna hello app.ino filen i mappen för hello-app i hello Arduino IDE.

   ![Öppna hello exempelprogrammet i Arduino IDE](media/iot-hub-adafruit-feather-m0-wifi-get-started/6_arduino-ide-open-sample-app.png)


1. Klicka på **filen** > **inställningar** (Windows-/ Linux) eller **Arduino** > **inställningar** (Mac) och kopiera och Klistra in hello länk nedan i hello **ytterligare anslagstavlor Manager URL: er** alternativet i hello Arduino IDE-inställningar.
   
   ```
   https://adafruit.github.io/arduino-board-index/package_adafruit_index.json, https://adafruit.github.io/arduino-board-index/package_adafruit_index.json
   ```

1. Klicka på **verktyg** > **Hanteringsstyrenhetens** > **anslagstavlor Manager**, och sedan installera hello `Arduino SAMD Boards` version `1.6.2` eller senare. 

1. I Hej samma fönster, installera `Adafruit SAMD Boards` paketet tooadd hello board definitioner.

   ![Hej esp8266 paketet har installerats](media/iot-hub-adafruit-feather-m0-wifi-get-started/7_arduino-ide-package-url.png)

4. Klicka på **verktyg** > **Hanteringsstyrenhetens** > **Adafruit M0 WiFi**.

5. Installera drivrutiner (för Windows). När du ansluter ludd M0 WiFi kan behöva du tooinstall en drivrutin. Klicka på [hello hämtningslänken på webbsidan hello](https://github.com/adafruit/Adafruit_Windows_Drivers/releases/download/1.1/adafruit_drivers.exe) toodownload hello drivrutinen installer. Följ hello steg tooinstall hello drivrutiner som du vill använda.

### <a name="install-necessary-libraries"></a>Installera nödvändiga bibliotek

1. I hello Arduino IDE, klickar du på **skiss** > **innehåller biblioteket** > **Hantera bibliotek**.

2. Sök efter hello efter namn på biblioteket i taget. Klicka på för alla bibliotek som du hittar **installera**:

   * `RTCZero`
   * `NTPClient`
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_HTTP`
   * `ArduinoJson`
   * `Adafruit BME280 Library`
   * `Adafruit Unified Sensor`

3. Installera manuellt `Adafruit_WINC1500`. Gå för[webbplatsen](https://github.com/adafruit/Adafruit_WINC1500) och på **kloning eller hämta** > **hämta ZIP**. Sedan gå för i din Arduino IDE**skiss** > **innehåller biblioteket** > **lägger du till .zip biblioteket** och Lägg till hello zip-filen.

### <a name="use-hello-sample-application-if-you-dont-have-a-real-bme280-sensor"></a>Använd hello exempelprogrammet om du inte har en verklig BME280 sensor

Om du inte har en verklig BME280 sensor simulera hello exempelprogrammet temperatur- och fuktighetskonsekvens data. tooset in hello programmet toouse simulerade exempeldata, Följ dessa steg:

1. Öppna hello `config.h` filen i hello `app` mapp.

2. Leta upp följande kodrad hello och ändrar hello-värdet från `false` för`true`:

   ```c
   define SIMULATED_DATA true
   ```
   ![Konfigurera hello exempeldata programmet toouse simulerade](media/iot-hub-adafruit-feather-m0-wifi-get-started/8_arduino-ide-configure-app-use-simulated-data.png)

3. Spara hello-filen med `Control-s`.

### <a name="deploy-hello-sample-application-toofeather-m0-wifi"></a>Distribuera hello exempel programmet tooFeather M0 WiFi

1. Klicka på hello Arduino IDE **verktyget** > **Port**, och klicka sedan på hello seriell port för Ludd M0 WiFi.

2. Klicka på **skiss** > **överför** toobuild och distribuera hello exempel programmet tooFeather M0 WiFi.

### <a name="enter-your-credentials"></a>Ange autentiseringsuppgifter

När hello överföringen är klar följer du dessa steg tooenter dina autentiseringsuppgifter:

1. Klicka på hello Arduino IDE **verktyg** > **seriella övervakaren**.

2. Markera i hello nedre högra hörnet av hello seriella övervakaren **ingen rad avslutas** i listrutan för hello hello vänster.
3. Välj **115200 baud** i hello listrutan på hello rätt.
4. Ange hello följande information om du är i hello-textrutan hello överst och tooprovide och klicka på **skicka**:

   * Wi-Fi SSID
   * Wi-Fi-lösenord
   * Anslutningssträngen för enhet

> [!Note]
> information om hello autentiseringsuppgifter lagras i hello EEPROM av ludd M0 WiFi. Om du klickar på hello Återställ-knappen i hello ludd M0 WiFi-kort frågar hello exempelprogrammet om du vill ha tooerase hello information. Ange `Y` tooerase hello information. Du uppmanas tooprovide hello information en gång.

### <a name="verify-that-hello-sample-application-is-running-successfully"></a>Kontrollera att hello exempelprogrammet körts

Om du ser hello följande utdata från hello seriella övervakningsfönstret och hello blinka Indikator på ludd M0 WiFi, hello exempelprogrammet körts:

![Slutversionen i Arduino IDE](media/iot-hub-adafruit-feather-m0-wifi-get-started/9_arduino-ide-final-output.png)

## <a name="next-steps"></a>Nästa steg

Du har anslutit ludd M0 WiFi tooyour IoT-hubb och skickas hello avbildas sensor data tooyour IoT-hubb. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

