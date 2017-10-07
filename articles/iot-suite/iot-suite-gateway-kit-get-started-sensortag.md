---
title: aaaConnect gateway-tooAzure IoT Suite med en Intel NUC | Microsoft Docs
description: "Använd hello Microsoft IoT kommersiella Gateway Kit och hello remote förkonfigurerade övervakningslösning. Använd hello Azure IoT Edge gateway tooenable en SensorTag enhet tooconnect toohello remote övervakningslösning, skicka telemetri toohello molnet och åtgärda toomethods anropas från hello lösning instrumentpanelen."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: dobett
ms.openlocfilehash: 6f98ee3c1e2311a8644da9d72d40e671e7cbcf00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-azure-iot-edge-gateway-toohello-remote-monitoring-preconfigured-solution-and-send-telemetry-from-a-sensortag"></a>Anslut dina Azure IoT Edge gateway toohello remote förkonfigurerade övervakningslösning och skicka telemetri från en SensorTag

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

Den här kursen visar hur toouse Azure IoT kant toosend temperatur- och fuktighetskonsekvens data från SensorTag toohello remote enhetsövervakning förkonfigurerade lösningen. Hej SensorTag ansluter toohello Intel NUC gateway med hjälp av Bluetooth. hello självstudiekursen används:

- Azure IoT kant tooimplement en exempel-gateway.
- Hej IoT Suite fjärrövervaknings förkonfigurerade lösning som hello molnbaserade serverdel.

## <a name="overview"></a>Översikt

Du har slutfört hello följa stegen i den här självstudiekursen:

- Distribuera en instans av enligt förkonfigurerade lösningen tooyour i hello fjärråtkomst övervakning Azure-prenumeration. Det här steget kan du automatiskt distribuerar och konfigurerar Azure-tjänster.
- Ställ in Intel NUC gateway-enhet toocommunicate med datorn och hello remote övervakningslösning.
- Konfigurera din Intel NUC gateway tooreceive telemetri från en SensorTag-enhet och skicka den toohello fjärråtkomst övervakning instrumentpanel.

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

[Texas Instruments TIVERA SensorTag][lnk-sensortag]. Den här självstudiekursen hämtar telemetridata via Bluetooth från hello SensorTag enhet.

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> hello fjärråtkomst övervakning lösning tillhandahåller en uppsättning Azure-tjänster i din Azure-prenumeration. hello distribution visar en verklig enterprise-arkitektur. tooavoid onödiga Azure-förbrukningen kostnader, ta bort din instans av hello förkonfigurerade lösningen i azureiotsuite.com när du är klar med den. Om du behöver hello förkonfigurerade lösningen igen, kan du enkelt återskapa den. Läs mer om att minskas när hello fjärråtkomst övervakning lösningen körs [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config].

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

## <a name="configure-bluetooth-connectivity"></a>Konfigurera Bluetooth-anslutning

Konfigurerar Bluetooth på hello Intel NUC tooenable hello SensorTag enhet tooconnect och skicka telemetri.

### <a name="find-hello-mac-address-of-hello-sensortag"></a>Hitta hello MAC-adressen för hello SensorTag

1. Hello shell på hello Intel NUC, kör hello efter kommandot toounblock hello Bluetooth-tjänst:

    ```bash
    sudo rfkill unblock bluetooth
    ```

