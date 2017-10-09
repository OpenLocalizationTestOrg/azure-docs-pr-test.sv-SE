När du skapar en virtuell nätverksgateway måste toospecify hello gateway SKU som du vill toouse. Välj hello SKU: er som uppfyller dina krav baserat på hello typer av arbetsbelastningar, genomflöden, funktioner och servicenivåavtal.

[!INCLUDE [classic SKU](./vpn-gateway-classic-sku-support-include.md)]

[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

###  <a name="workloads"></a>Produktion *jämfört med* Arbetsbelastningar för utvecklingstest

På grund av toohello skillnader i SLA: er och funktioner, rekommenderar vi följande SKU: er för produktion hello *kontra* dev-test:

| **Arbetsbelastning**                       | **SKU: er**               |
| ---                                | ---                    |
| **Produktion, kritiska arbetsbelastningar** | VpnGw1 VpnGw2, VpnGw3 |
| **Utv-test eller konceptbevis**   | Basic                  |
|                                    |                        |

Om du använder gamla hello är SKU: er, hello produktion SKU rekommendationer Standard och HighPerformance SKU: er. Mer information om hello gamla SKU: er finns [Gateway-SKU: er (äldre SKU: er)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md).

###  <a name="feature"></a>Funktionsuppsättningarna för gateway-SKU:er

hello ny gateway SKU: er förenkla hello funktionsuppsättningar erbjuds via hello gatewayer:

| **SKU**| **Funktioner**|
| ---    | ---         |
|**Basic**   | **Ruttbaserad VPN**: 10 tunnlar med P2S<br><br>**Principbaserad VPN**: (IKEv1): 1 tunnel; ingen P2S|
| **VpnGw1, VpnGw2 och VpnGw3** | **Ruttbaserad VPN**: upp too30 tunnlar (*), P2S, BGP, aktiv-aktiv, anpassade IPsec/IKE-principen, ExpressRoute/VPN samexistent |
|        |             |

(*) Du kan konfigurera ”PolicyBasedTrafficSelectors” tooconnect en ruttbaserad VPN-gateway (VpnGw1, VpnGw2, VpnGw3) toomultiple lokala principbaserad brandväggen enheter. Se för[Anslut VPN-gatewayer toomultiple lokalt principbaserad VPN-enheter med hjälp av PowerShell](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) mer information.

###  <a name="resize"></a>Ändra storlek på gateway-SKU: er

1. Du kan ändra storlek mellan VpnGw1, VpnGw2 och VpnGw3 SKU: er.
2. När du arbetar med hello gamla gateway-SKU: er, kan du ändra storlek på mellan Basic, Standard och HighPerformance SKU: er.
2. Du **kan** ändra storlek från SKU: er för Basic/Standard/HighPerformance toohello nya VpnGw3-VpnGw1/VpnGw2 SKU: er. Du måste i stället [migrera](#migrate) toohello nya SKU: er.

###  <a name="migrate"></a>Migrera från den gamla SKU: er toohello nya SKU: er

[!INCLUDE [Migrate SKU](./vpn-gateway-migrate-legacy-sku-include.md)]
