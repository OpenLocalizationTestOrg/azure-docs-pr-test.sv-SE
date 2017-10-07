---
title: aaaIntel modern toocloud (C) - ansluta Intel modern tooAzure IoT-hubb | Microsoft Docs
description: "Lär dig hur toosetup och ansluta Intel modern tooAzure IoT-hubb för Intel modern toosend data toohello Azure-molnplattform i den här självstudiekursen."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Azure iot intel modern, intel modern iot-hubb, intel modern skicka data toocloud, intel modern toocloud
ms.assetid: 4885fa2c-c2ee-4253-b37f-ccd55f92b006
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/17/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d0928e6c7870d724ff2044280937a45a9e032c75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-intel-edison-tooazure-iot-hub-c"></a>Ansluta Intel modern tooAzure IoT Hub (C)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

I den här kursen kan du börja genom att lära dig hello grunderna i att arbeta med Intel modern. Du lär dig hur tooseamlessly ansluta enheter toohello molnet med hjälp av [Azure IoT Hub](iot-hub-what-is-iot-hub.md).

Har du en kit ännu? Starta [här](https://azure.microsoft.com/develop/iot/starter-kits)

## <a name="what-you-do"></a>Vad du gör

* Konfigurera Intel modern och och Groove moduler.
* Skapa en IoT-hubb.
* Registrera en enhet för modern i din IoT-hubb.
* Kör ett exempelprogram på modern toosend sensor data tooyour IoT-hubb.

Ansluta Intel modern tooan IoT-hubb som du skapar. Sedan kör du ett exempelprogram på modern toocollect temperatur- och fuktighetskonsekvens data från en Groove-temperatursensor. Slutligen kan skicka du hello sensor data tooyour IoT-hubb.

## <a name="what-you-learn"></a>Detta får du får lära dig

* Hur toocreate en Azure IoT-hubb och få nya enheten anslutningssträngen.
* Hur tooconnect modern med en Groove-temperatursensor.
* Hur toocollect sensordata genom att köra ett exempelprogram på modern.
* Hur toosend sensor data tooyour IoT-hubb.

## <a name="what-you-need"></a>Vad du behöver

![Vad du behöver](media/iot-hub-intel-edison-kit-c-get-started/0_kit.png)

* hello Intel modern-kort
* Arduino expansion-kort
* En aktiv Azure-prenumeration. Om du inte har ett Azure-konto [skapa ett kostnadsfritt Azure konto](https://azure.microsoft.com/free/) i bara några minuter.
* En Mac- eller en dator som kör Windows eller Linux.
* En Internetanslutning.
* Micro B-tooType en USB-kabel
* En strömförsörjning direct aktuella (DC). Din strömförsörjning bör klassificeras enligt följande:
  - 7 15V DC
  - Minst 1500mA
  - hello center/inre PIN-kod måste vara positiva hello Polen av hello-strömförsörjning

hello följande objekt är valfria:

* Groove grundläggande Shield V2
* Groove - temperatursensor
* Groove-kabel
* En GIF-staplar eller skruvar som ingår i hello paketering, inklusive två skruvar toofasten hello modulen toohello expansionskort och fyra uppsättningar av skruvar och form mellanrum.

> [!NOTE] 
Objekten är valfritt eftersom hello kod exempel support simulerade sensordata.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-intel-edison"></a>Installationsprogrammet Intel modern

### <a name="assemble-your-board"></a>Assemblera kortets

Det här avsnittet innehåller steg tooattach din Intel® modern modulen tooyour expansionskort.

1. Placera hello Intel® modern modul i hello vit disposition på expansionskort, Rada upp hello hål på hello modulen med hello skruvar på hello expansion-kort.

2. Tryck ned på hello modulen nedanför hello ord `What will you make?` tills du anser att en snapin.

   ![Assemblera board 2](media/iot-hub-intel-edison-kit-c-get-started/1_assemble_board2.jpg)

3. Använd hello två hexadecimala nuts (ingår i hello-paket) toosecure hello modulen toohello expansionskort.

   ![Assemblera board 3](media/iot-hub-intel-edison-kit-c-get-started/2_assemble_board3.jpg)

4. Infoga en skruv i de hello fyra hörnet hål på hello expansion-kort. Genom att vrida och öka en vit hello form mellanrum till hello skruv.

   ![Assemblera board 4](media/iot-hub-intel-edison-kit-c-get-started/3_assemble_board4.jpg)

5. Upprepa för hello andra tre hörnet mellanrum.

   ![Assemblera board 5](media/iot-hub-intel-edison-kit-c-get-started/4_assemble_board5.jpg)

Nu är kortets monterad.

   ![monterade board](media/iot-hub-intel-edison-kit-c-get-started/5_assembled_board.jpg)

### <a name="connect-hello-grove-base-shield-and-hello-temperature-sensor"></a>Ansluta hello Groove Base Shield och hello-temperatursensor

1. Placera hello Groove Base Shield på tooyour-kort. Kontrollera att alla stift nära är anslutna till din kort.
   
   ![Groove grundläggande Shield](media/iot-hub-intel-edison-kit-c-get-started/6_grove_base_sheild.jpg)

2. Använd Groove kabel tooconnect Groove-temperatursensor till hello Groove Base Shield **A0** port.

   ![Ansluta tootemperature sensor](media/iot-hub-intel-edison-kit-c-get-started/7_temperature_sensor.jpg)
   
   ![Modern och sensor-anslutning](media/iot-hub-intel-edison-kit-c-get-started/16_edion_sensor.png)

Din sensor är nu klar.

### <a name="power-up-edison"></a>Använd modern

1. Anslut hello strömförsörjning.

   ![Plugin-strömförsörjning](media/iot-hub-intel-edison-kit-c-get-started/8_plug_power.jpg)

2. En grön Indikator (heter DS1 på hello Arduino * expansion board) ska lysa upp och hålla upplysta.

3. Vänta en minut för hello board toofinish startas.

   > [!NOTE]
   > Om du inte har en DC-strömförsörjning kan du fortfarande power hello board via en USB-port. Se `Connect Edison tooyour computer` information. Startar din board på detta sätt kan resultera i oväntade funktionssätt från dina ändringar, särskilt när med hjälp av Wi-Fi eller drivande motorer.

### <a name="connect-edison-tooyour-computer"></a>Ansluta modern tooyour dator

1. Växla ned hello microswitch mot hello två micro USB-portar, så att modern enhet-läge. Skillnader mellan enheten och värden läge referera [här](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).

   ![Växla ned hello microswitch](media/iot-hub-intel-edison-kit-c-get-started/9_toggle_down_microswitch.jpg)

2. Anslut hello micro USB-kabel till hello översta micro USB-port.

   ![Övre micro USB-port](media/iot-hub-intel-edison-kit-c-get-started/10_top_usbport.jpg)

3. Plug hello andra änden av USB-kabel till datorn.

   ![Datorn USB](media/iot-hub-intel-edison-kit-c-get-started/11_computer_usb.jpg)

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

   ![Ansluta tootemperature sensor](media/iot-hub-intel-edison-kit-c-get-started/12_configuration_tool.png)

Grattis! Du har konfigurerat modern.

## <a name="run-a-sample-application-on-intel-edison"></a>Kör ett exempelprogram på Intel modern

### <a name="prepare-hello-azure-iot-device-sdk"></a>Förbereda hello Azure SDK för IoT-enhet

1. Använd någon av följande SSH-klienter från din värd datorn tooconnect tooyour Intel modern hello. hello IP-adressen är från hello konfigurationsverktyget och hello lösenordet är hello något du har angett i verktyget.
    - [PuTTY](http://www.putty.org/) för Windows.
    - hello inbyggda SSH-klienten på Ubuntu eller macOS (kör `ssh root@"hello IP address"`).

2. Klona hello exempel app tooyour klientenheten. 
   
   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-intel-edison-client-app.git
   ```

3. Gå sedan toohello lagringsplatsen mappen toorun hello efter kommandot toobuild Azure IoT-SDK

   ```bash
   cd iot-hub-c-intel-edison-client-app
   sed -i -e 's/\r$//' buildSDK.sh
   chmod 755 buildSDK.sh
   ./buildSDK.sh
   ```

### <a name="configure-hello-sample-application"></a>Konfigurera hello exempelprogrammet

1. Öppna hello config-fil genom att köra följande kommandon hello:

   ```bash
   nano config.h
   ```

   ![Config-fil](media/iot-hub-intel-edison-kit-c-get-started/13_configure_file.png)

   Det finns två makron i den här filen kan du configurate. hello först en är `INTERVAL`, som definierar hello tidsintervallet mellan två meddelanden som skickar toocloud. Hej andra `SIMULATED_DATA`, vilket är ett booleskt värde för om toouse simulerade sensordata eller inte.

   Om du **saknar hello sensor**, Ställ in hello `SIMULATED_DATA` värdet för`1` toomake hello exempelprogrammet skapa och använda simulerade sensordata.

2. Spara och avsluta genom att trycka på CTRL-O > Ange > kontrollen X.

### <a name="build-and-run-hello-sample-application"></a>Skapa och köra hello exempelprogrammet

1. Skapa hello exempelprogrammet genom att köra följande kommando hello:

   ```bash
   cmake . && make
   ```
   ![Skapa utdata](media/iot-hub-intel-edison-kit-c-get-started/14_build_output.png)

1. Kör exempelprogrammet hello genom att köra följande kommando hello:

   ```bash
   sudo ./app '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   Kontrollera att du kopiera / klistra in anslutningssträngen för hello enhet till hello enkla citattecken.

Du bör se hello följande utdata som visar hello sensor data och hello meddelanden som skickas tooyour IoT-hubb.

![Utdata - sensordata som skickas från Intel modern tooyour IoT-hubb](media/iot-hub-intel-edison-kit-c-get-started/15_message_sent.png)

## <a name="next-steps"></a>Nästa steg

Du har kört ett program toocollect sensor exempeldata och skicka den tooyour IoT-hubb.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
