## <a name="how-to-create-a-classic-vnet-using-azure-cli"></a><span data-ttu-id="4094b-101">Så här skapar du ett klassiska VNet med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="4094b-101">How to create a classic VNet using Azure CLI</span></span>
<span data-ttu-id="4094b-102">Du kan använda Azure CLI för att hantera dina Azure-resurser från kommandotolken från valfri dator som kör Windows, Linux eller OSX.</span><span class="sxs-lookup"><span data-stu-id="4094b-102">You can use the Azure CLI to manage your Azure resources from the command prompt from any computer running Windows, Linux, or OSX.</span></span> <span data-ttu-id="4094b-103">Följ stegen nedan för att skapa ett VNet med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="4094b-103">To create a VNet by using the Azure CLI, follow the steps below.</span></span>

1. <span data-ttu-id="4094b-104">Om du aldrig har använt Azure CLI, se [installera och konfigurera Azure CLI](../articles/cli-install-nodejs.md) och följ instruktionerna upp till den punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4094b-104">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../articles/cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="4094b-105">Kör kommandot **azure network vnet create** för att skapa en VNet och ett undernät som det visas nedan.</span><span class="sxs-lookup"><span data-stu-id="4094b-105">Run the **azure network vnet create** command to create a VNet and a subnet, as shown below.</span></span> <span data-ttu-id="4094b-106">Listan som visas efter alla utdata förklarar parametrarna som använts.</span><span class="sxs-lookup"><span data-stu-id="4094b-106">The list shown after the output explains the parameters used.</span></span>
   
            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
   
    <span data-ttu-id="4094b-107">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="4094b-107">Expected output:</span></span>
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK
   
   * <span data-ttu-id="4094b-108">**--vnet**.</span><span class="sxs-lookup"><span data-stu-id="4094b-108">**--vnet**.</span></span> <span data-ttu-id="4094b-109">Namnet på den VNet som ska skapas.</span><span class="sxs-lookup"><span data-stu-id="4094b-109">Name of the VNet to be created.</span></span> <span data-ttu-id="4094b-110">I vårt exempel, *TestVNet*</span><span class="sxs-lookup"><span data-stu-id="4094b-110">For our scenario, *TestVNet*</span></span>
   * <span data-ttu-id="4094b-111">**-e (eller---adressutrymmet)**.</span><span class="sxs-lookup"><span data-stu-id="4094b-111">**-e (or --address-space)**.</span></span> <span data-ttu-id="4094b-112">VNet-adressutrymmet.</span><span class="sxs-lookup"><span data-stu-id="4094b-112">VNet address space.</span></span> <span data-ttu-id="4094b-113">I vårt scenario, *192.168.0.0*</span><span class="sxs-lookup"><span data-stu-id="4094b-113">For our scenario, *192.168.0.0*</span></span>
   * <span data-ttu-id="4094b-114">**-i (eller - cidr)**.</span><span class="sxs-lookup"><span data-stu-id="4094b-114">**-i (or -cidr)**.</span></span> <span data-ttu-id="4094b-115">Nätverksmasken i CIDR-format.</span><span class="sxs-lookup"><span data-stu-id="4094b-115">Network mask in CIDR format.</span></span> <span data-ttu-id="4094b-116">I vårt scenario, *16*.</span><span class="sxs-lookup"><span data-stu-id="4094b-116">For our scenario, *16*.</span></span>
   * <span data-ttu-id="4094b-117">**-n (eller--undernätsnamn**).</span><span class="sxs-lookup"><span data-stu-id="4094b-117">**-n (or --subnet-name**).</span></span> <span data-ttu-id="4094b-118">Namnet på det första undernätet.</span><span class="sxs-lookup"><span data-stu-id="4094b-118">Name of the first subnet.</span></span> <span data-ttu-id="4094b-119">I vårt scenario, *FrontEnd*.</span><span class="sxs-lookup"><span data-stu-id="4094b-119">For our scenario, *FrontEnd*.</span></span>
   * <span data-ttu-id="4094b-120">**-p (eller--undernät-start-ip)**.</span><span class="sxs-lookup"><span data-stu-id="4094b-120">**-p (or --subnet-start-ip)**.</span></span> <span data-ttu-id="4094b-121">Första IP-adressen för undernätet eller undernätsadressutrymmet.</span><span class="sxs-lookup"><span data-stu-id="4094b-121">Starting IP address for subnet, or subnet address space.</span></span> <span data-ttu-id="4094b-122">I vårt scenario, *192.168.1.0*.</span><span class="sxs-lookup"><span data-stu-id="4094b-122">For our scenario, *192.168.1.0*.</span></span>
   * <span data-ttu-id="4094b-123">**-r (eller--undernät cidr)**.</span><span class="sxs-lookup"><span data-stu-id="4094b-123">**-r (or --subnet-cidr)**.</span></span> <span data-ttu-id="4094b-124">Nätverksmasken i CIDR-format för undernätet.</span><span class="sxs-lookup"><span data-stu-id="4094b-124">Network mask in CIDR format for subnet.</span></span> <span data-ttu-id="4094b-125">I vårt scenario, *24*.</span><span class="sxs-lookup"><span data-stu-id="4094b-125">For our scenario, *24*.</span></span>
   * <span data-ttu-id="4094b-126">**-l (eller --location)**.</span><span class="sxs-lookup"><span data-stu-id="4094b-126">**-l (or --location)**.</span></span> <span data-ttu-id="4094b-127">Azure-region där VNet kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="4094b-127">Azure region where the VNet will be created.</span></span> <span data-ttu-id="4094b-128">I vårt scenario, *centrala USA*.</span><span class="sxs-lookup"><span data-stu-id="4094b-128">For our scenario, *Central US*.</span></span>
