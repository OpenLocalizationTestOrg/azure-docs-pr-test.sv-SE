---
title: "Planering och design för anslutningar mellan platser: Azure VPN-Gateway | Microsoft Docs"
description: "Lär dig mer om VPN-Gateway planering och design för anslutningar mellan platser, hybrid och VNet-till-VNet-anslutningar"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: d5aaab83-4e74-4484-8bf0-cc465811e757
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/27/2017
ms.author: cherylmc
ms.openlocfilehash: 3d4587ba31d163384212eca88a7e2c0ba8f3b21f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="planning-and-design-for-vpn-gateway"></a>Planering och design för VPN-gateway

Planera och utforma din mellan platser och VNet-till-VNet-konfigurationer kan vara antingen enkla eller komplexa, beroende på dina nätverk behov. Den här artikeln vägleder dig genom grundläggande överväganden för planering och design.

## <a name="planning"></a>Planera

### <a name="compare"></a>Alternativ för nätverksanslutning mellan platser

Om du vill tooconnect dina lokala platser på ett säkert sätt tooa virtuellt nätverk, du har tre olika sätt toodo så: plats-till-plats, punkt-till-plats och ExpressRoute. Jämför hello olika anslutningar mellan platser som är tillgängliga. hello alternativ du väljer kan bero på olika överväganden som:

* Vilken typ av dataflöde kräver din lösning?
* Vill du toocommunicate över hello offentliga Internet via en säker VPN eller över en privat anslutning?
* Har du en offentlig IP-adress tillgängliga toouse?
* Planerar du toouse en VPN-enhet? I så fall, är den kompatibel?
* Ansluter du bara några få datorer eller vill du ha en permanent anslutning för platsen?
* Vilken typ av VPN-gateway krävs för hello lösningen som du vill toocreate?
* Vilken gateway SKU bör du använda?

### <a name="planningtable"></a>Planera tabell

hello i den följande tabellen kan hjälpa dig avgöra hello bästa anslutningsmöjlighet för lösningen.

[!INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]

### <a name="gwsku"></a>Gateway-SKU:er