1. Kör hello följande kommandon toostart hello Bluetooth-tjänsten på hello Intel NUC och ange hello Bluetooth shell:

    ```bash
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. Kör hello efter kommandot toopower på hello Bluetooth domänkontrollant:

    ```bash
    power on
    ```

    När hello domänkontrollant är aktiverat kan du se ett meddelande **ändra power på lyckades**.

1. Kör hello efter kommandot tooscan för närliggande Bluetooth-enheter:

    ```bash
    scan on
    ```

1. Tryck på hello power-knappen på hello SensorTag toomake den kan identifieras. hello grön Indikator blinkar.

1. När du ser ett meddelande i hello shell hello styrenheten har identifierat hello SensorTag, anteckna hello hello enhetens MAC-adress. hello MAC-adress som ser ut som **A0:E6:F8:B5:F6:00**. Behöver du hello MAC-adress senare i självstudiekursen hello när du konfigurerar hello-gateway.

1. Kör hello efter kommandot tooturn av Bluetooth-sökning:

    ```bash
    scan off
    ```

1. Kör hello efter kommandot tooverify att du kan ansluta toohello SensorTag-enheter:

    ```bash
    connect <SensorTag MAC address>
    ```

    Om du ansluter har hello shell visar hello-meddelande **lyckad anslutning** och skriver ut information om hello SensorTag enhet. Om du inte kan ansluta Kontrollera hello SensorTag fortfarande är påslagen.

1. Du kan nu koppla från hello SensorTag och avsluta hello Bluetooth-gränssnittet genom att köra följande kommandon hello:

    ```bash
    disconnect
    exit
    ```

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-software](../../includes/iot-suite-gateway-kit-prepare-nuc-software.md)]

## <a name="build-hello-custom-iot-edge-module"></a>Skapa hello anpassad IoT kant-modul

Du kan nu skapa hello anpassad IoT kant-modul som möjliggör hello gateway toosend meddelanden toohello remote övervakningslösning. Mer information om hur du konfigurerar en gateway- och IoT-Edge moduler finns [Azure IoT kant begrepp][lnk-gateway-concepts].

Hämta hello källkoden för hello anpassade IoT kant moduler från GitHub använder hello följande kommandon:

```bash
cd ~
git clone https://github.com/Azure-Samples/iot-remote-monitoring-c-intel-nuc-gateway-getting-started.git
```

Skapa hello anpassade IoT kant-modul med hello följande kommandon:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

hello build-skript placerar hello libsensor2remotemonitoring.so anpassad IoT kant-modul i hello build-mappen.

## <a name="configure-and-run-hello-iot-edge-gateway"></a>Konfigurera och köra hello IoT gräns-gatewayen

Du kan nu konfigurera hello IoT Edge gateway toosend telemetri från dina SensorTag enhet tooyour fjärråtkomst övervakning instrumentpanel. Mer information om hur du konfigurerar en gateway- och IoT-Edge moduler finns [Azure IoT kant begrepp][lnk-gateway-concepts].

> [!TIP]
> I den här kursen använder du hello standard `vi` textredigerare på hello Intel NUC. Om du inte har använt `vi` innan, bör du genomföra en inledande vägledning som [Unix - hello vi Editor kursen] [ lnk-vi-tutorial] toofamiliarize dig med den här redigeraren. Du kan också installera hello mer användarvänlig [nano](https://www.nano-editor.org/) editor hello kommandot `smart install nano -y`.

Öppna hello exempelkonfigurationsfilen i hello **vi** redigeraren med hello följande kommando:

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic/remote_monitoring.json
```

Leta upp följande rader i hello konfiguration för hello IoTHub modulen hello:

```json
"args": {
  "IoTHubName": "<<Azure IoT Hub Name>>",
  "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
  "Transport": "http"
}
```

Ersätt hello platshållare för värden med hello IoT-hubb information du skapat och sparat på hello början av den här kursen. hello-värdet för IoTHubName ser ut som **yourrmsolution37e08**, och hello värdet för IoTSuffix är vanligtvis **azure devices.net**.

Leta upp följande rader i hello konfiguration för hello mappningsmodul hello:

```json
args": [
  {
    "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

Ersätt hello **macAddress** med hello MAC-adressen för din SensorTag som du antecknade tidigare. Ersätt hello **deviceID** och **deviceKey** platshållarna med hello-ID: N och nycklarna för hello två enheter som du skapade tidigare i hello remote övervakningslösning.

Leta upp följande rader i hello konfiguration för hello SensorTag modulen hello:

```json
"args": {
  "controller_index": 0,
  "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
  ...
}
```

Ersätt hello **enhet\_mac\_adress** med hello MAC-adressen för din SensorTag som du antecknade tidigare.

Spara ändringarna.

Du kan nu köra hello gateway med hello följande kommandon:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
/usr/share/azureiotgatewaysdk/samples/ble_gateway/ble_gateway remote_monitoring.json
```

Hej IoT gräns-gatewayen på hello Intel NUC påbörjas och skickar telemetri från hello SensorTag toohello remote övervakningslösning:

![IoT-gräns-gatewayen skickar telemetri från hello SensorTag][img-telemetry]

Tryck på **Ctrl-C** tooexit hello program när som helst.

## <a name="view-hello-telemetry"></a>Visa hello telemetri

hello-gateway nu skicka telemetri från hello SensorTag enhet toohello remote övervakningslösning. Du kan visa hello telemetri på instrumentpanelen för hello-lösning. Du kan också skicka kommandon tooyour SensorTag enheten via hello gateway hello lösning instrumentpanel.

- Navigera toohello lösning instrumentpanelen.
- Välj hello-enhet som du konfigurerade i hello-gateway som representerar hello SensorTag i hello **enhet tooView** listrutan.
- hello telemetri från hello SensorTag enhet visar hello instrumentpanelen.

![Visa telemetri från hello SensorTag-enheter][img-telemetry-display]

> [!WARNING]
> Om du lämnar hello remote övervakningslösning som körs i ditt Azure-konto, debiteras du för hello gång den körs. Läs mer om att minskas när hello fjärråtkomst övervakning lösningen körs [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config]. Ta bort hello förkonfigurerade lösningen från ditt Azure-konto när du är klar.


## <a name="next-steps"></a>Nästa steg

Besök hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) fler exempel och dokumentation om Azure IoT.

[img-telemetry]: ./media/iot-suite-gateway-kit-get-started-sensortag/appoutput.png


[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-sensortag/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-sensortag]: http://www.ti.com/ww/en/wireless_connectivity/sensortag/
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started