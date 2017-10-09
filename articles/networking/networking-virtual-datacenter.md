---
title: aaaMicrosoft Azure virtuella Datacenter | Microsoft Docs
description: "Lär dig hur toobuild din virtuella datacenter i Azure"
services: networking
author: tracsman
manager: rossort
tags: azure-resource-manager
ms.service: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: jonor
ms.openlocfilehash: 84f77b16edaece202a6a94b6107f1c9585ec7f38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-virtual-data-center"></a>Virtuella Microsoft Azure-Datacenter
**Microsoft Azure**: flytta snabbare, spara pengar, integrera lokala appar och data

## <a name="overview"></a>Översikt
Migrera lokala program tooAzure ger även utan några betydande ändringar (en metod som kallas ”lyfta och flytta”), organisationer hello fördelarna med en säker och kostnadseffektiv infrastruktur. Dock toomake hello mest av hello smidighet möjligt med cloud computing-lösningar, företag bör utvecklas sina arkitekturer tootake nytta av funktioner i Azure. Microsoft Azure ger storskaliga tjänster och infrastruktur, funktioner i företagsklass och tillförlitlighet och många alternativ för hybridanslutning. Kunder kan välja tooaccess dessa cloud services antingen via hello Internet eller med Azure ExpressRoute som tillhandahåller anslutningar privata nätverk. hello Microsoft Azure-plattformen gör att kunder tooseamlessly utökar sin infrastruktur till molnet hello och bygga arkitektur för flera nivåer. Dessutom kan Microsoft-partner innehåller förbättrade funktioner genom att erbjuda tjänster och virtuella installationer som är optimerad toorun i Azure.

Den här artikeln innehåller en översikt över mönster och design som kan använda toosolve hello arkitektur skala, prestanda och säkerhet frågor som många kunder står inför när du tänker flytta masse toohello moln. En översikt över hur toofit olika organisationens IT-roller till hello hantering och styrning av hello system också diskuteras, med betoning toosecurity krav och kostnadsoptimering.

## <a name="what-is-a-virtual-data-center"></a>Vad är ett virtuella Datacenter?
I hello Internets första dagar molnet lösningar var utformad toohost enda, relativt isolerade program i hello offentliga spektrumet. Den här metoden fungerade bra för några år. Dock är hello fördelarna med molnet lösningar blev tydligt och flera stora arbetsbelastningar har finns i hello molnet adressering säkerhet, tillförlitlighet och prestanda och kostnad gäller för distributioner i en eller flera områden blev viktigt i hela hello livscykel för hello-Molntjänsten.

hello följande molnet distribution diagram visar några exempel på säkerhetsbrister (röd ruta) och utrymme för optimering virtuella nätverksenheter över arbetsbelastningar (gul ruta).

[![0]][0]

hello virtuella datacenter (vDC) föddes från detta är nödvändigt för att skala toosupport företagsarbetsbelastningar och hello måste toodeal hello problem som införs när stöder storskaliga program i hello offentligt moln.

En vDC är inte bara hello programbelastningar i hello moln, men även hello nätverk, säkerhet, hantering och infrastruktur (till exempel DNS- och Directory Services). Vanligtvis finns även en privat anslutning tillbaka tooan lokala nätverk eller data center. Fler och fler arbetsbelastningar flytta tooAzure, är det viktigt toothink om hello stöder infrastrukturen och objekt som placerats i dessa arbetsbelastningar. Tänka noggrant hur resurser är strukturerade undviker hello ökande mängden av hundratals ”arbetsbelastning i Oceanien och Västindien” som måste hanteras separat med oberoende dataflöde, säkerhetsmodeller och kompatibilitet utmaningar.

Ett virtuellt Datacenter är egentligen en samling av separat men relaterade entiteter med vanliga stödjande funktioner, funktioner och infrastruktur. Genom att visa dina arbetsbelastningar som en integrerad vDC, inser du minskade kostnader på grund av tooeconomies skala, optimerade säkerhet via komponenten och data som flödar centralisering tillsammans med enklare åtgärder, hantering och kompatibilitet granskningar.

> [!NOTE]
> Det är viktigt toounderstand som hello vDC är **inte** diskreta Azure produkten, men hello kombination av olika funktioner och möjligheter för uppfyller dina exakta krav. vDC är ett sätt att du tänker dina arbetsbelastningar och Azure användning toomaximize dina resurser och förmågor i hello molnet. hello virtuella domänkontrollanter är därför en modulär metod på hur toobuild in IT-tjänster i hello Azure, respektera organisationens roller och ansvarsområden.

hello vDC hjälper företag hämta arbetsbelastningar och program till Azure för hello följande scenarier:

-   Värd för flera relaterade arbetsbelastningar
-   Migrera arbetsbelastningar från en lokal miljö tooAzure
-   Implementerar delade eller centraliserad säkerhet och åtkomstkraven över arbetsbelastningar
-   Blandning av DevOps och centraliserad IT korrekt för ett stort företag

Hej viktiga toounlock hello fördelarna med vDC, är en centraliserad topologi (NAV och ekrar) med en blandning av Azure-funktioner: [Azure VNet][VNet], [NSG: er] [ NSG], [VNet-Peering][VNetPeering], [användardefinierade vägar (UDR)][UDR], och Azure identitet med [ Rollbaserad åtkomstkontroll (RBAC)][RBAC].

## <a name="who-needs-a-virtual-data-center"></a>Som behöver ett virtuella Datacenter?
Azure kunder som behöver toomove mer än ett par arbetsbelastningar till Azure kan utnyttja tänka använder gemensamma resurser. Beroende på hello omfattning, även i enskilda program kan dra nytta av hello mönster och användas toobuild en vDC.

Om din organisation har en central IT-avdelningen, nätverk, säkerhet och/eller/efterlevnadsavdelningen team, en vDC hjälper dig att tillämpa principen punkter, uppdelning av tull och enhetlig av hello underliggande gemensamma komponenter medan du ger programmet team så mycket frihet och kontroll som är lämpliga för dina behov.

Organisationer som behöver tooDevOps kan använda hello vDC begrepp tooprovide auktoriserade bara till Azure-resurser och se till att de har fullständig kontroll i gruppen (prenumerationen eller resursen grupp i en gemensam prenumeration), men hello nätverk och säkerhetsgränser förblir kompatibla som definieras av en central princip i en NAV VNet och resursgruppen.

## <a name="considerations-on-implementing-a-virtual-data-center"></a>Överväganden om hur du implementerar ett virtuella Datacenter
När du designar en vDC, finns det flera spela en central problem tooconsider:

-   Identitets- och katalogtjänster
-   Säkerhetsinfrastruktur
-   Anslutningen toohello moln
-   Anslutningen inom hello moln

