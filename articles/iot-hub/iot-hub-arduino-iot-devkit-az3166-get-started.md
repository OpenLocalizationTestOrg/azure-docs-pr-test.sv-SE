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
# <a name="connect-iot-devkit-az3166-tooazure-iot-hub-in-hello-cloud"></a><span data-ttu-id="5d6ab-103">Ansluta IoT DevKit AZ3166 tooAzure IoT-hubb i hello moln</span><span class="sxs-lookup"><span data-stu-id="5d6ab-103">Connect IoT DevKit AZ3166 tooAzure IoT Hub in hello cloud</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="5d6ab-104">Hej [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) kan vara används toodevelop och prototyp Sakernas Internet (IoT) lösningar utnyttja Microsoft Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-104">hello [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) can be used toodevelop and prototype Internet of Things (IoT) solutions leveraging Microsoft Azure services.</span></span> <span data-ttu-id="5d6ab-105">Den innehåller en kompatibel skiva Arduino med omfattande kringutrustning och sensorer, ett paket med öppen källkod board och en växande [projekt katalogen](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).</span><span class="sxs-lookup"><span data-stu-id="5d6ab-105">It includes an Arduino compatible board with rich peripherals and sensors, an open-source board package and a growing [projects catalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="5d6ab-106">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="5d6ab-106">What you do</span></span>
<span data-ttu-id="5d6ab-107">Ansluta [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) tooan Azure IoT-hubb som du skapar, samla in hello temperatur- och fuktighetskonsekvens data från sensorer och skicka hello data tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-107">Connect [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) tooan Azure IoT hub that you create, collect hello temperature and humidity data from sensors and send hello data tooIoT hub.</span></span>

<span data-ttu-id="5d6ab-108">Har du en DevKit ännu?</span><span class="sxs-lookup"><span data-stu-id="5d6ab-108">Don't have a DevKit yet?</span></span> <span data-ttu-id="5d6ab-109">Skaffa ett nytt [här](https://aka.ms/iot-devkit-purchase).</span><span class="sxs-lookup"><span data-stu-id="5d6ab-109">Get a new one [here](https://aka.ms/iot-devkit-purchase).</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="5d6ab-110">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="5d6ab-110">What you learn</span></span>

* <span data-ttu-id="5d6ab-111">Hur tooconnect IoT DevKit tooWireless åtkomst peka och förbereda din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-111">How tooconnect IoT DevKit tooWireless access point and prepare your development environment.</span></span>
* <span data-ttu-id="5d6ab-112">Hur toocreate en IoT-hubb och registrera en enhet för MXChip IoT DevKit.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-112">How toocreate an IoT hub and register a device for MXChip IoT DevKit.</span></span>
* <span data-ttu-id="5d6ab-113">Hur toocollect sensordata genom att köra ett exempelprogram på MXChip IoT DevKit.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-113">How toocollect sensor data by running a sample application on MXChip IoT DevKit.</span></span>
* <span data-ttu-id="5d6ab-114">Hur hello toosend sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-114">How toosend hello sensor data tooyour IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="5d6ab-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="5d6ab-115">What you need</span></span>

