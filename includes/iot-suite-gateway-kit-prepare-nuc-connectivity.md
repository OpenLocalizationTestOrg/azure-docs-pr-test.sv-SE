## <a name="prepare-your-intel-nuc"></a><span data-ttu-id="96bbf-101">Förbered din Intel NUC</span><span class="sxs-lookup"><span data-stu-id="96bbf-101">Prepare your Intel NUC</span></span>

<span data-ttu-id="96bbf-102">toocomplete hello maskinvaruinställningar, måste du:</span><span class="sxs-lookup"><span data-stu-id="96bbf-102">toocomplete hello hardware setup, you need to:</span></span>

- <span data-ttu-id="96bbf-103">Anslut din Intel NUC toohello strömförsörjning ingår i hello kit.</span><span class="sxs-lookup"><span data-stu-id="96bbf-103">Connect your Intel NUC toohello power supply included in hello kit.</span></span>
- <span data-ttu-id="96bbf-104">Ansluta nätverket Intel NUC tooyour med hjälp av en Ethernet-kabel.</span><span class="sxs-lookup"><span data-stu-id="96bbf-104">Connect your Intel NUC tooyour network using an Ethernet cable.</span></span>

<span data-ttu-id="96bbf-105">Du har nu slutfört hello maskinvaruinställningar av Intel NUC-gateway-enheten.</span><span class="sxs-lookup"><span data-stu-id="96bbf-105">You have now completed hello hardware setup of your Intel NUC gateway device.</span></span>

### <a name="sign-in-and-access-hello-terminal"></a><span data-ttu-id="96bbf-106">Logga in och komma åt hello terminal</span><span class="sxs-lookup"><span data-stu-id="96bbf-106">Sign in and access hello terminal</span></span>

<span data-ttu-id="96bbf-107">Du har två alternativ tooaccess terminal miljö på Intel-NUC:</span><span class="sxs-lookup"><span data-stu-id="96bbf-107">You have two options tooaccess a terminal environment on your Intel NUC:</span></span>

- <span data-ttu-id="96bbf-108">Om du har ett tangentbord och övervaka anslutna tooyour Intel NUC kan du komma åt hello shell direkt.</span><span class="sxs-lookup"><span data-stu-id="96bbf-108">If you have a keyboard and monitor connected tooyour Intel NUC, you can access hello shell directly.</span></span> <span data-ttu-id="96bbf-109">hello standardautentiseringsuppgifter är användarnamn **rot** och lösenord **rot**.</span><span class="sxs-lookup"><span data-stu-id="96bbf-109">hello default credentials are username **root** and password **root**.</span></span>

- <span data-ttu-id="96bbf-110">Åtkomst hello shell på din Intel NUC använder SSH från den stationära datorn.</span><span class="sxs-lookup"><span data-stu-id="96bbf-110">Access hello shell on your Intel NUC using SSH from your desktop machine.</span></span>

#### <a name="sign-in-with-ssh"></a><span data-ttu-id="96bbf-111">Logga in med SSH</span><span class="sxs-lookup"><span data-stu-id="96bbf-111">Sign in with SSH</span></span>

<span data-ttu-id="96bbf-112">toosign in med SSH måste hello IP-adressen för Intel-NUC.</span><span class="sxs-lookup"><span data-stu-id="96bbf-112">toosign in with SSH, you need hello IP address of your Intel NUC.</span></span> <span data-ttu-id="96bbf-113">Om du har ett tangentbord och övervaka anslutna tooyour Intel NUC använda hello `ifconfig` kommandot toofind hello IP-adress.</span><span class="sxs-lookup"><span data-stu-id="96bbf-113">If you have a keyboard and monitor connected tooyour Intel NUC, use hello `ifconfig` command toofind hello IP address.</span></span> <span data-ttu-id="96bbf-114">Du kan också ansluta tooyour router toolist hello-adresserna för enheter i nätverket.</span><span class="sxs-lookup"><span data-stu-id="96bbf-114">Alternatively, connect tooyour router toolist hello addresses of devices on your network.</span></span>

<span data-ttu-id="96bbf-115">Logga in med användarnamn **rot** och lösenord **rot**.</span><span class="sxs-lookup"><span data-stu-id="96bbf-115">Sign in with username **root** and password **root**.</span></span>

#### <a name="optional-share-a-folder-on-your-intel-nuc"></a><span data-ttu-id="96bbf-116">Valfritt: Dela en mapp på Intel-NUC</span><span class="sxs-lookup"><span data-stu-id="96bbf-116">Optional: Share a folder on your Intel NUC</span></span>

<span data-ttu-id="96bbf-117">Alternativt kan kanske du tooshare en mapp på Intel-NUC med skrivbordsmiljön.</span><span class="sxs-lookup"><span data-stu-id="96bbf-117">Optionally, you may want tooshare a folder on your Intel NUC with your desktop environment.</span></span> <span data-ttu-id="96bbf-118">Dela en mapp kan du toouse din önskade skrivbord textredigerare (exempelvis [Visual Studio Code](https://code.visualstudio.com/) eller [Sublime Text](http://www.sublimetext.com/)) tooedit filer på din Intel NUC istället för att använda `nano` eller `vi`.</span><span class="sxs-lookup"><span data-stu-id="96bbf-118">Sharing a folder enables you toouse your preferred desktop text editor (such as [Visual Studio Code](https://code.visualstudio.com/) or [Sublime Text](http://www.sublimetext.com/)) tooedit files on your Intel NUC instead of using `nano` or `vi`.</span></span>

<span data-ttu-id="96bbf-119">tooshare en mapp med Windows, konfigurera en Samba-server på hello Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="96bbf-119">tooshare a folder with Windows, configure a Samba server on hello Intel NUC.</span></span> <span data-ttu-id="96bbf-120">Du kan också använda hello SFTP-server på hello Intel NUC med SFTP-klienten på den stationära datorn.</span><span class="sxs-lookup"><span data-stu-id="96bbf-120">Alternatively, use hello SFTP server on hello Intel NUC with an SFTP client on your desktop machine.</span></span>