##### <a name="identity-and-directory-service"></a>*Identitets- och katalogtjänst*
Identitets- och Directory services är en viktig aspekt av alla data resurser, både lokalt och i hello molnet. Identiteten är relaterade tooall aspekter av åtkomst och auktorisering tooservices inom hello vDC. toohelp se till att endast auktoriserade användare och processer åtkomst till ditt Azure-konto och resurser, Azure använder flera typer av autentiseringsuppgifter för autentisering. Dessa inkluderar lösenord (tooaccess hello Azure-konto), krypteringsnycklar, digitala signaturer och certifikat. [*Azure Multi-Factor Authentication* (MFA)] [ MFA] är ett extra lager av säkerhet för åtkomst till Azure-tjänster. Azure MFA ger stark autentisering med ett antal alternativ för enkel verifiering – telefonsamtal, textmeddelande eller mobilapp, och att kunderna toochoose hello metod.

Stora företag måste toodefine en process för identiteten som beskriver hello hanteringen av individuella identiteter, deras autentisering, auktorisering, roller och privilegier inom eller mellan hello vDC. hello målen för den här processen ska vara tooincrease säkerhet och produktivitet och minskade kostnader, driftstopp och repetitiva manuella uppgifter.

Företag eller organisationer kan kräva en krävande blandning av olika rad-för-företag (LOB)-tjänster och anställda som ofta har olika roller ni med olika projekt. En vDC kräver bra samarbete mellan olika team med vissa rolldefinitioner tooget system som kör med bra styrning. hello-matrisen med ansvar, åtkomst och rättigheter kan vara mycket komplex. Identity management i vDC implementeras via [ *Azure Active Directory* (AAD)] [ AAD] och rollbaserad åtkomstkontroll (RBAC).

En katalogtjänst är en delad informationsinfrastruktur för att hitta, hantera, administrera och ordna vanliga objekt och nätverksresurser. Dessa resurser kan innehålla volymer, mappar, filer, skrivare, användare, grupper, enheter och andra objekt. Varje resurs i nätverket hello betraktas som ett objekt av hello katalogservern. Information om en resurs lagras som en uppsättning attribut som är associerade med denna resurs eller objekt.

Alla Microsoft online företagstjänster förlitar sig på Azure Active Directory (AAD) för inloggning och andra identitet. Azure Active Directory är en omfattande molnlösning för identitets- och åtkomsthantering med hög tillgänglighet som kombinerar kärnkatalogstjänster, avancerad identitetsstyrning och programåtkomsthantering. AAD kan integreras med lokala Active Directory tooenable enkel inloggning för alla molnbaserade och lokalt (lokalt) program. hello användarattribut av lokala Active Directory kan vara automatiskt synkroniserade tooAAD.

En enda global administratör och är inte obligatoriska tooassign alla behörigheter i en vDC. I stället kan varje viss avdelning (eller grupp av användare eller tjänster i hello katalogtjänsten) ha hello behörigheter krävs toomanage sina egna resurser inom en vDC. Behörighet att strukturera kräver belastningsutjämning. För många behörigheter kan hindra prestanda effektivitet och för få eller Lös behörigheter kan öka säkerhetsrisker. Azure rollbaserad åtkomstkontroll (RBAC) hjälper tooaddress det här problemet genom att erbjuda detaljerad åtkomsthantering för vDC-resurser.

##### <a name="security-infrastructure"></a>*Säkerhetsinfrastruktur*
Säkerhetsinfrastruktur hello kontexten för en vDC är huvudsakligen relaterade toohello avgränsning av trafiken i hello vDC specifika virtuella nätverkssegment och hur toocontrol ingående och utgående flödar i hela hello vDC. Azure är baserad på arkitektur med flera innehavare som förhindrar att obehöriga och oavsiktliga trafik mellan distributioner, med isolering av virtuella nätverk (VNet) åtkomstkontrollistor (ACL), belastningsutjämnare och IP-filter, tillsammans med principer för flödet av trafik. Network address translation (NAT) separerar interna nätverkstrafiken från externa trafiken.

hello Azure-strukturen och allokerar infrastrukturresurser tootenant arbetsbelastningar och hanterar kommunikation tooand från virtuella datorer (VM). hello Azure hypervisor tillämpar minne och processen åtskillnad mellan virtuella datorer och på ett säkert sätt vägar nätverket trafik tooguest OS innehavare.

##### <a name="connectivity-toohello-cloud"></a>*Anslutningen toohello moln*
hello vDC måste ansluta till externa nätverk toooffer services toocustomers, partners och/eller interna användare. Detta innebär vanligen anslutningen toohello Internet utan även tooon lokala nätverk och datacenter.

Kunder kan skapa sina säkerhet principer toocontrol vad och hur specifika vDC värdbaserade tjänster som är tillgängliga från hello Internet med hjälp av virtuella nätverksinstallationer (med filtrering och trafik kontroll), och anpassade principer för Routning och nätverket filtrering ( Användardefinierade Routning och Nätverkssäkerhetsgrupper).

Företag behöver ofta tooconnect VDC tooon lokala Datacenter eller andra resurser. hello-anslutning mellan Azure och lokala nätverk är därför en viktig aspekt när du skapar en effektiv arkitektur. Företag som har två olika sätt toocreate en sambandet mellan virtuella och lokala i Azure: överföring över hello Internet och/eller privata direkta anslutningar.

En [ **Azure plats-till-plats-VPN** ] [ VPN] är en tjänst för samtrafik över hello Internet mellan lokala nätverk och hello vDC, kan fastställas genom säker krypterad anslutningar (IPsec/IKE-tunnlar). Azure plats-till-plats-anslutning är flexibel och snabb toocreate och kräver inte några ytterligare inköp, som alla anslutningar som ansluter via hello internet.

[**ExpressRoute** ] [ ExR] är en Azure anslutningen-tjänst som låter dig skapa privata anslutningar mellan vDC och hello lokala nätverk. ExpressRoute anslutningar inte gå över hello offentliga Internet och ger högre säkerhet, tillförlitlighet och högre hastigheter (upp too10 Gbit/s) tillsammans med konsekvent svarstid. ExpressRoute är mycket användbar för VDC, som ExpressRoute kunder kan få hello fördelarna med kompatibilitetsregler som är associerade med privata anslutningar.

Distribuera ExpressRoute anslutningar innebär att arbeta med en ExpressRoute-leverantör. Det är vanligt tooinitially plats-till-plats VPN-tooestablish anslutningen mellan hello vDC och lokala resurser för kunder som behöver toostart snabbt, och sedan migrera tooExpressRoute anslutning.

