---
title: aaaESP8266 toocloud - ansluta ludd HUZZAH ESP8266 tooAzure IoT-hubb | Microsoft Docs
description: "Lär dig hur toosetup och ansluta Adafruit ludd HUZZAH ESP8266 tooAzure IoT-hubb för den toosend dataplattform toohello Azure-molnet i den här självstudiekursen."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.assetid: c505aacf-89a8-40ed-a853-493b75bec524
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2017
ms.author: xshi
ms.openlocfilehash: 44fd47232488948d21c7aa71bdd865397e41e63e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-adafruit-feather-huzzah-esp8266-tooazure-iot-hub-in-hello-cloud"></a>Ansluta Adafruit ludd HUZZAH ESP8266 tooAzure IoT-hubb i hello moln

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Anslutningen mellan DHT22 och ludd HUZZAH ESP8266 IoT-hubb](media/iot-hub-arduino-huzzah-esp8266-get-started/1_connection-hdt22-feather-huzzah-iot-hub.png)

## <a name="what-you-do"></a>Vad du gör


Ansluta Adafruit ludd HUZZAH ESP8266 tooan IoT-hubb som du skapar. Sedan kör du ett exempelprogram på ESP8266 toocollect hello temperatur- och fuktighetskonsekvens data från en DHT22 sensor. Slutligen kan skicka du hello sensor data tooyour IoT-hubb.

