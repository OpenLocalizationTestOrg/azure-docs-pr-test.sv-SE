---
title: 'Connect Raspberry PI (C) tooAzure IoT - lektionen 4: moln till enhet | Microsoft Docs'
description: "Ett exempelprogram som körs på Pi och övervakar inkommande meddelanden från din IoT-hubb. En ny uppgift gulp skickar meddelanden tooPi från din IoT-hubb tooblink hello Indikator."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "molnet toodevice, meddelande från molnet"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: fcbc0dd0-cae3-47b0-8e58-240e4f406f75
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 5596bf3a83c21f2bd54b2f83e2a8fdad7a608b94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-tooreceive-cloud-to-device-messages"></a>Kör ett exempel programmet tooreceive meddelanden moln till enhet
I den här artikeln kan du distribuera ett exempelprogram på hallon Pi 3. hello exempelprogrammet övervakar inkommande meddelanden från din IoT-hubb. Du också köra en aktivitet med gulp på din dator toosend meddelanden tooPi från IoT-hubb. När hello exempelprogrammet får hälsningsmeddelande, blinkar hello-Indikator. Om du har några problem med söka efter lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-do"></a>Vad du ska göra
* Ansluta hello exempel programmet tooyour IoT-hubb.
* Distribuera och köra hello exempelprogrammet.
* Skicka meddelanden från din IoT-hubb tooPi tooblink hello Indikator.

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:
* Hur toomonitor inkommande meddelanden från din IoT-hubb.
* Hur toosend moln till enhet meddelanden från din IoT-hubb tooPi.

## <a name="what-you-need"></a>Vad du behöver
* Raspberry Pi 3 ställts in för användning. hur tooset in Pi, se toolearn [konfigurera din enhet](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md).
* En IoT-hubb som skapas i din Azure-prenumeration. toolearn hur toocreate din IoT-hubb finns [skapa din IoT-hubb och registrera hallon Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md).

## <a name="connect-hello-sample-application-tooyour-iot-hub"></a>Ansluta hello exempel programmet tooyour IoT-hubb
1. Kontrollera att du arbetar i hello lagringsplatsen mappen `iot-hub-c-raspberrypi-getting-started`. Öppna hello exempelprogrammet i Visual Studio Code genom att köra följande kommandon hello:

   ```bash
   cd Lesson4
   code .
   ```

   Meddelande hello `app.c` filen i hello `app` undermappen. Hej `app.c` filen är hello källa som innehåller hello toomonitor inkommande meddelanden från hello IoT-hubb. Hej `blinkLED` funktionen blinkar hello-Indikator.

   ![Lagringsplatsen strukturen i hello exempelprogram](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure_c.png)
2. Initiera hello konfigurationsfilen genom att köra följande kommandon hello:

   ```bash
   npm install
   gulp init
   ```

   Om du har slutfört hello stegen i [skapa ett Azure-funktion appen och storage-konto](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) på den här datorn alla konfigurationer av hello ärvs, så du kan hoppa över toostep toohello uppgiften att distribuera och köra hello exempelprogrammet. Om du har slutfört hello stegen i [skapa ett Azure-funktion appen och storage-konto](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) på en annan dator måste tooreplace hello platshållare i hello `config-raspberrypi.json` fil. Hej `config-raspberrypi.json` filen har hello undermapp i arbetsmappen.

   ![Innehållet i hello raspberrypi.json config-fil](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

* Ersätt **[enhet värdnamn eller IP-adress]** med Pis IP-adressen eller värdnamnet namn som du får genom att köra hello `devdisco list --eth` kommando.
* Ersätt **[anslutningssträngen för IoT-enhet]** med anslutningssträngen för hello enhet som du får genom att köra hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` kommando.
* Ersätt **[anslutningssträngen för IoT-hubb]** med hello anslutningssträngen för IoT-hubb som är tillgängliga genom att köra hello `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` kommando.

> [!NOTE]
> Kör **gulp installera verktyg** samt, om du inte gjort det i lektionen 1.

## <a name="deploy-and-run-hello-sample-application"></a>Distribuera och köra hello exempelprogrammet
Distribuera och köra hello exempelprogrammet på Pi genom att köra följande kommandon hello:

```
gulp deploy && gulp run
```

Hej gulp kommandot körs hello först installera verktyg aktivitet. Den distribuerar sedan hello exempel programmet tooPi. Slutligen körs programmet hello på Pi och en separat åtgärd på din värd datorn toosend 20 blinka meddelanden tooPi från IoT-hubb.

När hello exempelprogrammet körs, startas lyssnar toomessages från IoT-hubb. Under tiden skickar hello gulp aktivitet flera ”blinkar” meddelanden från din IoT-hubb tooPi. För varje meddelande blinka som tar emot Pi hello exempelprogrammet anropar hello `blinkLED` funktionen tooblink hello Indikator.

Du bör se hello Indikator blinka varannan sekund som hello gulp aktivitet skickar 20 meddelanden från din IoT-hubb tooPi. hello senast är en ett ”stop” visas som stoppar hello program från att köras.

![Exempelprogrammet med gulp kommandot och blinkar meddelanden](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink_c.png)

## <a name="summary"></a>Sammanfattning
Du har skickat meddelanden från din IoT-hubb tooPi tooblink hello Indikator. hello nästa uppgift är valfritt: ändra hello och inaktivera beteendet för hello Indikator.

## <a name="next-steps"></a>Nästa steg
[Ändra hello och inaktivera beteendet för hello Indikator](iot-hub-raspberry-pi-kit-c-lesson4-change-led-behavior.md)
