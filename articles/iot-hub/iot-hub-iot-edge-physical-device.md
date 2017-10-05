---
title: "Använd en fysisk enhet med Azure IoT kant | Microsoft Docs"
description: "Hur du använder en Texas instrument SensorTag-enhet för att skicka data till en IoT-hubb via en IoT-Edge gateway körs på en hallon Pi 3-enhet. Gatewayen skapas med Azure IoT kant."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 212dacbf-e5e9-48b2-9c8a-1c14d9e7b913
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2017
ms.author: andbuc
ms.openlocfilehash: 02962a91c739a53dfcf947bcc736e5c293b9384f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-iot-edge-on-a-raspberry-pi-to-forward-device-to-cloud-messages-to-iot-hub"></a><span data-ttu-id="c4da1-104">Använda Azure IoT kanten på en hallon Pi för att vidarebefordra meddelanden från enhet till moln till IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="c4da1-104">Use Azure IoT Edge on a Raspberry Pi to forward device-to-cloud messages to IoT Hub</span></span>

<span data-ttu-id="c4da1-105">Den här genomgången av den [Bluetooth låg energiförbrukning exempel] [ lnk-ble-samplecode] visar hur du använder [Azure IoT kant] [ lnk-sdk] till:</span><span class="sxs-lookup"><span data-stu-id="c4da1-105">This walkthrough of the [Bluetooth low energy sample][lnk-ble-samplecode] shows you how to use [Azure IoT Edge][lnk-sdk] to:</span></span>

* <span data-ttu-id="c4da1-106">Vidarebefordra enhet till moln telemetri till IoT-hubb från en fysisk enhet.</span><span class="sxs-lookup"><span data-stu-id="c4da1-106">Forward device-to-cloud telemetry to IoT Hub from a physical device.</span></span>
* <span data-ttu-id="c4da1-107">Väg kommandon från IoT-hubben till en fysisk enhet.</span><span class="sxs-lookup"><span data-stu-id="c4da1-107">Route commands from IoT Hub to a physical device.</span></span>

<span data-ttu-id="c4da1-108">Den här genomgången omfattar:</span><span class="sxs-lookup"><span data-stu-id="c4da1-108">This walkthrough covers:</span></span>

* <span data-ttu-id="c4da1-109">**Arkitektur för**: viktig arkitektur information om Bluetooth låg energiförbrukning exemplet.</span><span class="sxs-lookup"><span data-stu-id="c4da1-109">**Architecture**: important architectural information about the Bluetooth low energy sample.</span></span>
* <span data-ttu-id="c4da1-110">**Skapa och kör**: stegen som krävs för att bygga och köra exemplet.</span><span class="sxs-lookup"><span data-stu-id="c4da1-110">**Build and run**: the steps required to build and run the sample.</span></span>

## <a name="architecture"></a><span data-ttu-id="c4da1-111">Arkitektur</span><span class="sxs-lookup"><span data-stu-id="c4da1-111">Architecture</span></span>

<span data-ttu-id="c4da1-112">Den här genomgången visar hur du skapar och kör en IoT-gräns-gatewayen på en hallon Pi 3 som kör Raspbian Linux.</span><span class="sxs-lookup"><span data-stu-id="c4da1-112">The walkthrough shows you how to build and run an IoT Edge gateway on a Raspberry Pi 3 that runs Raspbian Linux.</span></span> <span data-ttu-id="c4da1-113">Gatewayen skapas med IoT kant.</span><span class="sxs-lookup"><span data-stu-id="c4da1-113">The gateway is built using IoT Edge.</span></span> <span data-ttu-id="c4da1-114">En Texas instrument SensorTag Bluetooth låg energiförbrukning (a)-enhet används för att samla in data om temperatur.</span><span class="sxs-lookup"><span data-stu-id="c4da1-114">The sample uses a Texas Instruments SensorTag Bluetooth Low Energy (BLE) device to collect temperature data.</span></span>

<span data-ttu-id="c4da1-115">När du kör IoT gräns-gatewayen den:</span><span class="sxs-lookup"><span data-stu-id="c4da1-115">When you run the IoT Edge gateway it:</span></span>

* <span data-ttu-id="c4da1-116">Ansluter till en SensorTag-enhet med hjälp av Bluetooth låg energiförbrukning (a)-protokollet.</span><span class="sxs-lookup"><span data-stu-id="c4da1-116">Connects to a SensorTag device using the Bluetooth Low Energy (BLE) protocol.</span></span>
* <span data-ttu-id="c4da1-117">Ansluter till IoT-hubb med hjälp av HTTP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="c4da1-117">Connects to IoT Hub using the HTTP protocol.</span></span>
* <span data-ttu-id="c4da1-118">Vidarebefordrar telemetri från SensorTag-enhet till IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c4da1-118">Forwards telemetry from the SensorTag device to IoT Hub.</span></span>
* <span data-ttu-id="c4da1-119">Vägar kommandon från IoT-hubb SensorTag-enheter.</span><span class="sxs-lookup"><span data-stu-id="c4da1-119">Routes commands from IoT Hub to the SensorTag device.</span></span>

<span data-ttu-id="c4da1-120">Gatewayen innehåller följande IoT kant moduler:</span><span class="sxs-lookup"><span data-stu-id="c4da1-120">The gateway contains the following IoT Edge modules:</span></span>

* <span data-ttu-id="c4da1-121">En *TIVERA modulen* som gränssnitt med TIVERA enheten ska ta emot temperatur data från enheten och skicka kommandon till enheten.</span><span class="sxs-lookup"><span data-stu-id="c4da1-121">A *BLE module* that interfaces with a BLE device to receive temperature data from the device and send commands to the device.</span></span>
* <span data-ttu-id="c4da1-122">En *TIVERA moln till enhet modulen* som omvandlar JSON-meddelanden som skickats från IoT-hubb i TIVERA instruktioner för den *TIVERA modulen*.</span><span class="sxs-lookup"><span data-stu-id="c4da1-122">A *BLE cloud to device module* that translates JSON messages sent from IoT Hub into BLE instructions for the *BLE module*.</span></span>
* <span data-ttu-id="c4da1-123">En *loggaren modulen* som loggar alla gateway-meddelanden till en lokal fil.</span><span class="sxs-lookup"><span data-stu-id="c4da1-123">A *logger module* that logs all gateway messages to a local file.</span></span>
* <span data-ttu-id="c4da1-124">En *identitet mappningsmodul* som omvandlar mellan TIVERA enhetens MAC-adresser och identiteter för Azure IoT Hub-enhet.</span><span class="sxs-lookup"><span data-stu-id="c4da1-124">An *identity mapping module* that translates between BLE device MAC addresses and Azure IoT Hub device identities.</span></span>
* <span data-ttu-id="c4da1-125">En *IoT-hubb modulen* som överför telemetridata till en IoT-hubb och tar emot enhetskommandon från en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c4da1-125">An *IoT Hub module* that uploads telemetry data to an IoT hub and receives device commands from an IoT hub.</span></span>
* <span data-ttu-id="c4da1-126">En *TIVERA skrivare modulen* som tolkar telemetri från TIVERA enheten och skriver ut formaterade data i konsolen för att aktivera felsökning.</span><span class="sxs-lookup"><span data-stu-id="c4da1-126">A *BLE printer module* that interprets telemetry from the BLE device and prints formatted data to the console to enable troubleshooting and debugging.</span></span>

