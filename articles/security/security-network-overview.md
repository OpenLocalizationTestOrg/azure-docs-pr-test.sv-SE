---
title: "aaaNetwork säkerhetsbegrepp & krav i Azure | Microsoft Docs"
description: " Den här artikeln gör det lätt för du toounderstand vad Microsoft Azure har toooffer under hello i nätverkssäkerhet. Vi ger grundläggande information om grundläggande koncept för säkerhet och krav och information om vilka Azure har toooffer i dessa olika områden. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TomSh
ms.assetid: bedf411a-0781-47b9-9742-d524cf3dbfc1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: terrylan
ms.openlocfilehash: 87d336064b880ddcf90ae4fcb79b7823367682b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-network-security-overview"></a>Översikt över säkerheten i Azure-nätverk
Microsoft Azure innehåller en robust nätverk infrastruktur toosupport programmet och service anslutningskrav. Nätverksanslutningen är möjlig mellan resurser i Azure, mellan lokala och Azure värdbaserade resurser, och tooand från hello Internet och Azure.

hello syftet med den här artikeln är toomake underlättar för du toounderstand vad Microsoft Azure har toooffer under hello i nätverkssäkerhet. Här kan vi ge grundläggande information om grundläggande koncept för säkerhet och krav. Vi tillhandahåller även information om vilka Azure har toooffer i dessa olika områden samt länkar toohelp du få en bättre förståelse av intressanta områden.

Översikt över Azure Network Security artikeln fokuserar på hello följande områden:

* Azure-nätverk
* Åtkomstkontrollen för nätverk
* Säker åtkomst och mellan platser för fjärranslutningar
* Tillgänglighet
* Namnmatchning
* DMZ-arkitektur
* Övervakning och hotidentifiering


## <a name="azure-networking"></a>Azure-nätverk
Virtuella datorer måste nätverksanslutningen. toosupport kräver detta krav, Azure virtuella datorer toobe anslutna tooan Azure Virtual Network. Ett virtuellt Azure-nätverk är en logisk konstruktion som bygger på hello fysisk Azure nätverksinfrastruktur. Varje logiskt virtuella Azure-nätverk är isolerat från alla andra virtuella Azure-nätverk. På så sätt kan du försäkra dig om att nätverkstrafik i din distribution inte är tillgänglig tooother Microsoft Azure-kunder.

Läs mer:

* [Översikt över virtuella nätverk](../virtual-network/virtual-networks-overview.md)


## <a name="network-access-control"></a>Åtkomstkontrollen för nätverk
Åtkomstkontrollen för nätverk är hello handling för att begränsa anslutningen tooand från specifika enheter och undernät i ett Azure Virtual Network. hello målet av åtkomstkontrollen för nätverk är toolimit åtkomst tooyour virtuella datorer och tjänster tooapproved användare och enheter. Åtkomstkontroller baseras på Tillåt eller neka beslut för anslutningar tooand från en virtuell dator eller tjänst.

Azure har stöd för flera typer av åtkomstkontrollen för nätverket som:

* Kontrollen av nätverkets lager
* Dirigera kontroll och Tvingad tunneltrafik
* Säkerhetsenheter för virtuellt nätverk

### <a name="network-layer-control"></a>Kontrollen av nätverkets lager
En säker distribution kräver vissa mått av åtkomstkontrollen för nätverket. hello målet av åtkomstkontrollen för nätverk är toorestrict virtuella toohello nödvändiga kommunikationssystem och att andra kommunikationsförsök blockeras.

