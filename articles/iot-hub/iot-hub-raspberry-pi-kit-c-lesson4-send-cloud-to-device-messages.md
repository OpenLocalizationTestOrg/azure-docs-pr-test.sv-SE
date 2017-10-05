---
title: 'Ansluta Raspberry Pi (C) till Azure IoT - lektionen 4: moln till enhet | Microsoft Docs'
description: "Ett exempelprogram som körs på Pi och övervakar inkommande meddelanden från din IoT-hubb. En ny uppgift gulp skickar meddelanden till Pi från din IoT-hubb blinkar på Indikator."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "moln till enhet, meddelande från molnet"
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
ms.openlocfilehash: 86c7be931319d9995c2a7311267c7e7c03c3c1b8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="run-a-sample-application-to-receive-cloud-to-device-messages"></a>Kör ett exempelprogram som tar emot meddelanden moln till enhet
I den här artikeln kan du distribuera ett exempelprogram på hallon Pi 3. Exempelprogrammet övervakar inkommande meddelanden från din IoT-hubb. Du kan också köra en aktivitet med gulp på datorn för att skicka meddelanden till Pi från din IoT-hubb. När exempelprogrammet som tar emot meddelanden, blinkar på Indikator. Om du har några problem kan hitta lösningar på den [felsökning sidan](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-do"></a>Vad du ska göra
* Ansluta till din IoT-hubb exempelprogrammet.
* Distribuera och köra exempelprogrammet.
* Skicka meddelanden från din IoT-hubb till Pi blinkar på Indikator.

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:
* Så här övervakar du inkommande meddelanden från din IoT-hubb.
* Hur du skickar meddelanden moln till enhet från din IoT-hubb till Pi.

## <a name="what-you-need"></a>Vad du behöver
* Raspberry Pi 3 ställts in för användning. Information om hur du ställer in Pi finns [konfigurera din enhet](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md).
* En IoT-hubb som skapas i din Azure-prenumeration. Information om hur du skapar din IoT-hubb finns [skapa din IoT-hubb och registrera hallon Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md).

## <a name="connect-the-sample-application-to-your-iot-hub"></a>Ansluta exempelprogrammet till din IoT-hubb
1. Kontrollera att du är i mappen lagringsplatsen `iot-hub-c-raspberrypi-getting-started`. Öppna exempelprogrammet i Visual Studio Code genom att köra följande kommandon:

   ```bash
   cd Lesson4
   code .
   ```

   Observera den `app.c` filen i den `app` undermappen. Den `app.c` filen är viktiga källfilen som innehåller koden för övervakning av inkommande meddelanden från IoT-hubben. Den `blinkLED` funktionen blinkar på Indikator.

   ![Lagringsplatsen strukturen i exempelprogrammet](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure_c.png)
2. Initiera konfigurationsfilen genom att köra följande kommandon:

   ```bash
   npm install
   gulp init
   ```

   Om du har slutfört stegen i [skapa ett Azure-funktion appen och storage-konto](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) på den här datorn alla konfigurationer ärvs, så du kan hoppa till steg för att distribuera och köra exempelprogrammet. Om du har slutfört stegen i [skapa ett Azure-funktion appen och storage-konto](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) på en annan dator som du behöver ersätta platshållare i den `config-raspberrypi.json` filen. Den `config-raspberrypi.json` filen finns i undermappen arbetsmappen.

   ![Innehållet i filen config raspberrypi.json](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

* Ersätt **[enhet värdnamn eller IP-adress]** med Pis IP-adressen eller värdnamnet namn som du kan hämta genom att köra den `devdisco list --eth` kommando.
* Ersätt **[anslutningssträngen för IoT-enhet]** med den anslutningssträng för enheten som du får genom att köra den `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` kommando.
* Ersätt **[anslutningssträngen för IoT-hubb]** med IoT-hubb anslutningssträngen som du får genom att köra den `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` kommando.

> [!NOTE]
> Kör **gulp installera verktyg** samt, om du inte gjort det i lektionen 1.

## <a name="deploy-and-run-the-sample-application"></a>Distribuera och köra exempelprogrammet
Distribuera och köra exempelprogrammet på Pi genom att köra följande kommandon:

```
gulp deploy && gulp run
```

Kommandot gulp körs aktiviteten installera verktyg först. Den distribuerar sedan exempelprogrammet till Pi. Slutligen körs programmet på Pi och en separat åtgärd på värddatorn för att skicka 20 blinka meddelanden till Pi från din IoT-hubb.

När exempelprogrammet som körs, startas lyssnar på meddelanden från din IoT-hubb. Under tiden skickar aktiviteten gulp flera ”blinkar” meddelanden från din IoT-hubb till Pi. För varje meddelande blinka som tar emot Pi exempelprogrammet anropar den `blinkLED` funktionen blinkar på Indikator.

Du bör se Indikator blinka varannan sekund som aktiviteten gulp 20 meddelanden skickas från din IoT-hubb till Pi. Den sista som är en ”stoppa” meddelande som förhindrar att program körs.

![Exempelprogrammet med gulp kommandot och blinkar meddelanden](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink_c.png)

## <a name="summary"></a>Sammanfattning
Du har skickat meddelanden från din IoT-hubb till Pi blinkar på Indikator. Nästa uppgift är valfritt: ändra på och av beteendet för Indikatorn.

## <a name="next-steps"></a>Nästa steg
[Ändra på och av beteendet för Indikatorn](iot-hub-raspberry-pi-kit-c-lesson4-change-led-behavior.md)
