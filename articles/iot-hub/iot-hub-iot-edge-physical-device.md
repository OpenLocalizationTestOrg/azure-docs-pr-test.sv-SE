---
title: aaaUse en fysisk enhet med Azure IoT kant | Microsoft Docs
description: "Hur toouse en Texas instrument SensorTag enhet toosend data tooan IoT-hubb via en IoT-Edge gateway körs på en hallon Pi 3-enhet. hello gateway skapas med Azure IoT kant."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 212dacbf-e5e9-48b2-9c8a-1c14d9e7b913
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2017
ms.author: andbuc
ms.openlocfilehash: a2385accdbd99012ad094232653ee47d4e5c7839
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-iot-edge-on-a-raspberry-pi-tooforward-device-to-cloud-messages-tooiot-hub"></a>Använda Azure IoT kanten på en hallon Pi tooforward meddelanden från enhet till moln tooIoT Hub

Den här genomgången av hello [Bluetooth låg energiförbrukning exempel] [ lnk-ble-samplecode] visas hur toouse [Azure IoT kant] [ lnk-sdk] till:

* Vidarebefordra enhet till moln telemetri tooIoT hubb från en fysisk enhet.
* Väg kommandon från IoT-hubb tooa fysisk enhet.

Den här genomgången omfattar:

* **Arkitektur för**: viktig arkitektur information om hello Bluetooth låg energiförbrukning exempel.
* **Skapa och köra**: hello steg krävs toobuild och kör hello exempel.

## <a name="architecture"></a>Arkitektur

hello genomgången visar hur toobuild och kör en IoT-gräns-gatewayen på en hallon Pi 3 som kör Raspbian Linux. hello gateway skapas med IoT kant. hello används en Texas instrument SensorTag Bluetooth låg energiförbrukning (TIVERA) enheten toocollect temperatur data.

När du kör hello IoT gräns-gatewayen den:

* Ansluter tooa SensorTag-enhet med hello Bluetooth låg energiförbrukning (a)-protokollet.
* Ansluter tooIoT hubb med hello HTTP-protokollet.
* Vidarebefordrar telemetri från hello SensorTag enhet tooIoT hubb.
* Dirigerar kommandon från IoT-hubb toohello SensorTag-enheter.

hello gateway innehåller hello följande IoT kant moduler:

* En *TIVERA modulen* som kontakt med en tabell tooreceive temperatur enhetsdata från hello enheten och skicka kommandon toohello enhet.
* En *TIVERA molnet toodevice modulen* som omvandlar JSON-meddelanden som skickats från IoT-hubb i TIVERA instruktioner för hello *TIVERA modulen*.
* En *loggaren modulen* som loggar alla gateway meddelanden tooa lokal fil.
* En *identitet mappningsmodul* som omvandlar mellan TIVERA enhetens MAC-adresser och identiteter för Azure IoT Hub-enhet.
* En *IoT-hubb modulen* som överför telemetri data tooan IoT-hubb och tar emot enhetskommandon från en IoT-hubb.
* En *TIVERA skrivare modulen* som tolkar telemetri från hello TIVERA enhet och skriver ut formaterade data toohello konsolen tooenable felsökning.

### <a name="how-data-flows-through-hello-gateway"></a>Hur data flödar via hello gateway

hello följande blockdiagram visar hello telemetri överför data flödet pipeline:

![Telemetri överför gateway pipeline](media/iot-hub-iot-edge-physical-device/gateway_ble_upload_data_flow.png)

hello stegen som ett objekt av telemetri tar reser från en tabell enheten tooIoT navet är:

1. hello TIVERA enheten genererar ett exempel på en temperatur och skickar det via Bluetooth toohello TIVERA modul i hello gateway.
1. hello TIVERA modulen tar emot hello exempel och publicerar den toohello broker tillsammans med hello MAC-adressen för hello enhet.
1. hello identitet mappningsmodul hämtar det här meddelandet och använder en intern tabell tootranslate hello hello enhetens MAC-adress till en IoT-hubb enhetens identitet. En IoT-hubb enhetsidentitet består av en enhets-ID och nyckeln för enheten.
1. hello identitet mappningsmodul publicerar ett nytt meddelande som innehåller hello temperatur exempeldata, hello MAC-adressen för hello enhet, hello enhets-ID och nyckeln för hello-enheten.
1. Hej IoT-hubb modulen får ett nytt meddelande (som genereras av hello identitet mappningsmodul) och publicerar den tooIoT hubb.
1. hello loggaren modulen loggar alla meddelanden från hello broker tooa lokal fil.

