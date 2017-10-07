---
title: "aaaAzure nätverkssäkerhet | Microsoft Docs"
description: "Läs mer om molnbaserad databearbetning tjänster som omfattar ett brett urval av compute-instanser och tjänster som kan skalas upp-och automatiskt toomeet hello behov program-eller enterprise."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/24/2017
ms.author: TomSh
ms.openlocfilehash: 7dae83bbe338a2727575447583a7fb773527dd59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-network-security"></a>Azure nätverkssäkerhet

Vi vet att säkerheten är jobbet en i hello molnet och det är viktigt att du hitta korrekt och rimlig information om säkerheten i Azure. En av hello bästa orsaker toouse Azure för dina program och tjänster är tootake fördel av Azures mängd säkerhetsverktyg och funktioner. Dessa verktyg och funktioner gör det möjligt toocreate säkra lösningar på hello Azure-plattformen.

Microsoft Azure tillhandahåller sekretess, integritet och tillgänglighet av kundinformation, samtidigt som transparent accountability. toohelp du bättre förstå hello samling nätverk säkerhetsåtgärder som implementerats i Microsoft Azure från hello kundens perspektiv, den här artikeln ”nätverkssäkerhet för Azure”, skrivs tooprovide omfattande titta på hello nätverkssäkerhet kontroller som är tillgängliga med Microsoft Azure.

Det här dokumentet är avsett tooinform dig om hello mängd nätverket styr som du kan konfigurera tooenhance hello säkerheten för hello lösningar som du distribuerar i Azure. Om du är intresserad av i vilken Microsoft toosecure hello nätverksinfrastruktur för Hej Azure-plattformen, i avsnittet hello Azure-säkerhet i hello [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/azure-security).

## <a name="azure-platform"></a>Azure-plattformen

Azure är en offentlig molntjänstplattform som har stöd för ett brett urval av operativsystem, programmeringsspråk, ramverk, verktyg, databaser, och enheter.  Det kan köras Linux behållare med Docker-integration. skapa appar med JavaScript, Python, .NET, PHP, Java och Node.js; build-servrar för iOS, Android och Windows enheter. Azure-molntjänster stöder hello samma tekniker miljoner utvecklare och IT-proffs redan förlitar sig på och litar på.

När du bygger på eller migrera IT tillgångar till en offentlig molntjänstleverantör, måste den organisationens förmåga tooprotect dina program och data med hello tjänster och hello kontroller ger toomanage hello säkerheten för din molnbaserade tillgångar.

Azures infrastruktur har utformats från hello anläggning tooapplications som värd för miljontals kunder samtidigt och det ger en säker grund som företag kan uppfylla sina säkerhetskrav. Dessutom Azure ger dig en omfattande samling konfigurerbara säkerhet alternativ och hello möjlighet toocontrol dem så att du kan anpassa toomeet hello unika säkerhetskrav för distributioner i din organisation.

## <a name="abstract"></a>Abstrakt

Microsofts offentliga molntjänster leverera storskaliga tjänster och infrastruktur, funktioner i företagsklass och många alternativ för hybridanslutning. Du kan välja tooaccess dessa tjänster via hello Internet eller med Azure ExpressRoute som tillhandahåller anslutningar privata nätverk. hello Microsoft Azure-plattformen gör du tooseamlessly utöka din infrastruktur i hello moln och bygga arkitektur med flera nivåer. Tredje part kan dessutom aktivera förbättrade funktioner genom att erbjuda tjänster och virtuella installationer.

Azures nätverkstjänster maximera flexibilitet, tillgänglighet, återhämtning, säkerhet och integritet avsiktligt. I det här dokumentet innehåller information om hello nätverk funktioner i Azure och information om hur kunder kan använda Azures ursprungligt funktioner toohelp skydda sina informationstillgångar.

hello avsedd målgrupper för den här rapporten innehåller:

- Tekniska chefer, administratörer och utvecklare som söker efter säkerhetslösningar tillgängliga och stöds i Azure.

-   Små och medelstora företag eller business process chefer som vill tooget en överblick hello Azure nätverkstekniker och tjänster som är relevanta i kring nätverkssäkerhet i hello offentliga Azure-molnet.

## <a name="azure-networking-big-picture"></a>Azure nätverk översikt
Microsoft Azure innehåller en robust nätverk infrastruktur toosupport programmet och service anslutningskrav. Nätverksanslutningen är möjlig mellan resurser i Azure, mellan lokala och Azure värdbaserade resurser, och tooand från hello Internet och Azure.

![Azure nätverk översikt](media/azure-network-security/azure-network-security-fig-1.png)

Hej [Azure nätverksinfrastruktur](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-networking-guidelines) aktiverar du toosecurely ansluta Azure-resurser tooeach andra med virtuella nätverk (Vnet). Ett virtuellt nätverk är en representation av ditt eget nätverk i hello molnet. Ett virtuellt nätverk är en logisk isolering av hello Azure-molnet dedikerad tooyour nätverksprenumerationen. Du kan ansluta Vnet tooyour lokala nätverk.

Har stöd för Azure dedikerad WAN-länken anslutningen tooyour lokala nätverk och ett Azure Virtual Network med [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction). hello länken mellan Azure och din plats använder en dedikerad anslutning som inte går via hello offentliga Internet. Om din Azure-program körs i flera datacenter, kan du använda [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) tooroute begäranden från användare som använder det Intelligent mellan instanser av programmet hello. Du kan också dirigera trafik tooservices inte körs i Azure om de är tillgängliga från hello Internet.

## <a name="enterprise-view-of-azure-networking-components"></a>Enterprise-vyn Azure nätverkskomponenter
Azure har många komponenter som är relevanta toonetwork säkerhet diskussioner. Vi beskriver dessa nätverkskomponenter och fokusera på hello säkerhet problem relaterade toothem.

> [!Note]
> Beskrivs inte alla aspekter av Azure-nätverk – diskuterar vi endast de som anses vara toobe spela en central i planering och utformning av en säker nätverksinfrastruktur runt dina tjänster och program som du distribuerar i Azure.

I det här dokumentet kommer att hello omfattar följande Azure nätverk enterprise-funktioner:

-   Grundläggande nätverksanslutning

-   Hybridanslutning

-   Säkerhetsåtgärder

-   Nätverksverifieringen

### <a name="basic-network-connectivity"></a>Grundläggande nätverksanslutning

