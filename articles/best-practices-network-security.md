---
title: "aaaAzure nätverk säkerhetsmetoder | Microsoft Docs"
description: "Lär dig mer om några av hello viktiga funktioner som är tillgängliga i Azure toohelp skapa säker nätverksmiljöer"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: d169387a-1243-4867-a602-01d6f2d8a2a1
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: b851b2862428a8bd5e7525c85584fc1c14ffcabe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-cloud-services-and-network-security"></a>Microsoft cloud services och nätverkssäkerhet
Microsoft-molntjänster leverera storskaliga tjänster och infrastruktur, funktioner i företagsklass och många alternativ för hybridanslutning. Kunder kan välja tooaccess dessa tjänster via hello Internet eller med Azure ExpressRoute som tillhandahåller anslutningar privata nätverk. hello Microsoft Azure-plattformen gör att kunder tooseamlessly utökar sin infrastruktur till molnet hello och bygga arkitektur för flera nivåer. Tredje part kan dessutom aktivera förbättrade funktioner genom att erbjuda tjänster och virtuella installationer. Den här rapporten innehåller en översikt över arkitekturen problem som kunder bör tänka på när du använder Microsoft-molntjänster som nås via ExpressRoute och säkerhet. Den omfattar också skapa säkrare tjänster i virtuella Azure-nätverk.

## <a name="fast-start"></a>Snabb start
hello följande logik diagrammet kan dirigera du tooa exempel på hello många säkerhetstekniker som är tillgängliga med hello Azure-plattformen. Hitta hello exempel som bäst passar ditt ärende för Snabbreferens. Fortsätta läsa igenom hello papper expanderade förklaringar.
[![0]][0]

