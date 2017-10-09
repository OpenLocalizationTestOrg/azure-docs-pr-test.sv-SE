hello visar följande tabell hello gateway-typer och hello uppskattade sammanställda genomflöde av gateway-SKU. Den här tabellen gäller toohello Resource Manager och klassiska distributionsmodeller. 

Prissättningen skiljer sig åt mellan gateway-SKU:er. Mer information finns i [Prissättning för VPN-gateway](https://azure.microsoft.com/pricing/details/vpn-gateway).

Observera att hello UltraPerformance gateway-SKU inte representeras i den här tabellen. Information om hello UltraPerformance SKU finns hello [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md) dokumentation.

|  | **VPN Gateway-genomflöde (1)** | **VPN Gateway, max. IPsec-tunnlar (2)** | **ExpressRoute-gateway, genomflöde** | **VPN-gateway och ExpressRoute samexisterar** |
| --- | --- | --- | --- | --- |
| **Basic SKU (3)(5)(6)** |100 Mbit/s |10 |500 Mbit/s (6) |Nej |
| **Standard SKU (4)(5)** |100 Mbit/s |10 |1000 Mbps |Ja |
| **High Performance SKU (4)** |200 Mbit/s |30 |2000 Mbps |Ja |


(1) hello VPN-genomströmning är en grov uppskattning baserat på hello mätningar mellan Vnet i hello samma Azure-region. Det är inte en garanterad genomströmning för anslutningar mellan platser över hello Internet. Det är hello högsta möjliga genomströmningen mått.

(2) hello antalet tunnlar innebär tooRouteBased VPN. En principbaserad VPN stöder bara en VPN-tunnel av typen plats-till-plats.

(3) BGP stöds inte för hello grundläggande SKU.

(4) Principbaserade VPN:er stöds inte för den här SKU:n. De stöds för hello grundläggande SKU.

(5) S2S VPN-Gateway-anslutningar av typen aktiv-aktiv stöds inte för denna SKU. Aktiv-aktiv stöds på hello HighPerformance SKU.

(6) grundläggande SKU är föråldrad för användning med ExpressRoute.