hello följande blockdiagram visar hello enheten kommandot data flödet pipeline:

![Enheten kommandot gateway pipeline](media/iot-hub-iot-edge-physical-device/gateway_ble_command_data_flow.png)

1. Hej IoT-hubb modulen avsöker med jämna mellanrum hello IoT-hubb för nya kommandomeddelanden.
1. När hello IoT-hubb modulen tar emot ett nytt kommando-meddelande, publicerar den den toohello broker.
1. hello identitet mappningsmodul hämtar kommandot hälsningsmeddelande och använder en intern tabell tootranslate hello IoT-hubb enheten ID tooa enhetens MAC-adress. Den publicerar sedan ett nytt meddelande som innehåller hello MAC-adressen för hello målenhet i hello egenskaper karta över hello-meddelande.
1. hello TIVERA moln till enhet modul hämtar det här meddelandet och översätter till hello rätt Bell-instruktion för hello TIVERA modulen. Den publicerar sedan ett nytt meddelande.
1. hello TIVERA modul hämtar det här meddelandet och utför hello i/o-instruktion genom att kommunicera med hello TIVERA enhet.
1. hello loggaren modulen loggar alla meddelanden från hello broker tooa fil.

## <a name="prerequisites"></a>Krav

toocomplete den här självstudiekursen kommer du behöver en aktiv Azure-prenumeration.

> [!NOTE]
> Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk-free-trial].

Du måste SSH-klienten på den stationära datorn tooenable du tooremotely access hello-kommando rad på hello hallon Pi.

