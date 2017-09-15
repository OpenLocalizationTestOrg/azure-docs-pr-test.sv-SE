

![Virtuella datorer i en fristående molntjänst](./media/virtual-machines-common-classic-connect-vms/CloudServiceExample.png)

<span data-ttu-id="d8389-102">Om du placerar dina virtuella datorer i ett virtuellt nätverk, kan du bestämma hur många molntjänster som du vill använda för att läsa in belastningsutjämning och tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="d8389-102">If you place your virtual machines in a virtual network, you can decide how many cloud services you want to use for load balancing and availability sets.</span></span> <span data-ttu-id="d8389-103">Du kan dessutom sortera de virtuella datorerna på undernät på samma sätt som ditt lokala nätverk och ansluta det virtuella nätverket till ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="d8389-103">Additionally, you can organize the virtual machines on subnets in the same way as your on-premises network and connect the virtual network to your on-premises network.</span></span> <span data-ttu-id="d8389-104">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="d8389-104">Here's an example:</span></span>

![Virtuella datorer i ett virtuellt nätverk](./media/virtual-machines-common-classic-connect-vms/VirtualNetworkExample.png)

<span data-ttu-id="d8389-106">Virtuella nätverk är det rekommenderade sättet att ansluta virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="d8389-106">Virtual networks are the recommended way to connect virtual machines in Azure.</span></span> <span data-ttu-id="d8389-107">Det bästa sättet är att konfigurera varje nivå av ditt program i en separat molntjänst.</span><span class="sxs-lookup"><span data-stu-id="d8389-107">The best practice is to configure each tier of your application in a separate cloud service.</span></span> <span data-ttu-id="d8389-108">Du kan dock behöva kombinera vissa virtuella datorer från olika program nivåer i samma molntjänst förblir inom högst 200 molntjänster per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d8389-108">However, you may need to combine some virtual machines from different application tiers into the same cloud service to remain within the maximum of 200 cloud services per subscription.</span></span> <span data-ttu-id="d8389-109">Det här och andra begränsningar finns [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../articles/azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="d8389-109">To review this and other limits, see [Azure Subscription and Service Limits, Quotas, and Constraints](../articles/azure-subscription-service-limits.md).</span></span>

## <a name="connect-vms-in-a-virtual-network"></a><span data-ttu-id="d8389-110">Ansluta virtuella datorer i ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="d8389-110">Connect VMs in a virtual network</span></span>
<span data-ttu-id="d8389-111">Att ansluta virtuella datorer i ett virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="d8389-111">To connect virtual machines in a virtual network:</span></span>

1. <span data-ttu-id="d8389-112">Skapa virtuella nätverk i den [Azure-portalen](../articles/virtual-network/virtual-networks-create-vnet-classic-pportal.md) och ange 'klassisk distribution ”.</span><span class="sxs-lookup"><span data-stu-id="d8389-112">Create the virtual network in the [Azure portal](../articles/virtual-network/virtual-networks-create-vnet-classic-pportal.md) and specify 'classic deployment'.</span></span>
2. <span data-ttu-id="d8389-113">Skapa en uppsättning molntjänster för distributionen för att avspegla din design för tillgänglighetsuppsättningar och belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="d8389-113">Create the set of cloud services for your deployment to reflect your design for availability sets and load balancing.</span></span> <span data-ttu-id="d8389-114">I Azure-portalen klickar du på **New > Compute > molntjänst** för varje tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="d8389-114">In the Azure portal, click **New > Compute > Cloud service** for each cloud service.</span></span>

  <span data-ttu-id="d8389-115">När du fyller i molnet tjänstinformation Välj samma _resursgruppen_ användas med det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="d8389-115">As you fill out the cloud service details, choose the same _resource group_ used with the virtual network.</span></span>

3. <span data-ttu-id="d8389-116">Klicka för att skapa varje ny virtuell dator **New > Compute**, Välj lämplig VM-avbildning från den **aktuella appar**.</span><span class="sxs-lookup"><span data-stu-id="d8389-116">To create each new virtual machine, click **New > Compute**, then select the appropriate VM image from the **Featured apps**.</span></span>

  <span data-ttu-id="d8389-117">I Virtuellt **grunderna** bladet Välj samma _resursgruppen_ användas med det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="d8389-117">In the VM **Basics** blade, choose the same _resource group_ used with the virtual network.</span></span>

  ![Grunderna i VM-bladet när du använder ett virtuellt nätverk](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_VN.png)

4. <span data-ttu-id="d8389-119">När du fyller i den virtuella datorn **inställningar**, Välj rätt _Molntjänsten_ eller _virtuellt nätverk_ för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d8389-119">As you fill out the VM **Settings**, choose the correct _Cloud service_ or _virtual network_ for the VM.</span></span>

  <span data-ttu-id="d8389-120">Azure markerar det andra objektet baserat på ditt val.</span><span class="sxs-lookup"><span data-stu-id="d8389-120">Azure will select the other item based on your selection.</span></span>

  ![VM inställningsbladet när du använder ett virtuellt nätverk](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_VN.png)


## <a name="connect-vms-in-a-standalone-cloud-service"></a><span data-ttu-id="d8389-122">Ansluta virtuella datorer i en fristående molntjänst</span><span class="sxs-lookup"><span data-stu-id="d8389-122">Connect VMs in a standalone cloud service</span></span>
<span data-ttu-id="d8389-123">Att ansluta virtuella datorer i en fristående molnbaserad tjänst:</span><span class="sxs-lookup"><span data-stu-id="d8389-123">To connect virtual machines in a standalone cloud service:</span></span>

1. <span data-ttu-id="d8389-124">Skapa molntjänst i den [Azure-portalen](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d8389-124">Create the cloud service in the [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="d8389-125">Klicka på **New > Compute > molntjänst**.</span><span class="sxs-lookup"><span data-stu-id="d8389-125">Click **New > Compute > Cloud service**.</span></span> <span data-ttu-id="d8389-126">Eller, du kan skapa Molntjänsten för din distribution när du skapar din första virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d8389-126">Or, you can create the cloud service for your deployment when you create your first virtual machine.</span></span>
2. <span data-ttu-id="d8389-127">När du skapar virtuella datorer väljer du samma resursgrupp som används med Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="d8389-127">When you create the virtual machines, choose the same resource group used with the cloud service.</span></span>

  ![Lägga till en virtuell dator i en befintlig molntjänst](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_SA.png)

3.  <span data-ttu-id="d8389-129">Välj namnet på Molntjänsten som skapats i det första steget när du fyller i VM-information.</span><span class="sxs-lookup"><span data-stu-id="d8389-129">As you fill out the VM details, choose the name of cloud service created in the first step.</span></span>

  ![Att välja en tjänst i molnet för en virtuell dator](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_SA.png)
