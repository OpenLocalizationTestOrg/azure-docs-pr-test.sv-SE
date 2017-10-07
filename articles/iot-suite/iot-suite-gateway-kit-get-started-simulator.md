---
title: aaaConnect gateway-tooAzure IoT Suite med en Intel NUC | Microsoft Docs
description: "Använd hello Microsoft IoT kommersiella Gateway Kit och hello remote förkonfigurerade övervakningslösning. Använd hello Azure IoT Edge gateway tooconnect toohello remote övervakningslösning, skicka simulerade telemetri toohello molnet och besvara toomethods anropas från hello lösning instrumentpanelen."
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
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 46b545fc21b054191c8f78ace20fc628f839a819
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-azure-iot-edge-gateway-toohello-remote-monitoring-preconfigured-solution-and-send-simulated-telemetry"></a>Anslut dina Azure IoT Edge gateway toohello remote förkonfigurerade övervakningslösning och skicka telemetri om simulerad

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

Den här kursen visar hur toouse Azure IoT kant toosimulate temperatur- och fuktighetskonsekvens data toosend toohello fjärrövervaknings förkonfigurerade lösningen. hello självstudiekursen används:

- Azure IoT kant tooimplement en exempel-gateway.
- Hej IoT Suite fjärrövervaknings förkonfigurerade lösning som hello molnbaserade serverdel.

## <a name="overview"></a>Översikt

Du har slutfört hello följa stegen i den här självstudiekursen:

- Distribuera en instans av enligt förkonfigurerade lösningen tooyour i hello fjärråtkomst övervakning Azure-prenumeration. Det här steget kan du automatiskt distribuerar och konfigurerar Azure-tjänster.
- Ställ in Intel NUC gateway-enhet toocommunicate med datorn och hello remote övervakningslösning.
- Konfigurera hello IoT Edge gateway toosend simulerade telemetri som kan visas på instrumentpanelen för hello-lösning.

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> hello fjärråtkomst övervakning lösning tillhandahåller en uppsättning Azure-tjänster i din Azure-prenumeration. hello distribution visar en verklig enterprise-arkitektur. tooavoid onödiga Azure-förbrukningen kostnader, ta bort din instans av hello förkonfigurerade lösningen i azureiotsuite.com när du är klar med den. Om du behöver hello förkonfigurerade lösningen igen, kan du enkelt återskapa den. Läs mer om att minskas när hello fjärråtkomst övervakning lösningen körs [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config].

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

Upprepa hello föregående steg tooadd en andra enhet med enhets-ID, till exempel **device02**. hello exemplet skickar data från två simulerade enheter i hello gateway toohello remote övervakningslösning.

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

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
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

hello build-skript placerar hello libsimulator.so anpassad IoT kant-modul i hello build-mappen.

## <a name="configure-and-run-hello-iot-edge-gateway"></a>Konfigurera och köra hello IoT gräns-gatewayen

Du kan nu konfigurera hello IoT Edge gateway toosend simulerade telemetri tooyour remote instrumentpanelen för övervakning. Mer information om hur du konfigurerar en gateway- och IoT-Edge moduler finns [Azure IoT kant begrepp][lnk-gateway-concepts].

> [!TIP]
> I den här kursen använder du hello standard `vi` textredigerare på hello Intel NUC. Om du inte har använt `vi` innan, bör du genomföra en inledande vägledning som [Unix - hello vi Editor kursen] [ lnk-vi-tutorial] toofamiliarize dig med den här redigeraren. Du kan också installera hello mer användarvänlig [nano](https://www.nano-editor.org/) editor hello kommandot `smart install nano -y`.

Öppna hello exempelkonfigurationsfilen i hello **vi** redigeraren med hello följande kommando:

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator/remote_monitoring.json
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
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  },
  {
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

Ersätt hello **deviceID** och **deviceKey** platshållarna med hello-ID: N och nycklarna för hello två enheter som du skapade tidigare i hello remote övervakningslösning.

Spara ändringarna.

Du kan nu köra hello IoT gräns-gatewayen med hello följande kommandon:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
/usr/share/azureiotgatewaysdk/samples/simulated_device_cloud_upload/simulated_device_cloud_upload remote_monitoring.json
```

hello gateway startar på hello Intel NUC och skickar simulerade telemetri toohello remote övervakningslösning:

![IoT-gräns-gatewayen genererar simulerade telemetri][img-simulated telemetry]

Tryck på **Ctrl-C** tooexit hello program när som helst.

## <a name="view-hello-telemetry"></a>Visa hello telemetri

Hej IoT gräns-gatewayen nu skickar simulerade telemetri toohello remote övervakningslösning. Du kan visa hello telemetri på instrumentpanelen för hello-lösning.

- Navigera toohello lösning instrumentpanelen.
- Välj en av hello två enheter som du konfigurerade i hello gateway i hello **enhet tooView** listrutan.
- hello telemetri från hello gatewayenheter visar hello instrumentpanelen.

![Visa telemetri från hello simulerade gateway-enheter][img-telemetry-display]

> [!WARNING]
> Om du lämnar hello remote övervakningslösning som körs i ditt Azure-konto, debiteras du för hello gång den körs. Läs mer om att minskas när hello fjärråtkomst övervakning lösningen körs [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config]. Ta bort hello förkonfigurerade lösningen från ditt Azure-konto när du är klar.

## <a name="next-steps"></a>Nästa steg

Besök hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) fler exempel och dokumentation om Azure IoT.

[img-simulated telemetry]: ./media/iot-suite-gateway-kit-get-started-simulator/appoutput.png

[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-simulator/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started