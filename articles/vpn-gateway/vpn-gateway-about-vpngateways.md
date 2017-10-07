---
title: "Översikt av VPN-Gateway: Skapa anslutningar mellan lokala VPN-anslutningar tooAzure virtuella nätverk | Microsoft Docs"
description: "Den här VPN-Gateway-översikten beskriver hello sätt tooconnect tooAzure virtuella nätverk med en VPN-anslutning via hello Internet. Översikten innehåller också diagram över grundläggande anslutningskonfigurationer."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 2358dd5a-cd76-42c3-baf3-2f35aadc64c8
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/05/2017
ms.author: cherylmc
ms.openlocfilehash: 899270734270632a5b12d56021c924e977725a7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="about-vpn-gateway"></a>Om VPN Gateway

En VPN-gateway är en typ av virtuell nätverksgateway som skickar krypterad trafik mellan en offentlig anslutning tooan lokal plats. Du kan också använda VPN-gatewayer toosend krypterad trafik mellan virtuella Azure-nätverk via hello Microsoft-nätverk. toosend krypterade nätverkstrafik mellan dina virtuella Azure-nätverket och den lokala platsen, måste du skapa en VPN-gateway för det virtuella nätverket.

Varje virtuellt nätverk kan ha endast en VPN-gateway, men du kan skapa flera anslutningar toohello samma VPN-gateway. Ett exempel på detta är en konfiguration med anslutning till flera platser. När du skapar flera anslutningar toohello samma VPN-gateway, alla VPN-tunnlar, inklusive punkt-till-plats-VPN kan dela hello bandbredd som är tillgängliga för hello gateway.

### <a name="whatis"></a>Vad är en virtuell nätverksgateway?

En virtuell nätverksgateway består av två eller flera virtuella datorer som är distribuerade tooa specifika undernät som kallas hello GatewaySubnet. hello virtuella datorer som finns i hello GatewaySubnet skapas när du skapar hello virtuell nätverksgateway. Virtuell nätverksgateway virtuella datorer är konfigurerade toocontain routningstabeller och gateway services specifika toohello gateway. Du kan inte konfigurera hello virtuella datorer som är en del av hello virtuell nätverksgateway direkt och du bör aldrig distribuera ytterligare resurser toohello GatewaySubnet.

När du skapar en virtuell nätverksgateway med hjälp av hello gateway-typ 'Vpn-, skapas en viss typ av virtuell nätverksgateway som krypterar trafik. en VPN-gateway. En VPN-gateway kan ta upp too45 minuter toocreate. Detta är eftersom hello virtuella datorer för hello VPN-gateway som är distribuerade toohello GatewaySubnet som konfigurerats med hello-inställningar som du angett. hello Gateway-SKU som du väljer bestämmer hur kraftfulla hello är.

## <a name="gwsku"></a>Gateway-SKU:er

