### <span data-ttu-id="0ccc8-101"><a name="noconnection"></a>toomodify lokala nätverket gateway IP-adressprefix - ingen gateway-anslutningen</span><span class="sxs-lookup"><span data-stu-id="0ccc8-101"><a name="noconnection"></a>toomodify local network gateway IP address prefixes - no gateway connection</span></span>

#### <a name="tooadd-additional-address-prefixes"></a><span data-ttu-id="0ccc8-102">tooadd ytterligare adressprefix:</span><span class="sxs-lookup"><span data-stu-id="0ccc8-102">tooadd additional address prefixes:</span></span>

1. <span data-ttu-id="0ccc8-103">På hello lokala nätverksgateway resurs i hello **inställningar** klickar du på **Configuration**.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-103">On hello Local Network Gateway resource, in hello **Settings** section, click **Configuration**.</span></span>
2. <span data-ttu-id="0ccc8-104">Lägg till hello IP-adressutrymme i hello *Lägg till ytterligare adressintervall* rutan.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-104">Add hello IP address space in hello *Add additional address range* box.</span></span>
3. <span data-ttu-id="0ccc8-105">Klicka på **spara** toosave dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-105">Click **Save** toosave your settings.</span></span>

#### <a name="tooremove-address-prefixes"></a><span data-ttu-id="0ccc8-106">tooremove adressprefix:</span><span class="sxs-lookup"><span data-stu-id="0ccc8-106">tooremove address prefixes:</span></span>

1. <span data-ttu-id="0ccc8-107">På hello lokala nätverksgateway resurs i hello **inställningar** klickar du på **Configuration**.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-107">On hello Local Network Gateway resource, in hello **Settings** section, click **Configuration**.</span></span>
2. <span data-ttu-id="0ccc8-108">Klicka på hello **”...”** hello rad som innehåller hello prefix du vill tooremove.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-108">Click hello **'...'** on hello line containing hello prefix you want tooremove.</span></span>
3. <span data-ttu-id="0ccc8-109">Klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-109">Click **Remove**.</span></span>
4. <span data-ttu-id="0ccc8-110">Klicka på **spara** toosave dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-110">Click **Save** toosave your settings.</span></span>

### <span data-ttu-id="0ccc8-111"><a name="withconnection"></a>toomodify lokala nätverket gateway IP-adressprefix - befintlig gateway-anslutningen</span><span class="sxs-lookup"><span data-stu-id="0ccc8-111"><a name="withconnection"></a>toomodify local network gateway IP address prefixes - existing gateway connection</span></span>

<span data-ttu-id="0ccc8-112">Om du har en gateway-anslutning och vill tooadd eller ta bort hello IP-adressprefixet som ingår i din lokala nätverksgateway, behöver toodo hello följa stegen i ordning.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-112">If you have a gateway connection and want tooadd or remove hello IP address prefixes contained in your local network gateway, you need toodo hello following steps, in order.</span></span> <span data-ttu-id="0ccc8-113">Det medför en del avbrott för din VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-113">This results in some downtime for your VPN connection.</span></span> <span data-ttu-id="0ccc8-114">När du ändrar IP-adressprefix, behöver du inte toodelete hello VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-114">When modifying IP address prefixes, you don't need toodelete hello VPN gateway.</span></span> <span data-ttu-id="0ccc8-115">Du behöver bara tooremove hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-115">You only need tooremove hello connection.</span></span>

#### <a name="1-remove-hello-connection"></a><span data-ttu-id="0ccc8-116">1. Ta bort hello anslutningen.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-116">1. Remove hello connection.</span></span>

1. <span data-ttu-id="0ccc8-117">På hello lokala nätverksgateway resurs i hello **inställningar** klickar du på **anslutningar**.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-117">On hello Local Network Gateway resource, in hello **Settings** section, click **Connections**.</span></span>
2. <span data-ttu-id="0ccc8-118">Klicka på hello **...**  på hello rad för varje anslutning, klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-118">Click hello **...** on hello line for each connection, then click **Delete**.</span></span>
3. <span data-ttu-id="0ccc8-119">Klicka på **spara** toosave dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-119">Click **Save** toosave your settings.</span></span>

#### <a name="2-modify-hello-address-prefixes"></a><span data-ttu-id="0ccc8-120">2. Ändra hello-adressprefix.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-120">2. Modify hello address prefixes.</span></span>

<span data-ttu-id="0ccc8-121">tooadd ytterligare adressprefix:</span><span class="sxs-lookup"><span data-stu-id="0ccc8-121">tooadd additional address prefixes:</span></span>

1. <span data-ttu-id="0ccc8-122">På hello lokala nätverksgateway resurs i hello **inställningar** klickar du på **Configuration**.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-122">On hello Local Network Gateway resource, in hello **Settings** section, click **Configuration**.</span></span>
2. <span data-ttu-id="0ccc8-123">Lägg till hello IP-adressutrymme.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-123">Add hello IP address space.</span></span>
3. <span data-ttu-id="0ccc8-124">Klicka på **spara** toosave dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-124">Click **Save** toosave your settings.</span></span>

<span data-ttu-id="0ccc8-125">tooremove adressprefix:</span><span class="sxs-lookup"><span data-stu-id="0ccc8-125">tooremove address prefixes:</span></span>

1. <span data-ttu-id="0ccc8-126">På hello lokala nätverksgateway resurs i hello **inställningar** klickar du på **Configuration**.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-126">On hello Local Network Gateway resource, in hello **Settings** section, click **Configuration**.</span></span>
2. <span data-ttu-id="0ccc8-127">Klicka på hello **...**  hello rad som innehåller hello prefix du vill tooremove.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-127">Click hello **...** on hello line containing hello prefix you want tooremove.</span></span>
3. <span data-ttu-id="0ccc8-128">Klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-128">Click **Remove**.</span></span>
4. <span data-ttu-id="0ccc8-129">Klicka på **spara** toosave dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-129">Click **Save** toosave your settings.</span></span>

#### <a name="3-recreate-hello-connection"></a><span data-ttu-id="0ccc8-130">3. Återskapa hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-130">3. Recreate hello connection.</span></span>

1. <span data-ttu-id="0ccc8-131">Navigera toohello virtuell nätverksgateway för din VNet.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-131">Navigate toohello Virtual Network Gateway for your VNet.</span></span> <span data-ttu-id="0ccc8-132">(Inte hello lokala nätverksgateway kvar.)</span><span class="sxs-lookup"><span data-stu-id="0ccc8-132">(Not hello Local Network Gateway.)</span></span>
2. <span data-ttu-id="0ccc8-133">På hello virtuell nätverksgateway i hello **inställningar** klickar du på **anslutningar**.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-133">On hello Virtual Network Gateway, in hello **Settings** section, click **Connections**.</span></span>
3. <span data-ttu-id="0ccc8-134">Klicka på hello **+ Lägg till** tooopen hello **Lägg till anslutning** bladet.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-134">Click hello **+ Add** tooopen hello **Add connection** blade.</span></span>
4. <span data-ttu-id="0ccc8-135">Återskapa din anslutning.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-135">Recreate your connection.</span></span>
5. <span data-ttu-id="0ccc8-136">Klicka på **OK** toocreate hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="0ccc8-136">Click **OK** toocreate hello connection.</span></span>
