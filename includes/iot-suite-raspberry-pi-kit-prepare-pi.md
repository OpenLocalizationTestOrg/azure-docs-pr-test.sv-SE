## <a name="prepare-your-raspberry-pi"></a><span data-ttu-id="df873-101">Förbered din Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="df873-101">Prepare your Raspberry Pi</span></span>

### <a name="install-raspbian"></a><span data-ttu-id="df873-102">Installera Raspbian</span><span class="sxs-lookup"><span data-stu-id="df873-102">Install Raspbian</span></span>

<span data-ttu-id="df873-103">Om detta är hello första gången du använder din hallon Pi måste tooinstall hello Raspbian operativsystem genom att använda NOOBS på hello SD-kort som ingår i hello kit.</span><span class="sxs-lookup"><span data-stu-id="df873-103">If this is hello first time you are using your Raspberry Pi, you need tooinstall hello Raspbian operating system using NOOBS on hello SD card included in hello kit.</span></span> <span data-ttu-id="df873-104">Hej [hallon Pi Software Guide] [ lnk-install-raspbian] beskriver hur tooinstall ett operativsystem på din hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="df873-104">hello [Raspberry Pi Software Guide][lnk-install-raspbian] describes how tooinstall an operating system on your Raspberry Pi.</span></span> <span data-ttu-id="df873-105">Den här självstudiekursen förutsätts att du har installerat hello Raspbian operativsystemet på din hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="df873-105">This tutorial assumes you have installed hello Raspbian operating system on your Raspberry Pi.</span></span>

> [!NOTE]
> <span data-ttu-id="df873-106">hello SD-kort som ingår i hello [Microsoft Azure IoT-startpaket för hallon Pi 3] [ lnk-starter-kits] NOOBS installerat finns redan.</span><span class="sxs-lookup"><span data-stu-id="df873-106">hello SD card included in hello [Microsoft Azure IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] already has NOOBS installed.</span></span> <span data-ttu-id="df873-107">Du kan starta hello hallon Pi från det här kortet och välj tooinstall hello Raspbian OS.</span><span class="sxs-lookup"><span data-stu-id="df873-107">You can boot hello Raspberry Pi from this card and choose tooinstall hello Raspbian OS.</span></span>

### <a name="set-up-hello-hardware"></a><span data-ttu-id="df873-108">Ställ in hello maskinvara</span><span class="sxs-lookup"><span data-stu-id="df873-108">Set up hello hardware</span></span>

<span data-ttu-id="df873-109">Den här kursen använder hello BME280 sensor som ingår i hello [Microsoft Azure IoT-startpaket för hallon Pi 3] [ lnk-starter-kits] toogenerate telemetridata.</span><span class="sxs-lookup"><span data-stu-id="df873-109">This tutorial uses hello BME280 sensor included in hello [Microsoft Azure IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] toogenerate telemetry data.</span></span> <span data-ttu-id="df873-110">Den använder en Indikator tooindicate när hello hallon Pi bearbetar ett metodanrop hello lösning instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="df873-110">It uses an LED tooindicate when hello Raspberry Pi processes a method invocation from hello solution dashboard.</span></span>

<span data-ttu-id="df873-111">hello-komponenter på hello bröd kort är:</span><span class="sxs-lookup"><span data-stu-id="df873-111">hello components on hello bread board are:</span></span>

- <span data-ttu-id="df873-112">Röd Indikator</span><span class="sxs-lookup"><span data-stu-id="df873-112">Red LED</span></span>
- <span data-ttu-id="df873-113">220 Ohm resistor (röd, red, Jansson)</span><span class="sxs-lookup"><span data-stu-id="df873-113">220-Ohm resistor (red, red, brown)</span></span>
- <span data-ttu-id="df873-114">BME280 sensor</span><span class="sxs-lookup"><span data-stu-id="df873-114">BME280 sensor</span></span>

<span data-ttu-id="df873-115">hello följande diagram visar hur tooconnect maskinvara:</span><span class="sxs-lookup"><span data-stu-id="df873-115">hello following diagram shows how tooconnect your hardware:</span></span>

![Maskinvaruinställningar för hallon Pi][img-connection-diagram]

<span data-ttu-id="df873-117">hello sammanfattas följande tabell hello anslutningar från hello hallon Pi toohello komponenter på hello breadboard:</span><span class="sxs-lookup"><span data-stu-id="df873-117">hello following table summarizes hello connections from hello Raspberry Pi toohello components on hello breadboard:</span></span>

