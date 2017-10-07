---
title: "aaaAzure nätverk | Microsoft Docs"
description: "Lär dig mer om Azure nätverkstjänster och funktioner."
services: networking
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/19/2017
ms.author: jdial
ms.openlocfilehash: 18945d139427f2e65138c0fd223e663fa46e9211
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-networking"></a>Azure-nätverk

Azure tillhandahåller en mängd olika nätverksfunktioner som kan användas tillsammans eller separat. Klicka på någon av följande viktiga funktioner toolearn mer om dem hello:
- [Anslutning mellan Azure-resurser](#connectivity): ansluta Azure-resurser tillsammans i en säker, privat virtuellt nätverk i hello molnet.
- [Internetanslutning](#internet-connectivity): kommunicerar tooand från Azure-resurser via hello Internet.
- [Lokal anslutning](#on-premises-connectivity): ansluta ett lokalt nätverk tooAzure resurser via ett virtuellt privat nätverk (VPN) via hello Internet eller via en dedicerad anslutning tooAzure.
- [Läsa in belastningsutjämning och trafik riktning](#load-balancing): Läs in belastningsutjämna trafik tooservers i hello samma plats och trafiken tooservers på olika platser.
- [Säkerhet](#security): Filtrera nätverkstrafiken mellan undernät eller enskilda virtuella datorer (VM).
- [Routning](#routing): Använd standardroutning eller helt styra routning mellan Azure och lokala resurser.
- [Hanterbarhet](#manageability): övervaka och hantera dina Azure nätverksresurser.
- [Verktyg för distribution och konfiguration av](#tools): använder en webbaserad portal eller plattformsoberoende kommandoradsverktyg toodeploy och konfigurera nätverksresurser.

## <a name="Connectivity"></a>Anslutning mellan Azure-resurser

Azure-resurser som virtuella datorer, Cloud Services, Skalningsuppsättningar i virtuella datorer och Azure App Service-miljöer kan privat kommunicerar med varandra via ett Azure Virtual Network (VNet). Ett virtuellt nätverk är en logisk isolering av hello Azure-molnet dedikerad tooyour [prenumeration](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fnetworking%2ftoc.json). Du kan implementera flera Vnet inom varje Azure-prenumeration och Azure [region](https://azure.microsoft.com/regions). Varje virtuellt nätverk är isolerad från andra Vnet. För varje virtuellt nätverk kan du:

- Ange en egen privata IP-adressutrymmet med hjälp av offentliga och privata (RFC 1918)-adresser. Azure tilldelar resurser anslutna toohello VNet en privat IP-adress från hello adressutrymme som du tilldelar.
- Segmentera hello VNet i en eller flera undernät och tilldela en del av hello VNet adressutrymme tooeach undernät.
- Använd Azure-tillhandahållna namnmatchning eller ange egna DNS-server för användning av resurser anslutna tooa VNet.

Mer om hello Azure Virtual Network service, läsa hello toolearn [översikt över virtuella nätverk](../virtual-network/virtual-networks-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel. Du kan ansluta Vnet tooeach andra är att aktivera resurser anslutna tooeither VNet toocommunicate med varandra över Vnet. Du kan använda ett eller båda av följande alternativ tooconnect Vnet tooeach andra hello:

- **Peering:** aktiverar resurser anslutna toodifferent Azure Vnet inom hello samma Azure-region toocommunicate med varandra. hello bandbredd och fördröjning mellan hello Vnet är hello samma som om hello resurserna var anslutna toohello samma virtuella nätverk. toolearn mer information om peering, läsa hello [peering översikt över virtuella nätverk](../virtual-network/virtual-network-peering-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **VPN-Gateway:** aktiverar resurser anslutna toodifferent Azure Vnet inom olika Azure-regioner toocommunicate med varandra. Trafik mellan Vnet går genom en Azure VPN-Gateway. Bandbredden mellan Vnet är begränsad toohello bandbredden för hello gateway. Mer om hur du ansluter Vnet med en VPN-Gateway kan läsa hello toolearn [konfigurerar du en VNet-till-VNet-anslutning mellan regioner](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

## <a name="internet-connectivity"></a>Internet-anslutning

Alla Azure-resurser anslutna tooa VNet har utgående anslutning toohello Internet som standard. hello privata IP-adressen för hello resursen är källan nätverksadress översättas (SNAT) tooa offentlig IP-adress av hello Azure-infrastrukturen. Mer om utgående anslutning till Internet, läsa hello toolearn [förstå utgående anslutningar i Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

toocommunicate inkommande tooAzure resurser från hello Internet eller toocommunicate utgående toohello Internet utan SNAT, en resurs måste tilldelas en offentlig IP-adress. Mer om den offentliga IP-adresser, läsa hello toolearn [offentliga IP-adresser](../virtual-network/virtual-network-public-ip-address.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

## <a name="on-premises-connectivity"></a>Lokal anslutning

Du kan komma åt resurser i ditt virtuella nätverk på ett säkert sätt via en VPN-anslutning eller en privat direktanslutning. toosend nätverkstrafik mellan dina virtuella Azure-nätverket och ditt lokala nätverk, måste du skapa en virtuell nätverksgateway. Du kan konfigurera inställningar för hello gateway toocreate hello typ av anslutning som du vill, VPN eller ExpressRoute.

Du kan ansluta dina lokala nätverk tooa VNet med valfri kombination av hello följande alternativ:

**Punkt-till-plats (VPN över SSTP)**

hello följande bild visar separat punkt toosite anslutningar mellan flera datorer och ett virtuellt nätverk:

![Punkt-till-plats](./media/networking-overview/point-to-site.png)

Den här anslutningen har upprättats mellan en dator och ett VNet. Den här anslutningen är bra om du precis har börjat med Azure, eller för utvecklare, eftersom det krävs lite eller ingen ändringar tooyour befintliga nätverk. Det är också praktiskt när du ansluter från en annan plats, till exempel en konferens eller hemma. Punkt-till-plats-anslutningar är ofta tillsammans med en plats-till-plats-anslutning via hello gateway i samma virtuella nätverk. hello anslutningen använder hello SSTP-protokollet tooprovide krypterad kommunikation över hello Internet mellan hello dator och hello virtuella nätverk. hello svarstid för en punkt-till-plats-VPN är oförutsägbart, eftersom hello trafik färdas genom hello Internet.

**Plats-till-plats (IPsec/IKE VPN-tunnel)**

![Plats-till-plats](./media/networking-overview/site-to-site.png)

Den här anslutningen har upprättats mellan din lokala VPN-enhet och en Azure VPN-Gateway. Den här anslutningstypen gör att alla lokala resurser du auktoriserar tooaccess hello virtuella nätverk. hello-anslutningen är en IPSec/IKE-VPN som tillhandahåller krypterad kommunikation över hello Internet mellan din lokala enhet och hello Azure VPN-gateway. Du kan ansluta flera lokala platser toohello samma VPN-gateway. hello lokala VPN-enhet på varje plats måste ha en externt riktade-offentliga IP-adress som inte finns bakom en NAT hello svarstid för en plats-till-plats-anslutning är oförutsägbart, eftersom hello trafik färdas genom hello Internet.

**ExpressRoute (dedikerad privata anslutning)**

![ExpressRoute](./media/networking-overview/expressroute.png)

Den här typen av anslutning upprättas mellan ditt nätverk och Azure, via en ExpressRoute-partner. Den här anslutningen är privat. Trafik inte går över hello Internet. hello svarstid för en ExpressRoute-anslutning är förutsägbar, eftersom trafiken inte passerar hello Internet. ExpressRoute kan kombineras med en plats-till-plats-anslutning.

Mer om alla hello tidigare anslutningsalternativ läsa hello toolearn [anslutning Topologidiagram](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

## <a name="load-balancing"></a>Läsa in belastningsutjämning och trafik riktning

Microsoft Azure tillhandahåller flera tjänster för att hantera hur nätverkstrafik distribueras och Utjämning av nätverksbelastning. Du kan använda någon av följande funktioner enskilt eller tillsammans hello:

**Belastningsutjämning med DNS**

hello Azure Traffic Manager-tjänsten tillhandahåller globala belastningsutjämning av typen DNS. Traffic Manager svarar tooclients med hello IP-adressen för en felfri slutpunkt, baserat på en av hello följande metoder:
- **Geografiska:** klienterna dirigeras toospecific slutpunkter (Azure externa eller kapslade) baserat på vilka geografiska plats deras DNS-fråga som kommer från. Den här metoden möjliggör scenarier där känna till en klient geografisk region och routning dem baserat på det, är viktig. Exempel: uppfyller data suveränitet uppdrag, lokalisering av innehåll & användarens upplevelse och mäta trafik från olika regioner.
- **Prestanda:** hello IP-adress returneras toohello klienten är hello ”närmaste” toohello klient. hello 'närmaste' slutpunkt är inte nödvändigtvis närmaste mätt av geografiska avstånd. Den här metoden anger i stället hello närmaste slutpunkten genom att mäta Nätverksfördröjningen. Traffic Manager upprätthåller ett Internet tabell tootrack hello fram och åter latenstid mellan IP-adressintervall och varje Azure-datacenter.
- **Prioritet:** trafik är riktat toohello primära (högsta prioritet) slutpunkt. Om hello primära slutpunkt inte är tillgänglig, hello Traffic Manager vägar trafik toohello andra slutpunkten. Om båda hello primära och sekundära slutpunkter inte är tillgängliga går hello trafik toohello tredje, och så vidare. Tillgänglighet för hello slutpunkten är baserat på hello konfigurerats status (aktiverade eller inaktiverade) och hello pågående slutpunktsövervakning.
- **Viktad resursallokering:** för varje begäran väljer Traffic Manager slumpmässigt en tillgänglig slutpunkt. hello sannolikheten för att välja en slutpunkt som baseras på hello viktningar som tilldelats tooall tillgängliga slutpunkter. Med hjälp av hello vikt samma över alla slutpunkter resultat i en trafiken fördelas jämnt. Med högre eller lägre vikterna på specifika slutpunkter gör dessa slutpunkter toobe returnerade mer eller mindre ofta i hello DNS-svar.

hello följande bild visar en begäran om en web application dirigeras tooa Web App slutpunkt. Slutpunkter kan också finnas andra Azure-tjänster, till exempel virtuella datorer och molntjänster.

![Traffic Manager](./media/networking-overview/traffic-manager.png)

hello klienten ansluter direkt toothat slutpunkt. Azure Traffic Manager identifierar när en slutpunkt har dåligt hälsotillstånd och dirigerar klienter tooa olika, felfri slutpunkt. Mer om Traffic Manager, läsa hello toolearn [översikt över Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

**Belastningsutjämning för program**

hello Azure Application Gateway service ger programmet leverans domänkontrollant (ADC) som en tjänst. Programgateway erbjuder olika Layer 7 (HTTP/HTTPS) belastningsutjämnande funktioner för ditt program, inklusive ett webbprogram i brandväggen tooprotect webbprogrammen från säkerhetsrisker och trojaner. Programgateway kan du också toooptimize web servergruppen produktivitet genom att avlasta processorintensiva SSL-avslutning toohello Programgateway. 

Andra lager 7 routningsfunktionerna inkluderar resursallokering distribution av inkommande trafik, cookie-baserad session tillhörighet, URL-sökväg-baserad Routning och hello möjlighet toohost flera webbplatser bakom en enda Programgateway. Programgateway kan konfigureras som en mot Internet-gateway, en endast internt-gateway eller en kombination av båda. Programgateway är fullständigt Azure hanterade, skalbara och hög tillgänglighet. För att få en bättre hantering ingår en omfattande uppsättning diagnostik- och loggningsfunktioner. Mer om Programgateway läsa hello toolearn [Programgateway översikt](../application-gateway/application-gateway-introduction.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

hello följande bild visar Webbadressen sökväg-baserade routning med Application Gateway:

![Application Gateway](./media/networking-overview/application-gateway.png)

**Utjämning av nätverksbelastning**

hello Azure belastningsutjämnare ger hög prestanda, låg latens lager 4 belastningsutjämning för alla UDP och TCP-protokoll. Den hanterar inkommande och utgående anslutningar. Du kan konfigurera offentliga och interna belastningsutjämnade slutpunkter. Du kan definiera regler toomap inkommande anslutningar tooback slutpunkt pool mål med hjälp av TCP- och HTTP-avsökning av hälsotillstånd alternativ toomanage tjänsttillgänglighet. Mer om belastningsutjämnare, läsa hello toolearn [belastningsutjämnaren översikt](../load-balancer/load-balancer-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

hello följande bild visar ett Internet-riktade flera nivåer program som använder både externa och interna belastningsutjämnare:

![Belastningsutjämnare](./media/networking-overview/load-balancer.png)

## <a name="security"></a>Säkerhet

Du kan filtrera trafik tooand från Azure-resurser med hjälp av hello följande alternativ:

- **Nätverk:** du kan implementera Azure network security grupper (NSG: er) toofilter inkommande och utgående trafik tooAzure resurser. Varje NSG innehåller en eller flera regler för inkommande och utgående. Varje regel anger hello käll-IP-adresser, mål-IP-adresser, port och protokoll som trafik filtreras med. NSG: er kan vara tillämpade tooindividual undernät och enskilda virtuella datorer. Mer om NSG: er, läsa hello toolearn [Network security groups översikt](../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Program:** med hjälp av en Programgateway med Brandvägg för webbaserade program kan du skydda webbprogrammen från säkerhetsrisker och trojaner. Vanliga exempel är SQL injection attacker, över flera webbplatser och felaktig huvuden. Programgateway filtrerar ut den här trafiken och stoppas från att nå webbservrar. Du kan kan tooconfigure vilka regler som du vill aktiverad. hello möjlighet tooconfigure SSL-förhandlingsprinciper tillhandahålls tooallow vissa principer toobe inaktiverad. Mer om hello Brandvägg för webbaserade program, läsa hello toolearn [Brandvägg för webbaserade program](../application-gateway/application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

Om du behöver nätverkskapacitet Azure inte ge eller vill toouse nätverksprogram som du använder lokalt kan du implementera hello produkter i virtuella datorer och ansluter dem tooyour VNet. Hej [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances) innehåller flera olika virtuella datorer förkonfigurerade med nätverksprogram som du kan använda. Dessa förkonfigurerade virtuella datorer är vanligtvis enligt tooas virtuella nätverksinstallationer (NVA). NVAs finns tillgängliga för program som brandvägg och WAN-optimering.

## <a name="routing"></a>Routning

Azure skapar standard routningstabeller som aktiverar resurser anslutna tooany undernätet i alla VNet toocommunicate med varandra. Du kan implementera något eller båda av följande typer av vägar toooverride hello hello standardvägar Azure skapar:
- **Användardefinierade:** kan du skapa anpassade routningstabeller med vägar som styr där är trafik dirigeras toofor varje undernät. Mer om användardefinierade vägar, läsa hello toolearn [användardefinierade vägar](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Border gateway protocol (BGP):** om du ansluter din VNet tooyour lokalt nätverk med en Azure VPN-Gateway eller ExpressRoute-anslutning du sprida BGP-vägar tooyour Vnet. BGP är hello standard routingprotokoll ofta används i hello Internet tooexchange Routning och reachability information mellan två eller flera nätverk. När du använder hello Azure virtuella nätverk, BGP aktiverar hello Azure VPN-gatewayer och din lokala VPN-enheter, kallas BGP-peers eller grannar, tooexchange ”dirigerar” som information om båda gateways på hello tillgänglighet och tillgängligheten för dessa prefix toogo via hello gateways eller routrar som ingår. BGP kan också aktivera transitroutning mellan flera nätverk av sprider vägar som en gateway med BGP lär sig från en BGP peer tooall andra BGP-peers. toolearn mer om BGP, se hello [BGP med Azure VPN-gatewayer översikt](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

## <a name="manageability"></a>Hanterbarhet

Azure tillhandahåller hello följande verktyg toomonitor och hantera nätverk:
- **Aktivitetsloggar:** alla Azure-resurser har aktivitetsloggar som innehåller information om åtgärder som vidtagits placera, status för åtgärder och vem som initierat hello igen. Mer om aktivitetsloggar läsa hello toolearn [aktivitet loggar översikt](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Diagnostikloggar:** Periodisk och eget initiativ händelser har skapats av nätverksresurser och loggas i Azure storage-konton, skickas tooan Azure Event Hub eller skickas tooAzure logganalys. Diagnostikloggar ge insikt toohello hälsotillståndet för en resurs. Diagnostikloggar har angetts för belastningsutjämnaren (Internet-inriktad), Nätverkssäkerhetsgrupper, vägar och Application Gateway. Mer om diagnostikloggar, läsa hello toolearn [diagnostikloggar översikt](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Mått:** är prestandamått och räknare som samlas in under en tidsperiod på resurser. Mått kan vara används tootrigger varningar baserat på tröskelvärden. Mått är för närvarande tillgängliga på Application Gateway. Mer om statistik, och Läs hello toolearn [mått översikt](../monitoring-and-diagnostics/monitoring-overview-metrics.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Felsökning:** felsökningsinformation är tillgänglig direkt i hello Azure-portalen. hello information hjälper till att felsöka vanliga problem med ExpressRoute, VPN-Gateway, Programgateway, säkerhetsloggar i nätverket, vägar, DNS, belastningsutjämnare och Traffic Manager.
- **Rollbaserad åtkomstkontroll (RBAC):** styra vem som kan skapa och hantera nätverksresurser med rollbaserad åtkomstkontroll (RBAC). Mer information om RBAC genom att läsa hello [Kom igång med RBAC](../active-directory/role-based-access-control-what-is.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel. 
- **Paketinsamling:** hello Azure Nätverksbevakaren tjänsten tillhandahåller hello möjlighet toorun ett paket avbilda på en virtuell dator via ett tillägg i hello VM. Den här funktionen är tillgänglig för Linux och Windows-datorer. Mer om paketinsamling läsa hello toolearn [paket avbilda översikt](../network-watcher/network-watcher-packet-capture-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Kontrollera IP-flöden:** Nätverksbevakaren kan du tooverify IP-flöden mellan en Azure VM och en fjärresurs toodetermine om paket tillåts eller nekas. Den här funktionen ger administratörer möjlighet hello tooquickly diagnostisera problem med nätverksanslutningen. toolearn mer information om hur tooverify IP flödar läsa hello [IP-flöde Kontrollera översikt](../network-watcher/network-watcher-ip-flow-verify-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Felsökning av VPN-anslutningar:** hello VPN-felsökaren möjligheterna för Nätverksbevakaren ger hello möjlighet tooquery anslutnings- eller gateway och kontrollera hello hello-resursers hälsa. Mer om hur du felsöker VPN-anslutningar kan läsa hello toolearn [felsökning översikt över VPN-anslutning](../network-watcher/network-watcher-troubleshoot-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Visa nätverkstopologi:** visar en grafisk representation av hello nätverksresurser i ett VNet med Nätverksbevakaren. Mer om hur du visar nätverkstopologi, läsa hello toolearn [labbtopologi](../network-watcher/network-watcher-topology-overview.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.

## <a name="tools"></a>Verktyg för distribution och konfiguration

Du kan distribuera och konfigurera Azure nätverksresurser med någon av följande verktyg hello:

- **Azure-portalen:** ett grafiskt användargränssnitt som körs i en webbläsare. Öppna hello [Azure-portalen](http://portal.azure.com).
- **Azure PowerShell:** kommandoradsverktyg för att hantera Azure från Windows-datorer. Lär dig mer om Azure PowerShell läser hello [översikt över Azure PowerShell](/powershell/azure/overview?view=azurermps-3.8.0?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Azure-kommandoradsgränssnittet (CLI):** kommandoradsverktyg för att hantera Azure från Linux, macOS eller Windows-datorer. Mer information om hello Azure CLI genom att läsa hello [översikt över Azure CLI](/cli/azure/get-started-with-azure-cli?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- **Azure Resource Manager-mallar:** en fil (i JSON-format) som definierar hello-infrastrukturen och konfigurationen av en Azure-lösning. Genom att använda en mall kan du distribuera lösningen flera gånger under dess livscykel och vara säker på att dina resurser distribueras konsekvent. Mer om att skapa mallar, läsa hello toolearn [bästa praxis för att skapa mallar](../azure-resource-manager/resource-manager-template-best-practices.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel. Mallar kan distribueras med CLI eller PowerShell hello Azure-portalen. tooget igång med mallar direkt, distribuera någon av hello många förkonfigurerade mallar i hello [Azure Quickstart mallar](https://azure.microsoft.com/resources/templates/?term=network) bibliotek. 

## <a name="pricing"></a>Prissättning

Vissa av hello Azure nätverkstjänster har kostnad, medan andra är kostnadsfri. Visa hello [för virtuella nätverk](https://azure.microsoft.com/pricing/details/virtual-network), [VPN-Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway), [Programgateway](https://azure.microsoft.com/en-us/pricing/details/application-gateway/), [belastningsutjämnaren](https://azure.microsoft.com/pricing/details/load-balancer), [Nätverksbevakaren](https://azure.microsoft.com/pricing/details/network-watcher), [DNS](https://azure.microsoft.com/pricing/details/dns), [Traffic Manager](https://azure.microsoft.com/pricing/details/traffic-manager) och [ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute) priser sidor för mer information.

## <a name="next-steps"></a>Nästa steg

- Skapa ditt första VNet och Anslut några virtuella datorer tooit genom att slutföra hello stegen i hello [skapa din första virtuella nätverket](../virtual-network/virtual-network-get-started-vnet-subnet.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- Anslut din dator tooa VNet genom att slutföra hello stegen i hello [konfigurerar en punkt-till-plats-anslutning](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
- Belastningsutjämna Internet-trafik toopublic servrar genom att slutföra hello stegen i hello [skapa en Internetriktade belastningsutjämnare](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) artikel.