* <span data-ttu-id="5d6ab-116">En MXChip IoT DevKit kort med micro USB-kabel.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-116">An MXChip IoT DevKit board with a micro USB cable.</span></span> [<span data-ttu-id="5d6ab-117">Hämta den nu</span><span class="sxs-lookup"><span data-stu-id="5d6ab-117">Get it now</span></span>](https://aka.ms/iot-devkit-purchase)
* <span data-ttu-id="5d6ab-118">En dator som kör Windows 10 eller macOS 10.10 +</span><span class="sxs-lookup"><span data-stu-id="5d6ab-118">A computer running Windows 10 or macOS 10.10+</span></span>
* <span data-ttu-id="5d6ab-119">En aktiv Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="5d6ab-119">An active Azure subscription</span></span>
  * <span data-ttu-id="5d6ab-120">Aktivera en [kostnadsfria 30-dagars utvärderingsversion av Microsoft Azure-konto](https://azureinfo.microsoft.com/us-freetrial.html)</span><span class="sxs-lookup"><span data-stu-id="5d6ab-120">Activate a [free 30-day trial Microsoft Azure account](https://azureinfo.microsoft.com/us-freetrial.html)</span></span>

## <a name="prepare-your-hardware"></a><span data-ttu-id="5d6ab-121">Förbered maskinvaran</span><span class="sxs-lookup"><span data-stu-id="5d6ab-121">Prepare your hardware</span></span>

<span data-ttu-id="5d6ab-122">Koppla samman hello maskinvara tooyour dator.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-122">Hook up hello hardware tooyour computer.</span></span>

### <a name="hardware-you-need"></a><span data-ttu-id="5d6ab-123">Maskinvara som behövs</span><span class="sxs-lookup"><span data-stu-id="5d6ab-123">Hardware you need</span></span>

* <span data-ttu-id="5d6ab-124">DevKit-kort</span><span class="sxs-lookup"><span data-stu-id="5d6ab-124">DevKit board</span></span>
* <span data-ttu-id="5d6ab-125">Micro USB-kabel</span><span class="sxs-lookup"><span data-stu-id="5d6ab-125">Micro USB cable</span></span>

![komma-igång-maskinvara](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

### <a name="connect-devkit-tooyour-computer"></a><span data-ttu-id="5d6ab-127">Ansluta DevKit tooyour dator</span><span class="sxs-lookup"><span data-stu-id="5d6ab-127">Connect DevKit tooyour computer</span></span>

1. <span data-ttu-id="5d6ab-128">Ansluta USB-end tooyour PC</span><span class="sxs-lookup"><span data-stu-id="5d6ab-128">Connect USB end tooyour PC</span></span>
2. <span data-ttu-id="5d6ab-129">Ansluta Micro USB slutet toohello DevKit</span><span class="sxs-lookup"><span data-stu-id="5d6ab-129">Connect Micro USB end toohello DevKit</span></span>
3. <span data-ttu-id="5d6ab-130">hello grön Indikator nästa toopower bekräftar anslutning</span><span class="sxs-lookup"><span data-stu-id="5d6ab-130">hello green LED next toopower confirms connection</span></span>

![komma igång-anslutning](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect.jpg)

## <a name="configure-wifi"></a><span data-ttu-id="5d6ab-132">Konfigurera Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="5d6ab-132">Configure WiFi</span></span>

<span data-ttu-id="5d6ab-133">IoT-projekt förlitar sig på Internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-133">IoT projects rely on Internet connectivity.</span></span> <span data-ttu-id="5d6ab-134">Använd följande instruktioner tooconfigure hello DevKit tooconnect tooWiFi hello.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-134">Use hello following instructions tooconfigure hello DevKit tooconnect tooWiFi.</span></span>

### <a name="enter-ap-mode"></a><span data-ttu-id="5d6ab-135">Ange AP läge</span><span class="sxs-lookup"><span data-stu-id="5d6ab-135">Enter AP Mode</span></span>

<span data-ttu-id="5d6ab-136">Håll ned knappen B, och sedan push och släpp hello Återställ-knappen och sedan versionen knappen B. Din DevKit anger AP läge för att konfigurera WiFi.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-136">Hold down button B, then push and release hello reset button, then release button B. Your DevKit will enter AP Mode for configuring WiFi.</span></span> <span data-ttu-id="5d6ab-137">hello-skärmen visar hello Service ange Identifier(SSID) av hello DevKit samt hello configuration portal IP-adress:</span><span class="sxs-lookup"><span data-stu-id="5d6ab-137">hello screen will display hello Service Set Identifier(SSID) of hello DevKit as well as hello configuration portal IP address:</span></span>

![komma-igång-Wi-Fi-ap](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ap.jpg)

### <a name="connect-toodevkit-ap"></a><span data-ttu-id="5d6ab-139">Ansluta tooDevKit AP</span><span class="sxs-lookup"><span data-stu-id="5d6ab-139">Connect tooDevKit AP</span></span>

<span data-ttu-id="5d6ab-140">Nu använda en annan Wi-Fi aktiverad enhet (dator eller mobiltelefon) tooconnect toohello DevKit SSID (markerat med hello skärmbilden ovan), lämna hello lösenordet tomt.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-140">Now, use another WiFi enabled device (PC or mobile phone) tooconnect toohello DevKit SSID (highlighted in hello screenshot above), leave hello password empty.</span></span>

![komma-igång-ssid](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect-ssid.png)

### <a name="configure-wifi-for-devkit"></a><span data-ttu-id="5d6ab-142">Konfigurera WiFi för DevKit</span><span class="sxs-lookup"><span data-stu-id="5d6ab-142">Configure WiFi for DevKit</span></span>

<span data-ttu-id="5d6ab-143">Öppna hello IP-adress på hello DevKit skärm på din dator eller mobiltelefon webbläsare, Välj hello WiFi-nätverk som du vill använda hello DevKit tooconnect till, skriv sedan hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-143">Open hello IP address shown on hello DevKit screen on your PC or mobile phone browser, select hello WiFi network you want hello DevKit tooconnect to, then type hello password.</span></span> <span data-ttu-id="5d6ab-144">Klicka på **Anslut** toocomplete:</span><span class="sxs-lookup"><span data-stu-id="5d6ab-144">Click **Connect** toocomplete:</span></span>

![komma igång-Wi-Fi-portal](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-portal.png)

<span data-ttu-id="5d6ab-146">När anslutningen hello hello DevKit kommer att startas om efter några sekunder.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-146">Once hello connection succeeds, hello DevKit will reboot in a few seconds.</span></span> <span data-ttu-id="5d6ab-147">Om lyckades visas hello WiFi namn och IP-adress på hello-skärmen:</span><span class="sxs-lookup"><span data-stu-id="5d6ab-147">If succeeded, you will see hello WiFi name and IP address on hello screen:</span></span>

![komma-igång-Wi-Fi-ip](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ip.jpg)

> [!NOTE] 
<span data-ttu-id="5d6ab-149">hello IP-adress som visas i hello foto kanske inte motsvarar faktiska IP-hello tilldelade och visas på hello DevKit skärm.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-149">hello IP address displayed in hello photo may not match hello actual IP assigned and displayed on hello DevKit screen.</span></span> <span data-ttu-id="5d6ab-150">Detta är normalt som WiFi använder DHCP toodynamically tilldela IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-150">This is normal as WiFi uses DHCP toodynamically assign IPs.</span></span>

<span data-ttu-id="5d6ab-151">När WiFi har konfigurerats, ska dina autentiseringsuppgifter behållas på hello enhet för anslutningen, även om kopplas från.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-151">After WiFi is configured, your credentials will be persisted on hello device for that connection, even if unplugged.</span></span> <span data-ttu-id="5d6ab-152">Till exempel om du har konfigurerat hello DevKit för Wi-Fi hemma och tog hello DevKit toohello office måste tooreconfigure AP-läge (från steg **ange AP läge**) tooconnect hello DevKit tooyour office WiFi.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-152">For example, if you configured hello DevKit for WiFi in your home and then took hello DevKit toohello office, you will need tooreconfigure AP mode (starting at step **Enter AP Mode**) tooconnect hello DevKit tooyour office WiFi.</span></span> 

## <a name="start-using-devkit"></a><span data-ttu-id="5d6ab-153">Börja använda DevKit</span><span class="sxs-lookup"><span data-stu-id="5d6ab-153">Start using DevKit</span></span>

<span data-ttu-id="5d6ab-154">hello standard app som körs på DevKit kontrollerar hello senaste versionen av den inbyggda programvaran hello och visas vissa diagnos sensordata för dig.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-154">hello default app running on DevKit will check hello latest version of hello firmware and display some sensor diagnosis data for you.</span></span>

### <a name="upgrade-toohello-latest-firmware"></a><span data-ttu-id="5d6ab-155">Uppgradera toohello senaste inbyggda programvaran</span><span class="sxs-lookup"><span data-stu-id="5d6ab-155">Upgrade toohello latest firmware</span></span>

<span data-ttu-id="5d6ab-156">Du uppmanas att hello-skärmen både hello version på inbyggd programvara aktuella och senaste om det finns en uppgradering som behövs.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-156">You will be prompted on hello screen both hello current and latest firmware version if there is an upgrade needed.</span></span> <span data-ttu-id="5d6ab-157">Följ [uppgradera inbyggda programvara](https://microsoft.github.io/azure-iot-developer-kit/docs/upgrading/) guide tooupgrade den.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-157">Follow [Upgrade firmware](https://microsoft.github.io/azure-iot-developer-kit/docs/upgrading/) guide tooupgrade it.</span></span>

![komma-igång-inbyggd programvara](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/firmware.jpg)

> [!NOTE] 
<span data-ttu-id="5d6ab-159">Det här är en enstaka ansträngning när du börja utveckla på hello DevKit och överför din app måste du hello senaste inbyggda programvaran medföljer din app.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-159">This is a one-time effort, once you start developing on hello DevKit and upload your app, you will have hello latest firmware come with your app.</span></span>

### <a name="test-various-sensors"></a><span data-ttu-id="5d6ab-160">Testa olika sensorer</span><span class="sxs-lookup"><span data-stu-id="5d6ab-160">Test various sensors</span></span>

<span data-ttu-id="5d6ab-161">Tryck på knappen B tootest sensorer kan fortsätta att trycka på och lanserar hello B knappen toocycle via varje sensor.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-161">Press button B tootest sensors, continue pressing and releasing hello B button toocycle through each sensor.</span></span>

![komma-igång-sensorer](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/sensors.jpg)

## <a name="prepare-development-environment"></a><span data-ttu-id="5d6ab-163">Förbereda utvecklingsmiljö</span><span class="sxs-lookup"><span data-stu-id="5d6ab-163">Prepare development environment</span></span>

<span data-ttu-id="5d6ab-164">Nu är det tid tooset hello utvecklingsmiljön: verktyg och paket för toobuild otrolig IoT-appar.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-164">Now it's time tooset up hello development environment: tools and packages for you toobuild stunning IoT applications.</span></span> <span data-ttu-id="5d6ab-165">Du kan välja Windows eller macOS version enligt tooyour operativsystem.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-165">You can choose Windows or macOS version according tooyour operating system.</span></span>


### <a name="windows"></a><span data-ttu-id="5d6ab-166">Windows</span><span class="sxs-lookup"><span data-stu-id="5d6ab-166">Windows</span></span>

<span data-ttu-id="5d6ab-167">Vi rekommenderar att du toouse hello installationen paketet tooprepare hello utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-167">We encourage you toouse hello installation package tooprepare hello development environment.</span></span> <span data-ttu-id="5d6ab-168">Om det uppstår problem du kan följa hello [manuella steg](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) tooget det gjort.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-168">If you encounter any issues, you can follow hello [manual steps](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) tooget it done.</span></span>

#### <a name="download-latest-package"></a><span data-ttu-id="5d6ab-169">Hämta senaste paketet</span><span class="sxs-lookup"><span data-stu-id="5d6ab-169">Download latest package</span></span>

<span data-ttu-id="5d6ab-170">Hej `.zip` du ladda ned filen innehåller alla nödvändiga verktyg och paket som krävs för DevKit utveckling.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-170">hello `.zip` file you download contains all necessary tools and packages required for DevKit development.</span></span>

> [!div class="button"]
[<span data-ttu-id="5d6ab-171">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="5d6ab-171">Download</span></span>](https://azureboard.azureedge.net/prod/installpackage/devkit_install_1.0.2.zip)


> [!NOTE] 
> <span data-ttu-id="5d6ab-172">Hej `.zip` filen innehåller hello följande verktyg och paket.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-172">hello `.zip` file contains hello following tools and packages.</span></span> <span data-ttu-id="5d6ab-173">Om du redan har vissa komponenter installerade hello skriptet identifierar och hoppa över dem.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-173">If you already have some components installed, hello script will detect and skip them.</span></span>
> * <span data-ttu-id="5d6ab-174">Node.js och Yarn: Runtime för hello installationsskriptet och automatiserade åtgärder</span><span class="sxs-lookup"><span data-stu-id="5d6ab-174">Node.js and Yarn: Runtime for hello setup script and automated tasks</span></span>
> * <span data-ttu-id="5d6ab-175">[Azure CLI 2.0 MSI](https://docs.microsoft.com//cli/azure/install-azure-cli#windows) -plattformar kommandoradsverktyget upplevelse för att hantera Azure-resurser, hello MSI innehåller beroende Python och pip.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-175">[Azure CLI 2.0 MSI](https://docs.microsoft.com//cli/azure/install-azure-cli#windows) - Cross-platform command-line experience for managing Azure resources, hello MSI contains dependent Python and pip.</span></span>
> * <span data-ttu-id="5d6ab-176">[Visual Studio Code](https://code.visualstudio.com/): Lightweight redigerare för DevKit-utveckling</span><span class="sxs-lookup"><span data-stu-id="5d6ab-176">[Visual Studio Code](https://code.visualstudio.com/): Lightweight code editor for DevKit development</span></span>
> * <span data-ttu-id="5d6ab-177">[Visual Studio Code-tillägget för Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): gör Arduino utveckling i VS-kod</span><span class="sxs-lookup"><span data-stu-id="5d6ab-177">[Visual Studio Code extension for Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): Enables Arduino development in VS Code</span></span>
> * <span data-ttu-id="5d6ab-178">[Arduino IDE](https://www.arduino.cc/en/Main/Software): hello-tillägget för Arduino är beroende av det här verktyget</span><span class="sxs-lookup"><span data-stu-id="5d6ab-178">[Arduino IDE](https://www.arduino.cc/en/Main/Software): hello extension for Arduino relies on this tool</span></span>
> * <span data-ttu-id="5d6ab-179">DevKit Board paket: Verktyget kedjor, bibliotek och projekt för hello DevKit</span><span class="sxs-lookup"><span data-stu-id="5d6ab-179">DevKit Board Package: Tool chains, libraries and projects for hello DevKit</span></span>
> * <span data-ttu-id="5d6ab-180">ST-Link-verktyget: Viktigt verktyg och drivrutiner</span><span class="sxs-lookup"><span data-stu-id="5d6ab-180">ST-Link Utility: Essential utilities and drivers</span></span>

#### <a name="run-installation-script"></a><span data-ttu-id="5d6ab-181">Kör skript för installation</span><span class="sxs-lookup"><span data-stu-id="5d6ab-181">Run installation script</span></span>

<span data-ttu-id="5d6ab-182">I Utforskaren, leta upp hello `.zip` och extrahera det, hitta `install.cmd`, högerklicka och välj **”kör som administratör”** toostart.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-182">In Windows File Explorer, locate hello `.zip` and extract it, find `install.cmd`, right-click and select **"Run as administrator"** toostart.</span></span>

![komma-igång-kör-admin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/run-admin.png)

<span data-ttu-id="5d6ab-184">Under installationen visas hello förloppet för varje verktyget eller paketet.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-184">During installation, you will see hello progress of each tool or package.</span></span>

![komma-igång-installation](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install.png)

#### <a name="confirm-tooinstall-drivers"></a><span data-ttu-id="5d6ab-186">Bekräfta tooinstall drivrutiner</span><span class="sxs-lookup"><span data-stu-id="5d6ab-186">Confirm tooinstall drivers</span></span>

<span data-ttu-id="5d6ab-187">hello VS-kod för Arduino tillägg beroende hello Arduino IDE.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-187">hello VS Code for Arduino extension relies on hello Arduino IDE.</span></span> <span data-ttu-id="5d6ab-188">Om detta är hello första gången du installerar hello Arduino IDE, kommer du att ange tooinstall relevanta drivrutiner:</span><span class="sxs-lookup"><span data-stu-id="5d6ab-188">If this is hello first time you are installing hello Arduino IDE, you will be prompted tooinstall relevant drivers:</span></span>

![komma-igång-drivrutin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/driver.png)

<span data-ttu-id="5d6ab-190">Det bör ta ungefär 10 minuter toofinish installation beroende på din Internet-hastighet.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-190">It should take around 10 minutes toofinish installation depending on your Internet speed.</span></span> <span data-ttu-id="5d6ab-191">När hello installationen är klar, bör du se Visual Studio Code och Arduino IDE genvägar på ditt skrivbord.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-191">Once hello installation is complete, you should see Visual Studio Code and Arduino IDE shortcuts on your desktop.</span></span>

> [!NOTE] 
<span data-ttu-id="5d6ab-192">Ibland, när du startar VS-kod uppmanas du med ett fel som inte kan hitta Arduino IDE- eller relaterade board paketet.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-192">Occasionally, when you launch VS Code, you will be prompted with an error that cannot find Arduino IDE or related board package.</span></span> <span data-ttu-id="5d6ab-193">toosolve den, Stäng VS-kod, starta Arduino IDE en gång och VS-kod ska hitta Arduino IDE sökväg korrekt.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-193">toosolve it, close VS Code, launch Arduino IDE once and VS Code should locate Arduino IDE path correctly.</span></span>


### <a name="macos-preview"></a><span data-ttu-id="5d6ab-194">macOS (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="5d6ab-194">macOS (Preview)</span></span>

<span data-ttu-id="5d6ab-195">Följ dessa steg tooprepare utvecklingsmiljö på macOS.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-195">Follow these steps tooprepare development environment on macOS.</span></span>

#### <a name="install-azure-cli-20"></a><span data-ttu-id="5d6ab-196">Installera Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="5d6ab-196">Install Azure CLI 2.0</span></span>

<span data-ttu-id="5d6ab-197">Följ hello [officiella guiden](https://docs.microsoft.com//cli/azure/install-azure-cli) tooinstall Azure CLI 2.0:</span><span class="sxs-lookup"><span data-stu-id="5d6ab-197">Follow hello [official guide](https://docs.microsoft.com//cli/azure/install-azure-cli) tooinstall Azure CLI 2.0:</span></span>

<span data-ttu-id="5d6ab-198">Installera Azure CLI 2.0 med en `curl` kommando:</span><span class="sxs-lookup"><span data-stu-id="5d6ab-198">Install Azure CLI 2.0 with one `curl` command:</span></span>

```bash
curl -L https://aka.ms/InstallAzureCli | bash
```

<span data-ttu-id="5d6ab-199">Och starta om din kommandogränssnittet för ändringar tootake gälla:</span><span class="sxs-lookup"><span data-stu-id="5d6ab-199">And restart your command shell for changes tootake effect:</span></span>

```bash
exec -l $SHELL
```

#### <a name="install-arduino-ide"></a><span data-ttu-id="5d6ab-200">Installera IDE Arduino</span><span class="sxs-lookup"><span data-stu-id="5d6ab-200">Install Arduino IDE</span></span>

<span data-ttu-id="5d6ab-201">hello Visual Studio Code Arduino tillägg beroende hello Arduino IDE.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-201">hello Visual Studio Code Arduino extension relies on hello Arduino IDE.</span></span> <span data-ttu-id="5d6ab-202">Hämta och installera hello [Arduino IDE för macOS](https://www.arduino.cc/en/Main/Software).</span><span class="sxs-lookup"><span data-stu-id="5d6ab-202">Download and install hello [Arduino IDE for macOS](https://www.arduino.cc/en/Main/Software).</span></span>

#### <a name="install-visual-studio-code"></a><span data-ttu-id="5d6ab-203">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5d6ab-203">Install Visual Studio Code</span></span>

<span data-ttu-id="5d6ab-204">Hämta och installera [Visual Studio-koden för macOS](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="5d6ab-204">Download and install [Visual Studio Code for macOS](https://code.visualstudio.com/).</span></span> <span data-ttu-id="5d6ab-205">Detta kan vara hello primära utvecklingsverktyg för att skapa DevKit IoT program.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-205">This will be hello primary development tool for building DevKit IoT applications.</span></span>

####  <a name="download-latest-package"></a><span data-ttu-id="5d6ab-206">Hämta senaste paketet</span><span class="sxs-lookup"><span data-stu-id="5d6ab-206">Download latest package</span></span>

1. <span data-ttu-id="5d6ab-207">Installera Node.js.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-207">Install Node.js.</span></span> <span data-ttu-id="5d6ab-208">Du kan använda populära macOS Pakethanteraren [Homebrew](https://brew.sh/) eller [inbyggd installer](https://nodejs.org/en/download/) tooinstall den.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-208">You can use popular macOS package manager [Homebrew](https://brew.sh/) or [pre-built installer](https://nodejs.org/en/download/) tooinstall it.</span></span>

2. <span data-ttu-id="5d6ab-209">Hämta `.zip` -fil som innehåller aktiviteten skript som krävs för DevKit utveckling i VS-kod.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-209">Download `.zip` file containing task scripts required for DevKit development in VS Code.</span></span>

   > [!div class="button"]
   [<span data-ttu-id="5d6ab-210">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="5d6ab-210">Download</span></span>](https://azureboard.azureedge.net/installpackage/devkit_tasks_1.0.2.zip)

   <span data-ttu-id="5d6ab-211">Leta upp hello `.zip` och extrahera den.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-211">Locate hello `.zip` and extract it.</span></span> <span data-ttu-id="5d6ab-212">Starta **Terminal** appen och kör hello följande kommandon tooconfigure:</span><span class="sxs-lookup"><span data-stu-id="5d6ab-212">Then launch **Terminal** app and run hello following commands tooconfigure:</span></span>

   <span data-ttu-id="5d6ab-213">Flytta den extraherade mappen tooyour macOS användarmappen:</span><span class="sxs-lookup"><span data-stu-id="5d6ab-213">Move extracted folder tooyour macOS user folder:</span></span>
   ```bash
   mv [.zip extracted folder]/azure-board-cli ~/. ; cd ~/azure-board-cli
   ```
  
   <span data-ttu-id="5d6ab-214">Installera `npm` paket:</span><span class="sxs-lookup"><span data-stu-id="5d6ab-214">Install `npm` packages:</span></span>
   ```
   npm install
   ```

#### <a name="install-vs-code-extension-for-arduino"></a><span data-ttu-id="5d6ab-215">Installera tillägg för VS-kod för Arduino</span><span class="sxs-lookup"><span data-stu-id="5d6ab-215">Install VS Code extension for Arduino</span></span>

<span data-ttu-id="5d6ab-216">Visual Studio Code kan du tooinstall Marketplace tillägg direkt i hello verktyget, klicka bara på hello tillägg ikonen hello vänstra menyn i fönstret och sedan söka `Arduino` tooinstall:</span><span class="sxs-lookup"><span data-stu-id="5d6ab-216">Visual Studio Code allows you tooinstall Marketplace extensions directly in hello tool, simply click hello extensions icon in hello left menu pane and then search `Arduino` tooinstall:</span></span>

![Installera tillägg](media/iot-hub-arduino-devkit-az3166-get-started/installation-extensions-mac.png)

#### <a name="install-devkit-board-package"></a><span data-ttu-id="5d6ab-218">Installera DevKit board paketet</span><span class="sxs-lookup"><span data-stu-id="5d6ab-218">Install DevKit board package</span></span>

<span data-ttu-id="5d6ab-219">Du måste använda hello Board Manager i Visual Studio Code tooadd hello DevKit kort.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-219">You will need tooadd hello DevKit board using hello Board Manager in Visual Studio Code.</span></span>

1. <span data-ttu-id="5d6ab-220">Använd `Cmd+Shift+P` tooinvoke kommandot paletten och typen **Arduino** sedan söka efter och välj **Arduino: Board Manager**.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-220">Use `Cmd+Shift+P` tooinvoke command palette and type **Arduino** then find and select **Arduino: Board Manager**.</span></span>

2. <span data-ttu-id="5d6ab-221">Klicka på **'Ytterligare URL: er'** på hello nedre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-221">Click **'Additional URLs'** at hello bottom right.</span></span>
   <span data-ttu-id="5d6ab-222">![installation av ytterligare webbadresser](media/iot-hub-arduino-devkit-az3166-get-started/installation-additional-urls-mac.png)</span><span class="sxs-lookup"><span data-stu-id="5d6ab-222">![installation-additional-urls](media/iot-hub-arduino-devkit-az3166-get-started/installation-additional-urls-mac.png)</span></span>

3. <span data-ttu-id="5d6ab-223">I hello `settings.json` filen, lägga till en rad längst ned hello `USER SETTINGS` fönstret och spara.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-223">In hello `settings.json` file, add a line at hello bottom of `USER SETTINGS` pane and save.</span></span>
   ```json
   "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
   ```
   ![json-installation-inställningar](media/iot-hub-arduino-devkit-az3166-get-started/installation-settings-json-mac.png)

4. <span data-ttu-id="5d6ab-225">Nu i hello Board Manager söker du efter 'az3166' och installera den senaste versionen av hello.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-225">Now in hello Board Manager search for 'az3166' and install hello latest version.</span></span>
   <span data-ttu-id="5d6ab-226">![installationen az3166](media/iot-hub-arduino-devkit-az3166-get-started/installation-az3166-mac.png)</span><span class="sxs-lookup"><span data-stu-id="5d6ab-226">![installation-az3166](media/iot-hub-arduino-devkit-az3166-get-started/installation-az3166-mac.png)</span></span>

<span data-ttu-id="5d6ab-227">Nu har du alla nödvändiga hello-verktyg och paket som installerats för macOS.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-227">You now have all hello necessary tools and packages installed for macOS.</span></span>


## <a name="open-project-folder"></a><span data-ttu-id="5d6ab-228">Öppna projektmappen</span><span class="sxs-lookup"><span data-stu-id="5d6ab-228">Open project folder</span></span>

### <a name="launch-vs-code"></a><span data-ttu-id="5d6ab-229">Starta VS Code</span><span class="sxs-lookup"><span data-stu-id="5d6ab-229">Launch VS Code</span></span>

<span data-ttu-id="5d6ab-230">Kontrollera att din DevKit inte är anslutet.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-230">Make sure your DevKit is not connected.</span></span> <span data-ttu-id="5d6ab-231">Starta VS kod först och ansluta hello DevKit tooyour dator.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-231">Launch VS Code first and connect hello DevKit tooyour computer.</span></span> <span data-ttu-id="5d6ab-232">VS kod kommer automatiskt att hitta den och popup-startsidan:</span><span class="sxs-lookup"><span data-stu-id="5d6ab-232">VS Code will automatically find it and pop up an introduction page:</span></span>

![Mini solution vscode](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-vscode.png)

> [!NOTE] 
<span data-ttu-id="5d6ab-234">Ibland, när du startar VS-kod uppmanas du med fel som inte kan hitta Arduino IDE eller relaterade board paketet.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-234">Occasionally, when you launch VS Code, you will be prompted with error that cannot find Arduino IDE or related board package.</span></span> <span data-ttu-id="5d6ab-235">toosolve den, Stäng VS-kod, starta Arduino IDE igen och VS-kod ska hitta Arduino IDE sökväg korrekt.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-235">toosolve it, close VS Code, launch Arduino IDE once again and VS Code should locate Arduino IDE path correctly.</span></span>

### <a name="open-arduino-examples-folder"></a><span data-ttu-id="5d6ab-236">Öppna Arduino exempel mapp</span><span class="sxs-lookup"><span data-stu-id="5d6ab-236">Open Arduino Examples folder</span></span>

<span data-ttu-id="5d6ab-237">Växla för**'Arduino exempel'** fliken, navigera för`Examples for MXCHIP AZ3166 > AzureIoT` och klicka på `GetStarted`.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-237">Switch too**'Arduino Examples'** tab, navigate too`Examples for MXCHIP AZ3166 > AzureIoT` and click on `GetStarted`.</span></span>

![Mini-solution-exempel](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-examples.png)

<span data-ttu-id="5d6ab-239">Om du råkar tooclose hello fönstret tooreload det, Använd `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) tooinvoke kommandot paletten och typen **Arduino** toofind och välj **Arduino: exempel**.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-239">If you happen tooclose hello pane, tooreload it, use `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) tooinvoke command palette and type **Arduino** toofind and select **Arduino: Examples**.</span></span>

## <a name="provision-azure-services"></a><span data-ttu-id="5d6ab-240">Etablera Azure-tjänster</span><span class="sxs-lookup"><span data-stu-id="5d6ab-240">Provision Azure services</span></span>

<span data-ttu-id="5d6ab-241">Kör uppgiften i hello lösning fönster `Ctrl+P` (macOS: `Cmd+P`) genom att skriva 'uppgift molnet etablera':</span><span class="sxs-lookup"><span data-stu-id="5d6ab-241">In hello solution window, run your task through `Ctrl+P` (macOS: `Cmd+P`) by typing 'task cloud-provision':</span></span>

<span data-ttu-id="5d6ab-242">I hello krävs VS kod terminal en interaktiv kommandorad vägleder dig genom etablering hello Azure-tjänster:</span><span class="sxs-lookup"><span data-stu-id="5d6ab-242">In hello VS Code terminal, an interactive command line will guide you through provisioning hello required Azure services:</span></span>

![Mini-solution-cloud-provision](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/cloud-provision.png)

## <a name="build-and-upload-arduino-sketch"></a><span data-ttu-id="5d6ab-244">Skapa och ladda upp Arduino skiss</span><span class="sxs-lookup"><span data-stu-id="5d6ab-244">Build and upload Arduino sketch</span></span>

### <a name="install-required-library"></a><span data-ttu-id="5d6ab-245">Installera nödvändiga bibliotek</span><span class="sxs-lookup"><span data-stu-id="5d6ab-245">Install required library</span></span>

1. <span data-ttu-id="5d6ab-246">Tryck på `F1` eller `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) tooinvoke kommandot paletten och typen **Arduino** sedan söka efter och välj **Arduino: Library Manager**.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-246">Press `F1` or `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) tooinvoke command palette and type **Arduino** then find and select **Arduino: Library Manager**.</span></span>

2. <span data-ttu-id="5d6ab-247">Sök efter `ArduinoJson` biblioteket och klicka på **installera**</span><span class="sxs-lookup"><span data-stu-id="5d6ab-247">Search for `ArduinoJson` library and click **Install**</span></span>

### <a name="build-and-upload-hello-device-code"></a><span data-ttu-id="5d6ab-248">Skapa och ladda upp hello enheten kod</span><span class="sxs-lookup"><span data-stu-id="5d6ab-248">Build and upload hello device code</span></span>

<span data-ttu-id="5d6ab-249">Använd `Ctrl+P` (macOS: `Cmd+P`) toorun 'aktivitet enhet-överföringen'.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-249">Use `Ctrl+P` (macOS: `Cmd+P`) toorun 'task device-upload'.</span></span> <span data-ttu-id="5d6ab-250">hello terminal uppmanas tooenter konfigurationsläge.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-250">hello terminal will prompt you tooenter configuration mode.</span></span> <span data-ttu-id="5d6ab-251">toodo så håller ned A sedan knappen Återställ för push- och versionen hello.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-251">toodo so, hold down button A, then push and release hello reset button.</span></span> <span data-ttu-id="5d6ab-252">hello-skärmen visar ”Configuration”.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-252">hello screen will display 'Configuration'.</span></span> <span data-ttu-id="5d6ab-253">Detta är tooset hello anslutningssträngen som hämtas från 'aktivitet moln-provision-steget.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-253">This is tooset hello connection string that retrieves from 'task cloud-provision' step.</span></span>

<span data-ttu-id="5d6ab-254">Sedan startas då verifierar och laddar upp hello Arduino skiss:</span><span class="sxs-lookup"><span data-stu-id="5d6ab-254">Then it will start verifying and uploading hello Arduino sketch:</span></span>

![Mini-solution-enhet-överföringen](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/device-upload.png)

<span data-ttu-id="5d6ab-256">Hej DevKit startar om och börja köra hello kod.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-256">hello DevKit will reboot and start running hello code.</span></span>

## <a name="test-hello-project"></a><span data-ttu-id="5d6ab-257">Testa hello-projekt</span><span class="sxs-lookup"><span data-stu-id="5d6ab-257">Test hello project</span></span>

<span data-ttu-id="5d6ab-258">Klicka hello power plug-ikonen i hello statusfältet tooopen hello seriella övervakaren i VS-kod.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-258">In VS Code, click hello power plug icon on hello status bar tooopen hello Serial Monitor.</span></span>

<span data-ttu-id="5d6ab-259">hello exempelprogrammet körs utan problem när du ser hello följande resultat:</span><span class="sxs-lookup"><span data-stu-id="5d6ab-259">hello sample application is running successfully when you see hello following results:</span></span>

* <span data-ttu-id="5d6ab-260">hello hello seriella övervakaren visar samma information som hello innehåll i hello skärmbilden nedan.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-260">hello Serial Monitor displays hello same information as hello content in hello screenshot below.</span></span>
* <span data-ttu-id="5d6ab-261">hello Indikator på MXChip IoT DevKit blinkar.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-261">hello LED on MXChip IoT DevKit is blinking.</span></span>

![Slutversionen i VS-kod](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/result-serial-output.png)

## <a name="problems-and-feedback"></a><span data-ttu-id="5d6ab-263">Problem och feedback</span><span class="sxs-lookup"><span data-stu-id="5d6ab-263">Problems and feedback</span></span>

<span data-ttu-id="5d6ab-264">Du kan hitta [vanliga frågor och svar](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) om du stöter på problem eller nå ut toous från hello kanaler nedan.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-264">You can find [FAQs](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) if you encounter problems or reach out toous from hello channels below.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5d6ab-265">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5d6ab-265">Next steps</span></span>

<span data-ttu-id="5d6ab-266">Du har anslutit en MXChip IoT DevKit tooyour IoT-hubb och skickade hello avbildas sensor data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="5d6ab-266">You have successfully connected an MXChip IoT DevKit tooyour IoT Hub, and sent hello captured sensor data tooyour IoT hub.</span></span>

<span data-ttu-id="5d6ab-267">toocontinue komma igång med IoT-hubb och tooexplore finns i andra IoT-scenarier:</span><span class="sxs-lookup"><span data-stu-id="5d6ab-267">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

- [<span data-ttu-id="5d6ab-268">Hantera meddelanden mellan enheter och molnet med iothub-explorer</span><span class="sxs-lookup"><span data-stu-id="5d6ab-268">Manage cloud device messaging with iothub-explorer</span></span>](https://docs.microsoft.com/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
- [<span data-ttu-id="5d6ab-269">Spara IoT-hubb meddelanden tooAzure datalagring</span><span class="sxs-lookup"><span data-stu-id="5d6ab-269">Save IoT Hub messages tooAzure data storage</span></span>](https://docs.microsoft.com//azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
- [<span data-ttu-id="5d6ab-270">Använd Power BI toovisualize realtid sensordata från Azure IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="5d6ab-270">Use Power BI toovisualize real-time sensor data from Azure IoT Hub</span></span>](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
- [<span data-ttu-id="5d6ab-271">Använd Azure Web Apps toovisualize realtid sensordata från Azure IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="5d6ab-271">Use Azure Web Apps toovisualize real-time sensor data from Azure IoT Hub</span></span>](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
- [<span data-ttu-id="5d6ab-272">Väder prognos med hello sensordata från IoT-hubb i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="5d6ab-272">Weather forecast using hello sensor data from your IoT hub in Azure Machine Learning</span></span>](https://docs.microsoft.com/azure/iot-hub/iot-hub-weather-forecast-machine-learning)
- [<span data-ttu-id="5d6ab-273">Enhetshantering med IoT Hub-utforskaren</span><span class="sxs-lookup"><span data-stu-id="5d6ab-273">Device management with iothub-explorer</span></span>](https://docs.microsoft.com/azure/iot-hub/iot-hub-device-management-iothub-explorer)
- [<span data-ttu-id="5d6ab-274">Fjärrövervakning och aviseringar med Logic Apps</span><span class="sxs-lookup"><span data-stu-id="5d6ab-274">Remote monitoring and notifications with Logic Apps</span></span>](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)