| <span data-ttu-id="df873-118">Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="df873-118">Raspberry Pi</span></span>            | <span data-ttu-id="df873-119">Breadboard</span><span class="sxs-lookup"><span data-stu-id="df873-119">Breadboard</span></span>             |<span data-ttu-id="df873-120">Färg</span><span class="sxs-lookup"><span data-stu-id="df873-120">Color</span></span>         |
| ----------------------- | ---------------------- | ------------- |
| <span data-ttu-id="df873-121">JORD (PIN-kod 14)</span><span class="sxs-lookup"><span data-stu-id="df873-121">GND (Pin 14)</span></span>            | <span data-ttu-id="df873-122">Indikator - para PIN-kod (18A)</span><span class="sxs-lookup"><span data-stu-id="df873-122">LED -ve pin (18A)</span></span>      | <span data-ttu-id="df873-123">Lila</span><span class="sxs-lookup"><span data-stu-id="df873-123">Purple</span></span>          |
| <span data-ttu-id="df873-124">GPCLK0 (PIN-kod 7)</span><span class="sxs-lookup"><span data-stu-id="df873-124">GPCLK0 (Pin 7)</span></span>          | <span data-ttu-id="df873-125">Resistor (25A)</span><span class="sxs-lookup"><span data-stu-id="df873-125">Resistor (25A)</span></span>         | <span data-ttu-id="df873-126">Orange</span><span class="sxs-lookup"><span data-stu-id="df873-126">Orange</span></span>          |
| <span data-ttu-id="df873-127">SPI_CE0 (PIN-kod 24)</span><span class="sxs-lookup"><span data-stu-id="df873-127">SPI_CE0 (Pin 24)</span></span>        | <span data-ttu-id="df873-128">CS (39A)</span><span class="sxs-lookup"><span data-stu-id="df873-128">CS (39A)</span></span>               | <span data-ttu-id="df873-129">Blå</span><span class="sxs-lookup"><span data-stu-id="df873-129">Blue</span></span>          |
| <span data-ttu-id="df873-130">SPI_SCLK (PIN-kod 23)</span><span class="sxs-lookup"><span data-stu-id="df873-130">SPI_SCLK (Pin 23)</span></span>       | <span data-ttu-id="df873-131">SCK (36A)</span><span class="sxs-lookup"><span data-stu-id="df873-131">SCK (36A)</span></span>              | <span data-ttu-id="df873-132">Gul</span><span class="sxs-lookup"><span data-stu-id="df873-132">Yellow</span></span>        |
| <span data-ttu-id="df873-133">SPI_MISO (PIN-kod 21)</span><span class="sxs-lookup"><span data-stu-id="df873-133">SPI_MISO (Pin 21)</span></span>       | <span data-ttu-id="df873-134">SDO (37A)</span><span class="sxs-lookup"><span data-stu-id="df873-134">SDO (37A)</span></span>              | <span data-ttu-id="df873-135">Vit</span><span class="sxs-lookup"><span data-stu-id="df873-135">White</span></span>         |
| <span data-ttu-id="df873-136">SPI_MOSI (PIN-kod 19)</span><span class="sxs-lookup"><span data-stu-id="df873-136">SPI_MOSI (Pin 19)</span></span>       | <span data-ttu-id="df873-137">SDI (38A)</span><span class="sxs-lookup"><span data-stu-id="df873-137">SDI (38A)</span></span>              | <span data-ttu-id="df873-138">Grön</span><span class="sxs-lookup"><span data-stu-id="df873-138">Green</span></span>         |
| <span data-ttu-id="df873-139">JORD (PIN-kod 6)</span><span class="sxs-lookup"><span data-stu-id="df873-139">GND (Pin 6)</span></span>             | <span data-ttu-id="df873-140">JORD (35A)</span><span class="sxs-lookup"><span data-stu-id="df873-140">GND (35A)</span></span>              | <span data-ttu-id="df873-141">Svart</span><span class="sxs-lookup"><span data-stu-id="df873-141">Black</span></span>         |
| <span data-ttu-id="df873-142">3.3 V (PIN-kod 1)</span><span class="sxs-lookup"><span data-stu-id="df873-142">3.3 V (Pin 1)</span></span>           | <span data-ttu-id="df873-143">3Vo (34A)</span><span class="sxs-lookup"><span data-stu-id="df873-143">3Vo (34A)</span></span>              | <span data-ttu-id="df873-144">Röd</span><span class="sxs-lookup"><span data-stu-id="df873-144">Red</span></span>           |

