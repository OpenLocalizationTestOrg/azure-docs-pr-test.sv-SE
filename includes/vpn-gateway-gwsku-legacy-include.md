hello äldre (gamla) VPN-gateway-SKU: er är:

* Basic
* Standard
* HighPerformance

Hej UltraPerformance gateway SKU används inte av VPN-Gateway. Information om hello UltraPerformance SKU finns hello [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md) dokumentation.

När du arbetar med hello äldre SKU: er bör du överväga att hello följande:

* Om du vill toouse en PolicyBased VPN-typ, måste du använda hello grundläggande SKU. PolicyBased VPN (tidigare kallade statisk routning) stöds inte på andra SKU:er.
* BGP stöds inte på hello grundläggande SKU.
* ExpressRoute-VPN-Gateway samexistera konfigurationer stöds inte på hello grundläggande SKU.
* Aktiv-aktiv S2S VPN-Gateway-anslutningar kan konfigureras på hello HighPerformance SKU.
