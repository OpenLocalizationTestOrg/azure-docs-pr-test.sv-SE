## <a name="prepare-your-raspberry-pi"></a><span data-ttu-id="dcf1e-101">Förbered din Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="dcf1e-101">Prepare your Raspberry Pi</span></span>

### <a name="install-raspbian"></a><span data-ttu-id="dcf1e-102">Installera Raspbian</span><span class="sxs-lookup"><span data-stu-id="dcf1e-102">Install Raspbian</span></span>

<span data-ttu-id="dcf1e-103">Om detta är hello första gången du använder din hallon Pi måste tooinstall hello Raspbian operativsystem genom att använda NOOBS på hello SD-kort som ingår i hello kit.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-103">If this is hello first time you are using your Raspberry Pi, you need tooinstall hello Raspbian operating system using NOOBS on hello SD card included in hello kit.</span></span> <span data-ttu-id="dcf1e-104">Hej [hallon Pi Software Guide] [ lnk-install-raspbian] beskriver hur tooinstall ett operativsystem på din hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-104">hello [Raspberry Pi Software Guide][lnk-install-raspbian] describes how tooinstall an operating system on your Raspberry Pi.</span></span> <span data-ttu-id="dcf1e-105">Den här självstudiekursen förutsätts att du har installerat hello Raspbian operativsystemet på din hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-105">This tutorial assumes you have installed hello Raspbian operating system on your Raspberry Pi.</span></span>

> [!NOTE]
> <span data-ttu-id="dcf1e-106">hello SD-kort som ingår i hello [Microsoft Azure IoT-startpaket för hallon Pi 3] [ lnk-starter-kits] NOOBS installerat finns redan.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-106">hello SD card included in hello [Microsoft Azure IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] already has NOOBS installed.</span></span> <span data-ttu-id="dcf1e-107">Du kan starta hello hallon Pi från det här kortet och välj tooinstall hello Raspbian OS.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-107">You can boot hello Raspberry Pi from this card and choose tooinstall hello Raspbian OS.</span></span>

<span data-ttu-id="dcf1e-108">toocomplete hello maskinvaruinställningar, måste du:</span><span class="sxs-lookup"><span data-stu-id="dcf1e-108">toocomplete hello hardware setup, you need to:</span></span>

- <span data-ttu-id="dcf1e-109">Anslut din hallon Pi toohello strömförsörjning ingår i hello kit.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-109">Connect your Raspberry Pi toohello power supply included in hello kit.</span></span>
- <span data-ttu-id="dcf1e-110">Ansluta med hello Ethernet-kabel ingår i din kit hallon Pi tooyour nätverket.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-110">Connect your Raspberry Pi tooyour network using hello Ethernet cable included in your kit.</span></span> <span data-ttu-id="dcf1e-111">Du kan också ställa in [trådlös anslutning] [ lnk-pi-wireless] för din hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-111">Alternatively, you can set up [Wireless Connectivity][lnk-pi-wireless] for your Raspberry Pi.</span></span>

<span data-ttu-id="dcf1e-112">Du har nu slutfört hello maskinvaruinställningar hallon-pi.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-112">You have now completed hello hardware setup of your Raspberry Pi.</span></span>

### <a name="sign-in-and-access-hello-terminal"></a><span data-ttu-id="dcf1e-113">Logga in och komma åt hello terminal</span><span class="sxs-lookup"><span data-stu-id="dcf1e-113">Sign in and access hello terminal</span></span>

<span data-ttu-id="dcf1e-114">Du har två alternativ tooaccess en miljö med terminal på din hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="dcf1e-114">You have two options tooaccess a terminal environment on your Raspberry Pi:</span></span>

- <span data-ttu-id="dcf1e-115">Om du har ett tangentbord och övervaka anslutna tooyour hallon Pi kan du använda hello Raspbian GUI tooaccess ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-115">If you have a keyboard and monitor connected tooyour Raspberry Pi, you can use hello Raspbian GUI tooaccess a terminal window.</span></span>

- <span data-ttu-id="dcf1e-116">Åtkomst hello kommandoraden på din hallon Pi använder SSH från den stationära datorn.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-116">Access hello command line on your Raspberry Pi using SSH from your desktop machine.</span></span>

