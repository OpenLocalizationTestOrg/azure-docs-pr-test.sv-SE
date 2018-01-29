---
title: ESP8266 till molnet - ansluta ludd HUZZAH ESP8266 till Azure IoT Hub | Microsoft Docs
description: "Lär dig mer om att konfigurera och ansluta Adafruit ludd HUZZAH ESP8266 till Azure IoT-hubb för att skicka data till Azure-molnplattform i den här självstudiekursen."
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
ms.openlocfilehash: 6a450579c848fe6030a328ddf410f139baae2324
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="connect-adafruit-feather-huzzah-esp8266-to-azure-iot-hub-in-the-cloud"></a>Ansluta Adafruit ludd HUZZAH ESP8266 till Azure IoT-hubb i molnet

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

![Anslutningen mellan DHT22 och ludd HUZZAH ESP8266 IoT-hubb](media/iot-hub-arduino-huzzah-esp8266-get-started/1_connection-hdt22-feather-huzzah-iot-hub.png)

## <a name="what-you-do"></a>Vad du gör


Ansluta Adafruit ludd HUZZAH ESP8266 till en IoT-hubb som du skapar. Sedan kör du ett exempelprogram på ESP8266 att samla in data för temperatur- och fuktighetskonsekvens från en DHT22 sensor. Slutligen kan du skicka dessa sensordata för din IoT-hubb.

