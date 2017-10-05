---
title: Intel modern till molnet (C) - ansluta Intel modern till Azure IoT Hub | Microsoft Docs
description: "Lär dig mer om att konfigurera och ansluta Intel modern till Azure IoT-hubb för Intel modern att skicka data till Azure-molnplattform i den här självstudiekursen."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Azure iot intel modern, intel modern iot-hubb, intel modern skicka data till molnet, intel modern till molnet
ms.assetid: 4885fa2c-c2ee-4253-b37f-ccd55f92b006
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/17/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: edbdbe0230f742cd7228f04a4a83c9bd567527e8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="connect-intel-edison-to-azure-iot-hub-c"></a>Ansluta Intel modern till Azure IoT Hub (C)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

I den här kursen kan du börja lära dig grunderna i att arbeta med Intel modern. Du lär dig sedan sömlöst ansluter enheterna till molnet med hjälp av [Azure IoT Hub](iot-hub-what-is-iot-hub.md).

Har du en kit ännu? Starta [här](https://azure.microsoft.com/develop/iot/starter-kits)

## <a name="what-you-do"></a>Vad du gör

* Konfigurera Intel modern och och Groove moduler.
* Skapa en IoT-hubb.
* Registrera en enhet för modern i din IoT-hubb.
* Kör ett exempelprogram på modern sensordata ska skickas till din IoT-hubb.

Intel modern att ansluta till en IoT-hubb som du skapar. Sedan kör du ett exempelprogram på modern samla in data för temperatur- och fuktighetskonsekvens från en Groove-temperatursensor. Slutligen kan du skicka dessa sensordata för din IoT-hubb.

## <a name="what-you-learn"></a>Detta får du får lära dig

* Hur du skapar en Azure IoT-hubb och hämta din nya anslutningssträngen för enheten.
* Så här ansluter du modern med en Groove-temperatursensor.
* Hur du samlar in sensordata genom att köra ett exempelprogram på modern.
* Hur du skickar sensordata till din IoT-hubb.

## <a name="what-you-need"></a>Vad du behöver

![Vad du behöver](media/iot-hub-intel-edison-kit-c-get-started/0_kit.png)

* Intel modern-kort
* Arduino expansion-kort
* En aktiv Azure-prenumeration. Om du inte har ett Azure-konto [skapa ett kostnadsfritt Azure konto](https://azure.microsoft.com/free/) i bara några minuter.
* En Mac- eller en dator som kör Windows eller Linux.
* En Internetanslutning.
* En Micro B till typ A USB-kabel
* En strömförsörjning direct aktuella (DC). Din strömförsörjning bör klassificeras enligt följande:
  - 7 15V DC
  - Minst 1500mA
  - Center/inre PIN-koden måste vara positiva Polen för strömförsörjningen

Följande objekt är valfria:

* Groove grundläggande Shield V2
* Groove - temperatursensor
* Groove-kabel
* En GIF-staplar eller skruvar som ingår i paketering, inklusive två skruvar för att fästa modulen som ska expansionskort och fyra uppsättningar av skruvar och form mellanrum.

> [!NOTE] 
Objekten är valfritt eftersom kod exempel support simulerade sensordata.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-intel-edison"></a>Installationsprogrammet Intel modern

### <a name="assemble-your-board"></a>Assemblera kortets

Det här avsnittet innehåller steg om du vill bifoga Intel® modern-modulen expansionskort.

1. Placera modulen Intel® modern inom vit kontur på expansionskort, Rada upp tomrum i modulen med skruvar på expansionskort.

2. Tryck ned i modulen nedanför orden `What will you make?` tills du anser att en snapin.

   ![Assemblera board 2](media/iot-hub-intel-edison-kit-c-get-started/1_assemble_board2.jpg)

3. Använd två hexadecimala nuts (ingår i paketet) för att skydda modulen expansion planen.

   ![Assemblera board 3](media/iot-hub-intel-edison-kit-c-get-started/2_assemble_board3.jpg)

4. Infoga en skruv i någon av fyra hörnet hål på expansionskort. Genom att vrida och öka en vit form mellanrum till skruven.

   ![Assemblera board 4](media/iot-hub-intel-edison-kit-c-get-started/3_assemble_board4.jpg)

5. Upprepa för de andra tre hörnet mellanrum.

   ![Assemblera board 5](media/iot-hub-intel-edison-kit-c-get-started/4_assemble_board5.jpg)

Nu är kortets monterad.

   ![monterade board](media/iot-hub-intel-edison-kit-c-get-started/5_assembled_board.jpg)

### <a name="connect-the-grove-base-shield-and-the-temperature-sensor"></a>Ansluta Groove Base Shield och -temperatursensor

1. Placera Groove Base Shield in dina ändringar. Kontrollera att alla stift nära är anslutna till din kort.
   
   ![Groove grundläggande Shield](media/iot-hub-intel-edison-kit-c-get-started/6_grove_base_sheild.jpg)

2. Använd Groove kabel för att ansluta Groove-temperatursensor till Groove Base Shield **A0** port.

   ![Anslut till-temperatursensor](media/iot-hub-intel-edison-kit-c-get-started/7_temperature_sensor.jpg)
   
   ![Modern och sensor-anslutning](media/iot-hub-intel-edison-kit-c-get-started/16_edion_sensor.png)

Din sensor är nu klar.

### <a name="power-up-edison"></a>Använd modern

1. Anslut strömförsörjningen.

   ![Plugin-strömförsörjning](media/iot-hub-intel-edison-kit-c-get-started/8_plug_power.jpg)

2. En grön Indikator (heter DS1 på expansion-kort Arduino *) ska lysa upp och hålla upplysta.

3. Vänta en minut för board att avsluta startar.

   > [!NOTE]
   > Om du inte har en DC-strömförsörjning kan du fortfarande power board via en USB-port. Se `Connect Edison to your computer` information. Startar din board på detta sätt kan resultera i oväntade funktionssätt från dina ändringar, särskilt när med hjälp av Wi-Fi eller drivande motorer.

### <a name="connect-edison-to-your-computer"></a>Ansluta modern till din dator

1. Växla ned microswitch mot de två micro USB-portarna så att modern enhet-läge. Skillnader mellan enheten och värden läge referera [här](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).

   ![Växla ned microswitch](media/iot-hub-intel-edison-kit-c-get-started/9_toggle_down_microswitch.jpg)

2. Anslut micro USB-kabel till översta micro USB-port.

   ![Övre micro USB-port](media/iot-hub-intel-edison-kit-c-get-started/10_top_usbport.jpg)

3. Anslut den andra änden av USB-kabel till din dator.

   ![Datorn USB](media/iot-hub-intel-edison-kit-c-get-started/11_computer_usb.jpg)

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

   ![Anslut till-temperatursensor](media/iot-hub-intel-edison-kit-c-get-started/12_configuration_tool.png)

Grattis! Du har konfigurerat modern.

## <a name="run-a-sample-application-on-intel-edison"></a>Kör ett exempelprogram på Intel modern

### <a name="prepare-the-azure-iot-device-sdk"></a>Förbered enheten för Azure IoT SDK

1. Använd en av följande SSH-klienter från värddatorn för att ansluta till Intel-modern. IP-adressen är från konfigurationsverktyget och lösenordet är det du har angett i verktyget.
    - [PuTTY](http://www.putty.org/) för Windows.
    - Den inbyggda SSH-klienten på Ubuntu eller macOS (kör `ssh root@"the IP address"`).

2. Klona exempelappen klient till din enhet. 
   
   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-intel-edison-client-app.git
   ```

3. Gå till mappen lagringsplatsen för att köra följande kommando för att skapa Azure IoT-SDK

   ```bash
   cd iot-hub-c-intel-edison-client-app
   sed -i -e 's/\r$//' buildSDK.sh
   chmod 755 buildSDK.sh
   ./buildSDK.sh
   ```

### <a name="configure-the-sample-application"></a>Konfigurera exempelprogrammet

1. Öppna konfigurationsfilen genom att köra följande kommandon:

   ```bash
   nano config.h
   ```

   ![Config-fil](media/iot-hub-intel-edison-kit-c-get-started/13_configure_file.png)

   Det finns två makron i den här filen kan du configurate. Den första är `INTERVAL`, som definierar tidsintervallet mellan två meddelanden som skickas till molnet. Det andra `SIMULATED_DATA`, vilket är ett booleskt värde för om du vill använda simulerade sensordata eller inte.

   Om du **saknar sensorn**, ange den `SIMULATED_DATA` värde till `1` för att skapa och använda simulerade sensordata exempelprogrammet.

2. Spara och avsluta genom att trycka på CTRL-O > Ange > kontrollen X.

### <a name="build-and-run-the-sample-application"></a>Skapa och köra exempelprogrammet

1. Skapa exempelprogrammet genom att köra följande kommando:

   ```bash
   cmake . && make
   ```
   ![Skapa utdata](media/iot-hub-intel-edison-kit-c-get-started/14_build_output.png)

1. Kör exempelprogrammet genom att köra följande kommando:

   ```bash
   sudo ./app '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   Kontrollera att du kopiera / klistra in anslutningssträngen enheten till de enkla citattecken.

Du bör se följande utdata som visar dessa sensordata och meddelanden som skickas till din IoT-hubb.

![Utdata - sensordata som skickats från Intel modern till din IoT-hubb](media/iot-hub-intel-edison-kit-c-get-started/15_message_sent.png)

## <a name="next-steps"></a>Nästa steg

Du har kört ett exempelprogram för att samla in sensordata och skicka den till din IoT-hubb.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
