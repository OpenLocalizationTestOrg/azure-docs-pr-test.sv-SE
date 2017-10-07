---
title: aaaESP8266 toocloud - ansluta Sparkfun ESP8266 sak Dev tooAzure IoT-hubb | Microsoft Docs
description: "Lär dig hur toosetup och ansluta Sparkfun ESP8266 sak Dev tooAzure IoT-hubb för den toosend dataplattform toohello Azure-molnet i den här självstudiekursen."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.assetid: 587fe292-9602-45b4-95ee-f39bba10e716
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2017
ms.author: xshi
ms.openlocfilehash: 19b249df23b6df516634853521c6d532f51014da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-sparkfun-esp8266-thing-dev-tooazure-iot-hub-in-hello-cloud"></a>Ansluta Sparkfun ESP8266 sak Dev tooAzure IoT-hubb i hello moln

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![anslutningen mellan DHT22 och sak Dev IoT-hubb](media/iot-hub-sparkfun-thing-dev-get-started/1_connection-hdt22-thing-dev-iot-hub.png)

## <a name="what-you-will-do"></a>Vad du ska göra

Ansluta Sparkfun ESP8266 sak Dev tooan IoT-hubb skapar du. Kör sedan ett exempelprogram på ESP8266 toocollect temperatur- och fuktighetskonsekvens data från en DHT22 sensor. Slutligen skicka hello sensor data tooyour IoT-hubb.

