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
# <a name="connect-iot-devkit-az3166-to-azure-iot-hub-in-the-cloud"></a><span data-ttu-id="9072e-103">Ansluta IoT DevKit AZ3166 till Azure IoT-hubb i molnet</span><span class="sxs-lookup"><span data-stu-id="9072e-103">Connect IoT DevKit AZ3166 to Azure IoT Hub in the cloud</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="9072e-104">Den [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) kan användas för att utveckla och prototyp Sakernas Internet (IoT) lösningar utnyttja Microsoft Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="9072e-104">The [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) can be used to develop and prototype Internet of Things (IoT) solutions leveraging Microsoft Azure services.</span></span> <span data-ttu-id="9072e-105">Den innehåller en kompatibel skiva Arduino med omfattande kringutrustning och sensorer, ett paket med öppen källkod board och en växande [projekt katalogen](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).</span><span class="sxs-lookup"><span data-stu-id="9072e-105">It includes an Arduino compatible board with rich peripherals and sensors, an open-source board package and a growing [projects catalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="9072e-106">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="9072e-106">What you do</span></span>
<span data-ttu-id="9072e-107">Ansluta [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) till en Azure IoT-hubb som du skapar, samla in data för temperatur- och fuktighetskonsekvens från sensorer och skicka data till IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="9072e-107">Connect [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) to an Azure IoT hub that you create, collect the temperature and humidity data from sensors and send the data to IoT hub.</span></span>

<span data-ttu-id="9072e-108">Har du en DevKit ännu?</span><span class="sxs-lookup"><span data-stu-id="9072e-108">Don't have a DevKit yet?</span></span> <span data-ttu-id="9072e-109">Skaffa ett nytt [här](https://aka.ms/iot-devkit-purchase).</span><span class="sxs-lookup"><span data-stu-id="9072e-109">Get a new one [here](https://aka.ms/iot-devkit-purchase).</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="9072e-110">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="9072e-110">What you learn</span></span>

* <span data-ttu-id="9072e-111">Hur du ansluter IoT DevKit till trådlös åtkomstpunkt och förbereda din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="9072e-111">How to connect IoT DevKit to Wireless access point and prepare your development environment.</span></span>
* <span data-ttu-id="9072e-112">Hur du skapar en IoT-hubb och registrera en enhet för MXChip IoT DevKit.</span><span class="sxs-lookup"><span data-stu-id="9072e-112">How to create an IoT hub and register a device for MXChip IoT DevKit.</span></span>
* <span data-ttu-id="9072e-113">Hur du samlar in sensordata genom att köra ett exempelprogram på MXChip IoT DevKit.</span><span class="sxs-lookup"><span data-stu-id="9072e-113">How to collect sensor data by running a sample application on MXChip IoT DevKit.</span></span>
* <span data-ttu-id="9072e-114">Hur du skickar dessa sensordata till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="9072e-114">How to send the sensor data to your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="9072e-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="9072e-115">What you need</span></span>

