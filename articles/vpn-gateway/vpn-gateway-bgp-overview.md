---
title: aaaOverview av BGP med Azure VPN-gatewayer | Microsoft Docs
description: "Den här artikeln innehåller en översikt över BGP med Azures VPN-gatewayer."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: f8c3985c-c128-4f34-835c-0e88742bf36e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/12/2017
ms.author: yushwang
ms.openlocfilehash: ced3f77ecd791c84fb72b96447e839be3bf94846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-bgp-with-azure-vpn-gateways"></a>Översikt över BGP med Azures VPN-gatewayer
Den här artikeln innehåller en översikt över BGP-stöd (Border Gateway Protocol) i Azures VPN-gatewayer.

BGP är hello standard routingprotokoll ofta används i hello Internet tooexchange Routning och reachability information mellan två eller flera nätverk. När användas i hello kontext med virtuella Azure-nätverk, kan BGP hello Azure VPN-gatewayer och din lokala VPN-enheter som kallas BGP-peers eller grannar, tooexchange ”dirigerar” som informerar båda gateways på hello tillgänglighet och tillgänglighet för dem prefix toogo via hello gateways eller routrar som ingår. BGP kan också aktivera transitroutning mellan flera nätverk av sprider vägar som en gateway med BGP lär sig från en BGP peer tooall andra BGP-peers. 

## <a name="why-use-bgp"></a>Varför ska man använda BGP?
BGP är en valfri funktion som du kan använda med Azures routningsbaserade VPN-gateways. Du bör också kontrollera din lokala VPN-enheter stöder BGP innan du aktiverar hello-funktionen. Du kan fortsätta toouse Azure VPN-gatewayer och lokala VPN-enheter utan BGP. Det är hello motsvarande för att använda statiska vägar (utan BGP) *kontra* med hjälp av dynamisk routning med BGP mellan ditt nätverk och Azure.

Det finns flera fördelar och nya funktioner med BGP:

### <a name="support-automatic-and-flexible-prefix-updates"></a>Stöd för automatiska och flexibla prefixuppdateringar
Med BGP behöver du bara toodeclare en minsta prefixet tooa specifika BGP-peer för hello IPsec S2S VPN-tunneln. Det kan vara så liten som en värdprefixet (/ 32) av hello BGP peer-IP-adressen för din lokala VPN-enhet. Du kan styra vilka lokala nätverksprefix som du vill tooadvertise tooAzure tooallow Azure Virtual Network-tooaccess.

Du kan också annonsera större prefix som kan innehålla några av dina VNet-adressprefixer, till exempel en stor privata IP-adressutrymme (till exempel 10.0.0.0/8). Observera att hello prefix inte får vara identiskt med något av VNet-prefix. Dessa vägar identiska tooyour VNet-prefix avvisas.

### <a name="support-multiple-tunnels-between-a-vnet-and-an-on-premises-site-with-automatic-failover-based-on-bgp"></a>Stöd för flera tunnlar mellan ett VNet och en lokal plats med automatisk redundans baserat på BGP
Du kan skapa flera anslutningar mellan ditt Azure VNet och lokala VPN-enheter i hello samma plats. Den här funktionen innehåller flera tunnlar (sökvägar) mellan hello två nätverk i en aktiv-aktiv konfiguration. Om en av hello tunnlar kopplas hello motsvarande vägar utgår via BGP och hello trafik flyttas automatiskt toohello återstående tunnlar.

hello följande diagram visar ett enkelt exempel på hög tillgänglighet installationen:

![Flera aktiva sökvägar](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

### <a name="support-transit-routing-between-your-on-premises-networks-and-multiple-azure-vnets"></a>Stöd för överföringsroutning mellan dina lokala nätverk och flera Azure-VNets
BGP kan flera gateways toolearn och sprida prefix från flera olika nätverk, om de är direkt eller indirekt anslutna. Detta kan möjliggöra överföringsroutning med Azures VPN- gatewayer mellan lokala platser eller över flera virtuella Azure-nätverk.

hello visar följande diagram ett exempel på en Multi-hop topologi med flera sökvägar som kan transit trafik mellan hello två lokala nätverk via Azure VPN-gatewayer i hello Microsoft Networks:

![Multihoppöverföring](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## <a name="bgp-faq"></a>BGP VANLIGA FRÅGOR OCH SVAR
[!INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)]

## <a name="next-steps"></a>Nästa steg
Se [komma igång med BGP på Azure VPN-gatewayer](vpn-gateway-bgp-resource-manager-ps.md) för steg tooconfigure BGP för anslutningar mellan platser och VNet-till-VNet-anslutningar.

