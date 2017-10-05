---
title: 'Connect Intel EDISON (C) till Azure IoT - lektionen 4: ta emot meddelanden | Microsoft Docs'
description: "Ett exempelprogram som körs på modern och övervakar inkommande meddelanden från din IoT-hubb. En ny uppgift gulp skickar meddelanden till modern från din IoT-hubb blinkar på Indikator."
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
ms.openlocfilehash: b7de7a8b53cdb1d7c2560225fce9166e555e5123
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="run-a-sample-application-to-receive-cloud-to-device-messages"></a>Kör ett exempelprogram som tar emot meddelanden moln till enhet
I den här artikeln kan du distribuera ett exempelprogram på Intel modern. Exempelprogrammet övervakar inkommande meddelanden från din IoT-hubb. Du kan också köra en aktivitet med gulp på datorn för att skicka meddelanden till modern från din IoT-hubb. När exempelprogrammet som tar emot meddelanden, blinkar på Indikator. Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].

## <a name="what-you-will-do"></a>Vad du ska göra
* Ansluta till din IoT-hubb exempelprogrammet.
* Distribuera och köra exempelprogrammet.
* Skicka meddelanden från din IoT-hubb för modern blinkar på Indikator.

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:
* Så här övervakar du inkommande meddelanden från din IoT-hubb.
* Hur du skickar meddelanden moln till enhet från din IoT-hubb till modern.

## <a name="what-you-need"></a>Vad du behöver
* Intel modern ställts in för användning. Information om hur du ställer in modern finns [konfigurera din enhet][configure-your-device].
* En IoT-hubb som skapas i din Azure-prenumeration. Information om hur du skapar din IoT-hubb finns [skapa Azure IoT Hub][create-your-azure-iot-hub].

## <a name="connect-the-sample-application-to-your-iot-hub"></a>Ansluta exempelprogrammet till din IoT-hubb
1. Kontrollera att du är i mappen lagringsplatsen `iot-hub-c-edison-getting-started`. Öppna exempelprogrammet i Visual Studio Code genom att köra följande kommandon:

   ```bash
   cd Lesson4
   code .
   ```

   Filen i den `app` undermapp är viktiga källfilen som innehåller koden för övervakning av inkommande meddelanden från IoT-hubben. Den `blinkLED` funktionen blinkar på Indikator.

   ![Lagringsplatsen strukturen i exempelprogrammet][repo-structure]
2. Initiera konfigurationsfilen genom att köra följande kommandon:

   ```bash
   npm install
   gulp init
   ```

   Om du har slutfört stegen i [skapa ett Azure-funktion appen och storage-konto] [ create-an-azure-function-app-and-storage-account] på den här datorn alla konfigurationer ärvs, så du kan hoppa över steg för att distribuera och köra exempelprogrammet. Om du har slutfört stegen i [skapa ett Azure-funktion appen och storage-konto] [ create-an-azure-function-app-and-storage-account] på en annan dator som du behöver ersätta platshållare i den `config-edison.json` filen. Den `config-edison.json` filen finns i undermappen arbetsmappen.

   ![Innehållet i filen config edison.json](media/iot-hub-intel-edison-lessons/lesson4/config-edison.png)

   * Ersätt **[enhet värdnamn eller IP-adress]** med enhetens IP-adress som du har markerat ned när du har konfigurerat din enhet.
   * Ersätt **[anslutningssträngen för IoT-enhet]** med den anslutningssträng för enheten som du får genom att köra den `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` kommando.
   * Ersätt **[anslutningssträngen för IoT-hubb]** med IoT-hubb anslutningssträngen som du får genom att köra den `az iot hub show-connection-string --name {my hub name}` kommando.

   > [!NOTE]
   > Kör **gulp installera verktyg** samt, om du inte gjort det i lektionen 1.

## <a name="deploy-and-run-the-sample-application"></a>Distribuera och köra exempelprogrammet
Distribuera och köra exempelprogrammet på modern genom att köra följande kommandon:

```bash
gulp deploy && gulp run
```

Kommandot gulp distribuerar exempelprogrammet till modern. Därefter körs programmet för modern och en separat åtgärd på värddatorn för att skicka 20 blinka meddelanden till modern från din IoT-hubb.

När exempelprogrammet som körs, startas lyssnar på meddelanden från din IoT-hubb. Gulp-aktivitet skickar under tiden kan flera ”blinkar” meddelanden från din IoT-hubb till modern. För varje meddelande blinka som tar emot modern exempelprogrammet anropar den `blinkLED` funktionen blinkar på Indikator.

Du bör se Indikator blinka varannan sekund som aktiviteten gulp 20 meddelanden skickas från din IoT-hubb till modern. Den sista som är en ”stoppa” meddelande som förhindrar att program körs.

![Exempelprogrammet med gulp kommandot och blinkar meddelanden][gulp-command-and-blink-messages]

## <a name="summary"></a>Sammanfattning
Du har skickat meddelanden från din IoT-hubb till modern blinkar på Indikator. Nästa uppgift är valfritt: ändra på och av beteendet för Indikatorn.

## <a name="next-steps"></a>Nästa steg
[Ändra på och av beteendet för Indikatorn][change-the-on-and-off-behavior-of-the-led]

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[configure-your-device]: iot-hub-intel-edison-kit-c-lesson1-configure-your-device.md
[create-your-azure-iot-hub]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson4/repo_structure_c.png
[create-an-azure-function-app-and-storage-account]: iot-hub-intel-edison-kit-c-lesson3-deploy-resource-manager-template.md
[gulp-command-and-blink-messages]: media/iot-hub-intel-edison-lessons/lesson4/gulp_blink_c.png
[change-the-on-and-off-behavior-of-the-led]: iot-hub-intel-edison-kit-c-lesson4-change-led-behavior.md