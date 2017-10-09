---
title: "aaaAzure nätverk säkerhetsmetoder | Microsoft Docs"
description: "Den här artikeln innehåller en uppsättning av bästa praxis för säkerhet med inbyggda funktioner i Azure."
services: security
documentationcenter: na
author: TomShinder
manager: swadhwa
editor: TomShinder
ms.assetid: 7f6aa45f-138f-4fde-a611-aaf7e8fe56d1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/09/2017
ms.author: TomSh
ms.openlocfilehash: 5867dea358b4da65c65b3e52fcab7e687e981642
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-network-security-best-practices"></a>Säkerhetsmetoder för Azure-nätverk
Microsoft Azure kan du tooconnect virtuella datorer och redskap tooother nätverksansluten enheter genom att placera dem på Azure-nätverk. Ett Azure Virtual Network är en konstruktion för virtuella nätverk som gör att du tooconnect virtuellt nätverk gränssnittet kort tooa virtuellt nätverk tooallow TCP/IP-baserade kommunikation mellan nätverksenheter som aktiveras. Virtuella Azure-datorer anslutna tooan Azure Virtual Network är kan tooconnect toodevices på hello samma Azure Virtual Network, olika virtuella Azure-nätverk, på hello Internet eller ens på din egen lokala nätverk.

I den här artikeln diskuteras en samling Azure-nätverk säkerhetsmetoder. Följande rekommendationer härleds från våra erfarenhet av Azure-nätverk och hello erfarenheter från kunder som dig själv.

För varje rekommenderar förklarar vi:

* Vilka hello bra
* Varför du vill tooenable som bästa praxis
* Vad kan vara hello resultat om du inte bästa praxis för tooenable hello
* Möjliga alternativ toohello bästa praxis
* Hur du kan lära dig tooenable hello bästa praxis

Den här artikeln Azure Network säkerhetsmetoder baseras på en konsensus åsikt och Azure plattformsfunktioner och funktioner, som de finns på hello tid som den här artikeln skrevs. Åsikter och tekniker ändras med tiden och den här artikeln kommer att uppdateras på ett regelbundet tooreflect ändringarna.

Azure Network Metodtips om säkerhet i den här artikeln omfattar:

* Logiskt segmentet undernät
* Styra dirigeringsbeteendet
* Aktivera Tvingad tunneltrafik
* Använda virtuella nätverksenheter
* Distribuera DMZs för säkerhet zonindelning
* Undvika exponering toohello Internet med fast WAN-länkar
* Optimera drifttid och prestanda
* Använd globala belastningsutjämning
* Inaktivera RDP-åtkomst tooAzure virtuella datorer
* Aktivera Azure Security Center
* Utöka ditt datacenter till Azure

