---
title: aaaUse en fysisk enhet med Azure IoT kant | Microsoft Docs
description: "Hur toouse en Texas instrument SensorTag enhet toosend data tooan IoT-hubb via en IoT-Edge gateway körs på en hallon Pi 3-enhet. hello gateway skapas med Azure IoT kant."
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
ms.openlocfilehash: a2385accdbd99012ad094232653ee47d4e5c7839
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-iot-edge-on-a-raspberry-pi-tooforward-device-to-cloud-messages-tooiot-hub"></a><span data-ttu-id="7ca65-104">Använda Azure IoT kanten på en hallon Pi tooforward meddelanden från enhet till moln tooIoT Hub</span><span class="sxs-lookup"><span data-stu-id="7ca65-104">Use Azure IoT Edge on a Raspberry Pi tooforward device-to-cloud messages tooIoT Hub</span></span>

<span data-ttu-id="7ca65-105">Den här genomgången av hello [Bluetooth låg energiförbrukning exempel] [ lnk-ble-samplecode] visas hur toouse [Azure IoT kant] [ lnk-sdk] till:</span><span class="sxs-lookup"><span data-stu-id="7ca65-105">This walkthrough of hello [Bluetooth low energy sample][lnk-ble-samplecode] shows you how toouse [Azure IoT Edge][lnk-sdk] to:</span></span>

* <span data-ttu-id="7ca65-106">Vidarebefordra enhet till moln telemetri tooIoT hubb från en fysisk enhet.</span><span class="sxs-lookup"><span data-stu-id="7ca65-106">Forward device-to-cloud telemetry tooIoT Hub from a physical device.</span></span>
* <span data-ttu-id="7ca65-107">Väg kommandon från IoT-hubb tooa fysisk enhet.</span><span class="sxs-lookup"><span data-stu-id="7ca65-107">Route commands from IoT Hub tooa physical device.</span></span>

<span data-ttu-id="7ca65-108">Den här genomgången omfattar:</span><span class="sxs-lookup"><span data-stu-id="7ca65-108">This walkthrough covers:</span></span>

* <span data-ttu-id="7ca65-109">**Arkitektur för**: viktig arkitektur information om hello Bluetooth låg energiförbrukning exempel.</span><span class="sxs-lookup"><span data-stu-id="7ca65-109">**Architecture**: important architectural information about hello Bluetooth low energy sample.</span></span>
* <span data-ttu-id="7ca65-110">**Skapa och köra**: hello steg krävs toobuild och kör hello exempel.</span><span class="sxs-lookup"><span data-stu-id="7ca65-110">**Build and run**: hello steps required toobuild and run hello sample.</span></span>

## <a name="architecture"></a><span data-ttu-id="7ca65-111">Arkitektur</span><span class="sxs-lookup"><span data-stu-id="7ca65-111">Architecture</span></span>

<span data-ttu-id="7ca65-112">hello genomgången visar hur toobuild och kör en IoT-gräns-gatewayen på en hallon Pi 3 som kör Raspbian Linux.</span><span class="sxs-lookup"><span data-stu-id="7ca65-112">hello walkthrough shows you how toobuild and run an IoT Edge gateway on a Raspberry Pi 3 that runs Raspbian Linux.</span></span> <span data-ttu-id="7ca65-113">hello gateway skapas med IoT kant.</span><span class="sxs-lookup"><span data-stu-id="7ca65-113">hello gateway is built using IoT Edge.</span></span> <span data-ttu-id="7ca65-114">hello används en Texas instrument SensorTag Bluetooth låg energiförbrukning (TIVERA) enheten toocollect temperatur data.</span><span class="sxs-lookup"><span data-stu-id="7ca65-114">hello sample uses a Texas Instruments SensorTag Bluetooth Low Energy (BLE) device toocollect temperature data.</span></span>

<span data-ttu-id="7ca65-115">När du kör hello IoT gräns-gatewayen den:</span><span class="sxs-lookup"><span data-stu-id="7ca65-115">When you run hello IoT Edge gateway it:</span></span>

* <span data-ttu-id="7ca65-116">Ansluter tooa SensorTag-enhet med hello Bluetooth låg energiförbrukning (a)-protokollet.</span><span class="sxs-lookup"><span data-stu-id="7ca65-116">Connects tooa SensorTag device using hello Bluetooth Low Energy (BLE) protocol.</span></span>
* <span data-ttu-id="7ca65-117">Ansluter tooIoT hubb med hello HTTP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="7ca65-117">Connects tooIoT Hub using hello HTTP protocol.</span></span>
* <span data-ttu-id="7ca65-118">Vidarebefordrar telemetri från hello SensorTag enhet tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="7ca65-118">Forwards telemetry from hello SensorTag device tooIoT Hub.</span></span>
* <span data-ttu-id="7ca65-119">Dirigerar kommandon från IoT-hubb toohello SensorTag-enheter.</span><span class="sxs-lookup"><span data-stu-id="7ca65-119">Routes commands from IoT Hub toohello SensorTag device.</span></span>

<span data-ttu-id="7ca65-120">hello gateway innehåller hello följande IoT kant moduler:</span><span class="sxs-lookup"><span data-stu-id="7ca65-120">hello gateway contains hello following IoT Edge modules:</span></span>

