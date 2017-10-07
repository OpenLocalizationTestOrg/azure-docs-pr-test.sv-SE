---
title: aaaIoT DevKit toocloud - ansluta IoT DevKit AZ3166 tooAzure IoT-hubb | Microsoft Docs
description: "Lär dig hur toosetup och ansluta IoT DevKit AZ3166 tooAzure IoT-hubb för den toosend dataplattform toohello Azure-molnet i den här självstudiekursen."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: xshi
ms.openlocfilehash: fd36abaed1fd9f73028b84312bca9e900fb9c004
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-iot-devkit-az3166-tooazure-iot-hub-in-hello-cloud"></a>Ansluta IoT DevKit AZ3166 tooAzure IoT-hubb i hello moln

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Hej [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) kan vara används toodevelop och prototyp Sakernas Internet (IoT) lösningar utnyttja Microsoft Azure-tjänster. Den innehåller en kompatibel skiva Arduino med omfattande kringutrustning och sensorer, ett paket med öppen källkod board och en växande [projekt katalogen](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).

## <a name="what-you-do"></a>Vad du gör
Ansluta [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) tooan Azure IoT-hubb som du skapar, samla in hello temperatur- och fuktighetskonsekvens data från sensorer och skicka hello data tooIoT hubb.

Har du en DevKit ännu? Skaffa ett nytt [här](https://aka.ms/iot-devkit-purchase).

## <a name="what-you-learn"></a>Detta får du får lära dig

* Hur tooconnect IoT DevKit tooWireless åtkomst peka och förbereda din utvecklingsmiljö.
* Hur toocreate en IoT-hubb och registrera en enhet för MXChip IoT DevKit.
* Hur toocollect sensordata genom att köra ett exempelprogram på MXChip IoT DevKit.
* Hur hello toosend sensor data tooyour IoT-hubb.

## <a name="what-you-need"></a>Vad du behöver

* En MXChip IoT DevKit kort med micro USB-kabel. [Hämta den nu](https://aka.ms/iot-devkit-purchase)
* En dator som kör Windows 10 eller macOS 10.10 +
* En aktiv Azure-prenumeration
  * Aktivera en [kostnadsfria 30-dagars utvärderingsversion av Microsoft Azure-konto](https://azureinfo.microsoft.com/us-freetrial.html)

## <a name="prepare-your-hardware"></a>Förbered maskinvaran

Koppla samman hello maskinvara tooyour dator.

### <a name="hardware-you-need"></a>Maskinvara som behövs

* DevKit-kort
* Micro USB-kabel

![komma-igång-maskinvara](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

### <a name="connect-devkit-tooyour-computer"></a>Ansluta DevKit tooyour dator

1. Ansluta USB-end tooyour PC
2. Ansluta Micro USB slutet toohello DevKit
3. hello grön Indikator nästa toopower bekräftar anslutning

![komma igång-anslutning](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect.jpg)

## <a name="configure-wifi"></a>Konfigurera Wi-Fi

IoT-projekt förlitar sig på Internet-anslutning. Använd följande instruktioner tooconfigure hello DevKit tooconnect tooWiFi hello.

### <a name="enter-ap-mode"></a>Ange AP läge

Håll ned knappen B, och sedan push och släpp hello Återställ-knappen och sedan versionen knappen B. Din DevKit anger AP läge för att konfigurera WiFi. hello-skärmen visar hello Service ange Identifier(SSID) av hello DevKit samt hello configuration portal IP-adress:

![komma-igång-Wi-Fi-ap](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ap.jpg)

### <a name="connect-toodevkit-ap"></a>Ansluta tooDevKit AP

Nu använda en annan Wi-Fi aktiverad enhet (dator eller mobiltelefon) tooconnect toohello DevKit SSID (markerat med hello skärmbilden ovan), lämna hello lösenordet tomt.

![komma-igång-ssid](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect-ssid.png)

### <a name="configure-wifi-for-devkit"></a>Konfigurera WiFi för DevKit

Öppna hello IP-adress på hello DevKit skärm på din dator eller mobiltelefon webbläsare, Välj hello WiFi-nätverk som du vill använda hello DevKit tooconnect till, skriv sedan hello lösenord. Klicka på **Anslut** toocomplete:

![komma igång-Wi-Fi-portal](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-portal.png)

När anslutningen hello hello DevKit kommer att startas om efter några sekunder. Om lyckades visas hello WiFi namn och IP-adress på hello-skärmen:

![komma-igång-Wi-Fi-ip](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ip.jpg)

> [!NOTE] 
hello IP-adress som visas i hello foto kanske inte motsvarar faktiska IP-hello tilldelade och visas på hello DevKit skärm. Detta är normalt som WiFi använder DHCP toodynamically tilldela IP-adresser.

När WiFi har konfigurerats, ska dina autentiseringsuppgifter behållas på hello enhet för anslutningen, även om kopplas från. Till exempel om du har konfigurerat hello DevKit för Wi-Fi hemma och tog hello DevKit toohello office måste tooreconfigure AP-läge (från steg **ange AP läge**) tooconnect hello DevKit tooyour office WiFi. 

## <a name="start-using-devkit"></a>Börja använda DevKit

hello standard app som körs på DevKit kontrollerar hello senaste versionen av den inbyggda programvaran hello och visas vissa diagnos sensordata för dig.

### <a name="upgrade-toohello-latest-firmware"></a>Uppgradera toohello senaste inbyggda programvaran

Du uppmanas att hello-skärmen både hello version på inbyggd programvara aktuella och senaste om det finns en uppgradering som behövs. Följ [uppgradera inbyggda programvara](https://microsoft.github.io/azure-iot-developer-kit/docs/upgrading/) guide tooupgrade den.

![komma-igång-inbyggd programvara](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/firmware.jpg)

> [!NOTE] 
Det här är en enstaka ansträngning när du börja utveckla på hello DevKit och överför din app måste du hello senaste inbyggda programvaran medföljer din app.

### <a name="test-various-sensors"></a>Testa olika sensorer

Tryck på knappen B tootest sensorer kan fortsätta att trycka på och lanserar hello B knappen toocycle via varje sensor.

![komma-igång-sensorer](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/sensors.jpg)

## <a name="prepare-development-environment"></a>Förbereda utvecklingsmiljö

Nu är det tid tooset hello utvecklingsmiljön: verktyg och paket för toobuild otrolig IoT-appar. Du kan välja Windows eller macOS version enligt tooyour operativsystem.


### <a name="windows"></a>Windows

Vi rekommenderar att du toouse hello installationen paketet tooprepare hello utvecklingsmiljö. Om det uppstår problem du kan följa hello [manuella steg](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) tooget det gjort.

#### <a name="download-latest-package"></a>Hämta senaste paketet

Hej `.zip` du ladda ned filen innehåller alla nödvändiga verktyg och paket som krävs för DevKit utveckling.

> [!div class="button"]
[Ladda ned](https://azureboard.azureedge.net/prod/installpackage/devkit_install_1.0.2.zip)


> [!NOTE] 
> Hej `.zip` filen innehåller hello följande verktyg och paket. Om du redan har vissa komponenter installerade hello skriptet identifierar och hoppa över dem.
> * Node.js och Yarn: Runtime för hello installationsskriptet och automatiserade åtgärder
> * [Azure CLI 2.0 MSI](https://docs.microsoft.com//cli/azure/install-azure-cli#windows) -plattformar kommandoradsverktyget upplevelse för att hantera Azure-resurser, hello MSI innehåller beroende Python och pip.
> * [Visual Studio Code](https://code.visualstudio.com/): Lightweight redigerare för DevKit-utveckling
> * [Visual Studio Code-tillägget för Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): gör Arduino utveckling i VS-kod
> * [Arduino IDE](https://www.arduino.cc/en/Main/Software): hello-tillägget för Arduino är beroende av det här verktyget
> * DevKit Board paket: Verktyget kedjor, bibliotek och projekt för hello DevKit
> * ST-Link-verktyget: Viktigt verktyg och drivrutiner

#### <a name="run-installation-script"></a>Kör skript för installation

I Utforskaren, leta upp hello `.zip` och extrahera det, hitta `install.cmd`, högerklicka och välj **”kör som administratör”** toostart.

![komma-igång-kör-admin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/run-admin.png)

Under installationen visas hello förloppet för varje verktyget eller paketet.

![komma-igång-installation](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install.png)

#### <a name="confirm-tooinstall-drivers"></a>Bekräfta tooinstall drivrutiner

hello VS-kod för Arduino tillägg beroende hello Arduino IDE. Om detta är hello första gången du installerar hello Arduino IDE, kommer du att ange tooinstall relevanta drivrutiner:

![komma-igång-drivrutin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/driver.png)

Det bör ta ungefär 10 minuter toofinish installation beroende på din Internet-hastighet. När hello installationen är klar, bör du se Visual Studio Code och Arduino IDE genvägar på ditt skrivbord.

> [!NOTE] 
Ibland, när du startar VS-kod uppmanas du med ett fel som inte kan hitta Arduino IDE- eller relaterade board paketet. toosolve den, Stäng VS-kod, starta Arduino IDE en gång och VS-kod ska hitta Arduino IDE sökväg korrekt.


### <a name="macos-preview"></a>macOS (förhandsgranskning)

Följ dessa steg tooprepare utvecklingsmiljö på macOS.

#### <a name="install-azure-cli-20"></a>Installera Azure CLI 2.0

Följ hello [officiella guiden](https://docs.microsoft.com//cli/azure/install-azure-cli) tooinstall Azure CLI 2.0:

Installera Azure CLI 2.0 med en `curl` kommando:

```bash
curl -L https://aka.ms/InstallAzureCli | bash
```

Och starta om din kommandogränssnittet för ändringar tootake gälla:

```bash
exec -l $SHELL
```

#### <a name="install-arduino-ide"></a>Installera IDE Arduino

hello Visual Studio Code Arduino tillägg beroende hello Arduino IDE. Hämta och installera hello [Arduino IDE för macOS](https://www.arduino.cc/en/Main/Software).

#### <a name="install-visual-studio-code"></a>Installera Visual Studio Code

Hämta och installera [Visual Studio-koden för macOS](https://code.visualstudio.com/). Detta kan vara hello primära utvecklingsverktyg för att skapa DevKit IoT program.

####  <a name="download-latest-package"></a>Hämta senaste paketet

1. Installera Node.js. Du kan använda populära macOS Pakethanteraren [Homebrew](https://brew.sh/) eller [inbyggd installer](https://nodejs.org/en/download/) tooinstall den.

2. Hämta `.zip` -fil som innehåller aktiviteten skript som krävs för DevKit utveckling i VS-kod.

   > [!div class="button"]
   [Ladda ned](https://azureboard.azureedge.net/installpackage/devkit_tasks_1.0.2.zip)

   Leta upp hello `.zip` och extrahera den. Starta **Terminal** appen och kör hello följande kommandon tooconfigure:

   Flytta den extraherade mappen tooyour macOS användarmappen:
   ```bash
   mv [.zip extracted folder]/azure-board-cli ~/. ; cd ~/azure-board-cli
   ```
  
   Installera `npm` paket:
   ```
   npm install
   ```

#### <a name="install-vs-code-extension-for-arduino"></a>Installera tillägg för VS-kod för Arduino

Visual Studio Code kan du tooinstall Marketplace tillägg direkt i hello verktyget, klicka bara på hello tillägg ikonen hello vänstra menyn i fönstret och sedan söka `Arduino` tooinstall:

![Installera tillägg](media/iot-hub-arduino-devkit-az3166-get-started/installation-extensions-mac.png)

#### <a name="install-devkit-board-package"></a>Installera DevKit board paketet

Du måste använda hello Board Manager i Visual Studio Code tooadd hello DevKit kort.

1. Använd `Cmd+Shift+P` tooinvoke kommandot paletten och typen **Arduino** sedan söka efter och välj **Arduino: Board Manager**.

2. Klicka på **'Ytterligare URL: er'** på hello nedre högra hörnet.
   ![installation av ytterligare webbadresser](media/iot-hub-arduino-devkit-az3166-get-started/installation-additional-urls-mac.png)

3. I hello `settings.json` filen, lägga till en rad längst ned hello `USER SETTINGS` fönstret och spara.
   ```json
   "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
   ```
   ![json-installation-inställningar](media/iot-hub-arduino-devkit-az3166-get-started/installation-settings-json-mac.png)

4. Nu i hello Board Manager söker du efter 'az3166' och installera den senaste versionen av hello.
   ![installationen az3166](media/iot-hub-arduino-devkit-az3166-get-started/installation-az3166-mac.png)

Nu har du alla nödvändiga hello-verktyg och paket som installerats för macOS.


## <a name="open-project-folder"></a>Öppna projektmappen

### <a name="launch-vs-code"></a>Starta VS Code

Kontrollera att din DevKit inte är anslutet. Starta VS kod först och ansluta hello DevKit tooyour dator. VS kod kommer automatiskt att hitta den och popup-startsidan:

![Mini solution vscode](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-vscode.png)

> [!NOTE] 
Ibland, när du startar VS-kod uppmanas du med fel som inte kan hitta Arduino IDE eller relaterade board paketet. toosolve den, Stäng VS-kod, starta Arduino IDE igen och VS-kod ska hitta Arduino IDE sökväg korrekt.

### <a name="open-arduino-examples-folder"></a>Öppna Arduino exempel mapp

Växla för**'Arduino exempel'** fliken, navigera för`Examples for MXCHIP AZ3166 > AzureIoT` och klicka på `GetStarted`.

![Mini-solution-exempel](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-examples.png)

Om du råkar tooclose hello fönstret tooreload det, Använd `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) tooinvoke kommandot paletten och typen **Arduino** toofind och välj **Arduino: exempel**.

## <a name="provision-azure-services"></a>Etablera Azure-tjänster

Kör uppgiften i hello lösning fönster `Ctrl+P` (macOS: `Cmd+P`) genom att skriva 'uppgift molnet etablera':

I hello krävs VS kod terminal en interaktiv kommandorad vägleder dig genom etablering hello Azure-tjänster:

![Mini-solution-cloud-provision](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/cloud-provision.png)

## <a name="build-and-upload-arduino-sketch"></a>Skapa och ladda upp Arduino skiss

### <a name="install-required-library"></a>Installera nödvändiga bibliotek

1. Tryck på `F1` eller `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) tooinvoke kommandot paletten och typen **Arduino** sedan söka efter och välj **Arduino: Library Manager**.

2. Sök efter `ArduinoJson` biblioteket och klicka på **installera**

### <a name="build-and-upload-hello-device-code"></a>Skapa och ladda upp hello enheten kod

Använd `Ctrl+P` (macOS: `Cmd+P`) toorun 'aktivitet enhet-överföringen'. hello terminal uppmanas tooenter konfigurationsläge. toodo så håller ned A sedan knappen Återställ för push- och versionen hello. hello-skärmen visar ”Configuration”. Detta är tooset hello anslutningssträngen som hämtas från 'aktivitet moln-provision-steget.

Sedan startas då verifierar och laddar upp hello Arduino skiss:

![Mini-solution-enhet-överföringen](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/device-upload.png)

Hej DevKit startar om och börja köra hello kod.

## <a name="test-hello-project"></a>Testa hello-projekt

Klicka hello power plug-ikonen i hello statusfältet tooopen hello seriella övervakaren i VS-kod.

hello exempelprogrammet körs utan problem när du ser hello följande resultat:

* hello hello seriella övervakaren visar samma information som hello innehåll i hello skärmbilden nedan.
* hello Indikator på MXChip IoT DevKit blinkar.

![Slutversionen i VS-kod](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/result-serial-output.png)

## <a name="problems-and-feedback"></a>Problem och feedback

Du kan hitta [vanliga frågor och svar](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) om du stöter på problem eller nå ut toous från hello kanaler nedan.

## <a name="next-steps"></a>Nästa steg

Du har anslutit en MXChip IoT DevKit tooyour IoT-hubb och skickade hello avbildas sensor data tooyour IoT-hubb.

toocontinue komma igång med IoT-hubb och tooexplore finns i andra IoT-scenarier:

- [Hantera meddelanden mellan enheter och molnet med iothub-explorer](https://docs.microsoft.com/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
- [Spara IoT-hubb meddelanden tooAzure datalagring](https://docs.microsoft.com//azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
- [Använd Power BI toovisualize realtid sensordata från Azure IoT-hubb](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
- [Använd Azure Web Apps toovisualize realtid sensordata från Azure IoT-hubb](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
- [Väder prognos med hello sensordata från IoT-hubb i Azure Machine Learning](https://docs.microsoft.com/azure/iot-hub/iot-hub-weather-forecast-machine-learning)
- [Enhetshantering med IoT Hub-utforskaren](https://docs.microsoft.com/azure/iot-hub/iot-hub-device-management-iothub-explorer)
- [Fjärrövervakning och aviseringar med Logic Apps](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)