[Exempel 1: Skapa ett perimeternätverk (även kallat DMZ, demilitariserad zon och avskärmat undernät) toohelp skydda program med nätverkssäkerhetsgrupper (NSG: er).](#example-1-build-a-perimeter-network-to-help-protect-applications-with-nsgs)</br>
[Exempel 2: Skapa en perimeter nätverket toohelp skydda program med en brandvägg och NSG: er.](#example-2-build-a-perimeter-network-to-help-protect-applications-with-a-firewall-and-nsgs)</br>
[Exempel 3: Skapa en perimeter nätverket toohelp skydda nätverk med en brandvägg, användardefinierad väg (UDR) och NSG.](#example-3-build-a-perimeter-network-to-help-protect-networks-with-a-firewall-and-udr-and-nsg)</br>
[Exempel 4: Lägga till en hybridanslutning med ett plats-till-plats, virtuella installation virtuellt privat nätverk (VPN).](#example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn)</br>
[Exempel 5: Lägg till en hybridanslutning med en plats-till-plats, Azure VPN-gateway.](#example-5-add-a-hybrid-connection-with-a-site-to-site-azure-vpn-gateway)</br>
[Exempel 6: Lägg till en hybridanslutning med ExpressRoute.](#example-6-add-a-hybrid-connection-with-expressroute)</br>
Exempel för att lägga till anslutningar mellan virtuella nätverk, hög tillgänglighet och tjänsten länkning läggs toothis dokumentet via hello kommande månaderna.

## <a name="microsoft-compliance-and-infrastructure-protection"></a>Microsoft skydd för efterlevnad och infrastruktur
toohelp organisationer uppfylla nationella, regionala, och branschspecifika krav för hello insamling och användning av data för enskilda användare, erbjuder Microsoft än 40 certifieringar och intyg. hello mest omfattande uppsättning eventuella molntjänstleverantören.

Mer information finns i hello kompatibilitetsinformation på hello [Microsoft Trust Center][TrustCenter].

Microsoft har en omfattande metod tooprotect infrastruktur som behövs för toorun storskaliga globala molntjänster. Microsoft-molninfrastruktur innehåller maskinvara, programvara, nätverk, och administrativa och driftspersonal, dessutom toohello fysiska datacenter.

![2]

Den här metoden ger en säkrare grunden för kunder toodeploy sina hello Microsoft cloud-tjänster. hello nästa steg är för kunder toodesign och skapa en säkerhet arkitektur tooprotect dessa tjänster.

## <a name="traditional-security-architectures-and-perimeter-networks"></a>Traditionell säkerhetsarkitekturer och perimeternätverk
Även om Microsoft investerar kraftigt för att skydda hello moln-infrastruktur, måste också kunder att skydda sina molntjänster och resursgrupper. En försvar toosecurity ger hello bästa skydd. En perimeter-nätverk säkerhetszon skyddar interna nätverksresurser från ett icke betrott nätverk. Ett perimeternätverk refererar toohello kanter eller delar av hello nätverket mellan hello Internet och hello skyddade enterprise IT-infrastruktur.

I typiska företagsnätverk spikas kraftigt hello grundläggande infrastruktur på hello ytgränser med flera lager av säkerhetsenheter. hello gränsen för varje lager består av enheter och principen tvingande punkter. Varje lager kan innehålla en kombination av hello efter nätverksenheter säkerhet: brandväggar, Denial of Service (DoS) förebyggande, intrångsidentifiering eller skyddssystem (ID: N/IP-adresser) och VPN-enheter. Tvingande principer kan ta hello form av Brandväggsprinciper, åtkomstkontrollistor (ACL) eller specifika routning. hello första försvarslinje i hello nätverk, direkt acceptera inkommande trafik från hello Internet, är en kombination av dessa mekanismer tooblock attacker och skadlig trafik samtidigt ytterligare legitima begäranden i hello nätverk. Den här trafiken dirigerar direkt tooresources i hello perimeternätverk. Den här resursen kan sedan ”prata” tooresources djupare i hello nätverk, transit hello nästa gräns för verifiering första. hello yttersta layer kallas hello perimeternätverk eftersom den här delen av hello nätverk är utsatta toohello Internet, normalt med någon form av skydd på båda sidor. hello följande bild visar ett exempel på ett enda undernät perimeternätverk i ett företagsnätverk, med två säkerhetsgränser.

![3]

Det finns många arkitekturer används tooimplement ett perimeternätverk. Dessa arkitekturer kan röra sig om ett enkelt belastningen belastningsutjämnaren tooa flera undernät perimeternätverk med olika metoder på varje gräns tooblock trafik och skydda hello djupare lager i hello företagsnätverket. Hur hello perimeternätverk byggs beror på hello särskilda behov hello organisation och dess övergripande risktolerans.

Kunder flytta sina arbetsbelastningar toopublic moln, är det kritiska toosupport liknande funktioner för perimeter nätverksarkitekturen i Azure toomeet kompatibilitet och säkerhetskrav. Det här dokumentet innehåller riktlinjer om hur kunder kan skapa en säker nätverksmiljö i Azure. Den fokuserar på hello perimeternätverk, men innehåller också en fullständig beskrivning av flera olika aspekter av nätverkssäkerhet. följande frågor hello informerar den här diskussionen:

* Hur byggas ett perimeternätverk i Azure?
* Vilka är några av hello Azure-funktioner tillgängliga toobuild hello perimeternätverket?
* Hur kan backend-arbetsbelastningar skyddade?
* Hur är Internet-kommunikation kontrollerade toohello arbetsbelastningar i Azure?
* Hur kan hello lokala nätverk skyddas från distributioner i Azure?
* När ska interna Azure säkerhetsfunktioner användas jämfört med tredjeparts-utrustning eller tjänster?

hello följande diagram visar olika säkerhetsnivåer som Azure tillhandahåller toocustomers. Dessa lager är både intern i hello Azure-plattformen och kunddefinierade funktioner:

![4]

Inkommande från hello Internet, Azure DDoS hjälper dig att skydda mot storskaliga attacker mot Azure. hello nästa säkerhetslager är kunddefinierade offentliga IP-adresser (slutpunkter) som används toodetermine vilken trafik kan passera hello cloud service toohello virtuellt nätverk. Interna Azure virtuell nätverksisolering garanterar fullständig isolering från andra nätverk och att trafiken endast flödar via konfigurerade användaren sökvägar och metoder. Dessa sökvägar och metoder är hello nästa lager, där NSG: er, UDR och virtuella nätverksenheter kan vara används toocreate säkerhet gränser tooprotect hello programdistributioner i hello skyddat nätverk.

hello nästa avsnitt innehåller en översikt över virtuella Azure-nätverk. Dessa virtuella nätverk skapas av kunder och är de distribuerade arbetsbelastningarna är anslutna till. Virtuella nätverken är hello grunden för alla hello funktioner för nätverkssäkerhet krävs tooestablish en perimeter tooprotect kunden nätverksdistributioner i Azure.

## <a name="overview-of-azure-virtual-networks"></a>Översikt över virtuella Azure-nätverk
Innan Internettrafik får toohello virtuella Azure-nätverk, finns det två lager av säkerhet inbyggd toohello Azure-plattformen:

1.    **DDoS-skydd**: DDoS-skydd är ett lager av hello Azure fysiska nätverk som skyddar hello Azure-plattformen från storskaliga Internet-baserade attacker. Angreppen använder flera ”bot” noder i ett försök toooverwhelm en Internet-tjänst. Azure har en robust DDoS-skydd nät alla inkommande, utgående och över flera Azure-region-anslutning. DDoS-skydd lagret har inga konfigurerbara användarattribut och är inte tillgänglig toohello kunden. Hej DDoS skyddslager skyddar Azure som en plattform från storskaliga attacker, övervakar även ut bunden trafik och trafik över flera Azure-region. Med virtuella nätverksenheter på hello VNet, kan ytterligare lager av återhämtning konfigureras med hello kunden mot en mindre skala angrepp som inte utlösas skydd på nätverksnivå hello plattform. Ett exempel på DDoS i åtgärden. Om en IP-adress för internetuppkopplad angripna av en storskalig DDoS-attack skulle Azure identifiera hello källor hello attacker och Skrubba hello felaktig trafik innan den har nått sin destination. I nästan alla fall påverkas hello angripna endpoint inte av hello-attack. I hello sällsynta fall att en slutpunkt påverkas är ingen trafik berörda tooother slutpunkter, angripna hello-slutpunkten. Därmed visas andra kunder och tjänster utan att påverka från den attacker. Det är kritiska toonote som Azure DDoS endast söker efter storskaliga attacker. Det är möjligt att en specifik tjänst kan vara för många innan hello plattform skydd på nätverksnivå tröskelvärden överskrids. Till exempel kan en webbplats på en enskild A0 IIS-server vara offline av en DDoS-attack innan Azure-plattformsnivå DDoS-skydd registrerat ett hot.

2.  **Offentliga IP-adresser**: offentliga IP-adresser (aktiverat via slutpunkter för offentliga IP-adresser, Programgateway och andra Azure-funktioner som presenterar en offentlig IP-adress toohello internet dirigeras tooyour-resurs) låter molntjänster eller resursgrupper toohave offentliga Internet IP-adresser och portar som visas. hello slutpunkten använder NAT (Network Address Translation) tooroute trafik toohello interna adress och port på hello virtuella Azure-nätverket. Den här sökvägen kan hello primära externa trafiken toopass i hello virtuellt nätverk. hello offentliga IP-adresser är konfigurerbar toodetermine vilken trafik som skickas och hur och var den översätts på toohello virtuellt nätverk.

När trafiken når hello virtuellt nätverk, finns det många funktioner som kommer till användning. Virtuella Azure-nätverk är hello grunden för kunder tooattach sina arbetsbelastningar och där grundläggande behörighet gäller. Det är ett privat nätverk (ett virtuellt nätverk överlägg) i Azure för kunder med hello följande funktioner och egenskaper:

* **Trafik isolering**: ett virtuellt nätverk är hello trafik isoleringsgräns på hello Azure-plattformen. Virtuella datorer (VM) i ett virtuellt nätverk kan inte kommunicera direkt tooVMs i ett annat virtuellt nätverk, även om båda virtuella nätverken skapas av hello samma kund. Isolering är en kritisk egenskap som garanterar kundens virtuella datorer och kommunikation förblir privat inom ett virtuellt nätverk.

>[!NOTE]
>Trafikisolering refererar bara tootraffic *inkommande* toohello virtuellt nätverk. Som standard utgående trafik från hello VNet toohello internet tillåts, men kan förhindras om du vill av NSG: er.
>
>

* **Topologi i flera skikt**: virtuella nätverk kan kunder toodefine topologi i flera skikt genom att allokera undernät och bestämma separat adressutrymmen för olika element eller ”nivåerna” för deras arbetsbelastning. Dessa logiska grupperingar topologier aktivera kunder toodefine olika princip utifrån hello typer av arbetsbelastningar och också kontrollera trafikflödet mellan hello nivåer.
* **Korsanslutningar**: kunder kan upprätta en anslutning mellan ett virtuellt nätverk och flera lokala platser eller andra virtuella nätverk i Azure. tooconstruct en anslutning som kunder kan använda VNet-Peering Azure VPN-gatewayer, från tredje part virtuella installationer eller ExpressRoute. Azure har stöd för plats-till-plats (S2S) VPN-anslutningar med IPsec/IKE standardprotokoll och privata ExpressRoute-anslutning.
* **NSG** kunder toocreate regler (ACL) på hello önskad nivå: gränssnitt, enskilda virtuella datorer eller virtuella undernät. Kunder kan styra åtkomsten genom att tillåta eller neka kommunikation mellan hello arbetsbelastningar inom ett virtuellt nätverk från system i kundens nätverk via anslutning, eller dirigera Internet-kommunikation.
* **UDR** och **IP-vidarebefordran** att kunder toodefine hello Kommunikationssökvägar mellan olika nivåer inom ett virtuellt nätverk. Kunder kan distribuera en brandvägg, ID: N/IP-adresser och andra virtuella installationer och dirigera nätverkstrafik via hushållsapparater säkerhet för tvingande säkerhetsprinciper gräns, granskning och kontroll.
* **Nätverks-virtuella installationer** i hello Azure Marketplace: säkerhetsenheter som brandväggar, belastningsutjämnare och ID: N/IP-adresser som är tillgängliga i hello Azure Marketplace och hello Galleri för VM-avbildning. Kunderna kan distribuera dessa enheter till sina virtuella nätverk, och särskilt på deras säkerhet gränser (inklusive hello perimeter undernätverk) toocomplete en flera nivåer säker nätverksmiljö.

Med dessa funktioner och möjligheter är ett exempel på hur en perimeter nätverksarkitektur kan konstrueras i Azure hello följande diagram:

![5]

## <a name="perimeter-network-characteristics-and-requirements"></a>Egenskaper för perimeter-nätverk och krav
hello perimeternätverk är hello klientdelen hello-nätverkets samverkar direkt kommunikation från hello Internet. inkommande hälsningspaket ska passera hello säkerhetsenheter som hello brandväggen, ID: N och IP-adresser, innan det nådde hello backend-servrar. Internet-bunden paket från hello arbetsbelastningar kan passera hello säkerhetsenheter i hello perimeternätverk för tvingande principer, kontroll och granskningsändamål innan de lämnar hello nätverk. Dessutom kan hello perimeternätverk värd mellan lokala VPN-gatewayer mellan kundens virtuella nätverk och lokala nätverk.

### <a name="perimeter-network-characteristics"></a>Egenskaper för perimeter-nätverk
Refererar till hello föregående bild är vissa hello-egenskaperna hos ett bra perimeternätverk följande:

* Mot Internet:
  * hello perimeter nätverk och själva undernätet är mot Internet, kommunicerar direkt med hello Internet.
  * Offentliga IP-adresser, virtuella IP-adresser och/eller Tjänsteslutpunkter skicka trafik Internet toohello klientnätverk och enheter.
  * Inkommande trafik från hello Internet passerar säkerhetsenheter innan andra resurser på hello klientnätverk.
  * Om utgående Säkerhetsaktiverade passerar trafik som genom säkerhetsenheter som hello sista steget, innan informationen skickas toohello Internet.
* Skyddat nätverk:
  * Det finns ingen direkt väg från hello Internet toohello grundläggande infrastruktur.
  * Kanaler toohello grundläggande infrastruktur måste passera via säkerhetsenheter som NSG: er, brandväggar eller VPN-enheter.
  * Andra enheter måste inte överbrygga Internet och hello grundläggande infrastruktur.
  * Säkerhetsenheter på både hello mot Internet och hello skyddade nätverk mot gränserna för hello perimeternätverk (till exempel hello två brandväggen ikoner hello föregående bild) faktiskt kan en enda virtuell installation med differentierade regler eller gränssnitt för varje gräns. Till exempel en fysisk enhet, logiskt avgränsade hantering hello belastning för båda gränserna för hello perimeternätverk.
* Andra vanliga metoder och begränsningar:
  * Arbetsbelastningar måste inte lagra viktig affärsinformation.
  * Åtkomst och uppdateringar tooperimeter nätverkskonfigurationer och distributioner är begränsad tooonly behörighet administratörer.

### <a name="perimeter-network-requirements"></a>Perimeternätverk nätverkskrav
tooenable dessa egenskaper följer dessa riktlinjer för virtuellt nätverk krav tooimplement en lyckad perimeternätverk:

* **Arkitektur för undernätet:** ange hello virtuella nätverket så att en hela undernätet dedicerade som hello perimeternätverk avskild från andra undernät i hello samma virtuella nätverk. Den här separationen innebär hello trafik mellan hello perimeternätverket och andra interna eller privata undernät nivåer flöden via en brandvägg eller en virtuell installation av ID/IP-adresser.  Användardefinierade vägar på hello gräns undernät är obligatoriska tooforward denna trafik toohello virtuell installation.
* **NSG:** hello perimeter undernät själva ska vara öppen tooallow kommunikation med hello Internet, men som innebär inte kunder ska kringgås NSG: er. Följ vanliga säkerhet praxis toominimize hello nätverk visas på portalen innehåller toohello Internet. Lås hello remote adressintervall tillåtna tooaccess hello distributioner eller hello programprotokoll och portar som är öppna. Det kan finnas omständigheter, men som en fullständig låsning inte är möjligt. Till exempel om kunder har en extern webbplats i Azure, hello perimeternätverk ska tillåta hello inkommande webbegäranden från offentliga IP-adresser, men bara öppna hello web application portar: TCP på port 80 och/eller TCP på port 443.
* **Routningstabellen:** hello perimeter undernät själva bör vara kan toocommunicate toohello Internet direkt, men bör inte tillåta direkt kommunikation tooand från hello tillbaka slutet eller lokalt nätverk utan att gå via en brandvägg eller postsäkerhet.
* **Installation av säkerhetskonfiguration:** tooroute och inspektera paket mellan hello perimeternätverket och hello resten av hello skyddat nätverk hello säkerhetsenheter som brandvägg, ID: N och IP-adresser enheter kan vara multihomed. De kan ha separata nätverkskort för hello perimeternätverket och hello backend-undernät. hello nätverkskort i perimeternätverket hello kommunicera direkt tooand från hello Internet, med hello motsvarande NSG: er och hello perimeter nätverks routningstabellen. hello nätverkskort ansluter toohello backend-undernät har mer begränsade NSG: er och routningstabeller hello motsvarande backend-undernät.
* **Säkerhet enhetens funktioner:** hello säkerhetsenheter som distribueras i perimeternätverket hello vanligtvis utför hello följande funktioner:
  * Brandvägg: Framtvinga brandväggsregler eller principer för åtkomstkontroll för hello inkommande begäranden.
  * Hot upptäcka och förhindra: identifiera och minska skadliga attacker från hello Internet.
  * Granskning och loggning: underhålla detaljerade loggar för granskning och analys.
  * Omvänd proxy: omdirigera hello inkommande begäranden toohello motsvarande backend-servrar. Den här omdirigering innebär mappning och översätta hello måladresser för hello frontend-enheter, vanligtvis brandväggar toohello backend-serveradresser.
  * Vidarebefordra proxy: tillhandahåller NAT och utföra granskning för startas från hello virtuellt nätverk toohello Internet-kommunikation.
  * Router: Vidarebefordrar inkommande och mellan undernät trafik i hello virtuella nätverket.
  * VPN-enhet: fungerar som hello mellan lokala VPN-gatewayer för flera lokala VPN-anslutningen mellan kundens lokala nätverk och virtuella Azure-nätverk.
  * VPN-servern: acceptera VPN-klienter som ansluter tooAzure virtuella nätverk.

> [!TIP]
> Behåll hello följande två grupper separata: hello enskilda användare behörighet tooaccess hello perimeter nätverket säkerhet växel och hello enskilda användare behörighet som administratörer för utveckling, distribution eller drift. Avgränsa dessa grupper gör det möjligt för en ansvarsfördelning och förhindrar att en enskild person kringgå både program och säkerhetskontroller i nätverket.
>
>

### <a name="questions-toobe-asked-when-building-network-boundaries"></a>Toobe frågor och svar när du skapar nätverkets gränser
I det här avsnittet om du inte uttryckligen nämns hello termen ”nätverk” refererar tooprivate Azure virtuella nätverk som skapats av en administratör för prenumerationen. hello termen refererar inte toohello underliggande fysiska nätverk i Azure.

Virtuella Azure-nätverk är också ofta använda tooextend traditionella lokala nätverk. Det är möjligt tooincorporate plats-till-plats eller ExpressRoute hybridnätverk lösningar med nätverksarkitekturer i perimeternätverket. Den här länken hybrid är viktigt i att skapa nätverk säkerhetsgränser.

hello är följande tre frågor kritiska tooanswer när du utvecklar ett nätverk med ett perimeternätverk och flera säkerhetsgränser.

#### <a name="1-how-many-boundaries-are-needed"></a>1) hur många gränser behövs?
hello är första beslut toodecide hur många säkerhetsgränser behövs i ett visst scenario:

* En enda gräns: en på hello frontend perimeternätverk, mellan hello virtuella nätverket och hello Internet.
* Två gränser: en på hello Internet sida av hello perimeternätverket och en annan mellan hello perimeter-undernät och hello backend-undernät i hello virtuella Azure-nätverk.
* Tre gränser: en längs hello Internet hello perimeternätverk, mellan hello perimeternätverket och backend-undernät och ett mellan hello backend-undernät och hello lokalt nätverk.
* N gränser: en variabel nummer. Beroende på säkerhetskrav finns det ingen gräns toohello säkerhetsgränser som kan användas i ett givet nätverk.

hello baserat antalet och typen av gränser som krävs varierar på företagets risk feltolerans och hello situation som genomförs. Det här beslutet görs ofta tillsammans med flera grupper inom en organisation, inklusive ofta ett team för risk och efterlevnad, ett nätverk och plattform team och en programutvecklingsteamet. Personer med kunskap om säkerhet, hello data ingår och hello tekniker som används bör ha en Säg i det här beslutet tooensure hello säkerhetsbehörighet behöver för varje implementering.

> [!TIP]
> Använda hello minsta antal gränser som uppfyller hello säkerhetskraven för en viss situation. Med flera gränser, kan åtgärder och felsökning vara svårare, samt hello management resursåtgång hantera hello flera principer för gräns över tid. Otillräcklig gränserna ökar dock risken. Hitta hello saldot är viktigt.
>
>

![6]

hello visas föregående bild en övergripande bild av ett tre säkerhet gräns nätverk. hello gränser är mellan hello perimeternätverket och hello Internet, hello Azure frontend och backend-privat undernät, och hello Azure backend-undernät och hello lokalt företagets nätverk.

#### <a name="2-where-are-hello-boundaries-located"></a>2) där finns hello gränser?
När hello antal gränser är valt, där hello tooimplement dem är nästa Beslutspunkt. Det finns vanligtvis tre alternativ:

* Med hjälp av en Internet-baserade mellanhandstjänster (till exempel en molnbaserad Brandvägg för webbaserade program, som inte beskrivs i det här dokumentet)
* Med inbyggda funktioner och/eller nätverket virtuella installationer i Azure
* Med hjälp av fysiska enheter på hello lokalt nätverk

Hello alternativ är intern Azure-funktioner (till exempel Azure belastningsutjämnare) på rent Azure nätverk eller virtuella nätverksinstallationer från hello omfattande partner ekosystemet Azure (till exempel brandväggar för Check Point).

Om en gräns krävs mellan Azure och ett lokalt nätverk kan hello säkerhetsenheter finnas på endera sidan av hello-anslutning (eller båda sidorna). Därför måste ett beslut fattas på hello plats tooplace säkerhet växeln.

I föregående bild hello hello Internet-anslutningar mellan två perimeternätverk hello framför-till-backend-gränser som ingår helt i Azure och måste vara intern Azure-funktioner eller virtuella nätverksenheter. Säkerhetsenheter på hello gränsen mellan Azure (backend-undernät) och hello företagsnätverk kan vara antingen på hello Azure sida eller hello lokalt på klientsidan eller med en kombination av enheter på båda sidor. Det kan finnas betydande fördelar och nackdelar tooeither alternativ som måste beaktas allvarligt.

Till exempel med befintlig fysisk säkerhet växel på hello lokalt nätverk på klientsidan har hello fördel att inga nya växeln krävs. Den måste bara omkonfiguration. hello Nackdelen är dock att all trafik måste komma tillbaka från Azure toohello lokalt nätverk toobe ses av hello säkerhet växeln. Därmed Azure-Azure-trafik kan innebära betydande fördröjning och påverkar programmets prestanda och användarupplevelse, om det tvingades tillbaka toohello lokalt nätverk för tvingande säkerhetsprinciper.

#### <a name="3-how-are-hello-boundaries-implemented"></a>3) hur implementeras hello gränser?
Varje säkerhetsgräns troligen har olika krav (t.ex, ID: N och brandväggsregler på hello Internet sida av hello perimeternätverk, men endast ACL: er mellan hello perimeternätverk och backend-undernät). Besluta om vilken enhet (eller hur många enheter) toouse beror på hello scenario och säkerhetskrav. I följande avsnitt hello, beskriver exempel 1, 2 och 3 vissa alternativ som kan användas. Granska hello Azure interna nätverksfunktioner och hello-enheter som är tillgängliga i Azure från hello partner ekosystemet visar hello många olika alternativ tillgängliga toosolve valfri scenario.

En annan är nyckel implementering beslut hur tooconnect hello lokalt nätverk med Azure. Bör du använda hello Azure virtuella gateway eller en virtuell nätverksenhet? Dessa alternativ beskrivs mer detaljerat i hello efter avsnittet (exempel 4, 5 och 6).

Dessutom kan kan trafik mellan virtuella nätverk i Azure behövas. Dessa scenarier läggs i hello framtida.

När du väl vet svaren hello toohello föregående frågor hello [snabb Start](#fast-start) avsnitt kan hjälpa dig att identifiera vilka exempel är mest lämpliga för ett visst scenario.

## <a name="examples-building-security-boundaries-with-azure-virtual-networks"></a>Exempel: Skapa säkerhetsgränser med virtuella Azure-nätverk
### <a name="example-1-build-a-perimeter-network-toohelp-protect-applications-with-nsgs"></a>Exempel 1 bygga en perimeter-nätverk toohelp skydda program med NSG: er
[Säkerhetskopiera tooFast start](#fast-start) | [detaljerad skapa instruktioner för det här exemplet][Example1]

[![7]][7]

#### <a name="environment-description"></a>Beskrivning av miljö
I det här exemplet finns det en prenumeration som innehåller hello följande resurser:

- En enskild resursgrupp
- Ett virtuellt nätverk med två undernät: ”FrontEnd” och ”BackEnd”
- En Nätverkssäkerhetsgrupp som är kopplade tooboth undernät
- En Windows-server som representerar en program-webbserver (”IIS01”)
- Två Windows-servrar som representerar backend-programservrar (”AppVM01”, ”AppVM02”)
- En Windows-server som representerar en DNS-server (”DNS01”)
- En offentlig IP-adress som är kopplade till webbservern för hello program

Skript och en Azure Resource Manager-mall finns hello [detaljerade instruktioner build][Example1].

#### <a name="nsg-description"></a>NSG-beskrivning
I det här exemplet bygger en NSG-grupp och sedan in med sex regler.

> [!TIP]
> Generellt sett du bör skapa specifika ”Tillåt” reglerna först, följt av generisk hello ”neka” regler. hello prioriteras avgör vilka regler som utvärderas först. När trafiken hittas tooapply tooa specifik regel, utvärderas inga fler regler. NSG-regler kan tillämpas i antingen hello inkommande eller utgående riktning (ur hello hello undernät).
>
>

Deklarativt, byggs hello följande regler för inkommande trafik:

1. Intern DNS-trafik (port 53) tillåts.
2. RDP-trafik (port 3389) från hello Internet tooany virtuella tillåts.
3. HTTP-trafik (port 80) från hello tooweb Internetserver (IIS01) tillåts.
4. All trafik (alla portar) från IIS01 tooAppVM1 tillåts.
5. All trafik (alla portar) från hello Internet toohello hela virtuella nätverk (båda undernäten) nekas.
6. All trafik (alla portar) från hello frontend undernät toohello backend-undernät nekas.

Med dessa regler bundna tooeach undernät, om en HTTP-begäran var inkommande trafik från hello webbserver för Internet toohello, både regler 3 (Tillåt) och 5 (neka) skulle tillämpas. Men eftersom regel 3 har högre prioritet kan bara den skulle tillämpas och regel 5 inte skulle komma till användning. Hello HTTP-begäran skulle därför tillåtna toohello webbservern. Om samma trafiken har tooreach hello DNS01 server, regel 5 (neka) skulle vara hello första tooapply och hello trafik inte skulle toopass toohello server. Regel 6 (neka) block hello frontend undernät från pratar toohello backend-undernät (förutom tillåten trafik i regler 1 och 4). Den här regeluppsättningen skyddar hello backend-nätverket om en angripare komprometterar hello webbprogram på hello klientdelen. hello angripare skulle har begränsad åtkomst toohello ”skyddad” serverdelsnätverk (endast tooresources som visas på hello AppVM01 server).

Det finns en utgående Standardregeln som tillåter trafik ut toohello Internet. I det här exemplet vi att tillåta utgående trafik och inte ändra de utgående reglerna. toolock ned i båda riktningarna användardefinierade routning krävs (se exempel 3).

#### <a name="conclusion"></a>Slutsats
Det här exemplet är ett relativt enkla och enkelt sätt att isolera hello backend-undernät från inkommande trafik. Mer information finns i hello [detaljerade instruktioner build][Example1]. De här anvisningarna inkluderar:

* Hur toobuild denna perimeter nätverk med klassiska PowerShell-skript.
* Hur toobuild denna perimeter nätverk med en Azure Resource Manager-mall.
* Detaljerade beskrivningar av varje NSG-kommando.
* Detaljerad trafik flödet scenarier visar hur trafik tillåts eller nekas i varje lager.


### <a name="example-2-build-a-perimeter-network-toohelp-protect-applications-with-a-firewall-and-nsgs"></a>Exempel 2 bygga en perimeter-nätverk toohelp skydda program med en brandvägg och NSG: er
[Säkerhetskopiera tooFast start](#fast-start) | [detaljerad skapa instruktioner för det här exemplet][Example2]

[![8]][8]

#### <a name="environment-description"></a>Beskrivning av miljö
I det här exemplet finns det en prenumeration som innehåller hello följande resurser:

* En enskild resursgrupp
* Ett virtuellt nätverk med två undernät: ”FrontEnd” och ”BackEnd”
* En Nätverkssäkerhetsgrupp som är kopplade tooboth undernät
* En virtuell nätverksenhet, i det här fallet en brandvägg, anslutna toohello frontend undernät
* En Windows-server som representerar en program-webbserver (”IIS01”)
* Två Windows-servrar som representerar backend-programservrar (”AppVM01”, ”AppVM02”)
* En Windows-server som representerar en DNS-server (”DNS01”)

Skript och en Azure Resource Manager-mall finns hello [detaljerade instruktioner build][Example2].

#### <a name="nsg-description"></a>NSG-beskrivning
I det här exemplet bygger en NSG-grupp och sedan in med sex regler.

> [!TIP]
> Generellt sett du bör skapa specifika ”Tillåt” reglerna först, följt av generisk hello ”neka” regler. hello prioriteras avgör vilka regler som utvärderas först. När trafiken hittas tooapply tooa specifik regel, utvärderas inga fler regler. NSG-regler kan tillämpas i antingen hello inkommande eller utgående riktning (ur hello hello undernät).
>
>

Deklarativt, byggs hello följande regler för inkommande trafik:

1. Intern DNS-trafik (port 53) tillåts.
2. RDP-trafik (port 3389) från hello Internet tooany virtuella tillåts.
3. Alla Internet-trafik (alla portar) toohello virtuell nätverksenhet (brandvägg) tillåts.
4. All trafik (alla portar) från IIS01 tooAppVM1 tillåts.
5. All trafik (alla portar) från hello Internet toohello hela virtuella nätverk (båda undernäten) nekas.
6. All trafik (alla portar) från hello frontend undernät toohello backend-undernät nekas.

Med dessa regler bundna tooeach undernät, om en HTTP-begäran var inkommande trafik från hello toohello Brandvägg för Internet-både regler 3 (Tillåt) och 5 (neka) skulle tillämpas. Men eftersom regel 3 har högre prioritet kan bara den skulle tillämpas och regel 5 inte skulle komma till användning. Hello HTTP-begäran skulle därför tillåtna toohello brandväggen. Om samma trafiken har tooreach hello IIS01 server, även om den är i hello frontend undernät, regel 5 (neka) är skulle ha använts, och hello trafik skulle inte toopass toohello server. Regel 6 (neka) block hello frontend undernät från pratar toohello backend-undernät (förutom tillåten trafik i regler 1 och 4). Den här regeluppsättningen skyddar hello backend-nätverket om en angripare komprometterar hello webbprogram på hello klientdelen. hello angripare skulle har begränsad åtkomst toohello ”skyddad” serverdelsnätverk (endast tooresources som visas på hello AppVM01 server).

Det finns en utgående Standardregeln som tillåter trafik ut toohello Internet. I det här exemplet vi att tillåta utgående trafik och inte ändra de utgående reglerna. toolock ned i båda riktningarna användardefinierade routning krävs (se exempel 3).

#### <a name="firewall-rule-description"></a>Beskrivning av regel för brandvägg
Hello Brandvägg för ska vidarebefordran av regler skapas. Eftersom det här exemplet endast vägar Internet-trafik inkommande toohello brandväggen och sedan toohello webbservern, bara en vidarebefordran network address translation (NAT) behövs regeln.

hello vidarebefordran regel godkänner alla inkommande källadress som träffar hello brandväggen försök tooreach HTTP (port 80 eller 443 för HTTPS). Det har skickas utanför hello brandväggen lokala gränssnittet och omdirigerad toohello webbserver med hello 10.0.1.5 IP-adress.

#### <a name="conclusion"></a>Slutsats
Det här exemplet är ett relativt enkelt sätt att skydda ditt program med en brandvägg och isolera hello backend-undernät från inkommande trafik. Mer information finns i hello [detaljerade instruktioner build][Example2]. De här anvisningarna inkluderar:

* Hur toobuild denna perimeter nätverk med klassiska PowerShell-skript.
* Hur toobuild det här exemplet med en Azure Resource Manager-mall.
* Detaljerade beskrivningar av varje NSG kommandot och brandväggen regel.
* Detaljerad trafik flödet scenarier visar hur trafik tillåts eller nekas i varje lager.

### <a name="example-3-build-a-perimeter-network-toohelp-protect-networks-with-a-firewall-and-udr-and-nsg"></a>Exempel 3 bygga en perimeter-nätverk toohelp skydda nätverk med en brandvägg och UDR och NSG
[Säkerhetskopiera tooFast start](#fast-start) | [detaljerad skapa instruktioner för det här exemplet][Example3]

[![9]][9]

#### <a name="environment-description"></a>Beskrivning av miljö
I det här exemplet finns det en prenumeration som innehåller hello följande resurser:

* En enskild resursgrupp
* Ett virtuellt nätverk med tre undernät: ”SecNet”, ”FrontEnd” och ”BackEnd”
* En virtuell nätverksenhet, i det här fallet en brandvägg, anslutna toohello SecNet undernät
* En Windows-server som representerar en program-webbserver (”IIS01”)
* Två Windows-servrar som representerar backend-programservrar (”AppVM01”, ”AppVM02”)
* En Windows-server som representerar en DNS-server (”DNS01”)

Skript och en Azure Resource Manager-mall finns hello [detaljerade instruktioner build][Example3].

#### <a name="udr-description"></a>UDR beskrivning
Som standard definieras hello följande systemvägar som:

        Effective routes :
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

Hej VNETLocal är alltid en eller flera definierade adressprefix som utgör hello virtuellt nätverk för det specifika nätverket (det vill säga ändras från virtuellt nätverk toovirtual nätverket, beroende på hur varje specifik virtuellt nätverk har definierats). hello återstående systemvägar är statiska och standard som anges i hello tabell.

I det här exemplet skapas två routningstabeller en vardera för hello frontend och backend-undernät. Varje tabell har lästs in med statiska vägar som är lämpliga för hello angivna undernätet. I det här exemplet har varje tabell tre vägar som dirigera om all trafik (0.0.0.0/0) via hello brandvägg (nästa hopp = virtuell installation IP-adress):

1. Lokal undernätstrafik med ingen nästa hopp definierats tooallow undernätet trafik toobypass hello brandväggen.
2. Trafik i virtuella nätverk med en nästa hopp har definierats som brandväggen. Den här nästa hopp åsidosätter hello Standardregeln som tillåter lokala virtuella nätverket trafik tooroute direkt.
3. Alla återstående trafik (0/0) med en nästa hopp har definierats som hello brandväggen.

> [!TIP]
> Har inte hello undernätet posten i hello UDR radbrytningar undernätet kommunikation.
>
> * I vårt exempel är 10.0.1.0/24 pekar tooVNETLocal viktigt! Utan den misslyckas paket lämnar hello Web Server (10.0.1.4) avsedda tooanother lokala servern (till exempel) 10.0.1.25 som de ska skickas toohello NVA. hello NVA skickar den toohello undernät och undernät hello skicka det toohello NVA i en oändlig loop.
> * hello risken för en routningsslinga är vanligtvis högre på installationer med flera nätverkskort som är anslutna tooseparate undernät, vilket ofta är av traditionell, lokala installationer.
>
>

När hello routningstabeller skapas, måste de vara bundna tootheir undernät. Hej frontend undernät routningstabellen, skapas en gång och bundna toohello undernät, som ser ut som den här utdata:

        Effective routes :
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active

> [!NOTE]
> UDR kan nu vara tillämpade toohello gateway-undernät på vilka hello ExpressRoute-kretsen är ansluten.
>
> Exempel på hur tooenable ditt perimeternätverk nätverk med ExpressRoute eller plats-till-plats-nätverk som visas i exempel 3 och 4.
>
>

#### <a name="ip-forwarding-description"></a>Beskrivning av IP-vidarebefordran
IP-vidarebefordran är en tillhörande funktionen tooUDR. IP-vidarebefordran är en inställning på en virtuell installation som gör att det tooreceive trafik som inte uttryckligen åtgärdas toohello installation och vidarebefordra trafik tooits ultimate destinationen.

Om AppVM01 gör en begäran toohello DNS01 server, skulle UDR vidarebefordra den här trafiken toohello brandväggen. Med IP-vidarebefordran aktiverad, hello trafiken för hello DNS01 mål (10.0.2.4) accepteras av hello-enhet (10.0.0.4) och vidarebefordras tooits slutdestinationen (10.0.2.4). Utan IP-vidarebefordran aktiverat hello brandväggen, skulle trafik inte godkännas av hello installation även om hello routningstabellen har hello brandväggen som hello nästa hopp. toouse en virtuell installation är kritiska tooremember tooenable IP-vidarebefordring tillsammans med UDR.

#### <a name="nsg-description"></a>NSG-beskrivning
I det här exemplet bygger en NSG-grupp och sedan in med en enskild regel. Den här gruppen är sedan bunden bara toohello frontend och backend-undernät (inte hello SecNet). Deklarativt skapas hello följande regel:

* All trafik (alla portar) från hello Internet toohello hela virtuella nätverk (alla undernät) nekas.

Även om NSG: er som används i det här exemplet är dess huvudsakliga syfte som en sekundär försvarslinje mot manuell felaktig konfiguration. hello målet är tooblock all inkommande trafik från hello Internet tooeither hello frontend- eller undernät. Trafiken den bör endast hello SecNet undernät toohello brandväggen (och sedan, om det är lämpligt i toohello frontend- eller undernät). Dessutom med hello UDR regler och skulle all trafik som gjorde det till hello frontend- eller undernät dirigeras ut toohello brandvägg (tack tooUDR). hello brandväggen ser den här trafiken som en asymmetrisk dataflöde och skulle sjunka hello utgående trafik. Det finns därför tre säkerhetsnivåer skydda hello undernät:

* Inga offentliga IP-adresser på alla FrontEnd och BackEnd-nätverkskort.
* NSG: er att neka trafik från hello Internet.
* hello brandväggen släppa asymmetriska trafik.

En intressant avseende hello NSG i det här exemplet är att den innehåller endast en regel som är toodeny Internet-trafik toohello hela virtuellt nätverk, inklusive hello säkerhet undernät. Men eftersom hello NSG är endast bunden toohello frontend och backend-undernät, hello regeln inte bearbetas på trafik inkommande toohello säkerhet undernät. Därför flödar trafiken toohello säkerhet undernät.

#### <a name="firewall-rules"></a>Brandväggsregler
Hello Brandvägg för ska vidarebefordran av regler skapas. Eftersom hello-brandväggen blockerar eller vidarebefordran alla inkommande, utgående och inom virtuella nätverkstrafik, behövs många brandväggsregler. Dessutom all inkommande trafik träffar hello Security Service offentlig IP-adress (på olika portar) toobe bearbetas av hello brandväggen. Ett bra tips är toodiagram hello logiska flöden innan du konfigurerar hello undernät och brandväggsregler, tooavoid omarbeta senare. hello är följande bild en logisk vy för hello brandväggsregler för det här exemplet:

![10]

> [!NOTE]
> Baserat på hello virtuell nätverksenhet används variera hello hanteringsportar. I det här exemplet refereras Barracuda nästa generation brandvägg, som använder port 22, 801 och 807. Läs hello installation leverantörens dokumentation toofind hello exakt portarna som används för hantering av hello enhet som används.
>
>

#### <a name="firewall-rules-description"></a>Brandväggen regler beskrivning
I hello föregående logiskt diagram, visas hello säkerhet undernät inte eftersom hello brandväggen hello endast resurs i undernätet. hello diagram visas hello brandväggsregler och hur de logiskt tillåta eller neka trafikflöden, inte hello faktiska routade sökväg. Dessutom hello externa portar som valts för hello RDP-trafik är högre låg portar (8014 – 8026) och har markerat tooloosely överensstämmer med hello senast två oktetterna i hello lokal IP-adress för att lättare att läsa (exempelvis lokala serveradress 10.0.1.4 associeras med externa-port 8014). Högre icke motstridig portar, men kan användas.

I det här exemplet behöver vi sju typer av regler:

* Externa regler (för inkommande trafik):
  1. Hantering av brandväggsregel: den här appen omdirigera regeln tillåter trafik toopass toohello hanteringsportar för hello virtuell nätverksenhet.
  2. RDP-regler (för varje Windows server): dessa fyra regler (en för varje server) kan hanteringen av hello enskilda servrar via RDP. också kan vara dolda hello fyra RDP-regler i en regel, beroende på hello funktionerna i hello virtuell nätverksenhet som används.
  3. Regler för nätverkstrafik för programmet: det finns två av dessa regler, hello först för hello frontwebbservern trafik och hello andra för hello backend-trafik (till exempel web server toodata nivån). hello konfigurationen av dessa regler är beroende av hello nätverksarkitekturen (där servrarna placeras) och trafikflöden (vilken riktning hello trafiken flödar och vilka portar som används).
     * hello första regeln låter hello faktiska trafik tooreach hello programmet programservern. Hello andra regler som tillåter för säkerhet och hantering, är regler för nätverkstrafik i programmet vad tillåta externa användare eller tjänster tooaccess hello program. I det här exemplet att det finns en enda webbserver på port 80. Därför dirigerar ett program för en enda brandväggsregel inkommande trafik toohello extern IP, toohello web servrar interna IP-adress. hello omdirigerad trafik session skulle översättas via NAT toohello interna servern.
     * hello andra regeln är hello backend-regel tooallow hello server tootalk toohello AppVM01 webbservern (men inte AppVM02) via alla portar.
* Interna regler (för trafik i inom virtuella nätverk)
  1. Regel för utgående tooInternet: den här regeln tillåter trafik från alla nätverk toopass toohello markerat nätverk. Den här regeln är vanligtvis en standardregel som redan finns på hello brandväggen, men i ett inaktiverat tillstånd. Den här regeln ska aktiveras för det här exemplet.
  2. DNS-regel: den här regeln kan bara DNS (port 53) trafik toopass toohello DNS-server. De flesta trafik från klientdelen hello toohello serverdel blockeras för den här miljön. Den här regeln kan särskilt DNS från alla lokala undernätet.
  3. Undernät toosubnet regel: den här regeln är tooallow alla servrar på backend-undernät hello tooconnect tooany server i hello frontend undernät (men inte hello omvänd).
* Felsäker regel (för trafik som inte uppfyller något av föregående hello):
  1. Neka alla trafikregel: neka-regeln ska alltid vara hello sista regeln (vad gäller prioritet) och som sådan om ett trafikflöde misslyckas toomatch något av föregående regler hello tas den bort av den här regeln. Den här regeln är en standardregel och vanligtvis på plats och vara aktiv. Inga ändringar är vanligtvis behövs toothis regeln.

> [!TIP]
> Hello tillåts andra program trafik regeln toosimplify det här exemplet är alla portar. I ett verkligt scenario ska hello mest specifika porten och adressen adressintervall använda tooreduce hello risken för angrepp på den här regeln.
>
>

När hello tidigare regler skapas, är det viktigt tooreview hello prioritet för varje regel tooensure trafik tillåts eller nekas efter behov. I det här exemplet är hello i prioritetsordning.

#### <a name="conclusion"></a>Slutsats
Det här exemplet är en mer komplex men slutföra sätt att skydda och isolera hello nätverket än hello föregående exempel. (Exempel 2 skyddar bara hello program och exempel 1 isolerar bara undernät). Den här designen kan för övervakning i båda riktningarna och skyddar inte bara hello som inkommande programserver men tillämpar nätverkets säkerhetsprincip för alla servrar på nätverket. Dessutom beroende på hello-enhet som används för kan fullständig trafik gransknings- och medvetenhet uppnås. Mer information finns i hello [detaljerade instruktioner build][Example3]. De här anvisningarna inkluderar:

* Hur toobuild det här exemplet perimeter nätverk med klassiska PowerShell-skript.
* Hur toobuild det här exemplet med en Azure Resource Manager-mall.
* Detaljerade beskrivningar av varje UDR NSG kommandot och brandväggsregel.
* Detaljerad trafik flödet scenarier visar hur trafik tillåts eller nekas i varje lager.

### <a name="example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn"></a>Exempel 4 lägga till en hybridanslutning med en plats-till-plats, virtuella installation VPN
[Säkerhetskopiera tooFast start](#fast-start) | Detaljerade build instruktioner tillgänglig snart

[![11]][11]

#### <a name="environment-description"></a>Beskrivning av miljö
Hybridnätverk med hjälp av en virtuell nätverksenhet (NVA) kan läggas till tooany av hello perimeter nätverkstyper som beskrivs i exempel 1, 2 eller 3.

I hello föregående bild visas är en VPN-anslutning via hello Internet (plats-till-plats) används tooconnect ett lokalt nätverk tooan via en NVA virtuella Azure-nätverket.

> [!NOTE]
> Om du använder ExpressRoute med hello Azure offentlig Peering-alternativ är aktiverat kan ska en statisk väg skapas. Den här statiska vägen ska vidarebefordra toohello NVA VPN IP-adress i ditt företags Internet och inte via hello ExpressRoute-anslutning. hello NAT krävs på hello ExpressRoute Azure offentlig Peering alternativet bryta hello VPN-sessionen.
>
>

När hello VPN är på plats, hello NVA blir hello central knutpunkt för alla nätverk och undernät. hello vidarebefordran brandväggsreglerna bestämmer vilken trafik som flödar tillåts översätts via NAT omdirigeras eller tas bort (även för trafikflödet mellan hello lokala nätverk och Azure).

Trafikflöden bör beaktas noggrant, eftersom de kan optimeras eller försämrad av det här designmönstret beroende på hello specifika användningsfall.

Använder inbyggd i exempel 3 hello-miljö och sedan lägga till en plats-till-plats VPN-hybrid nätverksanslutning, ger hello följande design:

[![12]][12]

hello lokala router eller annan nätverksenhet som är kompatibel med din NVA för VPN, skulle vara hello VPN-klienten. Fysiska enheten ska ansvara för att initiera och underhålla hello VPN-anslutning med din NVA.

Logiskt toohello NVA hello nätverket ser ut som fyra separata ”säkerhetszoner” med hello regler på hello NVA som hello primära chef för trafik mellan dessa zoner:

![13]

#### <a name="conclusion"></a>Slutsats
hello lägga till en plats-till-plats VPN-hybrid nätverk anslutning tooan virtuella Azure-nätverket kan utöka hello lokala nätverk till Azure på ett säkert sätt. Med en VPN-anslutning trafiken krypteras och skickar via hello Internet. Hej NVA i det här exemplet innehåller en central plats tooenforce och hantera hello säkerhetsprincip. Mer information finns i hello detaljerad skapa instruktioner (kommande). De här anvisningarna inkluderar:

* Hur toobuild det här exemplet perimeter nätverk med PowerShell-skript.
* Hur toobuild det här exemplet med en Azure Resource Manager-mall.
* Detaljerad trafik flödet scenarier visar hur trafiken flödar via den här designen.

### <a name="example-5-add-a-hybrid-connection-with-a-site-to-site-azure-vpn-gateway"></a>Exempel 5 lägga till en hybridanslutning med en plats-till-plats, Azure VPN-gateway
[Säkerhetskopiera tooFast start](#fast-start) | Detaljerade build instruktioner tillgänglig snart

[![14]][14]

#### <a name="environment-description"></a>Beskrivning av miljö
Hybridnätverk med hjälp av en Azure VPN-gateway kan läggas till tooeither perimeternätverk nätverkstyp som beskrivs i exempel 1 eller 2.

I hello föregående bild visas är en VPN-anslutning via hello Internet (plats-till-plats) används tooconnect ett lokalt nätverk tooan Azure-nätverk via en Azure VPN-gateway.

> [!NOTE]
> Om du använder ExpressRoute med hello Azure offentlig Peering-alternativ är aktiverat kan ska en statisk väg skapas. Den här statiska vägen ska vidarebefordra toohello NVA VPN IP-adress i ditt företags Internet och inte via hello ExpressRoute WAN. hello NAT krävs på hello ExpressRoute Azure offentlig Peering alternativet bryta hello VPN-sessionen.
>
>

hello följande bild visar hello två nätverk kanter i det här exemplet. På första kant hello hello NVA och NSG: er styr trafikflöde för inom Azure-nätverk och mellan Azure och hello Internet. hello andra kant är hello Azure VPN-gateway, vilket är en separat och isolerade nätverk kant mellan lokala och Azure.

Trafikflöden bör beaktas noggrant, eftersom de kan optimeras eller försämrad av det här designmönstret beroende på hello specifika användningsfall.

Använder hello-miljö som är inbyggd i exempel 1, och sedan lägga till en plats-till-plats VPN-hybrid nätverksanslutning, ger hello följande design:

[![15]][15]

#### <a name="conclusion"></a>Slutsats
hello lägga till en plats-till-plats VPN-hybrid nätverk anslutning tooan virtuella Azure-nätverket kan utöka hello lokala nätverk till Azure på ett säkert sätt. Använder hello interna Azure VPN-gateway, trafiken är IPSec-krypterad och dirigerar via hello Internet. Dessutom ger hello Azure VPN-gateway en lägre kostnad alternativet (inga ytterligare licenser kostnad som NVAs från tredje part). Det här alternativet är mest ekonomiska i exempel 1, där ingen NVA används. Mer information finns i hello detaljerad skapa instruktioner (kommande). De här anvisningarna inkluderar:

* Hur toobuild det här exemplet perimeter nätverk med PowerShell-skript.
* Hur toobuild det här exemplet med en Azure Resource Manager-mall.
* Detaljerad trafik flödet scenarier visar hur trafiken flödar via den här designen.

### <a name="example-6-add-a-hybrid-connection-with-expressroute"></a>Exempel 6 Lägg till en hybridanslutning med ExpressRoute
[Säkerhetskopiera tooFast start](#fast-start) | Detaljerade build instruktioner tillgänglig snart

[![16]][16]

#### <a name="environment-description"></a>Beskrivning av miljö
Hybridnätverk med hjälp av en privat peering anslutning kan vara ExpressRoute lägga tooeither perimeternätverk nätverkstyp som beskrivs i exempel 1 eller 2.

I hello föregående bild visas privat peering i ExpressRoute innehåller en direkt anslutning mellan ditt lokala nätverk och hello virtuella Azure-nätverket. Trafik eltransit endast hello service provider nätverk och hello Microsoft Azure-nätverk, aldrig vidrör hello Internet.

> [!TIP]
> Med hjälp av ExpressRoute för att hålla företagets nätverkstrafik av hello Internet. Dessutom kan serviceavtal från ExpressRoute-providern. hello Azure Gateway kan skicka in too10 Gbit/s med ExpressRoute, med plats-till-plats VPN hello Azure Gateway maximalt dataflöde är 200 Mbit/s.
>
>

Som det visas i följande diagram hello med det här alternativet hello har miljö nu två nätverk kanter. Hej NVA och NSG kontrollen trafikflöden för inom Azure-nätverk och mellan Azure och hello Internet, medan hello gateway är en separat och isolerade nätverk kant mellan lokala och Azure.

Trafikflöden bör beaktas noggrant, eftersom de kan optimeras eller försämrad av det här designmönstret beroende på hello specifika användningsfall.

Använder hello-miljö som är inbyggd i exempel 1, och sedan lägga till en ExpressRoute hybrid nätverksanslutning, ger hello följande design:

[![17]][17]

#### <a name="conclusion"></a>Slutsats
hello lägga till en privat ExpressRoute-Peering-nätverksanslutning utöka hello lokala nätverk till Azure i en säker, lägre latens, högre utför sätt. Med hjälp av hello tillhandahåller interna Azure Gateway, som i följande exempel också en lägre kostnad alternativ (ingen ytterligare licensiering som NVAs från tredje part). Mer information finns i hello detaljerad skapa instruktioner (kommande). De här anvisningarna inkluderar:

* Hur toobuild det här exemplet perimeter nätverk med PowerShell-skript.
* Hur toobuild det här exemplet med en Azure Resource Manager-mall.
* Detaljerad trafik flödet scenarier visar hur trafiken flödar via den här designen.

## <a name="references"></a>Referenser
### <a name="helpful-websites-and-documentation"></a>Användbara webbplatser och dokumentation
* Åtkomst till Azure med Azure Resource Manager:
* Åtkomst till Azure med PowerShell: [https://docs.microsoft.com/powershell/azureps-cmdlets-docs/](/powershell/azure/overview)
* Virtuella nätverk dokumentation: [https://docs.microsoft.com/azure/virtual-network/](https://docs.microsoft.com/azure/virtual-network/)
* Network security group-dokumentationen: [https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg](virtual-network/virtual-networks-nsg.md)
* Användardefinierade routning dokumentation: [https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview](virtual-network/virtual-networks-udr-overview.md)
* Azure virtuella gateways: [https://docs.microsoft.com/azure/vpn-gateway/](https://docs.microsoft.com/azure/vpn-gateway/)
* Plats-till-plats-VPN: [https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell](vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* ExpressRoute dokumentation (vara säker på att toocheck ut hello ”komma igång” och ”How To” avsnitt): [https://docs.microsoft.com/azure/expressroute/](https://docs.microsoft.com/azure/expressroute/)

<!--Image References-->
[0]: ./media/best-practices-network-security/flowchart.png "Flödesschema för säkerhetsalternativ"
[2]: ./media/best-practices-network-security/azuresecurityfeatures.png "azure säkerhetsfunktioner"
[3]: ./media/best-practices-network-security/dmzcorporate.png "A DMZ i ett företagsnätverk"
[4]: ./media/best-practices-network-security/azuresecurityarchitecture.png "azure säkerhetsarkitekturen"
[5]: ./media/best-practices-network-security/dmzazure.png "en DMZ i Azure-nätverk"
[6]: ./media/best-practices-network-security/dmzhybrid.png "hybrid nätverk med tre säkerhetsgränser"
[7]: ./media/best-practices-network-security/example1design.png "Inkommande DMZ med NSG"
[8]: ./media/best-practices-network-security/example2design.png "Inkommande DMZ med NVA och NSG"
[9]: ./media/best-practices-network-security/example3design.png "Dubbelriktad DMZ med NVA, NSG och UDR"
[10]: ./media/best-practices-network-security/example3firewalllogical.png "logisk vy för hello brandväggsregler"
[11]: ./media/best-practices-network-security/example3designoptions.png "Hybrid är anslutna till DMZ med NVA"
[12]: ./media/best-practices-network-security/example4designs2s.png "DMZ med NVA som är anslutna via ett plats-till-plats-VPN"
[13]: ./media/best-practices-network-security/example4networklogical.png "logiskt nätverk från NVA perspektiv"
[14]: ./media/best-practices-network-security/example5designoptions.png "Plats-till-plats Hybrid är anslutna till DMZ med Azure-Gateway"
[15]: ./media/best-practices-network-security/example5designs2s.png "DMZ med Azure med hjälp av plats-till-plats VPN-Gateway"
[16]: ./media/best-practices-network-security/example6designoptions.png "ExpressRoute-Hybrid är anslutna till DMZ med Azure-Gateway"
[17]: ./media/best-practices-network-security/example6designexpressroute.png "DMZ med Azure-Gateway med hjälp av en ExpressRoute-anslutning"

<!--Link References-->
[TrustCenter]: https://azure.microsoft.com/support/trust-center/compliance/
[Example1]: ./virtual-network/virtual-networks-dmz-nsg.md
[Example2]: ./virtual-network/virtual-networks-dmz-nsg-fw-asm.md
[Example3]: ./virtual-network/virtual-networks-dmz-nsg-fw-udr-asm.md
[Example4]: ./virtual-network/virtual-networks-hybrid-s2s-nva-asm.md
[Example5]: ./virtual-network/virtual-networks-hybrid-s2s-agw-asm.md
[Example6]: ./virtual-network/virtual-networks-hybrid-expressroute-asm.md
[Example7]: ./virtual-network/virtual-networks-vnet2vnet-direct-asm.md
[Example8]: ./virtual-network/virtual-networks-vnet2vnet-transit-asm.md
