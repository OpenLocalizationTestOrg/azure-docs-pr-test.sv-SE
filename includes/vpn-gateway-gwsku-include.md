<span data-ttu-id="e1fff-101">När du skapar en virtuell nätverksgateway måste toospecify hello gateway SKU som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="e1fff-101">When you create a virtual network gateway, you need toospecify hello gateway SKU that you want toouse.</span></span> <span data-ttu-id="e1fff-102">Välj hello SKU: er som uppfyller dina krav baserat på hello typer av arbetsbelastningar, genomflöden, funktioner och servicenivåavtal.</span><span class="sxs-lookup"><span data-stu-id="e1fff-102">Select hello SKUs that satisfy your requirements based on hello types of workloads, throughputs, features, and SLAs.</span></span>

[!INCLUDE [classic SKU](./vpn-gateway-classic-sku-support-include.md)]

[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

###  <span data-ttu-id="e1fff-103"><a name="workloads"></a>Produktion *jämfört med* Arbetsbelastningar för utvecklingstest</span><span class="sxs-lookup"><span data-stu-id="e1fff-103"><a name="workloads"></a>Production *vs.* Dev-Test Workloads</span></span>

<span data-ttu-id="e1fff-104">På grund av toohello skillnader i SLA: er och funktioner, rekommenderar vi följande SKU: er för produktion hello *kontra* dev-test:</span><span class="sxs-lookup"><span data-stu-id="e1fff-104">Due toohello differences in SLAs and feature sets, we recommend hello following SKUs for production *vs.* dev-test:</span></span>

| <span data-ttu-id="e1fff-105">**Arbetsbelastning**</span><span class="sxs-lookup"><span data-stu-id="e1fff-105">**Workload**</span></span>                       | <span data-ttu-id="e1fff-106">**SKU: er**</span><span class="sxs-lookup"><span data-stu-id="e1fff-106">**SKUs**</span></span>               |
| ---                                | ---                    |
| <span data-ttu-id="e1fff-107">**Produktion, kritiska arbetsbelastningar**</span><span class="sxs-lookup"><span data-stu-id="e1fff-107">**Production, critical workloads**</span></span> | <span data-ttu-id="e1fff-108">VpnGw1 VpnGw2, VpnGw3</span><span class="sxs-lookup"><span data-stu-id="e1fff-108">VpnGw1, VpnGw2, VpnGw3</span></span> |
| <span data-ttu-id="e1fff-109">**Utv-test eller konceptbevis**</span><span class="sxs-lookup"><span data-stu-id="e1fff-109">**Dev-test or proof of concept**</span></span>   | <span data-ttu-id="e1fff-110">Basic</span><span class="sxs-lookup"><span data-stu-id="e1fff-110">Basic</span></span>                  |
|                                    |                        |

<span data-ttu-id="e1fff-111">Om du använder gamla hello är SKU: er, hello produktion SKU rekommendationer Standard och HighPerformance SKU: er.</span><span class="sxs-lookup"><span data-stu-id="e1fff-111">If you are using hello old SKUs, hello production SKU recommendations are Standard and HighPerformance SKUs.</span></span> <span data-ttu-id="e1fff-112">Mer information om hello gamla SKU: er finns [Gateway-SKU: er (äldre SKU: er)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md).</span><span class="sxs-lookup"><span data-stu-id="e1fff-112">For information on hello old SKUs, see [Gateway SKUs (legacy SKUs)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md).</span></span>

###  <span data-ttu-id="e1fff-113"><a name="feature"></a>Funktionsuppsättningarna för gateway-SKU:er</span><span class="sxs-lookup"><span data-stu-id="e1fff-113"><a name="feature"></a>Gateway SKU feature sets</span></span>

<span data-ttu-id="e1fff-114">hello ny gateway SKU: er förenkla hello funktionsuppsättningar erbjuds via hello gatewayer:</span><span class="sxs-lookup"><span data-stu-id="e1fff-114">hello new gateway SKUs streamline hello feature sets offered on hello gateways:</span></span>

| <span data-ttu-id="e1fff-115">**SKU**</span><span class="sxs-lookup"><span data-stu-id="e1fff-115">**SKU**</span></span>| <span data-ttu-id="e1fff-116">**Funktioner**</span><span class="sxs-lookup"><span data-stu-id="e1fff-116">**Features**</span></span>|
| ---    | ---         |
|<span data-ttu-id="e1fff-117">**Basic**</span><span class="sxs-lookup"><span data-stu-id="e1fff-117">**Basic**</span></span>   | <span data-ttu-id="e1fff-118">**Ruttbaserad VPN**: 10 tunnlar med P2S</span><span class="sxs-lookup"><span data-stu-id="e1fff-118">**Route-based VPN**: 10 tunnels with P2S</span></span><br><br><span data-ttu-id="e1fff-119">**Principbaserad VPN**: (IKEv1): 1 tunnel; ingen P2S</span><span class="sxs-lookup"><span data-stu-id="e1fff-119">**Policy-based VPN**: (IKEv1): 1 tunnel; no P2S</span></span>|
| <span data-ttu-id="e1fff-120">**VpnGw1, VpnGw2 och VpnGw3**</span><span class="sxs-lookup"><span data-stu-id="e1fff-120">**VpnGw1, VpnGw2, and VpnGw3**</span></span> | <span data-ttu-id="e1fff-121">**Ruttbaserad VPN**: upp too30 tunnlar (*), P2S, BGP, aktiv-aktiv, anpassade IPsec/IKE-principen, ExpressRoute/VPN samexistent</span><span class="sxs-lookup"><span data-stu-id="e1fff-121">**Route-based VPN**: up too30 tunnels (*), P2S, BGP, active-active, custom IPsec/IKE policy, ExpressRoute/VPN co-existence</span></span> |
|        |             |

<span data-ttu-id="e1fff-122">(*) Du kan konfigurera ”PolicyBasedTrafficSelectors” tooconnect en ruttbaserad VPN-gateway (VpnGw1, VpnGw2, VpnGw3) toomultiple lokala principbaserad brandväggen enheter.</span><span class="sxs-lookup"><span data-stu-id="e1fff-122">(*) You can configure "PolicyBasedTrafficSelectors" tooconnect a route-based VPN gateway (VpnGw1, VpnGw2, VpnGw3) toomultiple on-premises policy-based firewall devices.</span></span> <span data-ttu-id="e1fff-123">Se för[Anslut VPN-gatewayer toomultiple lokalt principbaserad VPN-enheter med hjälp av PowerShell](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="e1fff-123">Refer too[Connect VPN gateways toomultiple on-premises policy-based VPN devices using PowerShell](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) for details.</span></span>

###  <span data-ttu-id="e1fff-124"><a name="resize"></a>Ändra storlek på gateway-SKU: er</span><span class="sxs-lookup"><span data-stu-id="e1fff-124"><a name="resize"></a>Resizing gateway SKUs</span></span>

1. <span data-ttu-id="e1fff-125">Du kan ändra storlek mellan VpnGw1, VpnGw2 och VpnGw3 SKU: er.</span><span class="sxs-lookup"><span data-stu-id="e1fff-125">You can resize between VpnGw1, VpnGw2, and VpnGw3 SKUs.</span></span>
2. <span data-ttu-id="e1fff-126">När du arbetar med hello gamla gateway-SKU: er, kan du ändra storlek på mellan Basic, Standard och HighPerformance SKU: er.</span><span class="sxs-lookup"><span data-stu-id="e1fff-126">When working with hello old gateway SKUs, you can resize between Basic, Standard, and HighPerformance SKUs.</span></span>
2. <span data-ttu-id="e1fff-127">Du **kan** ändra storlek från SKU: er för Basic/Standard/HighPerformance toohello nya VpnGw3-VpnGw1/VpnGw2 SKU: er.</span><span class="sxs-lookup"><span data-stu-id="e1fff-127">You **cannot** resize from Basic/Standard/HighPerformance SKUs toohello new VpnGw1/VpnGw2/VpnGw3 SKUs.</span></span> <span data-ttu-id="e1fff-128">Du måste i stället [migrera](#migrate) toohello nya SKU: er.</span><span class="sxs-lookup"><span data-stu-id="e1fff-128">You must, instead, [migrate](#migrate) toohello new SKUs.</span></span>

###  <span data-ttu-id="e1fff-129"><a name="migrate"></a>Migrera från den gamla SKU: er toohello nya SKU: er</span><span class="sxs-lookup"><span data-stu-id="e1fff-129"><a name="migrate"></a>Migrating from old SKUs toohello new SKUs</span></span>

[!INCLUDE [Migrate SKU](./vpn-gateway-migrate-legacy-sku-include.md)]
