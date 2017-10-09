## <a name="route-tables"></a><span data-ttu-id="f3518-101">Vägtabeller</span><span class="sxs-lookup"><span data-stu-id="f3518-101">Route tables</span></span>
<span data-ttu-id="f3518-102">Väg tabellen resurser innehåller vägar toodefine hur trafiken flödar i Azure-infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="f3518-102">Route table resources contains routes used toodefine how traffic flows within your Azure infrastructure.</span></span> <span data-ttu-id="f3518-103">Du kan använda användardefinierade vägar (UDR) toosend all trafik från ett visst undernät tooa virtuell installation, till exempel en brandvägg eller intrångsidentifiering identifiering-system (ID).</span><span class="sxs-lookup"><span data-stu-id="f3518-103">You can use user defined routes (UDR) toosend all traffic from a given subnet tooa virtual appliance, such as a firewall or intrusion detection system (IDS).</span></span> <span data-ttu-id="f3518-104">Du kan associera en väg tabellen toosubnets.</span><span class="sxs-lookup"><span data-stu-id="f3518-104">You can associate a route table toosubnets.</span></span> 

<span data-ttu-id="f3518-105">Routningstabeller innehålla hello följande egenskaper.</span><span class="sxs-lookup"><span data-stu-id="f3518-105">Route tables contain hello following properties.</span></span>

| <span data-ttu-id="f3518-106">Egenskap</span><span class="sxs-lookup"><span data-stu-id="f3518-106">Property</span></span> | <span data-ttu-id="f3518-107">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f3518-107">Description</span></span> | <span data-ttu-id="f3518-108">Exempelvärden</span><span class="sxs-lookup"><span data-stu-id="f3518-108">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f3518-109">**vägar**</span><span class="sxs-lookup"><span data-stu-id="f3518-109">**routes**</span></span> |<span data-ttu-id="f3518-110">Samling användardefinierade vägar i routningstabellen för hello</span><span class="sxs-lookup"><span data-stu-id="f3518-110">Collection of user defined routes in hello route table</span></span> |<span data-ttu-id="f3518-111">Se [användardefinierade vägar](#User-defined-routes)</span><span class="sxs-lookup"><span data-stu-id="f3518-111">see [user defined routes](#User-defined-routes)</span></span> |
| <span data-ttu-id="f3518-112">**undernät**</span><span class="sxs-lookup"><span data-stu-id="f3518-112">**subnets**</span></span> |<span data-ttu-id="f3518-113">Samling av routningstabellen för undernät hello används för</span><span class="sxs-lookup"><span data-stu-id="f3518-113">Collection of subnets hello route table is applied too</span></span>|<span data-ttu-id="f3518-114">Se [undernät](#Subnets)</span><span class="sxs-lookup"><span data-stu-id="f3518-114">see [subnets](#Subnets)</span></span> |

### <a name="user-defined-routes"></a><span data-ttu-id="f3518-115">Användardefinierade vägar</span><span class="sxs-lookup"><span data-stu-id="f3518-115">User defined routes</span></span>
<span data-ttu-id="f3518-116">Du kan skapa udr: er toospecify där trafik ska skickas till, baserat på dess mål-adress.</span><span class="sxs-lookup"><span data-stu-id="f3518-116">You can create UDRs toospecify where traffic should be sent to, based on its destination address.</span></span> <span data-ttu-id="f3518-117">Du kan se en väg som hello gateway standarddefinition baserat på hello måladress för ett nätverkspaket.</span><span class="sxs-lookup"><span data-stu-id="f3518-117">You can think of a route as hello default gateway definition based on hello destination address of a network packet.</span></span>

<span data-ttu-id="f3518-118">Udr: er innehåller hello följande egenskaper.</span><span class="sxs-lookup"><span data-stu-id="f3518-118">UDRs contain hello following properties.</span></span> 

| <span data-ttu-id="f3518-119">Egenskap</span><span class="sxs-lookup"><span data-stu-id="f3518-119">Property</span></span> | <span data-ttu-id="f3518-120">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f3518-120">Description</span></span> | <span data-ttu-id="f3518-121">Exempelvärden</span><span class="sxs-lookup"><span data-stu-id="f3518-121">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f3518-122">**addressPrefix**</span><span class="sxs-lookup"><span data-stu-id="f3518-122">**addressPrefix**</span></span> |<span data-ttu-id="f3518-123">Adressprefixet eller fullständig IP-adress för hello mål</span><span class="sxs-lookup"><span data-stu-id="f3518-123">Address prefix, or full IP address for hello destination</span></span> |<span data-ttu-id="f3518-124">192.168.1.0/24, 192.168.1.101</span><span class="sxs-lookup"><span data-stu-id="f3518-124">192.168.1.0/24, 192.168.1.101</span></span> |
| <span data-ttu-id="f3518-125">**nextHopType**</span><span class="sxs-lookup"><span data-stu-id="f3518-125">**nextHopType**</span></span> |<span data-ttu-id="f3518-126">Typ av enhet hello trafik skickas för</span><span class="sxs-lookup"><span data-stu-id="f3518-126">Type of device hello traffic will be sent too</span></span>|<span data-ttu-id="f3518-127">VirtualAppliance, VPN-Gateway, Internet</span><span class="sxs-lookup"><span data-stu-id="f3518-127">VirtualAppliance, VPN Gateway, Internet</span></span> |
| <span data-ttu-id="f3518-128">**nextHopIpAddress**</span><span class="sxs-lookup"><span data-stu-id="f3518-128">**nextHopIpAddress**</span></span> |<span data-ttu-id="f3518-129">IP-adress för hello nästa hopp</span><span class="sxs-lookup"><span data-stu-id="f3518-129">IP address for hello next hop</span></span> |<span data-ttu-id="f3518-130">192.168.1.4</span><span class="sxs-lookup"><span data-stu-id="f3518-130">192.168.1.4</span></span> |

<span data-ttu-id="f3518-131">Exempel routningstabellen i JSON-format:</span><span class="sxs-lookup"><span data-stu-id="f3518-131">Sample route table in JSON format:</span></span>

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

### <a name="additional-resources"></a><span data-ttu-id="f3518-132">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="f3518-132">Additional resources</span></span>
* <span data-ttu-id="f3518-133">Hämta mer information [udr: er](../articles/virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f3518-133">Get more information about [UDRs](../articles/virtual-network/virtual-networks-udr-overview.md).</span></span>
* <span data-ttu-id="f3518-134">Läs hello [REST API-referensdokumentation](https://msdn.microsoft.com/library/azure/mt502549.aspx) för routningstabeller.</span><span class="sxs-lookup"><span data-stu-id="f3518-134">Read hello [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt502549.aspx) for route tables.</span></span>
* <span data-ttu-id="f3518-135">Läs hello [REST API-referensdokumentation](https://msdn.microsoft.com/library/azure/mt502539.aspx) definierats vägar (udr: er) för användaren.</span><span class="sxs-lookup"><span data-stu-id="f3518-135">Read hello [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt502539.aspx) for user defined routes (UDRs).</span></span>