<span data-ttu-id="df873-145">toocomplete hello maskinvaruinställningar, måste du:</span><span class="sxs-lookup"><span data-stu-id="df873-145">toocomplete hello hardware setup, you need to:</span></span>

- <span data-ttu-id="df873-146">Anslut din hallon Pi toohello strömförsörjning ingår i hello kit.</span><span class="sxs-lookup"><span data-stu-id="df873-146">Connect your Raspberry Pi toohello power supply included in hello kit.</span></span>
- <span data-ttu-id="df873-147">Ansluta med hello Ethernet-kabel ingår i din kit hallon Pi tooyour nätverket.</span><span class="sxs-lookup"><span data-stu-id="df873-147">Connect your Raspberry Pi tooyour network using hello Ethernet cable included in your kit.</span></span> <span data-ttu-id="df873-148">Du kan också ställa in [trådlös anslutning] [ lnk-pi-wireless] för din hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="df873-148">Alternatively, you can set up [Wireless Connectivity][lnk-pi-wireless] for your Raspberry Pi.</span></span>

<span data-ttu-id="df873-149">Du har nu slutfört hello maskinvaruinställningar hallon-pi.</span><span class="sxs-lookup"><span data-stu-id="df873-149">You have now completed hello hardware setup of your Raspberry Pi.</span></span>

### <a name="sign-in-and-access-hello-terminal"></a><span data-ttu-id="df873-150">Logga in och komma åt hello terminal</span><span class="sxs-lookup"><span data-stu-id="df873-150">Sign in and access hello terminal</span></span>

<span data-ttu-id="df873-151">Du har två alternativ tooaccess en miljö med terminal på din hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="df873-151">You have two options tooaccess a terminal environment on your Raspberry Pi:</span></span>

- <span data-ttu-id="df873-152">Om du har ett tangentbord och övervaka anslutna tooyour hallon Pi kan du använda hello Raspbian GUI tooaccess ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="df873-152">If you have a keyboard and monitor connected tooyour Raspberry Pi, you can use hello Raspbian GUI tooaccess a terminal window.</span></span>

- <span data-ttu-id="df873-153">Åtkomst hello kommandoraden på din hallon Pi använder SSH från den stationära datorn.</span><span class="sxs-lookup"><span data-stu-id="df873-153">Access hello command line on your Raspberry Pi using SSH from your desktop machine.</span></span>

#### <a name="use-a-terminal-window-in-hello-gui"></a><span data-ttu-id="df873-154">Använd ett terminalfönster i hello GUI</span><span class="sxs-lookup"><span data-stu-id="df873-154">Use a terminal Window in hello GUI</span></span>

<span data-ttu-id="df873-155">Hej standardautentiseringsuppgifterna för Raspbian är användarnamn **pi** och lösenord **hallon**.</span><span class="sxs-lookup"><span data-stu-id="df873-155">hello default credentials for Raspbian are username **pi** and password **raspberry**.</span></span> <span data-ttu-id="df873-156">I Aktivitetsfältet i hello GUI hello kan du starta hello **Terminal** verktyget med hello-ikonen som ser ut som en bildskärm.</span><span class="sxs-lookup"><span data-stu-id="df873-156">In hello task bar in hello GUI, you can launch hello **Terminal** utility using hello icon that looks like a monitor.</span></span>

#### <a name="sign-in-with-ssh"></a><span data-ttu-id="df873-157">Logga in med SSH</span><span class="sxs-lookup"><span data-stu-id="df873-157">Sign in with SSH</span></span>

<span data-ttu-id="df873-158">Du kan använda SSH för kommandoradsåtkomst tooyour hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="df873-158">You can use SSH for command-line access tooyour Raspberry Pi.</span></span> <span data-ttu-id="df873-159">hello artikel [SSH (Secure Shell)] [ lnk-pi-ssh] beskriver hur tooconfigure SSH på din hallon Pi, och hur tooconnect från [Windows] [ lnk-ssh-windows] eller [Linux och Mac OS][lnk-ssh-linux].</span><span class="sxs-lookup"><span data-stu-id="df873-159">hello article [SSH (Secure Shell)][lnk-pi-ssh] describes how tooconfigure SSH on your Raspberry Pi, and how tooconnect from [Windows][lnk-ssh-windows] or [Linux & Mac OS][lnk-ssh-linux].</span></span>

<span data-ttu-id="df873-160">Logga in med användarnamn **pi** och lösenord **hallon**.</span><span class="sxs-lookup"><span data-stu-id="df873-160">Sign in with username **pi** and password **raspberry**.</span></span>