### <a name="how-data-flows-through-the-gateway"></a><span data-ttu-id="c4da1-127">Hur data flödar via gatewayen</span><span class="sxs-lookup"><span data-stu-id="c4da1-127">How data flows through the gateway</span></span>

<span data-ttu-id="c4da1-128">Blockera följande diagram illustrerar flödet-pipeline telemetri överför data:</span><span class="sxs-lookup"><span data-stu-id="c4da1-128">The following block diagram illustrates the telemetry upload data flow pipeline:</span></span>

![Telemetri överför gateway pipeline](media/iot-hub-iot-edge-physical-device/gateway_ble_upload_data_flow.png)

<span data-ttu-id="c4da1-130">De steg som ett objekt av telemetri tar reser från en tabell enhet till IoT-hubben är:</span><span class="sxs-lookup"><span data-stu-id="c4da1-130">The steps that an item of telemetry takes traveling from a BLE device to IoT Hub are:</span></span>

1. <span data-ttu-id="c4da1-131">Aktivera enheten genererar ett exempel på en temperatur och skickar den via Bluetooth till modulen TIVERA i gatewayen.</span><span class="sxs-lookup"><span data-stu-id="c4da1-131">The BLE device generates a temperature sample and sends it over Bluetooth to the BLE module in the gateway.</span></span>
1. <span data-ttu-id="c4da1-132">Modulen TIVERA tar emot exemplet och publicerar till Service broker tillsammans med MAC-adressen till enheten.</span><span class="sxs-lookup"><span data-stu-id="c4da1-132">The BLE module receives the sample and publishes it to the broker along with the MAC address of the device.</span></span>
1. <span data-ttu-id="c4da1-133">Mappningsmodul identitet hämtar det här meddelandet och använder en intern tabell för att översätta MAC-adressen till enheten till en IoT-hubb enhetens identitet.</span><span class="sxs-lookup"><span data-stu-id="c4da1-133">The identity mapping module picks up this message and uses an internal table to translate the MAC address of the device into an IoT Hub device identity.</span></span> <span data-ttu-id="c4da1-134">En IoT-hubb enhetsidentitet består av en enhets-ID och nyckeln för enheten.</span><span class="sxs-lookup"><span data-stu-id="c4da1-134">An IoT Hub device identity consists of a device ID and device key.</span></span>
1. <span data-ttu-id="c4da1-135">Modulen för mappning identitet publicerar ett nytt meddelande som innehåller exempeldata temperatur, MAC-adressen till enheten, enhets-ID och nyckeln för enheten.</span><span class="sxs-lookup"><span data-stu-id="c4da1-135">The identity mapping module publishes a new message that contains the temperature sample data, the MAC address of the device, the device ID, and the device key.</span></span>
1. <span data-ttu-id="c4da1-136">IoT-hubb-modulen får ett nytt meddelande (som genererats av modulen för mappning identitet) och publicerar till IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c4da1-136">The IoT Hub module receives this new message (generated by the identity mapping module) and publishes it to IoT Hub.</span></span>
1. <span data-ttu-id="c4da1-137">Modulen loggaren loggar alla meddelanden från Service broker till en lokal fil.</span><span class="sxs-lookup"><span data-stu-id="c4da1-137">The logger module logs all messages from the broker to a local file.</span></span>

<span data-ttu-id="c4da1-138">Blockera följande diagram illustrerar enheten kommandot data flödet pipeline:</span><span class="sxs-lookup"><span data-stu-id="c4da1-138">The following block diagram illustrates the device command data flow pipeline:</span></span>

![Enheten kommandot gateway pipeline](media/iot-hub-iot-edge-physical-device/gateway_ble_command_data_flow.png)

1. <span data-ttu-id="c4da1-140">Modulen IoT-hubb avsöker med jämna mellanrum IoT-hubb för nya kommandomeddelanden.</span><span class="sxs-lookup"><span data-stu-id="c4da1-140">The IoT Hub module periodically polls the IoT hub for new command messages.</span></span>
1. <span data-ttu-id="c4da1-141">När modulen IoT-hubb tar emot ett nytt kommando-meddelande, publicerar den till Service broker.</span><span class="sxs-lookup"><span data-stu-id="c4da1-141">When the IoT Hub module receives a new command message, it publishes it to the broker.</span></span>
1. <span data-ttu-id="c4da1-142">Modulen för mappning identitet hämtar kommandot meddelandet och använder en intern tabell för att översätta IoT-hubb enhets-ID till en enhet MAC-adress.</span><span class="sxs-lookup"><span data-stu-id="c4da1-142">The identity mapping module picks up the command message and uses an internal table to translate the IoT Hub device ID to a device MAC address.</span></span> <span data-ttu-id="c4da1-143">Den publicerar sedan ett nytt meddelande som innehåller MAC-adressen för målenheten i Egenskaper för kartan i meddelandet.</span><span class="sxs-lookup"><span data-stu-id="c4da1-143">It then publishes a new message that includes the MAC address of the target device in the properties map of the message.</span></span>
1. <span data-ttu-id="c4da1-144">Modulen TIVERA moln till enhet tar upp det här meddelandet och översätter till rätt TIVERA instruktionen för modulen tabell.</span><span class="sxs-lookup"><span data-stu-id="c4da1-144">The BLE Cloud-to-Device module picks up this message and translates it into the proper BLE instruction for the BLE module.</span></span> <span data-ttu-id="c4da1-145">Den publicerar sedan ett nytt meddelande.</span><span class="sxs-lookup"><span data-stu-id="c4da1-145">It then publishes a new message.</span></span>
1. <span data-ttu-id="c4da1-146">Modulen TIVERA hämtar det här meddelandet och utför i/o-instruktion genom att kommunicera med Bell-enheter.</span><span class="sxs-lookup"><span data-stu-id="c4da1-146">The BLE module picks up this message and executes the I/O instruction by communicating with the BLE device.</span></span>
1. <span data-ttu-id="c4da1-147">Modulen loggaren loggar alla meddelanden från Service broker på en disk.</span><span class="sxs-lookup"><span data-stu-id="c4da1-147">The logger module logs all messages from the broker to a disk file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4da1-148">Krav</span><span class="sxs-lookup"><span data-stu-id="c4da1-148">Prerequisites</span></span>