- Windows innehåller inte en SSH-klient. Vi rekommenderar att du använder [PuTTY](http://www.putty.org/).
- De flesta distributioner för Linux och Mac OS inkluderar hello SSH-kommandoradsverktyget. Mer information finns i [SSH med Linux- eller Mac OS x](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

## <a name="prepare-your-hardware"></a>Förbered maskinvaran

Den här kursen förutsätter att du använder en [Texas instrument SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) enheten ansluten tooa hallon Pi 3 kör Raspbian.

### <a name="install-raspbian"></a>Installera Raspbian

Du kan använda något av följande alternativ tooinstall Raspbian på enheten hallon Pi 3 hello.

* tooinstall hello senaste versionen av Raspbian, Använd hello [NOOBS] [ lnk-noobs] grafiskt användargränssnitt.
* Manuellt [hämta] [ lnk-raspbian] och skriva hello senaste bild av hello Raspbian operativsystemet tooan SD-kort.

### <a name="sign-in-and-access-hello-terminal"></a>Logga in och komma åt hello terminal

Du har två alternativ tooaccess en miljö med terminal på din hallon Pi:

* Om du har ett tangentbord och övervaka anslutna tooyour hallon Pi kan du använda hello Raspbian GUI tooaccess ett terminalfönster.

* Åtkomst hello kommandoraden på din hallon Pi använder SSH från den stationära datorn.

#### <a name="use-a-terminal-window-in-hello-gui"></a>Använd ett terminalfönster i hello GUI

Hej standardautentiseringsuppgifterna för Raspbian är användarnamn **pi** och lösenord **hallon**. I Aktivitetsfältet i hello GUI hello kan du starta hello **Terminal** verktyget med hello-ikonen som ser ut som en bildskärm.

#### <a name="sign-in-with-ssh"></a>Logga in med SSH

Du kan använda SSH för kommandoradsåtkomst tooyour hallon Pi. hello artikel [SSH (Secure Shell)] [ lnk-pi-ssh] beskriver hur tooconfigure SSH på din hallon Pi, och hur tooconnect från [Windows] [ lnk-ssh-windows] eller [Linux och Mac OS][lnk-ssh-linux].

Logga in med användarnamn **pi** och lösenord **hallon**.

### <a name="install-bluez-537"></a>Installera BlueZ 5.37

hello TIVERA moduler prata toohello Bluetooth maskinvaran via hello BlueZ stacken. Du behöver version 5.37 av BlueZ för hello moduler toowork korrekt. Dessa instruktioner Kontrollera hello rätt version av BlueZ är installerat.

1. Stoppa hello aktuella Bluetooth-daemon:

    ```sh
    sudo systemctl stop bluetooth
    ```

1. Installera hello BlueZ beroenden:

    ```sh
    sudo apt-get update
    sudo apt-get install bluetooth bluez-tools build-essential autoconf glib2.0 libglib2.0-dev libdbus-1-dev libudev-dev libical-dev libreadline-dev
    ```

1. Hämta hello BlueZ källkod från bluez.org:

    ```sh
    wget http://www.kernel.org/pub/linux/bluetooth/bluez-5.37.tar.xz
    ```

1. Packa upp hello källkoden:

    ```sh
    tar -xvf bluez-5.37.tar.xz
    ```

1. Ändra kataloger toohello nyskapad mapp:

    ```sh
    cd bluez-5.37
    ```

1. Konfigurera hello BlueZ kod toobe inbyggda:

    ```sh
    ./configure --disable-udev --disable-systemd --enable-experimental
    ```

1. Skapa BlueZ:

    ```sh
    make
    ```

1. Installera BlueZ när det är klart byggnads:

    ```sh
    sudo make install
    ```

1. Ändra systemd tjänstkonfigurationen för bluetooth så att den pekar toohello nya Bluetooth-daemon i hello filen `/lib/systemd/system/bluetooth.service`. Ersätt hello 'ExecStart' rad med hello följande text:

    ```conf
    ExecStart=/usr/local/libexec/bluetooth/bluetoothd -E
    ```

### <a name="enable-connectivity-toohello-sensortag-device-from-your-raspberry-pi-3-device"></a>Aktivera anslutningen toohello SensorTag enhet från enheten hallon Pi 3

Innan du kör hello exemplet måste tooverify att din hallon Pi 3 kan ansluta toohello SensorTag-enheter.

1. Se till att hello `rfkill` verktyg installeras:

    ```sh
    sudo apt-get install rfkill
    ```

1. Tillåt bluetooth på hello hallon Pi 3 och kontrollera att hello versionsnumret är **5.37**:

    ```sh
    sudo rfkill unblock bluetooth
    bluetoothctl --version
    ```

1. tooenter hello interaktiva bluetooth shell starta hello Bluetooth-tjänsten och kör hello **bluetoothctl** kommando:

    ```sh
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. Ange hello kommando **effekt på** toopower in hello Bluetooth-styrenhet. hello kommando returnerar utdata liknande toohello följande:

    ```sh
    [NEW] Controller 98:4F:EE:04:1F:DF C3 raspberrypi [default]
    ```

1. Hello interaktiva bluetooth Shell, ange hello kommando **genomsökning på** tooscan Bluetooth-enheter. hello kommando returnerar utdata liknande toohello följande:

    ```sh
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

1. Gör hello SensorTag enheten kan upptäckas genom att trycka på hello liten knapp (hello grön Indikator bör flash). hello hallon Pi 3 ska identifiera hello SensorTag enhet:

    ```sh
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```

    I det här exemplet ser du att hello MAC-adressen för hello SensorTag enheten är **A0:E6:F8:B5:F6:00**.

1. Inaktivera genomsökning genom att ange hello **genomsökning av** kommando:

    ```sh
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

1. Anslut tooyour SensorTag-enhet med hjälp av dess MAC-adress genom att ange **ansluta \<MAC-adress\>**. följande exempel på utdata hello förkortas för tydlighetens skull:

    ```sh
    Attempting tooconnect tooA0:E6:F8:B5:F6:00
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: yes
    Connection successful
    [CHG] Device A0:E6:F8:B5:F6:00 UUIDs: 00001800-0000-1000-8000-00805f9b34fb
    ...
    [NEW] Primary Service
            /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
            Device Information
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 GattServices: /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 Name: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Alias: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Modalias: bluetooth:v000Dp0000d0110
    ```

    > Du kan ange hello GATT egenskaper hello enheten igen med hello **listan attribut** kommando.

1. Du kan nu koppla från hello-enhet med hjälp av hello **koppla från** kommandot och avsluta hello bluetooth shell med hello **avsluta** kommando:

    ```sh
    Attempting toodisconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

Nu är du redo toorun hello TIVERA IoT kant prov på din hallon Pi 3.

## <a name="run-hello-iot-edge-ble-sample"></a>Kör hello IoT kant TIVERA exempel

toorun hello IoT kant TIVERA exemplet behöver du toocomplete tre uppgifter:

* Konfigurera två exempel enheter i din IoT-hubb.
* Skapa IoT kanten på enheten hallon Pi 3.
* Konfigurera och köra hello TIVERA exemplet på enheten hallon Pi 3.

När hello skrivning stöder IoT kant endast TIVERA moduler i gateways som körs på Linux.

### <a name="configure-two-sample-devices-in-your-iot-hub"></a>Konfigurera två exempel enheter i din IoT-hubb

* [Skapa en IoT-hubb] [ lnk-create-hub] i din Azure-prenumeration behöver du hello namnet på din hubb toocomplete den här genomgången. Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.
* Lägg till en enhet som kallas **SensorTag_01** tooyour IoT-hubb och anteckna dess id och enheten nyckel. Du kan använda hello [enheten explorer eller iothub-explorer] [ lnk-explorer-tools] verktyg tooadd för den här enheten toohello IoT-hubb som du skapade i föregående steg i hello och tooretrieve dess nyckel. Du kan mappa enheten toohello SensorTag enheten när du konfigurerar hello gateway.

### <a name="build-azure-iot-edge-on-your-raspberry-pi-3"></a>Skapa Azure IoT kanten på din Raspberry Pi 3

Installera beroenden för Azure IoT kant:

```sh
sudo apt-get install cmake uuid-dev curl libcurl4-openssl-dev libssl-dev
```

Använd hello följande kommandon tooclone IoT kant och alla dess submodules tooyour-hemkataloger:

```sh
cd ~
git clone https://github.com/Azure/iot-edge.git
```

När du har en fullständig kopia av hello IoT kant databasen på din hallon Pi 3 kan du skapa den med hjälp av följande kommando från hello-mapp som innehåller hello SDK hello:

```sh
cd ~/iot-edge
./tools/build.sh  --disable-native-remote-modules
```

### <a name="configure-and-run-hello-ble-sample-on-your-raspberry-pi-3"></a>Konfigurera och köra hello TIVERA prov på din hallon Pi 3

toobootstrap och kör hello exempel, måste du konfigurera varje IoT kant-modul som deltar i hello gateway. Den här konfigurationen finns i en JSON-fil och du måste konfigurera alla fem deltagande IoT kant moduler. Det finns ett exempel JSON-fil i hello databas kallas **gateway\_sample.json** som du kan använda som hello som startpunkt för att skapa egna konfigurationsfilen. Den här filen har hello **src-exempel/ble_gateway** mapp i lokal kopia av hello IoT kant-databasen.

hello följande avsnitt beskrivs hur tooedit den här konfigurationen för hello TIVERA exempel och förutsätter att hello IoT kant databasen i hello **/home/pi/iot-edge /** mapp på din hallon Pi 3. Om hello databasen är någon annanstans, justera hello sökvägar i enlighet med detta.

#### <a name="logger-configuration"></a>Loggaren konfiguration

Under förutsättning att hello gateway-databasen finns i hello **/home/pi/iot-edge /** mapp, konfigurera hello loggaren modulen på följande sätt:

```json
{
  "name": "Logger",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path" : "build/modules/logger/liblogger.so"
    }
  },
  "args":
  {
    "filename": "<</path/to/log-file.log>>"
  }
}
```

#### <a name="ble-module-configuration"></a>TIVERA Modulkonfiguration

hello exempelkonfiguration för hello TIVERA enhet förutsätter en Texas instrument SensorTag-enhet. Alla standard Bell-enheter som kan fungera som en kringutrustning GATT bör fungera, men du måste kanske tooupdate hello GATT egenskaps-ID: N och data. Lägg till hello MAC-adressen för SensorTag-enheter:

```json
{
  "name": "SensorTag",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/ble/libble.so"
    }
  },
  "args": {
    "controller_index": 0,
    "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
    "instructions": [
      {
        "type": "read_once",
        "characteristic_uuid": "00002A24-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A25-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A26-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A27-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A28-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A29-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "write_at_init",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AQ=="
      },
      {
        "type": "read_periodic",
        "characteristic_uuid": "F000AA01-0451-4000-B000-000000000000",
        "interval_in_ms": 1000
      },
      {
        "type": "write_at_exit",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AA=="
      }
    ]
  }
}
```

Om du inte använder en SensorTag-enhet, granska hello dokumentationen för din enhet toodetermine för tabell om du behöver tooupdate hello GATT egenskaps-ID: N och datavärden.

#### <a name="iot-hub-module"></a>Modul för IoT-hubb

Lägg till hello namn för din IoT-hubb. Hej postsuffixvärdet är vanligtvis **azure devices.net**:

```json
{
  "name": "IoTHub",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/iothub/libiothub.so"
    }
  },
  "args": {
    "IoTHubName": "<<Azure IoT Hub Name>>",
    "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
    "Transport" : "amqp"
  }
}
```

#### <a name="identity-mapping-module-configuration"></a>Identitet mappning konfiguration

Lägg till hello MAC-adressen för din SensorTag enhet och hello enhets-ID och nyckeln för hello **SensorTag_01** enhet som du har lagt till tooyour IoT-hubb:

```json
{
  "name": "mapping",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/identitymap/libidentity_map.so"
    }
  },
  "args": [
    {
      "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
      "deviceId": "<<Azure IoT Hub Device ID>>",
      "deviceKey": "<<Azure IoT Hub Device Key>>"
    }
  ]
}
```

#### <a name="ble-printer-module-configuration"></a>Modulkonfiguration TIVERA skrivare

```json
{
  "name": "BLE Printer",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/samples/ble_gateway/ble_printer/libble_printer.so"
    }
  },
  "args": null
}
```

#### <a name="blec2d-module-configuration"></a>BLEC2D konfiguration

```json
{
  "name": "BLEC2D",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/ble/libble_c2d.so"
    }
  },
  "args": null
}
```

#### <a name="routing-configuration"></a>Routningskonfiguration

hello följande konfigurationen garanterar hello följande routning mellan IoT kant moduler:

* Hej **loggaren** modulen tar emot och loggar alla meddelanden.
* Hej **SensorTag** modulen skickar meddelanden tooboth hello **mappning** och **TIVERA skrivare** moduler.
* Hej **mappning** modulen skickar meddelanden toohello **IoTHub** modulen toobe skickas upp tooyour IoT-hubb.
* Hej **IoTHub** modulen skickar tillbaka toohello **mappning** modul.
* Hej **mappning** modulen skickar meddelanden toohello **BLEC2D** modul.
* Hej **BLEC2D** modulen skickar tillbaka toohello **Sensor Tag** modul.

```json
"links" : [
    {"source" : "*", "sink" : "Logger" },
    {"source" : "SensorTag", "sink" : "mapping" },
    {"source" : "SensorTag", "sink" : "BLE Printer" },
    {"source" : "mapping", "sink" : "IoTHub" },
    {"source" : "IoTHub", "sink" : "mapping" },
    {"source" : "mapping", "sink" : "BLEC2D" },
    {"source" : "BLEC2D", "sink" : "SensorTag"}
 ]
```

toorun hello exemplet pass hello sökvägen toohello JSON-konfigurationsfil som en parameter toohello **tivera\_gateway** binär. hello följande kommando förutsätter att du använder hello **gateway_sample.json** konfigurationsfilen. Kör kommandot från hello **iot-edge** mapp på hello hallon Pi:

```sh
./build/samples/ble_gateway/ble_gateway ./samples/ble_gateway/src/gateway_sample.json
```

Du kan behöva toopress hello små knappen på hello SensorTag enhet toomake den identifierbart innan du kör hello exempel.

När du kör hello exemplet, kan du använda hello [enheten explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) eller hello [iothub explorer](https://github.com/Azure/iothub-explorer) verktyget toomonitor hello meddelanden hello IoT gräns-gatewayen vidarebefordrar från hello SensorTag enhet. Till exempel kan Utforskaren iothub du övervaka meddelanden från enhet till moln med hello följande kommando:

```sh
iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
```

## <a name="send-cloud-to-device-messages"></a>Skicka meddelanden från moln till enhet

hello TIVERA modulen stöder också skicka kommandon från IoT-hubb toohello enhet. Du kan använda hello [enheten explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) eller hello [iothub explorer](https://github.com/Azure/iothub-explorer) verktyget toosend JSON meddelanden hello TIVERA gateway modulen vidarebefordrar på toohello TIVERA enhet.

Om du använder hello Texas instrument SensorTag-enheter kan aktivera du hello röd Indikator eller grön Indikator Summer genom att skicka kommandon från IoT-hubb. Innan du skickar kommandon från IoT-hubb först skicka hello följande två JSON-meddelanden i ordning. Du kan sedan skicka någon hello kommandon tooturn på hello ljus eller Summer.

1. Återställ alla indikatorer och hello Summer (inaktivera dem):

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

1. Konfigurera i/o som 'fjärr':

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

Nu kan du skicka något av följande kommandon tooturn på hello ljus eller Summer på hello SensorTag enhet hello:

* Aktivera hello röd Indikator:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

* Aktivera hello grön Indikator:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

* Aktivera hello Summer:

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

## <a name="next-steps"></a>Nästa steg

Om du vill toogain en större kunskap om IoT kant och experimentera med vissa kodexempel finns hello följande developer självstudier och resurser:

* [Azure IoT kant][lnk-sdk]

toofurther utforska hello funktionerna i IoT Hub, se:

* [Utvecklarhandbok för IoT-hubb][lnk-devguide]

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/iot-edge/tree/master/samples/ble_gateway
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-sdk]: https://github.com/Azure/iot-edge/
[lnk-noobs]: https://www.raspberrypi.org/documentation/installation/noobs.md
[lnk-raspbian]: https://www.raspberrypi.org/downloads/raspbian/
[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
