---
title: 'Connect Intel EDISON (C) tooAzure IoT - lektionen 4: ta emot meddelanden | Microsoft Docs'
description: "Ett exempelprogram som körs på modern och övervakar inkommande meddelanden från din IoT-hubb. En ny uppgift gulp skickar meddelanden tooEdison från din IoT-hubb tooblink hello Indikator."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "arduino kontroll ledde från webben, arduino kontroll ledde via webben"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 820d38f3-d3b8-4249-9e2b-f1b9b771e62f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: f0424506ff755e0b9514684787b37584d406d320
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-tooreceive-cloud-to-device-messages"></a>Kör ett exempel programmet tooreceive meddelanden moln till enhet
I den här artikeln kan du distribuera ett exempelprogram på Intel modern. hello exempelprogrammet övervakar inkommande meddelanden från din IoT-hubb. Du också köra en aktivitet med gulp på din dator toosend meddelanden tooEdison från IoT-hubb. När hello exempelprogrammet får hälsningsmeddelande, blinkar hello-Indikator. Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].

## <a name="what-you-will-do"></a>Vad du ska göra
* Ansluta hello exempel programmet tooyour IoT-hubb.
* Distribuera och köra hello exempelprogrammet.
* Skicka meddelanden från din IoT-hubb tooEdison tooblink hello Indikator.

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:
* Hur toomonitor inkommande meddelanden från din IoT-hubb.
* Hur toosend moln till enhet meddelanden från din IoT-hubb tooEdison.

## <a name="what-you-need"></a>Vad du behöver
* Intel modern ställts in för användning. hur tooset in modern, se toolearn [konfigurera din enhet][configure-your-device].
* En IoT-hubb som skapas i din Azure-prenumeration. toolearn hur toocreate din IoT-hubb finns [skapa Azure IoT Hub][create-your-azure-iot-hub].

## <a name="connect-hello-sample-application-tooyour-iot-hub"></a>Ansluta hello exempel programmet tooyour IoT-hubb
1. Kontrollera att du arbetar i hello lagringsplatsen mappen `iot-hub-c-edison-getting-started`. Öppna hello exempelprogrammet i Visual Studio Code genom att köra följande kommandon hello:

   ```bash
   cd Lesson4
   code .
   ```

   hello-filen i hello `app` undermapp är hello källa fil som innehåller hello toomonitor inkommande meddelanden från hello IoT-hubb. Hej `blinkLED` funktionen blinkar hello-Indikator.

   ![Lagringsplatsen strukturen i hello exempelprogram][repo-structure]
2. Initiera hello konfigurationsfilen genom att köra följande kommandon hello:

   ```bash
   npm install
   gulp init
   ```

   Om du har slutfört hello stegen i [skapa ett Azure-funktion appen och storage-konto] [ create-an-azure-function-app-and-storage-account] på den här datorn alla konfigurationer av hello ärvs, så du kan hoppa över hello steg toohello aktiviteten för att distribuera och Kör hello exempelprogrammet. Om du har slutfört hello stegen i [skapa ett Azure-funktion appen och storage-konto] [ create-an-azure-function-app-and-storage-account] på en annan dator måste tooreplace hello platshållare i hello `config-edison.json` fil. Hej `config-edison.json` filen har hello undermapp i arbetsmappen.

   ![Innehållet i hello edison.json config-fil](media/iot-hub-intel-edison-lessons/lesson4/config-edison.png)

   * Ersätt **[enhet värdnamn eller IP-adress]** med hello enhetens IP-adress som du har markerat ned när du har konfigurerat din enhet.
   * Ersätt **[anslutningssträngen för IoT-enhet]** med anslutningssträngen för hello enhet som du får genom att köra hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` kommando.
   * Ersätt **[anslutningssträngen för IoT-hubb]** med hello anslutningssträngen för IoT-hubb som är tillgängliga genom att köra hello `az iot hub show-connection-string --name {my hub name}` kommando.

   > [!NOTE]
   > Kör **gulp installera verktyg** samt, om du inte gjort det i lektionen 1.

## <a name="deploy-and-run-hello-sample-application"></a>Distribuera och köra hello exempelprogrammet
Distribuera och köra hello exempelprogrammet på modern genom att köra följande kommandon hello:

```bash
gulp deploy && gulp run
```

Hej gulp kommandot distribuerar hello exempel programmet tooEdison. Sedan körs programmet hello på modern och en separat åtgärd på din värd datorn toosend 20 blinka meddelanden tooEdison från IoT-hubb.

När hello exempelprogrammet körs, startas lyssnar toomessages från IoT-hubb. Under tiden skickar hello gulp aktivitet flera ”blinkar” meddelanden från din IoT-hubb tooEdison. För varje meddelande blinka som tar emot modern hello exempelprogrammet anropar hello `blinkLED` funktionen tooblink hello Indikator.

Du bör se hello Indikator blinka varannan sekund som hello gulp aktivitet skickar 20 meddelanden från din IoT-hubb tooEdison. hello senast är en ett ”stop” visas som stoppar hello program från att köras.

![Exempelprogrammet med gulp kommandot och blinkar meddelanden][gulp-command-and-blink-messages]

## <a name="summary"></a>Sammanfattning
Du har skickat meddelanden från din IoT-hubb tooEdison tooblink hello Indikator. hello nästa uppgift är valfritt: ändra hello och inaktivera beteendet för hello Indikator.

## <a name="next-steps"></a>Nästa steg
[Ändra hello och inaktivera beteendet för hello Indikator][change-the-on-and-off-behavior-of-the-led]

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[configure-your-device]: iot-hub-intel-edison-kit-c-lesson1-configure-your-device.md
[create-your-azure-iot-hub]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson4/repo_structure_c.png
[create-an-azure-function-app-and-storage-account]: iot-hub-intel-edison-kit-c-lesson3-deploy-resource-manager-template.md
[gulp-command-and-blink-messages]: media/iot-hub-intel-edison-lessons/lesson4/gulp_blink_c.png
[change-the-on-and-off-behavior-of-the-led]: iot-hub-intel-edison-kit-c-lesson4-change-led-behavior.md