> [!NOTE]
> Om du använder andra ESP8266 anslagstavlor fortfarande följer du dessa steg tooconnect den tooyour IoT-hubb. Beroende på hello ESP8266 ändringar som du använder, kanske du måste tooreconfigure hello `LED_PIN`. Till exempel, om du använder ESP8266 från AI-Thinker kan du ändra det från `0` för`2`. Har du en kit ännu? Hämta det från hello [Azure-webbplatsen](http://azure.com/iotstarterkits).




## <a name="what-you-learn"></a>Detta får du får lära dig

* Hur toocreate en IoT-hubb och registrera en enhet för Ludd HUZZAH ESP8266
* Hur tooconnect ludd HUZZAH ESP8266 med hello sensor och datorn
* Hur toocollect sensordata genom att köra ett exempelprogram på ludd HUZZAH ESP8266
* Hur toosend hello sensor data tooyour IoT-hubb

## <a name="what-you-need"></a>Vad du behöver

![delar som behövs för hello självstudiekursen](media/iot-hub-arduino-huzzah-esp8266-get-started/2_parts-needed-for-the-tutorial.png)

toocomplete den här åtgärden måste följande delar från ludd för HUZZAH ESP8266 startpaket hello:

* hello ludd HUZZAH ESP8266-kort
* Micro USB-tooType en USB-kabel

Du måste också följande saker för din utvecklingsmiljö hello:

* En aktiv Azure-prenumeration. Om du inte har ett Azure-konto [skapa ett kostnadsfritt Azure konto](https://azure.microsoft.com/free/) i bara några minuter.
* Eller Mac-dator som kör Windows eller Ubuntu.
* Ludd HUZZAH ESP8266 tooconnect till trådlöst nätverk.
* Internet-anslutning toodownload hello-konfigurationsverktyget.
* [Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 eller senare. Tidigare versioner fungerar inte med hello AzureIoT bibliotek.

hello följande objekt är valfritt om du inte har en sensor. Du kan också ha hello möjlighet att använda simulerade sensordata.

* En Adafruit DHT22 temperatur- och fuktighetskonsekvens sensor
* En breadboard
* M/M omkopplare kablar


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-huzzah-esp8266-with-hello-sensor-and-your-computer"></a>Anslut ludd HUZZAH ESP8266 med hello sensor och din dator
I det här avsnittet kan du ansluta hello sensorer tooyour kort. Sedan ansluter du enheten tooyour datorn för framtida användning.
### <a name="connect-a-dht22-temperature-and-humidity-sensor-toofeather-huzzah-esp8266"></a>Ansluta en DHT22 temperatur- och fuktighetskonsekvens sensor tooFeather HUZZAH ESP8266

Använd hello breadboard och omkopplare kablar toomake hello-anslutning på följande sätt. Om du inte har en sensor hoppa över det här avsnittet, eftersom du kan använda simulerade sensordata i stället.

![Referens för anslutningar](media/iot-hub-arduino-huzzah-esp8266-get-started/15_connections_on_breadboard.png)


För sensor PIN-koder, använder du följande kablar hello:


| Starta (Sensor)           | END (moderkort)           | Kabel färg   |
| -----------------------  | ---------------------- | ------------: |
| VDD (PIN-kod 31F)            | 3v (fästa 58H)           | Röd kabel     |
| DATA (PIN-kod 32F)           | GPIO 2 (PIN-kod 46A)       | Blå kabeln    |
| JORD (PIN-kod 34F)            | JORD (PIN-kod 56I)          | Svart kabel   |

Mer information finns i [Adafruit DHT22 sensor installationsprogrammet](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) och [Adafruit ludd HUZZAH Esp8266 Pinouts](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts).



Nu bör din ludd Huzzah ESP8266 anslutas med en fungerar sensor.

![Anslut DHT22 med ludd Huzzah](media/iot-hub-arduino-huzzah-esp8266-get-started/8_connect-dht22-feather-huzzah.png)

### <a name="connect-feather-huzzah-esp8266-tooyour-computer"></a>Ansluta ludd HUZZAH ESP8266 tooyour dator

Använda hello Micro USB tooType en USB-kabel tooconnect ludd HUZZAH ESP8266 tooyour dator visas nästa.

![Ansluta ludd Huzzah tooyour dator](media/iot-hub-arduino-huzzah-esp8266-get-started/9_connect-feather-huzzah-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a>Lägg till behörigheter för seriell port (Ubuntu)


Om du använder Ubuntu, kontrollera att du har hello behörigheter toooperate på hello USB-port för Ludd HUZZAH ESP8266. tooadd serieport behörigheter så här:


1. Kör följande kommandon i en terminal hello:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   Du får ett av hello följande utdata:

   * crw-rw---1 rot uucp xxxxxxxx
   * crw-rw---1 rot antingen xxxxxxxx

   Hello utdata och notera att `uucp` eller `dialout` är hello ägare gruppnamn hello USB-port.

1. Lägg till hello toohello användargrupp genom att köra följande kommando hello:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>`är hello ägare gruppnamn du fick i hello föregående steg. `<username>`är din Ubuntu användarnamn.

1. Logga ut från Ubuntu och loggar sedan in igen för hello ändra tooappear.

## <a name="collect-sensor-data-and-send-it-tooyour-iot-hub"></a>Samla in sensordata och skicka den tooyour IoT-hubb

I det här avsnittet, distribuera och köra ett exempelprogram på ludd HUZZAH ESP8266. hello exempelprogrammet blinkar hello Indikator på ludd HUZZAH ESP8266 och skickar hello temperatur och fuktighet data som samlas in från hello DHT22 sensor tooyour IoT-hubb.

### <a name="get-hello-sample-application-from-github"></a>Hämta exempelprogrammet hello från GitHub

hello exempelprogrammet finns på GitHub. Klona hello exempel lagringsplats som innehåller hello exempelprogrammet från GitHub. tooclone hello exempel lagringsplatsen, gör du följande:

1. Öppna en kommandotolk eller ett terminalfönster.
1. Gå tooa mappen där du vill att hello exempel programmet toobe lagras.
1. Kör följande kommando hello:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-feather-huzzah-client-app.git
   ```

Installera hello paketet för Ludd HUZZAH ESP8266 i hello Arduino IDE:

1. Öppna hello mappen där hello exempelprogrammet lagras.
1. Öppna hello app.ino filen i mappen för hello-app i hello Arduino IDE.

   ![Öppna hello exempelprogrammet i Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/10_arduino-ide-open-sample-app.png)

1. Klicka på hello Arduino IDE **filen** > **inställningar**.
1. I hello **inställningar** dialogrutan klickar du på hello ikonen nästa toohello **ytterligare anslagstavlor Manager URL: er** rutan.
1. Ange hello följande URL i hello popup-fönster, och klicka sedan på **OK**.

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![Peka tooa webbadressen i Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/11_arduino-ide-package-url.png)

1. I hello **inställningar** dialogrutan klickar du på **OK**.
1. Klicka på **verktyg** > **Hanteringsstyrenhetens** > **anslagstavlor Manager**, och sök sedan efter esp8266.

   Anslagstavlor Manager anger att ESP8266 med en version av 2.2.0 eller senare är installerad.

   ![Hej esp8266 paketet har installerats](media/iot-hub-arduino-huzzah-esp8266-get-started/12_arduino-ide-esp8266-installed.png)

1. Klicka på **verktyg** > **Hanteringsstyrenhetens** > **Adafruit HUZZAH ESP8266**.

### <a name="install-necessary-libraries"></a>Installera nödvändiga bibliotek

1. I hello Arduino IDE, klickar du på **skiss** > **innehåller biblioteket** > **Hantera bibliotek**.
1. Sök efter hello efter namn på biblioteket i taget. Klicka på för alla bibliotek som du hittar **installera**.
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a>Har en verklig DHT22 sensor?

hello exempelprogrammet simulera temperatur- och fuktighetskonsekvens data om du inte har en verklig DHT22 sensor. tooset in hello programmet toouse simulerade exempeldata, Följ dessa steg:

1. Öppna hello `config.h` filen i hello `app` mapp.
1. Leta upp följande kodrad hello och ändrar hello-värdet från `false` för`true`:
   ```c
   define SIMULATED_DATA true
   ```
   ![Konfigurera hello exempeldata programmet toouse simulerade](media/iot-hub-arduino-huzzah-esp8266-get-started/13_arduino-ide-configure-app-use-simulated-data.png)

1. Spara hello-filen med `Control-s`.

### <a name="deploy-hello-sample-application-toofeather-huzzah-esp8266"></a>Distribuera hello exempel programmet tooFeather HUZZAH ESP8266

1. Klicka på hello Arduino IDE **verktyget** > **Port**, och klicka sedan på hello seriell port för Ludd HUZZAH ESP8266.
1. Klicka på **skiss** > **överför** toobuild och distribuera hello exempel programmet tooFeather HUZZAH ESP8266.

### <a name="enter-your-credentials"></a>Ange autentiseringsuppgifter

När hello överföringen är klar följer du dessa steg tooenter dina autentiseringsuppgifter:

1. Klicka på hello Arduino IDE **verktyg** > **seriella övervakaren**.
1. Observera hello två listrutorna i hello nedre högra hörnet i hello seriella övervakningsfönstret.
1. Välj **ingen rad avslutas** för hello vänstra nedrullningsbara listan.
1. Välj **115200 baud** för hello höger listrutan.
1. Ange hello efter information om du uppmanas tooprovide i hello-textrutan finns hello överst i hello seriella övervakningsfönstret dem, och klicka sedan på **skicka**.
   * Wi-Fi SSID
   * Wi-Fi-lösenord
   * Anslutningssträngen för enhet

> [!Note]
> information om hello autentiseringsuppgifter lagras i hello EEPROM av ludd HUZZAH ESP8266. Om du klickar på hello Återställ-knappen i hello ludd HUZZAH ESP8266 board ombeds hello exempelprogrammet tooerase hello information. Ange `Y` toohave hello information tas bort. Du uppmanas tooprovide hello information en gång.

### <a name="verify-hello-sample-application-is-running-successfully"></a>Kontrollera hello exempelprogrammet körts

Om du ser hello följande utdata från hello seriella övervakningsfönstret och hello blinka Indikator på ludd HUZZAH ESP8266, hello exempelprogrammet körts.

![Slutversionen i Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a>Nästa steg

Du har anslutna ludd HUZZAH ESP8266 tooyour IoT-hubb, och skickas hello avbildas sensor data tooyour IoT-hubb. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