[!INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

### <a name="wf"></a>Arbetsflöde

hello hello följande lista beskrivs vanliga arbetsflöde för moln-anslutningen:

1. Utforma och planera din anslutning topologi och lista hello adressutrymmen för alla nätverk du vill tooconnect.
2. Skapa ett virtuellt Azure-nätverk. 
3. Skapa en VPN-gateway för hello virtuella nätverket.
4. Skapa och konfigurera anslutningar tooon lokala nätverk eller andra virtuella nätverk (vid behov).
5. Skapa och konfigurera en punkt-till-plats-anslutning för din Azure VPN-gateway (vid behov).

## <a name="design"></a>Design
### <a name="topologies"></a>Topologier för anslutning

Starta genom att titta på hello diagrammen i hello [om VPN-Gateway](vpn-gateway-about-vpngateways.md) artikel. hello artikeln innehåller vanliga diagram, hello distributionsmodeller för varje topologi och hello tillgänglig distributionsverktyg som du kan använda toodeploy din konfiguration.

### <a name="designbasics"></a>Design-grunderna

hello följande avsnitt beskrivs grunderna för hello VPN-gateway. 

#### <a name="servicelimits"></a>Nätverk tjänster gränser

Rulla hello tabeller tooview [nätverk services gränser](../azure-subscription-service-limits.md#networking-limits). hello begränsningarna kan påverka din design.

#### <a name="subnets"></a>Om undernät

När du skapar anslutningar måste du undernät-intervall. Du kan inte ha överlappande undernät-adressintervall. Ett annat överlappande undernät är när en virtuell nätverks- eller lokal plats innehåller hello innehåller samma adressutrymme som hello annan plats. Det innebär att du måste nätverk-tekniker för din lokala lokalt nätverk toocarve ut ett intervall för du toouse för din Azure-IP-adressering utrymme-undernät. Du måste adressutrymme som inte används på hello lokala lokalt nätverk.

Undvika överlappande undernät är också viktigt när du arbetar med VNet-till-VNet-anslutningar. Om dina undernät överlappar varandra och en IP-adress finns i både skicka hello- och målservrar Vnet, misslyckas VNet-till-VNet-anslutningar. Azure kan inte vidarebefordra hello data toohello andra VNet eftersom hello måladress är en del av hello skicka VNet.

VPN-gatewayer kräver ett specifikt undernät som kallas en gateway-undernätet. Alla gateway-undernät måste ha namnet GatewaySubnet toowork korrekt. Glöm inte tooname din gateway-undernät med ett annat namn och inte distribuera virtuella datorer eller något annat toohello gateway-undernätet. Se [Gateway-undernät](vpn-gateway-about-vpn-gateway-settings.md#gwsub).

#### <a name="local"></a>Om lokala nätverksgatewayer

hello lokal nätverksgateway refererar vanligtvis tooyour lokal plats. Hello lokal nätverksgateway är refererad tooas en lokal nätverksplats i hello klassiska distributionsmodellen. När du konfigurerar en lokal nätverksgateway kan du ge det ett namn Ange hello offentliga IP-adressen för hello lokala VPN-enhet och ange hello adressprefix i hello lokal plats. Azure tittar på hello mål-adressprefix för nätverkstrafik, tittar hello-konfiguration som du har angett för hello lokal nätverksgateway och därefter dirigerar paket. Du kan ändra hello adressprefix efter behov. Mer information finns i [lokala nätverksgatewayer](vpn-gateway-about-vpn-gateway-settings.md#lng).

#### <a name="gwtype"></a>Om gateway-typer

Det är viktigt att välja hello rätt gateway-typ för din topologi. Om du väljer hello fel typ fungerar din gateway inte korrekt. hello gateway-typen anger hur själva hello gatewayen ansluter och är en konfigurationsinställning som krävs för hello Resource Manager-modellen.

hello gateway typer är:

* Vpn
* ExpressRoute

#### <a name="connectiontype"></a>Om anslutningstyper

Varje konfiguration kräver en viss anslutningstyp. hello anslutningstyper är:

* IPsec
* Vnet2Vnet
* ExpressRoute
* VPNClient

#### <a name="vpntype"></a>Om VPN-typer

Varje konfigurationen kräver en viss typ av VPN. Om du kombinerar två konfigurationer, till exempel skapa en plats-till-plats-anslutning och en punkt-till-plats-anslutning toohello samma virtuella nätverk, måste du använda en VPN-typ som uppfyller båda anslutning krav.

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

hello följande tabeller visar hello VPN-typ som mappas tooeach anslutningskonfiguration. Se till att hello VPN-typ för din gateway matchar hello-konfiguration som du vill toocreate. 

[!INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)]

### <a name="devices"></a>Enheter för VPN för plats-till-plats-anslutningar

tooconfigure plats-till-plats-anslutning, oavsett distributionsmodell, behöver du hello följande objekt:

* En VPN-enhet som är kompatibel med Azure VPN-gatewayer
* En offentlig IPv4-adress som inte finns bakom en NAT

Du behöver toohave erfarenhet att konfigurera din VPN-enhet eller har någon som du kan konfigurera hello enhet för dig.

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

### <a name="forcedtunnel"></a>Överväg att framtvingad tunnel Routning

Du kan konfigurera Tvingad tunneling för de flesta konfigurationer. Tvingad tunneltrafik kan du omdirigera eller ”force” alla Internet-bunden trafik tillbaka tooyour lokal plats via en plats-till-plats VPN-tunnel för kontroll och granskning. Detta är en kritisk säkerhetskraven för de flesta företags IT principer. 

Utan Tvingad tunneling ska Internet-bunden trafik från din virtuella datorer i Azure alltid passerar från Azure nätverksinfrastruktur direkt ut toohello Internet, utan hello alternativet tooallow du tooinspect eller audit hello-trafik. Obehörig Internetåtkomst kan leda tooinformation avslöjas eller andra typer av säkerhetsintrång.

En anslutning för Tvingad tunneltrafik kan konfigureras i båda distributionsmodellerna och med hjälp av olika verktyg. Mer information finns i [konfigurera Tvingad tunneltrafik](vpn-gateway-forced-tunneling-rm.md).

**Tvingad tunneltrafik diagram**

![Azure VPN-Gateway Tvingad tunneltrafik diagram](./media/vpn-gateway-plan-design/forced-tunneling-diagram.png)

## <a name="next-steps"></a>Nästa steg

Se hello [VPN-Gateway FAQ](vpn-gateway-vpn-faq.md) och [om VPN-Gateway](vpn-gateway-about-vpngateways.md) artiklar för mer information toohelp du med din design.

Mer information om inställningar för specifika gatewayen finns [om VPN-Gateway inställningar](vpn-gateway-about-vpn-gateway-settings.md).