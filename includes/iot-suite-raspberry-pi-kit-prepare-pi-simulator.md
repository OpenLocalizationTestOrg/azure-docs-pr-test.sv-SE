## <a name="prepare-your-raspberry-pi"></a><span data-ttu-id="a45db-101">Förbered din Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="a45db-101">Prepare your Raspberry Pi</span></span>

### <a name="install-raspbian"></a><span data-ttu-id="a45db-102">Installera Raspbian</span><span class="sxs-lookup"><span data-stu-id="a45db-102">Install Raspbian</span></span>

<span data-ttu-id="a45db-103">Om det här är första gången du använder din hallon Pi som du behöver installera Raspbian operativsystemet med hjälp av NOOBS på SD-kort som ingår i kit.</span><span class="sxs-lookup"><span data-stu-id="a45db-103">If this is the first time you are using your Raspberry Pi, you need to install the Raspbian operating system using NOOBS on the SD card included in the kit.</span></span> <span data-ttu-id="a45db-104">Den [hallon Pi Software Guide] [ lnk-install-raspbian] beskriver hur du installerar ett operativsystem på din hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="a45db-104">The [Raspberry Pi Software Guide][lnk-install-raspbian] describes how to install an operating system on your Raspberry Pi.</span></span> <span data-ttu-id="a45db-105">Den här kursen förutsätter att du har installerat operativsystemet Raspbian på din hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="a45db-105">This tutorial assumes you have installed the Raspbian operating system on your Raspberry Pi.</span></span>

> [!NOTE]
> <span data-ttu-id="a45db-106">SD-kort som ingår i den [Microsoft Azure IoT-startpaket för hallon Pi 3] [ lnk-starter-kits] NOOBS installerat finns redan.</span><span class="sxs-lookup"><span data-stu-id="a45db-106">The SD card included in the [Microsoft Azure IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] already has NOOBS installed.</span></span> <span data-ttu-id="a45db-107">Du kan starta hallon Pi från det här kortet och välja att installera Raspbian OS.</span><span class="sxs-lookup"><span data-stu-id="a45db-107">You can boot the Raspberry Pi from this card and choose to install the Raspbian OS.</span></span>

<span data-ttu-id="a45db-108">Du slutför installationen av maskinvara, måste du:</span><span class="sxs-lookup"><span data-stu-id="a45db-108">To complete the hardware setup, you need to:</span></span>

- <span data-ttu-id="a45db-109">Anslut din hallon Pi till strömförsörjning som ingår i kit.</span><span class="sxs-lookup"><span data-stu-id="a45db-109">Connect your Raspberry Pi to the power supply included in the kit.</span></span>
- <span data-ttu-id="a45db-110">Anslut din hallon Pi till nätverket via Ethernet-kabel som ingår i din kit.</span><span class="sxs-lookup"><span data-stu-id="a45db-110">Connect your Raspberry Pi to your network using the Ethernet cable included in your kit.</span></span> <span data-ttu-id="a45db-111">Du kan också ställa in [trådlös anslutning] [ lnk-pi-wireless] för din hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="a45db-111">Alternatively, you can set up [Wireless Connectivity][lnk-pi-wireless] for your Raspberry Pi.</span></span>

<span data-ttu-id="a45db-112">Du har nu slutfört maskinvaruinställningar hallon-pi.</span><span class="sxs-lookup"><span data-stu-id="a45db-112">You have now completed the hardware setup of your Raspberry Pi.</span></span>

### <a name="sign-in-and-access-the-terminal"></a><span data-ttu-id="a45db-113">Logga in och öppna terminalen</span><span class="sxs-lookup"><span data-stu-id="a45db-113">Sign in and access the terminal</span></span>

<span data-ttu-id="a45db-114">Du har två alternativ för att komma åt en terminal miljö på din hallon Pi:</span><span class="sxs-lookup"><span data-stu-id="a45db-114">You have two options to access a terminal environment on your Raspberry Pi:</span></span>

- <span data-ttu-id="a45db-115">Om du har ett tangentbord och bildskärm ansluten till din hallon Pi, kan du använda det grafiska Användargränssnittet Raspbian att komma åt ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="a45db-115">If you have a keyboard and monitor connected to your Raspberry Pi, you can use the Raspbian GUI to access a terminal window.</span></span>