## <a name="logically-segment-subnets"></a>Logiskt segmentet undernät
[Virtuella Azure-nätverk](https://azure.microsoft.com/documentation/services/virtual-network/) är liknande tooa LAN på ditt lokala nätverk. hello idé bakom ett virtuellt Azure-nätverk är att du skapar ett enda privat IP-adress-baserade nätverk där du kan placera alla dina [Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/). hello privata IP-adressutrymmen tillgängliga finns i hello klass A (10.0.0.0/8) klass B (172.16.0.0/12) och klass C (192.168.0.0/16) intervall.

Liknande toowhat du lokalt, ska du toosegment hello större adressutrymme i undernät. Du kan använda [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) baserat undernät principer toocreate dina undernät.

Routning mellan undernät sker automatiskt och du behöver inte konfigurera routningstabeller för toomanually. Hello standardinställningen är dock att det finns inga nätverk åtkomstkontroller mellan hello undernät som du skapar på hello Azure Virtual Network. I ordning toocreate nätverk åtkomstkontroller mellan undernät behöver du tooput något mellan hello undernät.

Hello saker du kan använda tooaccomplish som den här aktiviteten är en [Nätverkssäkerhetsgruppen](../virtual-network/virtual-networks-nsg.md) (NSG). NSG: er är enkla tillståndskänslig paketinspektion enheter som använder hello 5-tuppel (hello källans IP-adress, källport, mål-IP, målport och lager 4 protocol), hanterar toocreate Tillåt/neka regler för nätverkstrafik. Du kan tillåta eller neka trafik tooand från IP-adress, tooand från flera IP-adresser eller även tooand från hela undernät.

Med hjälp av NSG: er för åtkomstkontrollen för nätverket mellan undernät kan du tooput resurser som tillhör toohello samma säkerhetszon eller roll i deras egna undernät. Tänk till exempel på en enkel 3-nivåprogram som har en webbnivå, ett program logik-nivå och en databasnivå. Du kan placera virtuella datorer som tillhör tooeach på dessa nivåer i sina egna undernät. Sedan kan du använda NSG: er toocontrol trafik mellan hello undernät:

* Web nivå virtuella datorer bara kan initiera anslutningar toohello programmet logik datorer och kan bara godkänna anslutningar från hello Internet
* Programmet logik virtuella datorer bara kan upprätta anslutningar med databasnivå och kan bara godkänna anslutningar från hello webbnivå
* Databasen nivå virtuella datorer kan inte upprätta anslutning med något utanför sin egen undernät och kan bara godkänna anslutningar från hello programmet logik nivå

toolearn mer om Nätverkssäkerhetsgrupper och hur du kan använda dem toologically segment dina virtuella Azure-nätverk finns hello artikel [vad är en Nätverkssäkerhetsgrupp](../virtual-network/virtual-networks-nsg.md) (NSG).

## <a name="control-routing-behavior"></a>Styra dirigeringsbeteendet
När du placerar en virtuell dator på ett Azure Virtual Network, Lägg märke till att hello den virtuella datorn kan ansluta tooany annan virtuell dator på hello samma Azure Virtual Network, även om hello andra virtuella datorer finns i olika undernät. hello beror varför det är möjligt på att det finns en uppsättning systemvägar som är aktiverad som standard som gör att den här typen av kommunikation. Dessa standardvägar att virtuella datorer på hello samma Azure Virtual Network tooinitiate anslutningar med varandra och med hello Internet (för utgående kommunikation toohello Internet endast).

Hello standardvägar system är användbara för flera olika distributionsscenarier, finns men det tider då du vill toocustomize hello routningskonfiguration för din distribution. Dessa anpassningar kan du tooconfigure hello nästa hopp-adress tooreach specifika mål.

Vi rekommenderar att du konfigurerar användardefinierade vägar när du distribuerar ett virtuellt nätverk postsäkerhet som kommer vi i senare bästa praxis.

> [!NOTE]
> Användardefinierade vägar är inte obligatoriska och hello standardvägar system fungerar i de flesta fall.
>
>

Du kan lära dig mer om användardefinierade vägar och hur tooconfigure dem genom att läsa hello artikel [vad är användardefinierade vägar och IP-vidarebefordran](../virtual-network/virtual-networks-udr-overview.md).

## <a name="enable-forced-tunneling"></a>Aktivera Tvingad tunneltrafik
toobetter förstå Tvingad tunneling, är det användbart toounderstand vilka ”delade tunnlar” är.
hello vanligaste exempel på delade tunnlar visas med VPN-anslutningar. Anta att du upprättar en VPN-anslutning från hotell rum tooyour företagets nätverk. Den här anslutningen kan du tooconnect tooresources i företagsnätverket och alla tooresources för kommunikation i företagsnätverket gå igenom hello VPN-tunnel.

Vad händer när du vill tooconnect tooresources på hello Internet? När delade tunnlar aktiveras anslutningarna gå direkt toohello Internet och inte via hello VPN-tunnel. Vissa säkerhetsexperter anser detta toobe en möjlig risk och rekommenderar därför att delade tunnlar inaktiveras och alla anslutningar, de avsedda för hello Internet och de avsedda för företagsresurser, gå igenom hello VPN-tunnel. hello fördelen med att göra detta är att anslutningar toohello Internet tvingas sedan via hello företagsnätverket säkerhetsenheter som skulle vara fallet hello om hello VPN-klienten ansluten toohello Internet utanför hello VPN-tunnel.

Nu ska vi ta detta tillbaka toovirtual datorer på ett Azure Virtual Network. hello standardvägar för ett Azure Virtual Network kan virtuella datorer tooinitiate trafik toohello Internet. Detta kan för utgöra en säkerhetsrisk eftersom dessa utgående anslutningar kan öka hello risken för angrepp på en virtuell dator och kan användas av angripare.
Därför rekommenderar vi att du aktiverar Tvingad tunneling på virtuella datorer när du har anslutning mellan ditt Azure-nätverk och ditt lokala nätverk. Vi pratar om mellan lokala anslutningen senare i Azure nätverk bästa praxis dokumentet.

Om du inte har en plats-anslutning, kontrollerar du att du dra nytta av Nätverkssäkerhetsgrupper (beskrivs tidigare) eller Azure virtuellt nätverk Säkerhet installationer (beskrivs bredvid) tooprevent utgående anslutningar toohello Internet från din Azure-virtuella Datorer.

Mer om toolearn Tvingad tunneltrafik och hur tooenable, Läs hello artikeln [konfigurera Tvingad Tunneling med PowerShell och Azure Resource Manager](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md).

## <a name="use-virtual-network-appliances"></a>Använda virtuella nätverksenheter
Medan Nätverkssäkerhetsgrupper och användaren definierat routning kan tillhandahålla ett viss mått av nätverkssäkerhet på hello nätverks- och lager i hello [OSI-modell](https://en.wikipedia.org/wiki/OSI_model), kommer det toobe situationer där du ska vill eller behöver tooenable säkerhet på hög hello stacken. I sådana situationer rekommenderar vi att du distribuerar virtuella nätverk säkerhetsenheter som tillhandahålls av Azure partners.

Azure security nätverksinstallationer kan leverera avsevärt förbättrad säkerhetsnivåer över vad som tillhandahålls av nätverkskontroller nivå. Hello nätverket säkerhetsfunktioner som tillhandahålls av virtuellt nätverk säkerhetsenheter bland annat:

* Firewalling
* Intrång identifiering/intrång förebyggande
* Hantering av säkerhetsrisk
* Programkontrollen
* Nätverksbaserade avvikelseidentifiering
* Filtrering av webbprogram
* Antivirusprogram
* Botnät skydd

Om du behöver en högre nivå av nätverkssäkerhet än du kan hämta med administratörsnivå nätverkskontroller sedan rekommenderar vi att du undersöka och distribuera virtuella Azure-nätverket säkerhetsenheter.

toolearn om vilka virtuella Azure-nätverket säkerhetsenheter som är tillgängliga och deras funktioner finns hello [Azure Marketplace](https://azure.microsoft.com/marketplace/) och Sök efter ”säkerhet” och ”nätverkssäkerhet”.

## <a name="deploy-dmzs-for-security-zoning"></a>Distribuera DMZs för säkerhet zonindelning
En DMZ ”perimeternätverk” är en fysisk eller logisk nätverkssegment som är utformade tooprovide ett extra säkerhetslager mellan dina tillgångar och hello Internet. hello syftet med hello DMZ är tooplace specialiserade nätverksenheter åtkomstkontroll på hello gränsen på hello DMZ nätverk så att endast önskade trafik tillåts tidigare hello nätverksenheten för säkerhet och i ditt Azure-nätverk.

DMZs är användbara eftersom du kan fokusera åtkomstkontrollhantering ditt nätverk, övervakning, loggning och rapportering av hello enheter längs hello ditt Azure-nätverk. Här skulle du vanligtvis aktivera DDoS förebyggande, intrång identifiering/intrångsskydd system (ID: N/IP-adresser), brandväggsregler och principer, webbfiltrering, nätverket program mot skadlig kod och mycket mer. hello nätverksenheter säkerhet mellan hello Internet och det virtuella Azure-nätverket och har ett gränssnitt för båda nätverken.

Detta är hello grundläggande utformning av en DMZ, men det finns många olika DMZ-Designer som back-to-back tre-homed, multi-homed med mera.

Vi rekommenderar för alla distributioner med hög säkerhet du överväga att distribuera en DMZ tooenhance hello nivå av nätverkssäkerhet för Azure-resurser.

Mer om DMZs och hur toodeploy dem i Azure, Läs hello artikeln toolearn [Microsofts molntjänster och nätverkssäkerhet](../best-practices-network-security.md).

## <a name="avoid-exposure-toohello-internet-with-dedicated-wan-links"></a>Undvika exponering toohello Internet med fast WAN-länkar
Många organisationer har valt hello Hybrid-IT vägen. I hybrid-IT är vissa hello företagets informationstillgångar i Azure, medan andra är lokalt. I många fall körs vissa komponenter av en tjänst i Azure medan andra komponenter är lokalt.

I hello hybrid IT-scenariot finns vanligtvis någon typ av korsanslutningar. Detta mellan en lokal anslutning kan hello företagets tooconnect sina lokala nätverk tooAzure virtuella nätverk. Det finns två anslutningar mellan lokala lösningar:

* Plats-till-plats VPN
* ExpressRoute

[Plats-till-plats VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) representerar en virtuell privat anslutning mellan ditt lokala nätverk och ett Azure Virtual Network. Den här anslutningen sker över hello Internet och kan du för ”tunnel” information i en krypterad anslutning mellan ditt nätverk och Azure. Plats-till-plats VPN är en säker, mogen teknik som har distribuerats av företag i alla storlekar för åren. Tunnel kryptering utförs med [IPSec-tunnelläge](https://technet.microsoft.com/library/cc786385.aspx).

Plats-till-plats VPN är en betrodd, tillförlitlig och etablerade teknik, trafik i hello tunnel passerar hello Internet. Dessutom är är relativt begränsad tooa maximalt om 200 Mbit/s.

Om du behöver en särskilda nivå av säkerhet och prestanda för anslutningar mellan platser, rekommenderar vi att du använder Azure ExpressRoute för dina korsanslutningar. ExpressRoute är en fast WAN länken mellan din lokala plats eller en värdleverantör för Exchange. Eftersom detta är en anger anslutning är data överföras inte via hello Internet och därför är inte synliga toohello potentiella risker vid Internet-kommunikation.

toolearn mer om hur Azure ExpressRoute fungerar och hur toodeploy, Läs hello artikeln [teknisk översikt för ExpressRoute](../expressroute/expressroute-introduction.md).

## <a name="optimize-uptime-and-performance"></a>Optimera drifttid och prestanda
Sekretess, integritet och tillgänglighet (CIA) utgör hello triad av dagens mest inflytelserik säkerhetsmodell. Sekretess är om kryptering och sekretess, integritet om att se till att data inte ändras av obehöriga personer och tillgänglighet om att se till att behöriga individer är kan tooaccess hello information de har behörighet tooaccess. Det gick inte att utföra någon av dessa områden representerar ett potentiellt angrepp i säkerhet.

Tillgängligheten kan betraktas som om drifttid och prestanda. Om en tjänst är nere kan kan inte information nås. Om prestanda är så låg som toomake hello data inte kan användas, vi kan bör du hello data toobe tillgänglig. Från ett säkerhetsperspektiv vi måste därför toodo oavsett vi kan toomake till våra tjänster har optimala drifttid och prestanda.
En populär och effektiv metod används tooenhance tillgänglighet och prestanda är toouse för belastningsutjämning. Belastningsutjämning är en metod för att distribuera nätverkstrafik på servrar som är en del av en tjänst. Om du har frontend-webbservrar som en del av din tjänst kan du till exempel använda belastningsutjämning toodistribute hello trafik mellan din flera frontend-webbservrar.

Den här distributionen av trafik ökar tillgängligheten eftersom om någon av hello webbservrar blir otillgänglig, hello belastningsutjämnaren stoppa skickar trafik toothat server och omdirigera trafik toohello servrar som fortfarande är online. Belastningsutjämning hjälper också till prestanda, eftersom hello processor, nätverk och minne för begäranden distribueras över alla hello belastningsutjämnade servrar.

Vi rekommenderar att du använder att belastningsutjämning när du kan och som passar dina tjänster. Vi ska hantera lämpligheten i hello följande avsnitt.
På hello Azure Virtual Network-nivå ger Azure du med tre huvudsakliga läsa in belastningsutjämning alternativ:

* HTTP-baserade belastningsutjämning
* Extern belastningsutjämning
* Intern belastningsutjämning

## <a name="http-based-load-balancing"></a>HTTP-baserade belastningsutjämning
HTTP-baserade belastningsbalanseringen baserar beslut om vilken toosend anslutningar med hjälp av egenskaperna hos hello HTTP-protokollet. Azure har en HTTP-belastningsutjämnare som går av Programgateway hello namn.

Vi rekommenderar att du oss Azure Programgateway när:

* Program som kräver begäranden från hello samma användare eller-klienten session tooreach hello samma backend-virtuella dator. Exempel på detta skulle shopping kundvagn appar och e-webbservrar.
* Program som vill toofree server webbservergrupper från SSL-avslutning omkostnader genom att utnyttja Application Gateway [SSL-avlastning](https://f5.com/glossary/ssl-offloading) funktion.
* Program, till exempel ett nätverk för innehållsleverans som kräver flera HTTP-förfrågningar på hello samma tidskrävande TCP-anslutning toobe dirigerade eller läsa in belastningsutjämnade toodifferent backend-servrar.

toolearn mer information om hur Azure Application Gateway fungerar och hur du kan använda den i dina distributioner Läs hello artikeln [Gateway Programöversikt](../application-gateway/application-gateway-introduction.md).

## <a name="external-load-balancing"></a>Extern belastningsutjämning
Extern belastningsutjämning sker när inkommande anslutningar från hello Internet belastningsutjämnas mellan servrarna finns i ett Azure Virtual Network. hello Azure extern belastningsutjämnare kan ge dig den här funktionen och vi rekommenderar att du använder den när du inte behöver hello Fäst sessioner eller SSL-avlastning.

TooHTTP-baserade belastningsutjämning, hello externa belastningsutjämnare använder däremot information på hello nätverks- och lager hello OSI nätverk modellen toomake beslut om vilken tooload saldo anslutning till.

Vi rekommenderar att du använder att externa belastningsutjämning när du har [tillståndslösa program](http://whatis.techtarget.com/definition/stateless-app) accepterar inkommande begäranden från hello Internet.

toolearn mer information om hur hello Azure externa belastningsutjämnaren fungerar och hur du kan distribuera, Läs hello artikeln [komma igång med en Internet Facing belastningsutjämnare i Resource Manager med hjälp av PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="internal-load-balancing"></a>Intern belastningsutjämning
Intern belastningsutjämning är liknande tooexternal belastningsutjämning och använder hello samma mekanism tooload saldo anslutningar toohello servrar bakom dem. hello endast skillnaden är att hello belastningsutjämnare i det här fallet accepterar anslutningar från virtuella datorer som inte är på hello Internet. I de flesta fall initieras hello-anslutningar som accepteras för belastningsutjämning av enheter i ett Azure Virtual Network.

Vi rekommenderar att du använder intern belastningsutjämning för scenarier som drar nytta av den här funktionen, till exempel när du behöver tooload saldo anslutningar tooSQL servrar eller interna webbservrar.

toolearn mer information om hur Azure intern belastningsutjämning fungerar och hur du kan distribuera, Läs hello artikeln [komma igång med en intern belastningsutjämnare använder PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md#update-an-existing-load-balancer).

## <a name="use-global-load-balancing"></a>Använd globala belastningsutjämning
Offentliga cloud computing-lösningar gör det möjligt toodeploy globalt distribuerade program där komponenter som finns i datacenter hello världen. Detta är möjligt i Microsoft Azure på grund av Tooazure's globala förekomst. Däremot nämnts toohello tekniker för belastningsutjämning tidigare globala belastningsutjämning gör möjliga toomake tjänster till den tillgängliga även om hela Datacenter kan bli otillgänglig.

Du kan hämta den här typen av global belastningsutjämning i Azure genom att utnyttja [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/). Traffic Manager gör är möjliga tooload saldo anslutningar tooyour tjänster baserat på hello plats hello användare.

Om hello användaren gör en begäran tooyour tjänst från hello Europa är hello anslutningen dirigerad tooyour tjänster finns i ett EU-datacenter. Den här delen av Traffic Manager globala belastningsutjämning hjälper tooimprove prestanda är eftersom ansluter toohello närmsta datacenter snabbare än att ansluta toodatacenters som är långt.

På sidan för hello tillgänglighet säkerställer globala belastningsutjämning att tjänsten är tillgänglig även om ett helt datacenter ska bli tillgänglig.

Om exempelvis ett Azure-datacenter ska bli tillgänglig på grund av tooenvironmental skäl eller förfallodatum toooutages (till exempel regionala nätverksfel), omdirigeras anslutningar tooyour tjänsten skulle vara toohello närmsta online datacenter. Det här globala belastningsutjämning åstadkoms genom att dra nytta av DNS-principer som du kan skapa i Traffic Manager.

Vi rekommenderar att du använder Traffic Manager för alla moln lösningar du utvecklar som har spridda omfattning över flera regioner och kräver hello högsta möjliga drifttid.

Mer om Azure Traffic Manager toolearn och hur toodeploy, Läs hello artikeln [vad är Traffic Manager](../traffic-manager/traffic-manager-overview.md).

## <a name="disable-rdpssh-access-tooazure-virtual-machines"></a>Inaktivera RDP/SSH-åtkomst tooAzure virtuella datorer
Det är möjligt tooreach Azure virtuella datorer med hjälp av hello [Remote Desktop Protocol](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) (RDP) och hello [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) (SSH)-protokoll. Dessa protokoll gör det möjligt toomanage virtuella datorer från fjärranslutna platser och är standard i datacenter datoranvändning.

hello potentiella säkerhetsproblem med hjälp av dessa protokoll via hello Internet är att angripare kan använda olika [brute force](https://en.wikipedia.org/wiki/Brute-force_attack) tekniker toogain åtkomst tooAzure virtuella datorer. När hello angripare får tillgång, de använda den virtuella datorn som en startpunkt för att kompromissa med andra datorer i ditt virtuella Azure-nätverk eller även angrepp nätverksenheter utanför Azure.

Därför rekommenderar vi att du inaktiverar direkt RDP och SSH åtkomst tooyour Azure virtuella datorer från hello Internet. När direkt RDP och SSH åtkomsten från hello Internet är inaktiverad, har du andra alternativ som du kan använda tooaccess dessa virtuella datorer för hantering av fjärråtkomst:

* Punkt-till-plats VPN
* Plats-till-plats VPN
* ExpressRoute

[Punkt-till-plats VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) är en annan term för en VPN-klient/server-anslutning för fjärråtkomst. En punkt-till-plats-VPN kan en enskild användare tooconnect tooan Azure Virtual Network över hello Internet. När hello punkt-till-plats-anslutning har upprättats, hello användaren kommer att kunna toouse RDP eller SSH tooconnect tooany virtuella datorer på hello Azure Virtual Network som hello användaren anslutna toovia punkt-till-plats VPN. Detta förutsätter att hello användaren är auktoriserad tooreach de virtuella datorerna.

Punkt-till-plats VPN är säkrare än direkta RDP eller SSH-anslutningar eftersom hello användare har tooauthenticate två gånger innan du ansluter tooa virtuella datorn. Först hello användaren måste tooauthenticate (och auktoriseras) tooestablish hello punkt-till-plats VPN-anslutning. andra hello användaren måste tooauthenticate (och auktoriseras) tooestablish hello RDP eller SSH-session.

En [plats-till-plats VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) ansluter ett hela nätverket tooanother nätverk via hello Internet. Du kan använda en plats-till-plats VPN-tooconnect ditt lokala nätverk tooan Azure Virtual Network. Om du distribuerar en plats-till-plats-VPN kan användare i ditt lokala nätverk kommer att kunna tooconnect toovirtual datorer i ditt virtuella Azure-nätverk med hjälp av hello RDP eller SSH-protokollet över hello plats-till-plats VPN-anslutning och inte kräver att du tooallow direkt via RDP eller SSH komma åt via hello Internet.

Du kan också använda en fast WAN-länk tooprovide funktioner liknande toohello plats-till-plats-VPN. hello viktigaste skillnaderna är 1. hello dedikerad WAN-länken passerar inte hello Internet och 2. fast WAN-länkar är vanligtvis mer stabilt och performant. Azure tillhandahåller en dedikerad WAN-länk-lösning i hello form av [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/).

## <a name="enable-azure-security-center"></a>Aktivera Azure Security Center
Azure Security Center hjälper dig att förebygga, upptäcka och åtgärda toothreats och ger du ökad insyn i, och kontroll över, hello säkerheten för dina Azure-resurser. Härifrån kan du övervaka och hantera principer för alla Azureprenumerationer på en gång och upptäcka hot som annars kanske skulle förbli oupptäckta. Azure Security Center fungerar tillsammans med ett vittomfattande ekosystem med säkerhetslösningar.

Azure Security Center hjälper dig att optimera och övervaka nätverkssäkerhet av:

* Att tillhandahålla säkerhetsrekommendationer för nätverk
* Övervaka hello tillståndet för nätverkskonfigurationen för säkerhet
* Varna dig toonetwork baserade hot både på hello slutpunkt och nätverk

Vi rekommenderar starkt att du aktiverar Azure Security Center för alla dina Azure-distributioner.

Mer om Azure Security Center och hur tooenable den för din distribution, Läs hello artikeln toolearn [introduktion tooAzure Security Center](../security-center/security-center-intro.md).

## <a name="securely-extend-your-datacenter-into-azure"></a>Utöka ditt datacenter på ett säkert sätt till Azure
Många företagets IT organisationer söker tooexpand i hello moln i stället för växande sina lokala datacenter. Detta representerar en förlängning av befintliga IT-infrastruktur i hello offentliga moln. Genom att utnyttja mellan lokala anslutningsalternativ som det är möjligt tootreat nätverksinfrastruktur dina virtuella Azure-nätverk som bara ett annat undernät på din lokala.

Men det är mycket planering och design problem som måste toobe beskrivs först. Detta är särskilt viktigt i hello område för nätverkssäkerhet. En hello bästa sätt toounderstand sättet du använder en sådan design är toosee ett exempel.

Microsoft har utvecklat hello [Datacenter tillägget referens Arkitekturdiagram](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84#content) och stöd för material från toohelp du förstår hur dessa filnamnstillägget datacenter skulle se ut. Detta ger ett exempel på referensimplementering som du kan använda tooplan och utforma en säker enterprise datacenter tillägget toohello moln. Vi rekommenderar att du läser det här dokumentet tooget en uppfattning om hello viktiga komponenter av en säker lösning.

toolearn mer information om hur toosecurely utökar ditt datacenter till Azure, visa hello video [Utöka ditt Datacenter tooMicrosoft Azure](https://www.youtube.com/watch?v=Th1oQQCb2KA).