3. <span data-ttu-id="4094b-129">Kör kommandot **azure network vnet subnet create** för att skapa ett undernät som det visas nedan.</span><span class="sxs-lookup"><span data-stu-id="4094b-129">Run the **azure network vnet subnet create** command to create a subnet as shown below.</span></span> <span data-ttu-id="4094b-130">Listan som visas efter alla utdata förklarar parametrarna som använts.</span><span class="sxs-lookup"><span data-stu-id="4094b-130">The list shown after the output explains the parameters used.</span></span>
   
            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
   
    <span data-ttu-id="4094b-131">Följande utdata förväntas från kommandot ovan:</span><span class="sxs-lookup"><span data-stu-id="4094b-131">Here is the expected output for the command above:</span></span>
   
            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK
   
   * <span data-ttu-id="4094b-132">**-t (eller--vnet-name**.</span><span class="sxs-lookup"><span data-stu-id="4094b-132">**-t (or --vnet-name**.</span></span> <span data-ttu-id="4094b-133">Namnet på VNet där undernätet kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="4094b-133">Name of the VNet where the subnet will be created.</span></span> <span data-ttu-id="4094b-134">I vårt scenario, *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="4094b-134">For our scenario, *TestVNet*.</span></span>
   * <span data-ttu-id="4094b-135">**-n (eller --name)**.</span><span class="sxs-lookup"><span data-stu-id="4094b-135">**-n (or --name)**.</span></span> <span data-ttu-id="4094b-136">Namnet på det nya undernätet.</span><span class="sxs-lookup"><span data-stu-id="4094b-136">Name of the new subnet.</span></span> <span data-ttu-id="4094b-137">I vårt scenario, *BackEnd*.</span><span class="sxs-lookup"><span data-stu-id="4094b-137">For our scenario, *BackEnd*.</span></span>
   * <span data-ttu-id="4094b-138">**-a (eller --address-prefix)**.</span><span class="sxs-lookup"><span data-stu-id="4094b-138">**-a (or --address-prefix)**.</span></span> <span data-ttu-id="4094b-139">CIDR-block för undernätet.</span><span class="sxs-lookup"><span data-stu-id="4094b-139">Subnet CIDR block.</span></span> <span data-ttu-id="4094b-140">Fyra vårt scenario, *192.168.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="4094b-140">Four our scenario, *192.168.2.0/24*.</span></span>
4. <span data-ttu-id="4094b-141">Kör kommandot **azure network vnet show** för att visa egenskaperna för den nya VNet som det visas nedan.</span><span class="sxs-lookup"><span data-stu-id="4094b-141">Run the **azure network vnet show** command to view the properties of the new vnet, as shown below.</span></span>
   
            azure network vnet show
   
    <span data-ttu-id="4094b-142">Följande utdata förväntas från kommandot ovan:</span><span class="sxs-lookup"><span data-stu-id="4094b-142">Here is the expected output for the command above:</span></span>
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK

