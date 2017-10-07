---
title: "aaaAzure Traffic Manager – vanliga frågor och svar | Microsoft Docs"
description: "Den här artikeln innehåller svar toofrequently frågor och svar om Traffic Manager"
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
ms.openlocfilehash: 5530d03b55bbf49a52002841e281e2cf5819c2b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="traffic-manager-frequently-asked-questions-faq"></a>Traffic Manager vanliga frågor (FAQ)

## <a name="traffic-manager-basics"></a>Traffic Manager-grunderna

### <a name="what-ip-address-does-traffic-manager-use"></a>Vilken IP-adress använder Traffic Manager?

Enligt beskrivningen i [så Traffic Manager fungerar](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager fungerar på hello DNS-nivå. DNS-svar skickas toodirect klienter toohello lämplig tjänst slutpunkt. Klienter sedan ansluta toohello tjänstslutpunkten direkt, inte via Traffic Manager.

Därför tillhandahåller Traffic Manager inte en slutpunkt eller IP-adressen för klienter tooconnect till. Därför, om du vill använda statiska IP-adressen för din tjänst som måste konfigureras på hello-tjänsten, inte i Trafikhanterarprofilen.

### <a name="does-traffic-manager-support-sticky-sessions"></a>Stöder Traffic Manager-Fäst' sessioner?

Enligt beskrivningen i [så Traffic Manager fungerar](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager fungerar på hello DNS-nivå. Den använder DNS-svar toodirect klienter toohello lämplig tjänstslutpunkten. Klienterna ansluter toohello tjänstslutpunkten direkt, inte via Trafikhanterarprofilen. Traffic Manager finns därför inte hello HTTP-trafik mellan hello klient- och hello.

Dessutom tillhör hello käll-IP-adressen för hello DNS-fråga tas emot av Traffic Manager toohello rekursiva DNS-tjänsten inte hello-klienten. Därför Traffic Manager har inga sätt tootrack enskilda klienter och kan inte implementera 'Fäst-sessioner. Den här begränsningen är vanliga tooall DNS-trafik hanteringssystem och är inte specifik tooTraffic Manager.

### <a name="why-am-i-seeing-an-http-error-when-using-traffic-manager"></a>Varför ser jag ett HTTP-fel när du använder Traffic Manager?

Enligt beskrivningen i [så Traffic Manager fungerar](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager fungerar på hello DNS-nivå. Den använder DNS-svar toodirect klienter toohello lämplig tjänstslutpunkten. Klienter sedan ansluta toohello tjänstslutpunkten direkt, inte via Traffic Manager. Traffic Manager stöder inte se HTTP-trafik mellan klienten och servern. Alla HTTP-felet som du ser måste därför kommer från ditt program. För hello tooconnect toohello klientprogrammet har alla DNS-Lösningssteg slutförts. Som innehåller någon interaktion med Traffic Manager på hello trafikflöde för programmet.

Ytterligare undersökning bör därför fokusera på programmet hello.

hello HTTP värdadressen skickas från hello klientens webbläsare är hello vanligaste källan till problemet. Se till att programmet hello är konfigurerade tooaccept hello rätt värdhuvud för hello domännamn som du använder. Slutpunkter som använder hello Azure App Service, finns [konfigurera ett anpassat domännamn för en webbapp i Azure App Service med Traffic Manager](../app-service-web/web-sites-traffic-manager-custom-domain-name.md).

### <a name="what-is-hello-performance-impact-of-using-traffic-manager"></a>Vad är hello prestandapåverkan med Traffic Manager?

Enligt beskrivningen i [så Traffic Manager fungerar](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager fungerar på hello DNS-nivå. Eftersom klienter ansluter direkt tooyour slutpunkter, påverkas inte prestanda som uppstår när du använder Traffic Manager när hello anslutningen har upprättats.

Eftersom Traffic Manager kan integreras med program på hello DNS-nivå, kräver den en ytterligare DNS-sökning toobe infogas i hello DNS-matchning av kedjan. hello inverkan av Traffic Manager på DNS-matchningstid är minimal. Traffic Manager använder ett globalt nätverk av namnservrar och använder [anycast](https://en.wikipedia.org/wiki/Anycast) nätverk tooensure DNS-frågor är alltid dirigeras toohello närmaste tillgängliga namnserver. Dessutom innebär cachelagring av DNS-svar att hello ytterligare DNS-latensen med Traffic Manager gäller endast tooa andel av sessioner.

hello prestanda metoden vägar trafik toohello närmaste tillgängliga slutpunkt. hello slutresultatet är att hello inverkan på övergripande prestanda som är associerade med den här metoden bör vara minimal. En ökning i DNS-svarstid bör att förskjutas av lägre latens toohello nätverksslutpunkten.

### <a name="what-application-protocols-can-i-use-with-traffic-manager"></a>Vilka programprotokoll kan du använda med Traffic Manager?

Enligt beskrivningen i [så Traffic Manager fungerar](../traffic-manager/traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager fungerar på hello DNS-nivå. När hello DNS-sökning är klar kan ansluter klienterna toohello programslutpunkten direkt, inte via Trafikhanterarprofilen. Därför kan hello använda alla protokoll. Om du väljer TCP eftersom hello övervakning-protokollet, Traffic Manager endpoint hälsoövervakning kan göras utan att använda alla protokoll på programnivå. Om du väljer toohave hello hälsa verifieras med ett programprotokoll behöver hello slutpunkten toobe kan toorespond tooeither HTTP eller HTTPS GET-begäranden.

### <a name="can-i-use-traffic-manager-with-a-naked-domain-name"></a>Kan jag använda Traffic Manager med ett 'asbestgaller' domännamn?

Nej. hello DNS-standarden tillåter inte att skapa CNAME-poster tooco-finns med andra DNS-posterna för hello samma namn. hello apex (eller rot) på en DNS-zon innehåller alltid två befintlig DNS-poster. hello SOA- och hello auktoritära NS-poster. Detta innebär en CNAME-post inte kan skapas just hello zonens apex utan brott mot hello DNS-standarden.

Traffic Manager kräver ett DNS CNAME post toomap hello alternativa DNS-namn. Exempelvis kan du mappa www.contoso.com toohello Traffic Manager-profilen DNS-namnet contoso.trafficmanager.net. Dessutom returnerar hello trafikhanterarprofil en andra DNS CNAME tooindicate vilken slutpunkt hello klienten ska ansluta till.

toowork runt det här problemet bör du använda en HTTP-omdirigering toodirect trafik från hello asbestgaller domain name tooa olika URL, som sedan kan använda Traffic Manager. Till exempel kan hello asbestgaller domänen ”contoso.com” omdirigera användare toohello CNAME ”www.contoso.com” som pekar toohello Traffic Manager-DNS-namn.

Fullständigt stöd för asbestgaller domäner i Traffic Manager spåras i vår funktionen eftersläpning. Du kan registrera ditt stöd för den här funktionsbegäran av [röstning för den på vår community feedbackwebbplats](https://feedback.azure.com/forums/217313-networking/suggestions/5485350-support-apex-naked-domains-more-seamlessly).

### <a name="does-traffic-manager-consider-hello-client-subnet-address-when-handling-dns-queries"></a>Traffic Manager betraktar hello klienten undernätsadress vid hantering av DNS-frågor? 
Nej, för närvarande Traffic Manager anser endast hello källans IP-adress för hello DNS-fråga tas emot som vanligtvis hello IP-adressen för DNS-matchning för hello när utföra sökningar för routningsmetoder för geografiska och prestanda.  
Mer specifikt [RFC 7871 – klientnät i DNS-frågor](https://tools.ietf.org/html/rfc7871) som ger en [tillägget mekanism för DNS (EDNS0)](https://tools.ietf.org/html/rfc2671) som kan överföra på hello klienten undernätsadress från matchare som stöder detta tooDNS servrar är för närvarande stöds inte i Traffic Manager. Du kan registrera ditt stöd för den här funktionsbegäran via vårt [community feedbackwebbplats](https://feedback.azure.com/forums/217313-networking).


### <a name="what-is-dns-ttl-and-how-does-it-impact-my-users"></a>Vad är DNS TTL och hur den påverkar Mina användare?

När en DNS-fråga hamnar på Traffic Manager, anger ett värde i hello svar anropades time to live (TTL). Det här värdet vars enheten är i sekunder, anger tooDNS matchare nedströms på hur lång tid toocache svaret. När DNS-matchare inte är garanterat toocache kan det här resultatet cachelagringen dem toorespond tooany efterföljande frågor av hello cacheminnet i stället för att gå tooTraffic Manager DNS-servrar. Detta påverkar hello svar på följande sätt:
- en högre TTL minskar hello antalet frågor som hamna på hello Traffic Manager-DNS-servrar som kan minska hello kostnaden för en kund eftersom antalet frågor som hanteras är en fakturerbar användning.
- ett högre TTL-värde kan minska hello tid det tar toodo en DNS-sökning.
- ett högre TTL-värde innebär också att dina data inte återspeglar hello senaste hälsoinformation som Traffic Manager har erhållits via dess avsöknings agenter.

### <a name="how-high-or-low-can-i-set-hello-ttl-for-traffic-manager-responses"></a>Hur högt eller lågt anger jag hello TTL-värde för Traffic Manager svar?

Du kan ange på en per profil nivå hello DNS TTL toobe så lågt som 0 sekunder och så högt som 2 147 483 647 sekunder (hello största intervall som är kompatibla med [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt )). TTL-värdet 0 innebär att den underordnade DNS-matchare cachelagrar inte svar på frågor och alla frågor är förväntat tooreach hello Traffic Manager DNS-servrar för namnmatchning.

## <a name="traffic-manager-geographic-traffic-routing-method"></a>Traffic Manager geografiska trafikroutningsmetod

### <a name="what-are-some-use-cases-where-geographic-routing-is-useful"></a>Vilka är några användningsområden där geografiska routning är användbar? 
Geografisk routning kan användas i scenarier där en Azure kunden behöver toodistinguish användarna baserat på geografiska områden. Till exempel kan använder hello geografiska trafikroutningsmetod, du ge användare från vissa regioner en annan användarupplevelse än de från andra regioner. Ett annat exempel uppfyller lokala data suveränitet uppdrag som kräver att användare från en viss region hanteras endast med slutpunkter i den regionen.

### <a name="what-are-hello-regions-that-are-supported-by-traffic-manager-for-geographic-routing"></a>Vad är hello regioner som stöds av Traffic Manager för geografisk Routning? 
hello land/region som används av Traffic Manager kan hittas [här](traffic-manager-geographic-regions.md). När den här sidan hålls uppdaterade med ändringar, kan du också programmässigt hämta hello samma information med hjälp av hello [Azure Traffic Manager REST API](https://docs.microsoft.com/rest/api/trafficmanager/). 

### <a name="how-does-traffic-manager-determine-where-a-user-is-querying-from"></a>Hur traffic manager kontrollerar där en användare efterfrågar från? 
Traffic Manager kontrollerar hello käll-IP för hello-fråga (det troligen är en lokal DNS-matchare gör hello frågar hello användarens räkning) och använder en intern IP-tooregion kartan toodetermine hello-plats. Den här kartan uppdateras på ett kontinuerligt tooaccount efter ändringar i hello internet. 

### <a name="is-it-guaranteed-that-traffic-manager-can-correctly-determine-hello-exact-geographic-location-of-hello-user-in-every-case"></a>Det garanteras att Traffic Manager korrekt kan fastställa hello exakt geografisk plats för hello användare i varje fall?
Nej, Traffic Manager kan inte garantera att hello geografiska region som vi härleda från hello källans IP-adress för en DNS-fråga alltid motsvarar toohello användarens plats på grund av toohello följande orsaker: 

- Först, enligt beskrivningen i hello tidigare vanliga frågor och svar hello källans IP-adress som vi finns som en DNS-matchare gör hello sökning hello användarens räkning. Hello geografiska plats hello DNS-matchning är en bra proxy för hello geografisk plats för hello användare, kan det också vara olika beroende på hello storleken på hello DNS-matchare tjänsten och hello specifika DNS-lösartjänsten har valt en kund toouse. Exempelvis kan en kund i Malaysia ange enhetens inställningar används en DNS-matchare tjänst vars DNS-servern i Singapore få plockats toohandle hello frågan lösningar för att användarenhet. På så sätt att fallet Traffic Manager kan bara se hello matcharen IP-adress som motsvarar toohello Singapore plats. Se även hello tidigare vanliga frågor och svar om klienten undernätsadress stöd på den här sidan.

- Andra använder Traffic Manager en intern kartan toodo hello IP-adress toogeographic region översättning. När den här kartan verifieras och uppdateras på ett kontinuerligt tooincrease dess riktighet och konto för hello utvecklas uppbyggnad hello internet, finns det fortfarande hello möjligheten att vår information inte är en exakt representation av hello geografisk plats för alla hello IP-adresser.


###  <a name="does-an-endpoint-need-toobe-physically-located-in-hello-same-region-as-hello-one-it-is-configured-with-for-geographic-routing"></a>En slutpunkt måste toobe fysiskt belägna i hello samma region som hello ett den har konfigurerats med geografiska routning för? 
Nej, hello platsen för hello endpoint medför inga begränsningar som regioner kan vara mappade tooit. Exempelvis kan en slutpunkt i USA, Central Azure-region har alla användare från Indien dirigeras tooit.

### <a name="can-i-assign-geographic-regions-tooendpoints-in-a-profile-that-is-not-configured-toodo-geographic-routing"></a>Kan jag tilldelar geografiska områden tooendpoints i en profil som inte är konfigurerad toodo geografiska Routning? 

Ja, om hello routningsmetod för en profil inte geografiska, du kan använda hello [Azure Traffic Manager REST API](https://docs.microsoft.com/rest/api/trafficmanager/) tooassign geografiska områden tooendpoints i den här profilen. Hello gäller icke-geografiska routning typen profiler ignoreras den här konfigurationen. Om du ändrar dessa en toogeographic routning Profiltyp vid ett senare tillfälle kan Traffic Manager använda mappningarna.


### <a name="why-am-i-getting-an-error-when-i-try-toochange-hello-routing-method-of-an-existing-profile-toogeographic"></a>Varför får ett fel när jag försöker toochange hello routningsmetod för en befintlig profil tooGeographic?

Alla hello slutpunkter under en profil med geografiska routning måste toohave minst en region mappas tooit. tooconvert en befintlig profil toogeographic routning typ, måste du först tooassociate geografiska områden tooall dess slutpunkter som använder hello [Azure Traffic Manager REST API](https://docs.microsoft.com/rest/api/trafficmanager/) innan du ändrar hello routning skriver toogeographic. Om portalen först ta bort med hello slutpunkter, ändra hello routningsmetod för hello profil toogeographic och Lägg sedan till hello slutpunkter tillsammans med deras geografiska region-mappning. 


###  <a name="why-is-it-strongly-recommended-that-customers-create-nested-profiles-instead-of-endpoints-under-a-profile-with-geographic-routing-enabled"></a>Varför det rekommenderas starkt att kunderna skapa kapslade profiler i stället för slutpunkter med en profil med geografiska routning aktiverad? 

En region kan tilldelas tooonly en slutpunkt inom en profil om dess med geografiska routning. Om den slutpunkten inte är en kapslad typ med en profil som är anslutna tooit underordnade om den slutpunkt som ska feltillstånd, Traffic Manager fortsätter toosend trafik tooit sedan hello alternativ för att inte skicka all trafik som inte är alla bättre. Traffic Manager har inte redundans tooanother slutpunkt, även om hello region tilldelade är ”överordnad” hello region tilldelade toohello slutpunkt som gått feltillstånd (till exempel om en slutpunkt som har region Spanien blir ohälsosamt vi kommer inte redundans tooanother slutpunkten som har hello region Europa tilldelade tooit). Detta görs tooensure Traffic Manager värnar hello geografisk gränser som kunden har ställts in sin profil. tooget hello fördelen misslyckande tooanother slutpunkt när en slutpunkt är inte felfri, rekommenderas att geografiska områden tilldelas toonested profiler med flera slutpunkter inom det i stället för enskilda slutpunkter. På så sätt kan en slutpunkt i hello kapslade underordnade profilen misslyckas, trafik kan redundans tooanother slutpunkt i hello kapslade samma underordnade profilen.

### <a name="are-there-any-restrictions-on-hello-api-version-that-supports-this-routing-type"></a>Finns det några begränsningar på hello API-version som stöder den här Routning?

Ja, endast API-versionen 2017-03-01 och senare stöder hello geografiska routning. Alla äldre API-versioner kan inte använda toocreated profiler för geografisk routning eller tilldela tooendpoints geografiska områden. Om ett äldre API-versionen är används tooretrieve profiler från en Azure-prenumeration, returneras inte någon profil geografiska routning. Dessutom när du använder äldre API-versioner, returnerade en profil som har slutpunkter med en geografisk region tilldelning, har inte dess tilldelning för geografiska region som visas.



## <a name="traffic-manager-endpoints"></a>Traffic Manager-slutpunkter

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Kan jag använda Traffic Manager med slutpunkter från flera prenumerationer?

Det går inte att använda slutpunkter från flera prenumerationer med Azure Web Apps. Azure Web Apps kräver att alla anpassade domännamn som används med Web Apps används endast inom en enda prenumeration. Det är inte möjligt toouse Web Apps från flera prenumerationer med hello samma domännamn.

För andra typer av slutpunkter är det möjligt toouse Traffic Manager med slutpunkter från mer än en prenumeration. I Resource Manager från någon prenumeration kan att lägga till slutpunkter tooTraffic Manager, så länge hello person konfigurera hello Traffic Manager-profil har läsbehörighet toohello slutpunkt. Dessa behörigheter kan beviljas med [Azure Resource Manager rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-control-configure.md).


### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>Kan jag använda Traffic Manager med Molntjänsten ”Staging” platser?

Ja. Molntjänsten mellanlagring distributionsplatser kan konfigureras i Traffic Manager som externa slutpunkter. Hälsokontroller debiteras fortfarande med hello Azure-slutpunkter hastighet. Eftersom hello extern slutpunktstypen ändringar toohello underliggande tjänst inte fångas upp automatiskt. Med externa slutpunkter kan inte Traffic Manager identifiera när hello Cloud Service har stoppats eller tagits bort. Hello Traffic Manager fortsätter därför fakturering hälsokontroller tills hello slutpunkten är inaktiverad eller borttagen.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>Traffic Manager har stöd för IPv6-slutpunkter?

Traffic Manager ger inte någon IPv6-addressible namnservrar för närvarande. Traffic Manager kan dock fortfarande användas av IPv6-klienter som ansluter tooIPv6 slutpunkter. En klient gör inte DNS-begäranden direkt tooTraffic Manager. I stället använder hello klienten en rekursiv DNS-tjänsten. En endast-IPv6-klient skickar begäranden toohello rekursiva DNS-tjänsten via IPv6. Sedan ska hello rekursiv tjänsten kunna toocontact hello Traffic Manager namnservrar med IPv4.

Traffic Manager svarar med hello DNS-namnet på hello slutpunkt. toosupport en IPv6-slutpunkt, en DNS AAAA registrera peka hello endpoint DNS-namnet toohello IPv6-adress måste finnas. Traffic Manager-hälsokontroller stöder endast IPv4-adresser. hello-tjänsten måste tooexpose en IPv4-slutpunkten på hello samma DNS-namn.

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-hello-same-region"></a>Kan jag använda Traffic Manager med mer än en Webbapp i hello samma region?

Traffic Manager är oftast används toodirect trafik tooapplications distribueras i olika regioner. Men det kan också användas om ett program har mer än en distribution i hello samma region. hello Azure Traffic Manager-slutpunkter tillåter inte mer än en slutpunkt för webbprogrammet från hello samma Azure-region toobe läggs toohello samma Traffic Manager-profilen.

##  <a name="traffic-manager-endpoint-monitoring"></a>Slutpunktsövervakning av Traffic Manager

### <a name="is-traffic-manager-resilient-tooazure-region-failures"></a>Är flexibel Traffic Manager-tooAzure region fel?

Traffic Manager är en viktig del av hello leverans av hög tillgänglighet program i Azure.
toodeliver hög tillgänglighet, Traffic Manager måste ha en ovanligt hög tillgänglighet och vara motståndskraftiga tooregional fel.

Avsiktligt är Traffic Manager-komponenter flexibel tooa fullständigt fel av någon Azure-region. Den här återhämtning gäller tooall Traffic Manager-komponenter: hello DNS-namnservrar, hello API, hello lagringsskikt och hello endpoint övervakningstjänsten.

Hello osannolika av ett avbrott i en hela Azure-region är Traffic Manager förväntade toocontinue toofunction normalt. Program som distribueras i flera Azure-regioner kan förlitar sig på Traffic Manager toodirect trafik tooan tillgänglig instans av sina program.

### <a name="how-does-hello-choice-of-resource-group-location-affect-traffic-manager"></a>Hur påverkar hello valet av resursgruppens plats Traffic Manager?

Traffic Manager är en global tjänst. Det är inte regional. hello valet av resursgruppens plats gör inga skillnaden tooTraffic Manager-profiler som distribuerats i resursgruppen.

Azure Resource Manager kräver att alla resurs grupper toospecify en plats, som anger hello standardplatsen för resurser har distribuerats i resursgruppen. När du skapar en Traffic Manager-profil skapas den i en resursgrupp. Alla Traffic Manager-profiler som använder **globala** som deras plats, åsidosätter hello resurs gruppen standard.

### <a name="how-do-i-determine-hello-current-health-of-each-endpoint"></a>Hur vet hello aktuellt hälsotillstånd för varje slutpunkt?

hello aktuella övervaka status för varje slutpunkt i tillägg toohello övergripande profilen visas i hello Azure-portalen. Den här informationen är också tillgängligt via hello trafik övervakaren [REST API](https://msdn.microsoft.com/library/azure/mt163667.aspx), [PowerShell-cmdlets](https://msdn.microsoft.com/library/mt125941.aspx), och [plattformsoberoende Azure CLI](../cli-install-nodejs.md).

Azure ger inte några historisk information om tidigare endpoint hälsa eller hello möjlighet tooraise aviseringar om ändringar tooendpoint hälsotillstånd.

### <a name="can-i-monitor-https-endpoints"></a>Övervakar jag HTTPS-slutpunkter

Ja. Traffic Manager har stöd för sökning via HTTPS. Konfigurera **HTTPS** som hello protokoll i hello övervakningskonfigurationen.

Traffic manager kan inte ange några certifikatvalidering inklusive:

* Serversidan certifikat verifieras inte
* SNI serversidan certifikat stöds inte
* Klientcertifikat stöds inte

### <a name="can-i-use-traffic-manager-even-if-my-application-does-not-have-support-for-http-or-https"></a>Kan jag använda Traffic Manager även om mitt program inte har stöd för HTTP eller HTTPS?

Ja. Du kan ange TCP hello övervakning protokoll och Traffic Manager kan initiera en TCP-anslutning och vänta på svar från hello slutpunkten. Om hello-ändpunkten svarar toohello anslutningsbegäran med en svar tooestablish hello anslutning, inom tidsgränsen hello har period och sedan denna slutpunkt markerats som felfri.

### <a name="what-specific-responses-are-required-from-hello-endpoint-when-using-tcp-monitoring"></a>Vilka specifika svar krävs hello slutpunkt när du använder TCP övervakning?

När TCP-övervakning används hello Traffic Manager startar en trevägs TCP handskakning genom att skicka en SYN begära tooendpoint på angivna porten. Det väntar sedan en viss tidsperiod (som anges i hello timeout-inställningar) på svar från hello slutpunkt. Om hello-ändpunkten svarar toohello SYN begäran med ett SYN ACK-svar inom tidsgränsen för hello som anges i hello övervakningsinställningarna sedan denna slutpunkt anses felfritt. Om hello SYN ACK svar tas emot, återställer hello Traffic Manager hello anslutningen genom att svara igen med en RST.

### <a name="how-fast-does-traffic-manager-move-my-users-away-from-an-unhealthy-endpoint"></a>Hur snabbt flytta mina användare från en ohälsosamt slutpunkt i Traffic Manager?

Traffic Manager innehåller flera inställningar som kan hjälpa dig att toocontrol hello funktion i Traffic Manager-profilen på följande sätt:
- Du kan ange att hello Traffic Manager-avsökningar hello slutpunkter oftare genom att ange hello intervall för sökning efter 10 sekunder. Detta säkerställer att alla slutpunkten ska feltillstånd kan identifieras så snart som möjligt. 
- Du kan ange hur länge toowait innan en förfrågan om kontroll av hälsotillstånd timeout (lägsta timeout-värdet är 5 sek).
- Du kan ange hur många fel kan uppstå innan hello endpoint markeras som ohälsosam. Det här värdet kan vara låg som 0, i vilken case hello-slutpunkt har dåligt hälsotillstånd när den inte hello första hälsokontroll. Använda hello lägsta värdet 0 för hello tolereras antalet fel kan dock leda tooendpoints som tas bort från rotationen på grund av tooany övergående problem som kan uppstå när hello sökning.
- Du kan ange hello time to live (TTL) för hello DNS-svar toobe så lågt som 0. Om du gör det innebär att DNS-matchare kan inte cachelagra hello svar och varje ny fråga hämtar ett svar som innehåller hello senaste hälsoinformation som hello Traffic Manager har.

Med hjälp av de här inställningarna ge Traffic Manager redundans under 10 sekunder efter att en slutpunkt blir ohälsosamt och en DNS-fråga görs mot motsvarande hello-profil.

### <a name="how-can-i-specify-different-monitoring-settings-for-different-endpoints-in-a-profile"></a>Hur kan jag ange olika övervakningsinställningarna för olika slutpunkter i en profil?

Traffic Manager övervakningsinställningarna finns på en per profil nivå. Om du behöver toouse en annan övervakning inställning för endast en slutpunkt kan också göra det genom att låta den slutpunkten som en [kapslad profil](traffic-manager-nested-profiles.md) vars övervakningsinställningarna skiljer sig från hello överordnade profil.

### <a name="what-host-header-do-endpoint-health-checks-use"></a>Vilka värden huvud gör endpoint kontroll används?

Traffic Manager använder värdhuvuden hälsokontroller för HTTP och HTTPS. hello värdadressen som används av Traffic Manager är hello namn hello endpoint mål som konfigurerats i hello profil. hello-värde som används i hello värdhuvudet kan inte anges separat från hello target-egenskap.

### <a name="what-are-hello-ip-addresses-from-which-hello-health-checks-originate"></a>Vad är hello IP-adresser som kontrollerar hello hälsotillstånd kommer?

hello innehåller följande lista hello IP-adresser som kontrollerar hälsa för Traffic Manager kan kommer. Du kan använda den här listan tooensure att inkommande anslutningar från dessa IP-adresser tillåts på hello slutpunkter toocheck dess hälsostatus.

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

### <a name="how-many-health-checks-toomy-endpoint-can-i-expect-from-traffic-manager"></a>Hur många hälsa kontrollerar toomy endpoint ges från Traffic Manager?

hello antalet Traffic Manager hälsa kontrollerar nå slutpunkten beror på hello följande:
- hello-värde som du har angett för hello Övervakningsintervall (mindre intervall innebär fler begäranden hamnar på din slutpunkt i en given tidsperiod).
- hello antal platser från där hello hälsokontroller kommer (hello IP-adresser från där du kan förvänta dig kontrollerna visas i hello föregående FAQ).

## <a name="traffic-manager-nested-profiles"></a>Traffic Manager kapslade profiler

### <a name="how-do-i-configure-nested-profiles"></a>Hur konfigurerar kapslade profiler?

Kapslade Traffic Manager-profiler kan konfigureras med båda hello Azure Resource Manager och hello klassiska Azure REST API: er, Azure PowerShell-cmdlets och plattformsoberoende Azure CLI-kommandona. De stöds också via hello nya Azure-portalen. De stöds inte i hello klassiska portalen.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>Stöd för hur många lager med kapsling har Traffic Manager?

Du kan kapsla profiler in too10 nivåer. 'Slinga' tillåts inte.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-hello-same-traffic-manager-profile"></a>Kan jag blanda andra typer av slutpunkter med kapslad underordnad profiler i hello samma Traffic Manager-profilen?

Ja. Det finns inga begränsningar för hur du kombinerar slutpunkter av olika typer i en profil.

### <a name="how-does-hello-billing-model-apply-for-nested-profiles"></a>Hur gäller hello faktureringsmodell som tillämpas för kapslade profiler?

Det är inte negativt priser effekten av att använda kapslade profiler.

Traffic Manager fakturering består av två komponenter: slutpunkt hälsokontroller och miljontals DNS-frågor

* Slutpunkten hälsokontroller: är gratis för en underordnad profil när konfigurerad som en slutpunkt i en överordnad-profil. Övervakning av hello slutpunkter i hello underordnade profilen faktureras i hello vanligt.
* DNS-frågor: varje fråga räknas endast en gång. En fråga till en överordnad-profil som returnerar en slutpunkt från en underordnad profil räknas mot hello överordnade profil.

Fullständig information finns i hello [Traffic Manager sida med priser](https://azure.microsoft.com/pricing/details/traffic-manager/).

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>Finns det en prestandapåverkan för kapslade profiler?

Nej. Det finns ingen inverkan på prestanda när med hjälp av kapslade profiler.

hello Traffic Manager-namnservrar passerar internt hello profil hierarkin vid bearbetning av varje DNS-fråga. En DNS-fråga tooa överordnade profil kan ta emot ett DNS-svar till en slutpunkt från en underordnad profil. En CNAME-post används om du använder en enskild profil eller kapslade profiler. Det finns inget behov av toocreate en CNAME-post för varje profil i hello hierarki.

### <a name="how-does-traffic-manager-compute-hello-health-of-a-nested-endpoint-in-a-parent-profile"></a>Hur compute hello hälsotillståndet för en kapslad slutpunkt i en överordnad profil i Traffic Manager?

hello överordnade profil utföra inte hälsokontroller på hello underordnade direkt. Hello hälsotillstånd hello underordnade profilen slutpunkter är i stället använda toocalculate hello övergripande hälsa för hello underordnade profilen. Den här informationen sprids in hello kapslad profil hierarkin toodetermine hello hälsa för hello kapslade slutpunkt. hello överordnade profilen använder den här aggregerade hälsa toodetermine om hello trafik kan vara riktad toohello underordnade.

hello beskrivs följande tabell hello beteendet för Traffic Manager hälsa söker efter en kapslad slutpunkt.

| Status för underordnade profilen Övervakare | Överordnad Övervakare för slutpunkten status | Anteckningar |
| --- | --- | --- |
| Inaktiverad. hello underordnade profilen har inaktiverats. |Stoppad |hello överordnade slutpunktstillståndet stoppas inte har inaktiverats. hello inaktiverat tillstånd är reserverat för som anger att du har inaktiverat hello slutpunkt i hello överordnade profil. |
| Försämrad. Minst en underordnad-profilens slutpunkt har statusen degraderad. |Online: hello antalet Online slutpunkter i hello underordnade profilen är minst hello värdet för MinChildEndpoints.<BR>CheckingEndpoint: hello antalet Online plus CheckingEndpoint slutpunkter i hello underordnade profilen är minst hello värdet för MinChildEndpoints.<BR>Försämrad: annars. |Trafiken är routade tooan slutpunkten för status CheckingEndpoint. Om MinChildEndpoints är för högt kan försämras alltid hello slutpunkt. |
| Online. Minst en underordnad-profilens slutpunkt är ett onlinetillstånd. Ingen slutpunkt är i hello degraderad tillstånd. |Se ovan. | |
| CheckingEndpoints. Minst en underordnad-profilens slutpunkt är 'CheckingEndpoint'. Inga slutpunkter är 'Online' eller 'Försämrad' |Samma som ovan. | |
| Inaktiva. Alla underordnade profilen slutpunkterna är inaktiverad eller stoppad eller den här profilen har inga slutpunkter. |Stoppad | |

## <a name="next-steps"></a>Nästa steg:
- Lär dig mer om Traffic Manager [endpoint övervaknings- och automatisk redundans](../traffic-manager/traffic-manager-monitoring.md).
- Lär dig mer om Traffic Manager [trafikroutningsmetoder](../traffic-manager/traffic-manager-routing-methods.md).