##### <a name="connectivity-within-hello-cloud"></a>*Anslutningen inom hello moln*
[Vnet] [ VNet] och [VNet-Peering] [ VNetPeering] är grundläggande hello nätverkstjänster anslutning i en vDC. Ett VNet garanterar en naturlig gräns för isolering för vDC-resurser och VNet-peering tillåter inbördes kommunikation mellan olika Vnet inom hello samma Azure-region. Kontrollera trafik i ett virtuellt nätverk och mellan virtuella nätverk måste toomatch en uppsättning säkerhet regler som angetts via åtkomstkontrollistor ([Nätverkssäkerhetsgruppen][NSG]), [virtuella nätverksenheter ] [ NVA], och anpassade routningstabeller ([UDR][UDR]).

## <a name="virtual-data-center-overview"></a>Översikt över virtuella Datacenter

### <a name="topology"></a>topologi
NAV och ekrar modellen utökade hello virtuella datacenter inom en enda Azure region

[![1]][1]

hello navet är hello central zon som styr och kontrollerar ingång och/eller utgående trafik mellan olika zoner: Internet, lokalt, och hello ekrar. hello NAV och eker-topologi ger hello IT-avdelningen ett effektivt sätt tooenforce säkerhetsprinciper på en central plats och samtidigt minska hello risken för felkonfiguration och exponering.

hello hubb innehåller hello vanliga tjänstkomponenter förbrukas av hello ekrar. Här följer några exempel på vanliga centrala tjänster:

-   hello Windows Active Directory-infrastruktur (med hello relaterade ADFS-tjänsten) krävs för autentisering av användare från tredje part kommer åt från ej betrodda nätverk innan hämtning åtkomst toohello arbetsbelastningar i hello ekrar
-   En DNS-tjänsten tooresolve naming för hello arbetsbelastning i hello ekrar, tooaccess resurser lokalt och på hello Internet
-   En PKI-infrastruktur, tooimplement enkel inloggning på arbetsbelastningar
-   Flödeskontroll (TCP/UDP) mellan hello ekrar och Internet
-   Flödeskontroll mellan hello ekrar och lokalt
-   Om du vill flödeskontroll mellan en ekrar

hello vDC minskar övergripande kostnader genom att använda hello delade hubb infrastruktur mellan flera ekrar.

hello rollen för varje ekrar kan vara toohost olika typer av arbetsbelastningar. hello ekrar kan också ge en modulär metod för repeterbara distributioner (till exempel utveckling och testning, användargodkännande, Förproduktion och produktion) av hello samma arbetsbelastningar. hello ekrar kan också använda toosegregate och aktivera olika grupper i din organisation (till exempel DevOps grupper). Inuti en ekrar är det möjligt toodeploy en grundläggande arbetsbelastning eller komplexa arbetsbelastningar i flera nivåer med trafik som styr mellan hello nivåer.

##### <a name="subscription-limits-and-multiple-hubs"></a>Prenumerationsbegränsningar och flera nav
I Azure distribueras varje komponent, oavsett hello-typ i en Azure-prenumeration. hello isolering av Azure komponenter i olika Azure-prenumerationer kan uppfylla hello av olika LOB, till exempel konfigurera olika nivåer av åtkomst och auktorisering.

En enda vDC kan skala upp toolarge antalet ekrar, även om som varje IT-system är plattformar gränser. hello hubb distribution är bundna tooa specifika Azure-prenumeration, som har begränsningar och gränser (till exempel en max antal VNet peerkopplingar - finns [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar] [ Limits] information). I fall där gränser kan vara ett problem hello arkitekturen kan skalas upp ytterligare genom att utöka hello modellen från ett enda NAV-ekrar tooa kluster med nav och ekrar. Flera nav i en eller flera Azure-regioner samman med hjälp av Express Route eller plats-till-plats-VPN.

[![2]][2]

Hej för flera nav ökar hello ansträngning kostnad och hantering av hello system och berättigat endast av skalbarhet (exempel: system gränser eller redundans) och regionala replikering (exempel: återställning av slutanvändare prestanda eller katastrof). I scenarier som kräver flera nav, alla hello hubbar bör sträva efter toooffer hello samma uppsättning tjänster för operativa enkel.

##### <a name="interconnection-between-spokes"></a>Sambandet mellan ekrar
I en enda ekrar är det möjligt tooimplement komplexa arbetsbelastningar i flera nivåer. Flera nivåer konfigurationer kan implementeras med undernät (ett för varje nivå) i hello samma virtuella nätverk och filtrera hello flödar med NSG: er.

Hej på andra sidan kan en systemarkitekt toodeploy en arbetsbelastning med flera nivåer över flera virtuella nätverk. Med VNet-peering ekrar kan ansluta tooother ekrar i hello samma hubb eller annan hubs. Ett typexempel på det här scenariot gäller hello där programmet bearbetning servrar finns i en ekrar (VNet), medan hello databasen distribueras i en annan ekrar (VNet). I det här fallet är enkelt toointerconnect hello ekrar med VNet-peering och undvika därigenom transporteras via hello hubb. En noggrann arkitektur och säkerhet granskning ska utföras tooensure som kringgå hello hubb inte kringgå viktiga säkerhets- eller granskning som kan endast finnas i hello hubb.

[![3]][3]

Ekrar kan också vara sammankopplade tooa eker som fungerar som ett nav. Den här metoden skapar en hierarki med två nivåer: hello ekrar i hello högre nivå (nivå 0) blir hello hubb för lägre ekrar (nivå 1) i hello hierarki. hello ekrar för vDC måste tooforward hello trafik toohello central knutpunkt tooreach ut toohello lokala nätverk eller internet. En arkitektur med två nivåer av NAV introducerar komplex routning som tar bort hello fördelarna med en enkel nav-eker-relation.

Även om Azure tillåter komplexa topologier, är en av hello core principerna för hello vDC konceptet repeterbarhet och enkelhet. toominimize hanteringsarbete hello enkel nav-eker-designen är hello rekommenderas vDC Referensarkitektur.

### <a name="components"></a>Komponenter
Ett virtuellt Datacenter består av fyra grundläggande komponent: **infrastruktur**, **perimeternätverk**, **arbetsbelastningar**, och **övervakning**.

Varje komponenttyp består av olika Azure-funktioner och resurser. Din vDC består av instanser av olika komponenter och flera varianter av hello samma komponenttyp. Du kan till exempel ha många olika, logiskt åtskilda arbetsbelastning instanser som representerar olika program. Du använder dessa komponenttyper av olika och instanser tooultimately skapa hello vDC.

[![4]][4]

hello Visar föregående övergripande arkitekturen för en vDC olika komponent som används i olika områden av hello topologi för NAV-ekrar. hello diagram visar infrastrukturkomponenter i olika delar av hello arkitektur.

Som en bra idé (för en lokal DC eller vDC) åtkomst vara rättigheter och behörigheter gruppbaserade. Behandlar grupper kan i stället för enskilda användare upprätthålla åtkomstprinciper konsekvent mellan olika team och hjälpmedel för minimera konfigurationsfel. Tilldela och ta bort användare tooand från relevanta grupper hjälper dig att hålla hello behörigheter för en specifik användare uppdaterade.

