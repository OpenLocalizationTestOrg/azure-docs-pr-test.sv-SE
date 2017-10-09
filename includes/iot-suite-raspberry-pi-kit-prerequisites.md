## <a name="prerequisites"></a><span data-ttu-id="072b6-101">Krav</span><span class="sxs-lookup"><span data-stu-id="072b6-101">Prerequisites</span></span>

<span data-ttu-id="072b6-102">toocomplete den här självstudiekursen kommer du behöver en aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="072b6-102">toocomplete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="072b6-103">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="072b6-103">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="072b6-104">Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="072b6-104">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

### <a name="required-software"></a><span data-ttu-id="072b6-105">Programvara som krävs</span><span class="sxs-lookup"><span data-stu-id="072b6-105">Required software</span></span>

<span data-ttu-id="072b6-106">Du måste SSH-klienten på den stationära datorn tooenable du tooremotely access hello-kommando rad på hello hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="072b6-106">You need SSH client on your desktop machine tooenable you tooremotely access hello command line on hello Raspberry Pi.</span></span>

- <span data-ttu-id="072b6-107">Windows innehåller inte en SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="072b6-107">Windows does not include an SSH client.</span></span> <span data-ttu-id="072b6-108">Vi rekommenderar att du använder [PuTTY](http://www.putty.org/).</span><span class="sxs-lookup"><span data-stu-id="072b6-108">We recommend using [PuTTY](http://www.putty.org/).</span></span>
- <span data-ttu-id="072b6-109">De flesta distributioner för Linux och Mac OS inkluderar hello SSH-kommandoradsverktyget.</span><span class="sxs-lookup"><span data-stu-id="072b6-109">Most Linux distributions and Mac OS include hello command-line SSH utility.</span></span> <span data-ttu-id="072b6-110">Mer information finns i [SSH med Linux- eller Mac OS x](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span><span class="sxs-lookup"><span data-stu-id="072b6-110">For more information, see [SSH Using Linux or Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span></span>

### <a name="required-hardware"></a><span data-ttu-id="072b6-111">Nödvändig maskinvara</span><span class="sxs-lookup"><span data-stu-id="072b6-111">Required hardware</span></span>

<span data-ttu-id="072b6-112">En stationär dator tooenable du tooconnect via fjärranslutning toohello kommandot rad på hello hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="072b6-112">A desktop computer tooenable you tooconnect remotely toohello command line on hello Raspberry Pi.</span></span>

<span data-ttu-id="072b6-113">[Microsoft IoT startpaket för hallon Pi 3] [ lnk-starter-kits] eller motsvarande komponenter.</span><span class="sxs-lookup"><span data-stu-id="072b6-113">[Microsoft IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] or equivalent components.</span></span> <span data-ttu-id="072b6-114">Den här kursen använder följande objekt från hello kit hello:</span><span class="sxs-lookup"><span data-stu-id="072b6-114">This tutorial uses hello following items from hello kit:</span></span>

- <span data-ttu-id="072b6-115">Raspberry Pi 3</span><span class="sxs-lookup"><span data-stu-id="072b6-115">Raspberry Pi 3</span></span>
- <span data-ttu-id="072b6-116">MicroSD-kort (med NOOBS)</span><span class="sxs-lookup"><span data-stu-id="072b6-116">MicroSD Card (with NOOBS)</span></span>
- <span data-ttu-id="072b6-117">En Mini USB-kabel</span><span class="sxs-lookup"><span data-stu-id="072b6-117">A USB Mini cable</span></span>
- <span data-ttu-id="072b6-118">En Ethernet-kabel</span><span class="sxs-lookup"><span data-stu-id="072b6-118">An Ethernet cable</span></span>
- <span data-ttu-id="072b6-119">BME280 sensor</span><span class="sxs-lookup"><span data-stu-id="072b6-119">BME280 sensor</span></span>
- <span data-ttu-id="072b6-120">Breadboard</span><span class="sxs-lookup"><span data-stu-id="072b6-120">Breadboard</span></span>
- <span data-ttu-id="072b6-121">Omkopplare kablar</span><span class="sxs-lookup"><span data-stu-id="072b6-121">Jumper wires</span></span>
- <span data-ttu-id="072b6-122">Motstånd</span><span class="sxs-lookup"><span data-stu-id="072b6-122">Resistors</span></span>
- <span data-ttu-id="072b6-123">Indikatorer</span><span class="sxs-lookup"><span data-stu-id="072b6-123">LEDs</span></span>

[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/