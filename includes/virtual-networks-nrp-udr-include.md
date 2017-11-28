## <a name="route-tables"></a><span data-ttu-id="50761-101">Vägtabeller</span><span class="sxs-lookup"><span data-stu-id="50761-101">Route tables</span></span>
<span data-ttu-id="50761-102">Väg tabellen resurser innehåller vägar som används för att definiera hur trafiken flödar i Azure-infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="50761-102">Route table resources contains routes used to define how traffic flows within your Azure infrastructure.</span></span> <span data-ttu-id="50761-103">Du kan använda användardefinierade vägar (UDR) för att skicka all trafik från ett visst undernät till en virtuell installation, till exempel en brandvägg eller intrångsidentifiering identifiering-system (ID).</span><span class="sxs-lookup"><span data-stu-id="50761-103">You can use user defined routes (UDR) to send all traffic from a given subnet to a virtual appliance, such as a firewall or intrusion detection system (IDS).</span></span> <span data-ttu-id="50761-104">Du kan associera en routingtabell till undernät.</span><span class="sxs-lookup"><span data-stu-id="50761-104">You can associate a route table to subnets.</span></span> 

<span data-ttu-id="50761-105">Routningstabeller innehåller följande egenskaper.</span><span class="sxs-lookup"><span data-stu-id="50761-105">Route tables contain the following properties.</span></span>

| <span data-ttu-id="50761-106">Egenskap</span><span class="sxs-lookup"><span data-stu-id="50761-106">Property</span></span> | <span data-ttu-id="50761-107">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="50761-107">Description</span></span> | <span data-ttu-id="50761-108">Exempelvärden</span><span class="sxs-lookup"><span data-stu-id="50761-108">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="50761-109">**vägar**</span><span class="sxs-lookup"><span data-stu-id="50761-109">**routes**</span></span> |<span data-ttu-id="50761-110">Samling användardefinierade vägar i routningstabellen</span><span class="sxs-lookup"><span data-stu-id="50761-110">Collection of user defined routes in the route table</span></span> |<span data-ttu-id="50761-111">Se [användardefinierade vägar](#User-defined-routes)</span><span class="sxs-lookup"><span data-stu-id="50761-111">see [user defined routes](#User-defined-routes)</span></span> |
| <span data-ttu-id="50761-112">**undernät**</span><span class="sxs-lookup"><span data-stu-id="50761-112">**subnets**</span></span> |<span data-ttu-id="50761-113">Samling av undernät i vägtabellen tillämpas på</span><span class="sxs-lookup"><span data-stu-id="50761-113">Collection of subnets the route table is applied to</span></span> |<span data-ttu-id="50761-114">Se [undernät](#Subnets)</span><span class="sxs-lookup"><span data-stu-id="50761-114">see [subnets](#Subnets)</span></span> |

### <a name="user-defined-routes"></a><span data-ttu-id="50761-115">Användardefinierade vägar</span><span class="sxs-lookup"><span data-stu-id="50761-115">User defined routes</span></span>
<span data-ttu-id="50761-116">Du kan skapa udr: er för att ange där trafik ska skickas till, baserat på dess mål-adress.</span><span class="sxs-lookup"><span data-stu-id="50761-116">You can create UDRs to specify where traffic should be sent to, based on its destination address.</span></span> <span data-ttu-id="50761-117">Du kan se en väg som standard-gateway-definition baserat på mål-adress för ett nätverkspaket.</span><span class="sxs-lookup"><span data-stu-id="50761-117">You can think of a route as the default gateway definition based on the destination address of a network packet.</span></span>

<span data-ttu-id="50761-118">Udr: er innehåller följande egenskaper.</span><span class="sxs-lookup"><span data-stu-id="50761-118">UDRs contain the following properties.</span></span> 

| <span data-ttu-id="50761-119">Egenskap</span><span class="sxs-lookup"><span data-stu-id="50761-119">Property</span></span> | <span data-ttu-id="50761-120">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="50761-120">Description</span></span> | <span data-ttu-id="50761-121">Exempelvärden</span><span class="sxs-lookup"><span data-stu-id="50761-121">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="50761-122">**addressPrefix**</span><span class="sxs-lookup"><span data-stu-id="50761-122">**addressPrefix**</span></span> |<span data-ttu-id="50761-123">Adressprefixet eller fullständig IP-adress för mål</span><span class="sxs-lookup"><span data-stu-id="50761-123">Address prefix, or full IP address for the destination</span></span> |<span data-ttu-id="50761-124">192.168.1.0/24, 192.168.1.101</span><span class="sxs-lookup"><span data-stu-id="50761-124">192.168.1.0/24, 192.168.1.101</span></span> |
| <span data-ttu-id="50761-125">**nextHopType**</span><span class="sxs-lookup"><span data-stu-id="50761-125">**nextHopType**</span></span> |<span data-ttu-id="50761-126">Typ av enhet som trafiken skickas till</span><span class="sxs-lookup"><span data-stu-id="50761-126">Type of device the traffic will be sent to</span></span> |<span data-ttu-id="50761-127">VirtualAppliance, VPN-Gateway, Internet</span><span class="sxs-lookup"><span data-stu-id="50761-127">VirtualAppliance, VPN Gateway, Internet</span></span> |
| <span data-ttu-id="50761-128">**nextHopIpAddress**</span><span class="sxs-lookup"><span data-stu-id="50761-128">**nextHopIpAddress**</span></span> |<span data-ttu-id="50761-129">IP-adressen för nästa hopp</span><span class="sxs-lookup"><span data-stu-id="50761-129">IP address for the next hop</span></span> |<span data-ttu-id="50761-130">192.168.1.4</span><span class="sxs-lookup"><span data-stu-id="50761-130">192.168.1.4</span></span> |

<span data-ttu-id="50761-131">Exempel routningstabellen i JSON-format:</span><span class="sxs-lookup"><span data-stu-id="50761-131">Sample route table in JSON format:</span></span>

    {
        "name": "UDR-BackEnd",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/routeTables",
        "location": "westus",
        "properties": {
            "provisioningState": "Succeeded",
            "routes": [
                {
                    "name": "RouteToFrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd/routes/RouteToFrontEnd",
                    "etag": "W/\"v\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "nextHopType": "VirtualAppliance",
                        "nextHopIpAddress": "192.168.0.4"
                    }
                }
            ],
            "subnets": [
                {
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd"
                }
            ]
        }
    }

### <a name="additional-resources"></a><span data-ttu-id="50761-132">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="50761-132">Additional resources</span></span>
* <span data-ttu-id="50761-133">Hämta mer information [udr: er](../articles/virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="50761-133">Get more information about [UDRs](../articles/virtual-network/virtual-networks-udr-overview.md).</span></span>
* <span data-ttu-id="50761-134">Läs den [REST API-referensdokumentation](https://msdn.microsoft.com/library/azure/mt502549.aspx) för routningstabeller.</span><span class="sxs-lookup"><span data-stu-id="50761-134">Read the [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt502549.aspx) for route tables.</span></span>
* <span data-ttu-id="50761-135">Läs den [REST API-referensdokumentation](https://msdn.microsoft.com/library/azure/mt502539.aspx) definierats vägar (udr: er) för användaren.</span><span class="sxs-lookup"><span data-stu-id="50761-135">Read the [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt502539.aspx) for user defined routes (UDRs).</span></span>

