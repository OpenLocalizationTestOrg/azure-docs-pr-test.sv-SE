## <a name="how-toocreate-a-classic-vnet-using-azure-cli"></a><span data-ttu-id="3de65-101">Hur toocreate ett klassiska VNet med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="3de65-101">How toocreate a classic VNet using Azure CLI</span></span>
<span data-ttu-id="3de65-102">Du kan använda hello Azure CLI toomanage Azure-resurser från hello Kommandotolken från valfri dator som kör Windows, Linux eller OSX.</span><span class="sxs-lookup"><span data-stu-id="3de65-102">You can use hello Azure CLI toomanage your Azure resources from hello command prompt from any computer running Windows, Linux, or OSX.</span></span> <span data-ttu-id="3de65-103">toocreate ett VNet med hjälp av hello Azure CLI, följ hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="3de65-103">toocreate a VNet by using hello Azure CLI, follow hello steps below.</span></span>

1. <span data-ttu-id="3de65-104">Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](../articles/cli-install-nodejs.md) och följer instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3de65-104">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../articles/cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="3de65-105">Kör hello **azure network vnet skapa** kommandot toocreate ett VNet och undernät, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="3de65-105">Run hello **azure network vnet create** command toocreate a VNet and a subnet, as shown below.</span></span> <span data-ttu-id="3de65-106">hello-listan som visas efter hello utdata förklarar hello parametrar som används.</span><span class="sxs-lookup"><span data-stu-id="3de65-106">hello list shown after hello output explains hello parameters used.</span></span>
   
            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
   
    <span data-ttu-id="3de65-107">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="3de65-107">Expected output:</span></span>
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK
   
   * <span data-ttu-id="3de65-108">**--vnet**.</span><span class="sxs-lookup"><span data-stu-id="3de65-108">**--vnet**.</span></span> <span data-ttu-id="3de65-109">Namnet på hello VNet toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="3de65-109">Name of hello VNet toobe created.</span></span> <span data-ttu-id="3de65-110">I vårt exempel, *TestVNet*</span><span class="sxs-lookup"><span data-stu-id="3de65-110">For our scenario, *TestVNet*</span></span>
   * <span data-ttu-id="3de65-111">**-e (eller---adressutrymmet)**.</span><span class="sxs-lookup"><span data-stu-id="3de65-111">**-e (or --address-space)**.</span></span> <span data-ttu-id="3de65-112">VNet-adressutrymmet.</span><span class="sxs-lookup"><span data-stu-id="3de65-112">VNet address space.</span></span> <span data-ttu-id="3de65-113">I vårt scenario, *192.168.0.0*</span><span class="sxs-lookup"><span data-stu-id="3de65-113">For our scenario, *192.168.0.0*</span></span>
   * <span data-ttu-id="3de65-114">**-i (eller - cidr)**.</span><span class="sxs-lookup"><span data-stu-id="3de65-114">**-i (or -cidr)**.</span></span> <span data-ttu-id="3de65-115">Nätverksmasken i CIDR-format.</span><span class="sxs-lookup"><span data-stu-id="3de65-115">Network mask in CIDR format.</span></span> <span data-ttu-id="3de65-116">I vårt scenario, *16*.</span><span class="sxs-lookup"><span data-stu-id="3de65-116">For our scenario, *16*.</span></span>
   * <span data-ttu-id="3de65-117">**-n (eller--undernätsnamn**).</span><span class="sxs-lookup"><span data-stu-id="3de65-117">**-n (or --subnet-name**).</span></span> <span data-ttu-id="3de65-118">Namnet på hello första undernätet.</span><span class="sxs-lookup"><span data-stu-id="3de65-118">Name of hello first subnet.</span></span> <span data-ttu-id="3de65-119">I vårt scenario, *FrontEnd*.</span><span class="sxs-lookup"><span data-stu-id="3de65-119">For our scenario, *FrontEnd*.</span></span>
   * <span data-ttu-id="3de65-120">**-p (eller--undernät-start-ip)**.</span><span class="sxs-lookup"><span data-stu-id="3de65-120">**-p (or --subnet-start-ip)**.</span></span> <span data-ttu-id="3de65-121">Första IP-adressen för undernätet eller undernätsadressutrymmet.</span><span class="sxs-lookup"><span data-stu-id="3de65-121">Starting IP address for subnet, or subnet address space.</span></span> <span data-ttu-id="3de65-122">I vårt scenario, *192.168.1.0*.</span><span class="sxs-lookup"><span data-stu-id="3de65-122">For our scenario, *192.168.1.0*.</span></span>
   * <span data-ttu-id="3de65-123">**-r (eller--undernät cidr)**.</span><span class="sxs-lookup"><span data-stu-id="3de65-123">**-r (or --subnet-cidr)**.</span></span> <span data-ttu-id="3de65-124">Nätverksmasken i CIDR-format för undernätet.</span><span class="sxs-lookup"><span data-stu-id="3de65-124">Network mask in CIDR format for subnet.</span></span> <span data-ttu-id="3de65-125">I vårt scenario, *24*.</span><span class="sxs-lookup"><span data-stu-id="3de65-125">For our scenario, *24*.</span></span>
   * <span data-ttu-id="3de65-126">**-l (eller --location)**.</span><span class="sxs-lookup"><span data-stu-id="3de65-126">**-l (or --location)**.</span></span> <span data-ttu-id="3de65-127">Azure-region där hello VNet kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="3de65-127">Azure region where hello VNet will be created.</span></span> <span data-ttu-id="3de65-128">I vårt scenario, *centrala USA*.</span><span class="sxs-lookup"><span data-stu-id="3de65-128">For our scenario, *Central US*.</span></span>
