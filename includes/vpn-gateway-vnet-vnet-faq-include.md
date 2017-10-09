Hej VNet-till-VNet vanliga frågor och svar gäller tooVPN Gateway-anslutningar. Om du letar efter VNet-Peering, se [Peering för virtuella nätverk](../articles/virtual-network/virtual-network-peering-overview.md)

### <a name="does-azure-charge-for-traffic-between-vnets"></a>Tar Azure ut avgifter för trafik mellan virtuella nätverk?

VNet-till-VNet-trafik i hello samma region som är gratis för båda riktningarna när du använder en VPN-anslutning för gateway. Mellan region VNet-till-VNet utgående debiteras trafik med hello utgående bland-VNet data överföringshastighet baserat på hello källa regioner. Se toohello [VPN-Gateway sida med priser](https://azure.microsoft.com/pricing/details/vpn-gateway/) mer information. Om du ansluter din Vnet med hjälp av VNet-Peering i stället VPN-Gateway finns hello [virtuellt nätverk sida med priser](https://azure.microsoft.com/pricing/details/virtual-network/).

### <a name="does-vnet-to-vnet-traffic-travel-across-hello-internet"></a>VNet-till-VNet-trafik som passerar hello Internet?

Nej. VNet-till-VNet-trafik skickas via hello Microsoft Azure-stamnät, inte hello Internet.

### <a name="is-vnet-to-vnet-traffic-secure"></a>Är trafiken mellan virtuella nätverk säker?

Ja, den är skyddad med IPsec/IKE-kryptering.

### <a name="do-i-need-a-vpn-device-tooconnect-vnets-together"></a>Behöver jag en VPN-enhet tooconnect Vnet tillsammans?

Nej. Att ansluta flera virtuella Azure-nätverk till varandra kräver inte någon VPN-enhet, såvida inte en anslutning mellan flera platser krävs.

### <a name="do-my-vnets-need-toobe-in-hello-same-region"></a>Behöver Mina Vnet toobe i hello samma region?

Nej. hello virtuella nätverk kan vara i hello samma eller olika Azure-regioner (platser).

### <a name="if-hello-vnets-are-not-in-hello-same-subscription-do-hello-subscriptions-need-toobe-associated-with-hello-same-ad-tenant"></a>Om hello Vnet inte är med i hello samma prenumeration, hello prenumerationer behöver toobe som associeras med innehavaren hello samma AD?

Nej.

### <a name="can-i-use-vnet-to-vnet-along-with-multi-site-connections"></a>Kan jag använda anslutningar mellan virtuella nätverk tillsammans med anslutningar mellan flera platser?

Ja. Virtuell nätverksanslutning kan användas samtidigt med VPN på flera platser.

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>Hur många lokala platser och virtuella nätverk kan ett virtuellt nätverk ansluta till?

Se tabellen med [Gateway-krav](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#requirements).

### <a name="can-i-use-vnet-to-vnet-tooconnect-vms-or-cloud-services-outside-of-a-vnet"></a>Kan jag använda VNet-till-VNet tooconnect virtuella datorer eller molntjänster utanför ett virtuellt nätverk?

Nej. VNet-till-VNet stöder anslutning av virtuella nätverk. Det stöder inte anslutning av virtuella datorer eller molntjänster som inte finns i ett virtuellt nätverk.

### <a name="can-a-cloud-service-or-a-load-balancing-endpoint-span-vnets"></a>Kan en molntjänst eller en slutpunkt för utjämning av nätverksbelastning sträcka sig över virtuella nätverk?

Nej. En molntjänst eller en slutpunkt för belastningsutjämning kan inte omfatta flera virtuella nätverk, även om de är sammankopplade.

### <a name="can-i-used-a-policybased-vpn-type-for-vnet-to-vnet-or-multi-site-connections"></a>Kan jag använda en principbaserad VPN-typ för virtuellt nätverk till virtuellt nätverk eller flerplatsanslutningar?

Nej. Anslutningar mellan olika virtuella nätverk och flera platser kräver Azure VPN-gatewayer med VPN-typen RouteBased (tidigare kallad dynamisk routning).

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-tooanother-vnet-with-a-policybased-vpn-type"></a>Kan jag ansluta ett virtuellt nätverk med en VPN-typ för RouteBased tooanother VNet med en PolicyBased VPN-typ?

Nej, båda virtuella nätverken MÅSTE använda routningsbaserade VPN-anslutningar (tidigare kallat dynamisk routning).

### <a name="do-vpn-tunnels-share-bandwidth"></a>Delar VPN-tunnlar bandbredd?

Ja. VPN-tunnlar i hello virtuellt nätverk dela hello tillgängliga bandbredd hello Azure VPN-gateway och hello samma VPN gateway drifttid SLA i Azure.

### <a name="are-redundant-tunnels-supported"></a>Finns det stöd för redundanta tunnlar?

Redundanta tunnlar mellan ett par med virtuella nätverk stöds när en virtuell nätverksgateway är konfigurerad som aktiv-aktiv.

### <a name="can-i-have-overlapping-address-spaces-for-vnet-to-vnet-configurations"></a>Kan jag har överlappande adressutrymmen för konfigurationer mellan virtuella nätverk?

Nej. Du kan inte ha överlappande IP-adressintervall.

### <a name="can-there-be-overlapping-address-spaces-among-connected-virtual-networks-and-on-premises-local-sites"></a>Får det finnas överlappande adressutrymmen i anslutna virtuella nätverk och på lokala platser?

Nej. Du kan inte ha överlappande IP-adressintervall.