[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

## <a name="configuring"></a>Konfigurera en VPN-gateway

En anslutning för VPN-gateway är beroende av flera resurser som är konfigurerade med specifika inställningar. De flesta av hello resurserna kan konfigureras separat, även om de måste konfigureras i en viss ordning i vissa fall.

### <a name="settings"></a>Inställningar

hello-inställningar som du har valt för varje resurs är kritiska toocreating anslutningen. Information om enskilda resurser och inställningar för VPN-gateway finns [Om inställningar för VPN-gateway](vpn-gateway-about-vpn-gateway-settings.md). hello artikeln innehåller information toohelp du förstår gateway-typer, VPN-typer, anslutningstyper, gateway-undernät, lokala nätverksgatewayer och andra resursinställningar som du kan ha tooconsider.

### <a name="tools"></a>Distributionsverktyg

Du kan börja skapa och konfigurera resurser med hjälp av en konfiguration med till exempel hello Azure-portalen. Du kan senare bestämmer dig för tooswitch tooanother verktyg, till exempel PowerShell tooconfigure ytterligare resurser, eller ändra befintliga resurser när så är tillämpligt. För närvarande kan konfigurera du inte varje resurs och inställning i hello Azure-portalen. hello-instruktioner i hello artiklar för varje anslutning topologi ange när en specifik konfigurationsverktyget krävs. 

### <a name="models"></a>Distributionsmodell

När du konfigurerar en VPN-gateway kan beroende du vidta åtgärder för hello hello distributionsmodell som du använde toocreate ditt virtuella nätverk. Till exempel om du har skapat ditt VNet med hjälp av hello klassiska distributionsmodellen du använder hello riktlinjer och instruktioner för distribution av hello klassiska modellen toocreate och konfigurera inställningarna för VPN-gateway. Mer information om distributionsmodellerna finns i [Förstå Resource Manager- och klassiska distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="diagrams"></a>Diagram för anslutningstopologi

Det är viktigt tooknow att det finns olika konfigurationer för VPN-gateway-anslutningar. Du måste toodetermine vilken konfiguration som bäst passar dina behov. I nedanstående hello avsnitt, kan du visa information och topologi diagram om hello följande VPN gateway-anslutningar: hello följande avsnitt innehåller tabeller som visa:

* tillgänglig distributionsmodell
* tillgängliga konfigurationsverktyg
* Länkar direkt tooan artikel, om tillgängligt

Använd hello diagram och beskrivningar toohelp väljer hello anslutning topologi toomatch dina krav. hello diagram visar hello huvudsakliga baslinje-topologier, men det är möjligt toobuild mer komplexa konfigurationer med hello diagram som en riktlinje.

## <a name="s2smulti"></a>Plats-till-plats och flera platser (IPsec/IKE VPN-tunnel)

### <a name="S2S"></a>Plats-till-plats-anslutning

En plats-till-plats-anslutning (S2S) för VPN-gateway är en anslutning via en VPN-tunnel med IPsec/IKE (IKEv1 eller IKEv2). En S2S-anslutning kräver en VPN-enhet som finns lokalt som har en offentlig IP-adress som tilldelats tooit och som inte finns bakom en NAT S2S-anslutningar kan användas för konfigurationer mellan platser och för hybridkonfigurationer.   

![Exempel på Azure VPN Gateway-anslutningar för plats-till-plats](./media/vpn-gateway-about-vpngateways/vpngateway-site-to-site-connection-diagram.png)

### <a name="Multi"></a>Anslutning till flera platser

Den här typen av anslutning är en variation av hello plats-till-plats-anslutning. Du kan skapa fler än en VPN-anslutning från din virtuella nätverksgateway, vanligtvis ansluta toomultiple lokala platser. När du arbetar med flera anslutningar måste du använda en RouteBased VPN-typ (kallas även en ”dynamisk gateway” för klassiska virtuella nätverk). Eftersom varje virtuellt nätverk kan bara ha en VPN-gateway, delar alla anslutningar via hello gateway hello tillgänglig bandbredd. Det här kallas ofta en ”anslutning till flera platser”.

![Exempel på Azure VPN Gateway-anslutningar för flera platser](./media/vpn-gateway-about-vpngateways/vpngateway-multisite-connection-diagram.png)

### <a name="deployment-models-and-methods-for-site-to-site-and-multi-site"></a>Distributionsmodeller och metoder för plats-till-plats och flera platser

[!INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)]

## <a name="P2S"></a>Punkt-till-plats (VPN över SSTP)

En punkt-till-plats (P2S) VPN-gateway kan du skapa en säker anslutning tooyour virtuellt nätverk från en enskild klientdator. Punkt-till-plats VPN-anslutningar är användbara när du vill tooconnect tooyour virtuella nätverk från en annan plats, exempelvis när du hemifrån home eller en konferens. P2S VPN är också en bra lösning toouse i stället för en plats-till-plats VPN-anslutning när du har bara ett fåtal klienter som behöver tooconnect tooa VNet. 

Till skillnad från S2S-anslutningar, kräver P2S-anslutningar en lokal offentlig IP-adress eller en VPN-enhet. P2S-anslutningar kan användas med S2S-anslutningar via hello samma VPN-gateway, så länge som alla hello konfigurationskrav för både anslutningar är kompatibla.

P2S använder hello Secure Socket Tunneling Protocol (SSTP), vilket är ett SSL-baserad VPN-protokoll. En P2S VPN-anslutning upprättas genom att starta från hello-klientdator.

![Exempel på Azure VPN-Gateway-anslutningar för punkt-till-plats](./media/vpn-gateway-about-vpngateways/vpngateway-point-to-site-connection-diagram.png)

### <a name="deployment-models-and-methods-for-point-to-site"></a>Distributionsmodeller och metoder för punkt-till-plats

[!INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="V2V"></a>Anslutningar mellan virtuella nätverk (IPsec/IKE VPN-tunnel)

Ansluta ett virtuellt nätverk tooanother virtuellt nätverk (VNet-till-VNet) är liknande tooconnecting ett VNet tooan lokal plats. Båda typerna av anslutningen använder en VPN-gateway tooprovide en säker tunnel med IPsec/IKE. Du kan till och med kombinera VNet-till-VNet-kommunikation med anslutningskonfigurationer för flera platser. Det innebär att du kan etablera nätverkstopologier som kombinerar anslutningen för flera platser med en intern virtuell nätverksanslutning.

Hej Vnet som du ansluter kan vara:

* i hello samma eller olika regioner
* i hello samma eller olika prenumerationer 
* i hello samma eller olika distributionsmodeller

![Azure VPN-Gateway VNet tooVNet anslutning exempel](./media/vpn-gateway-about-vpngateways/vpngateway-vnet-to-vnet-connection-diagram.png)

### <a name="connections-between-deployment-models"></a>Anslutningar mellan distributionsmodeller

Azure har för närvarande två distributionsmodeller: klassisk och Resource Manager. Om du har använt Azure ett tag har du förmodligen virtuella Azure-datorer och instansroller som kör i ett klassiskt VNet. Dina nyare virtuella datorer och rollinstanser kanske körs i ett VNet som skapats i Resource Manager. Du kan skapa en anslutning mellan hello Vnet tooallow hello resurser i ett virtuellt nätverk toocommunicate direkt med resurser i en annan.

### <a name="vnet-peering"></a>VNet-peering

Du kan toouse VNet-peering toocreate anslutningen, så länge som det virtuella nätverket uppfyller vissa krav. Ingen VNet-gateway används för VNet-peering. Mer information finns i [VNet peering (Vnet-peering)](../virtual-network/virtual-network-peering-overview.md).

### <a name="deployment-models-and-methods-for-vnet-to-vnet"></a>Distributionsmodeller och metoder för VNet-till-VNet

[!INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## <a name="ExpressRoute"></a>ExpressRoute (dedicerad privat anslutning)

Microsoft Azure ExpressRoute kan du utöka ditt lokala nätverk till hello Microsoft cloud via en dedikerad privata anslutning underlättas av en provider för anslutningen. Du kan upprätta anslutningar molntjänster tooMicrosoft, till exempel Microsoft Azure, Office 365 och CRM Online med ExpressRoute. Anslutningen kan vara från ett ”any-to-any”-nätverk (IP VPN), ett ”point-to-point”-nätverk med Ethernet eller en virtuell korsanslutning via en anslutningsleverantör på en samlokaliseringsanläggning.

ExpressRoute-anslutningar inte överskrider hello offentliga Internet. Detta tillåter ExpressRoute anslutningar toooffer mer tillförlitlighet, högre hastighet, lägre latens och högre säkerhet än vanliga anslutningar via hello Internet.

En ExpressRoute-anslutning använder inte en VPN-gateway, men en virtuell nätverksgateway används som en del i den obligatoriska konfigurationen. Hello virtuell nätverksgateway har konfigurerats med hello gateway-typ 'ExpressRoute', i stället för ”Vpn' i en ExpressRoute-anslutning. Mer information om ExpressRoute finns hello [teknisk översikt för ExpressRoute](../expressroute/expressroute-introduction.md).

## <a name="coexisting"></a>Plats-till-plats- och samexisterande ExpressRoute-anslutningar

ExpressRoute är en direkt dedikerad anslutning från din WAN (inte över hello offentliga Internet) tooMicrosoft tjänster, inklusive Azure. Plats-till-plats VPN-trafik resor krypteras över hello offentliga Internet. Som kan tooconfigure anslutningar för plats-till-plats VPN- och ExpressRoute för hello samma virtuella nätverk har flera fördelar.

Du kan konfigurera en plats-till-plats-VPN som en säker redundans sökväg för ExpressRoute eller använda plats-till-plats VPN tooconnect toosites som inte är del av nätverket, men som är anslutna via ExpressRoute. Observera att den här konfigurationen krävs två virtuella nätverks-gateway för hello samma virtuella nätverk, ett med hjälp av hello gateway-typ 'Vpn- och hello annan med hello gateway-typ 'ExpressRoute'.

![Exempel på samtidiga ExpressRoute- och VPN Gateway-anslutningar](./media/vpn-gateway-about-vpngateways/expressroute-vpngateway-coexisting-connections-diagram.png)

### <a name="deployment-models-and-methods-for-s2s-and-expressroute"></a>Distributionsmodeller och metoder för S2S och ExpressRoute

[!INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)]

## <a name="pricing"></a>Prissättning

[!INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)]

Se [Gateway-SKU:er](vpn-gateway-about-vpn-gateway-settings.md#gwsku) för information om gateway-SKU:er för VPN-gateway.

## <a name="faq"></a>Vanliga frågor och svar

Vanliga frågor om VPN-gateway finns hello [VPN-Gateway FAQ](vpn-gateway-vpn-faq.md).

## <a name="next-steps"></a>Nästa steg

- Planera konfigurationen av din VPN-gateway. Se [Planering och design för VPN-gateway](vpn-gateway-plan-design.md).
- Visa hello [VPN-Gateway FAQ](vpn-gateway-vpn-faq.md) för ytterligare information.
- Visa hello [prenumeration och tjänstbegränsningarna](../azure-subscription-service-limits.md#networking-limits).
- Lär dig mer om hello andra nyckeln [nätverk](../networking/networking-overview.md) Azure.