Varje grupp med rollen ska ha ett unikt prefix för deras namn som gör det enkelt tooidentify vilken grupp som är associerad med vilken arbetsbelastning. Exempelvis kan en arbetsbelastning som värd för en autentiseringstjänst kan ha grupper med namnet *AuthServiceNetOps, AuthServiceSecOps, AuthServiceDevOps och AuthServiceInfraOps.* På samma sätt för centraliserad roller eller roller inte relaterade tooa viss tjänst, kan inleds med ”Corp” *CorpNetOps* t.ex.

Många organisationer använder en variation av hello följande grupper tooprovide en större uppdelning av roller:

-   Hej *central IT-grupp (Corp)* hello ägarskap rättigheter toocontrol infrastruktur (till exempel nätverk och säkerhet) komponenter, och därför måste toohave hello roll deltagare i prenumeration hello (och ha kontroll över hello NAV) och bidragsgivarbehörighet i hello ekrar. Stora företag ofta dela dessa management ansvar mellan flera team som; en nätverksåtgärder (CorpNetOps)-gruppen (med exklusiv fokus på nätverk) och gruppen säkerhetsåtgärder (CorpSecOps) (ansvarig för brandväggen och säkerhet). I den här specifika fall måste två olika grupper toobe som skapats för tilldelning av dessa anpassade roller.
-   Hej *dev & testgrupp (AppDevOps)* har hello ansvar toodeploy arbetsbelastningar (appar eller tjänster). Den här gruppen tar hello av Virtual Machine-deltagare för IaaS-distributioner och/eller en eller flera PaaS-deltagare roller (se [inbyggda roller för rollbaserad åtkomstkontroll][Roles]). Du kan också hello dev & testgruppen behöva toohave synlighet för IPSec-principer (NSG: er) och routningsprinciper (UDR) i hubben hello eller en specifik ekrar. Därför i tillägg toohello roller för deltagare för arbetsbelastningar måste gruppen också hello rollen läsare i nätverket.
-   Hej *drift och underhåll grupp (CorpInfraOps eller AppInfraOps)* har hello ansvaret för att hantera arbetsbelastningar i produktion. Den här gruppen måste toobe prenumeration deltagare i arbetsbelastningar i några prenumerationer för produktion. Vissa organisationer kan även utvärdera om de behöver en ytterligare eskalering team supportgrupp med hello rollen för prenumerationen deltagare i produktion och hello central knutpunkt prenumeration, i ordning toofix potentiella konfigurationsproblem i hello produktion miljö.

En vDC är strukturerad så att grupper som skapats för hello central IT-grupper hantera hello hubben har motsvarande grupper på hello arbetsbelastningsnivå. Dessutom är toomanaging hubb endast hello central IT resursgrupper kan toocontrol extern åtkomst och översta behörigheter på hello prenumeration. Arbetsbelastningsgrupper skulle dock kan toocontrol resurser och behörigheter för sina VNet oberoende på Central IT.

hello vDC behov toobe partitionerad toosecurely värden flera projekt över olika rad-för-företag (LOB). Alla projekt kräver olika isolerade miljöer (Dev, UAT, produktion). Separata Azure-prenumerationer för var och en av dessa miljöer isolera naturlig.

[![5]][5]

hello Visar föregående diagram hello förhållandet mellan en organisations projekt, användare, grupper och hello miljöer där hello Azure komponenter har distribuerats.

Vanligtvis i IT-miljö (eller nivån) är ett system där flera program har distribuerats och körs. Stora företag använder en utvecklingsmiljö (där ursprungligen gjorda ändringar och testas) och en produktionsmiljö (vad slutanvändare använda). Dessa miljöer är ofta åtskilda med flera mellanlagringsmiljöer mellan dem tooallow faser distribution (distribution), testning och återställning vid problem. Distribution arkitekturer variera avsevärt, men vanligtvis hello grundläggande processen startar vid utveckling (utveckling) och slutar vid produktion (PROD) fortfarande följs.

En gemensam arkitektur för dessa typer av miljöer med flera nivåer består av DevOps (utveckling och testning), UAT (mellanlagring) och produktionsmiljöer. Organisationer kan använda en enda eller flera Azure AD-innehavare toodefine åtkomst och rättigheter toothese miljöer. hello föregående diagram visar ett fall där två olika används för Azure AD-klienter: en för DevOps och UAT och hello andra exklusivt för produktion.

Hej förekomst av olika Azure AD hyresgäster tillämpar hello åtskillnad mellan miljöer. Hej samma grupp av användare (exempelvis Central IT) tooauthenticate med hjälp av en annan URI tooaccess en annan AD-klient måste ändra hello roller eller behörigheter för antingen hello DevOps eller produktionsmiljöer för ett projekt. hello förekomst av olika miljöer för olika användare autentisering tooaccess minskar möjliga avbrott och andra problem som orsakas av mänskliga fel.

#### <a name="component-type-infrastructure"></a>Komponenttyp: infrastruktur
Den här komponenten är där de flesta av hello stöd för infrastruktur finns. Det är också där din centraliserad IT, säkerhet, och/eller efterlevnad teamen lägger största delen av tiden.

[![6]][6]

Ange en sambandet mellan hello olika komponenterna i en vDC infrastrukturkomponenter och finns i både hello NAV och ekrar hello. hello ansvar för att hantera och underhålla hello infrastrukturkomponenter normalt tilldelas toohello central IT och/eller säkerhetsteam.

En av hello primära uppgifter i hello IT-team för infrastruktur är tooguarantee hello konsekvenskontroll av IP-adress scheman över hello företaget. hello privata IP-adressutrymme som tilldelats toohello vDC behöver toobe konsekvent och inte överlappar privata IP-adresser tilldelas på ditt lokala nätverk.

När NAT på hello lokala routrar i utkanten eller i Azure miljöer kan undvika IP-adresskonflikter läggs komplikationer tooyour infrastrukturkomponenter. Enkel hantering är en av hello huvudmålen med vDC, så att använda NAT toohandle IP-frågor inte är en rekommenderad lösning.

Infrastrukturkomponenter innehålla hello följande funktioner:

