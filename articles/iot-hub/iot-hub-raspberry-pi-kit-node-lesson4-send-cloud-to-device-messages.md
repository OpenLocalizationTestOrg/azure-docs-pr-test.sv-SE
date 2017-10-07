---
featureFlags: usabilla
title: 'Ansluta hallon Pi (nod) tooAzure IoT - lektionen 4: moln till enhet | Microsoft Docs'
description: "hello exempelprogrammet körs på Pi och övervakar inkommande meddelanden från din IoT-hubb. En ny uppgift gulp skickar meddelanden tooPi från din IoT-hubb tooblink hello Indikator."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "molnet toodevice, meddelande från molnet"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 6ae6539e-1163-4490-8d72-fdf7803e3054
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d69ded4e30c27378481ab2a4fb9c5b73be7bd44e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-hello-sample-application-tooreceive-cloud-to-device-messages"></a>Kör exempel programmet tooreceive moln till enhet hälsningsmeddelande
I den här artikeln kan du distribuera ett exempelprogram på hallon Pi 3. hello exempelprogrammet övervakar inkommande meddelanden från din IoT-hubb. Du också köra en aktivitet med gulp på din dator toosend meddelanden tooPi från IoT-hubb. När hello exempelprogrammet får hälsningsmeddelande, blinkar hello-Indikator. Om du har några problem med sökning lösningar på hello [felsökning sidan](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-do"></a>Vad du ska göra
* Ansluta hello exempel programmet tooyour IoT-hubb.
* Distribuera och köra hello exempelprogrammet.
* Skicka meddelanden från din IoT-hubb tooPi tooblink hello Indikator.

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:
* Hur toomonitor inkommande meddelanden från din IoT-hubb
* Hur toosend moln till enhet meddelanden från din IoT-hubb tooPi.

## <a name="what-you-need"></a>Vad du behöver
* Raspberry Pi 3 ställts in för användning. hur tooset in Pi, se toolearn [konfigurera din enhet](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md).
* En IoT-hubb som skapas i din Azure-prenumeration. toolearn hur toocreate din IoT-hubb finns [skapa din IoT-hubb och registrera hallon Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).

## <a name="connect-hello-sample-application-tooyour-iot-hub"></a>Ansluta hello exempel programmet tooyour IoT-hubb
1. Kontrollera att du arbetar i hello lagringsplatsen mappen `iot-hub-node-raspberrypi-getting-started`. Öppna hello exempelprogrammet i Visual Studio Code genom att köra följande kommandon hello:
   
   ```bash
   cd Lesson4
   code .
   ```
   
   Meddelande hello `app.js` filen i hello `app` undermappen. Hej `app.js` filen är hello källa som innehåller hello toomonitor inkommande meddelanden från hello IoT-hubb. Hej `blinkLED` funktionen blinkar hello-Indikator.
   
   ![Lagringsplatsen strukturen i hello exempelprogram](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure.png)
2. Initiera hello-konfigurationsfilen med hjälp av hello följande kommandon:
   
   ```bash
   npm install
   gulp init
   ```
   
   Om du har slutfört hello stegen i [skapa ett Azure-funktion appen och storage-konto](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) på den här datorn alla konfigurationer av hello ärvs, så du kan hoppa över toohello uppgiften att distribuera och köra hello exempelprogrammet. Om du har slutfört hello stegen i [skapa ett Azure-funktion appen och storage-konto](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) på en annan dator måste tooreplace hello platshållare i hello `config-raspberrypi.json` fil. Hej `config-raspberrypi.json` filen har hello undermapp i arbetsmappen.
   
   ![Innehållet i hello raspberrypi.json config-fil](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

* Ersätt **[enhet värdnamn eller IP-adress]** med hello IP-adress för Pi eller hello värdnamn som du får genom att köra hello `devdisco list --eth` kommando.
* Ersätt **[anslutningssträngen för IoT-enhet]** med anslutningssträngen för hello enhet som du får genom att köra hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` kommando.
* Ersätt **[anslutningssträngen för IoT-hubb]** med hello anslutningssträngen för IoT-hubb som är tillgängliga genom att köra hello `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` kommando.

> [!NOTE]
> Kör **gulp installera verktyg** samt, om du inte gjort det i lektionen 1.

## <a name="deploy-and-run-hello-sample-application"></a>Distribuera och köra hello exempelprogrammet
Distribuera och köra hello exempelprogrammet på Pi genom att köra följande kommando hello:

```bash
gulp deploy && gulp run
```

hello kommandot distribuerar hello exempel programmet tooPi. Sedan körs programmet hello på Pi och en separat åtgärd på din värd datorn toosend 20 blinka meddelanden tooPi från IoT-hubb.

När hello exempelprogrammet körs, startas lyssnar toomessages från IoT-hubb. Under tiden skickar hello gulp aktivitet flera ”blinkar” meddelanden från din IoT-hubb tooPi. För varje meddelande blinka som tar emot Pi hello exempelprogrammet anropar hello `blinkLED` funktionen tooblink hello Indikator.

Du bör se hello Indikator blinka varannan sekund som hello gulp aktivitet skickar 20 meddelanden från din IoT-hubb tooPi. hello senast är en ett ”stop” visas som talar om hello programmet toostop körs.

![Exempelprogrammet med gulp kommandot och blinkar meddelanden](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink.png)

## <a name="summary"></a>Sammanfattning
Du har skickat meddelanden från din IoT-hubb tooPi tooblink hello Indikator. hello nästa uppgift är valfritt: ändra hello och inaktivera beteendet för hello Indikator.

## <a name="next-steps"></a>Nästa steg
[Ändra hello och inaktivera beteendet för hello Indikator](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)

