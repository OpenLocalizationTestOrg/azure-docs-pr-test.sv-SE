---
title: 'Ansluta Intel modern (nod) till Azure IoT - lektionen 1: Konfigurera enhet | Microsoft Docs'
description: "Konfigurera Intel modern för första gången."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino ställer in, ansluta arduino till pc, installationsprogrammet arduino, arduino-kort"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 372c9b6d-e701-4ff6-8151-d262aa76aa55
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 87bf3a917af096e43a43a2143afa4bf43a72d7fe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-your-intel-edison"></a>Konfigurera Intel-modern
## <a name="what-you-will-do"></a>Vad du ska göra
Konfigurera Intel modern för första gången som samlar in ändringar, startar upp och installera verktyget för serverkonfiguration för ditt skrivbord OS till flash Moderns inbyggd programvara, ange sitt lösenord och ansluter till Wi-Fi. Om du har några problem kan hitta lösningar på den [felsökning sidan][troubleshooting].

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:

* Så här montering modern board och slår upp.
* Så här flash Moderns inbyggd programvara, ange lösenordet och ansluta Wi-Fi.

## <a name="what-you-need"></a>Vad du behöver
För att slutföra den här åtgärden behöver du följande delar från din Intel modern startpaket:

* Intel® modern modul
* Arduino expansion-kort
* En GIF-staplar eller skruvar som ingår i paketering, inklusive två skruvar för att fästa modulen som ska expansionskort och fyra uppsättningar av skruvar och form mellanrum.
* En Micro B till typ A USB-kabel
* En strömförsörjning direct aktuella (DC). Din strömförsörjning bör klassificeras enligt följande:
  - 7 15V DC
  - Minst 1500mA
  - Center/inre PIN-koden måste vara positiva Polen för strömförsörjningen

  ![Saker i din startpaket](media/iot-hub-intel-edison-lessons/lesson1/kit.png)

Du behöver också:

* En dator som kör Windows, Mac eller Linux.
* En trådlös anslutning för modern att ansluta till.
* En Internet-anslutning att hämta verktyget för serverkonfiguration.

## <a name="assemble-your-board"></a>Assemblera kortets

Det här avsnittet innehåller steg om du vill bifoga Intel® modern-modulen expansionskort.

1. Placera modulen Intel® modern inom vit kontur på expansionskort, Rada upp tomrum i modulen med skruvar på expansionskort.

2. Tryck ned i modulen nedanför orden `What will you make?` tills du anser att en snapin.

   ![Assemblera board 2](media/iot-hub-intel-edison-lessons/lesson1/assemble_board2.jpg)

3. Använd två hexadecimala nuts (ingår i paketet) för att skydda modulen expansion planen.

   ![Assemblera board 3](media/iot-hub-intel-edison-lessons/lesson1/assemble_board3.jpg)

4. Infoga en skruv i någon av fyra hörnet hål på expansionskort. Genom att vrida och öka en vit form mellanrum till skruven.

   ![Assemblera board 4](media/iot-hub-intel-edison-lessons/lesson1/assemble_board4.jpg)

5. Upprepa för de andra tre hörnet mellanrum.

   ![Assemblera board 5](media/iot-hub-intel-edison-lessons/lesson1/assemble_board5.jpg)

Nu är kortets monterad.

   ![monterade board](media/iot-hub-intel-edison-lessons/lesson1/assembled_board.jpg)

## <a name="power-up-edison"></a>Använd modern

1. Anslut strömförsörjningen.

   ![Plugin-strömförsörjning](media/iot-hub-intel-edison-lessons/lesson1/plug_power.jpg)

2. En grön Indikator (heter DS1 på expansion-kort Arduino *) ska lysa upp och hålla upplysta.

3. Vänta en minut för board att avsluta startar.

   > [!NOTE]
   > Om du inte har en DC-strömförsörjning kan du fortfarande power board via en USB-port. Se `Connect Edison to your computer` information. Startar din board på detta sätt kan resultera i oväntade funktionssätt från dina ändringar, särskilt när med hjälp av Wi-Fi eller drivande motorer.

## <a name="connect-edison-to-your-computer"></a>Ansluta modern till din dator

1. Växla ned microswitch mot de två micro USB-portarna så att modern enhet-läge. Skillnader mellan enheten och värden läge referera [här](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).

   ![Växla ned microswitch](media/iot-hub-intel-edison-lessons/lesson1/toggle_down_microswitch.jpg)

2. Anslut micro USB-kabel till översta micro USB-port.

   ![Övre micro USB-port](media/iot-hub-intel-edison-lessons/lesson1/top_usbport.jpg)

3. Anslut den andra änden av USB-kabel till din dator.

   ![Datorn USB](media/iot-hub-intel-edison-lessons/lesson1/computer_usb.jpg)

4. Vet du kortets initieras fullständigt när datorn monterar en ny enhet (ungefär som att lägga till ett SD-kort i datorn).

## <a name="download-and-run-the-configuration-tool"></a>Hämta och kör verktyget configuration
Hämta den senaste konfiguration för från [länken](https://software.intel.com/en-us/iot/hardware/edison/downloads) visas under den `Installers` rubrik. Kör verktyget och följ dess på skärmen instruktioner, klicka på Nästa om det behövs

### <a name="flash-firmware"></a>Flash inbyggd programvara
1. På den `Set up options` klickar du på `Flash Firmware`.
2. Välj avbildningen som flash till kortets genom att göra något av följande:
   - Du kan hämta och flash din board med den senaste firmware-avbildningen från Intel, Välj `Download the latest image version xxxx`.
   - För att flash dina ändringar med en avbildning som redan har sparats på datorn, Välj `Select the local image`. Bläddra till och välj den avbildning du vill flash din planen.
3. Verktyget installationsprogrammet försöker flash kortets. Hela blinkande processen kan ta upp till 10 minuter.

### <a name="set-password"></a>Ange ett lösenord
1. På den `Set up options` klickar du på `Enable Security`.
2. Du kan ange ett eget namn för Intel® modern-kort. Det här är valfritt.
3. Ange ett lösenord för dina ändringar och klicka sedan på `Set password`.
4. Anteckna lösenordet som används senare.

### <a name="connect-wi-fi"></a>Ansluta Wi-Fi
1. På den `Set up options` klickar du på `Connect Wi-Fi`. Vänta i upp till en minut som datorn söker efter tillgängliga Wi-Fi-nätverk.
2. Från den `Detected Networks` listrutan väljer du nätverket.
3. Från den `Security` listrutan väljer du nätverkstyp säkerhet.
4. Ange din information inloggningsnamn och lösenord och klicka sedan på `Configure Wi-Fi`.
5. Anteckna den IP-adressen som används senare.

> [!NOTE]
> Kontrollera att modern är ansluten till samma nätverk som datorn. Datorn ansluter till din modern med hjälp av IP-adress.

Grattis! Du har konfigurerat modern.

## <a name="summary"></a>Sammanfattning
Du har lärt dig hur du montera modern board, flash dess inbyggd programvara, installationsprogrammet lösenord och ansluter till Wi-Fi med hjälp av verktyget för serverkonfiguration i den här artikeln. Observera att Indikatorn ännu inte lysa upp. Nästa uppgift är att installera verktyg och program som förberedelse för att köra ett exempelprogram på modern.

## <a name="next-steps"></a>Nästa steg
[Skaffa dig verktyg][get-the-tools]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[get-the-tools]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md