#### <a name="use-a-terminal-window-in-hello-gui"></a><span data-ttu-id="dcf1e-117">Använd ett terminalfönster i hello GUI</span><span class="sxs-lookup"><span data-stu-id="dcf1e-117">Use a terminal Window in hello GUI</span></span>

<span data-ttu-id="dcf1e-118">Hej standardautentiseringsuppgifterna för Raspbian är användarnamn **pi** och lösenord **hallon**.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-118">hello default credentials for Raspbian are username **pi** and password **raspberry**.</span></span> <span data-ttu-id="dcf1e-119">I Aktivitetsfältet i hello GUI hello kan du starta hello **Terminal** verktyget med hello-ikonen som ser ut som en bildskärm.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-119">In hello task bar in hello GUI, you can launch hello **Terminal** utility using hello icon that looks like a monitor.</span></span>

#### <a name="sign-in-with-ssh"></a><span data-ttu-id="dcf1e-120">Logga in med SSH</span><span class="sxs-lookup"><span data-stu-id="dcf1e-120">Sign in with SSH</span></span>

<span data-ttu-id="dcf1e-121">Du kan använda SSH för kommandoradsåtkomst tooyour hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-121">You can use SSH for command-line access tooyour Raspberry Pi.</span></span> <span data-ttu-id="dcf1e-122">hello artikel [SSH (Secure Shell)] [ lnk-pi-ssh] beskriver hur tooconfigure SSH på din hallon Pi, och hur tooconnect från [Windows] [ lnk-ssh-windows] eller [Linux och Mac OS][lnk-ssh-linux].</span><span class="sxs-lookup"><span data-stu-id="dcf1e-122">hello article [SSH (Secure Shell)][lnk-pi-ssh] describes how tooconfigure SSH on your Raspberry Pi, and how tooconnect from [Windows][lnk-ssh-windows] or [Linux & Mac OS][lnk-ssh-linux].</span></span>

<span data-ttu-id="dcf1e-123">Logga in med användarnamn **pi** och lösenord **hallon**.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-123">Sign in with username **pi** and password **raspberry**.</span></span>

#### <a name="optional-share-a-folder-on-your-raspberry-pi"></a><span data-ttu-id="dcf1e-124">Valfritt: Dela en mapp på din hallon Pi</span><span class="sxs-lookup"><span data-stu-id="dcf1e-124">Optional: Share a folder on your Raspberry Pi</span></span>

<span data-ttu-id="dcf1e-125">Alternativt kan kanske du tooshare en mapp på din hallon Pi med skrivbordsmiljön.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-125">Optionally, you may want tooshare a folder on your Raspberry Pi with your desktop environment.</span></span> <span data-ttu-id="dcf1e-126">Dela en mapp kan du toouse din önskade skrivbord textredigerare (exempelvis [Visual Studio Code](https://code.visualstudio.com/) eller [Sublime Text](http://www.sublimetext.com/)) tooedit filer på din hallon Pi istället för att använda `nano` eller `vi`.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-126">Sharing a folder enables you toouse your preferred desktop text editor (such as [Visual Studio Code](https://code.visualstudio.com/) or [Sublime Text](http://www.sublimetext.com/)) tooedit files on your Raspberry Pi instead of using `nano` or `vi`.</span></span>

<span data-ttu-id="dcf1e-127">tooshare en mapp med Windows, konfigurera en Samba-server på hello hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-127">tooshare a folder with Windows, configure a Samba server on hello Raspberry Pi.</span></span> <span data-ttu-id="dcf1e-128">Du kan också använda hello inbyggda [SFTP](https://www.raspberrypi.org/documentation/remote-access/) server med en SFTP-klient på skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="dcf1e-128">Alternatively, use hello built-in [SFTP](https://www.raspberrypi.org/documentation/remote-access/) server with an SFTP client on your desktop.</span></span>

[lnk-install-raspbian]: https://www.raspberrypi.org/learning/software-guide/quickstart/
[lnk-pi-wireless]: https://www.raspberrypi.org/documentation/configuration/wireless/README.md
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/