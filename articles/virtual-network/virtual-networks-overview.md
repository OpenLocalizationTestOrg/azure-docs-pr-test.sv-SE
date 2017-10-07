---
title: "aaaAzure virtuellt nätverk | Microsoft Docs"
description: "Läs mer om Azure Virtual Network koncept och funktioner."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 9633de4b-a867-4ddf-be3c-a332edf02e24
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/23/2017
ms.author: jdial
ms.openlocfilehash: 55ae6a131d882ad893aeffcaa4127bc47beda552
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-network"></a>Azure Virtual Network

hello Azure Virtual Network service aktiverar du toosecurely ansluta Azure-resurser tooeach andra med virtuella nätverk (Vnet). Ett virtuellt nätverk är en representation av ditt eget nätverk i hello molnet. Ett virtuellt nätverk är en logisk isolering av hello Azure-molnet dedikerad tooyour prenumeration. Du kan också ansluta Vnet tooyour lokalt nätverk. hello följande bild visar några av hello funktionerna i hello Azure Virtual Network service:

![Nätverksdiagram](./media/virtual-networks-overview/virtual-network-overview.png)

toolearn mer om hello följande Azure Virtual Network-funktioner, klickar du på hello kapaciteten:
- **[Isolering:](#isolation) ** Vnet isoleras från varandra. Du kan skapa separata Vnet för utveckling, testning och produktion som Använd hello samma CIDR-Adressblock. Däremot kan du skapa flera virtuella nätverk som använder olika CIDR-Adressblock och ansluta nätverk tillsammans. Du kan dela ett VNet i flera undernät. Azure erbjuder intern namnmatchning för virtuella datorer och molntjänster rollinstanser anslutna tooa VNet. Du kan också konfigurera ett virtuellt nätverk toouse egna DNS-servrar, istället för att använda intern namnmatchning för Azure.
- **[Internet-anslutning:](#internet) ** alla Azure virtuella datorer (VM) och molntjänster rollinstanser anslutna tooa VNet har åtkomst till toohello Internet, som standard. Du kan också aktivera inkommande åtkomst toospecific resurser efter behov.
- **[Azure-resurs-anslutningen:](#within-vnet) ** Azure-resurser som molntjänster och virtuella datorer kan vara anslutna toohello samma virtuella nätverk. hello resurser kan ansluta tooeach andra använder privata IP-adresser, även om de finns i olika undernät. Azure tillhandahåller standardroutning mellan undernät, virtuella nätverk och lokala nätverk, så att du inte har tooconfigure och hantera vägar.
- **[VNet-anslutningen:](#connect-vnets) ** Vnet kan vara anslutna tooeach andra, aktivera resurser anslutna tooany VNet toocommunicate med alla resurser för andra virtuella nätverk.
- **[Lokal anslutning:](#connect-on-premises) ** Vnet kan vara anslutna tooon lokala nätverk via privata nätverksanslutningar mellan ditt nätverk och Azure, eller en plats-till-plats VPN-anslutning via hello Internet.
- **[Trafikfiltrering:](#filtering) ** molntjänster och Virtuella nätverkstrafik för rollen instanser inkommande och utgående kan filtreras efter källans IP-adress och port, mål-IP-adress och port och protokoll.
- **[Routning:](#routing) ** Alternativt kan du åsidosätta Azures standard routning genom att konfigurera egna vägar eller använda BGP-vägar via en nätverksgateway.

## <a name = "isolation"></a>Isolering av nätverk och segmentering

Du kan implementera flera Vnet i varje Azure [prenumeration](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) och Azure [region](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region). Varje virtuellt nätverk är isolerad från andra Vnet. För varje virtuellt nätverk kan du:
- Ange en egen privata IP-adressutrymmet med hjälp av offentliga och privata (RFC 1918)-adresser. Azure tilldelar resurser anslutna toohello VNet en privat IP-adress från hello adressutrymme som du tilldelar.
- Segmentera hello VNet i en eller flera undernät och tilldela en del av hello VNet adressutrymme tooeach undernät.
- Använd Azure-tillhandahållna namnmatchning eller ange egna DNS-server för användning av resurser anslutna tooa VNet. Mer information om namnmatchning i Vnet, läsa hello toolearn [namnmatchning för virtuella datorer och molntjänster](virtual-networks-name-resolution-for-vms-and-role-instances.md) artikel.

## <a name = "internet"></a>Ansluta toohello Internet
Alla resurser anslutna tooa VNet har utgående anslutning toohello Internet som standard. hello privata IP-adressen för hello resursen är källan nätverksadress översättas (SNAT) tooa offentlig IP-adress av hello Azure-infrastrukturen. Mer om utgående anslutning till Internet, läsa hello toolearn [förstå utgående anslutningar i Azure](..\load-balancer\load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json#standalone-vm-with-no-instance-level-public-ip-address) artikel. Du kan ändra hello standard anslutningen genom att implementera anpassad Routning och filtrera trafik.

toocommunicate inkommande tooAzure resurser från hello Internet eller toocommunicate utgående toohello Internet utan SNAT, en resurs måste tilldelas en offentlig IP-adress. Mer om den offentliga IP-adresser, läsa hello toolearn [offentliga IP-adresser](virtual-network-public-ip-address.md) artikel.

## <a name="within-vnet"></a>Anslut Azure-resurser
Du kan ansluta flera Azure-resurser tooa VNet, till exempel virtuella datorer (VM), Cloud Services, Apptjänstmiljöer och Skalningsuppsättningar i virtuella datorer. Virtuella datorer ansluta tooa undernät inom ett VNet till ett nätverksgränssnitt (NIC). Mer om nätverkskort, läsa hello toolearn [nätverksgränssnitt](virtual-network-network-interface.md) artikel.

## <a name="connect-vnets"></a>Ansluta virtuella nätverk

Du kan ansluta Vnet tooeach andra är att aktivera resurser anslutna tooeither VNet toocommunicate med varandra över Vnet. Du kan använda ett eller båda av följande alternativ tooconnect Vnet tooeach andra hello:
- **Peering:** aktiverar resurser anslutna toodifferent Azure Vnet inom hello samma Azure-plats toocommunicate med varandra. hello bandbredd och fördröjning mellan hello Vnet är hello samma som om hello resurserna var anslutna toohello samma virtuella nätverk. toolearn mer information om peering, läsa hello [virtuella nätverk peering](virtual-network-peering-overview.md) artikel.
- **VNet-till-VNet-anslutningen:** aktiverar resurser anslutna toodifferent Azure VNet inom hello samma eller olika Azure-platser. Till skillnad från peering, är bandbredd eftersom begränsade mellan Vnet trafik måste gå genom en Azure VPN-Gateway. Mer om hur du ansluter Vnet med ett VNet-till-VNet-anslutningen, läsa hello toolearn [konfigurera VNet-till-VNet-anslutningen](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel.

## <a name="connect-on-premises"></a>Ansluta tooan lokalt nätverk

Du kan ansluta dina lokala nätverk tooa VNet med valfri kombination av hello följande alternativ:
- **Punkt-till-plats virtuellt privat nätverk (VPN):** upprättas mellan en enskild dator ansluten tooyour nätverk och hello VNet. Den här anslutningen är bra om du precis har börjat med Azure, eller för utvecklare, eftersom det krävs lite eller ingen ändringar tooyour befintliga nätverk. hello anslutningen använder hello SSTP-protokollet tooprovide krypterad kommunikation över hello Internet mellan hello PC och hello virtuella nätverk. hello svarstid för en punkt-till-plats-VPN är oförutsägbart, eftersom hello trafik färdas genom hello Internet.
- **Plats-till-plats-VPN:** mellan din VPN-enhet och en Azure VPN-Gateway. Den här anslutningstypen gör att alla lokala resurser du auktoriserar tooaccess ett VNet. hello-anslutningen är en IPSec/IKE-VPN som tillhandahåller krypterad kommunikation över hello Internet mellan din lokala enhet och hello Azure VPN-gateway. hello svarstid för en plats-till-plats-anslutning är oförutsägbart, eftersom hello trafik färdas genom hello Internet.
- **Azure ExpressRoute:** upprättas mellan ditt nätverk och Azure, via en ExpressRoute-partner. Den här anslutningen är privat. Trafik inte går över hello Internet. hello svarstid för en ExpressRoute-anslutning är förutsägbar, eftersom trafiken inte passerar hello Internet.

Mer om alla hello tidigare anslutningsalternativ läsa hello toolearn [anslutning Topologidiagram](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams) artikel.

## <a name="filtering"></a>Filtrera nätverkstrafik
Du kan filtrera nätverkstrafiken mellan undernät med ett eller båda av följande alternativ för hello:
- **Nätverkssäkerhetsgrupper (NSG):** varje NSG kan innehålla flera inkommande och utgående säkerhetsregler som gör att du toofilter trafik genom käll- och IP-adress, port och protokoll. Du kan använda en NSG tooeach NIC på en virtuell dator. Du kan också använda en NSG toohello undernätet ett nätverkskort eller andra Azure-resurs är ansluten till. Mer om NSG: er, läsa hello toolearn [Nätverkssäkerhetsgrupper](virtual-networks-nsg.md) artikel.
- **Nätverks-virtuella installationer (NVA):** en NVA är en virtuell dator kör programvara som utför en funktion i nätverket, till exempel en brandvägg. Visa en lista över tillgängliga NVAs i hello [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). NVAs är också tillgängliga som ger WAN-optimering och andra nätverksenheter trafik funktioner. NVAs används vanligtvis med användardefinierade eller BGP-vägar. Du kan också använda en NVA toofilter trafik mellan virtuella nätverk.

## <a name="routing"></a>Dirigera nätverkstrafik

Azure skapar routningstabeller som aktiverar resurser anslutna tooany undernätet i alla VNet toocommunicate med varandra som standard. Du kan implementera något eller båda av följande alternativ toooverride hello hello standardvägar Azure skapar:
- **Användardefinierade vägar:** kan du skapa anpassade routningstabeller med vägar som styr där är trafik dirigeras toofor varje undernät. Mer om användardefinierade vägar, läsa hello toolearn [användardefinierade vägar](virtual-networks-udr-overview.md) artikel.
- **BGP-vägar:** om du ansluter din VNet tooyour lokalt nätverk med en Azure VPN-Gateway eller ExpressRoute-anslutning du sprida BGP-vägar tooyour Vnet.

## <a name="pricing"></a>Prissättning

Det är gratis för virtuella nätverk, undernät, vägtabeller eller nätverket säkerhetsgrupper. Utgående Internet-bandbreddsanvändning, offentliga IP-adresser, peering virtuellt nätverk, VPN-gatewayer och ExpressRoute varje har olika priser strukturer. Visa hello [för virtuella nätverk](https://azure.microsoft.com/pricing/details/virtual-network), [VPN-Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway), och [ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute) priser sidor för mer information.

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR

vanliga frågor och svar om virtuella nätverk, tooreview finns hello [virtuella nätverk vanliga frågor och svar](virtual-networks-faq.md) artikel.


## <a name="next-steps"></a>Nästa steg

- Skapa ditt första VNet och Anslut några virtuella datorer tooit genom att slutföra hello stegen i hello [skapa din första virtuella nätverket](virtual-network-get-started-vnet-subnet.md) artikel.
- Skapa en punkt-till-plats-anslutning tooa VNet genom att fylla hello stegen i hello [konfigurerar en punkt-till-plats-anslutning](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel.
- Lär dig mer om hello andra nyckeln [nätverk funktioner](../networking/networking-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Azure.
