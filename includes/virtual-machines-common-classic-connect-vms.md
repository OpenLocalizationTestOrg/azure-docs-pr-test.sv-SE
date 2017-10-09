

![Virtuella datorer i en fristående molntjänst](./media/virtual-machines-common-classic-connect-vms/CloudServiceExample.png)

<span data-ttu-id="fbd32-102">Om du placerar dina virtuella datorer i ett virtuellt nätverk, kan du bestämma hur många molntjänster som du vill använda toouse för att läsa in belastningsutjämning och tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="fbd32-102">If you place your virtual machines in a virtual network, you can decide how many cloud services you want toouse for load balancing and availability sets.</span></span> <span data-ttu-id="fbd32-103">Dessutom kan du ordna hello virtuella datorer på undernät i hello samma sätt som ditt lokala nätverk och ansluta hello virtuellt nätverk tooyour lokalt nätverk.</span><span class="sxs-lookup"><span data-stu-id="fbd32-103">Additionally, you can organize hello virtual machines on subnets in hello same way as your on-premises network and connect hello virtual network tooyour on-premises network.</span></span> <span data-ttu-id="fbd32-104">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="fbd32-104">Here's an example:</span></span>

![Virtuella datorer i ett virtuellt nätverk](./media/virtual-machines-common-classic-connect-vms/VirtualNetworkExample.png)

<span data-ttu-id="fbd32-106">Virtuella nätverken är hello rekommenderat sätt tooconnect virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="fbd32-106">Virtual networks are hello recommended way tooconnect virtual machines in Azure.</span></span> <span data-ttu-id="fbd32-107">Hej bästa praxis är tooconfigure varje nivå av ditt program i en separat molntjänst.</span><span class="sxs-lookup"><span data-stu-id="fbd32-107">hello best practice is tooconfigure each tier of your application in a separate cloud service.</span></span> <span data-ttu-id="fbd32-108">Men du kanske måste toocombine vissa virtuella datorer från olika program nivåer i hello samma cloud service tooremain inom hello högst 200 molntjänster per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="fbd32-108">However, you may need toocombine some virtual machines from different application tiers into hello same cloud service tooremain within hello maximum of 200 cloud services per subscription.</span></span> <span data-ttu-id="fbd32-109">tooreview detta och andra begränsningar, finns [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../articles/azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="fbd32-109">tooreview this and other limits, see [Azure Subscription and Service Limits, Quotas, and Constraints](../articles/azure-subscription-service-limits.md).</span></span>

## <a name="connect-vms-in-a-virtual-network"></a><span data-ttu-id="fbd32-110">Ansluta virtuella datorer i ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="fbd32-110">Connect VMs in a virtual network</span></span>
<span data-ttu-id="fbd32-111">tooconnect virtuella datorer i ett virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="fbd32-111">tooconnect virtual machines in a virtual network:</span></span>

1. <span data-ttu-id="fbd32-112">Skapa hello virtuellt nätverk i hello [Azure-portalen](../articles/virtual-network/virtual-networks-create-vnet-classic-pportal.md) och ange 'klassisk distribution ”.</span><span class="sxs-lookup"><span data-stu-id="fbd32-112">Create hello virtual network in hello [Azure portal](../articles/virtual-network/virtual-networks-create-vnet-classic-pportal.md) and specify 'classic deployment'.</span></span>
2. <span data-ttu-id="fbd32-113">Skapa hello molntjänster för din distribution tooreflect din design för tillgänglighetsuppsättningar och belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="fbd32-113">Create hello set of cloud services for your deployment tooreflect your design for availability sets and load balancing.</span></span> <span data-ttu-id="fbd32-114">I hello Azure-portalen klickar du på **New > Compute > molntjänst** för varje tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="fbd32-114">In hello Azure portal, click **New > Compute > Cloud service** for each cloud service.</span></span>

  <span data-ttu-id="fbd32-115">När du fyller i hello molnet tjänstinformation väljer hello samma _resursgruppen_ används med hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="fbd32-115">As you fill out hello cloud service details, choose hello same _resource group_ used with hello virtual network.</span></span>

3. <span data-ttu-id="fbd32-116">toocreate varje nya virtuella datorn, klickar du på **New > Compute**, och sedan väljer hello lämpliga VM-avbildning från hello **aktuella appar**.</span><span class="sxs-lookup"><span data-stu-id="fbd32-116">toocreate each new virtual machine, click **New > Compute**, then select hello appropriate VM image from hello **Featured apps**.</span></span>

  <span data-ttu-id="fbd32-117">I hello VM **grunderna** bladet välj hello samma _resursgruppen_ används med hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="fbd32-117">In hello VM **Basics** blade, choose hello same _resource group_ used with hello virtual network.</span></span>

  ![Grunderna i VM-bladet när du använder ett virtuellt nätverk](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_VN.png)

4. <span data-ttu-id="fbd32-119">När du fyller i hello VM **inställningar**, Välj rätt hello _Molntjänsten_ eller _virtuellt nätverk_ för hello VM.</span><span class="sxs-lookup"><span data-stu-id="fbd32-119">As you fill out hello VM **Settings**, choose hello correct _Cloud service_ or _virtual network_ for hello VM.</span></span>

  <span data-ttu-id="fbd32-120">Azure markerar hello andra objekt baserat på ditt val.</span><span class="sxs-lookup"><span data-stu-id="fbd32-120">Azure will select hello other item based on your selection.</span></span>

  ![VM inställningsbladet när du använder ett virtuellt nätverk](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_VN.png)


## <a name="connect-vms-in-a-standalone-cloud-service"></a><span data-ttu-id="fbd32-122">Ansluta virtuella datorer i en fristående molntjänst</span><span class="sxs-lookup"><span data-stu-id="fbd32-122">Connect VMs in a standalone cloud service</span></span>
<span data-ttu-id="fbd32-123">tooconnect virtuella datorer i en fristående molnbaserad tjänst:</span><span class="sxs-lookup"><span data-stu-id="fbd32-123">tooconnect virtual machines in a standalone cloud service:</span></span>

1. <span data-ttu-id="fbd32-124">Skapa hello Molntjänsten i hello [Azure-portalen](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fbd32-124">Create hello cloud service in hello [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="fbd32-125">Klicka på **New > Compute > molntjänst**.</span><span class="sxs-lookup"><span data-stu-id="fbd32-125">Click **New > Compute > Cloud service**.</span></span> <span data-ttu-id="fbd32-126">Eller, du kan skapa hello Molntjänsten för din distribution när du skapar din första virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="fbd32-126">Or, you can create hello cloud service for your deployment when you create your first virtual machine.</span></span>
2. <span data-ttu-id="fbd32-127">När du skapar hello virtuella datorer väljer du hello samma resursgrupp som används med hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="fbd32-127">When you create hello virtual machines, choose hello same resource group used with hello cloud service.</span></span>

  ![Lägg till en virtuell dator tooan befintlig molntjänst](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_SA.png)

3.  <span data-ttu-id="fbd32-129">När du fyller i hello VM information Välj hello namnet på Molntjänsten som skapats i hello första steget.</span><span class="sxs-lookup"><span data-stu-id="fbd32-129">As you fill out hello VM details, choose hello name of cloud service created in hello first step.</span></span>

  ![Att välja en tjänst i molnet för en virtuell dator](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_SA.png)