3. <span data-ttu-id="3de65-129">Kör hello **azure network vnet subnet skapa** kommandot toocreate ett undernät som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="3de65-129">Run hello **azure network vnet subnet create** command toocreate a subnet as shown below.</span></span> <span data-ttu-id="3de65-130">hello-listan som visas efter hello utdata förklarar hello parametrar som används.</span><span class="sxs-lookup"><span data-stu-id="3de65-130">hello list shown after hello output explains hello parameters used.</span></span>
   
            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
   
    <span data-ttu-id="3de65-131">Här är förväntat hello utdata för hello ovanstående kommando:</span><span class="sxs-lookup"><span data-stu-id="3de65-131">Here is hello expected output for hello command above:</span></span>
   
            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up hello subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK
   
   * <span data-ttu-id="3de65-132">**-t (eller--vnet-name**.</span><span class="sxs-lookup"><span data-stu-id="3de65-132">**-t (or --vnet-name**.</span></span> <span data-ttu-id="3de65-133">Namnet på hello VNet där undernätet hello kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="3de65-133">Name of hello VNet where hello subnet will be created.</span></span> <span data-ttu-id="3de65-134">I vårt scenario, *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="3de65-134">For our scenario, *TestVNet*.</span></span>
   * <span data-ttu-id="3de65-135">**-n (eller --name)**.</span><span class="sxs-lookup"><span data-stu-id="3de65-135">**-n (or --name)**.</span></span> <span data-ttu-id="3de65-136">Namnet på hello Nytt undernät.</span><span class="sxs-lookup"><span data-stu-id="3de65-136">Name of hello new subnet.</span></span> <span data-ttu-id="3de65-137">I vårt scenario, *BackEnd*.</span><span class="sxs-lookup"><span data-stu-id="3de65-137">For our scenario, *BackEnd*.</span></span>
   * <span data-ttu-id="3de65-138">**-a (eller --address-prefix)**.</span><span class="sxs-lookup"><span data-stu-id="3de65-138">**-a (or --address-prefix)**.</span></span> <span data-ttu-id="3de65-139">CIDR-block för undernätet.</span><span class="sxs-lookup"><span data-stu-id="3de65-139">Subnet CIDR block.</span></span> <span data-ttu-id="3de65-140">Fyra vårt scenario, *192.168.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="3de65-140">Four our scenario, *192.168.2.0/24*.</span></span>
4. <span data-ttu-id="3de65-141">Kör hello **azure network vnet show** kommandot tooview hello egenskaper för hello nya vnet, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="3de65-141">Run hello **azure network vnet show** command tooview hello properties of hello new vnet, as shown below.</span></span>
   
            azure network vnet show
   
    <span data-ttu-id="3de65-142">Här är förväntat hello utdata för hello ovanstående kommando:</span><span class="sxs-lookup"><span data-stu-id="3de65-142">Here is hello expected output for hello command above:</span></span>
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up hello virtual network sites
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