-   [**Identitets- och directory services**][AAD]. Åtkomst tooevery resurstypen i Azure styrs av en identitet som lagras i en katalogtjänst. hello directory tjänsten lagrar inte bara hello lista över användare, utan också hello åtkomst rättigheter tooresources i en viss Azure-prenumeration. Dessa tjänster kan finnas endast molnbaserad eller de kan synkroniseras med lokala identitet som lagras i Active Directory.
-   [**Virtuellt nätverk**][VPN]. Virtuella nätverk är en av huvudkomponenterna i en vDC och aktivera toocreate en gräns för isolering av trafik på hello Azure-plattformen. Ett virtuellt nätverk består av en eller flera virtuella nätverkssegment, var och en med ett specifikt IP-nätverksprefix (undernät). hello virtuellt nätverk definierar ett internt perimeter område där IaaS virtuella datorer och PaaS-tjänster kan upprätta privata meddelanden. Virtuella datorer (och PaaS-tjänster) i ett virtuellt nätverk inte kan kommunicera direkt tooVMs (och PaaS-tjänster) i ett annat virtuellt nätverk, även om båda virtuella nätverken skapas av hello samma kund under hello samma prenumeration. Isolering är en kritisk egenskap som garanterar kundens virtuella datorer och kommunikation förblir privat inom ett virtuellt nätverk.
-   [**UDR**][UDR]. Som standard utifrån hello system routningstabellen omdirigeras trafik i ett virtuellt nätverk. En användardefinierad väg är en anpassad routningstabell att nätverksadministratörer kan associera tooone eller flera undernät toooverwrite hello funktionssätt hello system routningstabellen och definiera en sökväg för kommunikation inom ett virtuellt nätverk. hello förekomst av udr: er garanterar att utgående trafik från hello ekrar överföringen via specifika egna virtuella datorer och/eller nätverket virtuella installationer och belastningsutjämnare finns i hello NAV och ekrar hello.
-   [**NSG**][NSG]. En Nätverkssäkerhetsgrupp är en lista över säkerhetsregler som fungerar som trafik filtrering på IP-källor, IP-målet, protokoll, IP-källportar och IP-målet portar. hello NSG kan vara tillämpade tooa undernät, ett virtuellt nätverkskort kort som är associerade med en Azure VM, eller båda. hello NSG: er är viktigt tooimplement rätt flödeskontroll i hello NAV och ekrar hello. hello säkerhetsnivå som tillhandahålls av hello NSG är en funktion av vilka portar som du öppnar och för vilket syfte. Kunder bör använda ytterligare per virtuell filter med värdbaserade brandväggar, till exempel IPtables eller hello Windows-brandväggen.
-   **DNS**. hello-namnmatchningen för resurser i hello Vnet av en vDC tillhandahålls via DNS. hello omfånget för namnmatchning av hello standard DNS är begränsad toohello VNet. Vanligtvis en anpassad DNS-tjänst behöver toobe distribueras i hello-hubb som en del av gemensamma tjänster, men hello huvudsakliga konsumenter av DNS-tjänster finns i hello ekrar. Om det behövs kan kunderna skapa en hierarkisk struktur för DNS med delegering av DNS-zoner toohello ekrar.
-   [** Prenumeration] [ SubMgmt] och [resursgruppen Management][RGMgmt]**. En prenumeration definierar en naturlig gräns toocreate flera grupper av resurser i Azure. Resurser i en prenumeration är sammanfogas i logiska behållare med namnet resursgrupper. hello resursgruppen representerar en logisk grupp tooorganize hello resurser av en vDC.
-   [**RBAC**][RBAC]. Via RBAC är det möjligt toomap organisationsroller tillsammans med rättigheter tooaccess specifika Azure-resurser, så att du toorestrict användare tooonly en viss delmängd av åtgärder. Med RBAC, kan du bevilja åtkomst genom att tilldela hello rätt roll toousers, grupper och program i hello relevanta omfång. hello omfånget för en rolltilldelning kan vara en Azure-prenumeration, resursgrupp eller en enskild resurs. RBAC kan arv av behörigheter. En roll som tilldelats en överordnad omfattning ger också åtkomst toohello underordnade som finns i den. Med RBAC kan du särskilja uppgifter och ge endast hello mängd åtkomst toousers de behöver tooperform sitt arbete. Till exempel använda RBAC toolet en medarbetare hantera virtuella datorer i en prenumeration medan en annan kan hantera SQL DBs inom hello samma prenumeration.
-   [**VNet-Peering**][VNetPeering]. hello grundläggande funktion används toocreate hello-infrastrukturen för en vDC är VNet-Peering, en mekanism som ansluter två virtuella nätverk (Vnet) i hello samma region via hello nätverk av hello Azure-datacenter.

#### <a name="component-type-perimeter-networks"></a>Komponenttyp: Perimeternätverk
[Perimeternätverk] [ DMZ] komponenter (även kallat DMZ nätverk) kan du tooprovide nätverksanslutning till din lokala eller fysiska datacenternätverk, tillsammans med eventuella anslutningen tooand från hello Internet. Det är också där din nätverks- och team som är sannolikt tillbringar större delen av tiden.

Inkommande paket ska passera hello säkerhetsenheter i hello hubb, till exempel hello brandväggen, ID: N och IP-adresser, innan det nådde hello backend-servrar i hello ekrar. Internet-bunden paket från hello arbetsbelastningar bör också flödar genom hello säkerhetsenheter i hello perimeternätverk för tvingande principer, kontroll och granskningsändamål innan de lämnar hello nätverk.

Perimeternätverk nätverkskomponenter innehåller hello följande funktioner:

-   [Virtuella nätverk][VNet], [UDR][UDR], [NSG][NSG]
-   [Virtuell nätverksenhet][NVA]
-   [Belastningsutjämnare][ALB]
-   [Programgateway][AppGW] / [Brandvägg][WAF]
-   [Offentliga IP-adresser][PIP]

Vanligtvis hello central IT och säkerhet team har ansvar för Kravdefinition av och drift av hello perimeternätverk.

[![7]][7]

hello föregående diagram visar hello tvingande av två ytgränser med toohello för åtkomst till internet och ett lokalt nätverk, både i hello hubb. I ett enda NAV kan hello perimeter nätverket toointernet skala upp toosupport stort antal LOB, med hjälp av flera grupper av Web Application brandväggar (WAFs) och/eller brandväggar.

[**Virtuella nätverk** ] [ VNet] hello hubb bygger vanligtvis på ett virtuellt nätverk med flera undernät toohost hello annan typ av tjänster filtrering och inspektera trafik tooor från hello internet via NVAs WAFs, och Gateways i Azure-program.

[**UDR** ] [ UDR] med UDR kunder kan distribuera brandväggar, ID: N/IP-adresser, och andra virtuella installationer och dirigera nätverkstrafik via hushållsapparater säkerhet för tvingande säkerhetsprinciper gräns, granskning och kontroll. Udr: er kan skapas i båda hello NAV- och hello ekrar tooguarantee som eltransit trafik via hello specifika egna virtuella datorer, virtuella nätverksenheter och belastningsutjämnare som används av hello vDC. tooguarantee som trafik genereras från virtuella datorer i hello ekrar överföring toohello rätt virtuella installationer, måste en UDR toobe ange i hello undernät i hello ekrar genom att ange hello frontend IP-adressen för hello intern belastningsutjämnare som hello nästa hopp. hello intern belastningsutjämnare distribuerar hello intern trafik toohello virtuella installationer (belastningen belastningsutjämnaren backend-pool).

[![8]][8]

[**Nätverks-virtuella installationer** ] [ NVA] i hello hubb hello perimeternätverk med åtkomst toohello internet normalt hanteras via en servergrupp brandväggar och/eller Web Application brandväggar (WAFs).

Dessa program brukar toosuffer från olika säkerhetsproblem och potentiella kryphål olika LOB använder ofta många webbprogram. Web Applications brandväggar är en särskild RAS av produkten används toodetect attacker mot webbprogram (HTTP/HTTPS) i djupare än en allmän brandvägg. Jämfört med tradition brandväggsteknik har WAFs en uppsättning specifika funktioner tooprotect interna webbservrar mot hot.

Grupp med en brandvägg är grupp brandväggar som arbetar tillsammans under hello samma vanliga administration, med en uppsättning säkerhet regler tooprotect hello arbetsbelastningar finns i hello ekrar och kontrollera åtkomst tooon lokala nätverk. Grupp med en brandvägg har mindre specialiserat programvara jämfört med en Brandvägg, men har en bred program omfång toofilter och inspektera varje typ av trafik i ut- och ingångsanspråk. Brandväggen servergrupper implementeras normalt i Azure via nätverk virtuella installationer (NVAs) som är tillgängliga i hello Azure marketplace.

Det rekommenderas toouse en uppsättning NVAs för trafik på hello Internet, och en annan för trafik som skickas lokalt. Med bara en uppsättning NVAs för både utgör en säkerhetsrisk eftersom det ger dig inga säkerhet perimeter mellan hello två uppsättningar av nätverkstrafik. Med separata NVAs minskar hello kontrollerar säkerhetsregler och det är tydligt vilka regler som motsvarar toowhich inkommande nätverksbegäran.

De flesta stora företag hantera flera domäner. Azure DNS kan vara används toohost hello DNS-poster för en viss domän. Som exempel kan hello virtuella IP-adress (VIP) hello Azure extern belastningsutjämnare (eller hello WAFs) registreras i hello A-post för en Azure DNS-post.

[**Azure belastningsutjämnare** ] [ ALB] Azure belastningsutjämnare erbjuder en lager 4 (TCP, UDP)-tjänsten, som kan distribuera inkommande trafik mellan tjänstinstanser som har definierats i en belastningsutjämnad uppsättning med hög tillgänglighet. Trafik skickas toohello belastningsutjämnaren från frontend slutpunkter (offentliga IP-slutpunkter eller privata IP-slutpunkter) får du distribuera med eller utan adress translation tooa uppsättning backend-IP-adresspool (exempel som; Virtuella installationer i nätverket eller virtuella datorer).

Azure belastningsutjämnare kan avsökning hello hälsotillstånd hello samt olika server-instanser och när en avsökning misslyckas toorespond hello belastningen belastningsutjämnaren slutar skicka trafik toohello ohälsosamt instans. I en vDC har vi hello förekomsten av en extern belastningsutjämnare i hello hubb (till exempel saldo hello trafik tooNVAs) och i hello ekrar (tooperform aktiviteter som belastningsutjämning trafik mellan olika virtuella datorer i en programarkitektur).

[**Programgateway** ] [ AppGW] Programgateway för Microsoft Azure är en dedikerad virtuell installation som ger programmet leverans domänkontrollant (ADC) som en tjänst med olika layer 7 belastningsutjämning för ditt program. Det gör toooptimize web servergruppen produktivitet genom att avlasta CPU beräkningsintensiva SSL-avslutning toohello Programgateway. Här finns även andra layer 7 funktioner inklusive resursallokering distribution av inkommande trafik, cookie-baserad session tillhörighet, URL-sökväg-baserad Routning och hello möjlighet toohost flera webbplatser bakom en enda Application Gateway. Det finns också en brandvägg för webbaserade program (Brandvägg) som en del av hello Programgateway Brandvägg SKU. Den här SKU skyddar tooweb program från vanliga web säkerhetsrisker och trojaner. Application Gateway kan konfigureras som internetuppkopplad gateway, endast intern gateway eller en kombination av båda. 

[**Offentliga IP-adresser** ] [ PIP] vissa Azure funktioner aktivera du tooassociate service slutpunkter tooa offentlig IP-adress som gör att tooyour resurs toobe nås från hello internet. Den här slutpunkten använder NAT (Network Address Translation) tooroute trafik toohello interna adress och port på hello virtuella Azure-nätverket. Den här sökvägen kan hello primära externa trafiken toopass i hello virtuellt nätverk. hello offentliga IP-adresser kan vara konfigurerade toodetermine vilken trafik som skickas och hur och var den översätts på toohello virtuellt nätverk.

#### <a name="component-type-monitoring"></a>Komponenttyp: övervakning
Övervakning komponenter ger synlighet och aviseringar från alla hello andra typer av komponenter. Alla team ska ha åtkomst toomonitoring för hello komponenter och tjänster som de har åtkomst till. Om du har en centraliserad help desk eller operations team, behöver de toohave integrerad åtkomst toohello data som tillhandahålls av dessa komponenter.

Azure erbjuder olika typer av loggning och övervakning services tootrack hello beteendet för Azure värdbaserade resurser. Styrning och kontroll av arbetsbelastningar i Azure är baserat inte bara på att samla in loggdata men hello möjlighet tootrigger åtgärder baserat på specifika rapporterade händelser.

Det finns två typer av loggar i Azure:

-   [**Aktivitetsloggar** ] [ ActLog] (kallas även ”arbetsloggen”) ger kunskaper om hello-åtgärder som utfördes på resurser i hello Azure-prenumeration. De här loggarna rapporterar hello kontroll-plan händelser för dina prenumerationer. Alla resurser i Azure ger granskningsloggar.

-   [**Azure diagnostikloggar** ] [ DiagLog] är loggar som genereras av en resurs som innehåller omfattande, ofta data om hello driften av den här resursen. hello innehållet i de här loggarna varierar beroende på resurstypen.

[![9]][9]

I en vDC är det mycket viktigt tootrack hello NSG: er loggar, särskilt informationen:

-   [**Händelseloggar**][NSGLog]: innehåller information om vilka NSG-regler är tillämpade tooVMs och instans roller baserat på MAC-adress.
-   [**Prestandaräknaren loggar**][NSGLog]: håller reda på hur många gånger varje NSG regel har utförd toodeny eller Tillåt trafiken.

Alla loggar kan lagras i Azure Storage-konton för granskning, statiska analys eller säkerhetskopiering. När hello loggfilerna lagras i ett Azure storage-konto, kunder kan använda olika typer av ramverk tooretrieve Förbered dig, analysera och visualisera denna data tooreport hello statusen och hälsan på molnresurser.