* <span data-ttu-id="7ca65-121">En *TIVERA modulen* som kontakt med en tabell tooreceive temperatur enhetsdata från hello enheten och skicka kommandon toohello enhet.</span><span class="sxs-lookup"><span data-stu-id="7ca65-121">A *BLE module* that interfaces with a BLE device tooreceive temperature data from hello device and send commands toohello device.</span></span>
* <span data-ttu-id="7ca65-122">En *TIVERA molnet toodevice modulen* som omvandlar JSON-meddelanden som skickats från IoT-hubb i TIVERA instruktioner för hello *TIVERA modulen*.</span><span class="sxs-lookup"><span data-stu-id="7ca65-122">A *BLE cloud toodevice module* that translates JSON messages sent from IoT Hub into BLE instructions for hello *BLE module*.</span></span>
* <span data-ttu-id="7ca65-123">En *loggaren modulen* som loggar alla gateway meddelanden tooa lokal fil.</span><span class="sxs-lookup"><span data-stu-id="7ca65-123">A *logger module* that logs all gateway messages tooa local file.</span></span>
* <span data-ttu-id="7ca65-124">En *identitet mappningsmodul* som omvandlar mellan TIVERA enhetens MAC-adresser och identiteter för Azure IoT Hub-enhet.</span><span class="sxs-lookup"><span data-stu-id="7ca65-124">An *identity mapping module* that translates between BLE device MAC addresses and Azure IoT Hub device identities.</span></span>
* <span data-ttu-id="7ca65-125">En *IoT-hubb modulen* som överför telemetri data tooan IoT-hubb och tar emot enhetskommandon från en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="7ca65-125">An *IoT Hub module* that uploads telemetry data tooan IoT hub and receives device commands from an IoT hub.</span></span>
* <span data-ttu-id="7ca65-126">En *TIVERA skrivare modulen* som tolkar telemetri från hello TIVERA enhet och skriver ut formaterade data toohello konsolen tooenable felsökning.</span><span class="sxs-lookup"><span data-stu-id="7ca65-126">A *BLE printer module* that interprets telemetry from hello BLE device and prints formatted data toohello console tooenable troubleshooting and debugging.</span></span>

### <a name="how-data-flows-through-hello-gateway"></a><span data-ttu-id="7ca65-127">Hur data flödar via hello gateway</span><span class="sxs-lookup"><span data-stu-id="7ca65-127">How data flows through hello gateway</span></span>

<span data-ttu-id="7ca65-128">hello följande blockdiagram visar hello telemetri överför data flödet pipeline:</span><span class="sxs-lookup"><span data-stu-id="7ca65-128">hello following block diagram illustrates hello telemetry upload data flow pipeline:</span></span>

![Telemetri överför gateway pipeline](media/iot-hub-iot-edge-physical-device/gateway_ble_upload_data_flow.png)

<span data-ttu-id="7ca65-130">hello stegen som ett objekt av telemetri tar reser från en tabell enheten tooIoT navet är:</span><span class="sxs-lookup"><span data-stu-id="7ca65-130">hello steps that an item of telemetry takes traveling from a BLE device tooIoT Hub are:</span></span>

1. <span data-ttu-id="7ca65-131">hello TIVERA enheten genererar ett exempel på en temperatur och skickar det via Bluetooth toohello TIVERA modul i hello gateway.</span><span class="sxs-lookup"><span data-stu-id="7ca65-131">hello BLE device generates a temperature sample and sends it over Bluetooth toohello BLE module in hello gateway.</span></span>
1. <span data-ttu-id="7ca65-132">hello TIVERA modulen tar emot hello exempel och publicerar den toohello broker tillsammans med hello MAC-adressen för hello enhet.</span><span class="sxs-lookup"><span data-stu-id="7ca65-132">hello BLE module receives hello sample and publishes it toohello broker along with hello MAC address of hello device.</span></span>
1. <span data-ttu-id="7ca65-133">hello identitet mappningsmodul hämtar det här meddelandet och använder en intern tabell tootranslate hello hello enhetens MAC-adress till en IoT-hubb enhetens identitet.</span><span class="sxs-lookup"><span data-stu-id="7ca65-133">hello identity mapping module picks up this message and uses an internal table tootranslate hello MAC address of hello device into an IoT Hub device identity.</span></span> <span data-ttu-id="7ca65-134">En IoT-hubb enhetsidentitet består av en enhets-ID och nyckeln för enheten.</span><span class="sxs-lookup"><span data-stu-id="7ca65-134">An IoT Hub device identity consists of a device ID and device key.</span></span>
1. <span data-ttu-id="7ca65-135">hello identitet mappningsmodul publicerar ett nytt meddelande som innehåller hello temperatur exempeldata, hello MAC-adressen för hello enhet, hello enhets-ID och nyckeln för hello-enheten.</span><span class="sxs-lookup"><span data-stu-id="7ca65-135">hello identity mapping module publishes a new message that contains hello temperature sample data, hello MAC address of hello device, hello device ID, and hello device key.</span></span>
1. <span data-ttu-id="7ca65-136">Hej IoT-hubb modulen får ett nytt meddelande (som genereras av hello identitet mappningsmodul) och publicerar den tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="7ca65-136">hello IoT Hub module receives this new message (generated by hello identity mapping module) and publishes it tooIoT Hub.</span></span>
1. <span data-ttu-id="7ca65-137">hello loggaren modulen loggar alla meddelanden från hello broker tooa lokal fil.</span><span class="sxs-lookup"><span data-stu-id="7ca65-137">hello logger module logs all messages from hello broker tooa local file.</span></span>

<span data-ttu-id="7ca65-138">hello följande blockdiagram visar hello enheten kommandot data flödet pipeline:</span><span class="sxs-lookup"><span data-stu-id="7ca65-138">hello following block diagram illustrates hello device command data flow pipeline:</span></span>

![Enheten kommandot gateway pipeline](media/iot-hub-iot-edge-physical-device/gateway_ble_command_data_flow.png)

1. <span data-ttu-id="7ca65-140">Hej IoT-hubb modulen avsöker med jämna mellanrum hello IoT-hubb för nya kommandomeddelanden.</span><span class="sxs-lookup"><span data-stu-id="7ca65-140">hello IoT Hub module periodically polls hello IoT hub for new command messages.</span></span>
1. <span data-ttu-id="7ca65-141">När hello IoT-hubb modulen tar emot ett nytt kommando-meddelande, publicerar den den toohello broker.</span><span class="sxs-lookup"><span data-stu-id="7ca65-141">When hello IoT Hub module receives a new command message, it publishes it toohello broker.</span></span>
1. <span data-ttu-id="7ca65-142">hello identitet mappningsmodul hämtar kommandot hälsningsmeddelande och använder en intern tabell tootranslate hello IoT-hubb enheten ID tooa enhetens MAC-adress.</span><span class="sxs-lookup"><span data-stu-id="7ca65-142">hello identity mapping module picks up hello command message and uses an internal table tootranslate hello IoT Hub device ID tooa device MAC address.</span></span> <span data-ttu-id="7ca65-143">Den publicerar sedan ett nytt meddelande som innehåller hello MAC-adressen för hello målenhet i hello egenskaper karta över hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="7ca65-143">It then publishes a new message that includes hello MAC address of hello target device in hello properties map of hello message.</span></span>
1. <span data-ttu-id="7ca65-144">hello TIVERA moln till enhet modul hämtar det här meddelandet och översätter till hello rätt Bell-instruktion för hello TIVERA modulen.</span><span class="sxs-lookup"><span data-stu-id="7ca65-144">hello BLE Cloud-to-Device module picks up this message and translates it into hello proper BLE instruction for hello BLE module.</span></span> <span data-ttu-id="7ca65-145">Den publicerar sedan ett nytt meddelande.</span><span class="sxs-lookup"><span data-stu-id="7ca65-145">It then publishes a new message.</span></span>
1. <span data-ttu-id="7ca65-146">hello TIVERA modul hämtar det här meddelandet och utför hello i/o-instruktion genom att kommunicera med hello TIVERA enhet.</span><span class="sxs-lookup"><span data-stu-id="7ca65-146">hello BLE module picks up this message and executes hello I/O instruction by communicating with hello BLE device.</span></span>
1. <span data-ttu-id="7ca65-147">hello loggaren modulen loggar alla meddelanden från hello broker tooa fil.</span><span class="sxs-lookup"><span data-stu-id="7ca65-147">hello logger module logs all messages from hello broker tooa disk file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7ca65-148">Krav</span><span class="sxs-lookup"><span data-stu-id="7ca65-148">Prerequisites</span></span>

