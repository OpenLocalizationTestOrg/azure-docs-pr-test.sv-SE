## <a name="prepare-your-intel-nuc"></a><span data-ttu-id="0b614-101">Förbered din Intel NUC</span><span class="sxs-lookup"><span data-stu-id="0b614-101">Prepare your Intel NUC</span></span>

<span data-ttu-id="0b614-102">Du slutför installationen av maskinvara, måste du:</span><span class="sxs-lookup"><span data-stu-id="0b614-102">To complete the hardware setup, you need to:</span></span>

- <span data-ttu-id="0b614-103">Anslut din Intel NUC till strömförsörjning som ingår i kit.</span><span class="sxs-lookup"><span data-stu-id="0b614-103">Connect your Intel NUC to the power supply included in the kit.</span></span>
- <span data-ttu-id="0b614-104">Anslut din Intel NUC till nätverket med hjälp av en Ethernet-kabel.</span><span class="sxs-lookup"><span data-stu-id="0b614-104">Connect your Intel NUC to your network using an Ethernet cable.</span></span>

<span data-ttu-id="0b614-105">Du har nu slutfört maskinvaruinställningar av Intel NUC-gateway-enheten.</span><span class="sxs-lookup"><span data-stu-id="0b614-105">You have now completed the hardware setup of your Intel NUC gateway device.</span></span>

### <a name="sign-in-and-access-the-terminal"></a><span data-ttu-id="0b614-106">Logga in och öppna terminalen</span><span class="sxs-lookup"><span data-stu-id="0b614-106">Sign in and access the terminal</span></span>

<span data-ttu-id="0b614-107">Du har två alternativ för att komma åt en terminal miljö på Intel-NUC:</span><span class="sxs-lookup"><span data-stu-id="0b614-107">You have two options to access a terminal environment on your Intel NUC:</span></span>

- <span data-ttu-id="0b614-108">Om du har ett tangentbord och bildskärm har anslutits till Intel-NUC, kan du komma åt gränssnittet direkt.</span><span class="sxs-lookup"><span data-stu-id="0b614-108">If you have a keyboard and monitor connected to your Intel NUC, you can access the shell directly.</span></span> <span data-ttu-id="0b614-109">Standardautentiseringsuppgifterna som är användarnamnet **rot** och lösenord **rot**.</span><span class="sxs-lookup"><span data-stu-id="0b614-109">The default credentials are username **root** and password **root**.</span></span>

- <span data-ttu-id="0b614-110">Åtkomst till gränssnittet på din Intel NUC använder SSH från den stationära datorn.</span><span class="sxs-lookup"><span data-stu-id="0b614-110">Access the shell on your Intel NUC using SSH from your desktop machine.</span></span>

#### <a name="sign-in-with-ssh"></a><span data-ttu-id="0b614-111">Logga in med SSH</span><span class="sxs-lookup"><span data-stu-id="0b614-111">Sign in with SSH</span></span>

<span data-ttu-id="0b614-112">Om du vill logga in med SSH, måste du IP-adressen för Intel-NUC.</span><span class="sxs-lookup"><span data-stu-id="0b614-112">To sign in with SSH, you need the IP address of your Intel NUC.</span></span> <span data-ttu-id="0b614-113">Om du har ett tangentbord och bildskärm har anslutits till Intel-NUC använder den `ifconfig` kommando för att hitta IP-adress.</span><span class="sxs-lookup"><span data-stu-id="0b614-113">If you have a keyboard and monitor connected to your Intel NUC, use the `ifconfig` command to find the IP address.</span></span> <span data-ttu-id="0b614-114">Du kan också ansluta till routern att lista adresserna till enheter i nätverket.</span><span class="sxs-lookup"><span data-stu-id="0b614-114">Alternatively, connect to your router to list the addresses of devices on your network.</span></span>

<span data-ttu-id="0b614-115">Logga in med användarnamn **rot** och lösenord **rot**.</span><span class="sxs-lookup"><span data-stu-id="0b614-115">Sign in with username **root** and password **root**.</span></span>

#### <a name="optional-share-a-folder-on-your-intel-nuc"></a><span data-ttu-id="0b614-116">Valfritt: Dela en mapp på Intel-NUC</span><span class="sxs-lookup"><span data-stu-id="0b614-116">Optional: Share a folder on your Intel NUC</span></span>

<span data-ttu-id="0b614-117">Alternativt kan behöva du dela en mapp på Intel-NUC med skrivbordsmiljön.</span><span class="sxs-lookup"><span data-stu-id="0b614-117">Optionally, you may want to share a folder on your Intel NUC with your desktop environment.</span></span> <span data-ttu-id="0b614-118">Dela en mapp kan du använda din önskade skrivbord textredigerare (t.ex [Visual Studio Code](https://code.visualstudio.com/) eller [Sublime Text](http://www.sublimetext.com/)) att redigera filerna på din Intel NUC istället för att använda `nano` eller `vi`.</span><span class="sxs-lookup"><span data-stu-id="0b614-118">Sharing a folder enables you to use your preferred desktop text editor (such as [Visual Studio Code](https://code.visualstudio.com/) or [Sublime Text](http://www.sublimetext.com/)) to edit files on your Intel NUC instead of using `nano` or `vi`.</span></span>

<span data-ttu-id="0b614-119">Konfigurera en Samba-server på Intel NUC om du vill dela en mapp med Windows.</span><span class="sxs-lookup"><span data-stu-id="0b614-119">To share a folder with Windows, configure a Samba server on the Intel NUC.</span></span> <span data-ttu-id="0b614-120">Du kan också använda SFTP-server på Intel NUC med SFTP-klienten på den stationära datorn.</span><span class="sxs-lookup"><span data-stu-id="0b614-120">Alternatively, use the SFTP server on the Intel NUC with an SFTP client on your desktop machine.</span></span>
