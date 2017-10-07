---
title: aaaConnect en hallon Pi tooAzure IoT Suite med C toosupport firmware-uppdateringar | Microsoft Docs
description: "Använd hello startpaket för Microsoft Azure IoT för hello hallon Pi 3 och Azure IoT Suite. Använda C tooconnect din hallon Pi toohello remote övervakningslösning, skicka telemetri från sensorer toohello molnet och utföra en fjärransluten firmware-uppdatering."
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
ms.openlocfilehash: 36d39c6d754ddb025fd3f6b74d7795ed907b754c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-enable-remote-firmware-updates-using-c"></a>Anslut din fjärranslutna övervakningslösning hallon Pi 3 toohello och aktivera fjärråtkomst firmware-uppdateringar med hjälp av C

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

Den här kursen visar hur toouse hello startpaket för Microsoft Azure IoT för hallon Pi 3:

* Utveckla en läsare för temperatur- och fuktighetskonsekvens som kan kommunicera med hello molnet.
* Aktivera och utföra ett klientprogram för fjärråtkomst firmware uppdatering tooupdate hello på hello hallon Pi.

hello självstudiekursen används:

* Raspbian OS hello C programmeringsspråket och hello Microsoft Azure IoT SDK för C tooimplement en exempel-enhet.
* Hej IoT Suite fjärrövervaknings förkonfigurerade lösning som hello molnbaserade serverdel.

## <a name="overview"></a>Översikt

Du har slutfört hello följa stegen i den här självstudiekursen:

* Distribuera en instans av enligt förkonfigurerade lösningen tooyour i hello fjärråtkomst övervakning Azure-prenumeration. Det här steget kan du automatiskt distribuerar och konfigurerar Azure-tjänster.
* Konfigurera din enhet och sensorer toocommunicate med datorn och hello remote övervakningslösning.
* Uppdatera hello exempel enhet kod tooconnect toohello remote övervakningslösning och skicka telemetri som kan visas på instrumentpanelen för hello-lösning.
* Använd hello enheten kod tooupdate hello klienten exempelprogrammet.

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> hello fjärråtkomst övervakning lösning tillhandahåller en uppsättning Azure-tjänster i din Azure-prenumeration. hello distribution visar en verklig enterprise-arkitektur. tooavoid onödiga Azure-förbrukningen kostnader, ta bort din instans av hello förkonfigurerade lösningen i azureiotsuite.com när du är klar med den. Om du behöver hello förkonfigurerade lösningen igen, kan du enkelt återskapa den. Läs mer om att minskas när hello fjärråtkomst övervakning lösningen körs [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config].

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-hello-sample"></a>Hämta och konfigurera hello-exempel

Du kan nu hämta och konfigurera hello fjärråtkomst övervakning klientprogrammet på din hallon Pi.

### <a name="clone-hello-repositories"></a>Klona hello databaser

Om du inte redan har gjort krävs klona hello databaser genom att köra hello följande kommandon på din Pi:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
```

### <a name="update-hello-device-connection-string"></a>Uppdatera anslutningssträngen för hello-enhet

Öppna hello exempelkonfigurationsfilen i hello **nano** redigeraren med hello följande kommando:

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/config/deviceinfo
```

Ersätt hello platshållarvärdena med hello-ID och IoT-hubb enhetsinformation du skapade och sparade hello början av den här kursen.

När du är klar hello innehållet i hello deviceinfo fil ska se ut så hello följande exempel:

```conf
yourdeviceid
HostName=youriothubname.azure-devices.net;DeviceId=yourdeviceid;SharedAccessKey=yourdevicekey
```

Spara dina ändringar (**Ctrl-O**, **RETUR**) och avsluta hello editor (**Ctrl + X**).

## <a name="build-hello-sample"></a>Skapa hello-exempel

Om du inte redan har gjort det hello nödvändiga paket installeras för hello Microsoft Azure IoT-enhet SDK för C genom att köra följande kommandon i en terminal på hello hallon Pi hello:

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

Du kan nu skapa hello exempellösning på hello hallon Pi:

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/1.0/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/1.0/build.sh
```

Du kan nu köra hello exempelprogrammet på hello hallon Pi. Ange hello-kommando:

  ```sh
  sudo ~/cmake/remote_monitoring/remote_monitoring
  ```

hello är följande exempel på utdata ett exempel på hello utdata som du ser i hello kommandotolk på hello hallon Pi:

![Utdata från Raspberry Pi-app][img-raspberry-output]

Tryck på **Ctrl-C** tooexit hello program när som helst.

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-advanced](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-advanced.md)]

1. I hello lösning instrumentpanelen, klickar du på **enheter** toovisit hello **enheter** sidan. Välj din hallon Pi i hello **enhetslistan**. Välj **metoder**:

    ![Lista över enheter i instrumentpanelen][img-list-devices]

1. På hello **anropa metoden** väljer **InitiateFirmwareUpdate** i hello **metoden** listrutan.

1. I hello **FWPackageURI** anger **https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit/raw/master/advanced/2.0/package/remote_monitoring.zip**. Det här arkivet innehåller hello implementering av version 2.0 av hello inbyggd programvara.

1. Välj **InvokeMethod**. hello-appen på hello hallon Pi skickar en bekräftelse tillbaka toohello lösning instrumentpanel. Därefter startar hello firmware-uppdateringen genom att hämta hello ny version av inbyggd programvara hello:

    ![Visa historiken för metoden][img-method-history]

## <a name="observe-hello-firmware-update-process"></a>Se hello uppdateringsprocessen för inbyggd programvara

Du kan se hello firmware uppdateringsprocessen när den körs på hello enhet och rapporterade egenskaper i hello lösning instrumentpanelen genom att visa hello:

1. Du kan visa hello förlopp i hello uppdateringsprocessen på hello hallon Pi:

    ![Visa förlopp för uppdatering][img-update-progress]

    > [!NOTE]
    > hello övervakning fjärrprogram startar om tyst när hello uppdateringen är klar. Kommandot hello `ps -ef` tooverify körs. Om du vill tooterminate hello processen, Använd hello `kill` med hello process-id.

1. Du kan visa hello status för hello firmware-uppdatering som rapporteras av hello-enhet i hello lösning portal. hello följande skärmbild visar hello status och varaktighet för varje steg i hello uppdateringsprocessen och hello nya version:

    ![Visa jobbstatus][img-job-status]

    Om du navigerar bakåt toohello instrumentpanelen kan du kontrollera hello enheten skickat fortfarande telemetri följande hello firmware-uppdatering.

> [!WARNING]
> Om du lämnar hello remote övervakningslösning som körs i ditt Azure-konto, debiteras du för hello gång den körs. Läs mer om att minskas när hello fjärråtkomst övervakning lösningen körs [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config]. Ta bort hello förkonfigurerade lösningen från ditt Azure-konto när du är klar.

## <a name="next-steps"></a>Nästa steg

Besök hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) fler exempel och dokumentation om Azure IoT.


[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/app-output.png
[img-update-progress]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/updateprogress.png
[img-job-status]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/jobstatus.png
[img-list-devices]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/listdevices.png
[img-method-history]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md