- <span data-ttu-id="a45db-116">Åtkomst till kommandoraden på din hallon Pi använder SSH från den stationära datorn.</span><span class="sxs-lookup"><span data-stu-id="a45db-116">Access the command line on your Raspberry Pi using SSH from your desktop machine.</span></span>

#### <a name="use-a-terminal-window-in-the-gui"></a><span data-ttu-id="a45db-117">Använd ett terminalfönster i det grafiska Användargränssnittet</span><span class="sxs-lookup"><span data-stu-id="a45db-117">Use a terminal Window in the GUI</span></span>

<span data-ttu-id="a45db-118">Standardautentiseringsuppgifterna för Raspbian är användarnamn **pi** och lösenord **hallon**.</span><span class="sxs-lookup"><span data-stu-id="a45db-118">The default credentials for Raspbian are username **pi** and password **raspberry**.</span></span> <span data-ttu-id="a45db-119">I Aktivitetsfältet i det grafiska Användargränssnittet kan du starta den **Terminal** verktyget med hjälp av ikonen som ser ut som en bildskärm.</span><span class="sxs-lookup"><span data-stu-id="a45db-119">In the task bar in the GUI, you can launch the **Terminal** utility using the icon that looks like a monitor.</span></span>

#### <a name="sign-in-with-ssh"></a><span data-ttu-id="a45db-120">Logga in med SSH</span><span class="sxs-lookup"><span data-stu-id="a45db-120">Sign in with SSH</span></span>

<span data-ttu-id="a45db-121">Du kan använda SSH för kommandoraden åtkomst till din hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="a45db-121">You can use SSH for command-line access to your Raspberry Pi.</span></span> <span data-ttu-id="a45db-122">Artikeln [SSH (Secure Shell)] [ lnk-pi-ssh] beskriver hur du konfigurerar SSH på din hallon Pi och hur du ansluter från [Windows] [ lnk-ssh-windows] eller [ Linux- och Mac OS][lnk-ssh-linux].</span><span class="sxs-lookup"><span data-stu-id="a45db-122">The article [SSH (Secure Shell)][lnk-pi-ssh] describes how to configure SSH on your Raspberry Pi, and how to connect from [Windows][lnk-ssh-windows] or [Linux & Mac OS][lnk-ssh-linux].</span></span>

<span data-ttu-id="a45db-123">Logga in med användarnamn **pi** och lösenord **hallon**.</span><span class="sxs-lookup"><span data-stu-id="a45db-123">Sign in with username **pi** and password **raspberry**.</span></span>

#### <a name="optional-share-a-folder-on-your-raspberry-pi"></a><span data-ttu-id="a45db-124">Valfritt: Dela en mapp på din hallon Pi</span><span class="sxs-lookup"><span data-stu-id="a45db-124">Optional: Share a folder on your Raspberry Pi</span></span>

<span data-ttu-id="a45db-125">Alternativt kan behöva du dela en mapp på din hallon Pi med skrivbordsmiljön.</span><span class="sxs-lookup"><span data-stu-id="a45db-125">Optionally, you may want to share a folder on your Raspberry Pi with your desktop environment.</span></span> <span data-ttu-id="a45db-126">Dela en mapp kan du använda din önskade skrivbord textredigerare (t.ex [Visual Studio Code](https://code.visualstudio.com/) eller [Sublime Text](http://www.sublimetext.com/)) att redigera filerna på din hallon Pi istället för att använda `nano` eller `vi`.</span><span class="sxs-lookup"><span data-stu-id="a45db-126">Sharing a folder enables you to use your preferred desktop text editor (such as [Visual Studio Code](https://code.visualstudio.com/) or [Sublime Text](http://www.sublimetext.com/)) to edit files on your Raspberry Pi instead of using `nano` or `vi`.</span></span>

<span data-ttu-id="a45db-127">Om du vill dela en mapp med Windows, konfigurera en Samba-server på hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="a45db-127">To share a folder with Windows, configure a Samba server on the Raspberry Pi.</span></span> <span data-ttu-id="a45db-128">Du kan också använda inbyggt [SFTP](https://www.raspberrypi.org/documentation/remote-access/) server med en SFTP-klient på skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="a45db-128">Alternatively, use the built-in [SFTP](https://www.raspberrypi.org/documentation/remote-access/) server with an SFTP client on your desktop.</span></span>

[lnk-install-raspbian]: https://www.raspberrypi.org/learning/software-guide/quickstart/
[lnk-pi-wireless]: https://www.raspberrypi.org/documentation/configuration/wireless/README.md
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/