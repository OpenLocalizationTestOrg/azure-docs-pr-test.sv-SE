### <a name="is-bgp-supported-on-all-azure-vpn-gateway-skus"></a>Stöds BGP på alla Azure VPN-gateway-SKU:er?
Nej, BGP stöds på Azure **Standard** och **HighPerformance** VPN-gatewayer. **Basic** SKU stöds inte.

### <a name="can-i-use-bgp-with-azure-policy-based-vpn-gateways"></a>Kan jag använda BGP med Azure principbaserade VPN-gatewayer?
Nej, BGP stöds bara på route-baserade VPN-gatewayer.

### <a name="can-i-use-private-asns-autonomous-system-numbers"></a>Kan jag använda privata ASN:er (Autonomous System Numbers)?
Ja, du kan använda dina egna offentliga ASN:er eller privata ASN:er för både dina lokala nätverk och virtuella Azure-nätverk.

### <a name="are-there-asns-reserved-by-azure"></a>Finns det ASN:er reserverade av Azure?
Ja, hello följande ASN: er är reserverade av Azure för både interna och externa peerkopplingar:

* Offentliga ASN:er: 8075, 8076, 12076
* Privata ASN:er: 65515, 65517, 65518, 65519, 65520

Du kan inte ange dessa ASN: er för dina lokala VPN-enheter när du ansluter tooAzure VPN-gatewayer.

