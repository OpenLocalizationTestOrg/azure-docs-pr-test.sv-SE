---
title: "Virtuella Azure-nätverket | Microsoft Docs"
description: "Läs mer om Azure Virtual Network koncept och funktioner."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 9633de4b-a867-4ddf-be3c-a332edf02e24
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/12/2017
ms.author: jdial
ms.openlocfilehash: 6cc7035e798ef72f69958a7536a741f80939d4fe
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/03/2018
---
# <a name="azure-virtual-network"></a>Azure Virtual Network

Tjänsten Microsoft Azure Virtual Network kan Azure-resurser för säker kommunikation med varandra i ett virtuellt nätverk. Ett virtuellt nätverk är en logisk isolering av Azure-molnet dedikerad till din prenumeration. Du kan ansluta virtuella nätverk till andra virtuella nätverk eller till ditt lokala nätverk. Följande bild visar några av funktionerna i Azure Virtual Network service:

![Diagram över nätverk](./media/virtual-networks-overview/virtual-network-overview.png)

Mer information om följande funktioner i Azure Virtual Network klickar du på funktionen:
- **[Isolering:](#isolation)**  virtuella nätverk är isolerade från varandra. Du kan skapa separata virtuella nätverk för utveckling, testning och produktion som använder samma CIDR (till exempel 10.0.0.0/0) adressen block. Däremot kan du skapa flera virtuella nätverk som använder olika CIDR-Adressblock och ansluta nätverken tillsammans. Du kan dela ett virtuellt nätverk i flera undernät. Azure erbjuder intern namnmatchning för resurser har distribuerats i ett virtuellt nätverk. Om det behövs kan du konfigurera ett virtuellt nätverk för att använda dina egna DNS-servrar i stället för intern namnmatchning för Azure.
- **[Internet-kommunikation:](#internet)**  resurser, till exempel virtuella datorer distribueras i ett virtuellt nätverk har åtkomst till Internet, som standard. Du kan också aktivera inkommande åtkomst till specifika resurser efter behov.
- **[Azure-resurs kommunikation:](#within-vnet)**  Azure resurser har distribuerats i ett virtuellt nätverk kan kommunicera med varandra med privata IP-adresser, även om resurserna som distribueras i olika undernät. Azure tillhandahåller standardroutning mellan undernät, anslutna virtuella nätverk och lokala nätverk, så du behöver att konfigurera och hantera vägar. Om du vill kan anpassa du Azures routning.
- **[Virtuell nätverksanslutning:](#connect-vnets)**  virtuella nätverk kan vara anslutna till varandra, aktivera resurser i något virtuellt nätverk för att kommunicera med resurser i det virtuella nätverket.
- **[Lokal anslutning:](#connect-on-premises)**  ett virtuellt nätverk kan vara ansluten till ett lokalt nätverk, aktivera resurser för att kommunicera med varandra.
- **[Trafikfiltrering:](#filtering)**  kan du filtrera nätverkstrafik till och från resurser i ett virtuellt nätverk med källans IP-adress och port, mål-IP-adress och port och protokoll.
- **[Routning:](#routing)**  du kan du åsidosätta Azures standard routning genom att konfigurera egna vägar eller genom att sprida BGP vägar via en nätverksgateway.

## <a name = "isolation"></a>Isolering av nätverk och segmentering

Du kan implementera flera virtuella nätverk i varje Azure [prenumeration](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) och Azure [region](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region). Varje virtuellt nätverk är isolerat från andra virtuella nätverk. För varje virtuellt nätverk kan du:
- Ange en egen privata IP-adressutrymmet med hjälp av offentliga och privata (RFC 1918)-adresser. Azure tilldelar resurser i ett virtuellt nätverk en privat IP-adress från det adressutrymme som du tilldelar.
- Segmentera det virtuella nätverket i en eller flera undernät och tilldela en del av det virtuella nätverkets adressutrymme till varje undernät.
- Använd Azure-tillhandahållna namnmatchning eller ange egna DNS-server för användning av resurser i ett virtuellt nätverk. Mer information om namnmatchning i virtuella nätverk finns [namnmatchning för resurser i virtuella nätverk](virtual-networks-name-resolution-for-vms-and-role-instances.md) artikel.

## <a name = "internet"></a>Internet-kommunikation
Alla resurser i ett virtuellt nätverk kan kommunicera utgående till Internet. Som standard är privata IP-adressen för resursen adress källnätverket översättas (SNAT) till en offentlig IP-adress som valts av Azure-infrastrukturen. Om du vill veta mer om utgående Internetanslutning kan du läsa den [förstå utgående anslutningar i Azure](..\load-balancer\load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json#standalone-vm-with-no-instance-level-public-ip-address) artikel. Du kan implementera anpassade vägar eller trafikfiltrering för att förhindra utgående Internetanslutning.

Att kommunicera inkommande Azure-resurser från Internet eller kommunicera utgående till Internet utan SNAT tilldelas en resurs en offentlig IP-adress. Mer information om den offentliga IP-adresser i [offentliga IP-adresser](virtual-network-public-ip-address.md) artikel.

## <a name="within-vnet"></a>Säker kommunikation mellan Azure-resurser

Du kan distribuera virtuella datorer i ett virtuellt nätverk. Virtuella datorer kommunicerar med andra resurser i ett virtuellt nätverk via ett nätverksgränssnitt. Läs mer om nätverksgränssnitt i [nätverksgränssnitt](virtual-network-network-interface.md).

Du kan också distribuera flera andra typer av Azure-resurser till ett virtuellt nätverk, till exempel Azure App Service-miljöer och Azure Skalningsuppsättningar i virtuella datorer. En fullständig lista över Azure-resurser du kan distribuera till ett virtuellt nätverk finns i [integrering för Azure-tjänster för virtuella nätverk tjänsten](virtual-network-for-azure-services.md).

Vissa resurser kan inte distribueras till ett virtuellt nätverk, men du kan begränsa kommunikationen till resurser i ett virtuellt nätverk. Läs mer om hur du begränsar åtkomsten till resurser i [virtuella nätverksslutpunkter](virtual-network-service-endpoints-overview.md). 

## <a name="connect-vnets"></a>Ansluta virtuella nätverk

Du kan ansluta virtuella nätverk till varandra, aktivera resurser i antingen virtuella nätverk för att kommunicera med varandra med hjälp av virtuellt nätverk peering. Bandbredd och svarstiden för kommunikationen mellan resurser i olika virtuella nätverk är densamma som om resurserna som fanns i samma virtuella nätverk. Mer information om peering den [virtuella nätverk peering](virtual-network-peering-overview.md) artikel.

## <a name="connect-on-premises"></a>Ansluta till ett lokalt nätverk

Du kan ansluta dina lokala nätverk till ett virtuellt nätverk med hjälp av följande alternativ:
- **Punkt-till-plats virtuellt privat nätverk (VPN):** upprättas mellan ett virtuellt nätverk och en enda dator i nätverket. Varje dator som du vill upprätta en anslutning med ett virtuellt nätverk måste konfigurera sin anslutning oberoende av varandra. Den här anslutningen är bra om du precis har börjat med Azure, eller för utvecklare, eftersom det krävs lite eller ingen ändringar i ditt befintliga nätverk. Anslutningen använder SSTP-protokollet för att tillhandahålla krypterad kommunikation via Internet mellan datorn och ett virtuellt nätverk. Svarstiden för en punkt-till-plats-VPN är oförutsägbart, eftersom trafiken färdas genom Internet.
- **Plats-till-plats-VPN:** mellan din VPN-enhet och en Azure VPN-Gateway distribueras i ett virtuellt nätverk. Den här anslutningstypen gör att alla lokala resurser som du har behörighet att få åtkomst till ett virtuellt nätverk. Anslutningen är en IPSec/IKE-VPN som tillhandahåller krypterad kommunikation via Internet mellan din lokala enhet och Azure VPN-gatewayen. Svarstiden för en plats-till-plats-anslutning är oförutsägbart, eftersom trafiken färdas genom Internet.
- **Azure ExpressRoute:** upprättas mellan ditt nätverk och Azure, via en ExpressRoute-partner. Den här anslutningen är privat. Trafik inte går över Internet. Svarstid för en ExpressRoute-anslutning är förutsägbar, eftersom trafiken inte sker via Internet.

Läs mer om alla tidigare anslutningsalternativ i [anslutning Topologidiagram](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams).

## <a name="filtering"></a>Filtrera nätverkstrafik
Du kan filtrera nätverkstrafiken mellan undernät med ett eller båda av följande alternativ:
- **Nätverkssäkerhetsgrupper:** en nätverkssäkerhetsgrupp kan innehålla flera inkommande och utgående säkerhetsregler som gör det möjligt att filtrera trafik genom käll- och IP-adress, port och protokoll. Du kan använda en nätverkssäkerhetsgrupp för varje nätverksgränssnitt i en virtuell dator. Du kan också använda en nätverkssäkerhetsgrupp till undernätet för ett nätverksgränssnitt eller andra Azure-resurs i. Läs mer om nätverkssäkerhetsgrupper i [Nätverkssäkerhetsgrupper](security-overview.md#network-security-groups).
- **Nätverks-virtuella installationer:** en virtuell nätverksenhet är en virtuell dator som kör programvara som utför en funktion i nätverket, till exempel en brandvägg. Visa en lista över tillgängliga nätverk virtuella installationer i den [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). Virtuella installationer i nätverket är också tillgängliga som har WAN-optimering och andra nätverksenheter trafik funktioner. Virtuella nätverksinstallationer används vanligtvis med användardefinierade eller BGP-vägar. Du kan också använda en virtuell nätverksenhet för att filtrera trafik mellan virtuella nätverk.

## <a name="routing"></a>Dirigera nätverkstrafik

Azure skapar routningstabeller som gör att resurser som är anslutna till alla undernät i alla virtuella nätverk för att kommunicera med varandra och Internet, som standard. Du kan implementera ett eller båda av följande alternativ för att åsidosätta standardvägar Azure skapar:
- **Användardefinierade vägar:** kan du skapa anpassade routningstabeller med vägar som styr där trafik dirigeras till för varje undernät. Mer information om användardefinierade vägar finns [användardefinierade vägar](virtual-networks-udr-overview.md#user-defined).
- **BGP-vägar:** om du ansluter det virtuella nätverket till ditt lokala nätverk med hjälp av en Azure VPN-Gateway eller ExpressRoute-anslutning du sprida BGP-vägar till ditt virtuella nätverk.

## <a name="pricing"></a>Prissättning

Det är gratis för virtuella nätverk, undernät, vägtabeller eller nätverket säkerhetsgrupper. Utgående Internet-bandbreddsanvändning, offentliga IP-adresser, peering virtuellt nätverk, VPN-gatewayer och ExpressRoute varje har olika priser strukturer. Visa den [för virtuella nätverk](https://azure.microsoft.com/pricing/details/virtual-network), [VPN-Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway), och [ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute) priser sidor för mer information.

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR

Vanliga frågor och svar om Azure-nätverk finns i [vanliga frågor och svar för virtuella nätverk](virtual-networks-faq.md) artikel.

## <a name="next-steps"></a>Nästa steg

- Skapa din första virtuella nätverket och distribuerar några virtuella datorer till den, genom att slutföra stegen i [skapa din första virtuella nätverket](virtual-network-get-started-vnet-subnet.md).
- Skapa en punkt-till-plats-anslutning till ett virtuellt nätverk genom att slutföra stegen i [konfigurerar en punkt-till-plats-anslutning](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Lär dig mer om den andra nyckeln [nätverk funktioner](../networking/networking-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Azure.
