---
title: "Azure Traffic Manager – vanliga frågor och svar | Microsoft Docs"
description: "Den här artikeln innehåller svar på vanliga frågor och svar om Traffic Manager"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 75d5ff9a-f4b9-4b05-af32-700e7bdfea5a
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/15/2017
ms.author: kumud
ms.openlocfilehash: 44762864e0a5adf568fcd4928b48661196f05b9e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="traffic-manager-frequently-asked-questions-faq"></a>Traffic Manager vanliga frågor (FAQ)

## <a name="traffic-manager-basics"></a>Traffic Manager-grunderna

### <a name="what-ip-address-does-traffic-manager-use"></a>Vilken IP-adress använder Traffic Manager?

Enligt beskrivningen i [så Traffic Manager fungerar](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager fungerar på DNS-nivå. Skickar den DNS-svar för att dirigera klienterna till lämplig tjänstslutpunkten. Klienter ansluter sedan till tjänstslutpunkten direkt, inte via Trafikhanterarprofilen.

Traffic Manager ger därför inte en slutpunkt eller IP-adress för klienter att ansluta till. Därför, om du vill använda statiska IP-adressen för din tjänst som måste konfigureras på tjänsten, inte i Trafikhanterarprofilen.

### <a name="does-traffic-manager-support-sticky-sessions"></a>Stöder Traffic Manager-Fäst' sessioner?

Enligt beskrivningen i [så Traffic Manager fungerar](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager fungerar på DNS-nivå. DNS-svar används för att dirigera klienterna till lämplig tjänstslutpunkten. Klienter som ansluter till tjänstslutpunkten direkt, inte via Trafikhanterarprofilen. Traffic Manager finns därför inte HTTP-trafik mellan klienten och servern.

Dessutom tillhör käll-IP-adressen för DNS-fråga tas emot av Traffic Manager rekursiv DNS-tjänsten inte klienten. Därför Traffic Manager har inget sätt att spåra enskilda klienter och kan inte implementera 'Fäst-sessioner. Den här begränsningen är gemensamma för alla system för DNS-baserad trafik och är inte specifik för Traffic Manager.

### <a name="why-am-i-seeing-an-http-error-when-using-traffic-manager"></a>Varför ser jag ett HTTP-fel när du använder Traffic Manager?

Enligt beskrivningen i [så Traffic Manager fungerar](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager fungerar på DNS-nivå. DNS-svar används för att dirigera klienterna till lämplig tjänstslutpunkten. Klienter ansluter sedan till tjänstslutpunkten direkt, inte via Trafikhanterarprofilen. Traffic Manager stöder inte se HTTP-trafik mellan klienten och servern. Alla HTTP-felet som du ser måste därför kommer från ditt program. Alla DNS-matchning steg har slutförts för klienten att ansluta till programmet. Som innehåller någon interaktion med Traffic Manager på trafikflöde program.

Ytterligare undersökning bör därför fokusera på programmet.

Värdadressen HTTP skickas från klientens webbläsare är de vanligaste källan till problemet. Kontrollera att programmet är konfigurerad för att acceptera rätt värdadressen för det domännamn som du använder. Slutpunkter som använder Azure App Service finns [konfigurera ett anpassat domännamn för en webbapp i Azure App Service med Traffic Manager](../app-service-web/web-sites-traffic-manager-custom-domain-name.md).

### <a name="what-is-the-performance-impact-of-using-traffic-manager"></a>Vad är prestandapåverkan med Traffic Manager?

Enligt beskrivningen i [så Traffic Manager fungerar](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager fungerar på DNS-nivå. Eftersom klienter ansluta direkt till dina Tjänsteslutpunkter, påverkas inte prestanda som uppstår när du använder Traffic Manager när anslutningen har upprättats.

Eftersom Traffic Manager integrerar med program på DNS-nivå, kräver den en ytterligare DNS-sökning som ska infogas i DNS-matchning av kedjan. Effekten av Traffic Manager på DNS-matchningstid är minimal. Traffic Manager använder ett globalt nätverk av namnservrar och använder [anycast](https://en.wikipedia.org/wiki/Anycast) nätverk för att säkerställa DNS frågor alltid dirigeras till närmaste tillgängliga namnservern. Dessutom innebär cachelagring av DNS-svar att ytterligare DNS-latensen med Traffic Manager gäller endast en del av sessioner.

Metoden prestanda dirigerar trafik till närmaste tillgängliga slutpunkten. Resultatet är att prestandapåverkan som är associerade med den här metoden bör vara minimal. En ökning i DNS-svarstid bör att förskjutas av lägre nätverksfördröjning till slutpunkten.

### <a name="what-application-protocols-can-i-use-with-traffic-manager"></a>Vilka programprotokoll kan du använda med Traffic Manager?

Enligt beskrivningen i [så Traffic Manager fungerar](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager fungerar på DNS-nivå. När DNS-sökning är klar kan ansluter klienterna till slutpunkten programmet direkt, inte via Trafikhanterarprofilen. Anslutningen kan därför använda alla protokoll. Om du väljer TCP som det övervakning-protokollet, Traffic Manager endpoint hälsoövervakning kan göras utan att använda alla protokoll på programnivå. Om du vill ha hälsotillståndet verifieras med ett programprotokoll måste slutpunkten kunna svara på begäranden för HTTP eller HTTPS hämta.

### <a name="can-i-use-traffic-manager-with-a-naked-domain-name"></a>Kan jag använda Traffic Manager med ett 'asbestgaller' domännamn?

Nej. DNS-standarden tillåter inte att skapa CNAME-poster samexisterar med andra DNS-poster med samma namn. En DNS-zon apex (eller rot) innehåller alltid två befintlig DNS-poster. SOA- och auktoritativa NS-poster. Detta innebär en CNAME-post inte kan skapas på zonens apex utan brott mot DNS-standarden.

Traffic Manager kräver en DNS CNAME-post för att mappa alternativa DNS-namn. Exempelvis kan du mappa www.contoso.com till Traffic Manager profil DNS-namnet contoso.trafficmanager.net. Dessutom returnerar trafikhanterarprofilen ett andra DNS CNAME för att ange vilken slutpunkt som klienten ska ansluta till.

Om du vill undvika det här problemet bör du använda en HTTP-omdirigering till trafiken från asbestgaller domännamnet till en annan URL, som sedan kan använda Traffic Manager. Asbestgaller domänen contoso.com kan till exempel omdirigera användare till CNAME ”www.contoso.com” som pekar på Traffic Manager-DNS-namn.

Fullständigt stöd för asbestgaller domäner i Traffic Manager spåras i vår funktionen eftersläpning. Du kan registrera ditt stöd för den här funktionsbegäran av [röstning för den på vår community feedbackwebbplats](https://feedback.azure.com/forums/217313-networking/suggestions/5485350-support-apex-naked-domains-more-seamlessly).

### <a name="does-traffic-manager-consider-the-client-subnet-address-when-handling-dns-queries"></a>Traffic Manager betraktar undernätsadress klienten vid hantering av DNS-frågor? 
Nej, för närvarande Traffic Manager anser endast källa IP-adressen för DNS-frågan tas emot som vanligtvis IP-adressen för DNS-matchning när utföra sökningar för routningsmetoder för geografiska och prestanda.  
Mer specifikt [RFC 7871 – klientnät i DNS-frågor](https://tools.ietf.org/html/rfc7871) som ger en [tillägget mekanism för DNS (EDNS0)](https://tools.ietf.org/html/rfc2671) som kan överföra på klienten undernätsadress från matchare som stöder detta till DNS-servrar är för närvarande stöds inte i Traffic Manager. Du kan registrera ditt stöd för den här funktionsbegäran via vårt [community feedbackwebbplats](https://feedback.azure.com/forums/217313-networking).


### <a name="what-is-dns-ttl-and-how-does-it-impact-my-users"></a>Vad är DNS TTL och hur den påverkar Mina användare?

När en DNS-fråga hamnar på Traffic Manager, anger ett värde i svaret kallas time to live (TTL). Det här värdet, vars enheten är i sekunder, anger hur länge till DNS-matchare nedströms på att cachelagra svaret. När DNS-matchare inte är säkert att cachelagra resultatet, gör cachelagringen det enkelt att besvara efterföljande frågor av cacheminnet i stället för att gå till Traffic Manager-DNS-servrar. Detta påverkar svaren på följande sätt:
- en högre TTL minskar antalet frågor som hamna på Traffic Manager-DNS-servrar som kan minska kostnaden för en kund eftersom antalet frågor som hanteras är en fakturerbar användning.
- ett högre TTL-värde kan minska den tid det tar för att göra en DNS-sökning.
- ett högre TTL-värde innebär också att dina data inte återspeglar den senaste hälsoinformation Traffic Manager har erhållits via dess avsöknings agenter.

### <a name="how-high-or-low-can-i-set-the-ttl-for-traffic-manager-responses"></a>Hur högt eller lågt anger jag TTL för Traffic Manager svar?

Du kan ange på en per profil nivå, DNS TTL vara så lågt som 0 sekunder och lika hög som 2 147 483 647 sekunder (största intervall som är kompatibla med [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt )). TTL-värdet 0 innebär att den underordnade DNS-matchare cachelagrar inte svar på frågor och alla frågor som förväntas nå Traffic Manager-DNS-servrar för namnmatchning.

## <a name="traffic-manager-geographic-traffic-routing-method"></a>Traffic Manager geografiska trafikroutningsmetod

### <a name="what-are-some-use-cases-where-geographic-routing-is-useful"></a>Vilka är några användningsområden där geografiska routning är användbar? 
Geografisk routning kan användas i scenarier där en Azure-kund behöver skilja användarna baserat på geografiska områden. Till exempel kan använder geografiska trafikroutningsmetod, du ge användare från vissa regioner en annan användarupplevelse än de från andra regioner. Ett annat exempel uppfyller lokala data suveränitet uppdrag som kräver att användare från en viss region hanteras endast med slutpunkter i den regionen.

### <a name="what-are-the-regions-that-are-supported-by-traffic-manager-for-geographic-routing"></a>Vilka är de regioner som stöds av Traffic Manager för geografisk Routning? 
Det land/region som används av Traffic Manager kan hittas [här](traffic-manager-geographic-regions.md). Medan den här sidan hålls uppdaterade med ändringar, kan du också programmässigt hämta samma information med hjälp av den [Azure Traffic Manager REST API](https://docs.microsoft.com/rest/api/trafficmanager/). 

### <a name="how-does-traffic-manager-determine-where-a-user-is-querying-from"></a>Hur traffic manager kontrollerar där en användare efterfrågar från? 
Traffic Manager tittar på käll-IP för frågan (det troligen är en lokal DNS-matchare gör frågor för användarens räkning) och använder en intern IP-adress till region karta för att avgöra var. Den här kartan uppdateras kontinuerligt för ändringar i internet. 

### <a name="is-it-guaranteed-that-traffic-manager-can-correctly-determine-the-exact-geographic-location-of-the-user-in-every-case"></a>Det garanteras att Traffic Manager korrekt kan bestämma den exakta geografiska platsen för användaren i samtliga fall?
Nej, Traffic Manager kan inte garantera att den geografiska region som vi härleda från käll-IP-adressen för en DNS-fråga alltid motsvarar användarens plats på grund av följande skäl: 

- Först enligt beskrivningen i föregående vanliga frågor och svar är källans IP-adress som visas en DNS-matchare gör sökning för användarens räkning. Den geografiska platsen för DNS-matchning är en bra proxy för den geografiska platsen för användaren, kan det också vara olika beroende på storleken på tjänsten DNS-matchare och specifika DNS-matchare tjänsten en kund har valt att använda. Exempelvis kan en kund i Malaysia sin enhet inställningarna används ange ett DNS-lösartjänsten vars DNS-servern i Singapore kan få ut för att hantera frågan lösningar för att användarenhet. I så fall kan Traffic Manager bara se den matcharen IP-adress som motsvarar Singapore platsen. Se även tidigare vanliga frågor om stöd för klienter undernät adress på den här sidan.

- Traffic Manager använder dessutom en intern mappning för att utföra geografiska region översättning IP-adressen. Den här kartan verifieras och uppdateras kontinuerligt att öka noggrannheten för dess och kontot för den växande strukturen på internet, men det finns fortfarande möjlighet att vår information inte är en exakt kopia av den geografiska platsen där alla IP-Adressen adresser.


###  <a name="does-an-endpoint-need-to-be-physically-located-in-the-same-region-as-the-one-it-is-configured-with-for-geographic-routing"></a>Behöver en slutpunkt fysiskt finnas i samma region som den är konfigurerad med för geografisk Routning? 
Nej, platsen för slutpunkten medför inga begränsningar som regioner kan mappas till den. Exempelvis kan en slutpunkt i USA, Central Azure-region har alla användare från Indien dirigeras till den.

### <a name="can-i-assign-geographic-regions-to-endpoints-in-a-profile-that-is-not-configured-to-do-geographic-routing"></a>Kan jag tilldelar geografiska områden slutpunkter i en profil som inte är konfigurerad för att göra geografiska Routning? 

Ja, om routningsmetod för en profil inte geografiska, du kan använda den [Azure Traffic Manager REST API](https://docs.microsoft.com/rest/api/trafficmanager/) tilldela geografiska områden till slutpunkterna i den här profilen. Icke-geografiska routning typen profiler kan ignoreras den här konfigurationen. Om du ändrar en sådan profil till geografiska routning vid ett senare tillfälle kan Traffic Manager använda mappningarna.


### <a name="why-am-i-getting-an-error-when-i-try-to-change-the-routing-method-of-an-existing-profile-to-geographic"></a>Varför får ett fel när jag försöker ändra routningsmetod för en befintlig profil till geografiska?

Alla slutpunkter under en profil med geografiska routning måste ha minst en region som mappats till den. Om du vill konvertera en befintlig profil till geografiska routning, måste du först associera geografiska områden till dess slutpunkter som använder den [Azure Traffic Manager REST API](https://docs.microsoft.com/rest/api/trafficmanager/) innan du ändrar typen routning till geografiska. Om du använder portalen först tar bort slutpunkterna, ändra routningsmetod för profilen till geografiska och sedan lägga till slutpunkter tillsammans med deras geografiska region-mappning. 


###  <a name="why-is-it-strongly-recommended-that-customers-create-nested-profiles-instead-of-endpoints-under-a-profile-with-geographic-routing-enabled"></a>Varför det rekommenderas starkt att kunderna skapa kapslade profiler i stället för slutpunkter med en profil med geografiska routning aktiverad? 

En region kan tilldelas till endast en slutpunkt i en profil för om dess med geografiska routning. Om den slutpunkten inte är en kapslad typ med en underordnad profil som är kopplat till den, om den slutpunkt som ska feltillstånd, trafik Manager fortsätter att skicka trafik till den sedan med alternativet att inte skicka all trafik som inte är alla bättre. Traffic Manager har inte växling till en annan slutpunkt, även om den region som tilldelats är ”överordnad” för den region som tilldelats den slutpunkt som gått feltillstånd (till exempel om en slutpunkt som har region Spanien blir ohälsosamt vi kommer inte gå över till en annan slutpunkt med den region som Europa tilldelade). Detta görs för att säkerställa att Traffic Manager respekterar geografisk gränser att en kund har installationen i profilen. För att få förmånen vid fel till en annan slutpunkt när en slutpunkt är inte felfri, rekommenderas det att geografiska områden ska tilldelas kapslade profiler med flera slutpunkter inom det i stället för enskilda slutpunkter. På så sätt kan trafik växling till en annan slutpunkt i samma kapslad underordnad profil om en slutpunkt i profilen för kapslad underordnad misslyckas.

### <a name="are-there-any-restrictions-on-the-api-version-that-supports-this-routing-type"></a>Finns det några begränsningar på API-version som stöder den här Routning?

Ja, endast API-versionen 2017-03-01 och senare stöder geografiska routning skriver. Alla äldre API-versioner kan inte användas för att skapade profiler för geografisk routning eller tilldela slutpunkter geografiska områden. Om ett äldre API-versionen används för att hämta profiler från en Azure-prenumeration, returneras inte någon profil geografiska routning. Dessutom när du använder äldre API-versioner, returnerade en profil som har slutpunkter med en geografisk region tilldelning, har inte dess tilldelning för geografiska region som visas.



## <a name="traffic-manager-endpoints"></a>Traffic Manager-slutpunkter

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Kan jag använda Traffic Manager med slutpunkter från flera prenumerationer?

Det går inte att använda slutpunkter från flera prenumerationer med Azure Web Apps. Azure Web Apps kräver att alla anpassade domännamn som används med Web Apps används endast inom en enda prenumeration. Det går inte att använda Web Apps från flera prenumerationer med samma domännamn.

För andra typer av slutpunkter är det möjligt att använda Traffic Manager med slutpunkter från mer än en prenumeration. I Resource Manager från någon prenumeration kan att lägga till slutpunkter i Traffic Manager så länge den person som konfigurerar Traffic Manager-profilen har läsbehörighet till slutpunkten. Dessa behörigheter kan beviljas med [Azure Resource Manager rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-control-configure.md).


### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>Kan jag använda Traffic Manager med Molntjänsten ”Staging” platser?

Ja. Molntjänsten mellanlagring distributionsplatser kan konfigureras i Traffic Manager som externa slutpunkter. Hälsokontroller debiteras fortfarande i Azure-slutpunkter takt. Eftersom den externa slutpunkten används, fångas ändringar i underliggande inte upp automatiskt. Med externa slutpunkter kan inte Traffic Manager identifiera när Molntjänsten har stoppats eller tagits bort. Traffic Manager fortsätter därför fakturering hälsokontroller tills slutpunkten är inaktiverad eller borttagen.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>Traffic Manager har stöd för IPv6-slutpunkter?

Traffic Manager ger inte någon IPv6-addressible namnservrar för närvarande. Traffic Manager kan dock fortfarande användas av IPv6-klienter som ansluter till IPv6-slutpunkter. En klient gör inte DNS-begäranden direkt till Trafikhanterarprofilen. I stället använder klienten en rekursiv DNS-tjänsten. En endast-IPv6-klient skickar begäranden till den rekursiva DNS-tjänsten via IPv6. Sedan ska rekursiv tjänsten kunna kontakta Traffic Manager-namnservrar med IPv4.

Traffic Manager svarar med DNS-namnet på slutpunkten. En DNS AAAA-post som pekar på DNS-namnet på slutpunkten på IPv6-adressen måste finnas för att stödja en IPv6-slutpunkt. Traffic Manager-hälsokontroller stöder endast IPv4-adresser. Tjänsten måste exponera en IPv4-slutpunkt på samma DNS-namn.

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region"></a>Kan jag använda Traffic Manager för mer än en Webbapp i samma region?

Traffic Manager används vanligtvis för att dirigera trafik till program som distribueras i olika regioner. Men kan det också användas om ett program har mer än en distribution i samma region. Azure Traffic Manager-slutpunkter tillåter inte mer än en slutpunkt för webb-App från samma Azure-region som ska läggas till samma Traffic Manager-profilen.

##  <a name="traffic-manager-endpoint-monitoring"></a>Slutpunktsövervakning av Traffic Manager

### <a name="is-traffic-manager-resilient-to-azure-region-failures"></a>Är Traffic Manager motståndskraftiga mot Azure-region fel?

Traffic Manager är en viktig del av leverans av hög tillgänglighet program i Azure.
För att ge hög tillgänglighet måste Traffic Manager har en ovanligt hög tillgänglighet och vara motståndskraftiga mot regionala fel.

Traffic Manager-komponenter är motståndskraftiga mot ett fullständigt fel av någon Azure-region avsiktligt. Den här återhämtning gäller alla Traffic Manager-komponenter: DNS-namn-servrar, API: et, lagringsskiktet och slutpunkten övervakningstjänsten.

I osannolika ett avbrott i en hela Azure-region förväntas Traffic Manager fungerar normalt. Program som distribueras i flera Azure-regioner kan förlita sig på Traffic Manager att dirigera trafik till en tillgänglig instans av sina program.

### <a name="how-does-the-choice-of-resource-group-location-affect-traffic-manager"></a>Hur påverkar valet av resursgruppens plats Traffic Manager?

Traffic Manager är en global tjänst. Det är inte regional. Valet av resursgruppens plats spelar ingen roll till Traffic Manager-profiler som distribuerats i resursgruppen.

Azure Resource Manager kräver att alla resursgrupper att ange en plats, som anger standardplatsen för resurser har distribuerats i resursgruppen. När du skapar en Traffic Manager-profil skapas den i en resursgrupp. Alla Traffic Manager-profiler som använder **globala** som deras plats, åsidosätta standardvärdet resurs grupp.

### <a name="how-do-i-determine-the-current-health-of-each-endpoint"></a>Hur tar jag reda på det aktuella hälsotillståndet för varje slutpunkt?

Den aktuella övervakning statusen för varje slutpunkt, utöver den övergripande profilen visas i Azure-portalen. Den här informationen är också tillgängligt via trafik övervakaren [REST API](https://msdn.microsoft.com/library/azure/mt163667.aspx), [PowerShell-cmdlets](https://msdn.microsoft.com/library/mt125941.aspx), och [plattformsoberoende Azure CLI](../cli-install-nodejs.md).

Azure tillhandahåller inte historisk information om den senaste slutpunkten hälsotillstånd eller möjligheten att aktivera aviseringar om ändringar för slutpunkten hälsa.

### <a name="can-i-monitor-https-endpoints"></a>Övervakar jag HTTPS-slutpunkter

Ja. Traffic Manager har stöd för sökning via HTTPS. Konfigurera **HTTPS** som protokoll i konfigurationen av övervakningen.

Traffic manager kan inte ange några certifikatvalidering inklusive:

* Serversidan certifikat verifieras inte
* SNI serversidan certifikat stöds inte
* Klientcertifikat stöds inte

### <a name="can-i-use-traffic-manager-even-if-my-application-does-not-have-support-for-http-or-https"></a>Kan jag använda Traffic Manager även om mitt program inte har stöd för HTTP eller HTTPS?

Ja. Du kan ange TCP som protokoll övervakning och Traffic Manager kan initiera en TCP-anslutning och vänta på svar från slutpunkten. Om slutpunkten svarar anslutningsbegäran med ett svar för att upprätta en anslutning inom den angivna tiden markeras den slutpunkten som felfri.

### <a name="what-specific-responses-are-required-from-the-endpoint-when-using-tcp-monitoring"></a>Vilka specifika svar krävs slutpunkten när du använder TCP övervakning?

När TCP-övervakning används startar Traffic Manager en trevägs TCP handskakning genom att skicka en begäran om SYN till slutpunkt på den angivna porten. Det väntar sedan en viss tidsperiod (som anges i timeout-inställningar) på ett svar från slutpunkten. Om slutpunkten svarar på SYN begäran med ett SYN ACK-svar inom tidsgränsen som anges i inställningarna för övervakning, sedan anses denna slutpunkt felfritt. Om SYN ACK-svar tas emot, återställer anslutningen genom att svara igen med en RST Traffic Manager.

### <a name="how-fast-does-traffic-manager-move-my-users-away-from-an-unhealthy-endpoint"></a>Hur snabbt flytta mina användare från en ohälsosamt slutpunkt i Traffic Manager?

Traffic Manager innehåller flera inställningar som kan hjälpa dig att styra redundans Traffic Manager-profilen på följande sätt:
- Du kan ange att Traffic Manager-avsökningar slutpunkterna oftare genom att ställa in intervall för sökning efter 10 sekunder. Detta säkerställer att alla slutpunkten ska feltillstånd kan identifieras så snart som möjligt. 
- Du kan ange hur länge ska vänta innan ett hälsotillstånd kolla begäran gånger (lägsta timeout-värdet är 5 sek).
- Du kan ange hur många fel kan uppstå innan slutpunkten markeras som ohälsosam. Det här värdet kan vara låg som 0, där fallet slutpunkten är markerat feltillstånd när den första hälsokontrollen misslyckas. Med det lägsta värdet 0 för tillåten antalet fel kan dock leda till slutpunkter som ska tas bort från rotationen på grund av tillfälliga problem som kan uppstå vid tidpunkten för sökning.
- Du kan ange den time to live (TTL) för DNS-svaret ska vara så lågt som 0. Om du gör det innebär att DNS-matchare kan inte cachelagrar svaret, och varje ny fråga hämtar ett svar som innehåller den allra senaste informationen för hälsotillstånd med Traffic Manager.

Med hjälp av de här inställningarna ge Traffic Manager redundans under 10 sekunder efter att en slutpunkt blir ohälsosamt och en DNS-fråga görs mot motsvarande profil.

### <a name="how-can-i-specify-different-monitoring-settings-for-different-endpoints-in-a-profile"></a>Hur kan jag ange olika övervakningsinställningarna för olika slutpunkter i en profil?

Traffic Manager övervakningsinställningarna finns på en per profil nivå. Om du behöver använda en annan övervakning inställning för endast en slutpunkt kan också göra det genom att låta den slutpunkten som en [kapslad profil](traffic-manager-nested-profiles.md) vars övervakningsinställningarna skiljer sig från den överordnade profilen.

### <a name="what-host-header-do-endpoint-health-checks-use"></a>Vilka värden huvud gör endpoint kontroll används?

Traffic Manager använder värdhuvuden hälsokontroller för HTTP och HTTPS. Värdadressen används av Traffic Manager är namnet på slutpunkten mål som konfigurerats i profilen. Det värde som används i värdhuvudet kan inte anges separat från egenskapen target.

### <a name="what-are-the-ip-addresses-from-which-the-health-checks-originate"></a>Vilka är de IP-adresser som kontrollerar hälsotillståndet kommer?

Följande lista innehåller IP-adresser som kontrollerar hälsa för Traffic Manager kan kommer. Du kan använda den här listan så att inkommande anslutningar från dessa IP-adresser tillåts på slutpunkterna kontrollera dess hälsostatus.

* 40.68.30.66
* 40.68.31.178
* 137.135.80.149
* 137.135.82.249
* 23.96.236.252
* 65.52.217.19
* 40.87.147.10
* 40.87.151.34
* 13.75.124.254
* 13.75.127.63
* 52.172.155.168
* 52.172.158.37
* 104.215.91.84
* 13.75.153.124
* 13.84.222.37
* 23.101.191.199
* 23.96.213.12
* 137.135.46.163
* 137.135.47.215
* 191.232.208.52
* 191.232.214.62
* 13.75.152.253
* 104.41.187.209
* 104.41.190.203

### <a name="how-many-health-checks-to-my-endpoint-can-i-expect-from-traffic-manager"></a>Hur många hälsokontroller till min slutpunkten ges från Traffic Manager?

Antalet Traffic Manager hälsa kontrollerar nå slutpunkten beror på följande:
- Det värde som du har angett för Övervakningsintervall (mindre intervall innebär fler begäranden hamnar på din slutpunkt i en given tidsperiod).
- antalet platser från där hälsotillståndet kontrollerar kommer (IP-adresser från där du kan förvänta dig kontrollerna visas i föregående FAQ).

## <a name="traffic-manager-nested-profiles"></a>Traffic Manager kapslade profiler

### <a name="how-do-i-configure-nested-profiles"></a>Hur konfigurerar kapslade profiler?

Kapslade Traffic Manager-profiler kan konfigureras med både Azure Resource Manager och den klassiska Azure REST API: er, Azure PowerShell-cmdlets och plattformsoberoende Azure CLI-kommandona. De stöds också via den nya Azure-portalen. De stöds inte i den klassiska portalen.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>Stöd för hur många lager med kapsling har Traffic Manager?

Du kan kapsla profiler upp till 10 nivåer. 'Slinga' tillåts inte.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-the-same-traffic-manager-profile"></a>Kan jag blanda andra typer av slutpunkter med kapslad underordnad profiler i samma Traffic Manager-profilen?

Ja. Det finns inga begränsningar för hur du kombinerar slutpunkter av olika typer i en profil.

### <a name="how-does-the-billing-model-apply-for-nested-profiles"></a>Hur gäller vilken faktureringsmodell som tillämpas för kapslade profiler?

Det är inte negativt priser effekten av att använda kapslade profiler.

Traffic Manager fakturering består av två komponenter: slutpunkt hälsokontroller och miljontals DNS-frågor

* Slutpunkten hälsokontroller: är gratis för en underordnad profil när konfigurerad som en slutpunkt i en överordnad-profil. Övervakning av slutpunkter i profilen för underordnade faktureras som vanligt.
* DNS-frågor: varje fråga räknas endast en gång. En fråga till en överordnad-profil som returnerar en slutpunkt från en underordnad profil är räknas av mot den överordnade profilen.

Fullständig information finns i [Traffic Manager sida med priser](https://azure.microsoft.com/pricing/details/traffic-manager/).

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>Finns det en prestandapåverkan för kapslade profiler?

Nej. Det finns ingen inverkan på prestanda när med hjälp av kapslade profiler.

Traffic Manager-namnservrar passerar internt profil hierarkin vid bearbetning av varje DNS-fråga. En DNS-fråga till en överordnad-profil kan ta emot ett DNS-svar till en slutpunkt från en underordnad profil. En CNAME-post används om du använder en enskild profil eller kapslade profiler. Det finns inget behov av att skapa en CNAME-post för varje profil i hierarkin.

### <a name="how-does-traffic-manager-compute-the-health-of-a-nested-endpoint-in-a-parent-profile"></a>Hur beräkna hälsotillståndet för en kapslad slutpunkt i en överordnad profil i Traffic Manager?

Den överordnade profilen utföra inte hälsokontroller på underordnade direkt. Hälsotillståndet för den underordnade profilen slutpunkter används i stället för att beräkna den övergripande hälsan för den underordnade profilen. Den här informationen sprids uppåt i hierarkin för kapslade profiler fastställa hälsotillståndet för den kapslade slutpunkten. Överordnad profilen använder denna sammanställda hälsa för att fastställa om trafiken dirigeras till underordnat.

I följande tabell beskrivs beteendet för Traffic Manager hälsa söker efter en kapslad slutpunkt.

| Status för underordnade profilen Övervakare | Överordnad Övervakare för slutpunkten status | Anteckningar |
| --- | --- | --- |
| Inaktiverad. Den underordnade profilen har inaktiverats. |Stoppad |Det överordnade slutpunktstillståndet stoppas inte har inaktiverats. Inaktiverat tillstånd är reserverat för som anger att du har inaktiverat slutpunkten i profilen för överordnade. |
| Försämrad. Minst en underordnad-profilens slutpunkt har statusen degraderad. |Online: antalet Online slutpunkter i profilen för underordnade är minst värdet för MinChildEndpoints.<BR>CheckingEndpoint: antalet Online plus CheckingEndpoint slutpunkter i profilen för underordnade är minst värdet för MinChildEndpoints.<BR>Försämrad: annars. |Trafik dirigeras till en slutpunkt av status CheckingEndpoint. Om MinChildEndpoints är för högt kan försämras alltid slutpunkten. |
| Online. Minst en underordnad-profilens slutpunkt är ett onlinetillstånd. Ingen slutpunkt har statusen degraderad. |Se ovan. | |
| CheckingEndpoints. Minst en underordnad-profilens slutpunkt är 'CheckingEndpoint'. Inga slutpunkter är 'Online' eller 'Försämrad' |Samma som ovan. | |
| Inaktiva. Alla underordnade profilen slutpunkterna är inaktiverad eller stoppad eller den här profilen har inga slutpunkter. |Stoppad | |

## <a name="next-steps"></a>Nästa steg:
- Lär dig mer om Traffic Manager [endpoint övervaknings- och automatisk redundans](../traffic-manager/traffic-manager-monitoring.md).
- Lär dig mer om Traffic Manager [trafikroutningsmetoder](../traffic-manager/traffic-manager-routing-methods.md).