Hej [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) tjänsten aktiverar du toosecurely ansluta Azure-resurser tooeach andra med virtuella nätverk (VNet). Ett virtuellt nätverk är en representation av ditt eget nätverk i hello molnet. Ett virtuellt nätverk är en logisk isolering av hello Azure-nätverk dedikerad infrastruktur tooyour prenumeration. Du kan också ansluta Vnet tooeach andra och tooyour lokalt nätverk med plats-till-plats VPN och dedikerade [WAN-länkar](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

![Grundläggande nätverksanslutning](media/azure-network-security/azure-network-security-fig-2.png)

Med hello förstå att du använder virtuella datorer toohost servrar i Azure, är hello frågan hur dessa virtuella datorer ansluter tooa nätverk. hello svaret är att virtuella datorer ansluta tooan [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

Virtuella Azure-nätverk är likadana hello virtuella nätverk som du använder lokala med din egen plattform virtualiseringslösningar, till exempel Microsoft Hyper-V- eller VMware.

#### <a name="intra-vnet-connectivity"></a>Intra-VNet-anslutningar

Du kan ansluta Vnet tooeach andra är att aktivera resurser anslutna tooeither VNet toocommunicate med varandra över Vnet. Du kan använda ett eller båda av följande alternativ tooconnect Vnet tooeach andra hello:

- **Peering:** aktiverar resurser anslutna toodifferent Azure Vnet inom hello samma Azure-plats toocommunicate med varandra. hello bandbredd och fördröjning mellan virtuella nätverk är hello hello samma som om hello resurserna var anslutna toohello samma virtuella nätverk. toolearn mer information om peering, läsa [virtuella nätverk peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).

 ![Peering](media/azure-network-security/azure-network-security-fig-3.png)

- **VNet-till-VNet-anslutningen:** aktiverar resurser anslutna toodifferent Azure VNet inom hello samma eller olika Azure-platser. Till skillnad från peering, är bandbredd eftersom begränsade mellan Vnet trafik måste gå genom en Azure VPN-Gateway.

![VNet-till-VNet-anslutningen](media/azure-network-security/azure-network-security-fig-4.png)


Mer om hur du ansluter Vnet med ett VNet-till-VNet-anslutningen, läsa hello toolearn [konfigurera en anslutning VNet-till-VNet-artikel](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal?toc=%2fazure%2fvirtual-network%2ftoc.json).

#### <a name="azure-virtual-network-capabilities"></a>Funktioner för virtuella Azure-nätverket:

Som du ser innehåller ett Azure Virtual Network virtuella datorer tooconnect toohello nätverk så att de kan ansluta tooother nätverksresurser på ett säkert sätt. Grundläggande anslutningsmöjligheter är dock bara hello början. hello exponera följande funktioner för hello Azure Virtual Network service hello Azure Virtual Network security egenskaper:

-   Isolering

-   Internetanslutning

-   Azure-resurs-anslutning

-   VNet-anslutningar

-   Lokala anslutningsmöjligheter

-   Trafikfiltrering

-   Routning

**Isolering**

Vnet är [isolerade](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) från varandra. Du kan skapa separata Vnet för utveckling, testning och produktion som att använda hello samma [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) adressen block. Däremot kan du skapa flera virtuella nätverk som använder olika CIDR-Adressblock och ansluta nätverk tillsammans. Du kan dela ett VNet i flera undernät.

Azure erbjuder intern namnmatchning för virtuella datorer och [molntjänster](https://azure.microsoft.com/services/cloud-services/) rollinstanser anslutna tooa VNet. Du kan också konfigurera ett virtuellt nätverk toouse egna DNS-servrar, istället för att använda intern namnmatchning för Azure.

Du kan implementera flera Vnet i varje Azure [prenumeration](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology?toc=%2fazure%2fvirtual-network%2ftoc.json) och Azure [region](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology?toc=%2fazure%2fvirtual-network%2ftoc.json). Varje virtuellt nätverk är isolerad från andra Vnet. För varje virtuellt nätverk kan du:

-   Ange en egen privata IP-adressutrymmet med hjälp av offentliga och privata (RFC 1918)-adresser. Azure tilldelar resurser anslutna toohello VNet en privat IP-adress från hello-adressutrymmet kan du allokera.

-   Segmentera hello VNet i en eller flera undernät och tilldela en del av hello VNet adressutrymme tooeach undernät.

-   Använd Azure-tillhandahållna namnmatchning eller ange egna DNS-server för användning av resurser anslutna tooa VNet. Mer information om namnmatchning i Vnet, läsa hello toolearn [namnmatchning för virtuella datorer och molntjänster](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances).

**Internetanslutning**

Alla [Azure virtuella datorer (VM)](https://docs.microsoft.com/azure/virtual-machines/windows/) och molntjänster rollinstanser anslutna tooa VNet har åtkomst till toohello Internet, som standard. Du kan också aktivera inkommande åtkomst toospecific resurser efter behov. (VM) och molntjänster rollinstanser anslutna tooa VNet har åtkomst till toohello Internet, som standard. Du kan också aktivera inkommande åtkomst toospecific resurser efter behov.

Alla resurser anslutna tooa VNet har utgående anslutning toohello Internet som standard. hello privata IP-adressen för hello resursen är källan nätverksadress översättas (SNAT) tooa offentlig IP-adress av hello Azure-infrastrukturen. Du kan ändra hello standard anslutningen genom att implementera anpassad Routning och filtrera trafik. Mer om utgående anslutning till Internet, läsa hello toolearn [förstå utgående anslutningar i Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections?toc=%2fazure%2fvirtual-network%2ftoc.json).

toocommunicate inkommande tooAzure resurser från hello Internet eller toocommunicate utgående toohello Internet utan SNAT, en resurs måste tilldelas en offentlig IP-adress. Mer om den offentliga IP-adresser, läsa hello toolearn [offentliga IP-adresser](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address).

**Azure-resurs-anslutning**

[Azure-resurser](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) som molntjänster och virtuella datorer kan vara anslutna toohello samma virtuella nätverk. hello resurser kan ansluta tooeach andra använder privata IP-adresser, även om de finns i olika undernät. Azure tillhandahåller standardroutning mellan undernät, virtuella nätverk och lokala nätverk, så att du inte har tooconfigure och hantera vägar.

Du kan ansluta flera Azure-resurser tooa VNet, till exempel virtuella datorer (VM), Cloud Services, Apptjänstmiljöer och Skalningsuppsättningar i virtuella datorer. Virtuella datorer ansluta tooa undernät inom ett VNet till ett nätverksgränssnitt (NIC). Mer om nätverkskort, läsa hello toolearn [nätverksgränssnitt](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface).

**VNet-anslutningar**

[Vnet](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) kan vara anslutna tooeach andra, aktivera resurser anslutna tooany VNet toocommunicate med alla resurser för andra virtuella nätverk.

Du kan ansluta Vnet tooeach andra är att aktivera resurser anslutna tooeither VNet toocommunicate med varandra över Vnet. Du kan använda ett eller båda av följande alternativ tooconnect Vnet tooeach andra hello:

- **Peering:** aktiverar resurser anslutna toodifferent Azure Vnet inom hello samma Azure-plats toocommunicate med varandra. hello bandbredd och fördröjning mellan hello Vnet är hello samma som om hello resurser var anslutna toohello samma VNet.toolearn mer om peering, läsa hello [virtuella nätverk peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).

- **VNet-till-VNet-anslutningen:** aktiverar resurser anslutna toodifferent Azure VNet inom hello samma eller olika Azure-platser. Till skillnad från peering, är bandbredd eftersom begränsade mellan Vnet trafik måste gå genom en Azure VPN-Gateway. toolearn mer om hur du ansluter Vnet med en VNet-till-VNet-anslutning. toolearn läsa fler hello [konfigurera VNet-till-VNet-anslutningen](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal?toc=%2fazure%2fvirtual-network%2ftoc.json) .

**Lokala anslutningsmöjligheter**

Vnet kan anslutas för[lokalt](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) nätverk via privata nätverksanslutningar mellan ditt nätverk och Azure, eller en plats-till-plats VPN-anslutning via hello Internet.

Du kan ansluta dina lokala nätverk tooa VNet med valfri kombination av hello följande alternativ:

- **Punkt-till-plats virtuellt privat nätverk (VPN):** upprättas mellan en enskild dator ansluten tooyour nätverk och hello VNet. Den här anslutningen är bra om du precis har börjat med Azure, eller för utvecklare, eftersom det krävs lite eller ingen ändringar tooyour befintliga nätverk. hello anslutningen använder hello SSTP-protokollet tooprovide krypterad kommunikation över hello Internet mellan hello PC och hello virtuella nätverk. hello svarstid för en punkt-till-plats-VPN är oförutsägbart eftersom hello trafik färdas genom hello Internet.

- **Plats-till-plats-VPN:** mellan din VPN-enhet och en Azure VPN-Gateway. Den här anslutningstypen gör att alla lokala resurser du auktoriserar tooaccess ett VNet. hello-anslutningen är en IPsec/IKE-VPN som tillhandahåller krypterad kommunikation över hello Internet mellan din lokala enhet och hello Azure VPN-gateway. hello svarstid för en plats-till-plats-anslutning är oförutsägbart eftersom hello trafik färdas genom hello Internet.

- **Azure ExpressRoute:** upprättas mellan ditt nätverk och Azure, via en ExpressRoute-partner. Den här anslutningen är privat. Trafik inte går över hello Internet. hello svarstid för en ExpressRoute-anslutning är förutsägbar eftersom trafiken inte passerar hello Internet. Mer om alla hello tidigare anslutningsalternativ läsa hello toolearn [anslutning Topologidiagram](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json).

**Trafikfiltrering**

Molntjänster och Virtuella rollinstanser [nätverkstrafik](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) kan filtreras inkommande och utgående efter källans IP-adress och port, mål-IP-adress och port och protokoll.

Du kan filtrera nätverkstrafiken mellan undernät med ett eller båda av följande alternativ för hello:

- **Nätverkssäkerhetsgrupper (NSG):** varje NSG kan innehålla flera inkommande och utgående säkerhetsregler som gör att du toofilter trafik genom käll- och IP-adress, port och protokoll. Du kan använda en NSG tooeach NIC på en virtuell dator. Du kan också använda en NSG toohello undernätet ett nätverkskort eller andra Azure-resurs är ansluten till. Mer om NSG: er, läsa hello toolearn [Nätverkssäkerhetsgrupper](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).

- **Virtuella nätverksinstallationer:** en virtuell nätverksenhet är en virtuell dator kör programvara som utför en funktion i nätverket, till exempel en brandvägg. Visa en lista över tillgängliga NVAs i hello Azure Marketplace. NVAs är också tillgängliga som ger WAN-optimering och andra nätverksenheter trafik funktioner. NVAs används vanligtvis med användardefinierade eller BGP-vägar. Du kan också använda en NVA toofilter trafik mellan virtuella nätverk.

**Routning**

Alternativt kan du åsidosätta Azures standard routning genom att konfigurera egna vägar eller använda BGP-vägar via en nätverksgateway.

Azure skapar routningstabeller som aktiverar resurser anslutna tooany undernätet i alla VNet toocommunicate med varandra som standard. Du kan implementera något eller båda av följande alternativ toooverride hello hello standardvägar Azure skapar:

- **Användardefinierade vägar:** kan du skapa anpassade routningstabeller med vägar som styr där är trafik dirigeras toofor varje undernät. Mer om användardefinierade vägar, läsa hello toolearn [användardefinierade vägar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview).

- **BGP-vägar:** om du ansluter din VNet tooyour lokalt nätverk med en Azure VPN-Gateway eller ExpressRoute-anslutning du sprida BGP-vägar tooyour Vnet.

### <a name="hybrid-internet-connectivity-connect-tooan-on-premises-network"></a>Hybrid Internetanslutning: ansluta tooan lokalt nätverk
Du kan ansluta dina lokala nätverk tooa VNet med valfri kombination av hello följande alternativ:

-   Internetanslutning

-   Punkt-till-plats-VPN (P2S VPN)

-   VPN för plats-till-plats (S2S VPN)

-   ExpressRoute

#### <a name="internet-connectivity"></a>Internet-anslutning

Som namnet antyder gör Internetanslutning dina arbetsbelastningar tillgänglig från hello Internet, genom att låta du exponera olika offentliga slutpunkter tooworkloads som bor i hello virtuella nätverket. Dessa arbetsbelastningar kan exponeras med hjälp av [Internetriktade belastningsutjämnare](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview) eller bara tilldela en offentlig IP-adressen toohello VM. På så sätt kan det blir möjligt för den virtuella datorn något annat hello Internet toobe kan tooreach anges en värdbrandväggen [nätverkssäkerhetsgrupper (NSG)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg), och [användardefinierade vägar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) tillåta som toohappen.

I det här scenariot kan du exponera ett program som behöver toobe offentliga toohello Internet och att kan tooconnect tooit från var som helst, eller från specifika platser, beroende på hello konfigurationen av din arbetsbelastning.

#### <a name="point-to-site-vpn-or-site-to-site-vpn"></a>Punkt-till-plats VPN eller VPN för plats-till-plats
Dessa två kan vara hello samma kategori. De båda behöver ditt VNet toohave en VPN-Gateway och du kan ansluta med en VPN-klient till din arbetsstation som en del av hello tooit [punkt-till-plats configuration](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) eller så kan du konfigurera dina lokala [VPN-enhet](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices) toobe kan tooterminate en plats-till-plats-VPN. Det här sättet kan lokala enheter ansluta tooresources inom hello virtuella nätverk.

En punkt-till-plats (P2S) konfiguration kan du skapa en säker anslutning från en enskild klient tooa virtuella datornätverk. P2S är en VPN-anslutning över SSTP (Secure Socket Tunneling Protocol).

![Punkt-till-plats VPN](media/azure-network-security/azure-network-security-fig-5.png)

Punkt-till-plats-anslutningar är användbara när du vill tooconnect tooyour virtuella nätverk från en fjärrdator som hemifrån eller ett konferensrum center eller när du har bara ett fåtal klienter som behöver tooconnect tooa virtuellt nätverk.

P2S-anslutningar kräver inte någon VPN-enhet eller en offentlig IP-adress. Du upprätta hello VPN-anslutning från hello-klientdator. Därför bör P2S inte sätt tooconnect tooAzure om du behöver en beständig anslutning från många lokala enheter och datorer tooyour Azure-nätverk.

![Plats-till-plats VPN](media/azure-network-security/azure-network-security-fig-6.png)

> [!Note]
> Mer information om anslutningar för punkt-till-plats finns hello [punkt-till-plats MI v Q](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal).

En plats-till-plats VPN-gateway-anslutningen har använt tooconnect ditt lokala nätverk tooan virtuella Azure-nätverket via en IPsec/IKE (IKEv1 eller IKEv2) VPN-tunnel.

Den här typen av anslutning kräver en VPN-enhet som finns lokalt som har ett externt Internetriktade offentliga IP-adress som tilldelats tooit. Den här anslutningen sker över hello Internet och kan du för ”tunnel” information i en krypterad anslutning mellan ditt nätverk och Azure. Plats-till-plats VPN är en säker, mogen teknik som har distribuerats av företag i alla storlekar för åren. Tunnel kryptering utförs med [IPSec-tunnelläge](https://technet.microsoft.com/library/cc786385.aspx).

Plats-till-plats VPN är en betrodd, tillförlitlig och etablerade teknik, trafik i hello tunnel passerar hello Internet. Dessutom är är relativt begränsad tooa högst 200 Mbit/s.

Om du behöver en särskilda nivå av säkerhet och prestanda för anslutningar mellan platser, rekommenderar vi att du använder Azure ExpressRoute för dina korsanslutningar. ExpressRoute är en fast WAN länken mellan din lokala plats eller en värdleverantör för Exchange. Eftersom detta är en anger anslutning är data överföras inte via hello Internet och därför är inte synliga toohello potentiella risker vid Internet-kommunikation.

> [!Note]
> Mer information om VPN-gatewayer finns [VPN-gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).

#### <a name="dedicated-wan-link"></a>Fast WAN-länk
Microsoft Azure ExpressRoute kan du utöka ditt lokala nätverk till hello Azure via en dedikerad privata anslutning underlättas av en provider för anslutningen.

ExpressRoute-anslutningar inte överskrider hello offentliga Internet. Detta tillåter ExpressRoute anslutningar toooffer mer tillförlitlighet, högre hastighet, lägre latens och högre säkerhet än vanliga anslutningar via hello Internet.

![ Fast WAN-länk](media/azure-network-security/azure-network-security-fig-7.png)

> [!Note]
> Mer information om hur tooconnect ditt nätverk tooMicrosoft med ExpressRoute finns [ExpressRoute anslutning modeller](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) och [teknisk översikt för ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

Som med hello plats-till-plats VPN-alternativ kan ExpressRoute du också tooconnect tooresources som inte nödvändigtvis bara ett virtuellt nätverk. Du kan i själva verket ansluta too10 Vnet beroende på hello SKU. Om du har hello [premium tillägg](https://docs.microsoft.com/azure/expressroute/expressroute-faqs), anslutningar tooup too100 Vnet är möjligt, beroende på bandbredd. Mer om vad dessa typer av anslutningar Sök vill, läsa toolearn [anslutning Topologidiagram](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="security-controls"></a>Säkerhetsåtgärder
Ett Azure Virtual Network ger en säker, logiska nätverk som är isolerad från andra virtuella nätverk och stöder många säkerhetsåtgärder som du använder på ditt lokala nätverk. Kunderna skapa sina egna struktur med hjälp av: undernät – de använda sina egna privata IP-adressintervall, konfigurera vägtabeller, nätverkssäkerhetsgrupper, åtkomstkontroll ACL-listor, gateways och virtuella installationer toorun arbetsbelastningen i hello molnet.

hello följande är säkerhetsåtgärder som du kan använda i dina virtuella Azure-nätverk:

-   Åtkomstkontroller för nätverk

-   Användardefinierade vägar

-   Säkerhet för nätverksenhet

-   Application Gateway

-   Brandvägg för Azure webbaserade program

-   Kontrollen av nätverkets tillgänglighet

#### <a name="network-access-controls"></a>Åtkomstkontroller för nätverk
Även om hello Azure Virtual Network (VNet) är hello hörnsten i modellen för Azure nätverk och ger isolering och skydd, hello [Nätverkssäkerhetsgrupp grupper (NSG)](https://blogs.msdn.microsoft.com/igorpag/2016/05/14/azure-network-security-groups-nsg-best-practices-and-lessons-learned/) är hello huvudsakliga verktyget tooenforce och kontrollera nätverkstrafik regler på hello nätverksnivån.

![ Åtkomstkontroller för nätverk](media/azure-network-security/azure-network-security-fig-8.png)


Du kan styra åtkomsten genom att tillåta eller neka kommunikation mellan hello arbetsbelastningar inom ett virtuellt nätverk från system i kundens nätverk via anslutning, eller dirigera Internet-kommunikation.

I hello diagram finns både Vnet och NSG: er i ett visst lager i hello Azure övergripande security-stacken, där NSG: er, UDR och virtuella nätverksenheter kan vara används toocreate säkerhet gränser tooprotect hello programdistributioner i hello skyddat nätverk.

NSG: er använder en 5-tuppel tooevaluate trafik (och används i hello regler som du konfigurerar för hello NSG):

-   [Käll- och IP-adress](https://support.microsoft.com/help/969029/the-functionality-for-source-ip-address-selection-in-windows-server-2008-and-in-windows-vista-differs-from-the-corresponding-functionality-in-earlier-versions-of-windows)

-   [Käll- och port](https://technet.microsoft.com/library/dd197515)

-   Protokoll: [Transmission Control Protocol (TCP)](https://technet.microsoft.com/library/cc940037.aspx) eller [User Datagram Protocol (UDP)](https://technet.microsoft.com/library/cc940034.aspx)

Det innebär att du kan styra åtkomsten mellan en enda virtuell dator och en grupp av virtuella datorer eller en enda VM-tooanother enkel virtuell dator eller mellan hela undernät. Igen, Tänk på att det är enkelt tillståndskänslig paket filtrering, inte fullständig paketinspektion. Det finns ingen protokollet verifiering eller nätverksnivån ID: N eller kapaciteten för IP-adresser i en Nätverkssäkerhetsgrupp.

En NSG innehåller vissa inbyggda regler som du bör vara medveten om. Dessa är:

-   **All trafik i ett specifikt virtuellt nätverk:** alla virtuella datorer på hello samma virtuella Azure-nätverket kan kommunicera med varandra.

-   **Tillåt Azure belastningsutjämning tooinbound:** regeln tillåter trafik från alla måladresser källa adress tooany för hello Azure belastningsutjämnare.

-   **Neka alla inkommande:** regeln blockerar all trafik uppspelning från hello Internet som du uttryckligen tillåts.

-   **Tillåt alla trafik utgående toohello Internet:** den här regeln kan virtuella datorer tooinitiate anslutningar toohello Internet. Om du inte vill att dessa anslutningar som initierats måste toocreate en regel tooblock anslutningarna eller tvinga tvingad tunneling.

#### <a name="system-routes-and-user-defined-routes"></a>Systemvägar och användardefinierade vägar

När du lägger till virtuella datorer (VM) tooa virtuella nätverk (VNet) i Azure kan du se hello VMs finns kan toocommunicate med varandra över hello nätverk automatiskt. Du behöver inte toospecify en gateway, även om hello virtuella datorer i olika undernät.

hello samma sak gäller för kommunikation från hello VMs toohello offentliga Internet och även tooyour lokala nätverk när en hybridanslutning från Azure tooyour eget datacenter finns.

![Systemvägar](media/azure-network-security/azure-network-security-fig-9.png)

Det här flödet av kommunikation är möjligt eftersom Azure använder en uppsättning system vägar toodefine hur IP-trafiken flödar. Systemvägar kontrollerar hello kommunikationsflödet i hello följande scenarier:

-   Inifrån hello i samma undernät.

-   Från ett undernät tooanother inom ett VNet.

-   Från virtuella datorer toohello Internet.

-   Från ett VNet tooanother virtuella nätverk via en VPN-gateway.

-   Från ett VNet tooanother virtuella nätverk via VNet-Peering ([Service länkning](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)).

-   Från ett VNet tooyour lokalt nätverk via en VPN-gateway.

Många företag har strikt säkerhet och krav på lokal kontroll av alla nätverk paket tooenforce specifika principerna. Azure tillhandahåller en mekanism som kallas [Tvingad tunneltrafik](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-forced-tunneling) som dirigerar trafik från hello VMs tooon lokala genom att skapa en anpassad väg eller genom att [Border Gateway Protocol (BGP)](https://docs.microsoft.com/windows-server/remote/remote-access/bgp/border-gateway-protocol-bgp) annonser via ExpressRoute eller VPN.

Tvingad tunneling i Azure konfigureras via virtuella nätverk användardefinierade vägar (UDR). Omdirigera trafik tooan lokal plats uttrycks som en standardväg toohello Azure VPN-gateway.

hello innehåller följande avsnitt hello aktuella begränsning av hello routningstabellen och vägar för ett Azure Virtual Network:

-   Varje undernät för virtuellt nätverk har en inbyggd, system-routningstabell. routningstabellen för hello system har hello följande tre grupper av vägar:

 -  **Lokal VNet vägar:** direkt toohello mål virtuella datorer i hello samma virtuella nätverk

 - **På lokala vägar:** toohello Azure VPN-gateway

 -  **Standardvägen:** direkt toohello Internet. Paket avsedda toohello privata IP-adresser som inte omfattas av hello föregående två vägar tas bort.

-   Hello versionen av användardefinierade vägar kan du skapa en routning tabell tooadd en standardväg och sedan associerar hello routning tabell tooyour VNet subnet tooenable Tvingad tunneltrafik på dessa undernät.

-   Du måste tooset en ”standardwebbplats” bland hello lokala mellan lokala platser anslutna toohello virtuellt nätverk.

-   Tvingad tunneling måste vara kopplad till ett virtuellt nätverk som har en dynamisk routning VPN-gateway (inte en statisk gateway).

- ExpressRoute Tvingad tunneltrafik konfigureras inte via den här mekanismen men i stället har aktiverats genom att annonsera en standardväg via hello ExpressRoute BGP peeringsessioner.

> [!Note]
> Mer information finns i hello [ExpressRoute dokumentation](https://azure.microsoft.com/documentation/services/expressroute/) för mer information.

#### <a name="network-security-appliances"></a>Nätverksinstallationer för säkerhet
Medan Nätverkssäkerhetsgrupper och användardefinierade vägar kan tillhandahålla ett viss mått av nätverkssäkerhet på hello nätverks- och lager i hello [OSI-modell](https://en.wikipedia.org/wiki/OSI_model), kommer det toobe situationer där du vill eller behöver tooenable säkerhet på högre nivåer av hello nätverksstacken. I sådana situationer rekommenderar vi att du distribuerar virtuella nätverk säkerhetsenheter som tillhandahålls av Azure partners.

![Nätverksinstallationer för säkerhet](./media/azure-network-security/azure-network-security-fig-10.png)

Azure security nätverksinstallationer förbättra VNet säkerhets- och nätverksfunktioner och de är tillgängliga från flera leverantörer via hello [Azure Marketplace](https://azuremarketplace.microsoft.com). Dessa virtuella säkerhetsenheter kan vara distribuerade tooprovide:

-   Hög tillgänglighet brandväggar

-   Intrångsskydd

-   Intrångsidentifiering

-   Web application brandväggar (WAFs)

-   WAN-optimering

-   Routning

-   Belastningsutjämning

-   VPN

-   Certifikathantering

-   Active Directory

-   Multifaktorautentisering

#### <a name="application-gateway"></a>Programgateway

[Microsoft Azure Programgateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) är en dedikerad virtuell installation som ger en programmet leverans styrenhet (ADC) som en tjänst.

 ![Application Gateway](./media/azure-network-security/azure-network-security-fig-11.png)

Programgateway kan du toooptimize web servergruppen prestanda och tillgänglighet genom att avlasta CPU beräkningsintensiva SSL-avslutning toohello Programgateway (SSL-avlastning). Det finns även andra layer 7 funktioner inklusive:

-   Resursallokering för inkommande trafik

-   Cookie-baserad session tillhörighet

-   URL-sökväg-baserad Routning

-   Möjlighet toohost flera webbplatser bakom en enda Programgateway


En [Brandvägg för webbaserade program (Brandvägg)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) ges också som en del av hello Programgateway. Detta ger skydd tooweb program från vanliga web säkerhetsrisker och trojaner. Programgateway kan konfigureras som en Internetuppkopplad gateway, interna endast gateway eller en kombination av båda.

Programmet Gateway Brandvägg kan köras i läget identifiering eller förebyggande. Det är ett vanliga användningsfall för administratörer toorun i identifiering tooobserve trafik för skadliga mönster. När potentiella kryphål identifieras blockerar aktivera tooprevention läge misstänkta inkommande trafik.

 ![Application Gateway](./media/azure-network-security/azure-network-security-fig-12.png)

Dessutom kan programmet Gateway Brandvägg hjälper dig att övervaka webbprogram mot attacker med hjälp av en Brandvägg realtidsloggen som är integrerad med [Azure-Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) och [Azure Security Center](https://azure.microsoft.com/services/security-center/) tootrack Brandvägg aviseringar och enkelt kan övervaka trender.

hello JSON-formaterad logg går direkt toohello kundens lagringskonto. Du har fullständig kontroll över dessa loggar och kan tillämpa en egen bevarandeprinciper.

Du kan också mata in dessa loggar i din egen analytics system med hjälp av [Azure Log-integrering](https://aka.ms/AzLog). Brandvägg loggar också är integrerade med [Operations Management Suite (OMS)](https://www.microsoft.com/cloud-platform/operations-management-suite) så att du kan använda OMS logganalys tooexecute sofistikerade detaljerade frågor.

#### <a name="azure-web-application-firewall-waf"></a>Azure web application firewall (Brandvägg)

Webbprogram allt är riktad mot angrepp som utnyttjar vanliga kända sårbarheter, till exempel SQL injection mellan platsen skriptmiljö attacker och andra attacker som visas i hello [OWASP topp 10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project). Förhindra sådan kryphål i hello program kräver rigorösa underhåll, uppdatering och övervakning av flera lager av hello programmet topologi.

 ![Azure Web Application Firewall (Brandvägg)](./media/azure-network-security/azure-network-security-fig-13.png)

En centraliserad [Brandvägg för webbaserade program (Brandvägg)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) kan skydda mot webbattacker och säkerhetshanteringen utan några ändringar i programmet.

En Brandvägg-lösning kan också reagera tooa säkerhetshot snabbare med uppdatering av ett känt problem på en central plats jämfört med att skydda alla enskilda webbprogram. Befintliga programgatewayer kan enkelt vara Programgateway för konverterade tooa web application-brandväggen är aktiverad.

#### <a name="network-availability-controls"></a>Nätverkskontroller tillgänglighet

Det finns olika alternativ toodistribute nätverkstrafik med hjälp av Microsoft Azure. De här alternativen fungerar på olika sätt, har olika funktionsuppsättningar och stöder olika scenarier. De kan användas var för sig eller i kombination med varandra.

Följande är hello nätverkskontroller tillgänglighet:

-   Azure Load Balancer

-   Application Gateway

-   Traffic Manager

**Azure belastningsutjämnare**

Ger hög tillgänglighet och nätverksprestanda tooyour program. Det är en belastningsutjämnare för lager 4 (TCP, UDP) som distribuerar inkommande trafik mellan felfri instanser av tjänster som definierats i en belastningsutjämnad uppsättning.

 ![Azure Load Balancer](media/azure-network-security/azure-network-security-fig-14.png)


Azure belastningsutjämnare kan konfigureras för att:

-   Läsa belastningsutjämna inkommande Internet-trafik toovirtual datorer. Den här konfigurationen kallas [mot Internet för belastningsutjämning](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview).

-   Läs in Utjämna trafiken mellan virtuella datorer i ett virtuellt nätverk, mellan virtuella datorer i molntjänster eller mellan lokala datorer och virtuella datorer i ett virtuellt nätverk mellan platser. Den här konfigurationen kallas [intern belastningsutjämning](https://docs.microsoft.com/azure/load-balancer/load-balancer-internal-overview).

-   Vidarebefordra externa trafiken tooa specifik virtuell dator.

Alla resurser i molnet hello måste en offentlig IP-adress toobe kan nås från hello Internet. hello moln-infrastruktur i Azure använder icke-dirigerbara IP-adresser för dess resurser. Azure använder network address translation (NAT) med offentliga IP-adresser toocommunicate toohello Internet.

 **Programgateway**

 Programgateway fungerar på programnivå hello (Layer 7 i hello OSI-nätverksstacken referens). Det fungerar som en omvänd proxy-tjänst, avslutar hello klientanslutningen och vidarebefordran begär tooback slutpunkt slutpunkter.

 **Traffic manager**

Microsoft Azure Traffic Manager kan du toocontrol hello distributionen av användartrafik för slutpunkter i olika datacenter. Service-slutpunkter som stöds av Traffic Manager omfattar virtuella Azure-datorer Web Apps och molntjänster. Du kan även använda Traffic Manager med externa slutpunkter som inte tillhör Azure.

Traffic Manager använder hello System DNS (Domain Name) toodirect klient begär toohello lämpligaste slutpunkten baserat på en [routning av nätverkstrafik metoden](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) och hello hälsa hello slutpunkter. Traffic Manager erbjuder en uppsättning routning av nätverkstrafik metoder toosuit olika programbehov, endpoint hälsa [övervakning](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring), och automatisk redundans. Traffic Manager är flexibel toofailure, inklusive hello fel på en hel Azure-region.

Azure Traffic Manager kan du toocontrol hello fördelning av trafik över dina slutpunkter för programmet. En slutpunkt är alla mot Internet-tjänst som finns i eller utanför Azure.

Traffic Manager har två viktiga fördelar:

-   Fördelning av trafik enligt tooone av flera [routning av nätverkstrafik metoder](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods).

-   [Kontinuerlig övervakning av hälsotillstånd för slutpunkten](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring) och automatisk redundans när slutpunkter misslyckas.

När en klient försöker tooconnect tooa service, måste det först lösa hello DNS-namnet på hello-tooan IP-adress. hello klienten ansluter sedan toothat IP-adress tooaccess hello-tjänsten. Traffic Manager använder DNS-toodirect klienter toospecific tjänstens slutpunkter baserat på hello regler för routning av nätverkstrafik hello-metoden. Klienterna ansluter direkt toohello markerad slutpunkt. Traffic Manager är inte en proxy eller en gateway. Traffic Manager kan inte se hello trafik som passerar mellan hello klient- och hello.

### <a name="azure-network-validation"></a>Validering av Azure-nätverk

Verifiering av Azure-nätverk är tooensure som hello Azure-nätverk fungerar eftersom den är konfigurerad och verifiering kan göras med hello tjänster och funktioner tillgängliga toomonitor hello-nätverk. Med Azure Nätverksbevakaren du har åtkomst till en mängd olika loggning och diagnostik funktioner som ge du insikter toounderstand nätverksprestanda- och hälsa. Dessa funktioner är tillgängliga via portalen, Power Shell, CLI, Rest-API och SDK.

Azure operativ säkerhet refererar toohello tjänster, kontroller och funktioner tillgängliga toousers för att skydda sina data, program och andra resurser i Microsoft Azure. Azure operativ säkerhet bygger på ett ramverk som inkluderar hello kunskap via en olika funktioner som är unika tooMicrosoft, inklusive hello Microsoft Security Development Lifecycle (SDL), hello Microsoft Security Response Center programmet och djup medvetenhet om hello cyber security hotbild.

-   [Azure Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview)

-   [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)

-   [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

-   [Azure nätverksbevakaren](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

-   [Azure Storage analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)

-   Azure Resource Manager

#### <a name="azure-resource-manager"></a>Azure resource manager

hello personer och processer som fungerar Microsoft Azure är kanske hello viktigaste säkerhetsfunktion hello plattform. Det här avsnittet beskrivs funktionerna i Microsofts globala infrastruktur underlätta och upprätthålla säkerheten, verksamhetskontinuitet och sekretess.

hello infrastrukturen för ditt program består normalt av många komponenter – kanske en virtuell dator, storage-konto och virtuella nätverk eller en webbapp, databas, databasserver och tjänster från tredje part. Du ser inte de här komponenterna som separata entiteter, utan som relaterade delar av samma enhet som är beroende av varandra. Du vill toodeploy, hantera och övervaka dem som en grupp. Azure Resource Manager aktiverar toowork med hello resurser i din lösning som en grupp.

Du kan distribuera, uppdatera eller ta bort alla hello resurser i lösningen i en enda, samordnad åtgärd. Du använder en mall för distributionen. Mallen kan användas i olika miljöer, till exempel för testning, mellanlagring och produktion. Resource Manager tillhandahåller säkerhets-, granskning och märkning funktioner toohelp hantera dina resurser efter distributionen.

**hello fördelarna med att använda Resource Manager**

Resource Manager har flera fördelar:

-   Du kan distribuera, hantera och övervaka alla hello resurser för din lösning som en grupp i stället för att hantera resurserna separat.

-   Du kan flera gånger distribuerar din lösning i hela hello utvecklingslivscykeln och vara säker på att dina resurser distribueras i ett konsekvent tillstånd.

-   Du kan hantera infrastrukturen med hjälp av deklarativa mallar i stället för skript.

-   Du kan definiera hello beroenden mellan resurser, så att de distribueras i rätt ordning för hello.

-   Du kan använda tjänster för åtkomstkontroll tooall i resursgruppen eftersom rollbaserad åtkomstkontroll (RBAC) är inbyggt i hello plattform.

-   Du kan lägga till taggar tooresources toologically ordna alla hello resurser i din prenumeration.

-   Du kan tydliggöra fakturering för din organisation genom att visa kostnaderna för en grupp med resurser som delar taggen.

> [!Note]
> Resource Manager tillhandahåller ett nytt sätt toodeploy och hantera dina lösningar. Om du använde hello tidigare distributionsmodellen och vill toolearn om hello ändringarna, se [förstå Resource Manager distribution och klassisk distribution](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model).

## <a name="azure-network-logging-and-monitoring"></a>Azure-nätverk loggning och övervakning

Azure erbjuder många verktyg toomonitor, förebygga, upptäcka och åtgärda toonetwork säkerhetshändelser. Några av hello mest kraftfulla verktyg tillgängliga tooyou i det här området är:

-   Network Watcher

-   Resursen nivån nätverksövervakning

-   Log Analytics

### <a name="network-watcher"></a>Nätverksbevakaren

[Nätverk Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) -scenariot-baserad övervakning medföljer hello funktioner i Nätverksbevakaren. Den här tjänsten innehåller paketinsamling, nästa hopp, IP-flöde Kontrollera NSG flödet loggar i gruppvyn säkerhet. Scenariot nivån övervakning innehåller en end tooend för nätverksresurser i kontrast tooindividual resurs nätverksövervakning.

 ![Network Watcher](./media/azure-network-security/azure-network-security-fig-15.png)

Nätverksbevakaren är en regionala tjänst som gör att du toomonitor och diagnostisera villkor på nätverket scenariot nivå, till och från Azure. Diagnostiska nätverks- och visualiseringsverktyg som finns tillgängliga med Nätverksbevakaren hjälpa dig att förstå, diagnostisera och få insikter tooyour nätverk i Azure.

Nätverksbevakaren har för närvarande hello följande funktioner:

#### <a name="topology"></a>topologi

[Topologi](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview) returnerar ett diagram över nätverksresurser i ett virtuellt nätverk. hello diagrammet visar hello sambandet mellan hello resurser toorepresent hello slutet tooend nätverksanslutning. I hello portal returnerar topologi hello resursobjekt på enligt grunden för virtuella nätverk. hello relationer illustreras med linjer mellan hello resurser utanför hello Nätverksbevakaren region, även om hello resurs grupp inte ska visas. hello resurser returneras i hello portal-vy är en delmängd av hello nätverkskomponenter som är visas i diagram registreringen. toosee hello fullständig lista över nätverksresurser, som du kan använda [PowerShell](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-powershell) eller [REST](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-rest).

Som resurser returneras modelleras hello anslutning mellan de under två relationer.

- **Inneslutning** -virtuellt nätverk innehåller ett undernät som innehåller ett nätverkskort.

- **Associerade** -A NIC som är kopplad till en virtuell dator.

#### <a name="variable-packet-capture"></a>Variabeln paketinsamling

Nätverk Watcher [variabeln paketinsamling](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) kan du toocreate paket avbilda sessioner tootrack trafik tooand från en virtuell dator. Paketinsamling hjälper toodiagnose nätverk avvikelser båda reaktivt och proactivity. Andra användningsområden omfattar att samla in nätverksstatistik får information om nätverket intrång, toodebug klient / server-kommunikation och mycket mer.

Paketinsamling är ett tillägg för virtuell dator som startas från en fjärrdator via Nätverksbevakaren. Den här funktionen underlättar hello belastningen för körs en paketinsamling manuellt på hello önskade virtuella datorn, vilket sparar värdefull tid. Paketinsamling kan aktiveras via hello portal, PowerShell, CLI eller REST API. Ett exempel på hur paketinsamling kan utlösas är med virtuella aviseringar.

#### <a name="ip-flow-verify"></a>Kontrollera IP-flöde

[Kontrollera IP-flöden](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) kontrollerar om ett paket tillåts eller nekas tooor från en virtuell dator baserat på 5-tuppel information. Den här informationen består av riktning, protokoll, lokal IP-, fjärr-IP, lokal port och Fjärrport. Om hello paket nekas av en säkerhetsgrupp, returneras hello namn på hello-regel som nekats hello-paket. När alla käll- eller IP-adresser kan väljas funktionen hjälper administratörer att snabbt diagnostisera problem med nätverksanslutningen från eller toohello internet och från eller toohello lokala miljö.

IP-flöden Kontrollera riktar sig till ett nätverksgränssnitt för en virtuell dator. Trafikflöde verifieras sedan baserat på konfigurerad hello inställningar tooor från nätverksgränssnittet. Den här funktionen är användbar för att bekräfta om en regel i en Nätverkssäkerhetsgrupp blockerar tooor för meddelanden om ingångs- eller utgående trafik från en virtuell dator.

#### <a name="next-hop"></a>Nästa hopp

Anger hello [nästa hopp](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) för paket som vidarebefordras i hello Azure nätverksinfrastruktur, aktiverar du toodiagnose alla felkonfigurerat användardefinierade vägar. Trafik från en virtuell dator skickas tooa mål baserat på hello effektiva vägar som är associerade med ett nätverkskort. Nästa hopp hämtar hello nästa hopptyp och IP-adressen för ett paket från en specifik virtuell dator och nätverkskort. Detta hjälper toodetermine om hello paketet tas dirigerad toohello mål eller är hello trafik som svart holed.

Nästa hopp returnerar också hello vägtabell associerad med hello nästa hopp. När du frågar ett nexthop om hello flödet definieras som en användardefinierad väg, returneras den rutten. Annars returnerar nästa hopp ”Systemväg”.

#### <a name="security-group-view"></a>gruppvy för säkerhet

Hämtar hello effektiva och tillämpade säkerhetsregler som tillämpas på en virtuell dator. Nätverkssäkerhetsgrupper associeras på en undernätverksnivå eller på en NIC-nivå. När associerade på en undernätverksnivå, tillämpas tooall hello VM-instanser i hello undernät. Nätverket [säkerhetsgrupp visa](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview) returnerar alla hello konfigurerats NSG: er och regler som är associerade på ett nätverkskort och undernät nivå för en virtuell dator som ger inblick i hello konfiguration. Dessutom returneras hello effektiva säkerhetsregler för varje hello nätverkskort i en virtuell dator. Med hjälp av Nätverkssäkerhetsgruppen vyn du utvärdera en virtuell dator för säkerhetsrisker i nätverket, till exempel öppna portar. Du kan också verifiera om säkerhetsgrupp för nätverk fungerar som förväntat baserat på en [jämförelse mellan hello konfigurerad och hello effektiva säkerhetsregler](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-auditing-powershell).

#### <a name="nsg-flow-logging"></a>NSG flöda loggning

 Loggarna flöde Nätverkssäkerhetsgrupper aktivera toocapture loggar relaterade tootraffic som tillåts eller nekas av hello säkerhetsregler i hello-gruppen. hello flödet definieras av en 5-tuppel information – käll-IP, mål-IP, källport, målport och protokoll.

[Nätverkssäkerhetsgruppen flöde loggar](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) är en funktion i Nätverksbevakaren som gör att du tooview information om ingående och utgående IP-trafik via en Nätverkssäkerhetsgrupp.

#### <a name="virtual-network-gateway-and-connection-troubleshooting"></a>Virtuella nätverksgatewayen och anslutningen felsökning

Nätverksbevakaren innehåller många funktioner när det gäller toounderstanding nätverksresurserna i Azure. En av dessa funktioner är resurs felsökning. [Felsökning av resursen](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest) kan anropas av PowerShell, CLI eller REST API. När den anropas, Nätverksbevakaren inspektera hello hälsotillståndet för en virtuell nätverksgateway eller en anslutning och returnerar resultatet.

Det här avsnittet tar dig igenom hello olika administrativa uppgifter som är tillgängliga för felsökning av resursen.

-   [Felsöka en virtuell nätverksgateway](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest)

-   [Felsöka en anslutning](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest)

#### <a name="network-subscription-limits"></a>Nätverket prenumerationsbegränsningar

[Nätverk prenumerationsbegränsningar](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) ger dig information om hello användning av varje hello nätverksresurs i en prenumeration i en region mot hello maximalt antal tillgängliga resurser.

#### <a name="configuring-diagnostics-log"></a>Konfigurera diagnostik logg

Nätverksbevakaren ger en [diagnostikloggar](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) vyn. Den här vyn innehåller alla nätverksresurser som stöder diagnostikloggning. I den här vyn kan du aktivera och inaktivera nätverksresurser lätt och snabbt.

### <a name="network-resource-level-monitoring"></a>Resursen nivån nätverksövervakning

hello följande funktioner är tillgängliga för nivån Resursövervakning:

#### <a name="audit-log"></a>granskningslogg

Åtgärder som utförs som en del av hello konfiguration av nätverk loggas. Dessa granskningsloggar är mycket viktigt tooestablish olika compliances. Dessa loggar kan visas i hello Azure-portalen eller hämtas med hjälp av Microsoft-verktyg, till exempel Power BI eller verktyg från tredje part. Granskningsloggar är tillgängliga via hello portal, PowerShell, CLI och Rest-API.

> [!Note]
> Mer information om granskningsloggarna finns [granskningsåtgärder med Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
Granskningsloggar är tillgängliga för åtgärder som utförs på alla nätverksresurser.


#### <a name="metrics"></a>Mått

Mått är prestandamått och prestandaräknare som samlats in under en period. Mått är tillgängliga för Programgateway. Mått kan vara används tootrigger varningar baserat på tröskelvärdet. Azure Application Gateway som standard övervakar hello hälsotillståndet för alla resurser i dess backend-pool och tar automatiskt bort alla resurser som bedöms som dåligt hello poolen. Programgateway fortsätter toomonitor hello ohälsosamt instanser och lägger till dem tillbaka toohello felfri backend-poolen när de blir tillgängliga och svara toohealth-avsökningar. Programgateway skickar hello hälsoavsökningar med hello samma port som har definierats i hello backend-HTTP-inställningar. Den här konfigurationen garanterar att hello avsökningen provar hello samma port som kunder kan använda tooconnect toohello backend.

> [!Note]
> Se [Gateway Programdiagnostik](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview) tooview hur mått kan vara används toocreate aviseringar.

#### <a name="diagnostic-logs"></a>Diagnostikloggar

Periodiska och eget initiativ händelser skapas av nätverksresurser och inloggad storage-konton, skickas tooan Event Hub eller logganalys. Dessa loggar ger insikter om hello hälsotillståndet för en resurs. Dessa loggar kan visas i verktyg som Power BI och logganalys. hur tooview diagnostikloggar, besök toolearn [logganalys](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics).

Diagnostikloggar är tillgängliga för [belastningsutjämnaren](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log), [Nätverkssäkerhetsgrupper](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log), vägar och [Programgateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).

Nätverksbevakaren ger diagnostikloggar vyn. Den här vyn innehåller alla nätverksresurser som stöder diagnostikloggning. I den här vyn kan du aktivera och inaktivera nätverksresurser lätt och snabbt.

### <a name="log-analytics"></a>Log Analytics

[Logga Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) är en tjänst i [Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) som övervakar dina molntjänster och lokala miljöer toomaintain deras tillgänglighet och prestanda. Den samlar in data som genereras av resurser i dina miljöer i molnet och lokalt och från andra övervakning verktyg tooprovide analys över flera källor.

Logganalys erbjuder följande lösningar för att övervaka dina nätverk hello:

-   Network Performance Monitor (NPM)

-   Azure Application Gateway analytics

-   Azure Network Security Group analytics

#### <a name="network-performance-monitor-npm"></a>Prestandaövervakaren för nätverk (NPM)
Hej [Network Performance Monitor](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor) hanteringslösning är ett nätverk övervakningslösning som övervakar hello hälsa, tillgänglighet och tillgänglighet av nätverk.

Det är används toomonitor anslutningen mellan:

-   offentliga molnet och lokalt

-   datacenter och användarplatser (avdelningskontor)

-   undernät som är värd för olika nivåer av ett program med flera nivåer.


#### <a name="azure-application-gateway-analytics-in-log-analytics"></a>Azure-program gateway analyser i logganalys

följande loggar hello stöds för Programgatewayer:

-   ApplicationGatewayAccessLog

-   ApplicationGatewayPerformanceLog

-   ApplicationGatewayFirewallLog

hello följande mått stöds för Programgatewayer:

-   5 minuter genomflöde

#### <a name="azure-network-security-group-analytics-in-log-analytics"></a>Analyser av gruppen för Azure-nätverk i logganalys

hello följande loggar stöds för [nätverkssäkerhetsgrupper](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log):

- **NetworkSecurityGroupEvent:** innehåller poster för vilka NSG regler är tillämpade tooVMs och instans roller baserat på MAC-adress. hello status för de här reglerna samlas var 60: e sekund.

- **NetworkSecurityGroupRuleCounter:** innehåller poster för hur många gånger varje NSG regel är tillämpade toodeny eller Tillåt trafiken.

## <a name="next-steps"></a>Nästa steg
Lär dig mer om säkerheten genom att läsa in några av våra djupgående säkerhetsfrågor:

-   [Log Analytics för Nätverkssäkerhetsgrupper (NSG: er)](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log)

-   [Nätverk innovationer som enheten hello avbrott i molnet](https://azure.microsoft.com/blog/networking-innovations-that-drive-the-cloud-disruption/)

-   [SONiC: hello nätverksprogramvara växeln som används av hello Microsoft globala molnet](https://azure.microsoft.com/blog/sonic-the-networking-switch-software-that-powers-the-microsoft-global-cloud/)

-   [Hur Microsoft bygger dess snabb och tillförlitlig globalt nätverk](https://azure.microsoft.com/blog/how-microsoft-builds-its-fast-and-reliable-global-network/)

-   [Ljus upp nätverk](https://azure.microsoft.com/blog/lighting-up-network-innovation/)
