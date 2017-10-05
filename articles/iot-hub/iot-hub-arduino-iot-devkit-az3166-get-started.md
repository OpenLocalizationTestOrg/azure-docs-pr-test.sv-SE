---
title: IoT DevKit till molnet - ansluta IoT DevKit AZ3166 till Azure IoT Hub | Microsoft Docs
description: "Lär dig mer om att konfigurera och ansluta IoT DevKit AZ3166 till Azure IoT-hubb för att skicka data till Azure-molnplattform i den här självstudiekursen."
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
ms.openlocfilehash: 122fac584ac5b954ef7b33a3121bee2c502ebc63
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="connect-iot-devkit-az3166-to-azure-iot-hub-in-the-cloud"></a>Ansluta IoT DevKit AZ3166 till Azure IoT-hubb i molnet

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Den [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) kan användas för att utveckla och prototyp Sakernas Internet (IoT) lösningar utnyttja Microsoft Azure-tjänster. Den innehåller en kompatibel skiva Arduino med omfattande kringutrustning och sensorer, ett paket med öppen källkod board och en växande [projekt katalogen](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).

## <a name="what-you-do"></a>Vad du gör
Ansluta [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) till en Azure IoT-hubb som du skapar, samla in data för temperatur- och fuktighetskonsekvens från sensorer och skicka data till IoT-hubb.