> [!NOTE]
> Om du använder andra ESP8266: er kan du fortfarande kan följa dessa steg tooconnect den tooyour IoT-hubb. Hej ESP8266 ändringar som du använder, måste du använda tooreconfigure hello `LED_PIN`. Till exempel om du använder ESP8266 från AI-Thinker, du kan ändra den från `0` för`2`. Har ett kit än?: Klicka på [här](http://azure.com/iotstarterkits)

## <a name="what-you-will-learn"></a>Vad får du lära dig

* Hur toocreate en IoT-hubb och registrera en enhet för sak Utvecklartestning
* Hur tooconnect sak Dev med hello sensor och din dator.
* Hur toocollect sensordata genom att köra ett exempelprogram på sak Utvecklartestning
* Hur hello toosend sensor data tooyour IoT-hubb.

## <a name="what-you-will-need"></a>Vad du behöver

![delar som behövs för hello självstudiekursen](media/iot-hub-sparkfun-thing-dev-get-started/2_parts-needed-for-the-tutorial.png)

toocomplete den här åtgärden måste följande delar från din sak Dev startpaket hello:

* Hej Sparkfun ESP8266 sak Dev-kort.
* Micro USB-tooType en USB-kabel.

Du måste också hello följande för din utvecklingsmiljö:

* En aktiv Azure-prenumeration. Om du inte har ett Azure-konto [skapa ett kostnadsfritt Azure konto](https://azure.microsoft.com/free/) i bara några minuter.
* Eller Mac-dator som kör Windows eller Ubuntu.
* Sparkfun ESP8266 sak Dev tooconnect till trådlöst nätverk.
* Internet-anslutning toodownload hello-konfigurationsverktyget.
* [Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 (eller senare), tidigare versioner fungerar inte med hello AzureIoT bibliotek.

hello följande objekt är valfritt om du inte har en sensor. Du kan också ha hello möjlighet att använda simulerade sensordata.

* En Adafruit DHT22 temperatur- och fuktighetskonsekvens sensor.
* En breadboard.
* M/M omkopplare kablar.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-esp8266-thing-dev-with-hello-sensor-and-your-computer"></a>Anslut ESP8266 sak Dev med hello sensor och din dator

### <a name="connect-a-dht22-temperature-and-humidity-sensor-tooesp8266-thing-dev"></a>Ansluta en DHT22 temperatur- och fuktighetskonsekvens sensor tooESP8266 sak Dev

Använd hello breadboard och omkopplare kablar toomake hello-anslutning på följande sätt. Om du inte har en sensor hoppa över det här avsnittet, eftersom du kan använda simulerade sensordata i stället.

![Referens för anslutningar](media/iot-hub-sparkfun-thing-dev-get-started/15_connections_on_breadboard.png)

För sensor PIN-koder, kommer vi att använda följande kablar hello:

| Starta (Sensor)           | END (moderkort)           | Kabel färg   |
| -----------------------  | ---------------------- | ------------: |
| VDD (PIN-kod 27F)            | 3v (8A PIN-kod)           | Röd kabel     |
| DATA (PIN-kod 28 f)           | GPIO 2 (9A PIN-kod)       | Vita kabeln    |
| JORD (PIN-kod 30 f)            | JORD (7J PIN-kod)          | Svart kabel   |


- Mer information finns: [DHT22 sensor installationsprogrammet](http://cdn.sparkfun.com/datasheets/Sensors/Weather/RHT03.pdf) och [Sparkfun ESP8266 sak Dev-specifikationen](https://www.sparkfun.com/products/13711)

Nu bör din Sparkfun ESP8266 sak Dev anslutas med en fungerar sensor.

![Anslut dht22 med ESP8266 sak Dev](media/iot-hub-sparkfun-thing-dev-get-started/8_connect-dht22-thing-dev.png)

### <a name="connect-sparkfun-esp8266-thing-dev-tooyour-computer"></a>Ansluta Sparkfun ESP8266 sak Dev tooyour dator

Använda hello Micro USB tooType en USB-kabel tooconnect Sparkfun ESP8266 sak Dev tooyour dator på följande sätt.

![ansluta ludd huzzah tooyour dator](media/iot-hub-sparkfun-thing-dev-get-started/9_connect-thing-dev-computer.png)

### <a name="add-serial-port-permissions--ubuntu-only"></a>Lägg till behörigheter för seriell port – Ubuntu endast

Om du använder Ubuntu Kontrollera normal användare har hello behörigheter toooperate på hello USB-port för Sparkfun ESP8266 sak Utvecklartestning tooadd serieport behörigheter för en normal användare så här:

1. Kör följande kommandon i en terminal hello:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   Du får ett av hello följande utdata:

   * crw-rw---1 rot uucp xxxxxxxx
   * crw-rw---1 rot antingen xxxxxxxx

   Hello utdata och notera `uucp` eller `dialout` som hello ägare gruppnamnet för hello USB-port.

1. Lägg till hello toohello användargrupp genom att köra följande kommando hello:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>`är hello ägare gruppnamn du fick i hello föregående steg. `<username>`är din Ubuntu användarnamn.

1. Logga ut Ubuntu och logga in det igen för hello ändra tootake effekt.

## <a name="collect-sensor-data-and-send-it-tooyour-iot-hub"></a>Samla in sensordata och skicka den tooyour IoT-hubb

I det här avsnittet kan du distribuera och köra ett exempelprogram på Sparkfun ESP8266 sak Utvecklartestning hello exempelprogrammet blinkar hello Indikator på Sparkfun ESP8266 sak Dev och skickar hello temperatur och fuktighet data som samlas in från hello DHT22 sensor tooyour IoT-hubb.

### <a name="get-hello-sample-application-from-github"></a>Hämta exempelprogrammet hello från GitHub

hello exempelprogrammet finns på GitHub. Klona hello exempel lagringsplats som innehåller hello exempelprogrammet från GitHub. tooclone hello exempel lagringsplatsen, gör du följande:

1. Öppna en kommandotolk eller ett terminalfönster.
1. Gå tooa mappen där du vill att hello exempel programmet toobe lagras.
1. Kör följande kommando hello:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-SparkFun-ThingDev-client-app.git
   ```

Installera hello paketet för Sparkfun ESP8266 sak Dev i Arduino IDE:

1. Öppna hello mappen där hello exempelprogrammet lagras.
1. Öppna hello app.ino filen i hello app mappen i Arduino IDE.

   ![Öppna hello exempelprogrammet i arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/10_arduino-ide-open-sample-app.png)

1. Klicka på hello Arduino IDE **filen** > **inställningar**.
1. I hello **inställningar** dialogrutan klickar du på hello ikonen nästa toohello **ytterligare anslagstavlor Manager URL: er** textruta.
1. Ange hello följande URL i hello popup-fönster, och klicka sedan på **OK**.

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![Peka tooa webbadressen i arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/11_arduino-ide-package-url.png)

1. I hello **inställningar** dialogrutan klickar du på **OK**.
1. Klicka på **verktyg** > **Hanteringsstyrenhetens** > **anslagstavlor Manager**, och sök sedan efter esp8266.
   ESP8266 med en version av 2.2.0 eller senare ska installeras.

   ![Hej esp8266 paketet har installerats](media/iot-hub-sparkfun-thing-dev-get-started/12_arduino-ide-esp8266-installed.png)

1. Klicka på **verktyg** > **Hanteringsstyrenhetens** > **Sparkfun ESP8266 sak Dev**.

### <a name="install-necessary-libraries"></a>Installera nödvändiga bibliotek

1. I hello Arduino IDE, klickar du på **skiss** > **innehåller biblioteket** > **Hantera bibliotek**.
1. Sök efter hello efter namn på biblioteket i taget. För var och en av hello-biblioteket som du hittar klickar **installera**.
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a>Har en verklig DHT22 sensor?

hello exempelprogrammet simulera temperatur- och fuktighetskonsekvens data om du inte har en verklig DHT22 sensor. tooenable hello programmet toouse simulerade exempeldata, Följ dessa steg:

1. Öppna hello `config.h` filen i hello `app` mapp.
1. Leta upp följande kodrad hello och ändrar hello-värdet från `false` för`true`:
   ```c
   define SIMULATED_DATA true
   ```
   ![Konfigurera hello exempeldata programmet toouse simulerade](media/iot-hub-sparkfun-thing-dev-get-started/13_arduino-ide-configure-app-use-simulated-data.png)
   
1. Spara med `Control-s`.

### <a name="deploy-hello-sample-application-toosparkfun-esp8266-thing-dev"></a>Distribuera hello exempel programmet tooSparkfun ESP8266 sak Dev

1. Klicka på hello Arduino IDE **verktyget** > **Port**, och klicka sedan på hello seriell port för Sparkfun ESP8266 sak Utvecklartestning
1. Klicka på **skiss** > **överför** toobuild och distribuera hello exempel programmet tooSparkfun ESP8266 sak Utvecklartestning

> [!Note]
> Om du använder macOS kunde du antagligen se hello efter meddelanden under överföring. `warning: espcomm_sync failed`,`error: espcomm_open failed`. Öppna fönstret ternimal och slutför under åtgärder toosolve problemet.
> ```bash
> cd /System/Library/Extensions/IOUSBFamily.kext/Contents/PlugIns
> sudo mv AppleUSBFTDI.kext AppleUSBFTDI.disabled
> sudo touch /System/Library/Extensions
> ```

### <a name="enter-your-credentials"></a>Ange autentiseringsuppgifter

När hello överföringen är klar så hello steg tooenter dina autentiseringsuppgifter:

1. Klicka på hello Arduino IDE **verktyg** > **seriella övervakaren**.
1. Observera hello två listrutorna på hello nedre högra hörnet i hello seriella övervakningsfönstret.
1. Välj **ingen rad avslutas** för hello vänstra nedrullningsbara listan.
1. Välj **115200 baud** för hello höger listrutan.
1. Ange hello efter information om du uppmanas tooprovide i hello-textrutan finns hello överst i hello seriella övervakningsfönstret dem, och klicka sedan på **skicka**.
   * Wi-Fi SSID
   * Wi-Fi-lösenord
   * Anslutningssträngen för enhet

> [!Note]
> information om hello autentiseringsuppgifter lagras i hello EEPROM av Sparkfun ESP8266 sak Utvecklartestning Om du klickar på hello Återställ-knappen i hello Sparkfun ESP8266 sak Dev board frågar hello exempelprogrammet om du vill ha tooerase hello information. Ange `Y` toohave hello information tas bort och du ombeds tooprovide hello informationen igen.

### <a name="verify-hello-sample-application-is-running-successfully"></a>Kontrollera hello exempelprogrammet körts

Om du ser hello följande utdata från hello seriella övervakningsfönstret och hello blinka Indikator på Sparkfun ESP8266 sak Dev hello exempelprogrammet körts.

![slutversionen i arduino ide](media/iot-hub-sparkfun-thing-dev-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a>Nästa steg

Du har anslutit en Sparkfun ESP8266 sak Dev tooyour IoT-hubb och skickas hello avbildas sensor data tooyour IoT-hubb. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