* <span data-ttu-id="9072e-116">En MXChip IoT DevKit kort med micro USB-kabel.</span><span class="sxs-lookup"><span data-stu-id="9072e-116">An MXChip IoT DevKit board with a micro USB cable.</span></span> [<span data-ttu-id="9072e-117">Hämta den nu</span><span class="sxs-lookup"><span data-stu-id="9072e-117">Get it now</span></span>](https://aka.ms/iot-devkit-purchase)
* <span data-ttu-id="9072e-118">En dator som kör Windows 10 eller macOS 10.10 +</span><span class="sxs-lookup"><span data-stu-id="9072e-118">A computer running Windows 10 or macOS 10.10+</span></span>
* <span data-ttu-id="9072e-119">En aktiv Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="9072e-119">An active Azure subscription</span></span>
  * <span data-ttu-id="9072e-120">Aktivera en [kostnadsfria 30-dagars utvärderingsversion av Microsoft Azure-konto](https://azureinfo.microsoft.com/us-freetrial.html)</span><span class="sxs-lookup"><span data-stu-id="9072e-120">Activate a [free 30-day trial Microsoft Azure account](https://azureinfo.microsoft.com/us-freetrial.html)</span></span>

## <a name="prepare-your-hardware"></a><span data-ttu-id="9072e-121">Förbered maskinvaran</span><span class="sxs-lookup"><span data-stu-id="9072e-121">Prepare your hardware</span></span>

<span data-ttu-id="9072e-122">Koppla samman maskinvaran till din dator.</span><span class="sxs-lookup"><span data-stu-id="9072e-122">Hook up the hardware to your computer.</span></span>

### <a name="hardware-you-need"></a><span data-ttu-id="9072e-123">Maskinvara som behövs</span><span class="sxs-lookup"><span data-stu-id="9072e-123">Hardware you need</span></span>

* <span data-ttu-id="9072e-124">DevKit-kort</span><span class="sxs-lookup"><span data-stu-id="9072e-124">DevKit board</span></span>
* <span data-ttu-id="9072e-125">Micro USB-kabel</span><span class="sxs-lookup"><span data-stu-id="9072e-125">Micro USB cable</span></span>

![komma-igång-maskinvara](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

### <a name="connect-devkit-to-your-computer"></a><span data-ttu-id="9072e-127">Anslut DevKit till datorn</span><span class="sxs-lookup"><span data-stu-id="9072e-127">Connect DevKit to your computer</span></span>

1. <span data-ttu-id="9072e-128">Anslut USB slut till din dator</span><span class="sxs-lookup"><span data-stu-id="9072e-128">Connect USB end to your PC</span></span>
2. <span data-ttu-id="9072e-129">Anslut Micro USB änden till DevKit</span><span class="sxs-lookup"><span data-stu-id="9072e-129">Connect Micro USB end to the DevKit</span></span>
3. <span data-ttu-id="9072e-130">Grön Indikator bredvid power bekräftar anslutning</span><span class="sxs-lookup"><span data-stu-id="9072e-130">The green LED next to power confirms connection</span></span>

![komma igång-anslutning](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect.jpg)

## <a name="configure-wifi"></a><span data-ttu-id="9072e-132">Konfigurera Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="9072e-132">Configure WiFi</span></span>

<span data-ttu-id="9072e-133">IoT-projekt förlitar sig på Internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="9072e-133">IoT projects rely on Internet connectivity.</span></span> <span data-ttu-id="9072e-134">Använd följande instruktioner för att konfigurera DevKit att ansluta till Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="9072e-134">Use the following instructions to configure the DevKit to connect to WiFi.</span></span>

### <a name="enter-ap-mode"></a><span data-ttu-id="9072e-135">Ange AP läge</span><span class="sxs-lookup"><span data-stu-id="9072e-135">Enter AP Mode</span></span>

<span data-ttu-id="9072e-136">Håll ned knappen B, sedan push och släpper återställningsknappen sedan Släpp B. Din DevKit anger AP läge för att konfigurera WiFi.</span><span class="sxs-lookup"><span data-stu-id="9072e-136">Hold down button B, then push and release the reset button, then release button B. Your DevKit will enter AP Mode for configuring WiFi.</span></span> <span data-ttu-id="9072e-137">Skärmen visar tjänsten ange Identifier(SSID) av DevKit som konfiguration portal IP-adressen:</span><span class="sxs-lookup"><span data-stu-id="9072e-137">The screen will display the Service Set Identifier(SSID) of the DevKit as well as the configuration portal IP address:</span></span>

![komma-igång-Wi-Fi-ap](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ap.jpg)

### <a name="connect-to-devkit-ap"></a><span data-ttu-id="9072e-139">Ansluta till DevKit AP</span><span class="sxs-lookup"><span data-stu-id="9072e-139">Connect to DevKit AP</span></span>

<span data-ttu-id="9072e-140">Nu kan du använda en annan WiFi aktiverad enhet (dator eller mobiltelefon) för att ansluta till DevKit SSID (markerat på skärmbilden ovan), lämna lösenordet tomt.</span><span class="sxs-lookup"><span data-stu-id="9072e-140">Now, use another WiFi enabled device (PC or mobile phone) to connect to the DevKit SSID (highlighted in the screenshot above), leave the password empty.</span></span>

![komma-igång-ssid](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect-ssid.png)

### <a name="configure-wifi-for-devkit"></a><span data-ttu-id="9072e-142">Konfigurera WiFi för DevKit</span><span class="sxs-lookup"><span data-stu-id="9072e-142">Configure WiFi for DevKit</span></span>

<span data-ttu-id="9072e-143">Öppna IP-adressen visas på skärmen DevKit på din dator eller mobiltelefon webbläsare, Välj Wi-Fi-nätverk som du vill DevKit att ansluta till och sedan ange lösenordet.</span><span class="sxs-lookup"><span data-stu-id="9072e-143">Open the IP address shown on the DevKit screen on your PC or mobile phone browser, select the WiFi network you want the DevKit to connect to, then type the password.</span></span> <span data-ttu-id="9072e-144">Klicka på **Anslut** att slutföra:</span><span class="sxs-lookup"><span data-stu-id="9072e-144">Click **Connect** to complete:</span></span>

![komma igång-Wi-Fi-portal](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-portal.png)

<span data-ttu-id="9072e-146">När anslutningen lyckas, kommer DevKit startas om efter några sekunder.</span><span class="sxs-lookup"><span data-stu-id="9072e-146">Once the connection succeeds, the DevKit will reboot in a few seconds.</span></span> <span data-ttu-id="9072e-147">Om lyckades visas WiFi namn och IP-adress på skärmen:</span><span class="sxs-lookup"><span data-stu-id="9072e-147">If succeeded, you will see the WiFi name and IP address on the screen:</span></span>

![komma-igång-Wi-Fi-ip](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ip.jpg)

> [!NOTE] 
<span data-ttu-id="9072e-149">IP-adress som visas i bilden kanske inte motsvarar den faktiska IP-Adressen tilldelas och visas på skärmen DevKit.</span><span class="sxs-lookup"><span data-stu-id="9072e-149">The IP address displayed in the photo may not match the actual IP assigned and displayed on the DevKit screen.</span></span> <span data-ttu-id="9072e-150">Detta är normalt eftersom WiFi använder DHCP för att dynamiskt tilldela IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="9072e-150">This is normal as WiFi uses DHCP to dynamically assign IPs.</span></span>

<span data-ttu-id="9072e-151">När WiFi har konfigurerats, beständiga dina autentiseringsuppgifter på enheten för anslutningen, även om kopplas från.</span><span class="sxs-lookup"><span data-stu-id="9072e-151">After WiFi is configured, your credentials will be persisted on the device for that connection, even if unplugged.</span></span> <span data-ttu-id="9072e-152">Till exempel om du har konfigurerat DevKit för Wi-Fi hemma och sedan tog DevKit till office, behöver du konfigurera om AP-läge (från steg **ange AP läge**) att ansluta DevKit till kontoret WiFi.</span><span class="sxs-lookup"><span data-stu-id="9072e-152">For example, if you configured the DevKit for WiFi in your home and then took the DevKit to the office, you will need to reconfigure AP mode (starting at step **Enter AP Mode**) to connect the DevKit to your office WiFi.</span></span> 

## <a name="start-using-devkit"></a><span data-ttu-id="9072e-153">Börja använda DevKit</span><span class="sxs-lookup"><span data-stu-id="9072e-153">Start using DevKit</span></span>

<span data-ttu-id="9072e-154">Standard-appar som körs på DevKit kontrollerar den senaste versionen av den inbyggda programvaran och visa vissa diagnos sensordata för dig.</span><span class="sxs-lookup"><span data-stu-id="9072e-154">The default app running on DevKit will check the latest version of the firmware and display some sensor diagnosis data for you.</span></span>

### <a name="upgrade-to-the-latest-firmware"></a><span data-ttu-id="9072e-155">Uppgradera till den senaste inbyggda programvaran</span><span class="sxs-lookup"><span data-stu-id="9072e-155">Upgrade to the latest firmware</span></span>

<span data-ttu-id="9072e-156">Du uppmanas på skärmen som behövs för både aktuella och den senaste versionen av inbyggd programvara om det finns en uppgradering.</span><span class="sxs-lookup"><span data-stu-id="9072e-156">You will be prompted on the screen both the current and latest firmware version if there is an upgrade needed.</span></span> <span data-ttu-id="9072e-157">Följ [uppgradera inbyggda programvara](https://microsoft.github.io/azure-iot-developer-kit/docs/upgrading/) guide för att uppgradera den.</span><span class="sxs-lookup"><span data-stu-id="9072e-157">Follow [Upgrade firmware](https://microsoft.github.io/azure-iot-developer-kit/docs/upgrading/) guide to upgrade it.</span></span>

![komma-igång-inbyggd programvara](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/firmware.jpg)

> [!NOTE] 
<span data-ttu-id="9072e-159">Det här är en enstaka ansträngning när du börja utveckla på DevKit och överför din app och du har den senaste inbyggda programvaran medföljer din app.</span><span class="sxs-lookup"><span data-stu-id="9072e-159">This is a one-time effort, once you start developing on the DevKit and upload your app, you will have the latest firmware come with your app.</span></span>

### <a name="test-various-sensors"></a><span data-ttu-id="9072e-160">Testa olika sensorer</span><span class="sxs-lookup"><span data-stu-id="9072e-160">Test various sensors</span></span>

<span data-ttu-id="9072e-161">Tryck på knappen B för att testa sensorer, fortsätta trycker ned och släpper knappen B för att gå igenom varje sensor.</span><span class="sxs-lookup"><span data-stu-id="9072e-161">Press button B to test sensors, continue pressing and releasing the B button to cycle through each sensor.</span></span>

![komma-igång-sensorer](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/sensors.jpg)

## <a name="prepare-development-environment"></a><span data-ttu-id="9072e-163">Förbereda utvecklingsmiljö</span><span class="sxs-lookup"><span data-stu-id="9072e-163">Prepare development environment</span></span>

<span data-ttu-id="9072e-164">Nu är det dags att konfigurera utvecklingsmiljön: verktyg och paket som du kan skapa snygg IoT-appar.</span><span class="sxs-lookup"><span data-stu-id="9072e-164">Now it's time to set up the development environment: tools and packages for you to build stunning IoT applications.</span></span> <span data-ttu-id="9072e-165">Du kan välja Windows eller macOS version enligt ditt operativsystem.</span><span class="sxs-lookup"><span data-stu-id="9072e-165">You can choose Windows or macOS version according to your operating system.</span></span>


### <a name="windows"></a><span data-ttu-id="9072e-166">Windows</span><span class="sxs-lookup"><span data-stu-id="9072e-166">Windows</span></span>

<span data-ttu-id="9072e-167">Vi rekommenderar att du kan använda installationspaketet för att förbereda utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="9072e-167">We encourage you to use the installation package to prepare the development environment.</span></span> <span data-ttu-id="9072e-168">Om det uppstår problem kan du följa den [manuella steg](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) att få det gjort.</span><span class="sxs-lookup"><span data-stu-id="9072e-168">If you encounter any issues, you can follow the [manual steps](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) to get it done.</span></span>

#### <a name="download-latest-package"></a><span data-ttu-id="9072e-169">Hämta senaste paketet</span><span class="sxs-lookup"><span data-stu-id="9072e-169">Download latest package</span></span>

<span data-ttu-id="9072e-170">Den `.zip` filen som du hämtar innehåller alla nödvändiga verktyg och paket som krävs för DevKit utveckling.</span><span class="sxs-lookup"><span data-stu-id="9072e-170">The `.zip` file you download contains all necessary tools and packages required for DevKit development.</span></span>

> [!div class="button"]
[<span data-ttu-id="9072e-171">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="9072e-171">Download</span></span>](https://azureboard.azureedge.net/prod/installpackage/devkit_install_1.0.2.zip)


> [!NOTE] 
> <span data-ttu-id="9072e-172">Den `.zip` filen innehåller följande verktyg och paket.</span><span class="sxs-lookup"><span data-stu-id="9072e-172">The `.zip` file contains the following tools and packages.</span></span> <span data-ttu-id="9072e-173">Skriptet identifierar och hoppa över dem om du redan har några komponenter vara installerade.</span><span class="sxs-lookup"><span data-stu-id="9072e-173">If you already have some components installed, the script will detect and skip them.</span></span>
> * <span data-ttu-id="9072e-174">Node.js och Yarn: Runtime för installationsskriptet och automatiserade åtgärder</span><span class="sxs-lookup"><span data-stu-id="9072e-174">Node.js and Yarn: Runtime for the setup script and automated tasks</span></span>
> * <span data-ttu-id="9072e-175">[Azure CLI 2.0 MSI](https://docs.microsoft.com//cli/azure/install-azure-cli#windows) -kommandoradsverktyget upplevelse för flera plattformar för att hantera Azure-resurser MSI innehåller beroende Python och pip.</span><span class="sxs-lookup"><span data-stu-id="9072e-175">[Azure CLI 2.0 MSI](https://docs.microsoft.com//cli/azure/install-azure-cli#windows) - Cross-platform command-line experience for managing Azure resources, the MSI contains dependent Python and pip.</span></span>
> * <span data-ttu-id="9072e-176">[Visual Studio Code](https://code.visualstudio.com/): Lightweight redigerare för DevKit-utveckling</span><span class="sxs-lookup"><span data-stu-id="9072e-176">[Visual Studio Code](https://code.visualstudio.com/): Lightweight code editor for DevKit development</span></span>
> * <span data-ttu-id="9072e-177">[Visual Studio Code-tillägget för Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): gör Arduino utveckling i VS-kod</span><span class="sxs-lookup"><span data-stu-id="9072e-177">[Visual Studio Code extension for Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): Enables Arduino development in VS Code</span></span>
> * <span data-ttu-id="9072e-178">[Arduino IDE](https://www.arduino.cc/en/Main/Software): tillägget för Arduino är beroende av det här verktyget</span><span class="sxs-lookup"><span data-stu-id="9072e-178">[Arduino IDE](https://www.arduino.cc/en/Main/Software): The extension for Arduino relies on this tool</span></span>
> * <span data-ttu-id="9072e-179">DevKit Board paket: Verktyget kedjor, bibliotek och projekt för DevKit</span><span class="sxs-lookup"><span data-stu-id="9072e-179">DevKit Board Package: Tool chains, libraries and projects for the DevKit</span></span>
> * <span data-ttu-id="9072e-180">ST-Link-verktyget: Viktigt verktyg och drivrutiner</span><span class="sxs-lookup"><span data-stu-id="9072e-180">ST-Link Utility: Essential utilities and drivers</span></span>

#### <a name="run-installation-script"></a><span data-ttu-id="9072e-181">Kör skript för installation</span><span class="sxs-lookup"><span data-stu-id="9072e-181">Run installation script</span></span>

<span data-ttu-id="9072e-182">I Utforskaren, leta upp den `.zip` och extrahera det, hitta `install.cmd`, högerklicka och välj **”kör som administratör”** att starta.</span><span class="sxs-lookup"><span data-stu-id="9072e-182">In Windows File Explorer, locate the `.zip` and extract it, find `install.cmd`, right-click and select **"Run as administrator"** to start.</span></span>

![komma-igång-kör-admin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/run-admin.png)

<span data-ttu-id="9072e-184">Under installationen visas förloppet för varje verktyget eller paketet.</span><span class="sxs-lookup"><span data-stu-id="9072e-184">During installation, you will see the progress of each tool or package.</span></span>

![komma-igång-installation](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install.png)

#### <a name="confirm-to-install-drivers"></a><span data-ttu-id="9072e-186">Bekräfta för att installera drivrutiner</span><span class="sxs-lookup"><span data-stu-id="9072e-186">Confirm to install drivers</span></span>

<span data-ttu-id="9072e-187">VS-koden för Arduino tillägg beroende Arduino IDE.</span><span class="sxs-lookup"><span data-stu-id="9072e-187">The VS Code for Arduino extension relies on the Arduino IDE.</span></span> <span data-ttu-id="9072e-188">Om det här är första gången du installerar Arduino IDE uppmanas du att installera relevanta drivrutiner:</span><span class="sxs-lookup"><span data-stu-id="9072e-188">If this is the first time you are installing the Arduino IDE, you will be prompted to install relevant drivers:</span></span>

![komma-igång-drivrutin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/driver.png)

<span data-ttu-id="9072e-190">Det bör ta ungefär 10 minuter för att slutföra installationen beroende på din Internet-hastighet.</span><span class="sxs-lookup"><span data-stu-id="9072e-190">It should take around 10 minutes to finish installation depending on your Internet speed.</span></span> <span data-ttu-id="9072e-191">När installationen är klar bör du se Visual Studio Code och Arduino IDE genvägar på ditt skrivbord.</span><span class="sxs-lookup"><span data-stu-id="9072e-191">Once the installation is complete, you should see Visual Studio Code and Arduino IDE shortcuts on your desktop.</span></span>

> [!NOTE] 
<span data-ttu-id="9072e-192">Ibland, när du startar VS-kod uppmanas du med ett fel som inte kan hitta Arduino IDE- eller relaterade board paketet.</span><span class="sxs-lookup"><span data-stu-id="9072e-192">Occasionally, when you launch VS Code, you will be prompted with an error that cannot find Arduino IDE or related board package.</span></span> <span data-ttu-id="9072e-193">Om du vill lösa det Stäng VS-kod, ska starta Arduino IDE en gång och VS-kod hitta Arduino IDE sökväg korrekt.</span><span class="sxs-lookup"><span data-stu-id="9072e-193">To solve it, close VS Code, launch Arduino IDE once and VS Code should locate Arduino IDE path correctly.</span></span>


### <a name="macos-preview"></a><span data-ttu-id="9072e-194">macOS (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="9072e-194">macOS (Preview)</span></span>

<span data-ttu-id="9072e-195">Följ dessa steg för att förbereda utvecklingsmiljö på macOS.</span><span class="sxs-lookup"><span data-stu-id="9072e-195">Follow these steps to prepare development environment on macOS.</span></span>

#### <a name="install-azure-cli-20"></a><span data-ttu-id="9072e-196">Installera Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="9072e-196">Install Azure CLI 2.0</span></span>

<span data-ttu-id="9072e-197">Följ den [officiella guiden](https://docs.microsoft.com//cli/azure/install-azure-cli) installerar Azure CLI 2.0:</span><span class="sxs-lookup"><span data-stu-id="9072e-197">Follow the [official guide](https://docs.microsoft.com//cli/azure/install-azure-cli) to install Azure CLI 2.0:</span></span>

<span data-ttu-id="9072e-198">Installera Azure CLI 2.0 med en `curl` kommando:</span><span class="sxs-lookup"><span data-stu-id="9072e-198">Install Azure CLI 2.0 with one `curl` command:</span></span>

```bash
curl -L https://aka.ms/InstallAzureCli | bash
```

<span data-ttu-id="9072e-199">Och starta om din kommandogränssnittet för att ändringarna ska börja gälla:</span><span class="sxs-lookup"><span data-stu-id="9072e-199">And restart your command shell for changes to take effect:</span></span>

```bash
exec -l $SHELL
```

#### <a name="install-arduino-ide"></a><span data-ttu-id="9072e-200">Installera IDE Arduino</span><span class="sxs-lookup"><span data-stu-id="9072e-200">Install Arduino IDE</span></span>

<span data-ttu-id="9072e-201">Visual Studio Code Arduino tillägget är beroende av Arduino IDE.</span><span class="sxs-lookup"><span data-stu-id="9072e-201">The Visual Studio Code Arduino extension relies on the Arduino IDE.</span></span> <span data-ttu-id="9072e-202">Hämta och installera den [Arduino IDE för macOS](https://www.arduino.cc/en/Main/Software).</span><span class="sxs-lookup"><span data-stu-id="9072e-202">Download and install the [Arduino IDE for macOS](https://www.arduino.cc/en/Main/Software).</span></span>

#### <a name="install-visual-studio-code"></a><span data-ttu-id="9072e-203">Installera Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9072e-203">Install Visual Studio Code</span></span>

<span data-ttu-id="9072e-204">Hämta och installera [Visual Studio-koden för macOS](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="9072e-204">Download and install [Visual Studio Code for macOS](https://code.visualstudio.com/).</span></span> <span data-ttu-id="9072e-205">Det här är den primära utvecklingsverktyg för att skapa DevKit IoT program.</span><span class="sxs-lookup"><span data-stu-id="9072e-205">This will be the primary development tool for building DevKit IoT applications.</span></span>

####  <a name="download-latest-package"></a><span data-ttu-id="9072e-206">Hämta senaste paketet</span><span class="sxs-lookup"><span data-stu-id="9072e-206">Download latest package</span></span>

1. <span data-ttu-id="9072e-207">Installera Node.js.</span><span class="sxs-lookup"><span data-stu-id="9072e-207">Install Node.js.</span></span> <span data-ttu-id="9072e-208">Du kan använda populära macOS Pakethanteraren [Homebrew](https://brew.sh/) eller [inbyggd installer](https://nodejs.org/en/download/) att installera den.</span><span class="sxs-lookup"><span data-stu-id="9072e-208">You can use popular macOS package manager [Homebrew](https://brew.sh/) or [pre-built installer](https://nodejs.org/en/download/) to install it.</span></span>

2. <span data-ttu-id="9072e-209">Hämta `.zip` -fil som innehåller aktiviteten skript som krävs för DevKit utveckling i VS-kod.</span><span class="sxs-lookup"><span data-stu-id="9072e-209">Download `.zip` file containing task scripts required for DevKit development in VS Code.</span></span>

   > [!div class="button"]
   [<span data-ttu-id="9072e-210">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="9072e-210">Download</span></span>](https://azureboard.azureedge.net/installpackage/devkit_tasks_1.0.2.zip)

   <span data-ttu-id="9072e-211">Leta upp den `.zip` och extrahera den.</span><span class="sxs-lookup"><span data-stu-id="9072e-211">Locate the `.zip` and extract it.</span></span> <span data-ttu-id="9072e-212">Starta **Terminal** app och kör följande kommandon för att konfigurera:</span><span class="sxs-lookup"><span data-stu-id="9072e-212">Then launch **Terminal** app and run the following commands to configure:</span></span>

   <span data-ttu-id="9072e-213">Flytta extraherade mappen i mappen macOS användare:</span><span class="sxs-lookup"><span data-stu-id="9072e-213">Move extracted folder to your macOS user folder:</span></span>
   ```bash
   mv [.zip extracted folder]/azure-board-cli ~/. ; cd ~/azure-board-cli
   ```
  
   <span data-ttu-id="9072e-214">Installera `npm` paket:</span><span class="sxs-lookup"><span data-stu-id="9072e-214">Install `npm` packages:</span></span>
   ```
   npm install
   ```

#### <a name="install-vs-code-extension-for-arduino"></a><span data-ttu-id="9072e-215">Installera tillägg för VS-kod för Arduino</span><span class="sxs-lookup"><span data-stu-id="9072e-215">Install VS Code extension for Arduino</span></span>

<span data-ttu-id="9072e-216">Visual Studio Code kan du installera Marketplace tillägg direkt i verktyget, klickar du på ikonen tillägg i den vänstra meny rutan och sök sedan `Arduino` att installera:</span><span class="sxs-lookup"><span data-stu-id="9072e-216">Visual Studio Code allows you to install Marketplace extensions directly in the tool, simply click the extensions icon in the left menu pane and then search `Arduino` to install:</span></span>

![Installera tillägg](media/iot-hub-arduino-devkit-az3166-get-started/installation-extensions-mac.png)

#### <a name="install-devkit-board-package"></a><span data-ttu-id="9072e-218">Installera DevKit board paketet</span><span class="sxs-lookup"><span data-stu-id="9072e-218">Install DevKit board package</span></span>

<span data-ttu-id="9072e-219">Du behöver lägga till alla DevKit med hjälp av hanteraren för ändringar i Visual Studio-koden.</span><span class="sxs-lookup"><span data-stu-id="9072e-219">You will need to add the DevKit board using the Board Manager in Visual Studio Code.</span></span>

1. <span data-ttu-id="9072e-220">Använd `Cmd+Shift+P` att anropa kommandot paletten och skriv **Arduino** sedan söka efter och välj **Arduino: Board Manager**.</span><span class="sxs-lookup"><span data-stu-id="9072e-220">Use `Cmd+Shift+P` to invoke command palette and type **Arduino** then find and select **Arduino: Board Manager**.</span></span>

2. <span data-ttu-id="9072e-221">Klicka på **'Ytterligare URL: er'** längst ned rätt.</span><span class="sxs-lookup"><span data-stu-id="9072e-221">Click **'Additional URLs'** at the bottom right.</span></span>
   <span data-ttu-id="9072e-222">![installation av ytterligare webbadresser](media/iot-hub-arduino-devkit-az3166-get-started/installation-additional-urls-mac.png)</span><span class="sxs-lookup"><span data-stu-id="9072e-222">![installation-additional-urls](media/iot-hub-arduino-devkit-az3166-get-started/installation-additional-urls-mac.png)</span></span>

3. <span data-ttu-id="9072e-223">I den `settings.json` filen, lägga till en rad längst ned i `USER SETTINGS` fönstret och spara.</span><span class="sxs-lookup"><span data-stu-id="9072e-223">In the `settings.json` file, add a line at the bottom of `USER SETTINGS` pane and save.</span></span>
   ```json
   "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
   ```
   ![json-installation-inställningar](media/iot-hub-arduino-devkit-az3166-get-started/installation-settings-json-mac.png)

4. <span data-ttu-id="9072e-225">Nu i Board Manager söka efter 'az3166' och installera den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="9072e-225">Now in the Board Manager search for 'az3166' and install the latest version.</span></span>
   <span data-ttu-id="9072e-226">![installationen az3166](media/iot-hub-arduino-devkit-az3166-get-started/installation-az3166-mac.png)</span><span class="sxs-lookup"><span data-stu-id="9072e-226">![installation-az3166](media/iot-hub-arduino-devkit-az3166-get-started/installation-az3166-mac.png)</span></span>

<span data-ttu-id="9072e-227">Nu har du alla nödvändiga verktyg och paket som har installerats för macOS.</span><span class="sxs-lookup"><span data-stu-id="9072e-227">You now have all the necessary tools and packages installed for macOS.</span></span>


## <a name="open-project-folder"></a><span data-ttu-id="9072e-228">Öppna projektmappen</span><span class="sxs-lookup"><span data-stu-id="9072e-228">Open project folder</span></span>

### <a name="launch-vs-code"></a><span data-ttu-id="9072e-229">Starta VS Code</span><span class="sxs-lookup"><span data-stu-id="9072e-229">Launch VS Code</span></span>

<span data-ttu-id="9072e-230">Kontrollera att din DevKit inte är anslutet.</span><span class="sxs-lookup"><span data-stu-id="9072e-230">Make sure your DevKit is not connected.</span></span> <span data-ttu-id="9072e-231">Starta VS kod först och ansluta DevKit till din dator.</span><span class="sxs-lookup"><span data-stu-id="9072e-231">Launch VS Code first and connect the DevKit to your computer.</span></span> <span data-ttu-id="9072e-232">VS kod kommer automatiskt att hitta den och popup-startsidan:</span><span class="sxs-lookup"><span data-stu-id="9072e-232">VS Code will automatically find it and pop up an introduction page:</span></span>

![Mini solution vscode](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-vscode.png)

> [!NOTE] 
<span data-ttu-id="9072e-234">Ibland, när du startar VS-kod uppmanas du med fel som inte kan hitta Arduino IDE eller relaterade board paketet.</span><span class="sxs-lookup"><span data-stu-id="9072e-234">Occasionally, when you launch VS Code, you will be prompted with error that cannot find Arduino IDE or related board package.</span></span> <span data-ttu-id="9072e-235">Om du vill lösa det Stäng VS-kod, ska starta Arduino IDE igen och VS-kod hitta Arduino IDE sökväg korrekt.</span><span class="sxs-lookup"><span data-stu-id="9072e-235">To solve it, close VS Code, launch Arduino IDE once again and VS Code should locate Arduino IDE path correctly.</span></span>

### <a name="open-arduino-examples-folder"></a><span data-ttu-id="9072e-236">Öppna Arduino exempel mapp</span><span class="sxs-lookup"><span data-stu-id="9072e-236">Open Arduino Examples folder</span></span>

<span data-ttu-id="9072e-237">Växla till **'Arduino exempel'** fliken, gå till `Examples for MXCHIP AZ3166 > AzureIoT` och klicka på `GetStarted`.</span><span class="sxs-lookup"><span data-stu-id="9072e-237">Switch to **'Arduino Examples'** tab, navigate to `Examples for MXCHIP AZ3166 > AzureIoT` and click on `GetStarted`.</span></span>

![Mini-solution-exempel](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-examples.png)

<span data-ttu-id="9072e-239">Om du råkar stänga fönstret kan använda för att läsa in den, `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) att anropa kommandot paletten och skriv **Arduino** att söka efter och välj **Arduino: exempel**.</span><span class="sxs-lookup"><span data-stu-id="9072e-239">If you happen to close the pane, to reload it, use `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) to invoke command palette and type **Arduino** to find and select **Arduino: Examples**.</span></span>

## <a name="provision-azure-services"></a><span data-ttu-id="9072e-240">Etablera Azure-tjänster</span><span class="sxs-lookup"><span data-stu-id="9072e-240">Provision Azure services</span></span>

<span data-ttu-id="9072e-241">I fönstret lösning kör uppgiften `Ctrl+P` (macOS: `Cmd+P`) genom att skriva 'uppgift molnet etablera':</span><span class="sxs-lookup"><span data-stu-id="9072e-241">In the solution window, run your task through `Ctrl+P` (macOS: `Cmd+P`) by typing 'task cloud-provision':</span></span>

<span data-ttu-id="9072e-242">I VS koden terminal hjälper en interaktiv kommandorad dig att etablera nödvändiga Azure-tjänster:</span><span class="sxs-lookup"><span data-stu-id="9072e-242">In the VS Code terminal, an interactive command line will guide you through provisioning the required Azure services:</span></span>

![Mini-solution-cloud-provision](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/cloud-provision.png)

## <a name="build-and-upload-arduino-sketch"></a><span data-ttu-id="9072e-244">Skapa och ladda upp Arduino skiss</span><span class="sxs-lookup"><span data-stu-id="9072e-244">Build and upload Arduino sketch</span></span>

### <a name="install-required-library"></a><span data-ttu-id="9072e-245">Installera nödvändiga bibliotek</span><span class="sxs-lookup"><span data-stu-id="9072e-245">Install required library</span></span>

1. <span data-ttu-id="9072e-246">Tryck på `F1` eller `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) att anropa kommandot paletten och skriv **Arduino** sedan söka efter och välj **Arduino: Library Manager**.</span><span class="sxs-lookup"><span data-stu-id="9072e-246">Press `F1` or `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) to invoke command palette and type **Arduino** then find and select **Arduino: Library Manager**.</span></span>

2. <span data-ttu-id="9072e-247">Sök efter `ArduinoJson` biblioteket och klicka på **installera**</span><span class="sxs-lookup"><span data-stu-id="9072e-247">Search for `ArduinoJson` library and click **Install**</span></span>

### <a name="build-and-upload-the-device-code"></a><span data-ttu-id="9072e-248">Skapa och skicka kod för enheten</span><span class="sxs-lookup"><span data-stu-id="9072e-248">Build and upload the device code</span></span>

<span data-ttu-id="9072e-249">Använd `Ctrl+P` (macOS: `Cmd+P`) att köra 'uppgift enhet-överföringen'.</span><span class="sxs-lookup"><span data-stu-id="9072e-249">Use `Ctrl+P` (macOS: `Cmd+P`) to run 'task device-upload'.</span></span> <span data-ttu-id="9072e-250">Terminalen uppmanas du att ange konfigurationsläge.</span><span class="sxs-lookup"><span data-stu-id="9072e-250">The terminal will prompt you to enter configuration mode.</span></span> <span data-ttu-id="9072e-251">Om du vill göra det, hålla ned A, och sedan push och släpp återställningsknappen.</span><span class="sxs-lookup"><span data-stu-id="9072e-251">To do so, hold down button A, then push and release the reset button.</span></span> <span data-ttu-id="9072e-252">Skärmen visar ”Configuration”.</span><span class="sxs-lookup"><span data-stu-id="9072e-252">The screen will display 'Configuration'.</span></span> <span data-ttu-id="9072e-253">Detta är att ange anslutningssträngen som hämtas från 'aktivitet moln-provision-steget.</span><span class="sxs-lookup"><span data-stu-id="9072e-253">This is to set the connection string that retrieves from 'task cloud-provision' step.</span></span>

<span data-ttu-id="9072e-254">Sedan startas då verifierar och laddar upp Arduino ritningarna:</span><span class="sxs-lookup"><span data-stu-id="9072e-254">Then it will start verifying and uploading the Arduino sketch:</span></span>

![Mini-solution-enhet-överföringen](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/device-upload.png)

<span data-ttu-id="9072e-256">DevKit startar om och börja köra koden.</span><span class="sxs-lookup"><span data-stu-id="9072e-256">The DevKit will reboot and start running the code.</span></span>

## <a name="test-the-project"></a><span data-ttu-id="9072e-257">Testa projektet</span><span class="sxs-lookup"><span data-stu-id="9072e-257">Test the project</span></span>

<span data-ttu-id="9072e-258">Klicka på ikonen power plugin i statusfältet för att öppna Serial övervakaren i VS-kod.</span><span class="sxs-lookup"><span data-stu-id="9072e-258">In VS Code, click the power plug icon on the status bar to open the Serial Monitor.</span></span>

<span data-ttu-id="9072e-259">Exempelprogrammet körts när du ser följande resultat:</span><span class="sxs-lookup"><span data-stu-id="9072e-259">The sample application is running successfully when you see the following results:</span></span>

* <span data-ttu-id="9072e-260">Seriell övervakaren visar samma information som innehållet i skärmbilden nedan.</span><span class="sxs-lookup"><span data-stu-id="9072e-260">The Serial Monitor displays the same information as the content in the screenshot below.</span></span>
* <span data-ttu-id="9072e-261">Indikator på MXChip IoT DevKit blinkar.</span><span class="sxs-lookup"><span data-stu-id="9072e-261">The LED on MXChip IoT DevKit is blinking.</span></span>

![Slutversionen i VS-kod](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/result-serial-output.png)

## <a name="problems-and-feedback"></a><span data-ttu-id="9072e-263">Problem och feedback</span><span class="sxs-lookup"><span data-stu-id="9072e-263">Problems and feedback</span></span>

<span data-ttu-id="9072e-264">Du kan hitta [vanliga frågor och svar](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) om du stöter på problem eller nå oss från kanaler nedan.</span><span class="sxs-lookup"><span data-stu-id="9072e-264">You can find [FAQs](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) if you encounter problems or reach out to us from the channels below.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9072e-265">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9072e-265">Next steps</span></span>

<span data-ttu-id="9072e-266">Du har anslutit en MXChip IoT DevKit till din IoT-hubb, och skickas avbildade sensordata till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="9072e-266">You have successfully connected an MXChip IoT DevKit to your IoT Hub, and sent the captured sensor data to your IoT hub.</span></span>

<span data-ttu-id="9072e-267">Mer information om hur du kan komma igång med IoT Hub och utforska andra IoT-scenarier finns här:</span><span class="sxs-lookup"><span data-stu-id="9072e-267">To continue getting started with IoT Hub and to explore other IoT scenarios, see:</span></span>

- [<span data-ttu-id="9072e-268">Hantera meddelanden mellan enheter och molnet med iothub-explorer</span><span class="sxs-lookup"><span data-stu-id="9072e-268">Manage cloud device messaging with iothub-explorer</span></span>](https://docs.microsoft.com/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
- [<span data-ttu-id="9072e-269">Spara meddelanden från IoT Hub i datalagring för Azure</span><span class="sxs-lookup"><span data-stu-id="9072e-269">Save IoT Hub messages to Azure data storage</span></span>](https://docs.microsoft.com//azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
- [<span data-ttu-id="9072e-270">Använd Power BI för att visualisera sensordata i realtid från Azure IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="9072e-270">Use Power BI to visualize real-time sensor data from Azure IoT Hub</span></span>](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
- [<span data-ttu-id="9072e-271">Använd Azure Web Apps visualisera sensordata i realtid från Azure IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="9072e-271">Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub</span></span>](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
- [<span data-ttu-id="9072e-272">Väder prognos använder sensordata från IoT-hubb i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="9072e-272">Weather forecast using the sensor data from your IoT hub in Azure Machine Learning</span></span>](https://docs.microsoft.com/azure/iot-hub/iot-hub-weather-forecast-machine-learning)
- [<span data-ttu-id="9072e-273">Enhetshantering med IoT Hub-utforskaren</span><span class="sxs-lookup"><span data-stu-id="9072e-273">Device management with iothub-explorer</span></span>](https://docs.microsoft.com/azure/iot-hub/iot-hub-device-management-iothub-explorer)
- [<span data-ttu-id="9072e-274">Fjärrövervakning och aviseringar med Logic Apps</span><span class="sxs-lookup"><span data-stu-id="9072e-274">Remote monitoring and notifications with Logic Apps</span></span>](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)