<span data-ttu-id="7ca65-149">toocomplete den här självstudiekursen kommer du behöver en aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7ca65-149">toocomplete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="7ca65-150">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="7ca65-150">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="7ca65-151">Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="7ca65-151">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

<span data-ttu-id="7ca65-152">Du måste SSH-klienten på den stationära datorn tooenable du tooremotely access hello-kommando rad på hello hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="7ca65-152">You need SSH client on your desktop machine tooenable you tooremotely access hello command line on hello Raspberry Pi.</span></span>

- <span data-ttu-id="7ca65-153">Windows innehåller inte en SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="7ca65-153">Windows does not include an SSH client.</span></span> <span data-ttu-id="7ca65-154">Vi rekommenderar att du använder [PuTTY](http://www.putty.org/).</span><span class="sxs-lookup"><span data-stu-id="7ca65-154">We recommend using [PuTTY](http://www.putty.org/).</span></span>
- <span data-ttu-id="7ca65-155">De flesta distributioner för Linux och Mac OS inkluderar hello SSH-kommandoradsverktyget.</span><span class="sxs-lookup"><span data-stu-id="7ca65-155">Most Linux distributions and Mac OS include hello command-line SSH utility.</span></span> <span data-ttu-id="7ca65-156">Mer information finns i [SSH med Linux- eller Mac OS x](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span><span class="sxs-lookup"><span data-stu-id="7ca65-156">For more information, see [SSH Using Linux or Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span></span>

## <a name="prepare-your-hardware"></a><span data-ttu-id="7ca65-157">Förbered maskinvaran</span><span class="sxs-lookup"><span data-stu-id="7ca65-157">Prepare your hardware</span></span>

<span data-ttu-id="7ca65-158">Den här kursen förutsätter att du använder en [Texas instrument SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) enheten ansluten tooa hallon Pi 3 kör Raspbian.</span><span class="sxs-lookup"><span data-stu-id="7ca65-158">This tutorial assumes you are using a [Texas Instruments SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) device connected tooa Raspberry Pi 3 running Raspbian.</span></span>

### <a name="install-raspbian"></a><span data-ttu-id="7ca65-159">Installera Raspbian</span><span class="sxs-lookup"><span data-stu-id="7ca65-159">Install Raspbian</span></span>

<span data-ttu-id="7ca65-160">Du kan använda något av följande alternativ tooinstall Raspbian på enheten hallon Pi 3 hello.</span><span class="sxs-lookup"><span data-stu-id="7ca65-160">You can use either of hello following options tooinstall Raspbian on your Raspberry Pi 3 device.</span></span>

* <span data-ttu-id="7ca65-161">tooinstall hello senaste versionen av Raspbian, Använd hello [NOOBS] [ lnk-noobs] grafiskt användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="7ca65-161">tooinstall hello latest version of Raspbian, use hello [NOOBS][lnk-noobs] graphical user interface.</span></span>
* <span data-ttu-id="7ca65-162">Manuellt [hämta] [ lnk-raspbian] och skriva hello senaste bild av hello Raspbian operativsystemet tooan SD-kort.</span><span class="sxs-lookup"><span data-stu-id="7ca65-162">Manually [download][lnk-raspbian] and write hello latest image of hello Raspbian operating system tooan SD card.</span></span>

### <a name="sign-in-and-access-hello-terminal"></a><span data-ttu-id="7ca65-163">Logga in och komma åt hello terminal</span><span class="sxs-lookup"><span data-stu-id="7ca65-163">Sign in and access hello terminal</span></span>

<span data-ttu-id="7ca65-164">Du har två alternativ tooaccess en miljö med terminal på din hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="7ca65-164">You have two options tooaccess a terminal environment on your Raspberry Pi:</span></span>

* <span data-ttu-id="7ca65-165">Om du har ett tangentbord och övervaka anslutna tooyour hallon Pi kan du använda hello Raspbian GUI tooaccess ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="7ca65-165">If you have a keyboard and monitor connected tooyour Raspberry Pi, you can use hello Raspbian GUI tooaccess a terminal window.</span></span>

* <span data-ttu-id="7ca65-166">Åtkomst hello kommandoraden på din hallon Pi använder SSH från den stationära datorn.</span><span class="sxs-lookup"><span data-stu-id="7ca65-166">Access hello command line on your Raspberry Pi using SSH from your desktop machine.</span></span>

#### <a name="use-a-terminal-window-in-hello-gui"></a><span data-ttu-id="7ca65-167">Använd ett terminalfönster i hello GUI</span><span class="sxs-lookup"><span data-stu-id="7ca65-167">Use a terminal Window in hello GUI</span></span>

<span data-ttu-id="7ca65-168">Hej standardautentiseringsuppgifterna för Raspbian är användarnamn **pi** och lösenord **hallon**.</span><span class="sxs-lookup"><span data-stu-id="7ca65-168">hello default credentials for Raspbian are username **pi** and password **raspberry**.</span></span> <span data-ttu-id="7ca65-169">I Aktivitetsfältet i hello GUI hello kan du starta hello **Terminal** verktyget med hello-ikonen som ser ut som en bildskärm.</span><span class="sxs-lookup"><span data-stu-id="7ca65-169">In hello task bar in hello GUI, you can launch hello **Terminal** utility using hello icon that looks like a monitor.</span></span>

#### <a name="sign-in-with-ssh"></a><span data-ttu-id="7ca65-170">Logga in med SSH</span><span class="sxs-lookup"><span data-stu-id="7ca65-170">Sign in with SSH</span></span>

<span data-ttu-id="7ca65-171">Du kan använda SSH för kommandoradsåtkomst tooyour hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="7ca65-171">You can use SSH for command-line access tooyour Raspberry Pi.</span></span> <span data-ttu-id="7ca65-172">hello artikel [SSH (Secure Shell)] [ lnk-pi-ssh] beskriver hur tooconfigure SSH på din hallon Pi, och hur tooconnect från [Windows] [ lnk-ssh-windows] eller [Linux och Mac OS][lnk-ssh-linux].</span><span class="sxs-lookup"><span data-stu-id="7ca65-172">hello article [SSH (Secure Shell)][lnk-pi-ssh] describes how tooconfigure SSH on your Raspberry Pi, and how tooconnect from [Windows][lnk-ssh-windows] or [Linux & Mac OS][lnk-ssh-linux].</span></span>

<span data-ttu-id="7ca65-173">Logga in med användarnamn **pi** och lösenord **hallon**.</span><span class="sxs-lookup"><span data-stu-id="7ca65-173">Sign in with username **pi** and password **raspberry**.</span></span>

### <a name="install-bluez-537"></a><span data-ttu-id="7ca65-174">Installera BlueZ 5.37</span><span class="sxs-lookup"><span data-stu-id="7ca65-174">Install BlueZ 5.37</span></span>

<span data-ttu-id="7ca65-175">hello TIVERA moduler prata toohello Bluetooth maskinvaran via hello BlueZ stacken.</span><span class="sxs-lookup"><span data-stu-id="7ca65-175">hello BLE modules talk toohello Bluetooth hardware via hello BlueZ stack.</span></span> <span data-ttu-id="7ca65-176">Du behöver version 5.37 av BlueZ för hello moduler toowork korrekt.</span><span class="sxs-lookup"><span data-stu-id="7ca65-176">You need version 5.37 of BlueZ for hello modules toowork correctly.</span></span> <span data-ttu-id="7ca65-177">Dessa instruktioner Kontrollera hello rätt version av BlueZ är installerat.</span><span class="sxs-lookup"><span data-stu-id="7ca65-177">These instructions make sure hello correct version of BlueZ is installed.</span></span>

1. <span data-ttu-id="7ca65-178">Stoppa hello aktuella Bluetooth-daemon:</span><span class="sxs-lookup"><span data-stu-id="7ca65-178">Stop hello current bluetooth daemon:</span></span>

    ```sh
    sudo systemctl stop bluetooth
    ```

1. <span data-ttu-id="7ca65-179">Installera hello BlueZ beroenden:</span><span class="sxs-lookup"><span data-stu-id="7ca65-179">Install hello BlueZ dependencies:</span></span>

    ```sh
    sudo apt-get update
    sudo apt-get install bluetooth bluez-tools build-essential autoconf glib2.0 libglib2.0-dev libdbus-1-dev libudev-dev libical-dev libreadline-dev
    ```

1. <span data-ttu-id="7ca65-180">Hämta hello BlueZ källkod från bluez.org:</span><span class="sxs-lookup"><span data-stu-id="7ca65-180">Download hello BlueZ source code from bluez.org:</span></span>

    ```sh
    wget http://www.kernel.org/pub/linux/bluetooth/bluez-5.37.tar.xz
    ```

1. <span data-ttu-id="7ca65-181">Packa upp hello källkoden:</span><span class="sxs-lookup"><span data-stu-id="7ca65-181">Unzip hello source code:</span></span>

    ```sh
    tar -xvf bluez-5.37.tar.xz
    ```

1. <span data-ttu-id="7ca65-182">Ändra kataloger toohello nyskapad mapp:</span><span class="sxs-lookup"><span data-stu-id="7ca65-182">Change directories toohello newly created folder:</span></span>

    ```sh
    cd bluez-5.37
    ```

1. <span data-ttu-id="7ca65-183">Konfigurera hello BlueZ kod toobe inbyggda:</span><span class="sxs-lookup"><span data-stu-id="7ca65-183">Configure hello BlueZ code toobe built:</span></span>

    ```sh
    ./configure --disable-udev --disable-systemd --enable-experimental
    ```

1. <span data-ttu-id="7ca65-184">Skapa BlueZ:</span><span class="sxs-lookup"><span data-stu-id="7ca65-184">Build BlueZ:</span></span>

    ```sh
    make
    ```

1. <span data-ttu-id="7ca65-185">Installera BlueZ när det är klart byggnads:</span><span class="sxs-lookup"><span data-stu-id="7ca65-185">Install BlueZ once it is done building:</span></span>

    ```sh
    sudo make install
    ```

1. <span data-ttu-id="7ca65-186">Ändra systemd tjänstkonfigurationen för bluetooth så att den pekar toohello nya Bluetooth-daemon i hello filen `/lib/systemd/system/bluetooth.service`.</span><span class="sxs-lookup"><span data-stu-id="7ca65-186">Change systemd service configuration for bluetooth so it points toohello new bluetooth daemon in hello file `/lib/systemd/system/bluetooth.service`.</span></span> <span data-ttu-id="7ca65-187">Ersätt hello 'ExecStart' rad med hello följande text:</span><span class="sxs-lookup"><span data-stu-id="7ca65-187">Replace hello 'ExecStart' line with hello following text:</span></span>

    ```conf
    ExecStart=/usr/local/libexec/bluetooth/bluetoothd -E
    ```

### <a name="enable-connectivity-toohello-sensortag-device-from-your-raspberry-pi-3-device"></a><span data-ttu-id="7ca65-188">Aktivera anslutningen toohello SensorTag enhet från enheten hallon Pi 3</span><span class="sxs-lookup"><span data-stu-id="7ca65-188">Enable connectivity toohello SensorTag device from your Raspberry Pi 3 device</span></span>

<span data-ttu-id="7ca65-189">Innan du kör hello exemplet måste tooverify att din hallon Pi 3 kan ansluta toohello SensorTag-enheter.</span><span class="sxs-lookup"><span data-stu-id="7ca65-189">Before running hello sample, you need tooverify that your Raspberry Pi 3 can connect toohello SensorTag device.</span></span>

1. <span data-ttu-id="7ca65-190">Se till att hello `rfkill` verktyg installeras:</span><span class="sxs-lookup"><span data-stu-id="7ca65-190">Ensure hello `rfkill` utility is installed:</span></span>

    ```sh
    sudo apt-get install rfkill
    ```

1. <span data-ttu-id="7ca65-191">Tillåt bluetooth på hello hallon Pi 3 och kontrollera att hello versionsnumret är **5.37**:</span><span class="sxs-lookup"><span data-stu-id="7ca65-191">Unblock bluetooth on hello Raspberry Pi 3 and check that hello version number is **5.37**:</span></span>

    ```sh
    sudo rfkill unblock bluetooth
    bluetoothctl --version
    ```

1. <span data-ttu-id="7ca65-192">tooenter hello interaktiva bluetooth shell starta hello Bluetooth-tjänsten och kör hello **bluetoothctl** kommando:</span><span class="sxs-lookup"><span data-stu-id="7ca65-192">tooenter hello interactive bluetooth shell, start hello bluetooth service and execute hello **bluetoothctl** command :</span></span>

    ```sh
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. <span data-ttu-id="7ca65-193">Ange hello kommando **effekt på** toopower in hello Bluetooth-styrenhet.</span><span class="sxs-lookup"><span data-stu-id="7ca65-193">Enter hello command **power on** toopower up hello bluetooth controller.</span></span> <span data-ttu-id="7ca65-194">hello kommando returnerar utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="7ca65-194">hello command returns output similar toohello following:</span></span>

    ```sh
    [NEW] Controller 98:4F:EE:04:1F:DF C3 raspberrypi [default]
    ```

1. <span data-ttu-id="7ca65-195">Hello interaktiva bluetooth Shell, ange hello kommando **genomsökning på** tooscan Bluetooth-enheter.</span><span class="sxs-lookup"><span data-stu-id="7ca65-195">In hello interactive bluetooth shell, enter hello command **scan on** tooscan for bluetooth devices.</span></span> <span data-ttu-id="7ca65-196">hello kommando returnerar utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="7ca65-196">hello command returns output similar toohello following:</span></span>

    ```sh
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

1. <span data-ttu-id="7ca65-197">Gör hello SensorTag enheten kan upptäckas genom att trycka på hello liten knapp (hello grön Indikator bör flash).</span><span class="sxs-lookup"><span data-stu-id="7ca65-197">Make hello SensorTag device discoverable by pressing hello small button (hello green LED should flash).</span></span> <span data-ttu-id="7ca65-198">hello hallon Pi 3 ska identifiera hello SensorTag enhet:</span><span class="sxs-lookup"><span data-stu-id="7ca65-198">hello Raspberry Pi 3 should discover hello SensorTag device:</span></span>

    ```sh
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```

    <span data-ttu-id="7ca65-199">I det här exemplet ser du att hello MAC-adressen för hello SensorTag enheten är **A0:E6:F8:B5:F6:00**.</span><span class="sxs-lookup"><span data-stu-id="7ca65-199">In this example, you can see that hello MAC address of hello SensorTag device is **A0:E6:F8:B5:F6:00**.</span></span>

1. <span data-ttu-id="7ca65-200">Inaktivera genomsökning genom att ange hello **genomsökning av** kommando:</span><span class="sxs-lookup"><span data-stu-id="7ca65-200">Turn off scanning by entering hello **scan off** command:</span></span>

    ```sh
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

1. <span data-ttu-id="7ca65-201">Anslut tooyour SensorTag-enhet med hjälp av dess MAC-adress genom att ange **ansluta \<MAC-adress\>**.</span><span class="sxs-lookup"><span data-stu-id="7ca65-201">Connect tooyour SensorTag device using its MAC address by entering **connect \<MAC address\>**.</span></span> <span data-ttu-id="7ca65-202">följande exempel på utdata hello förkortas för tydlighetens skull:</span><span class="sxs-lookup"><span data-stu-id="7ca65-202">hello following sample output is abbreviated for clarity:</span></span>

    ```sh
    Attempting tooconnect tooA0:E6:F8:B5:F6:00
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

    > <span data-ttu-id="7ca65-203">Du kan ange hello GATT egenskaper hello enheten igen med hello **listan attribut** kommando.</span><span class="sxs-lookup"><span data-stu-id="7ca65-203">You can list hello GATT characteristics of hello device again using hello **list-attributes** command.</span></span>

1. <span data-ttu-id="7ca65-204">Du kan nu koppla från hello-enhet med hjälp av hello **koppla från** kommandot och avsluta hello bluetooth shell med hello **avsluta** kommando:</span><span class="sxs-lookup"><span data-stu-id="7ca65-204">You can now disconnect from hello device using hello **disconnect** command and then exit from hello bluetooth shell using hello **quit** command:</span></span>

    ```sh
    Attempting toodisconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

<span data-ttu-id="7ca65-205">Nu är du redo toorun hello TIVERA IoT kant prov på din hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="7ca65-205">You're now ready toorun hello BLE IoT Edge sample on your Raspberry Pi 3.</span></span>

## <a name="run-hello-iot-edge-ble-sample"></a><span data-ttu-id="7ca65-206">Kör hello IoT kant TIVERA exempel</span><span class="sxs-lookup"><span data-stu-id="7ca65-206">Run hello IoT Edge BLE sample</span></span>

<span data-ttu-id="7ca65-207">toorun hello IoT kant TIVERA exemplet behöver du toocomplete tre uppgifter:</span><span class="sxs-lookup"><span data-stu-id="7ca65-207">toorun hello IoT Edge BLE sample, you need toocomplete three tasks:</span></span>

* <span data-ttu-id="7ca65-208">Konfigurera två exempel enheter i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="7ca65-208">Configure two sample devices in your IoT Hub.</span></span>
* <span data-ttu-id="7ca65-209">Skapa IoT kanten på enheten hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="7ca65-209">Build IoT Edge on your Raspberry Pi 3 device.</span></span>
* <span data-ttu-id="7ca65-210">Konfigurera och köra hello TIVERA exemplet på enheten hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="7ca65-210">Configure and run hello BLE sample on your Raspberry Pi 3 device.</span></span>

<span data-ttu-id="7ca65-211">När hello skrivning stöder IoT kant endast TIVERA moduler i gateways som körs på Linux.</span><span class="sxs-lookup"><span data-stu-id="7ca65-211">At hello time of writing, IoT Edge only supports BLE modules in gateways running on Linux.</span></span>

### <a name="configure-two-sample-devices-in-your-iot-hub"></a><span data-ttu-id="7ca65-212">Konfigurera två exempel enheter i din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="7ca65-212">Configure two sample devices in your IoT Hub</span></span>

* <span data-ttu-id="7ca65-213">[Skapa en IoT-hubb] [ lnk-create-hub] i din Azure-prenumeration behöver du hello namnet på din hubb toocomplete den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="7ca65-213">[Create an IoT hub][lnk-create-hub] in your Azure subscription, you need hello name of your hub toocomplete this walkthrough.</span></span> <span data-ttu-id="7ca65-214">Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="7ca65-214">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="7ca65-215">Lägg till en enhet som kallas **SensorTag_01** tooyour IoT-hubb och anteckna dess id och enheten nyckel.</span><span class="sxs-lookup"><span data-stu-id="7ca65-215">Add one device called **SensorTag_01** tooyour IoT hub and make a note of its id and device key.</span></span> <span data-ttu-id="7ca65-216">Du kan använda hello [enheten explorer eller iothub-explorer] [ lnk-explorer-tools] verktyg tooadd för den här enheten toohello IoT-hubb som du skapade i föregående steg i hello och tooretrieve dess nyckel.</span><span class="sxs-lookup"><span data-stu-id="7ca65-216">You can use hello [device explorer or iothub-explorer][lnk-explorer-tools] tools tooadd this device toohello IoT hub you created in hello previous step and tooretrieve its key.</span></span> <span data-ttu-id="7ca65-217">Du kan mappa enheten toohello SensorTag enheten när du konfigurerar hello gateway.</span><span class="sxs-lookup"><span data-stu-id="7ca65-217">You map this device toohello SensorTag device when you configure hello gateway.</span></span>

### <a name="build-azure-iot-edge-on-your-raspberry-pi-3"></a><span data-ttu-id="7ca65-218">Skapa Azure IoT kanten på din Raspberry Pi 3</span><span class="sxs-lookup"><span data-stu-id="7ca65-218">Build Azure IoT Edge on your Raspberry Pi 3</span></span>

<span data-ttu-id="7ca65-219">Installera beroenden för Azure IoT kant:</span><span class="sxs-lookup"><span data-stu-id="7ca65-219">Install dependencies for Azure IoT Edge:</span></span>

```sh
sudo apt-get install cmake uuid-dev curl libcurl4-openssl-dev libssl-dev
```

<span data-ttu-id="7ca65-220">Använd hello följande kommandon tooclone IoT kant och alla dess submodules tooyour-hemkataloger:</span><span class="sxs-lookup"><span data-stu-id="7ca65-220">Use hello following commands tooclone IoT Edge and all its submodules tooyour home directory:</span></span>

```sh
cd ~
git clone https://github.com/Azure/iot-edge.git
```

<span data-ttu-id="7ca65-221">När du har en fullständig kopia av hello IoT kant databasen på din hallon Pi 3 kan du skapa den med hjälp av följande kommando från hello-mapp som innehåller hello SDK hello:</span><span class="sxs-lookup"><span data-stu-id="7ca65-221">When you have a complete copy of hello IoT Edge repository on your Raspberry Pi 3, you can build it using hello following command from hello folder that contains hello SDK:</span></span>

```sh
cd ~/iot-edge
./tools/build.sh  --disable-native-remote-modules
```

### <a name="configure-and-run-hello-ble-sample-on-your-raspberry-pi-3"></a><span data-ttu-id="7ca65-222">Konfigurera och köra hello TIVERA prov på din hallon Pi 3</span><span class="sxs-lookup"><span data-stu-id="7ca65-222">Configure and run hello BLE sample on your Raspberry Pi 3</span></span>

<span data-ttu-id="7ca65-223">toobootstrap och kör hello exempel, måste du konfigurera varje IoT kant-modul som deltar i hello gateway.</span><span class="sxs-lookup"><span data-stu-id="7ca65-223">toobootstrap and run hello sample, you must configure each IoT Edge module that participates in hello gateway.</span></span> <span data-ttu-id="7ca65-224">Den här konfigurationen finns i en JSON-fil och du måste konfigurera alla fem deltagande IoT kant moduler.</span><span class="sxs-lookup"><span data-stu-id="7ca65-224">This configuration is provided in a JSON file and you must configure all five participating IoT Edge modules.</span></span> <span data-ttu-id="7ca65-225">Det finns ett exempel JSON-fil i hello databas kallas **gateway\_sample.json** som du kan använda som hello som startpunkt för att skapa egna konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="7ca65-225">There is a sample JSON file in hello repository called **gateway\_sample.json** that you can use as hello starting point for building your own configuration file.</span></span> <span data-ttu-id="7ca65-226">Den här filen har hello **src-exempel/ble_gateway** mapp i lokal kopia av hello IoT kant-databasen.</span><span class="sxs-lookup"><span data-stu-id="7ca65-226">This file is in hello **samples/ble_gateway/src** folder in local copy of hello IoT Edge repository.</span></span>

<span data-ttu-id="7ca65-227">hello följande avsnitt beskrivs hur tooedit den här konfigurationen för hello TIVERA exempel och förutsätter att hello IoT kant databasen i hello **/home/pi/iot-edge /** mapp på din hallon Pi 3.</span><span class="sxs-lookup"><span data-stu-id="7ca65-227">hello following sections describe how tooedit this configuration file for hello BLE sample and assume that hello IoT Edge repository is in hello **/home/pi/iot-edge/** folder on your Raspberry Pi 3.</span></span> <span data-ttu-id="7ca65-228">Om hello databasen är någon annanstans, justera hello sökvägar i enlighet med detta.</span><span class="sxs-lookup"><span data-stu-id="7ca65-228">If hello repository is elsewhere, adjust hello paths accordingly.</span></span>

#### <a name="logger-configuration"></a><span data-ttu-id="7ca65-229">Loggaren konfiguration</span><span class="sxs-lookup"><span data-stu-id="7ca65-229">Logger configuration</span></span>

<span data-ttu-id="7ca65-230">Under förutsättning att hello gateway-databasen finns i hello **/home/pi/iot-edge /** mapp, konfigurera hello loggaren modulen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="7ca65-230">Assuming hello gateway repository is located in hello **/home/pi/iot-edge/** folder, configure hello logger module as follows:</span></span>

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

#### <a name="ble-module-configuration"></a><span data-ttu-id="7ca65-231">TIVERA Modulkonfiguration</span><span class="sxs-lookup"><span data-stu-id="7ca65-231">BLE module configuration</span></span>

<span data-ttu-id="7ca65-232">hello exempelkonfiguration för hello TIVERA enhet förutsätter en Texas instrument SensorTag-enhet.</span><span class="sxs-lookup"><span data-stu-id="7ca65-232">hello sample configuration for hello BLE device assumes a Texas Instruments SensorTag device.</span></span> <span data-ttu-id="7ca65-233">Alla standard Bell-enheter som kan fungera som en kringutrustning GATT bör fungera, men du måste kanske tooupdate hello GATT egenskaps-ID: N och data.</span><span class="sxs-lookup"><span data-stu-id="7ca65-233">Any standard BLE device that can operate as a GATT peripheral should work but you may need tooupdate hello GATT characteristic IDs and data.</span></span> <span data-ttu-id="7ca65-234">Lägg till hello MAC-adressen för SensorTag-enheter:</span><span class="sxs-lookup"><span data-stu-id="7ca65-234">Add hello MAC address of your SensorTag device:</span></span>

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

<span data-ttu-id="7ca65-235">Om du inte använder en SensorTag-enhet, granska hello dokumentationen för din enhet toodetermine för tabell om du behöver tooupdate hello GATT egenskaps-ID: N och datavärden.</span><span class="sxs-lookup"><span data-stu-id="7ca65-235">If you are not using a SensorTag device, review hello documentation for your BLE device toodetermine whether you need tooupdate hello GATT characteristic IDs and data values.</span></span>

#### <a name="iot-hub-module"></a><span data-ttu-id="7ca65-236">Modul för IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="7ca65-236">IoT Hub module</span></span>

<span data-ttu-id="7ca65-237">Lägg till hello namn för din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="7ca65-237">Add hello name of your IoT Hub.</span></span> <span data-ttu-id="7ca65-238">Hej postsuffixvärdet är vanligtvis **azure devices.net**:</span><span class="sxs-lookup"><span data-stu-id="7ca65-238">hello suffix value is typically **azure-devices.net**:</span></span>

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

#### <a name="identity-mapping-module-configuration"></a><span data-ttu-id="7ca65-239">Identitet mappning konfiguration</span><span class="sxs-lookup"><span data-stu-id="7ca65-239">Identity mapping module configuration</span></span>

<span data-ttu-id="7ca65-240">Lägg till hello MAC-adressen för din SensorTag enhet och hello enhets-ID och nyckeln för hello **SensorTag_01** enhet som du har lagt till tooyour IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="7ca65-240">Add hello MAC address of your SensorTag device and hello device ID and key of hello **SensorTag_01** device you added tooyour IoT Hub:</span></span>

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

#### <a name="ble-printer-module-configuration"></a><span data-ttu-id="7ca65-241">Modulkonfiguration TIVERA skrivare</span><span class="sxs-lookup"><span data-stu-id="7ca65-241">BLE Printer module configuration</span></span>

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

#### <a name="blec2d-module-configuration"></a><span data-ttu-id="7ca65-242">BLEC2D konfiguration</span><span class="sxs-lookup"><span data-stu-id="7ca65-242">BLEC2D Module Configuration</span></span>

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

#### <a name="routing-configuration"></a><span data-ttu-id="7ca65-243">Routningskonfiguration</span><span class="sxs-lookup"><span data-stu-id="7ca65-243">Routing Configuration</span></span>

<span data-ttu-id="7ca65-244">hello följande konfigurationen garanterar hello följande routning mellan IoT kant moduler:</span><span class="sxs-lookup"><span data-stu-id="7ca65-244">hello following configuration ensures hello following routing between IoT Edge modules:</span></span>

* <span data-ttu-id="7ca65-245">Hej **loggaren** modulen tar emot och loggar alla meddelanden.</span><span class="sxs-lookup"><span data-stu-id="7ca65-245">hello **Logger** module receives and logs all messages.</span></span>
* <span data-ttu-id="7ca65-246">Hej **SensorTag** modulen skickar meddelanden tooboth hello **mappning** och **TIVERA skrivare** moduler.</span><span class="sxs-lookup"><span data-stu-id="7ca65-246">hello **SensorTag** module sends messages tooboth hello **mapping** and **BLE Printer** modules.</span></span>
* <span data-ttu-id="7ca65-247">Hej **mappning** modulen skickar meddelanden toohello **IoTHub** modulen toobe skickas upp tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="7ca65-247">hello **mapping** module sends messages toohello **IoTHub** module toobe sent up tooyour IoT Hub.</span></span>
* <span data-ttu-id="7ca65-248">Hej **IoTHub** modulen skickar tillbaka toohello **mappning** modul.</span><span class="sxs-lookup"><span data-stu-id="7ca65-248">hello **IoTHub** module sends messages back toohello **mapping** module.</span></span>
* <span data-ttu-id="7ca65-249">Hej **mappning** modulen skickar meddelanden toohello **BLEC2D** modul.</span><span class="sxs-lookup"><span data-stu-id="7ca65-249">hello **mapping** module sends messages toohello **BLEC2D** module.</span></span>
* <span data-ttu-id="7ca65-250">Hej **BLEC2D** modulen skickar tillbaka toohello **Sensor Tag** modul.</span><span class="sxs-lookup"><span data-stu-id="7ca65-250">hello **BLEC2D** module sends messages back toohello **Sensor Tag** module.</span></span>

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

<span data-ttu-id="7ca65-251">toorun hello exemplet pass hello sökvägen toohello JSON-konfigurationsfil som en parameter toohello **tivera\_gateway** binär.</span><span class="sxs-lookup"><span data-stu-id="7ca65-251">toorun hello sample, pass hello path toohello JSON configuration file as a parameter toohello **ble\_gateway** binary.</span></span> <span data-ttu-id="7ca65-252">hello följande kommando förutsätter att du använder hello **gateway_sample.json** konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="7ca65-252">hello following command assumes you are using hello **gateway_sample.json** configuration file.</span></span> <span data-ttu-id="7ca65-253">Kör kommandot från hello **iot-edge** mapp på hello hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="7ca65-253">Execute this command from hello **iot-edge** folder on hello Raspberry Pi:</span></span>

```sh
./build/samples/ble_gateway/ble_gateway ./samples/ble_gateway/src/gateway_sample.json
```

<span data-ttu-id="7ca65-254">Du kan behöva toopress hello små knappen på hello SensorTag enhet toomake den identifierbart innan du kör hello exempel.</span><span class="sxs-lookup"><span data-stu-id="7ca65-254">You may need toopress hello small button on hello SensorTag device toomake it discoverable before you run hello sample.</span></span>

<span data-ttu-id="7ca65-255">När du kör hello exemplet, kan du använda hello [enheten explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) eller hello [iothub explorer](https://github.com/Azure/iothub-explorer) verktyget toomonitor hello meddelanden hello IoT gräns-gatewayen vidarebefordrar från hello SensorTag enhet.</span><span class="sxs-lookup"><span data-stu-id="7ca65-255">When you run hello sample, you can use hello [device explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) or hello [iothub-explorer](https://github.com/Azure/iothub-explorer) tool toomonitor hello messages hello IoT Edge gateway forwards from hello SensorTag device.</span></span> <span data-ttu-id="7ca65-256">Till exempel kan Utforskaren iothub du övervaka meddelanden från enhet till moln med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7ca65-256">For example, using iothub-explorer you can monitor device-to-cloud messages using hello following command:</span></span>

```sh
iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
```

## <a name="send-cloud-to-device-messages"></a><span data-ttu-id="7ca65-257">Skicka meddelanden från moln till enhet</span><span class="sxs-lookup"><span data-stu-id="7ca65-257">Send cloud-to-device messages</span></span>

<span data-ttu-id="7ca65-258">hello TIVERA modulen stöder också skicka kommandon från IoT-hubb toohello enhet.</span><span class="sxs-lookup"><span data-stu-id="7ca65-258">hello BLE module also supports sending commands from IoT Hub toohello device.</span></span> <span data-ttu-id="7ca65-259">Du kan använda hello [enheten explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) eller hello [iothub explorer](https://github.com/Azure/iothub-explorer) verktyget toosend JSON meddelanden hello TIVERA gateway modulen vidarebefordrar på toohello TIVERA enhet.</span><span class="sxs-lookup"><span data-stu-id="7ca65-259">You can use hello [device explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) or hello [iothub-explorer](https://github.com/Azure/iothub-explorer) tool toosend JSON messages that hello BLE gateway module forwards on toohello BLE device.</span></span>

<span data-ttu-id="7ca65-260">Om du använder hello Texas instrument SensorTag-enheter kan aktivera du hello röd Indikator eller grön Indikator Summer genom att skicka kommandon från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="7ca65-260">If you are using hello Texas Instruments SensorTag device, you can turn on hello red LED, green LED, or buzzer by sending commands from IoT Hub.</span></span> <span data-ttu-id="7ca65-261">Innan du skickar kommandon från IoT-hubb först skicka hello följande två JSON-meddelanden i ordning.</span><span class="sxs-lookup"><span data-stu-id="7ca65-261">Before you send commands from IoT Hub, first send hello following two JSON messages in order.</span></span> <span data-ttu-id="7ca65-262">Du kan sedan skicka någon hello kommandon tooturn på hello ljus eller Summer.</span><span class="sxs-lookup"><span data-stu-id="7ca65-262">Then you can send any of hello commands tooturn on hello lights or buzzer.</span></span>

1. <span data-ttu-id="7ca65-263">Återställ alla indikatorer och hello Summer (inaktivera dem):</span><span class="sxs-lookup"><span data-stu-id="7ca65-263">Reset all LEDs and hello buzzer (turn them off):</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

1. <span data-ttu-id="7ca65-264">Konfigurera i/o som 'fjärr':</span><span class="sxs-lookup"><span data-stu-id="7ca65-264">Configure I/O as 'remote':</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

<span data-ttu-id="7ca65-265">Nu kan du skicka något av följande kommandon tooturn på hello ljus eller Summer på hello SensorTag enhet hello:</span><span class="sxs-lookup"><span data-stu-id="7ca65-265">Now you can send any of hello following commands tooturn on hello lights or buzzer on hello SensorTag device:</span></span>

* <span data-ttu-id="7ca65-266">Aktivera hello röd Indikator:</span><span class="sxs-lookup"><span data-stu-id="7ca65-266">Turn on hello red LED:</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

* <span data-ttu-id="7ca65-267">Aktivera hello grön Indikator:</span><span class="sxs-lookup"><span data-stu-id="7ca65-267">Turn on hello green LED:</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

* <span data-ttu-id="7ca65-268">Aktivera hello Summer:</span><span class="sxs-lookup"><span data-stu-id="7ca65-268">Turn on hello buzzer:</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

## <a name="next-steps"></a><span data-ttu-id="7ca65-269">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7ca65-269">Next Steps</span></span>

<span data-ttu-id="7ca65-270">Om du vill toogain en större kunskap om IoT kant och experimentera med vissa kodexempel finns hello följande developer självstudier och resurser:</span><span class="sxs-lookup"><span data-stu-id="7ca65-270">If you want toogain a more advanced understanding of IoT Edge and experiment with some code examples, visit hello following developer tutorials and resources:</span></span>

* <span data-ttu-id="7ca65-271">[Azure IoT kant][lnk-sdk]</span><span class="sxs-lookup"><span data-stu-id="7ca65-271">[Azure IoT Edge][lnk-sdk]</span></span>

<span data-ttu-id="7ca65-272">toofurther utforska hello funktionerna i IoT Hub, se:</span><span class="sxs-lookup"><span data-stu-id="7ca65-272">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="7ca65-273">[Utvecklarhandbok för IoT-hubb][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="7ca65-273">[IoT Hub developer guide][lnk-devguide]</span></span>

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