Stora företag bör redan ha fått ett ramverk som standard för övervakning lokalt system och kan utöka att framework toointegrate loggar som genereras av distributioner. För organisationer som vill tookeep alla hello loggning i hello molnet [Microsoft Operations Management Suite (OMS)] [ OMS] är ett bra alternativ. Eftersom OMS implementeras som en molnbaserad tjänst kan du snabbt komma igång med minimal investering i infrastrukturtjänster. OMS kan också integreras med System Center-komponenter, till exempel System Center Operations Manager tooextend management befintliga investeringar i hello moln.

OMS Log analytics är en komponent i hello OMS framework toohelp samla in, korrelera, Sök och agera för logg-och prestandadata som genererats av operativsystem, program, infrastruktur molnet komponenter. Den ger kunder realtid åtgärdsinformation som använder integrerad sökning och anpassade instrumentpaneler tooanalyze alla hello-poster för alla arbetsbelastningar i en vDC.

#### <a name="component-type-workloads"></a>Komponenttyp: arbetsbelastningar
Komponenter för arbetsbelastningen är där dina faktiska program och tjänster finns. Det är också där dina program utvecklingsgrupper tillbringar större delen av tiden.

hello arbetsbelastning möjligheterna är verkligen oändliga. hello följande är några av hello möjliga arbetsbelastning typer:

**Internt LOB-program**

Line-of-business-program är datorn program kritiska toohello pågående åtgärder på ett företag. LOB-program har vissa gemensamma egenskaper:

-   **Interaktiv**. LOB-program är interaktiva karaktär: data har angetts och returneras resultatet/rapporter.
-   **Datadrivna**. LOB-program är data beräkningsintensiva med frekvent åtkomst toohello databaser eller andra lagring.
-   **Integrerad**. LOB-program erbjudande integrering med andra system inom eller utanför hello organisation.

**Kundinriktade webbplatser (Internet eller internt inriktad)** de flesta program som interagerar med hello Internet är webbplatser. Azure erbjuder hello kapaciteten toorun en webbplats på en IaaS-VM eller från en [Azure Web Apps] [ WebApps] plats (PaaS). Azure Web Apps stöd för integrering med Vnet som tillåter hello distribution av hello Web Apps i hello ekrar av en vDC. Med hello VNET integration, behöver inte tooexpose en Internet-slutpunkt för dina program men kan använda hello resurser privata icke-dirigerbara Internetadressen från ditt privata virtuella nätverk i stället.

**Stor dataanalys** när data måste tooscale in tooa mycket stora volymer, databaser kan inte skalas upp korrekt. Hadoop-teknik ger ett system toorun distribuerade frågor parallellt på många noder. Kunder har hello alternativet toorun data arbetsbelastningar i IaaS-VM eller PaaS ([HDInsight][HDI]). HDInsight stöder distribution i en plats-baserade virtuella nätverk, kan vara distribuerade tooa i en talade hello vDC.

**Händelser och meddelanden**
[Azure Event Hubs] [ EventHubs] är en tjänst för införandet av storskaliga telemetri som samlar in, omvandlar och lagrar miljontals händelser. En distribuerad strömmande plattform som erbjuder låg latens och konfigurerbara tid kvarhållning, vilket gör att du tooingest enorma mängder telemetri till Azure och läsa data från flera program. Med Händelsehubbar, kan en enda ström stöder både i realtid och batch-baserade pipelines.

Ett mycket pålitlig moln meddelandetjänsten mellan program och tjänster, kan implementeras via [Azure Service Bus] [ ServiceBus] att erbjudanden asynkron asynkron meddelandetjänst mellan klienten och servern, tillsammans med strukturerade första in first out (FIFO) meddelanden och publicera/prenumerera funktioner.

[![10]][10]

### <a name="multiple-vdc"></a>Flera vDC
Hittills har den här artikeln fokuserar på en enda vDC som beskriver grundläggande hello-komponenter och arkitektur som bidrar tooa flexibel vDC. Azure-funktioner, till exempel Azure belastningen belastningsutjämnare, NVAs, tillgänglighetsuppsättningar skalningsuppsättningar, tillsammans med andra mekanismer bidrar tooa system som tillåter toobuild fasta SLA-nivåer i produktions-tjänster.

Men en enda vDC ligger på en enskild region och är sårbara toomajor avbrott som kan påverka hela regionen. Kunder som vill tooachieve hög SLA: er måste tooprotect hello tjänster via distributioner av hello samma projekt i två (eller fler) VDC placeras i olika regioner.

Det finns flera vanliga scenarier där distribuera flera VDC klokt i tillägg tooSLA gäller:

-   Nationella inställningar/Global närvaro
-   Katastrofåterställning
-   Mekanism toodivert trafik mellan DC

#### <a name="regionalglobal-presence"></a>Nationella inställningar/Global närvaro
Azure-datacenter finns i flera regioner över hela världen. När du väljer flera Azure-datacenter, kunder måste tooconsider två relaterade faktorer: geografiska avstånd och svarstid. Kunder måste tooevaluate hello geografiska avståndet mellan hello VDC och hello avståndet mellan hello vDC och hello slutanvändare toooffer hello bästa användarupplevelsen.

hello Azure-Region där VDC finns måste också tooconform med regelkrav upprättas av alla juridiska behörighet som din organisation fungerar.

#### <a name="disaster-recovery"></a>Katastrofåterställning
hello implementering av en plan för katastrofåterställning är starkt relaterade toohello typ av arbetsbelastning berörda och hello möjlighet toosynchronize hello arbetsbelastning tillstånd mellan olika VDC. Vi rekommenderar vill de flesta kunder toosynchronize programdata mellan distributioner som körs i två olika VDC tooimplement en snabb failover mekanism. De flesta program är känslig toolatency och som kan orsaka potentiella timeout och fördröjning i datasynkronisering.

Synkronisering eller heartbeat-övervakning av program i olika VDC kräver kommunikation mellan dem. Två VDC i olika regioner kan vara ansluten via:

-   ExpressRoute privat peering när hello vDC-hubbar är anslutna toohello samma ExpressRoute-krets
-   flera ExpressRoute-kretsar ansluts med ditt företags stamnät och vDC-nät anslutna toohello ExpressRoute-kretsar
-   Plats-till-plats VPN-anslutningar mellan vDC-hubbar i varje Azure-Region

Hej ExpressRoute-anslutning är vanligtvis hello förvalda mekanism på grund av högre bandbredd och konsekvent fördröjning när transporteras via hello Microsoft stamnät.

Det finns inga magiskt recept toovalidate ett program som distribueras mellan två (eller fler) olika VDC i olika områden. Kunder bör köra kriteriet testerna tooverify hello Nätverksfördröjningen och bandbredd av hello anslutningar och mål om synkron eller asynkron replikering är rätt och vilka hello optimal återställning tid mål för Återställningstid kan vara för din arbetsbelastningar.