Har du en DevKit ännu? Skaffa ett nytt [här](https://aka.ms/iot-devkit-purchase).

## <a name="what-you-learn"></a>Detta får du får lära dig

* Hur du ansluter IoT DevKit till trådlös åtkomstpunkt och förbereda din utvecklingsmiljö.
* Hur du skapar en IoT-hubb och registrera en enhet för MXChip IoT DevKit.
* Hur du samlar in sensordata genom att köra ett exempelprogram på MXChip IoT DevKit.
* Hur du skickar dessa sensordata till din IoT-hubb.

## <a name="what-you-need"></a>Vad du behöver

* En MXChip IoT DevKit kort med micro USB-kabel. [Hämta den nu](https://aka.ms/iot-devkit-purchase)
* En dator som kör Windows 10 eller macOS 10.10 +
* En aktiv Azure-prenumeration
  * Aktivera en [kostnadsfria 30-dagars utvärderingsversion av Microsoft Azure-konto](https://azureinfo.microsoft.com/us-freetrial.html)

## <a name="prepare-your-hardware"></a>Förbered maskinvaran

Koppla samman maskinvaran till din dator.

### <a name="hardware-you-need"></a>Maskinvara som behövs

* DevKit-kort
* Micro USB-kabel

![komma-igång-maskinvara](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

### <a name="connect-devkit-to-your-computer"></a>Anslut DevKit till datorn

1. Anslut USB slut till din dator
2. Anslut Micro USB änden till DevKit
3. Grön Indikator bredvid power bekräftar anslutning

![komma igång-anslutning](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect.jpg)

## <a name="configure-wifi"></a>Konfigurera Wi-Fi

IoT-projekt förlitar sig på Internet-anslutning. Använd följande instruktioner för att konfigurera DevKit att ansluta till Wi-Fi.

### <a name="enter-ap-mode"></a>Ange AP läge

Håll ned knappen B, sedan push och släpper återställningsknappen sedan Släpp B. Din DevKit anger AP läge för att konfigurera WiFi. Skärmen visar tjänsten ange Identifier(SSID) av DevKit som konfiguration portal IP-adressen:

![komma-igång-Wi-Fi-ap](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ap.jpg)

### <a name="connect-to-devkit-ap"></a>Ansluta till DevKit AP

Nu kan du använda en annan WiFi aktiverad enhet (dator eller mobiltelefon) för att ansluta till DevKit SSID (markerat på skärmbilden ovan), lämna lösenordet tomt.

![komma-igång-ssid](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect-ssid.png)

### <a name="configure-wifi-for-devkit"></a>Konfigurera WiFi för DevKit

Öppna IP-adressen visas på skärmen DevKit på din dator eller mobiltelefon webbläsare, Välj Wi-Fi-nätverk som du vill DevKit att ansluta till och sedan ange lösenordet. Klicka på **Anslut** att slutföra:

![komma igång-Wi-Fi-portal](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-portal.png)

När anslutningen lyckas, kommer DevKit startas om efter några sekunder. Om lyckades visas WiFi namn och IP-adress på skärmen:

![komma-igång-Wi-Fi-ip](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ip.jpg)

> [!NOTE] 
IP-adress som visas i bilden kanske inte motsvarar den faktiska IP-Adressen tilldelas och visas på skärmen DevKit. Detta är normalt eftersom WiFi använder DHCP för att dynamiskt tilldela IP-adresser.

När WiFi har konfigurerats, beständiga dina autentiseringsuppgifter på enheten för anslutningen, även om kopplas från. Till exempel om du har konfigurerat DevKit för Wi-Fi hemma och sedan tog DevKit till office, behöver du konfigurera om AP-läge (från steg **ange AP läge**) att ansluta DevKit till kontoret WiFi. 

## <a name="start-using-devkit"></a>Börja använda DevKit

Standard-appar som körs på DevKit kontrollerar den senaste versionen av den inbyggda programvaran och visa vissa diagnos sensordata för dig.

### <a name="upgrade-to-the-latest-firmware"></a>Uppgradera till den senaste inbyggda programvaran

Du uppmanas på skärmen som behövs för både aktuella och den senaste versionen av inbyggd programvara om det finns en uppgradering. Följ [uppgradera inbyggda programvara](https://microsoft.github.io/azure-iot-developer-kit/docs/upgrading/) guide för att uppgradera den.

![komma-igång-inbyggd programvara](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/firmware.jpg)

> [!NOTE] 
Det här är en enstaka ansträngning när du börja utveckla på DevKit och överför din app och du har den senaste inbyggda programvaran medföljer din app.

### <a name="test-various-sensors"></a>Testa olika sensorer

Tryck på knappen B för att testa sensorer, fortsätta trycker ned och släpper knappen B för att gå igenom varje sensor.

![komma-igång-sensorer](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/sensors.jpg)

## <a name="prepare-development-environment"></a>Förbereda utvecklingsmiljö

Nu är det dags att konfigurera utvecklingsmiljön: verktyg och paket som du kan skapa snygg IoT-appar. Du kan välja Windows eller macOS version enligt ditt operativsystem.


### <a name="windows"></a>Windows

Vi rekommenderar att du kan använda installationspaketet för att förbereda utvecklingsmiljö. Om det uppstår problem kan du följa den [manuella steg](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) att få det gjort.

#### <a name="download-latest-package"></a>Hämta senaste paketet

Den `.zip` filen som du hämtar innehåller alla nödvändiga verktyg och paket som krävs för DevKit utveckling.

> [!div class="button"]
[Ladda ned](https://azureboard.azureedge.net/prod/installpackage/devkit_install_1.0.2.zip)


> [!NOTE] 
> Den `.zip` filen innehåller följande verktyg och paket. Skriptet identifierar och hoppa över dem om du redan har några komponenter vara installerade.
> * Node.js och Yarn: Runtime för installationsskriptet och automatiserade åtgärder
> * [Azure CLI 2.0 MSI](https://docs.microsoft.com//cli/azure/install-azure-cli#windows) -kommandoradsverktyget upplevelse för flera plattformar för att hantera Azure-resurser MSI innehåller beroende Python och pip.
> * [Visual Studio Code](https://code.visualstudio.com/): Lightweight redigerare för DevKit-utveckling
> * [Visual Studio Code-tillägget för Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): gör Arduino utveckling i VS-kod
> * [Arduino IDE](https://www.arduino.cc/en/Main/Software): tillägget för Arduino är beroende av det här verktyget
> * DevKit Board paket: Verktyget kedjor, bibliotek och projekt för DevKit
> * ST-Link-verktyget: Viktigt verktyg och drivrutiner

#### <a name="run-installation-script"></a>Kör skript för installation

I Utforskaren, leta upp den `.zip` och extrahera det, hitta `install.cmd`, högerklicka och välj **”kör som administratör”** att starta.

![komma-igång-kör-admin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/run-admin.png)

Under installationen visas förloppet för varje verktyget eller paketet.

![komma-igång-installation](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install.png)

#### <a name="confirm-to-install-drivers"></a>Bekräfta för att installera drivrutiner

VS-koden för Arduino tillägg beroende Arduino IDE. Om det här är första gången du installerar Arduino IDE uppmanas du att installera relevanta drivrutiner:

![komma-igång-drivrutin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/driver.png)

Det bör ta ungefär 10 minuter för att slutföra installationen beroende på din Internet-hastighet. När installationen är klar bör du se Visual Studio Code och Arduino IDE genvägar på ditt skrivbord.

> [!NOTE] 
Ibland, när du startar VS-kod uppmanas du med ett fel som inte kan hitta Arduino IDE- eller relaterade board paketet. Om du vill lösa det Stäng VS-kod, ska starta Arduino IDE en gång och VS-kod hitta Arduino IDE sökväg korrekt.


### <a name="macos-preview"></a>macOS (förhandsgranskning)

Följ dessa steg för att förbereda utvecklingsmiljö på macOS.

#### <a name="install-azure-cli-20"></a>Installera Azure CLI 2.0

Följ den [officiella guiden](https://docs.microsoft.com//cli/azure/install-azure-cli) installerar Azure CLI 2.0:

Installera Azure CLI 2.0 med en `curl` kommando:

```bash
curl -L https://aka.ms/InstallAzureCli | bash
```

Och starta om din kommandogränssnittet för att ändringarna ska börja gälla:

```bash
exec -l $SHELL
```

#### <a name="install-arduino-ide"></a>Installera IDE Arduino

Visual Studio Code Arduino tillägget är beroende av Arduino IDE. Hämta och installera den [Arduino IDE för macOS](https://www.arduino.cc/en/Main/Software).

#### <a name="install-visual-studio-code"></a>Installera Visual Studio Code

Hämta och installera [Visual Studio-koden för macOS](https://code.visualstudio.com/). Det här är den primära utvecklingsverktyg för att skapa DevKit IoT program.

####  <a name="download-latest-package"></a>Hämta senaste paketet

1. Installera Node.js. Du kan använda populära macOS Pakethanteraren [Homebrew](https://brew.sh/) eller [inbyggd installer](https://nodejs.org/en/download/) att installera den.

2. Hämta `.zip` -fil som innehåller aktiviteten skript som krävs för DevKit utveckling i VS-kod.

   > [!div class="button"]
   [Ladda ned](https://azureboard.azureedge.net/installpackage/devkit_tasks_1.0.2.zip)

   Leta upp den `.zip` och extrahera den. Starta **Terminal** app och kör följande kommandon för att konfigurera:

   Flytta extraherade mappen i mappen macOS användare:
   ```bash
   mv [.zip extracted folder]/azure-board-cli ~/. ; cd ~/azure-board-cli
   ```
  
   Installera `npm` paket:
   ```
   npm install
   ```

#### <a name="install-vs-code-extension-for-arduino"></a>Installera tillägg för VS-kod för Arduino

Visual Studio Code kan du installera Marketplace tillägg direkt i verktyget, klickar du på ikonen tillägg i den vänstra meny rutan och sök sedan `Arduino` att installera:

![Installera tillägg](media/iot-hub-arduino-devkit-az3166-get-started/installation-extensions-mac.png)

#### <a name="install-devkit-board-package"></a>Installera DevKit board paketet

Du behöver lägga till alla DevKit med hjälp av hanteraren för ändringar i Visual Studio-koden.

1. Använd `Cmd+Shift+P` att anropa kommandot paletten och skriv **Arduino** sedan söka efter och välj **Arduino: Board Manager**.

2. Klicka på **'Ytterligare URL: er'** längst ned rätt.
   ![installation av ytterligare webbadresser](media/iot-hub-arduino-devkit-az3166-get-started/installation-additional-urls-mac.png)

3. I den `settings.json` filen, lägga till en rad längst ned i `USER SETTINGS` fönstret och spara.
   ```json
   "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
   ```
   ![json-installation-inställningar](media/iot-hub-arduino-devkit-az3166-get-started/installation-settings-json-mac.png)

4. Nu i Board Manager söka efter 'az3166' och installera den senaste versionen.
   ![installationen az3166](media/iot-hub-arduino-devkit-az3166-get-started/installation-az3166-mac.png)

Nu har du alla nödvändiga verktyg och paket som har installerats för macOS.


## <a name="open-project-folder"></a>Öppna projektmappen

### <a name="launch-vs-code"></a>Starta VS Code

Kontrollera att din DevKit inte är anslutet. Starta VS kod först och ansluta DevKit till din dator. VS kod kommer automatiskt att hitta den och popup-startsidan:

![Mini solution vscode](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-vscode.png)

> [!NOTE] 
Ibland, när du startar VS-kod uppmanas du med fel som inte kan hitta Arduino IDE eller relaterade board paketet. Om du vill lösa det Stäng VS-kod, ska starta Arduino IDE igen och VS-kod hitta Arduino IDE sökväg korrekt.

### <a name="open-arduino-examples-folder"></a>Öppna Arduino exempel mapp

Växla till **'Arduino exempel'** fliken, gå till `Examples for MXCHIP AZ3166 > AzureIoT` och klicka på `GetStarted`.

![Mini-solution-exempel](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-examples.png)

Om du råkar stänga fönstret kan använda för att läsa in den, `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) att anropa kommandot paletten och skriv **Arduino** att söka efter och välj **Arduino: exempel**.

## <a name="provision-azure-services"></a>Etablera Azure-tjänster

I fönstret lösning kör uppgiften `Ctrl+P` (macOS: `Cmd+P`) genom att skriva 'uppgift molnet etablera':

I VS koden terminal hjälper en interaktiv kommandorad dig att etablera nödvändiga Azure-tjänster:

![Mini-solution-cloud-provision](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/cloud-provision.png)

## <a name="build-and-upload-arduino-sketch"></a>Skapa och ladda upp Arduino skiss

### <a name="install-required-library"></a>Installera nödvändiga bibliotek

1. Tryck på `F1` eller `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) att anropa kommandot paletten och skriv **Arduino** sedan söka efter och välj **Arduino: Library Manager**.

2. Sök efter `ArduinoJson` biblioteket och klicka på **installera**

### <a name="build-and-upload-the-device-code"></a>Skapa och skicka kod för enheten

Använd `Ctrl+P` (macOS: `Cmd+P`) att köra 'uppgift enhet-överföringen'. Terminalen uppmanas du att ange konfigurationsläge. Om du vill göra det, hålla ned A, och sedan push och släpp återställningsknappen. Skärmen visar ”Configuration”. Detta är att ange anslutningssträngen som hämtas från 'aktivitet moln-provision-steget.

Sedan startas då verifierar och laddar upp Arduino ritningarna:

![Mini-solution-enhet-överföringen](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/device-upload.png)

DevKit startar om och börja köra koden.

## <a name="test-the-project"></a>Testa projektet

Klicka på ikonen power plugin i statusfältet för att öppna Serial övervakaren i VS-kod.

Exempelprogrammet körts när du ser följande resultat:

* Seriell övervakaren visar samma information som innehållet i skärmbilden nedan.
* Indikator på MXChip IoT DevKit blinkar.

![Slutversionen i VS-kod](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/result-serial-output.png)

## <a name="problems-and-feedback"></a>Problem och feedback

Du kan hitta [vanliga frågor och svar](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) om du stöter på problem eller nå oss från kanaler nedan.

## <a name="next-steps"></a>Nästa steg

Du har anslutit en MXChip IoT DevKit till din IoT-hubb, och skickas avbildade sensordata till din IoT-hubb.

Mer information om hur du kan komma igång med IoT Hub och utforska andra IoT-scenarier finns här:

- [Hantera meddelanden mellan enheter och molnet med iothub-explorer](https://docs.microsoft.com/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
- [Spara meddelanden från IoT Hub i datalagring för Azure](https://docs.microsoft.com//azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
- [Använd Power BI för att visualisera sensordata i realtid från Azure IoT-hubb](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
- [Använd Azure Web Apps visualisera sensordata i realtid från Azure IoT-hubb](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
- [Väder prognos använder sensordata från IoT-hubb i Azure Machine Learning](https://docs.microsoft.com/azure/iot-hub/iot-hub-weather-forecast-machine-learning)
- [Enhetshantering med IoT Hub-utforskaren](https://docs.microsoft.com/azure/iot-hub/iot-hub-device-management-iothub-explorer)
- [Fjärrövervakning och aviseringar med Logic Apps](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)