### <a name="are-there-any-other-asns-that-i-cant-use"></a>Finns det andra ASN-nummer som jag inte kan använda?
Ja, hello efter ASN: er är [reserverat av IANA](http://www.iana.org/assignments/iana-as-numbers-special-registry/iana-as-numbers-special-registry.xhtml) och kan inte konfigureras på din Azure VPN-Gateway:

23456, 64496-64511, 65535-65551 och 429496729

### <a name="can-i-use-hello-same-asn-for-both-on-premises-vpn-networks-and-azure-vnets"></a>Kan jag använda hello samma ASN för både lokala VPN-nätverk och virtuella Azure-nätverk?
Nej, måste du tilldela olika ASN:er mellan dina lokala nätverk och dina virtuella Azure-nätverk om du ska ansluta dem till varandra med BGP. Azure-VPN-gatewayer är tilldelade en standard-ASN som är 65515, oavsett om BGP är aktiverat eller inte för dina korsanslutningar. Du kan åsidosätta denna standardinställning genom att tilldela en annan ASN när du skapar hello VPN-gateway eller ändra hello ASN efter hello gateway har skapats. Du behöver tooassign dina lokala ASN: er toohello motsvarande Azure-lokala Nätverksgatewayer.

### <a name="what-address-prefixes-will-azure-vpn-gateways-advertise-toome"></a>Vilken adress prefix kommer Azure VPN-gatewayer att meddela toome?
Azure VPN-gateway kommer att meddela hello följande vägar tooyour lokala BGP-enheter:

* Dina VNet-adressprefixer
* Adressprefixer för varje lokal nätverksgateway anslutna toohello Azure VPN-gateway
* Vägar som lärts in från andra BGP peering sessioner anslutna toohello Azure VPN-gateway, **förutom standardrutten eller flera vägar överlappas av ett VNet-prefix**.

### <a name="can-i-advertise-default-route-00000-tooazure-vpn-gateways"></a>Kan jag annonsera standard väg (0.0.0.0/0) tooAzure VPN-gatewayer?
Ja.

Observera att detta tvingar alla VNet utgående trafik mot den lokala platsen och förhindrar hello VNet virtuella datorer från att acceptera offentlig kommunikation från hello Internet direkt, sådan RDP eller SSH från hello Internet toohello virtuella datorer.

### <a name="can-i-advertise-hello-exact-prefixes-as-my-virtual-network-prefixes"></a>Kan jag annonsera hello exakt prefix som min prefix för det virtuella nätverket?

Nej, reklam hello samma prefix som någon av dina virtuella nätverk adressprefix kommer att blockeras eller filtrerade efter hello Azure-plattformen. Du kan dock annonsera ett prefix som är en överordnad uppsättning av det du har i det virtuella nätverket. 

Om det virtuella nätverket används hello adressutrymme 10.0.0.0/16, kan du annonsera 10.0.0.0/8. Men du kan inte annonsera 10.0.0.0/16 eller 10.0.0.0/24.

### <a name="can-i-use-bgp-with-my-vnet-to-vnet-connections"></a>Kan jag använda BGP med mina VNet-till-VNet-anslutningar?
Ja, du kan använda BGP för både anslutningar mellan platser och VNet-till-VNet-anslutningar.

### <a name="can-i-mix-bgp-with-non-bgp-connections-for-my-azure-vpn-gateways"></a>Kan jag blanda BGP- med icke-BGP-anslutningar för mina Azure VPN- gatewayer?
Ja, du kan blanda både BGP och icke-BGP-anslutningar för hello samma Azure VPN-gateway.

### <a name="does-azure-vpn-gateway-support-bgp-transit-routing"></a>Stöder Azure VPN-gateway BGP-överföringsrutter?
Ja, BGP-överföringsrutter stöds, med undantag för hello som Azure VPN-gatewayer **inte** annonserar standard dirigerar tooother BGP-peers. tooenable transit Routning över flera Azure VPN-gatewayer, måste du aktivera BGP på alla mellanliggande VNet-till-VNet-anslutningar.

### <a name="can-i-have-more-than-one-tunnel-between-azure-vpn-gateway-and-my-on-premises-network"></a>Kan jag har fler än en tunnel mellan en Azure VPN-gateway och mitt lokala nätverk?
Ja, du kan skapa fler än en S2S-VPN-tunnel mellan en Azure VPN-gateway och ditt lokala nätverk. Observera att alla tunnlarna kommer att räknas av mot hello totala antalet tunnlar för dina Azure VPN-gatewayer och måste du aktivera BGP på båda tunnlar.

Om du har två redundanta tunnlar mellan din Azure VPN-gateway och en av dina lokala nätverk, kommer de förbruka 2 tunnlar av hello totala kvot för din Azure VPN-gateway (10 för Standard) och 30 för HighPerformance.

### <a name="can-i-have-multiple-tunnels-between-two-azure-vnets-with-bgp"></a>Kan jag har flera tunnlar mellan två Azure VNets med BGP?
Ja, men minst en av hello virtuella nätverks-gateway måste vara aktiv-aktiv konfiguration.

### <a name="can-i-use-bgp-for-s2s-vpn-in-an-expressroutes2s-vpn-co-existence-configuration"></a>Kan jag använda BGP för S2S VPN i en ExpressRoute/S2S-VPN samexistent konfiguration?
Ja. 

### <a name="what-address-does-azure-vpn-gateway-use-for-bgp-peer-ip"></a>Vilken adress använder Azure VPN-gateway för BGP-peer-IP?
hello Azure VPN-gatewayen allokerar en IP-adress från hello Gatewayundernäts-intervallet som definierats för hello virtuellt nätverk. Som standard är den hello näst sista adressen i intervallet hello. Till exempel om ditt Gatewayundernät 10.12.255.0/27, allt från 10.12.255.0 too10.12.255.31 hello BGP-Peer-IP-adress på hello Azure VPN-gatewayen vara 10.12.255.30. Du hittar den här informationen när du listar hello Azure VPN gatewayinformation.

### <a name="what-are-hello-requirements-for-hello-bgp-peer-ip-addresses-on-my-vpn-device"></a>Vad är hello kraven för hello BGP-Peer-IP-adresser på min VPN-enhet?
Din lokala BGP peer-adress **får inte** hello vara samma som hello offentliga IP-adressen för VPN-enhet. Använd en annan IP-adress på hello VPN-enhet för BGP-Peer-IP. Det kan vara en adress som tilldelats toohello loopback-gränssnittet på hello enhet. Ange adressen i hello motsvarande lokala nätverksgateway som representerar hello plats.

### <a name="what-should-i-specify-as-my-address-prefixes-for-hello-local-network-gateway-when-i-use-bgp"></a>Vad ska jag ange som Mina adressprefix för hello lokala Nätverksgatewayen när jag använder BGP?
Azure lokal nätverksgateway anger hello första adressprefixen för hello lokalt nätverk. Med BGP, måste du allokera hello värdprefixet (/ 32 prefix) för din BGP-Peer-IP-adress som hello-adressutrymmet för det lokala nätverket. Om BGP-Peer-IP är 10.52.255.254, ska du ange ”10.52.255.254/32” som localNetworkAddressSpace hello av hello lokala nätverksgateway som representerar det lokala nätverket. Detta är tooensure som hello Azure VPN-gatewayen upprättar hello BGP-sessionen via hello S2S VPN-tunnel.

### <a name="what-should-i-add-toomy-on-premises-vpn-device-for-hello-bgp-peering-session"></a>Vad bör jag lägga till toomy lokala VPN-enhet för hello BGP-peeringsessionen?
Du bör lägga till en värdrutt för hello Azure BGP peer-IP-adressen på din VPN-enhet som pekar toohello IPsec S2S VPN-tunnel. Om hello Azure VPN-Peer-IP är ”10.12.255.30”, bör du till exempel lägga till en värdrutt för ”10.12.255.30” med ett nexthop-gränssnitt för hello matchande IPsec-tunnelgränssnittet på din VPN-enhet.