#### <a name="mechanism-toodivert-traffic-between-dc"></a>Mekanism toodivert trafik mellan DC
En effektiv metod toodivert hello-trafik inkommande i en DC tooanother baseras på DNS. [Azure Traffic Manager] [ TM] använder hello Domain Name System (DNS) mekanism toodirect hello slutanvändarens trafik toohello lämpligaste offentlig slutpunkt i en specifik vDC. Traffic Manager söker regelbundet hello tjänstens hälsa för offentliga slutpunkter i olika VDC via avsökningar, och vid avbrott för dessa slutpunkter dirigerar automatiskt toohello sekundära vDC.

Traffic Manager fungerar på Azure offentliga slutpunkter och kan användas, till exempel toocontrol/omdirigera trafik tooAzure virtuella datorer och Web Apps i hello lämpliga vDC. Traffic Manager är flexibla även i hello står inför ett hela Azure-region misslyckas och kan styra hello distributionen av användartrafik för slutpunkter i olika VDC flera villkor (till exempel fel på en tjänst i en specifik vDC eller välja hello vDC med hello lägsta Nätverksfördröjningen för hello klient).

### <a name="conclusion"></a>Slutsats
hello virtuella Datacenter är en metod toodata center migrering i hello moln som använder en kombination av funktioner och möjligheter toocreate en skalbar arkitektur i Azure som maximerar molnet Resursanvändning, minskar kostnaderna och förenkla system styrning. hello vDC konceptet är baserad på en hub ekrar topologi tillhandahålla vanliga delade tjänster i hello hubb och tillåter specifika program/arbetsbelastningar i hello ekrar. En vDC matchar strukturen hello för företag roller, där olika avdelningar (Central IT, DevOps, drift och underhåll) arbetar tillsammans med en specifik lista över roller och ansvarsområden. En vDC uppfyller hello krav för migrering ”lyfter och SKIFT”, men också ger många fördelar toonative distributioner.

## <a name="references"></a>Referenser
hello följande funktioner beskrivs i det här dokumentet. Klicka på hello länkar toolearn mer.

| | | |
|-|-|-|
|Nätverksfunktioner|Belastningsutjämning|Anslutning|
|[Virtuella Azure-nätverk][VNet]</br>[Nätverkssäkerhetsgrupper][NSG]</br>[NSG-loggar][NSGLog]</br>[Användardefinierade Routning][UDR]</br>[Virtuella nätverksenheter][NVA]</br>[Offentliga IP-adresser][PIP]|[Azure belastningsutjämnare (L3)][ALB]</br>[Programgateway (L7)][AppGW]</br>[Brandvägg för webbaserade program][WAF]</br>[Azure Traffic Manager][TM] |[VNet-Peering][VNetPeering]</br>[Virtuellt privat nätverk][VPN]</br>[ExpressRoute][ExR]
|Identitet</br>|Övervakning</br>|Metodtips</br>|
|[Azure Active Directory][AAD]</br>[Multifaktorautentisering][MFA]</br>[Rollen grundläggande åtkomstkontroller][RBAC]</br>[Standardroller i AAD][Roles] |[Aktivitetsloggar][ActLog]</br>[Diagnostikloggar][DiagLog]</br>[Microsoft Operations Management Suite][OMS]</br> |[Metodtips för perimeter-nätverk][DMZ]</br>[Prenumerationshantering][SubMgmt]</br>[Hantering av resursgruppen.][RGMgmt]</br>[Azure-prenumerationsbegränsningar][Limits] |
|Andra Azure-tjänster|
|[Azure-Webbappar][WebApps]</br>[HDInsights (Hadoop)][HDI]</br>[Event Hubs][EventHubs]</br>[Service Bus][ServiceBus]|



## <a name="next-steps"></a>Nästa steg
 - Utforska [VNet-Peering][VNetPeering], hello gäller teknik för vDC NAV och eker Designer
 - Implementera [AAD] [ AAD] tooget igång med [RBAC] [ RBAC] undersökning
 - Utveckla en prenumeration och resurs management-modell och RBAC modell toomeet hello struktur, krav, och principerna för din organisation. hello viktigaste aktivitet planering. Planera så mycket som praktiska, omorganisering, sammanslagningar, ny rad för produkten och så vidare.

<!--Image References-->
[0]: ./media/networking-virtual-datacenter/redundant-equipment.png "Exempel på komponenten överlappar varandra" 
[1]: ./media/networking-virtual-datacenter/vdc-high-level.png "Utförligt exempel på NAV och ekrar vDC"
[2]: ./media/networking-virtual-datacenter/hub-spokes-cluster.png "Kluster med nav och ekrar"
[3]: ./media/networking-virtual-datacenter/spoke-to-spoke.png "Eker-eker"
[4]: ./media/networking-virtual-datacenter/vdc-block-level-diagram.png "Nivån blockdiagram där hello vDC"
[5]: ./media/networking-virtual-datacenter/users-groups-subsciptions.png "Användare, grupper, prenumerationer och projekt"
[6]: ./media/networking-virtual-datacenter/infrastructure-high-level.png "Övergripande infrastrukturdiagram"
[7]: ./media/networking-virtual-datacenter/highlevel-perimeter-networks.png "Övergripande infrastrukturdiagram"
[8]: ./media/networking-virtual-datacenter/vnet-peering-perimeter-neworks.png "VNet-Peering och perimeter-nätverk"
[9]: ./media/networking-virtual-datacenter/high-level-diagram-monitoring.png "Översiktsdiagram för övervakning"
[10]: ./media/networking-virtual-datacenter/high-level-workloads.png "Översiktsdiagram för arbetsbelastning"

<!--Link References-->
[Limits]: https://docs.microsoft.com/azure/azure-subscription-service-limits
[Roles]: https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles
[VNet]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview
[NSG]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg 
[VNetPeering]: https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview 
[UDR]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview 
[RBAC]: https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is
[MFA]: https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication
[AAD]: https://docs.microsoft.com/azure/active-directory/active-directory-whatis
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways 
[ExR]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction 
[NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha
[SubMgmt]: https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-subscription-governance 
[RGMgmt]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview
[DMZ]: https://docs.microsoft.com/azure/best-practices-network-security
[ALB]: https://docs.microsoft.com/azure/load-balancer/load-balancer-overview
[PIP]: https://docs.microsoft.com/azure/virtual-network/resource-groups-networking#public-ip-address
[AppGW]: https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction
[WAF]: https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview
[ActLog]: https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs 
[DiagLog]: https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs
[NSGLog]: https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log
[OMS]: https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview
[WebApps]: https://docs.microsoft.com/azure/app-service-web/
[HDI]: https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-introduction
[EventHubs]: https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs 
[ServiceBus]: https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview
[TM]: https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview
