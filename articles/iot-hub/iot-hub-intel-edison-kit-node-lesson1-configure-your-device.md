---
title: 'Ansluta Intel modern (nod) tooAzure IoT - lektionen 1: Konfigurera enhet | Microsoft Docs'
description: "Konfigurera Intel modern för första gången."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "arduino ställer in, ansluta arduino toopc, installationsprogrammet arduino, arduino-kort"
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
ms.openlocfilehash: c0609542a49924c979f71297d808c09acefca0bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-your-intel-edison"></a>Konfigurera Intel-modern
## <a name="what-you-will-do"></a>Vad du ska göra
Konfigurera Intel modern för första gången som samlar in hello ändringar, startar upp och installera configuration tool tooyour skrivbord OS tooflash Moderns inbyggd programvara, ange sitt lösenord och ansluta den tooWi-Fi. Om du har några problem med söka efter lösningar på hello [felsökning sidan][troubleshooting].

## <a name="what-you-will-learn"></a>Vad får du lära dig
I den här artikeln får du lära dig:

* Hur tooassemble modern board och slår upp.
* Hur tooflash modern inbyggd programvara, ange lösenordet och Anslut Wi-Fi.

## <a name="what-you-need"></a>Vad du behöver
toocomplete den här åtgärden måste följande delar från din Intel modern startpaket hello:

* Intel® modern modul
* Arduino expansion-kort
* En GIF-staplar eller skruvar som ingår i hello paketering, inklusive två skruvar toofasten hello modulen toohello expansionskort och fyra uppsättningar av skruvar och form mellanrum.
* Micro B-tooType en USB-kabel
* En strömförsörjning direct aktuella (DC). Din strömförsörjning bör klassificeras enligt följande:
  - 7 15V DC
  - Minst 1500mA
  - hello center/inre PIN-kod måste vara positiva hello Polen av hello-strömförsörjning

  ![Saker i din startpaket](media/iot-hub-intel-edison-lessons/lesson1/kit.png)

Du behöver också:

* En dator som kör Windows, Mac eller Linux.
* En trådlös anslutning för modern tooconnect till.
* En Internet-anslutning toodownload-konfigurationsverktyget.

## <a name="assemble-your-board"></a>Assemblera kortets

Det här avsnittet innehåller steg tooattach din Intel® modern modulen tooyour expansionskort.

1. Placera hello Intel® modern modul i hello vit disposition på expansionskort, Rada upp hello hål på hello modulen med hello skruvar på hello expansion-kort.

2. Tryck ned på hello modulen nedanför hello ord `What will you make?` tills du anser att en snapin.

   ![Assemblera board 2](media/iot-hub-intel-edison-lessons/lesson1/assemble_board2.jpg)

3. Använd hello två hexadecimala nuts (ingår i hello-paket) toosecure hello modulen toohello expansionskort.

   ![Assemblera board 3](media/iot-hub-intel-edison-lessons/lesson1/assemble_board3.jpg)

4. Infoga en skruv i de hello fyra hörnet hål på hello expansion-kort. Genom att vrida och öka en vit hello form mellanrum till hello skruv.

   ![Assemblera board 4](media/iot-hub-intel-edison-lessons/lesson1/assemble_board4.jpg)

5. Upprepa för hello andra tre hörnet mellanrum.

   ![Assemblera board 5](media/iot-hub-intel-edison-lessons/lesson1/assemble_board5.jpg)

Nu är kortets monterad.

   ![monterade board](media/iot-hub-intel-edison-lessons/lesson1/assembled_board.jpg)

## <a name="power-up-edison"></a>Använd modern

1. Anslut hello strömförsörjning.

   ![Plugin-strömförsörjning](media/iot-hub-intel-edison-lessons/lesson1/plug_power.jpg)

2. En grön Indikator (heter DS1 på hello Arduino * expansion board) ska lysa upp och hålla upplysta.

3. Vänta en minut för hello board toofinish startas.

   > [!NOTE]
   > Om du inte har en DC-strömförsörjning kan du fortfarande power hello board via en USB-port. Se `Connect Edison tooyour computer` information. Startar din board på detta sätt kan resultera i oväntade funktionssätt från dina ändringar, särskilt när med hjälp av Wi-Fi eller drivande motorer.

## <a name="connect-edison-tooyour-computer"></a>Ansluta modern tooyour dator

1. Växla ned hello microswitch mot hello två micro USB-portar, så att modern enhet-läge. Skillnader mellan enheten och värden läge referera [här](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).

   ![Växla ned hello microswitch](media/iot-hub-intel-edison-lessons/lesson1/toggle_down_microswitch.jpg)

2. Anslut hello micro USB-kabel till hello översta micro USB-port.

   ![Övre micro USB-port](media/iot-hub-intel-edison-lessons/lesson1/top_usbport.jpg)

3. Plug hello andra änden av USB-kabel till datorn.

   ![Datorn USB](media/iot-hub-intel-edison-lessons/lesson1/computer_usb.jpg)

4. Vet du kortets initieras fullständigt när datorn monterar en ny enhet (ungefär som att lägga till ett SD-kort i datorn).

## <a name="download-and-run-hello-configuration-tool"></a>Hämta och kör konfigurationsverktyget för hello
Hämta hello senaste konfigurationsverktyget från [länken](https://software.intel.com/en-us/iot/hardware/edison/downloads) visas under hello `Installers` rubrik. Kör verktyget hello och följ dess på skärmen instruktioner, klicka på Nästa om det behövs

### <a name="flash-firmware"></a>Flash inbyggd programvara
1. På hello `Set up options` klickar du på `Flash Firmware`.
2. Välj hello avbildningen tooflash till dina ändringar genom att göra något av följande hello:
   - toodownload och flash din board med hello senaste firmware-avbildning från Intel, Välj `Download hello latest image version xxxx`.
   - tooflash dina ändringar med en avbildning som redan har sparats på din dator, Välj `Select hello local image`. Bläddra tooand väljer hello bild tooflash tooyour-kort.
3. hello installationsverktyget försöker tooflash kortets. hello hela blinkande processen kan ta too10 minuter.

### <a name="set-password"></a>Ange ett lösenord
1. På hello `Set up options` klickar du på `Enable Security`.
2. Du kan ange ett eget namn för Intel® modern-kort. Det här är valfritt.
3. Ange ett lösenord för dina ändringar och klicka sedan på `Set password`.
4. Anteckna hello lösenord, som används senare.

### <a name="connect-wi-fi"></a>Ansluta Wi-Fi
1. På hello `Set up options` klickar du på `Connect Wi-Fi`. Vänta upp tooone minut som din dator sökningar efter tillgängliga Wi-Fi-nätverk.
2. Från hello `Detected Networks` listrutan väljer du nätverket.
3. Från hello `Security` listrutan, Välj hello nätverket säkerhetstyp.
4. Ange din information inloggningsnamn och lösenord och klicka sedan på `Configure Wi-Fi`.
5. Anteckna hello IP-adress som används senare.

> [!NOTE]
> Se till att modern är anslutna toohello samma nätverk som datorn. Datorn ansluter tooyour modern med hello IP-adress.

Grattis! Du har konfigurerat modern.

## <a name="summary"></a>Sammanfattning
I den här artikeln har du lärt dig hur tooassemble hello modern board, flash dess inbyggd programvara, installationsprogrammet lösenord och ansluta tooWi-Fi med hjälp av verktyget för serverkonfiguration. Observera att hello Indikator ännu inte lysa upp. hello nästa uppgift är tooinstall hello nödvändiga verktyg och program som förberedelse för att köra ett exempelprogram på modern.

## <a name="next-steps"></a>Nästa steg
[Hämta hello-verktyg][get-the-tools]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[get-the-tools]: iot-hub-intel-edison-kit-node-lesson1-get-the-tools-win32.md