#### <a name="optional-share-a-folder-on-your-raspberry-pi"></a><span data-ttu-id="df873-161">Valfritt: Dela en mapp på din hallon Pi</span><span class="sxs-lookup"><span data-stu-id="df873-161">Optional: Share a folder on your Raspberry Pi</span></span>

<span data-ttu-id="df873-162">Alternativt kan kanske du tooshare en mapp på din hallon Pi med skrivbordsmiljön.</span><span class="sxs-lookup"><span data-stu-id="df873-162">Optionally, you may want tooshare a folder on your Raspberry Pi with your desktop environment.</span></span> <span data-ttu-id="df873-163">Dela en mapp kan du toouse din önskade skrivbord textredigerare (exempelvis [Visual Studio Code](https://code.visualstudio.com/) eller [Sublime Text](http://www.sublimetext.com/)) tooedit filer på din hallon Pi istället för att använda `nano` eller `vi`.</span><span class="sxs-lookup"><span data-stu-id="df873-163">Sharing a folder enables you toouse your preferred desktop text editor (such as [Visual Studio Code](https://code.visualstudio.com/) or [Sublime Text](http://www.sublimetext.com/)) tooedit files on your Raspberry Pi instead of using `nano` or `vi`.</span></span>

<span data-ttu-id="df873-164">tooshare en mapp med Windows, konfigurera en Samba-server på hello hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="df873-164">tooshare a folder with Windows, configure a Samba server on hello Raspberry Pi.</span></span> <span data-ttu-id="df873-165">Du kan också använda hello inbyggda [SFTP](https://www.raspberrypi.org/documentation/remote-access/) server med en SFTP-klient på skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="df873-165">Alternatively, use hello built-in [SFTP](https://www.raspberrypi.org/documentation/remote-access/) server with an SFTP client on your desktop.</span></span>

### <a name="enable-spi"></a><span data-ttu-id="df873-166">Aktivera SPI</span><span class="sxs-lookup"><span data-stu-id="df873-166">Enable SPI</span></span>

<span data-ttu-id="df873-167">Innan du kan köra hello exempelprogrammet måste du aktivera hello seriella kringutrustning gränssnittet SPI ()-bussen på hello hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="df873-167">Before you can run hello sample application, you must enable hello Serial Peripheral Interface (SPI) bus on hello Raspberry Pi.</span></span> <span data-ttu-id="df873-168">hello hallon Pi kommunicerar med hello BME280 sensor enheten via hello SPI bus.</span><span class="sxs-lookup"><span data-stu-id="df873-168">hello Raspberry Pi communicates with hello BME280 sensor device over hello SPI bus.</span></span> <span data-ttu-id="df873-169">Använd hello följande kommando tooedit hello konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="df873-169">Use hello following command tooedit hello configuration file:</span></span>

```sh
sudo nano /boot/config.txt
```

<span data-ttu-id="df873-170">Hitta hello rad:</span><span class="sxs-lookup"><span data-stu-id="df873-170">Find hello line:</span></span>

`#dtparam=spi=on`

- <span data-ttu-id="df873-171">toouncomment hello rad, ta bort hello `#` hello början.</span><span class="sxs-lookup"><span data-stu-id="df873-171">toouncomment hello line, delete hello `#` at hello start.</span></span>
- <span data-ttu-id="df873-172">Spara dina ändringar (**Ctrl-O**, **RETUR**) och avsluta hello editor (**Ctrl + X**).</span><span class="sxs-lookup"><span data-stu-id="df873-172">Save your changes (**Ctrl-O**, **Enter**) and exit hello editor (**Ctrl-X**).</span></span>
- <span data-ttu-id="df873-173">tooenable SPI omstart hello hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="df873-173">tooenable SPI, reboot hello Raspberry Pi.</span></span> <span data-ttu-id="df873-174">Omstart kopplar hello terminal måste du toosign i igen när hello hallon Pi ska startas om:</span><span class="sxs-lookup"><span data-stu-id="df873-174">Rebooting disconnects hello terminal, you need toosign in again when hello Raspberry Pi restarts:</span></span>

  ```sh
  sudo reboot
  ```


[img-connection-diagram]: media/iot-suite-raspberry-pi-kit-prepare-pi/rpi2_remote_monitoring.png

[lnk-install-raspbian]: https://www.raspberrypi.org/learning/software-guide/quickstart/
[lnk-pi-wireless]: https://www.raspberrypi.org/documentation/configuration/wireless/README.md
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/