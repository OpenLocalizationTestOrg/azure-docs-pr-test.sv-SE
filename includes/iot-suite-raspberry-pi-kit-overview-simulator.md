## <a name="overview"></a><span data-ttu-id="ff364-101">Översikt</span><span class="sxs-lookup"><span data-stu-id="ff364-101">Overview</span></span>

<span data-ttu-id="ff364-102">Du har slutfört hello följa stegen i den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="ff364-102">In this tutorial, you complete hello following steps:</span></span>

- <span data-ttu-id="ff364-103">Distribuera en instans av enligt förkonfigurerade lösningen tooyour i hello fjärråtkomst övervakning Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ff364-103">Deploy an instance of hello remote monitoring preconfigured solution tooyour Azure subscription.</span></span> <span data-ttu-id="ff364-104">Det här steget kan du automatiskt distribuerar och konfigurerar Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="ff364-104">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="ff364-105">Konfigurera din enhet toocommunicate med datorn och hello remote övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="ff364-105">Set up your device toocommunicate with your computer and hello remote monitoring solution.</span></span>
- <span data-ttu-id="ff364-106">Uppdatera hello exempel enhet kod tooconnect toohello remote övervakningslösning och skicka simulerade telemetri som kan visas på instrumentpanelen för hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="ff364-106">Update hello sample device code tooconnect toohello remote monitoring solution, and send simulated telemetry that you can view on hello solution dashboard.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff364-107">Krav</span><span class="sxs-lookup"><span data-stu-id="ff364-107">Prerequisites</span></span>

<span data-ttu-id="ff364-108">toocomplete den här självstudiekursen kommer du behöver en aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ff364-108">toocomplete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="ff364-109">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="ff364-109">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="ff364-110">Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="ff364-110">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

### <a name="required-software"></a><span data-ttu-id="ff364-111">Programvara som krävs</span><span class="sxs-lookup"><span data-stu-id="ff364-111">Required software</span></span>

<span data-ttu-id="ff364-112">Du måste SSH-klienten på den stationära datorn tooenable du tooremotely access hello-kommando rad på hello hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="ff364-112">You need SSH client on your desktop machine tooenable you tooremotely access hello command line on hello Raspberry Pi.</span></span>

- <span data-ttu-id="ff364-113">Windows innehåller inte en SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="ff364-113">Windows does not include an SSH client.</span></span> <span data-ttu-id="ff364-114">Vi rekommenderar att du använder [PuTTY](http://www.putty.org/).</span><span class="sxs-lookup"><span data-stu-id="ff364-114">We recommend using [PuTTY](http://www.putty.org/).</span></span>
- <span data-ttu-id="ff364-115">De flesta distributioner för Linux och Mac OS inkluderar hello SSH-kommandoradsverktyget.</span><span class="sxs-lookup"><span data-stu-id="ff364-115">Most Linux distributions and Mac OS include hello command-line SSH utility.</span></span> <span data-ttu-id="ff364-116">Mer information finns i [SSH med Linux- eller Mac OS x](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span><span class="sxs-lookup"><span data-stu-id="ff364-116">For more information, see [SSH Using Linux or Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span></span>

### <a name="required-hardware"></a><span data-ttu-id="ff364-117">Nödvändig maskinvara</span><span class="sxs-lookup"><span data-stu-id="ff364-117">Required hardware</span></span>

<span data-ttu-id="ff364-118">En stationär dator tooenable du tooconnect via fjärranslutning toohello kommandot rad på hello hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="ff364-118">A desktop computer tooenable you tooconnect remotely toohello command line on hello Raspberry Pi.</span></span>

<span data-ttu-id="ff364-119">[Microsoft IoT startpaket för hallon Pi 3] [ lnk-starter-kits] eller motsvarande komponenter.</span><span class="sxs-lookup"><span data-stu-id="ff364-119">[Microsoft IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] or equivalent components.</span></span> <span data-ttu-id="ff364-120">Den här kursen använder följande objekt från hello kit hello:</span><span class="sxs-lookup"><span data-stu-id="ff364-120">This tutorial uses hello following items from hello kit:</span></span>

- <span data-ttu-id="ff364-121">Raspberry Pi 3</span><span class="sxs-lookup"><span data-stu-id="ff364-121">Raspberry Pi 3</span></span>
- <span data-ttu-id="ff364-122">MicroSD-kort (med NOOBS)</span><span class="sxs-lookup"><span data-stu-id="ff364-122">MicroSD Card (with NOOBS)</span></span>
- <span data-ttu-id="ff364-123">En Mini USB-kabel</span><span class="sxs-lookup"><span data-stu-id="ff364-123">A USB Mini cable</span></span>
- <span data-ttu-id="ff364-124">En Ethernet-kabel</span><span class="sxs-lookup"><span data-stu-id="ff364-124">An Ethernet cable</span></span>

[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/