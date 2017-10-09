<span data-ttu-id="a15fc-101">Hej VNet-till-VNet vanliga frågor och svar gäller tooVPN Gateway-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="a15fc-101">hello VNet-to-VNet FAQ applies tooVPN Gateway connections.</span></span> <span data-ttu-id="a15fc-102">Om du letar efter VNet-Peering, se [Peering för virtuella nätverk](../articles/virtual-network/virtual-network-peering-overview.md)</span><span class="sxs-lookup"><span data-stu-id="a15fc-102">If you are looking for VNet Peering, see [Virtual Network Peering](../articles/virtual-network/virtual-network-peering-overview.md)</span></span>

### <a name="does-azure-charge-for-traffic-between-vnets"></a><span data-ttu-id="a15fc-103">Tar Azure ut avgifter för trafik mellan virtuella nätverk?</span><span class="sxs-lookup"><span data-stu-id="a15fc-103">Does Azure charge for traffic between VNets?</span></span>

<span data-ttu-id="a15fc-104">VNet-till-VNet-trafik i hello samma region som är gratis för båda riktningarna när du använder en VPN-anslutning för gateway.</span><span class="sxs-lookup"><span data-stu-id="a15fc-104">VNet-to-VNet traffic within hello same region is free for both directions when using a VPN gateway connection.</span></span> <span data-ttu-id="a15fc-105">Mellan region VNet-till-VNet utgående debiteras trafik med hello utgående bland-VNet data överföringshastighet baserat på hello källa regioner.</span><span class="sxs-lookup"><span data-stu-id="a15fc-105">Cross region VNet-to-VNet egress traffic is charged with hello outbound inter-VNet data transfer rates based on hello source regions.</span></span> <span data-ttu-id="a15fc-106">Se toohello [VPN-Gateway sida med priser](https://azure.microsoft.com/pricing/details/vpn-gateway/) mer information.</span><span class="sxs-lookup"><span data-stu-id="a15fc-106">Refer toohello [VPN Gateway pricing page](https://azure.microsoft.com/pricing/details/vpn-gateway/) for details.</span></span> <span data-ttu-id="a15fc-107">Om du ansluter din Vnet med hjälp av VNet-Peering i stället VPN-Gateway finns hello [virtuellt nätverk sida med priser](https://azure.microsoft.com/pricing/details/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="a15fc-107">If you are connecting your VNets using VNet Peering, rather than VPN Gateway, see hello [Virtual Network pricing page](https://azure.microsoft.com/pricing/details/virtual-network/).</span></span>

### <a name="does-vnet-to-vnet-traffic-travel-across-hello-internet"></a><span data-ttu-id="a15fc-108">VNet-till-VNet-trafik som passerar hello Internet?</span><span class="sxs-lookup"><span data-stu-id="a15fc-108">Does VNet-to-VNet traffic travel across hello Internet?</span></span>

<span data-ttu-id="a15fc-109">Nej.</span><span class="sxs-lookup"><span data-stu-id="a15fc-109">No.</span></span> <span data-ttu-id="a15fc-110">VNet-till-VNet-trafik skickas via hello Microsoft Azure-stamnät, inte hello Internet.</span><span class="sxs-lookup"><span data-stu-id="a15fc-110">VNet-to-VNet traffic travels across hello Microsoft Azure backbone, not hello Internet.</span></span>

### <a name="is-vnet-to-vnet-traffic-secure"></a><span data-ttu-id="a15fc-111">Är trafiken mellan virtuella nätverk säker?</span><span class="sxs-lookup"><span data-stu-id="a15fc-111">Is VNet-to-VNet traffic secure?</span></span>

<span data-ttu-id="a15fc-112">Ja, den är skyddad med IPsec/IKE-kryptering.</span><span class="sxs-lookup"><span data-stu-id="a15fc-112">Yes, it is protected by IPsec/IKE encryption.</span></span>

### <a name="do-i-need-a-vpn-device-tooconnect-vnets-together"></a><span data-ttu-id="a15fc-113">Behöver jag en VPN-enhet tooconnect Vnet tillsammans?</span><span class="sxs-lookup"><span data-stu-id="a15fc-113">Do I need a VPN device tooconnect VNets together?</span></span>

<span data-ttu-id="a15fc-114">Nej.</span><span class="sxs-lookup"><span data-stu-id="a15fc-114">No.</span></span> <span data-ttu-id="a15fc-115">Att ansluta flera virtuella Azure-nätverk till varandra kräver inte någon VPN-enhet, såvida inte en anslutning mellan flera platser krävs.</span><span class="sxs-lookup"><span data-stu-id="a15fc-115">Connecting multiple Azure virtual networks together doesn't require a VPN device unless cross-premises connectivity is required.</span></span>

### <a name="do-my-vnets-need-toobe-in-hello-same-region"></a><span data-ttu-id="a15fc-116">Behöver Mina Vnet toobe i hello samma region?</span><span class="sxs-lookup"><span data-stu-id="a15fc-116">Do my VNets need toobe in hello same region?</span></span>

<span data-ttu-id="a15fc-117">Nej.</span><span class="sxs-lookup"><span data-stu-id="a15fc-117">No.</span></span> <span data-ttu-id="a15fc-118">hello virtuella nätverk kan vara i hello samma eller olika Azure-regioner (platser).</span><span class="sxs-lookup"><span data-stu-id="a15fc-118">hello virtual networks can be in hello same or different Azure regions (locations).</span></span>

### <a name="if-hello-vnets-are-not-in-hello-same-subscription-do-hello-subscriptions-need-toobe-associated-with-hello-same-ad-tenant"></a><span data-ttu-id="a15fc-119">Om hello Vnet inte är med i hello samma prenumeration, hello prenumerationer behöver toobe som associeras med innehavaren hello samma AD?</span><span class="sxs-lookup"><span data-stu-id="a15fc-119">If hello VNets are not in hello same subscription, do hello subscriptions need toobe associated with hello same AD tenant?</span></span>

<span data-ttu-id="a15fc-120">Nej.</span><span class="sxs-lookup"><span data-stu-id="a15fc-120">No.</span></span>

### <a name="can-i-use-vnet-to-vnet-along-with-multi-site-connections"></a><span data-ttu-id="a15fc-121">Kan jag använda anslutningar mellan virtuella nätverk tillsammans med anslutningar mellan flera platser?</span><span class="sxs-lookup"><span data-stu-id="a15fc-121">Can I use VNet-to-VNet along with multi-site connections?</span></span>

<span data-ttu-id="a15fc-122">Ja.</span><span class="sxs-lookup"><span data-stu-id="a15fc-122">Yes.</span></span> <span data-ttu-id="a15fc-123">Virtuell nätverksanslutning kan användas samtidigt med VPN på flera platser.</span><span class="sxs-lookup"><span data-stu-id="a15fc-123">Virtual network connectivity can be used simultaneously with multi-site VPNs.</span></span>

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a><span data-ttu-id="a15fc-124">Hur många lokala platser och virtuella nätverk kan ett virtuellt nätverk ansluta till?</span><span class="sxs-lookup"><span data-stu-id="a15fc-124">How many on-premises sites and virtual networks can one virtual network connect to?</span></span>

<span data-ttu-id="a15fc-125">Se tabellen med [Gateway-krav](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#requirements).</span><span class="sxs-lookup"><span data-stu-id="a15fc-125">See [Gateway requirements](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#requirements) table.</span></span>

### <a name="can-i-use-vnet-to-vnet-tooconnect-vms-or-cloud-services-outside-of-a-vnet"></a><span data-ttu-id="a15fc-126">Kan jag använda VNet-till-VNet tooconnect virtuella datorer eller molntjänster utanför ett virtuellt nätverk?</span><span class="sxs-lookup"><span data-stu-id="a15fc-126">Can I use VNet-to-VNet tooconnect VMs or cloud services outside of a VNet?</span></span>

<span data-ttu-id="a15fc-127">Nej.</span><span class="sxs-lookup"><span data-stu-id="a15fc-127">No.</span></span> <span data-ttu-id="a15fc-128">VNet-till-VNet stöder anslutning av virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="a15fc-128">VNet-to-VNet supports connecting virtual networks.</span></span> <span data-ttu-id="a15fc-129">Det stöder inte anslutning av virtuella datorer eller molntjänster som inte finns i ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="a15fc-129">It does not support connecting virtual machines or cloud services that are not in a virtual network.</span></span>

### <a name="can-a-cloud-service-or-a-load-balancing-endpoint-span-vnets"></a><span data-ttu-id="a15fc-130">Kan en molntjänst eller en slutpunkt för utjämning av nätverksbelastning sträcka sig över virtuella nätverk?</span><span class="sxs-lookup"><span data-stu-id="a15fc-130">Can a cloud service or a load balancing endpoint span VNets?</span></span>

<span data-ttu-id="a15fc-131">Nej.</span><span class="sxs-lookup"><span data-stu-id="a15fc-131">No.</span></span> <span data-ttu-id="a15fc-132">En molntjänst eller en slutpunkt för belastningsutjämning kan inte omfatta flera virtuella nätverk, även om de är sammankopplade.</span><span class="sxs-lookup"><span data-stu-id="a15fc-132">A cloud service or a load balancing endpoint can't span across virtual networks, even if they are connected together.</span></span>

### <a name="can-i-used-a-policybased-vpn-type-for-vnet-to-vnet-or-multi-site-connections"></a><span data-ttu-id="a15fc-133">Kan jag använda en principbaserad VPN-typ för virtuellt nätverk till virtuellt nätverk eller flerplatsanslutningar?</span><span class="sxs-lookup"><span data-stu-id="a15fc-133">Can I used a PolicyBased VPN type for VNet-to-VNet or Multi-Site connections?</span></span>

<span data-ttu-id="a15fc-134">Nej.</span><span class="sxs-lookup"><span data-stu-id="a15fc-134">No.</span></span> <span data-ttu-id="a15fc-135">Anslutningar mellan olika virtuella nätverk och flera platser kräver Azure VPN-gatewayer med VPN-typen RouteBased (tidigare kallad dynamisk routning).</span><span class="sxs-lookup"><span data-stu-id="a15fc-135">VNet-to-VNet and Multi-Site connections require Azure VPN gateways with RouteBased (previously called Dynamic Routing) VPN types.</span></span>

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-tooanother-vnet-with-a-policybased-vpn-type"></a><span data-ttu-id="a15fc-136">Kan jag ansluta ett virtuellt nätverk med en VPN-typ för RouteBased tooanother VNet med en PolicyBased VPN-typ?</span><span class="sxs-lookup"><span data-stu-id="a15fc-136">Can I connect a VNet with a RouteBased VPN Type tooanother VNet with a PolicyBased VPN type?</span></span>

<span data-ttu-id="a15fc-137">Nej, båda virtuella nätverken MÅSTE använda routningsbaserade VPN-anslutningar (tidigare kallat dynamisk routning).</span><span class="sxs-lookup"><span data-stu-id="a15fc-137">No, both virtual networks MUST be using route-based (previously called Dynamic Routing) VPNs.</span></span>

### <a name="do-vpn-tunnels-share-bandwidth"></a><span data-ttu-id="a15fc-138">Delar VPN-tunnlar bandbredd?</span><span class="sxs-lookup"><span data-stu-id="a15fc-138">Do VPN tunnels share bandwidth?</span></span>

<span data-ttu-id="a15fc-139">Ja.</span><span class="sxs-lookup"><span data-stu-id="a15fc-139">Yes.</span></span> <span data-ttu-id="a15fc-140">VPN-tunnlar i hello virtuellt nätverk dela hello tillgängliga bandbredd hello Azure VPN-gateway och hello samma VPN gateway drifttid SLA i Azure.</span><span class="sxs-lookup"><span data-stu-id="a15fc-140">All VPN tunnels of hello virtual network share hello available bandwidth on hello Azure VPN gateway and hello same VPN gateway uptime SLA in Azure.</span></span>

### <a name="are-redundant-tunnels-supported"></a><span data-ttu-id="a15fc-141">Finns det stöd för redundanta tunnlar?</span><span class="sxs-lookup"><span data-stu-id="a15fc-141">Are redundant tunnels supported?</span></span>

<span data-ttu-id="a15fc-142">Redundanta tunnlar mellan ett par med virtuella nätverk stöds när en virtuell nätverksgateway är konfigurerad som aktiv-aktiv.</span><span class="sxs-lookup"><span data-stu-id="a15fc-142">Redundant tunnels between a pair of virtual networks are supported when one virtual network gateway is configured as active-active.</span></span>

### <a name="can-i-have-overlapping-address-spaces-for-vnet-to-vnet-configurations"></a><span data-ttu-id="a15fc-143">Kan jag har överlappande adressutrymmen för konfigurationer mellan virtuella nätverk?</span><span class="sxs-lookup"><span data-stu-id="a15fc-143">Can I have overlapping address spaces for VNet-to-VNet configurations?</span></span>

<span data-ttu-id="a15fc-144">Nej.</span><span class="sxs-lookup"><span data-stu-id="a15fc-144">No.</span></span> <span data-ttu-id="a15fc-145">Du kan inte ha överlappande IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="a15fc-145">You can't have overlapping IP address ranges.</span></span>

### <a name="can-there-be-overlapping-address-spaces-among-connected-virtual-networks-and-on-premises-local-sites"></a><span data-ttu-id="a15fc-146">Får det finnas överlappande adressutrymmen i anslutna virtuella nätverk och på lokala platser?</span><span class="sxs-lookup"><span data-stu-id="a15fc-146">Can there be overlapping address spaces among connected virtual networks and on-premises local sites?</span></span>

<span data-ttu-id="a15fc-147">Nej.</span><span class="sxs-lookup"><span data-stu-id="a15fc-147">No.</span></span> <span data-ttu-id="a15fc-148">Du kan inte ha överlappande IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="a15fc-148">You can't have overlapping IP address ranges.</span></span>