<span data-ttu-id="c4da1-149">Du behöver en aktiv Azure-prenumeration för att kunna utföra stegen i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="c4da1-149">To complete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="c4da1-150">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="c4da1-150">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="c4da1-151">Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="c4da1-151">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

<span data-ttu-id="c4da1-152">Du måste SSH-klienten på den stationära datorn så att du kan fjärransluta till kommandoraden på hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="c4da1-152">You need SSH client on your desktop machine to enable you to remotely access the command line on the Raspberry Pi.</span></span>

- <span data-ttu-id="c4da1-153">Windows innehåller inte en SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="c4da1-153">Windows does not include an SSH client.</span></span> <span data-ttu-id="c4da1-154">Vi rekommenderar att du använder [PuTTY](http://www.putty.org/).</span><span class="sxs-lookup"><span data-stu-id="c4da1-154">We recommend using [PuTTY](http://www.putty.org/).</span></span>
- <span data-ttu-id="c4da1-155">De flesta distributioner för Linux och Mac OS inkluderar SSH-kommandoradsverktyget.</span><span class="sxs-lookup"><span data-stu-id="c4da1-155">Most Linux distributions and Mac OS include the command-line SSH utility.</span></span> <span data-ttu-id="c4da1-156">Mer information finns i [SSH med Linux- eller Mac OS x](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span><span class="sxs-lookup"><span data-stu-id="c4da1-156">For more information, see [SSH Using Linux or Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span></span>

## <a name="prepare-your-hardware"></a><span data-ttu-id="c4da1-157">Förbered maskinvaran</span><span class="sxs-lookup"><span data-stu-id="c4da1-157">Prepare your hardware</span></span>

<span data-ttu-id="c4da1-158">Den här kursen förutsätter att du använder en [Texas instrument SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) enheten ansluten till en hallon Pi 3 kör Raspbian.</span><span class="sxs-lookup"><span data-stu-id="c4da1-158">This tutorial assumes you are using a [Texas Instruments SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) device connected to a Raspberry Pi 3 running Raspbian.</span></span>

### <a name="install-raspbian"></a><span data-ttu-id="c4da1-159">Installera Raspbian</span><span class="sxs-lookup"><span data-stu-id="c4da1-159">Install Raspbian</span></span>

<span data-ttu-id="c4da1-160">Du kan använda något av följande alternativ för att installera Raspbian på enheten hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="c4da1-160">You can use either of the following options to install Raspbian on your Raspberry Pi 3 device.</span></span>

* <span data-ttu-id="c4da1-161">Du kan installera den senaste versionen av Raspbian den [NOOBS] [ lnk-noobs] grafiskt användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="c4da1-161">To install the latest version of Raspbian, use the [NOOBS][lnk-noobs] graphical user interface.</span></span>
* <span data-ttu-id="c4da1-162">Manuellt [hämta] [ lnk-raspbian] och skriva den senaste avbildningen av operativsystemet Raspbian till ett SD-kort.</span><span class="sxs-lookup"><span data-stu-id="c4da1-162">Manually [download][lnk-raspbian] and write the latest image of the Raspbian operating system to an SD card.</span></span>

### <a name="sign-in-and-access-the-terminal"></a><span data-ttu-id="c4da1-163">Logga in och öppna terminalen</span><span class="sxs-lookup"><span data-stu-id="c4da1-163">Sign in and access the terminal</span></span>

<span data-ttu-id="c4da1-164">Du har två alternativ för att komma åt en terminal miljö på din hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="c4da1-164">You have two options to access a terminal environment on your Raspberry Pi:</span></span>

* <span data-ttu-id="c4da1-165">Om du har ett tangentbord och bildskärm ansluten till din hallon Pi, kan du använda det grafiska Användargränssnittet Raspbian att komma åt ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="c4da1-165">If you have a keyboard and monitor connected to your Raspberry Pi, you can use the Raspbian GUI to access a terminal window.</span></span>

* <span data-ttu-id="c4da1-166">Åtkomst till kommandoraden på din hallon Pi använder SSH från den stationära datorn.</span><span class="sxs-lookup"><span data-stu-id="c4da1-166">Access the command line on your Raspberry Pi using SSH from your desktop machine.</span></span>

#### <a name="use-a-terminal-window-in-the-gui"></a><span data-ttu-id="c4da1-167">Använd ett terminalfönster i det grafiska Användargränssnittet</span><span class="sxs-lookup"><span data-stu-id="c4da1-167">Use a terminal Window in the GUI</span></span>

<span data-ttu-id="c4da1-168">Standardautentiseringsuppgifterna för Raspbian är användarnamn **pi** och lösenord **hallon**.</span><span class="sxs-lookup"><span data-stu-id="c4da1-168">The default credentials for Raspbian are username **pi** and password **raspberry**.</span></span> <span data-ttu-id="c4da1-169">I Aktivitetsfältet i det grafiska Användargränssnittet kan du starta den **Terminal** verktyget med hjälp av ikonen som ser ut som en bildskärm.</span><span class="sxs-lookup"><span data-stu-id="c4da1-169">In the task bar in the GUI, you can launch the **Terminal** utility using the icon that looks like a monitor.</span></span>

#### <a name="sign-in-with-ssh"></a><span data-ttu-id="c4da1-170">Logga in med SSH</span><span class="sxs-lookup"><span data-stu-id="c4da1-170">Sign in with SSH</span></span>

<span data-ttu-id="c4da1-171">Du kan använda SSH för kommandoraden åtkomst till din hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="c4da1-171">You can use SSH for command-line access to your Raspberry Pi.</span></span> <span data-ttu-id="c4da1-172">Artikeln [SSH (Secure Shell)] [ lnk-pi-ssh] beskriver hur du konfigurerar SSH på din hallon Pi och hur du ansluter från [Windows] [ lnk-ssh-windows] eller [ Linux- och Mac OS][lnk-ssh-linux].</span><span class="sxs-lookup"><span data-stu-id="c4da1-172">The article [SSH (Secure Shell)][lnk-pi-ssh] describes how to configure SSH on your Raspberry Pi, and how to connect from [Windows][lnk-ssh-windows] or [Linux & Mac OS][lnk-ssh-linux].</span></span>

<span data-ttu-id="c4da1-173">Logga in med användarnamn **pi** och lösenord **hallon**.</span><span class="sxs-lookup"><span data-stu-id="c4da1-173">Sign in with username **pi** and password **raspberry**.</span></span>

### <a name="install-bluez-537"></a><span data-ttu-id="c4da1-174">Installera BlueZ 5.37</span><span class="sxs-lookup"><span data-stu-id="c4da1-174">Install BlueZ 5.37</span></span>

<span data-ttu-id="c4da1-175">TIVERA moduler prata med Bluetooth maskinvaran via BlueZ stacken.</span><span class="sxs-lookup"><span data-stu-id="c4da1-175">The BLE modules talk to the Bluetooth hardware via the BlueZ stack.</span></span> <span data-ttu-id="c4da1-176">Du behöver version 5.37 av BlueZ för modulerna ska fungera korrekt.</span><span class="sxs-lookup"><span data-stu-id="c4da1-176">You need version 5.37 of BlueZ for the modules to work correctly.</span></span> <span data-ttu-id="c4da1-177">Dessa anvisningar se till att rätt version av BlueZ är installerad.</span><span class="sxs-lookup"><span data-stu-id="c4da1-177">These instructions make sure the correct version of BlueZ is installed.</span></span>

1. <span data-ttu-id="c4da1-178">Stoppa den aktuella bluetooth-daemonen:</span><span class="sxs-lookup"><span data-stu-id="c4da1-178">Stop the current bluetooth daemon:</span></span>

    ```sh
    sudo systemctl stop bluetooth
    ```

1. <span data-ttu-id="c4da1-179">Installera BlueZ beroenden:</span><span class="sxs-lookup"><span data-stu-id="c4da1-179">Install the BlueZ dependencies:</span></span>

    ```sh
    sudo apt-get update
    sudo apt-get install bluetooth bluez-tools build-essential autoconf glib2.0 libglib2.0-dev libdbus-1-dev libudev-dev libical-dev libreadline-dev
    ```

1. <span data-ttu-id="c4da1-180">Ladda ned källkoden BlueZ från bluez.org:</span><span class="sxs-lookup"><span data-stu-id="c4da1-180">Download the BlueZ source code from bluez.org:</span></span>

    ```sh
    wget http://www.kernel.org/pub/linux/bluetooth/bluez-5.37.tar.xz
    ```

1. <span data-ttu-id="c4da1-181">Packa upp källkoden:</span><span class="sxs-lookup"><span data-stu-id="c4da1-181">Unzip the source code:</span></span>

    ```sh
    tar -xvf bluez-5.37.tar.xz
    ```

1. <span data-ttu-id="c4da1-182">Ändra sökvägen till den nya mappen:</span><span class="sxs-lookup"><span data-stu-id="c4da1-182">Change directories to the newly created folder:</span></span>

    ```sh
    cd bluez-5.37
    ```

1. <span data-ttu-id="c4da1-183">Konfigurera BlueZ koden skapas:</span><span class="sxs-lookup"><span data-stu-id="c4da1-183">Configure the BlueZ code to be built:</span></span>

    ```sh
    ./configure --disable-udev --disable-systemd --enable-experimental
    ```

1. <span data-ttu-id="c4da1-184">Skapa BlueZ:</span><span class="sxs-lookup"><span data-stu-id="c4da1-184">Build BlueZ:</span></span>

    ```sh
    make
    ```

1. <span data-ttu-id="c4da1-185">Installera BlueZ när det är klart byggnads:</span><span class="sxs-lookup"><span data-stu-id="c4da1-185">Install BlueZ once it is done building:</span></span>

    ```sh
    sudo make install
    ```

1. <span data-ttu-id="c4da1-186">Ändra systemd tjänstkonfigurationen för bluetooth så att den pekar på den nya bluetooth-daemonen i filen `/lib/systemd/system/bluetooth.service`.</span><span class="sxs-lookup"><span data-stu-id="c4da1-186">Change systemd service configuration for bluetooth so it points to the new bluetooth daemon in the file `/lib/systemd/system/bluetooth.service`.</span></span> <span data-ttu-id="c4da1-187">Ersätt 'ExecStart' raden med följande text:</span><span class="sxs-lookup"><span data-stu-id="c4da1-187">Replace the 'ExecStart' line with the following text:</span></span>

    ```conf
    ExecStart=/usr/local/libexec/bluetooth/bluetoothd -E
    ```

### <a name="enable-connectivity-to-the-sensortag-device-from-your-raspberry-pi-3-device"></a><span data-ttu-id="c4da1-188">Aktivera anslutning till SensorTag-enheter från enheten hallon Pi 3</span><span class="sxs-lookup"><span data-stu-id="c4da1-188">Enable connectivity to the SensorTag device from your Raspberry Pi 3 device</span></span>

<span data-ttu-id="c4da1-189">Du måste verifiera att din hallon Pi 3 kan ansluta till SensorTag-enheten innan du kör exemplet.</span><span class="sxs-lookup"><span data-stu-id="c4da1-189">Before running the sample, you need to verify that your Raspberry Pi 3 can connect to the SensorTag device.</span></span>

1. <span data-ttu-id="c4da1-190">Se till att den `rfkill` verktyg installeras:</span><span class="sxs-lookup"><span data-stu-id="c4da1-190">Ensure the `rfkill` utility is installed:</span></span>

    ```sh
    sudo apt-get install rfkill
    ```

1. <span data-ttu-id="c4da1-191">Tillåt bluetooth på hallon Pi 3 och kontrollera att versionsnumret är **5.37**:</span><span class="sxs-lookup"><span data-stu-id="c4da1-191">Unblock bluetooth on the Raspberry Pi 3 and check that the version number is **5.37**:</span></span>

    ```sh
    sudo rfkill unblock bluetooth
    bluetoothctl --version
    ```

1. <span data-ttu-id="c4da1-192">Du anger interaktiv bluetooth-gränssnittet genom att starta tjänsten bluetooth och köra den **bluetoothctl** kommando:</span><span class="sxs-lookup"><span data-stu-id="c4da1-192">To enter the interactive bluetooth shell, start the bluetooth service and execute the **bluetoothctl** command :</span></span>

    ```sh
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. <span data-ttu-id="c4da1-193">Ange kommandot **effekt på** att starta bluetooth-styrenhet.</span><span class="sxs-lookup"><span data-stu-id="c4da1-193">Enter the command **power on** to power up the bluetooth controller.</span></span> <span data-ttu-id="c4da1-194">Kommandot returnerar utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="c4da1-194">The command returns output similar to the following:</span></span>

    ```sh
    [NEW] Controller 98:4F:EE:04:1F:DF C3 raspberrypi [default]
    ```

1. <span data-ttu-id="c4da1-195">Ange kommandot i interaktiva bluetooth-shell **genomsökning på** att söka efter Bluetooth-enheter.</span><span class="sxs-lookup"><span data-stu-id="c4da1-195">In the interactive bluetooth shell, enter the command **scan on** to scan for bluetooth devices.</span></span> <span data-ttu-id="c4da1-196">Kommandot returnerar utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="c4da1-196">The command returns output similar to the following:</span></span>

    ```sh
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

1. <span data-ttu-id="c4da1-197">Att den SensorTag kan identifieras genom att trycka på knappen liten (grön Indikator bör flash).</span><span class="sxs-lookup"><span data-stu-id="c4da1-197">Make the SensorTag device discoverable by pressing the small button (the green LED should flash).</span></span> <span data-ttu-id="c4da1-198">Hallon Pi 3 ska identifiera SensorTag-enheter:</span><span class="sxs-lookup"><span data-stu-id="c4da1-198">The Raspberry Pi 3 should discover the SensorTag device:</span></span>

    ```sh
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```

    <span data-ttu-id="c4da1-199">I det här exemplet ser du att MAC-adressen för SensorTag-enheten är **A0:E6:F8:B5:F6:00**.</span><span class="sxs-lookup"><span data-stu-id="c4da1-199">In this example, you can see that the MAC address of the SensorTag device is **A0:E6:F8:B5:F6:00**.</span></span>

1. <span data-ttu-id="c4da1-200">Inaktivera genomsökning genom att ange den **genomsökning av** kommando:</span><span class="sxs-lookup"><span data-stu-id="c4da1-200">Turn off scanning by entering the **scan off** command:</span></span>

    ```sh
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

1. <span data-ttu-id="c4da1-201">Ansluta till enheten SensorTag med dess MAC-adress genom att ange **ansluta \<MAC-adress\>**.</span><span class="sxs-lookup"><span data-stu-id="c4da1-201">Connect to your SensorTag device using its MAC address by entering **connect \<MAC address\>**.</span></span> <span data-ttu-id="c4da1-202">Följande exempel på utdata förkortas för tydlighetens skull:</span><span class="sxs-lookup"><span data-stu-id="c4da1-202">The following sample output is abbreviated for clarity:</span></span>

    ```sh
    Attempting to connect to A0:E6:F8:B5:F6:00
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: yes
    Connection successful
    [CHG] Device A0:E6:F8:B5:F6:00 UUIDs: 00001800-0000-1000-8000-00805f9b34fb
    ...
    [NEW] Primary Service
            /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
            Device Information
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 GattServices: /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 Name: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Alias: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Modalias: bluetooth:v000Dp0000d0110
    ```

    > <span data-ttu-id="c4da1-203">Du kan ange GATT-egenskaperna för enheten igen med den **listan attribut** kommando.</span><span class="sxs-lookup"><span data-stu-id="c4da1-203">You can list the GATT characteristics of the device again using the **list-attributes** command.</span></span>

1. <span data-ttu-id="c4da1-204">Du kan nu koppla från enheten med den **koppla från** kommandot och avsluta sedan från bluetooth shell använder den **avsluta** kommando:</span><span class="sxs-lookup"><span data-stu-id="c4da1-204">You can now disconnect from the device using the **disconnect** command and then exit from the bluetooth shell using the **quit** command:</span></span>

    ```sh
    Attempting to disconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

<span data-ttu-id="c4da1-205">Du är nu redo att köra exemplet TIVERA IoT kanten på din hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="c4da1-205">You're now ready to run the BLE IoT Edge sample on your Raspberry Pi 3.</span></span>

## <a name="run-the-iot-edge-ble-sample"></a><span data-ttu-id="c4da1-206">Kör IoT kant Bell-exempel</span><span class="sxs-lookup"><span data-stu-id="c4da1-206">Run the IoT Edge BLE sample</span></span>

<span data-ttu-id="c4da1-207">Om du vill köra exemplet IoT kant ort, måste du utföra tre uppgifter:</span><span class="sxs-lookup"><span data-stu-id="c4da1-207">To run the IoT Edge BLE sample, you need to complete three tasks:</span></span>

* <span data-ttu-id="c4da1-208">Konfigurera två exempel enheter i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c4da1-208">Configure two sample devices in your IoT Hub.</span></span>
* <span data-ttu-id="c4da1-209">Skapa IoT kanten på enheten hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="c4da1-209">Build IoT Edge on your Raspberry Pi 3 device.</span></span>
* <span data-ttu-id="c4da1-210">Konfigurera och köra exemplet TIVERA på enheten hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="c4da1-210">Configure and run the BLE sample on your Raspberry Pi 3 device.</span></span>

<span data-ttu-id="c4da1-211">Vid tidpunkten som skrivs stöder IoT kant endast TIVERA moduler i gateways som körs på Linux.</span><span class="sxs-lookup"><span data-stu-id="c4da1-211">At the time of writing, IoT Edge only supports BLE modules in gateways running on Linux.</span></span>

### <a name="configure-two-sample-devices-in-your-iot-hub"></a><span data-ttu-id="c4da1-212">Konfigurera två exempel enheter i din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="c4da1-212">Configure two sample devices in your IoT Hub</span></span>

* <span data-ttu-id="c4da1-213">[Skapa en IoT-hubb] [ lnk-create-hub] i din Azure-prenumeration, måste namnet på din hubb för att slutföra den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="c4da1-213">[Create an IoT hub][lnk-create-hub] in your Azure subscription, you need the name of your hub to complete this walkthrough.</span></span> <span data-ttu-id="c4da1-214">Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="c4da1-214">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="c4da1-215">Lägg till en enhet som kallas **SensorTag_01** till IoT-hubb och gjort en notering om dess id och enheten nyckel.</span><span class="sxs-lookup"><span data-stu-id="c4da1-215">Add one device called **SensorTag_01** to your IoT hub and make a note of its id and device key.</span></span> <span data-ttu-id="c4da1-216">Du kan använda den [enheten explorer eller iothub-explorer] [ lnk-explorer-tools] verktyg för att lägga till den här enheten i IoT-hubb som du skapade i föregående steg och hämta dess nyckel.</span><span class="sxs-lookup"><span data-stu-id="c4da1-216">You can use the [device explorer or iothub-explorer][lnk-explorer-tools] tools to add this device to the IoT hub you created in the previous step and to retrieve its key.</span></span> <span data-ttu-id="c4da1-217">Du kan mappa den här enheten till SensorTag-enheter när du konfigurerar en gateway.</span><span class="sxs-lookup"><span data-stu-id="c4da1-217">You map this device to the SensorTag device when you configure the gateway.</span></span>

### <a name="build-azure-iot-edge-on-your-raspberry-pi-3"></a><span data-ttu-id="c4da1-218">Skapa Azure IoT kanten på din Raspberry Pi 3</span><span class="sxs-lookup"><span data-stu-id="c4da1-218">Build Azure IoT Edge on your Raspberry Pi 3</span></span>

<span data-ttu-id="c4da1-219">Installera beroenden för Azure IoT kant:</span><span class="sxs-lookup"><span data-stu-id="c4da1-219">Install dependencies for Azure IoT Edge:</span></span>

```sh
sudo apt-get install cmake uuid-dev curl libcurl4-openssl-dev libssl-dev
```

<span data-ttu-id="c4da1-220">Använd följande kommandon för att klona IoT kant och alla dess submodules till arbetskatalogen:</span><span class="sxs-lookup"><span data-stu-id="c4da1-220">Use the following commands to clone IoT Edge and all its submodules to your home directory:</span></span>

```sh
cd ~
git clone https://github.com/Azure/iot-edge.git
```

<span data-ttu-id="c4da1-221">När du har en fullständig kopia av databasen IoT kanten på din hallon Pi 3 kan du skapa den med hjälp av följande kommando från mappen som innehåller SDK:</span><span class="sxs-lookup"><span data-stu-id="c4da1-221">When you have a complete copy of the IoT Edge repository on your Raspberry Pi 3, you can build it using the following command from the folder that contains the SDK:</span></span>

```sh
cd ~/iot-edge
./tools/build.sh  --disable-native-remote-modules
```

### <a name="configure-and-run-the-ble-sample-on-your-raspberry-pi-3"></a><span data-ttu-id="c4da1-222">Konfigurera och köra exemplet TIVERA på din hallon Pi 3</span><span class="sxs-lookup"><span data-stu-id="c4da1-222">Configure and run the BLE sample on your Raspberry Pi 3</span></span>

<span data-ttu-id="c4da1-223">För att starta och köra exemplet, måste du konfigurera varje IoT kant-modul som deltar i gatewayen.</span><span class="sxs-lookup"><span data-stu-id="c4da1-223">To bootstrap and run the sample, you must configure each IoT Edge module that participates in the gateway.</span></span> <span data-ttu-id="c4da1-224">Den här konfigurationen finns i en JSON-fil och du måste konfigurera alla fem deltagande IoT kant moduler.</span><span class="sxs-lookup"><span data-stu-id="c4da1-224">This configuration is provided in a JSON file and you must configure all five participating IoT Edge modules.</span></span> <span data-ttu-id="c4da1-225">Det finns ett exempel JSON-fil i databasen kallas **gateway\_sample.json** att du kan använda som utgångspunkt för att skapa egna konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="c4da1-225">There is a sample JSON file in the repository called **gateway\_sample.json** that you can use as the starting point for building your own configuration file.</span></span> <span data-ttu-id="c4da1-226">Den här filen finns i den **src-exempel/ble_gateway** mapp i lokal kopia av databasen IoT kant.</span><span class="sxs-lookup"><span data-stu-id="c4da1-226">This file is in the **samples/ble_gateway/src** folder in local copy of the IoT Edge repository.</span></span>

<span data-ttu-id="c4da1-227">I följande avsnitt beskrivs hur du redigerar den här konfigurationsfilen för tabell och förutsätter att IoT kant-databasen finns i den **/home/pi/iot-edge /** mapp på din hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="c4da1-227">The following sections describe how to edit this configuration file for the BLE sample and assume that the IoT Edge repository is in the **/home/pi/iot-edge/** folder on your Raspberry Pi 3.</span></span> <span data-ttu-id="c4da1-228">Om databasen finns på en annan plats, justera sökvägarna i enlighet med detta.</span><span class="sxs-lookup"><span data-stu-id="c4da1-228">If the repository is elsewhere, adjust the paths accordingly.</span></span>

#### <a name="logger-configuration"></a><span data-ttu-id="c4da1-229">Loggaren konfiguration</span><span class="sxs-lookup"><span data-stu-id="c4da1-229">Logger configuration</span></span>

<span data-ttu-id="c4da1-230">Under förutsättning att gateway-databasen finns i den **/home/pi/iot-edge /** mapp, konfigurera modulen loggaren på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="c4da1-230">Assuming the gateway repository is located in the **/home/pi/iot-edge/** folder, configure the logger module as follows:</span></span>

```json
{
  "name": "Logger",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path" : "build/modules/logger/liblogger.so"
    }
  },
  "args":
  {
    "filename": "<</path/to/log-file.log>>"
  }
}
```

#### <a name="ble-module-configuration"></a><span data-ttu-id="c4da1-231">TIVERA Modulkonfiguration</span><span class="sxs-lookup"><span data-stu-id="c4da1-231">BLE module configuration</span></span>

<span data-ttu-id="c4da1-232">Exempelkonfiguration för enheten TIVERA förutsätter en Texas instrument SensorTag-enhet.</span><span class="sxs-lookup"><span data-stu-id="c4da1-232">The sample configuration for the BLE device assumes a Texas Instruments SensorTag device.</span></span> <span data-ttu-id="c4da1-233">Alla standard Bell-enheter som kan fungera som en kringutrustning GATT bör fungera, men du kan behöva uppdatera GATT karakteristiken-ID: N och data.</span><span class="sxs-lookup"><span data-stu-id="c4da1-233">Any standard BLE device that can operate as a GATT peripheral should work but you may need to update the GATT characteristic IDs and data.</span></span> <span data-ttu-id="c4da1-234">Lägg till MAC-adressen för SensorTag-enheter:</span><span class="sxs-lookup"><span data-stu-id="c4da1-234">Add the MAC address of your SensorTag device:</span></span>

```json
{
  "name": "SensorTag",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/ble/libble.so"
    }
  },
  "args": {
    "controller_index": 0,
    "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
    "instructions": [
      {
        "type": "read_once",
        "characteristic_uuid": "00002A24-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A25-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A26-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A27-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A28-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A29-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "write_at_init",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AQ=="
      },
      {
        "type": "read_periodic",
        "characteristic_uuid": "F000AA01-0451-4000-B000-000000000000",
        "interval_in_ms": 1000
      },
      {
        "type": "write_at_exit",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AA=="
      }
    ]
  }
}
```

<span data-ttu-id="c4da1-235">Om du inte använder en SensorTag-enhet, granska dokumentationen för enheten tabell för att avgöra om du behöver uppdatera GATT egenskaps-ID: N och datavärden.</span><span class="sxs-lookup"><span data-stu-id="c4da1-235">If you are not using a SensorTag device, review the documentation for your BLE device to determine whether you need to update the GATT characteristic IDs and data values.</span></span>

#### <a name="iot-hub-module"></a><span data-ttu-id="c4da1-236">Modul för IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="c4da1-236">IoT Hub module</span></span>

<span data-ttu-id="c4da1-237">Lägg till namnet på din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c4da1-237">Add the name of your IoT Hub.</span></span> <span data-ttu-id="c4da1-238">Suffixvärdet för är vanligtvis **azure devices.net**:</span><span class="sxs-lookup"><span data-stu-id="c4da1-238">The suffix value is typically **azure-devices.net**:</span></span>

```json
{
  "name": "IoTHub",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/iothub/libiothub.so"
    }
  },
  "args": {
    "IoTHubName": "<<Azure IoT Hub Name>>",
    "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
    "Transport" : "amqp"
  }
}
```

#### <a name="identity-mapping-module-configuration"></a><span data-ttu-id="c4da1-239">Identitet mappning konfiguration</span><span class="sxs-lookup"><span data-stu-id="c4da1-239">Identity mapping module configuration</span></span>

<span data-ttu-id="c4da1-240">Lägg till MAC-adressen till enheten SensorTag och enhets-ID och nyckeln för den **SensorTag_01** enhet som du har lagt till din IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="c4da1-240">Add the MAC address of your SensorTag device and the device ID and key of the **SensorTag_01** device you added to your IoT Hub:</span></span>

```json
{
  "name": "mapping",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/identitymap/libidentity_map.so"
    }
  },
  "args": [
    {
      "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
      "deviceId": "<<Azure IoT Hub Device ID>>",
      "deviceKey": "<<Azure IoT Hub Device Key>>"
    }
  ]
}
```

#### <a name="ble-printer-module-configuration"></a><span data-ttu-id="c4da1-241">Modulkonfiguration TIVERA skrivare</span><span class="sxs-lookup"><span data-stu-id="c4da1-241">BLE Printer module configuration</span></span>

```json
{
  "name": "BLE Printer",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/samples/ble_gateway/ble_printer/libble_printer.so"
    }
  },
  "args": null
}
```

#### <a name="blec2d-module-configuration"></a><span data-ttu-id="c4da1-242">BLEC2D konfiguration</span><span class="sxs-lookup"><span data-stu-id="c4da1-242">BLEC2D Module Configuration</span></span>

```json
{
  "name": "BLEC2D",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/ble/libble_c2d.so"
    }
  },
  "args": null
}
```

#### <a name="routing-configuration"></a><span data-ttu-id="c4da1-243">Routningskonfiguration</span><span class="sxs-lookup"><span data-stu-id="c4da1-243">Routing Configuration</span></span>

<span data-ttu-id="c4da1-244">Följande konfiguration ser följande routning mellan IoT kant moduler:</span><span class="sxs-lookup"><span data-stu-id="c4da1-244">The following configuration ensures the following routing between IoT Edge modules:</span></span>

* <span data-ttu-id="c4da1-245">Den **loggaren** modulen tar emot och loggar alla meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c4da1-245">The **Logger** module receives and logs all messages.</span></span>
* <span data-ttu-id="c4da1-246">Den **SensorTag** modulen skickar meddelanden till både den **mappning** och **TIVERA skrivare** moduler.</span><span class="sxs-lookup"><span data-stu-id="c4da1-246">The **SensorTag** module sends messages to both the **mapping** and **BLE Printer** modules.</span></span>
* <span data-ttu-id="c4da1-247">Den **mappning** modulen skickar meddelanden till den **IoTHub** modulen skickas till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c4da1-247">The **mapping** module sends messages to the **IoTHub** module to be sent up to your IoT Hub.</span></span>
* <span data-ttu-id="c4da1-248">Den **IoTHub** modulen skickar tillbaka till den **mappning** modul.</span><span class="sxs-lookup"><span data-stu-id="c4da1-248">The **IoTHub** module sends messages back to the **mapping** module.</span></span>
* <span data-ttu-id="c4da1-249">Den **mappning** modulen skickar meddelanden till den **BLEC2D** modul.</span><span class="sxs-lookup"><span data-stu-id="c4da1-249">The **mapping** module sends messages to the **BLEC2D** module.</span></span>
* <span data-ttu-id="c4da1-250">Den **BLEC2D** modulen skickar tillbaka till den **Sensor Tag** modul.</span><span class="sxs-lookup"><span data-stu-id="c4da1-250">The **BLEC2D** module sends messages back to the **Sensor Tag** module.</span></span>

```json
"links" : [
    {"source" : "*", "sink" : "Logger" },
    {"source" : "SensorTag", "sink" : "mapping" },
    {"source" : "SensorTag", "sink" : "BLE Printer" },
    {"source" : "mapping", "sink" : "IoTHub" },
    {"source" : "IoTHub", "sink" : "mapping" },
    {"source" : "mapping", "sink" : "BLEC2D" },
    {"source" : "BLEC2D", "sink" : "SensorTag"}
 ]
```

<span data-ttu-id="c4da1-251">Om du vill köra exemplet skickar du sökvägen till JSON-konfigurationsfil som en parameter för den **tivera\_gateway** binär.</span><span class="sxs-lookup"><span data-stu-id="c4da1-251">To run the sample, pass the path to the JSON configuration file as a parameter to the **ble\_gateway** binary.</span></span> <span data-ttu-id="c4da1-252">Följande kommando förutsätter att du använder den **gateway_sample.json** konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="c4da1-252">The following command assumes you are using the **gateway_sample.json** configuration file.</span></span> <span data-ttu-id="c4da1-253">Kör kommandot från den **iot-edge** mapp på hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="c4da1-253">Execute this command from the **iot-edge** folder on the Raspberry Pi:</span></span>

```sh
./build/samples/ble_gateway/ble_gateway ./samples/ble_gateway/src/gateway_sample.json
```

<span data-ttu-id="c4da1-254">Du kan behöva trycka på knappen små på SensorTag-enhet för att göra den synlig innan du kör exemplet.</span><span class="sxs-lookup"><span data-stu-id="c4da1-254">You may need to press the small button on the SensorTag device to make it discoverable before you run the sample.</span></span>

<span data-ttu-id="c4da1-255">När du kör exemplet, kan du använda den [enheten explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) eller [iothub explorer](https://github.com/Azure/iothub-explorer) verktyg för att övervaka de meddelanden som IoT gräns-gatewayen vidarebefordrar från SensorTag-enheter.</span><span class="sxs-lookup"><span data-stu-id="c4da1-255">When you run the sample, you can use the [device explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) or the [iothub-explorer](https://github.com/Azure/iothub-explorer) tool to monitor the messages the IoT Edge gateway forwards from the SensorTag device.</span></span> <span data-ttu-id="c4da1-256">Till exempel kan Utforskaren iothub du övervaka meddelanden från enhet till moln med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c4da1-256">For example, using iothub-explorer you can monitor device-to-cloud messages using the following command:</span></span>

```sh
iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
```

## <a name="send-cloud-to-device-messages"></a><span data-ttu-id="c4da1-257">Skicka meddelanden från moln till enhet</span><span class="sxs-lookup"><span data-stu-id="c4da1-257">Send cloud-to-device messages</span></span>

<span data-ttu-id="c4da1-258">Modulen TIVERA stöder också skicka kommandon från IoT-hubb till enheten.</span><span class="sxs-lookup"><span data-stu-id="c4da1-258">The BLE module also supports sending commands from IoT Hub to the device.</span></span> <span data-ttu-id="c4da1-259">Du kan använda den [enheten explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) eller [iothub explorer](https://github.com/Azure/iothub-explorer) verktyg för att skicka JSON-meddelanden som gateway-modulen TIVERA vidarebefordrar in Bell-enheter.</span><span class="sxs-lookup"><span data-stu-id="c4da1-259">You can use the [device explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) or the [iothub-explorer](https://github.com/Azure/iothub-explorer) tool to send JSON messages that the BLE gateway module forwards on to the BLE device.</span></span>

<span data-ttu-id="c4da1-260">Om du använder Texas instrument SensorTag-enheter kan aktivera du den röda Indikator, grön Indikator eller Summer genom att skicka kommandon från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c4da1-260">If you are using the Texas Instruments SensorTag device, you can turn on the red LED, green LED, or buzzer by sending commands from IoT Hub.</span></span> <span data-ttu-id="c4da1-261">Innan du kan skicka kommandon från IoT-hubb, måste du först skicka följande JSON meddelanden i ordning.</span><span class="sxs-lookup"><span data-stu-id="c4da1-261">Before you send commands from IoT Hub, first send the following two JSON messages in order.</span></span> <span data-ttu-id="c4da1-262">Du kan sedan skicka något av kommandona för att aktivera ljus eller Summer.</span><span class="sxs-lookup"><span data-stu-id="c4da1-262">Then you can send any of the commands to turn on the lights or buzzer.</span></span>

1. <span data-ttu-id="c4da1-263">Återställa alla indikatorer och Summer (inaktivera dem):</span><span class="sxs-lookup"><span data-stu-id="c4da1-263">Reset all LEDs and the buzzer (turn them off):</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

1. <span data-ttu-id="c4da1-264">Konfigurera i/o som 'fjärr':</span><span class="sxs-lookup"><span data-stu-id="c4da1-264">Configure I/O as 'remote':</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

<span data-ttu-id="c4da1-265">Nu kan du skicka något av följande kommandon för att aktivera ljus eller Summer på SensorTag-enheter:</span><span class="sxs-lookup"><span data-stu-id="c4da1-265">Now you can send any of the following commands to turn on the lights or buzzer on the SensorTag device:</span></span>

* <span data-ttu-id="c4da1-266">Aktivera röd Indikator:</span><span class="sxs-lookup"><span data-stu-id="c4da1-266">Turn on the red LED:</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

* <span data-ttu-id="c4da1-267">Aktivera grön Indikator:</span><span class="sxs-lookup"><span data-stu-id="c4da1-267">Turn on the green LED:</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

* <span data-ttu-id="c4da1-268">Aktivera Summer:</span><span class="sxs-lookup"><span data-stu-id="c4da1-268">Turn on the buzzer:</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

## <a name="next-steps"></a><span data-ttu-id="c4da1-269">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c4da1-269">Next Steps</span></span>

<span data-ttu-id="c4da1-270">Om du vill få större kunskap om IoT kant och experimentera med vissa kodexempel finns följande kurser för utvecklare och resurser:</span><span class="sxs-lookup"><span data-stu-id="c4da1-270">If you want to gain a more advanced understanding of IoT Edge and experiment with some code examples, visit the following developer tutorials and resources:</span></span>

* <span data-ttu-id="c4da1-271">[Azure IoT kant][lnk-sdk]</span><span class="sxs-lookup"><span data-stu-id="c4da1-271">[Azure IoT Edge][lnk-sdk]</span></span>

<span data-ttu-id="c4da1-272">Om du vill utforska ytterligare funktionerna i IoT-hubb, se:</span><span class="sxs-lookup"><span data-stu-id="c4da1-272">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="c4da1-273">[Utvecklarhandbok för IoT-hubb][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="c4da1-273">[IoT Hub developer guide][lnk-devguide]</span></span>

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/iot-edge/tree/master/samples/ble_gateway
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-sdk]: https://github.com/Azure/iot-edge/
[lnk-noobs]: https://www.raspberrypi.org/documentation/installation/noobs.md
[lnk-raspbian]: https://www.raspberrypi.org/downloads/raspbian/
[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