> [!NOTE]
> Du kan följa stegen nedan för att ansluta till din IoT-hubb även om du använder andra ESP8266: er. Beroende på ESP8266-kort som du använder, kan du behöva konfigurera om den `LED_PIN`. Till exempel, om du använder ESP8266 från AI-Thinker kan du ändra det från `0` till `2`. Har du en kit ännu? Hämta det från den [Azure-webbplatsen](http://azure.com/iotstarterkits).




## <a name="what-you-learn"></a>Detta får du får lära dig

* Hur du skapar en IoT-hubb och registrera en enhet för Ludd HUZZAH ESP8266
* Så här ansluter du ludd HUZZAH ESP8266 med sensorn och datorn
* Hur du samlar in sensordata genom att köra ett exempelprogram på ludd HUZZAH ESP8266
* Hur du skickar dessa sensordata till din IoT-hubb

## <a name="what-you-need"></a>Vad du behöver

![Delar som behövs för kursen](media/iot-hub-arduino-huzzah-esp8266-get-started/2_parts-needed-for-the-tutorial.png)

För att slutföra den här åtgärden behöver du följande delar från din startpaket för Ludd HUZZAH ESP8266:

* Ludd HUZZAH ESP8266-kort
* En Micro USB till typ A USB-kabel

Du måste också följande saker för din utvecklingsmiljö:

* En aktiv Azure-prenumeration. Om du inte har ett Azure-konto [skapa ett kostnadsfritt Azure konto](https://azure.microsoft.com/free/) i bara några minuter.
* Eller Mac-dator som kör Windows eller Ubuntu.
* Trådlöst nätverk för Ludd HUZZAH ESP8266 att ansluta till.
* Internet-anslutning att hämta verktyget configuration.
* [Arduino IDE](https://www.arduino.cc/en/main/software) version 1.6.8 eller senare. Tidigare versioner fungerar inte med AzureIoT-biblioteket.

Följande objekt är valfritt om du inte har en sensor. Du har också möjlighet att använda simulerade sensordata.

* En Adafruit DHT22 temperatur- och fuktighetskonsekvens sensor
* En breadboard
* M/M omkopplare kablar


[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="connect-feather-huzzah-esp8266-with-the-sensor-and-your-computer"></a>Anslut ludd HUZZAH ESP8266 med sensorn och din dator
I det här avsnittet kan ansluta du sensorerna till kortets. Sedan ansluta du enheten till din dator för framtida användning.
### <a name="connect-a-dht22-temperature-and-humidity-sensor-to-feather-huzzah-esp8266"></a>Ansluta en DHT22 temperatur- och fuktighetskonsekvens sensor till ludd HUZZAH ESP8266

Använd breadboard och omkopplare kablar för att ansluta på följande sätt. Om du inte har en sensor hoppa över det här avsnittet, eftersom du kan använda simulerade sensordata i stället.

![Referens för anslutningar](media/iot-hub-arduino-huzzah-esp8266-get-started/15_connections_on_breadboard.png)


Använd följande-kablar för sensor PIN:


| Starta (Sensor)           | END (moderkort)           | Kabel färg   |
| -----------------------  | ---------------------- | ------------: |
| VDD (PIN-kod 31F)            | 3v (fästa 58H)           | Röd kabel     |
| DATA (PIN-kod 32F)           | GPIO 2 (PIN-kod 46A)       | Blå kabeln    |
| JORD (PIN-kod 34F)            | JORD (PIN-kod 56I)          | Svart kabel   |

Mer information finns i [Adafruit DHT22 sensor installationsprogrammet](https://learn.adafruit.com/dht/connecting-to-a-dhtxx-sensor) och [Adafruit ludd HUZZAH Esp8266 Pinouts](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266/using-arduino-ide?view=all#pinouts).



Nu bör din ludd Huzzah ESP8266 anslutas med en fungerar sensor.

![Anslut DHT22 med ludd Huzzah](media/iot-hub-arduino-huzzah-esp8266-get-started/8_connect-dht22-feather-huzzah.png)

### <a name="connect-feather-huzzah-esp8266-to-your-computer"></a>Ansluta ludd HUZZAH ESP8266 till din dator

Enligt nästa, kan du använda Micro USB till typ A USB-kabel för att ansluta ludd HUZZAH ESP8266 till datorn.

![Ansluta ludd Huzzah till din dator](media/iot-hub-arduino-huzzah-esp8266-get-started/9_connect-feather-huzzah-computer.png)

### <a name="add-serial-port-permissions-ubuntu-only"></a>Lägg till behörigheter för seriell port (Ubuntu)


Om du använder Ubuntu, kontrollera att du har behörighet att fungera på den USB-port för Ludd HUZZAH ESP8266. Följ dessa steg för att lägga till behörigheter för seriell port:


1. Kör följande kommandon i en terminal:

   ```bash
   ls -l /dev/ttyUSB*
   ls -l /dev/ttyACM*
   ```

   Du får ett av följande utdata:

   * crw-rw---1 rot uucp xxxxxxxx
   * crw-rw---1 rot antingen xxxxxxxx

   Observera att i utdata, `uucp` eller `dialout` är gruppnamn för ägare av USB-port.

1. Lägg till användaren i gruppen genom att köra följande kommando:

   ```bash
   sudo usermod -a -G <group-owner-name> <username>
   ```

   `<group-owner-name>`är ägare gruppnamnet du hämtade i föregående steg. `<username>`är din Ubuntu användarnamn.

1. Logga ut från Ubuntu och loggar sedan in igen för att ändringen ska visas.

## <a name="collect-sensor-data-and-send-it-to-your-iot-hub"></a>Samla in sensordata och skicka den till din IoT-hubb

I det här avsnittet, distribuera och köra ett exempelprogram på ludd HUZZAH ESP8266. Exempelprogrammet blinkar Indikator på ludd HUZZAH ESP8266 och skickar temperatur- och fuktighetskonsekvens data som samlas in från DHT22 sensor till din IoT-hubb.

### <a name="get-the-sample-application-from-github"></a>Hämta exempelprogrammet från GitHub

Exempelprogrammet finns på GitHub. Klona lagringsplatsen prov som innehåller exempelprogrammet från GitHub. Följ dessa steg om du vill klona databasen exempel:

1. Öppna en kommandotolk eller ett terminalfönster.
1. Gå till en mapp där du vill att exempelprogrammet ska lagras.
1. Kör följande kommando:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-feather-huzzah-client-app.git
   ```

Installera paketet för Ludd HUZZAH ESP8266 i Arduino IDE:

1. Öppna mappen där exempelprogrammet lagras.
1. Öppna filen app.ino i mappen app i Arduino IDE.

   ![Öppna exempelprogrammet i Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/10_arduino-ide-open-sample-app.png)

1. Klicka i IDE Arduino **filen** > **inställningar**.
1. I den **inställningar** dialogrutan klickar du på ikonen bredvid den **ytterligare anslagstavlor Manager URL: er** rutan.
1. Ange följande URL i popup-fönstret och klicka sedan på **OK**.

   `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

   ![Peka på en paket-url i Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/11_arduino-ide-package-url.png)

1. I den **inställningar** dialogrutan klickar du på **OK**.
1. Klicka på **verktyg** > **Hanteringsstyrenhetens** > **anslagstavlor Manager**, och sök sedan efter esp8266.

   Anslagstavlor Manager anger att ESP8266 med en version av 2.2.0 eller senare är installerad.

   ![Esp8266 paketet har installerats](media/iot-hub-arduino-huzzah-esp8266-get-started/12_arduino-ide-esp8266-installed.png)

1. Klicka på **verktyg** > **Hanteringsstyrenhetens** > **Adafruit HUZZAH ESP8266**.

### <a name="install-necessary-libraries"></a>Installera nödvändiga bibliotek

1. I Arduino IDE, klickar du på **skiss** > **innehåller biblioteket** > **Hantera bibliotek**.
1. Sök efter följande biblioteket namn i taget. Klicka på för alla bibliotek som du hittar **installera**.
   * `AzureIoTHub`
   * `AzureIoTUtility`
   * `AzureIoTProtocol_MQTT`
   * `ArduinoJson`
   * `DHT sensor library`
   * `Adafruit Unified Sensor`

### <a name="dont-have-a-real-dht22-sensor"></a>Har en verklig DHT22 sensor?

Exempelprogrammet simulera temperatur- och fuktighetskonsekvens data om du inte har en verklig DHT22 sensor. Följ dessa steg om du vill konfigurera exempelprogrammet använda simulerade data:

1. Öppna den `config.h` filen i den `app` mapp.
1. Leta upp följande kodrad och ändrar du värdet från `false` till `true`:
   ```c
   define SIMULATED_DATA true
   ```
   ![Konfigurera exempelprogrammet använda simulerade data](media/iot-hub-arduino-huzzah-esp8266-get-started/13_arduino-ide-configure-app-use-simulated-data.png)

1. Spara filen med `Control-s`.

### <a name="deploy-the-sample-application-to-feather-huzzah-esp8266"></a>Distribuera exempelprogrammet till ludd HUZZAH ESP8266

1. Klicka i IDE Arduino **verktyget** > **Port**, och klicka sedan på den seriella porten för Ludd HUZZAH ESP8266.
1. Klicka på **skiss** > **överför** att skapa och distribuera exempelprogrammet till ludd HUZZAH ESP8266.

### <a name="enter-your-credentials"></a>Ange autentiseringsuppgifter

När överföringen är klar följer du stegen nedan för att ange dina autentiseringsuppgifter:

1. Klicka i IDE Arduino **verktyg** > **seriella övervakaren**.
1. Observera två listrutorna i det nedre högra hörnet i fönstret seriella monitor.
1. Välj **ingen rad avslutas** för den vänstra listrutan.
1. Välj **115200 baud** för höger nedrullningsbara listan.
1. Ange följande information i inmatningsrutan som visas överst i fönstret seriell, om du uppmanas att ange dem och klickar sedan på **skicka**.
   * Wi-Fi SSID
   * Wi-Fi-lösenord
   * Anslutningssträngen för enhet

> [!Note]
> Information om autentiseringsuppgifter lagras i EEPROM av ludd HUZZAH ESP8266. Om du klickar på återställningsknappen på ludd HUZZAH ESP8266-kort frågar exempelprogrammet om du vill radera informationen. Ange `Y` att ha informationen raderas. Du uppmanas att ange informationen som en andra gång.

### <a name="verify-the-sample-application-is-running-successfully"></a>Kontrollera exempelprogrammet körts

Om du ser i följande utdata från den seriella övervakningsfönstret och blinkande Indikator på ludd HUZZAH ESP8266 exempelprogrammet körts.

![Slutversionen i Arduino IDE](media/iot-hub-arduino-huzzah-esp8266-get-started/14_arduino-ide-final-output.png)

## <a name="next-steps"></a>Nästa steg

Du har anslutit en ludd HUZZAH ESP8266 till din IoT-hubb, och skickas avbildade sensordata till din IoT-hubb. 

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