Om du behöver grundläggande nivån nätverksåtkomstkontroll (baserat på IP-adress och hello TCP eller UDP-protokoll), kan du använda Nätverkssäkerhetsgrupper. En Nätverkssäkerhetsgrupp (NSG) är ett grundläggande tillståndskänslig paket filtrering brandvägg och det gör att du toocontrol åtkomst baserat på en [5-tuppel](https://www.techopedia.com/definition/28190/5-tuple). NSG: er ger inte programmet layer inspektion eller autentiserad åtkomstkontroller.

Läs mer:

* [Nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md)

### <a name="route-control-and-forced-tunneling"></a>Dirigera kontroll och Tvingad tunneltrafik
hello är möjlighet toocontrol routning på dina virtuella Azure-nätverk en kritisk säkerhets- och kontroll nätverkskapacitet. Om routning är felaktigt konfigurerad, kan program och tjänster som finns på den virtuella datorn ansluta toounauthorized enheter, inklusive system ägs och drivs av potentiella angripare.

Azure nätverk stöder hello möjlighet toocustomize hello dirigeringsbeteendet för nätverkstrafik på dina virtuella Azure-nätverk. Detta gör att du tooalter hello standardposter i routningstabellen i ditt virtuella Azure-nätverk. Kontroll av dirigeringsbeteendet hjälper dig att se till att all trafik från en viss enhet eller grupp av enheter anländer till eller lämnar det virtuella nätverket via en viss plats.

Till exempel kanske en virtuell nätverksenhet för säkerhet på ditt Azure-nätverk. Vill du att all trafik tooand från det virtuella Azure-nätverket passerar den virtuella postsäkerhet toomake. Du kan göra detta genom att konfigurera [användardefinierade vägar](../virtual-network/virtual-networks-udr-overview.md) i Azure.

[Tvingad tunneltrafik](https://www.petri.com/azure-forced-tunneling) är en mekanism som du kan använda tooensure som dina tjänster inte är tillåtna tooinitiate en anslutning toodevices på hello Internet. Observera att detta skiljer sig från att acceptera inkommande anslutningar och sedan svarar toothem. Frontend-webbservrar måste toorespond toorequests från Internet-värdar och så källkod för Internet-trafik tillåts inkommande toothese webbservrar och hello webbservrar tillåts toorespond.

Vad du inte vill att tooallow är en frontend-webbservrar server tooinitiate en utgående begäran. Sådana begäranden kan utgöra en säkerhetsrisk eftersom dessa anslutningar kunde använda toodownload skadlig kod. Även om du vill att dessa frontend servrar tooinitiate utgående begär toohello Internet kan du vilja ha tooforce dem toogo via din lokala web proxyservrar så att du kan dra nytta av URL-filtrering och loggning.

Du kan i stället toouse Tvingad tunneltrafik tooprevent. När du aktiverar Tvingad tunneling tvingas alla anslutningar toohello Internet via ditt lokala gateway. Du kan konfigurera Tvingad tunneling genom att utnyttja användardefinierade vägar.

Läs mer:

* [Vad är användardefinierade vägar och IP-vidarebefordring](../virtual-network/virtual-networks-udr-overview.md)

### <a name="virtual-network-security-appliances"></a>Säkerhetsenheter för virtuellt nätverk
Medan Nätverkssäkerhetsgrupper, användardefinierade vägar och Tvingad tunneling ger en hög säkerhetsnivå på hello nätverks- och lager i hello [OSI-modell](https://en.wikipedia.org/wiki/OSI_model), det kan finnas tillfällen när du vill tooenable säkerhet på högre nivåer än hello nätverk.

Till exempel kan ditt säkerhetskrav innehålla:

* Autentisering och auktorisering innan åtkomst tooyour program
* Identifiering av adressintrång och intrångsidentifiering svar
* Programmet layer-kontroll för övergripande protokoll
* URL-filtrering
* Nätverket nivån antivirus eller antimalware
* Skydd mot bot
* Åtkomstkontroll för programmet
* Ytterligare DDoS-skydd (ovanför hello DDoS-skydd som ges hello Azure fabric själva)

Du kan komma åt dessa förbättrade funktioner för nätverkssäkerhet med hjälp av en Azure partnerlösning. Du kan hitta hello senaste Azure network security partnerlösningar genom att besöka hello [Azure Marketplace](https://azure.microsoft.com/marketplace/) och söka efter ”säkerhet” och ”nätverkssäkerhet”.

## <a name="secure-remote-access-and-cross-premises-connectivity"></a>Säker fjärråtkomst och plats-anslutning
Installation, konfiguration och hantering av resurserna i Azure måste du toobe ske på distans. Dessutom kan du behöva toodeploy [hybrid IT](http://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) lösningar som har komponenter på lokalt och i hello offentliga Azure-molnet. Dessa scenarier kräver säker fjärråtkomst.

Azure-nätverk har stöd för följande scenarier för säker fjärråtkomst hello:

* Ansluta enskilda arbetsstationer tooan Azure Virtual Network
* Ansluta din lokala nätverk tooan Azure-nätverk med en VPN-anslutning
* Ansluta din lokala nätverk tooan Azure-nätverk med en fast WAN-länk
* Anslut Azure Virtual Networks tooeach andra

### <a name="connect-individual-workstations-tooan-azure-virtual-network"></a>Ansluta enskilda arbetsstationer tooan Azure Virtual Network
Det kan finnas tillfällen när du vill tooenable enskilda utvecklare eller åtgärder personal toomanage virtuella datorer och tjänster i Azure. Till exempel måste du komma åt tooa virtuell dator på ett virtuellt Azure-nätverk och din säkerhetsprincip tillåter inte fjärråtkomst via RDP eller SSH tooindividual virtuella datorer. I det här fallet kan du använda en punkt-till-plats VPN-anslutning.

Hej VPN-anslutning använder hello punkt-till-plats [SSTP VPN](https://technet.microsoft.com/library/cc731352.aspx) protokollet tooenable tooset en privat och säker anslutning mellan hello användare och hello Azure Virtual Network. När hello VPN-anslutningen har upprättats hello användaren kommer att kunna tooRDP eller SSH över hello VPN länka till en virtuell dator på hello Azure Virtual Network (förutsatt att hello användaren kan autentiseras och auktoriseras).

Läs mer:

* [Konfigurera en punkt-till-plats-anslutning tooa virtuella nätverk med hjälp av PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="connect-your-on-premises-network-tooan-azure-virtual-network-with-a-vpn"></a>Ansluta din lokala nätverk tooan Azure-nätverk med en VPN-anslutning
Du kanske vill tooconnect hela företagsnätverket, eller delar av det, tooan Azure Virtual Network. Detta är vanligt i hybrid IT scenarier där företag [utöka sina lokala datacenter till Azure](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84). I många fall företag ska vara värd för delar av en tjänst i Azure och delar lokalt, till exempel när en lösning innehåller frontend-webbservrar i Azure och backend-databaser på lokalt. Dessa typer av ”anslutningar mellan platser” Se också hantering av Azure finns resurser mer säker och aktivera scenarier, till exempel utöka Active Directory-domänkontrollanter i Azure.

Enkelriktade tooaccomplish toouse är en [plats-till-plats VPN](https://www.techopedia.com/definition/30747/site-to-site-vpn). hello skillnaden mellan en plats-till-plats-VPN och en punkt-till-plats-VPN är att en punkt-till-plats-VPN ansluter en enskild enhet tooan Azure Virtual Network, medan en plats-till-plats-VPN ansluter en tooan Azure Virtual Network för hela nätverket (till exempel ditt lokala nätverk) . Plats-till-plats VPN tooan Azure Virtual Network använder hello mycket säkert IPSec-tunneln läge VPN-protokollet.

Läs mer:

* [Skapa ett VNet Resource Manager med en plats-till-plats VPN-anslutning med hjälp av hello Azure-portalen](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [Planering och design för VPN-gateway](../vpn-gateway/vpn-gateway-plan-design.md)

### <a name="connect-your-on-premises-network-tooan-azure-virtual-network-with-a-dedicated-wan-link"></a>Ansluta din lokala nätverk tooan Azure Virtual Network med en dedikerad WAN-länk
Punkt-till-plats och plats-till-plats VPN-anslutningar är effektiv för att aktivera korsanslutningar. Vissa organisationer överväga dem toohave hello följande nackdelar:

* VPN-anslutningar flytta data över hello Internet – detta visar dessa anslutningar toopotential säkerhetsproblem med flytta data över ett offentligt nätverk. Dessutom kan tillförlitlighet och tillgänglighet för Internet-anslutningar inte garanteras.
* VPN-anslutningar tooAzure virtuella nätverk kan anses bandbredden begränsad för vissa program och syften som de högsta ut vid runt 200 Mbit/s.

Organisationer som behöver hello högsta nivå av säkerhet och tillgänglighet för sina anslutningar mellan platser normalt använda dedicerade WAN-länkar tooconnect tooremote platser. Azure tillhandahåller du hello möjlighet toouse dedikerad WAN-länk som du kan använda tooconnect ditt lokala nätverk tooan Azure Virtual Network. Detta aktiveras via Azure ExpressRoute.

Läs mer:

* [Teknisk översikt för ExpressRoute](../expressroute/expressroute-introduction.md)

### <a name="connect-azure-virtual-networks-tooeach-other"></a>Anslut Azure Virtual Networks tooEach andra
Det är möjligt att du toouse flera virtuella Azure-nätverk för din distribution. Det finns många orsaker till varför du kan göra detta. Ett hello orsaker kan vara toosimplify förvaltning. en annan kanske av säkerhetsskäl. Oavsett hello syfte eller motiveringen för att publicera resurser på olika virtuella Azure-nätverk, kan det finnas tillfällen när du vill att resurser på varje hello nätverk tooconnect med varandra.

Ett alternativ kan vara för tjänsterna på ett Azure Virtual Network tooconnect tooservices på en annan Azure-nätverk genom att ”loopa tillbaka” via hello Internet. hello anslutning skulle starta i ett Azure Virtual Network, gå igenom hello Internet och sedan gå tillbaka toohello mål Azure Virtual Network. Det här alternativet visar hello anslutning toohello säkerhet problem med inbyggd tooany Internet-baserad kommunikation.

Ett bättre alternativ kanske toocreate ett Azure Virtual Network-Azure-virtuella nätverk plats-till-plats VPN. Den här Azure-virtuellt nätverk till Azure virtuellt nätverk som använder plats-till-plats VPN hello samma [IPSec-tunnelläge](https://technet.microsoft.com/library/cc786385.aspx) protokoll som nämns ovan hello mellan lokala plats-till-plats VPN-anslutning.

hello fördelen med att använda en Azure virtuella nätverk till Azure virtuella nätverk plats-till-plats VPN är att hello VPN-anslutningen har upprättats över hello Azure nätverksinfrastruktur i stället för att ansluta via hello Internet. Det här ger du en extra nivå av säkerhet jämfört med toosite-till-plats VPN som ansluter via hello Internet.

Läs mer:

* [Konfigurera en VNet-till-VNet-anslutning med hjälp av Azure Resource Manager och PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="availability"></a>Tillgänglighet
Tillgänglighet är en viktig del av security-programmet. Om dina användare och datorer har inte åtkomst till vilka de behöver tooaccess över hello nätverk, hello service kan ses komprometteras. Azure har nätverkstekniker som stöd hello följande mekanismer för hög tillgänglighet:

* HTTP-baserade belastningsutjämning
* Nivån för belastningsutjämning
* Globala belastningsutjämning

Belastningsutjämning är en mekanism avsedd tooequally distribuera anslutningar mellan flera enheter. hello målen med belastningsutjämning är:

* Öka tillgänglighet – när du läser in saldo anslutningar på flera enheter, en eller flera av hello enheter kan bli tillgänglig och hello-tjänster som körs på hello återstående online enheter kan fortsätta tooserve hello innehåll från hello-tjänsten
* Öka prestanda – när du läser in saldo anslutningar på flera enheter en enskild enhet inte har tootake hello processor träffar. I stället hello och minnesresurser krav betjänar hello innehåll sprids över flera enheter.

### <a name="http-based-load-balancing"></a>HTTP-baserade belastningsutjämning
Organisationer som att köra webbaserade tjänster ofta önskar toohave en HTTP-baserad belastningsutjämnare framför dessa web services toohelp säkerställa tillräcklig nivåer av prestanda och hög tillgänglighet. Däremot tootraditional nätverksbaserade belastningsutjämnare, hello belastningen belastningsutjämning beslut HTTP-baserad belastningsutjämnare baseras på egenskaperna hos hello HTTP-protokollet inte på hello nätverks- och transport layer-protokoll.

tooprovide du HTTP-baserade belastningsutjämning för dina webbaserade tjänster Azure tillhandahåller du hello Azure Application Gateway. Azure Application Gateway har stöd för hello:

* HTTP-baserade belastningsutjämning – belastningen belastningsutjämning beslut fattas baserat på karakteristiken särskilda toohello HTTP-protokoll
* Cookie-baserad session tillhörighet – den här funktionen ser till att upprätta anslutningar tooone hello servrar bakom som belastningsutjämnare intakt mellan hello klienten och servern. Detta garanterar stabiliteten för transaktioner.
* SSL-avlastning – när en klientanslutning har upprättats med hello belastningsutjämnare session mellan hello klient och hello belastningsutjämnaren krypteras med hjälp av hello HTTPS (SSL /) protokollet. Du kan dock ha hello alternativet toohave hello anslutning mellan hello belastningsutjämnare och hello webbserver bakom hello Använd hello HTTP (okrypterat) belastningsutjämningsprotokollet i ordning tooincrease prestanda. Det beror hänvisade tooas ”SSL avlastning” hello webbservrar bakom belastningsutjämnaren hello inte drabbas av hello processor arbetet med kryptering och bör därför kan tooservice begäranden snabbt.
* URL-baserade innehåll routing – den här funktionen gör det möjligt för hello belastningen belastningsutjämnaren toomake beslut om var tooforward anslutningar baserat på hello mål-URL. Detta ger mycket större flexibilitet än lösningar som gör att läsa in belastningsutjämning beslut baserat på IP-adresser.

Läs mer:

* [Översikt över Gateway](../application-gateway/application-gateway-introduction.md)

### <a name="network-level-load-balancing"></a>Nivån för belastningsutjämning
TooHTTP-baserade belastningsutjämning, nivå belastningsutjämning gör däremot läsa in belastningsutjämning beslut baserat på IP-adressen och porten (TCP eller UDP) nummer.
Du kan få hello fördelarna med nivå belastningsutjämning i Azure med hjälp av hello Azure belastningsutjämnare. Vissa kännetecknar hello Azure belastningsutjämnare är:

* Belastningsutjämning i nivån baserat på IP-adress och port nummer
* Stöd för alla program layer protocol
* TooAzure virtuella datorer för belastningsutjämning och cloud services rollinstanser
* Kan användas för både Internet-riktade (extern belastningsutjämning) och icke-Internet facing (intern belastningsutjämning) program och virtuella datorer
* Slutpunkten som övervakning, vilket är används toodetermine om någon av hello tjänster bakom hello belastningsutjämnare har blivit tillgänglig

Läs mer:

* [Belastningsutjämnare för Internet mellan flera virtuella datorer eller tjänster](../load-balancer/load-balancer-internet-overview.md)
* [Översikt över interna belastningsutjämnare](../load-balancer/load-balancer-internal-overview.md)

### <a name="global-load-balancing"></a>Globala belastningsutjämning
Vissa organisationer vill hello högsta nivå för tillgänglighet som möjligt. Enkelriktade tooreach det här målet är toohost program i globalt distribuerade datacenter. När ett program finns i datacenter finns i hela hello world, är det möjligt för en hel geopolitiska region toobecome tillgänglig och fortfarande har programmet hello och körs.

Dessutom toohello tillgänglighet fördelarna du får genom att lägga upp program i globalt distribuerade datacenter, också kan du få prestandafördelarna. Dessa prestandafördelar kan erhållas med hjälp av en mekanism som begäranden om hello service toohello datacenter som är närmast toohello enhet som gjort hello-begäran.

Globala belastningsutjämning kan ge dig båda av dessa fördelar. I Azure, kan du få hello fördelarna med globala belastningsutjämning med hjälp av Azure Traffic Manager.

Läs mer:

* [Vad är Traffic Manager?](../traffic-manager/traffic-manager-overview.md)


## <a name="name-resolution"></a>Namnmatchning
Namnmatchning är en viktig funktion för alla tjänster som du har i Azure. Från ett säkerhetsperspektiv leda röjande av hello name resolution funktionen tooan angripare omdirigerar begäranden från platser tooan angriparens webbplats. Säker namnmatchning är ett krav för alla molnbaserade tjänster.

Det finns två typer av namnmatchning som du behöver tooaddress:

* Intern namnmatchning – intern namnmatchning används av tjänsterna i ditt virtuella Azure-nätverk, ditt lokala nätverk eller båda. Namn som används för intern namnmatchning för är inte tillgängliga via hello Internet. För optimal säkerhet är det viktigt att din lösning interna namn-schemat inte är tillgänglig tooexternal användare.
* Externa namnmatchning – externa namnmatchning används av användare och enheter utanför din lokala och virtuella Azure-nätverk. Dessa är hello-namn som är synliga toohello Internet, används toodirect anslutning tooyour molnbaserade tjänster.

För intern namnmatchning har du två alternativ:

* Ett Azure Virtual Network DNS-server – när du skapar en ny Azure Virtual Network, en DNS-server skapas åt dig. DNS-servern kan matcha hello namnen på hello datorer på det virtuella Azure-nätverket. Den här DNS-servern kan inte konfigureras och hanteras av hello Azure fabric manager, vilket gör det en säker name resolution lösning.
* Ta med din egen DNS-server – har hello möjlighet att placera en DNS-server om du väljer själv i ditt virtuella Azure-nätverk. Den här DNS-server kan vara en Active Directory-integrerade DNS-server eller en dedikerad DNS-server-lösning som tillhandahålls av en Azure-partner som du kan hämta från hello Azure Marketplace.

Läs mer:

* [Översikt över virtuella nätverk](../virtual-network/virtual-networks-overview.md)
* [Hantera DNS-servrar som används av ett virtuellt nätverk (VNet)](../virtual-network/virtual-network-manage-network.md#dns-servers)

För externa DNS-matchning har du två alternativ:

* Värd för dina egna externa DNS-server lokalt
* Värd för externa DNS-servern med en tjänstleverantör

Många stora företag ska vara värd för sina egna DNS-servrar lokalt. De kan göra detta eftersom de har hello nätverks-kunskaper och global närvaro toodo så.

I de flesta fall är det bättre toohost DNS-namnmatchningen tjänster med en tjänstprovider. Dessa leverantörer har hello nätverket kunskaper och global närvaro tooensure mycket hög tillgänglighet för din namnmatchningstjänster. Tillgänglighet är viktigt för DNS services eftersom om din namnmatchningstjänster misslyckas, ingen kommer att kunna tooreach din Internetuppkopplad tjänster.

Azure tillhandahåller du hög tillgänglighet och performant externa DNS-lösning i hello form av Azure DNS. Externa name resolution lösningen utnyttjar hello över hela världen Azure DNS-infrastrukturen. Det gör att du toohost hello din domän i Azure med samma autentiseringsuppgifter, API: er, verktyg och fakturering som andra Azure-tjänster. Som en del av Azure ärver också starkt hello säkerhetsåtgärder som är inbyggda i hello-plattformen.

Läs mer:

* [Översikt över Azure DNS](../dns/dns-overview.md)

## <a name="dmz-architecture"></a>DMZ-arkitektur
Många företag använda DMZs toosegment sina nätverk toocreate en buffert zon mellan hello Internet och sina tjänster. hello DMZ del av nätverket på hello anses vara en zon med låg säkerhet och inga värdefulla tillgångar placeras i det nätverkssegmentet. Du ser vanligtvis säkerhet nätverksenheter som har ett nätverksgränssnitt på hello DMZ segmentet och ett annat nätverk gränssnittet anslutna tooa nätverk som har virtuella datorer och tjänster som accepterar inkommande anslutningar från hello Internet.

Det finns ett antal variationer av DMZ design och hello beslut toodeploy en DMZ och sedan på vilken typ av DMZ toouse om du väljer toouse, baseras på ditt nätverkssäkerhetskrav.

Läs mer:

* [Microsoft-molntjänster och nätverkssäkerhet](../best-practices-network-security.md)


## <a name="monitoring-and-threat-detection"></a>Övervakning och hotidentifiering

Azure tillhandahåller funktioner toohelp du i det här viktiga området med tidig identifiering, övervakning och hello möjlighet toocollect och granska nätverkstrafik.

### <a name="azure-network-watcher"></a>Azure Nätverksbevakaren
Azure Nätverksbevakaren innehåller ett stort antal funktioner som hjälp med felsökning samt ge en helt ny uppsättning verktyg tooassist hello identifiering av säkerhetsproblem.

[Säkerhetsgrupp visa ](/network-watcher/network-watcher-security-group-view-overview.md) hjälper till med gransknings- och kompatibilitet för virtuella datorer och kan vara används tooperform programmässiga granskningar jämföra hello baslinjer principer som definierats av din organisation tooeffective regler för var och en av dina virtuella datorer. Detta kan hjälpa dig att identifiera eventuella konfigurationsavvikelser.

[Paketinsamling](/network-watcher/network-watcher-packet-capture-overview.md) kan du toocapture nätverk trafik tooand från hello virtuell dator. Förutom att hjälpa genom att låta dig toocollect nätverksstatistik och hello felsökningen av programmet vara problem paketinsamling ovärderlig i hello undersökning av intrång i nätverket. Du kan också använda den här funktionen tillsammans med Azure Functions toostart nätverksinsamlingar i svaret toospecific Azure aviseringar.

Mer information om Azure Nätverksbevakaren och hur toostart testa vissa av hello funktionerna i ditt labb ta en titt på hello [Azure nätverksbevakaren översikt över övervakning](/network-watcher/network-watcher-monitoring-overview.md)

>[!NOTE]
Azure nätverksbevakaren är fortfarande förhandsversion så inte kan ha hello samma nivå av tillgänglighet och tillförlitlighet som tjänster som är i allmänhet tillgänglighet versionen. Vissa funktioner kanske inte stöds, kan ha begränsad kapacitet och kanske inte tillgänglig på alla platser i Azure. För hello senaste meddelanden på tillgänglighet och status för den här tjänsten, kontrollera hello [Azure uppdateringar](https://azure.microsoft.com/updates/?product=network-watcher)

### <a name="azure-security-center"></a>Azure Security Center
Security Center hjälper dig att förebygga, upptäcka och åtgärda toothreats och ger du ökad insyn i, och kontroll över, hello säkerheten för dina Azure-resurser. Det ger integrerad säkerhet övervaka och hantera principer för dina Azure-prenumerationer, och upptäcka hot som annars kanske skulle förbli oupptäckta, fungerar med en stor mängd lösningar för informationssäkerhet.

Azure Security Center hjälper dig att optimera och övervaka nätverkssäkerhet av:

* Att tillhandahålla säkerhetsrekommendationer för nätverk
* Övervaka hello tillståndet för nätverkskonfigurationen för säkerhet
* Varna dig toonetwork baserade hot både på hello slutpunkt och nätverk

Läs mer:

* [Introduktion tooAzure Security Center](../security-center/security-center-intro.md)


### <a name="logging"></a>Loggning
Loggning på en nivå är en funktion för alla scenariot säkerhet för. I Azure, kan du logga information som erhålls för Nätverkssäkerhetsgrupper tooget nätverksnivån loggar information. Med NSG loggning får du information från:

* [Aktivitetsloggar](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) – de här loggarna är används tooview alla åtgärder har skickats tooyour Azure prenumerationer. Dessa loggar är aktiverade som standard och kan användas i hello Azure-portalen. De tidigare kallas ”granskningsloggar” eller ”Arbetsloggarna”.
* Händelseloggar – de här loggarna ger information om vilka NSG-regler har tillämpats.
* Räknare loggar – dessa loggar kan du vet hur många gånger varje NSG regel har tillämpade toodeny eller Tillåt trafiken.

Du kan också använda [Microsoft Power BI](https://powerbi.microsoft.com/what-is-power-bi/), en kraftfull datavisualisering verktyget tooview och analysera dessa loggar.

Läs mer:

* [Log Analytics för Nätverkssäkerhetsgrupper (NSG: er)](../virtual-network/virtual-network-nsg-manage-log.md)
