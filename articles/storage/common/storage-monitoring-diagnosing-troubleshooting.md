---
title: "aaaMonitor, diagnostisera och felsöka Azure Storage | Microsoft Docs"
description: "Använd funktioner som storage analytics, klientsidan loggning och andra verktyg från tredje part tooidentify diagnostisera och felsöka problem med Azure Storage."
services: storage
documentationcenter: 
author: fhryo-msft
manager: jahogg
editor: tysonn
ms.assetid: d1e87d98-c763-4caa-ba20-2cf85f853303
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: fhryo-msft
ms.openlocfilehash: 43f34a4ccbc3071cdb489958252498a1f88e1a34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-diagnose-and-troubleshoot-microsoft-azure-storage"></a>Övervaka, diagnostisera och felsök Microsoft Azure Storage
[!INCLUDE [storage-selector-portal-monitoring-diagnosing-troubleshooting](../../../includes/storage-selector-portal-monitoring-diagnosing-troubleshooting.md)]

## <a name="overview"></a>Översikt
Diagnostisera och felsöka problem i ett distribuerat program som finns i en molnmiljö kan vara mer komplicerat än i vanliga miljöer. Program kan distribueras i en PaaS eller IaaS-infrastruktur lokalt på en mobil enhet eller i en kombination av dessa. Normalt nätverkstrafik för ditt program kan bläddra offentliga och privata nätverk och ditt program kan använda flera lagringstekniker som Microsoft Azure Storage-tabeller, Blobbar, köer eller filer i tillägg tooother data lagras så som relationella och dokumentera databaser.

toomanage sådana program har du bör övervaka dem proaktivt och förstå hur toodiagnose och felsöka alla aspekter av dem och deras beroende tekniker. Som en användare i Azure Storage-tjänster du bör alltid övervaka hello Storage-tjänster som används i ditt program för eventuella oväntade ändringar i beteendet (till exempel långsammare än vanligt svarstider) och använda loggning toocollect mer detaljerad data och tooanalyze en problem i mer detalj. hello diagnostikinformation som du hämtar från övervakning och loggning kan du toodetermine hello grundorsaken till hello utfärda tillämpningsprogrammet påträffades. Sedan kan du felsöka problemet hello och fastställa hello lämpliga åtgärder du kan vidta tooremediate den. Azure Storage är en grundläggande Azure-tjänsten och utgör en viktig del av hello merparten av lösningar som kunder distribuerar toohello Azure-infrastrukturen. Azure Storage innehåller funktioner toosimplify övervaka, diagnostisera och felsöka problem med lagring i dina molnbaserade program.

> [!NOTE]
> hello Azure File storage stöder inte loggning just nu.
> 

En praktiska guide tooend-till-slutpunkt felsökning i Azure Storage-program, se [slutpunkt till slutpunkt felsökning med hjälp av Azure Storage-mätvärden och loggning, AzCopy och Message Analyzer](../storage-e2e-troubleshooting.md).

* [Introduktion]
  * [Hur den här guiden är organiserat]
* [övervakning lagringstjänsten]
  * [Övervakning av tjänstens hälsa]
  * [Övervakning av kapacitet]
  * [Övervaka tillgänglighet]
  * [Övervaka prestanda]
* [diagnostisera problem med lagring]
  * [Hälsotillståndsproblem med tjänsten för]
  * [Prestandaproblem]
  * [Diagnostisera fel]
  * [Storage-emulatorn problem]
  * [Loggningsverktyg för lagring]
  * [Med hjälp av verktyg för loggning]
* [slutpunkt till slutpunkt spårning]
  * [Korrelerar loggdata]
  * [ID för klientbegäran]
  * [Server begäran-ID]
  * [Tidsstämplar]
* [felsökningsanvisningar]
  * [mätvärdena visar AverageE2ELatency hög och låg AverageServerLatency]
  * [Mätvärdena visar låg AverageE2ELatency och låg AverageServerLatency men hello-klienten har hög fördröjning]
  * [Mätningar visar ett högt värde för AverageServerLatency]
  * [Du upplever oväntade fördröjningar i en köad meddelandeleverans]
  * [mätvärdena visar en ökning i PercentThrottlingError]
  * [mätvärdena visar en ökning i PercentTimeoutError]
  * [Mätningar visar en ökning i PercentNetworkError]
  * [hello klienten tar emot HTTP 403 (förbjuden) meddelanden]
  * [hello klienten tar emot HTTP 404 (inget hittas) meddelanden]
  * [hello klienten tar emot HTTP 409 (konflikt) meddelanden]
  * [mätvärdena visar låg PercentSuccess eller analytics loggposter innehålla åtgärder med status för ClientOtherErrors]
  * [Kapacitetsdata visar en oväntad ökning i kapacitetsförbrukning för lagring]
  * [Det uppstår oväntade omstarter av virtuella datorer som har ett stort antal anslutna virtuella hårddiskar]
  * [Problemet uppstår från att använda hello storage-emulatorn för utveckling eller testning]
  * [Det uppstår problem med att installera hello Azure SDK för .NET]
  * [Du har ett annat problem med en storage-tjänst]
  * [Felsökning av problem med Azure File storage med Windows](../files/storage-troubleshoot-windows-file-connection-problems.md)   
  * [Felsökning av problem med Azure File storage med Linux](../files/storage-troubleshoot-linux-file-connection-problems.md)
* [tilläggen]
  * [bilaga 1: med Fiddler toocapture HTTP och HTTPS-trafik]
  * [bilaga 2: använda Wireshark toocapture nätverkstrafik]
  * [tillägg 3: använda Microsoft Message Analyzer toocapture nätverkstrafik]
  * [Tillägg 4: Använda Excel tooview mått och loggar data]
  * [Tillägg 5: Övervaka med Programinsikter för Visual Studio Team Services]

## <a name="introduction"></a>Introduktion
Den här guiden visar du hur toouse funktioner, till exempel Azure Storage Analytics klientsidan loggning i hello Azure Storage-klientbiblioteket och andra verktyg från tredje part tooidentify diagnostisera och felsöka Azure Storage-relaterade problem.

![][1]

Den här guiden är avsedd toobe läsa främst av utvecklare av onlinetjänster som använder Azure Storage-tjänster och IT-experter som ansvarar för att hantera dessa online services. hello målen med den här guiden är:

* toohelp du underhålla hello hälsotillstånd och prestanda för dina Azure Storage-konton.
* tooprovide du med hello nödvändiga processer och verktyg toohelp du bestämma om ett problem eller problem i ett program relaterad tooAzure lagring.
* tooprovide du tillämplig vägledning för att lösa problem relaterade tooAzure lagring.

### <a name="how-this-guide-is-organized"></a>Hur den här guiden är organiserat
Hej avsnittet ”[övervakning lagringstjänsten]” beskriver hur toomonitor hello hälsotillstånd och prestanda för ditt Azure Storage-tjänster med hjälp av Azure Storage Analytics mätvärden (Storage-mätvärden).

Hej avsnittet ”[diagnostisera problem med lagring]” beskriver hur toodiagnose problem med Azure Storage Analytics loggning (Storage loggning). Det beskriver hur tooenable klientsidan loggar med hello verksamhet på ett av hello klientbibliotek, till exempel hello Storage-klientbibliotek för .NET eller hello Azure SDK för Java.

Hej avsnittet ”[slutpunkt till slutpunkt spårning]” beskriver hur du kan jämföra hello information som finns i olika loggfiler och mätvärdesdata.

Hej avsnittet ”[felsökningsanvisningar]” innehåller felsökningsinformation för några av hello vanliga lagringsrelaterade problem som kan uppstå.

Hej ”[tilläggen]” innehåller information om hur du använder andra verktyg som Wireshark och Netmon för analysera nätverk paketdata, Fiddler för att analysera HTTP/HTTPS-meddelanden och Microsoft Message Analyzer för korrelation mellan logga data.

## <a name="monitoring-your-storage-service"></a>Övervaka storage-tjänst
Om du är bekant med prestandaövervakning av Windows kan du se Storage-mätvärden som motsvarande Azure Storage för Windows-prestandaräknare. Storage-mätvärden innehåller en omfattande uppsättning mått (räknare i Prestandaövervakaren terminologi), till exempel tjänsttillgänglighet, totalt antal begäranden tooservice eller procent av lyckade begäranden tooservice. En fullständig lista över tillgängliga mått för hello finns [Storage Analytics mätvärden tabellschemat](http://msdn.microsoft.com/library/azure/hh343264.aspx). Du kan ange om du vill hello storage service toocollect och aggregera mått varje timme eller varje minut. Mer information om hur tooenable mått och övervaka dina lagringskonton finns [aktivera storage-mätvärden och visa mätvärdesdata](http://go.microsoft.com/fwlink/?LinkId=510865).

Du kan välja vilka timvis mått du vill använda toodisplay i hello [Azure-portalen](https://portal.azure.com) och konfigurera regler som administratörer meddelas via e-post när en timvis måttet överskrider ett visst tröskelvärde. Mer information finns i [ta emot aviseringar](/azure/monitoring-and-diagnostics/monitoring-overview-alerts.md). 

hello lagringstjänsten samlar in mått med bästa prestanda, men kan inte registrera varje Lagringsåtgärden.

Du kan visa mätvärden, till exempel tillgänglighet, totalt antal begäranden och genomsnittlig svarstid för nummer för ett lagringskonto i hello Azure-portalen. En meddelanderegel har också ställts in tooalert administratör om tillgänglighet sjunker under en viss nivå. Från att visa dessa data är ett möjligt området för undersökning hello tabellen service lyckade procentandel är lägre än 100% (Mer information finns i avsnittet hello ”[mätvärdena visar låg PercentSuccess eller analytics loggposter innehålla åtgärder med status för ClientOtherErrors]”).

Du bör alltid övervaka din tooensure för Azure-program som de är felfria och fungerar som väntat genom att:

* Upprätta vissa baslinjen mått för program som aktiverar du toocompare aktuella data och identifiera eventuella viktiga ändringar i hello beteendet för Azure storage och ditt program. hello värdena för baslinje-mätvärden, i många fall blir specifika program och du bör fastställa dem när du är prestandatestning ditt program.
* Registrering minut mått och använder dem toomonitor aktivt för oväntade fel och inkonsekvenser, till exempel toppar i fel antal eller begäran priser.
* Spela in varje timme mått och använda dem toomonitor genomsnittsvärden som genomsnittlig fel antal och begära priser.
* Undersöker möjliga problem med diagnosverktyg som beskrivs senare i avsnittet hello ”[diagnostisera problem med lagring]”.

hello diagram i hello följande bild illustrerar hur hello medelvärdet som inträffar för varje timme mått kan dölja toppar i aktiviteten. hello timvis mått visas tooshow en konstant andel begäranden när hello minut mått avslöja hello variationer som verkligen äger rum.

![][3]

hello resten av det här avsnittet beskriver vilka mått som du bör övervaka och varför.

### <a name="monitoring-service-health"></a>Övervakning av tjänstens hälsa
Du kan använda hello [Azure-portalen](https://portal.azure.com) tooview hello hälsotillstånd hello Storage-tjänst (och andra Azure-tjänster) i alla hello Azure-regioner hello världen. Detta gör att du toosee omedelbart om problemet inte kan kontrollera påverkar hello lagringstjänsten i hello region som du använder för ditt program.

Hej [Azure-portalen](https://portal.azure.com) kan också ge meddelanden om incidenter som påverkar hello olika Azure-tjänster.
Obs: Den här informationen tidigare var tillgänglig, tillsammans med historiska data om hello [Azure Service instrumentpanelen](http://status.azure.com).

När hello [Azure-portalen](https://portal.azure.com) samlar in hälsouppgifter från inuti hello Azure-datacenter (innanför ut övervakning), kan du också överväga införandet av en metod utanför i toogenerate syntetiska transaktioner som med jämna mellanrum åtkomst till Azure-baserad webbprogrammet från flera platser. Hej tjänster som erbjuds av [Dynatrace](http://www.dynatrace.com/en/synthetic-monitoring) och Programinsikter för Visual Studio Team Services är exempel på den här metoden för utanför. Mer information om Application Insights för Visual Studio Team Services finns i bilaga hello ”[tillägg 5: övervaka med Programinsikter för Visual Studio Team Services](#appendix-5)”.

### <a name="monitoring-capacity"></a>Övervakning av kapacitet
Storage-mätvärden lagrar bara kapacitetsdata för hello blob-tjänsten eftersom blobbar vanligtvis konto hello största del av lagrade data (för närvarande hello skrivning, det är inte möjligt toouse Storage-mätvärden toomonitor hello kapacitet för tabeller och köer) . Du hittar dessa data i hello **$MetricsCapacityBlob** tabell om du har aktiverat övervakning för hello Blob-tjänsten. Storage-mätvärden registrerar dessa data en gång per dag och du kan använda hello värdet för hello **RowKey** toodetermine om hello raden innehåller en entitet som är kopplad toouser data (värdet **data**) eller analytics data ( värdet **analytics**). Innehåller information om hello mängden lagringsutrymme som används för varje lagrade entitet (**kapacitet** mätt i byte) och hello aktuellt antal behållare (**ContainerCount**) och blobbar ( **ObjectCount**) används i hello storage-konto. Mer information om hello kapacitetsdata lagras i hello **$MetricsCapacityBlob** tabell, se [Storage Analytics mätvärden tabellschemat](http://msdn.microsoft.com/library/azure/hh343264.aspx).

> [!NOTE]
> Du bör övervaka de här värdena för en tidig varning att du närmar dig hello kapacitetsbegränsningar för ditt lagringskonto. Hello Azure-portalen, kan du lägga till Varningsregler toonotify du om sammanställd lagringsanvändning överskrider eller understiger tröskelvärden som du anger.
> 
> 

Hjälp beräknar hello storlek för olika lagringsobjekt,, till exempel blobbar finns i bloggposten hello [förstå Azure Storage fakturering – bandbredd, transaktioner och kapacitet](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

### <a name="monitoring-availability"></a>Övervaka tillgänglighet
Du bör övervaka hello tillgängligheten för hello lagringstjänster i ditt lagringskonto genom att övervaka hello värdet i hello **tillgänglighet** kolumn i hello timvis eller minut mått tabeller – **$ MetricsHourPrimaryTransactionsBlob**, **$MetricsHourPrimaryTransactionsTable**, **$MetricsHourPrimaryTransactionsQueue**, **$ MetricsMinutePrimaryTransactionsBlob**, **$MetricsMinutePrimaryTransactionsTable**, **$MetricsMinutePrimaryTransactionsQueue**, **$ MetricsCapacityBlob**. Hej **tillgänglighet** kolumnen innehåller ett procentvärde som visar hello tillgängligheten för hello tjänsten eller hello API-åtgärden som representeras av hello raden (hello **RowKey** visar om hello raden innehåller mätvärden för hello tjänsten som helhet eller för en specifik åtgärd API).

Alla värdet som är mindre än 100% anger att vissa lagringsbegäranden skickas. Du kan se varför de inte genom att undersöka hello andra kolumner i hello mätvärdesdata som visar hello antal begäranden med olika feltyper som **ServerTimeoutError**. Du kan förvänta toosee **tillgänglighet** faller tillfälligt under 100% skäl, till exempel tillfälligt server timeout medan hello tjänsten flyttar partitioner toobetter belastningsutjämna begäran; hello försök logiken i ditt klientprogram bör hantera dessa återkommande villkor. hello artikel [Storage Analytics loggade åtgärder och statusmeddelanden](http://msdn.microsoft.com/library/azure/hh343260.aspx) visar hello transaktionstyper med Storage-mätvärden i dess **tillgänglighet** beräkning.

I hello [Azure-portalen](https://portal.azure.com), kan du lägga till Varningsregler toonotify du om **tillgänglighet** för en tjänst som ligger under ett tröskelvärde som du anger.

Hej ”[felsökningsanvisningar]” i den här guiden beskriver vissa vanliga storage service problem relaterade tooavailability.

### <a name="monitoring-performance"></a>Övervaka prestanda
toomonitor hello prestanda för hello lagringstjänster, kan du använda hello följande mätvärden från hello varje timme och minut mått tabeller.

* Hej värden i hello **AverageE2ELatency** och **AverageServerLatency** kolumner visar hello genomsnittstiden hello lagringstjänsten eller API åtgärdstypen tar tooprocess begäranden. **AverageE2ELatency** är ett mått för slutpunkt till slutpunkt latens som innehåller hello tidsåtgång tooread hello begäran och skicka hello svar i tillägg toohello tidsåtgång tooprocess hello begäran (därför innehåller Nätverksfördröjningen när hello begäran når hello lagringstjänsten); **AverageServerLatency** är ett mått på bara hello bearbetningstid och därför inte omfattar några Nätverksfördröjningen relaterade toocommunicating med hello-klienten. Hello i avsnittet ”[mätvärdena visar AverageE2ELatency hög och låg AverageServerLatency]” senare i den här guiden för en beskrivning av varför det finnas betydande skillnader mellan dessa två värden.
* Hej värden i hello **TotalIngress** och **TotalEgress** kolumner visar hello total mängd data, i byte, kommer i tooand utanför lagringstjänsten eller via en viss typ för API-åtgärden.
* Hej värden i hello **TotalRequests** kolumnen visar hello Totalt antal begäranden som hello lagringstjänsten av API-åtgärd som tar emot. **TotalRequests** är hello Totalt antal begäranden som tar emot hello storage-tjänst.

Normalt ska övervakas av oväntade förändringar i någon av dessa värden som en indikator på att du har ett problem som kräver en undersökning.

I hello [Azure-portalen](https://portal.azure.com), kan du lägga till Varningsregler toonotify du om något av hello prestandastatistik för den här tjänsten faller under eller överskrida ett tröskelvärde som du anger.

Hej ”[felsökningsanvisningar]” i den här guiden beskriver vissa vanliga storage service problem relaterade tooperformance.

## <a name="diagnosing-storage-issues"></a>Diagnostisera problem med lagring
Det finns ett antal sätt att kan du blir medveten om problem och frågor i ditt program, bland annat:

* Ett allvarligt fel inträffar som orsakar hello programmet toocrash eller toostop arbetar.
* Betydande förändringar från baslinjevärdena i hello mått som du övervakar enligt beskrivningen i föregående avsnitt i hello ”[övervakning lagringstjänsten]”.
* -Rapporter från användare av ditt program inte har en viss åtgärd slutförs som förväntat eller att vissa funktionen inte fungerar.
* Fel som har genererats i programmet som visas i loggfiler eller via någon annan metod för meddelande.

Normalt indelas lagringstjänster för problem relaterade tooAzure i fyra kategorier:

* Programmet har ett prestandaproblem rapporteras av användarna eller framgick av ändringar i hello prestandamått.
* Det finns ett problem med hello Azure Storage-infrastrukturen i en eller flera regioner.
* Programmet har uppstått ett fel rapporterat av användarna eller visas med en ökning i någon av du övervaka hello fel antal mått.
* Under utveckling och testning, kan du använda hello lokala lagringsemulatorn; Du kan stöta på problem som rör särskilt toousage av hello storage-emulatorn.

hello beskriver följande avsnitt hello steg du bör följa toodiagnose och felsökning av problem i alla dessa fyra kategorier. Hej avsnittet ”[felsökningsanvisningar]” senare i den här guiden ger fler detaljer för några vanliga problem du kan stöta på.

### <a name="service-health-issues"></a>Hälsotillståndsproblem med tjänsten för
Tjänstens hälsotillstånd är vanligtvis inte kan kontrollera. Hej [Azure-portalen](https://portal.azure.com) ger information om pågående problem med Azure-tjänster inklusive lagringstjänster. Om du har valt för Geo-Redundant lagring med läsbehörighet när du skapade ditt lagringskonto, kunde sedan i dina data som inte var tillgänglig i hello primär plats hello-händelse, tillämpningsprogrammet tillfälligt växla toohello skrivskyddad kopia hello sekundär plats. toodo kan ditt program måste innehålla kan tooswitch mellan använder hello primära och sekundära lagringsplatser och vara kan toowork i begränsat läge med skrivskyddade data. hello på Azure Storage-klientbibliotek Tillåt toodefine en återförsöksprincip som kan läsa från sekundär lagring om en läsning från primära lagring misslyckas. Programmet måste också toobe medveten om att hello data på hello sekundär plats är överensstämmelse. Mer information finns i blogginlägget hello [Azure lagringsalternativ för redundans och Geo-Redundant lagring med läsbehörighet](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

### <a name="performance-issues"></a>Prestandaproblem
hello prestanda för ett program kan vara subjektiva, särskilt ur användare. Det är därför viktigt toohave baslinjen mått tillgängliga toohelp du identifiera där det kanske finns ett prestandaproblem. Många faktorer kan påverka hello prestanda för ett Azure storage-tjänst ur hello klienten program. Dessa faktorer kan fungera i hello lagringstjänsten, hello klienten eller hello nätverksinfrastruktur. Det är därför viktigt toohave en strategi för att identifiera hello ursprung hello prestandaproblem.

När du har identifierat hello sannolikt plats hello orsak till hello prestandaproblemet från hello mått kan kan därefter du använda hello loggen filer toofind detaljerad information toodiagnose och felsöka hello problemet ytterligare.

Hej avsnittet ”[felsökningsanvisningar]” senare i den här guiden ger mer information om vanliga prestanda relaterade problem som du kan stöta på.

### <a name="diagnosing-errors"></a>Diagnostisera fel
Användare av ditt program kan meddela dig om felen som rapporteras av klientprogrammet hello. Storage-mätvärden även information om antal fel för olika typer av storage-tjänster som **NetworkError**, **ClientTimeoutError**, eller **AuthorizationError**. Medan Storage-mätvärden spelar bara antal olika feltyper, hittar du mer information om enskilda begäranden genom att undersöka serversidan och klientsidan loggar för nätverket. Normalt ger hello HTTP-statuskod som returneras av hello lagringstjänsten en indikation om varför hello begäran misslyckades.

> [!NOTE]
> Kom ihåg att du kan förvänta toosee återkommande fel: till exempel fel på grund av tootransient nätverksförhållanden eller programfel.
> 
> 

hello är följande resurser användbara för att förstå lagringsrelaterade status och felkoder:

* [Vanliga REST API-felkoder](http://msdn.microsoft.com/library/azure/dd179357.aspx)
* [Felkoder för BLOB-tjänst](http://msdn.microsoft.com/library/azure/dd179439.aspx)
* [Felkoder för kön](http://msdn.microsoft.com/library/azure/dd179446.aspx)
* [Felkoder för tabellen](http://msdn.microsoft.com/library/azure/dd179438.aspx)
* [Felkoder för filen](https://msdn.microsoft.com/library/azure/dn690119.aspx)

### <a name="storage-emulator-issues"></a>Storage-emulatorn problem
hello Azure SDK innehåller en storage-emulatorn som du kan köra på en arbetsstation för utveckling. Den här emulatorn simulerar de flesta av hello funktionssätt hello Azure storage-tjänster och är användbart under utveckling och testning måste aktivera toorun program som använder Azure storage-tjänster utan hello för en Azure-prenumeration och ett Azure storage-konto.

Hej ”[felsökningsanvisningar]” i den här guiden beskriver några vanliga problem som uppstod med hello storage-emulatorn.

### <a name="storage-logging-tools"></a>Loggningsverktyg för lagring
Storage-loggning innehåller serversidan loggningen av lagring i ditt Azure storage-konto. Mer information om hur tooenable serversidan loggning och åtkomst hello logga data finns [aktivera loggning för lagring och åtkomst till loggdata](http://go.microsoft.com/fwlink/?LinkId=510867).

hello Storage-klientbibliotek för .NET kan du loggdata för toocollect på klientsidan som relaterar toostorage åtgärder som utförs av ditt program. Mer information finns i [klientsidan loggning med hello Storage-klientbibliotek för .NET](http://go.microsoft.com/fwlink/?LinkId=510868).

> [!NOTE]
> I vissa fall (till exempel SAS auktoriseringsproblem) kan en användare rapportera ett fel som finns det inga data om begäran i hello serversidan lagring loggar. Du kan använda hello funktioner för loggning av hello Storage-klientbibliotek tooinvestigate om hello hello problemet beror på hello klient eller använda verktyg tooinvestigate hello nätverk för nätverksövervakning.
> 
> 

### <a name="using-network-logging-tools"></a>Med hjälp av verktyg för loggning
Du kan avbilda hello trafik mellan hello klienten och servern tooprovide detaljerad information om hello data hello klienten och servern är utbyta och hello underliggande nätverksförhållanden. Användbara verktyg för loggning är:

* [Fiddler](http://www.telerik.com/fiddler) är en gratis webbplatser felsökning proxy som gör att du tooexamine hello sidhuvuden och nyttolasten för HTTP och HTTPS-begäran och svar-meddelanden. Mer information finns i [bilaga 1: med Fiddler toocapture HTTP och HTTPS-trafik](#appendix-1).
* [Microsoft Network Monitor (Netmon)](http://www.microsoft.com/download/details.aspx?id=4865) och [Wireshark](http://www.wireshark.org/) är ledigt nätverk protokollet analyzers som gör att du tooview detaljerad Paketinformation om för en mängd olika nätverksprotokoll. Läs mer om Wireshark ”[bilaga 2: använda Wireshark toocapture nätverkstrafik](#appendix-2)”.
* Microsoft Message Analyzer är ett verktyg från Microsoft som ersätter Netmon och att dessutom toocapturing nätverk paketdata, hjälper dig att tooview och analysera hello loggdata som hämtats från andra verktyg. Mer information finns i ”[tillägg 3: använda Microsoft Message Analyzer toocapture nätverkstrafik](#appendix-3)”.
* Om du vill tooperform en grundläggande anslutningsmöjligheter test toocheck att klientdatorn kan ansluta toohello Azure storage-tjänst hello nätverket kan du inte göra detta med hello standard **ping** verktyget på hello-klienten. Du kan dock använda hello [ **tcping** verktyget](http://www.elifulkerson.com/projects/tcping.php) toocheck anslutning.

I många fall hello loggdata från lagring loggning och hello Storage-klientbibliotek ska vara tillräckligt toodiagnose ett problem, men i vissa fall kan du behöva hello mer detaljerad information som kan tillhandahålla dessa verktyg för loggning. Till exempel med Fiddler tooview HTTP och HTTPS meddelanden aktiverar du tooview huvud och nyttolast data skickas tooand från hello storage-tjänster som gör att du tooexamine hur ett klientprogram försöker lagringsåtgärder. Protokollet analyzers, till exempel Wireshark fungerar med hello paketnivå som gör att du tooview TCP-data, som möjliggör tootroubleshoot förlorat paket och problem med nätverksanslutningen. Message Analyzer kan köras på både HTTP och TCP-lager.

## <a name="end-to-end-tracing"></a>Slutpunkt till slutpunkt-spårning
Spårning för slutpunkt till slutpunkt med ett antal olika loggfiler är en användbar teknik för att undersöka potentiella problem. Du kan använda hello tidsvärdet information från dina data mått som en indikation på där toostart söker i hello loggfiler hello detaljerad information som hjälper dig att felsöka problemet hello.

### <a name="correlating-log-data"></a>Korrelerar loggdata
När du visar loggar från klientprogram spårar nätverk och servern lagring loggning den är kritiska toobe kan toocorrelate begäranden över hello olika loggfilerna. hello loggfiler innehåller ett antal olika fält som är användbara Korrelations-ID: n. id för klientbegäran hello är hello mest användbara fältet toouse toocorrelate poster i hello olika loggar. Men ibland kan det vara användbart toouse hello server begäran-id eller tidsstämplar. hello innehåller följande avsnitt mer information om dessa alternativ.

### <a name="client-request-id"></a>ID för klientbegäran
hello Storage-klientbibliotek genererar automatiskt ett unikt id för klientbegäran för varje begäran.

* I hello klientsidan logg som hello Storage-klientbibliotek skapar, hello id för klientbegäran visas i hello **ID för klientbegäran** i varje loggpost om toohello begäran.
* En spårning i nätverket, till exempel en avbildas Fiddler, hello id för klientbegäran är synligt i begärandemeddelanden som hello **x-ms-client-request-id** värde för HTTP-huvudet.
* I hello serversidan lagring loggning loggen visas hello id för klientbegäran i hello klienten begäran ID-kolumnen.

> [!NOTE]
> Det är möjligt för flera begäranden tooshare hello samma id för klientbegäran eftersom hello klient kan tilldela det här värdet (även om hello Storage-klientbibliotek tilldelar automatiskt ett nytt värde). Hello skiftläget för återförsök hello-klient, delar alla försök hello samma id för klientbegäran. I en batch som skickats från klienten hello hello fallet har hello batch en enskild klient-id för förfrågan.
> 
> 

### <a name="server-request-id"></a>Server-ID för begäran
hello lagringstjänsten genererar automatiskt server begäran-ID: n.

* Hello server begäran-id visas i hello serversidan lagring loggning loggfilen hello **huvud för begäran-ID** kolumn.
* I en spårning i nätverket, till exempel en avbildas Fiddler, hello server begäran-id visas i svarsmeddelanden som hello **x-ms-begäran-id** värde för HTTP-huvudet.
* I hello klientsidan logg som hello Storage-klientbibliotek skapar, hello server begäran-id visas i hello **åtgärden Text** kolumnen för hello loggpost visar information om hello serversvar.

> [!NOTE]
> hello lagringstjänsten tilldelar alltid en unik server begäran-id tooevery begäran tas emot så att varje nytt försök hello-klient och varje åtgärd som ingår i en batch har en unik server-id för förfrågan.
> 
> 

Om hello Storage-klientbibliotek utlöser en **StorageException** hello-klienten hello **RequestInformation** -egenskapen innehåller en **RequestResult** -objekt som innehåller en **ServiceRequestID** egenskapen. Du kan också komma åt en **RequestResult** objekt från en **OperationContext** instans.

hello-kodexempel nedan visar hur tooset en anpassad **ClientRequestId** värde genom att koppla en **OperationContext** objekt hello begäran toohello storage-tjänst. Den visar även hur tooretrieve hello **ServerRequestId** värde från hello svarsmeddelande.

```csharp
//Parse hello connection string for hello storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Create an Operation Context that includes custom ClientRequestId string based on constants defined within hello application along with a Guid.
OperationContext oc = new OperationContext();
oc.ClientRequestID = String.Format("{0} {1} {2} {3}", HOSTNAME, APPNAME, USERID, Guid.NewGuid().ToString());

try
{
    CloudBlobContainer container = blobClient.GetContainerReference("democontainer");
    ICloudBlob blob = container.GetBlobReferenceFromServer("testImage.jpg", null, null, oc);  
    var downloadToPath = string.Format("./{0}", blob.Name);
    using (var fs = File.OpenWrite(downloadToPath))
    {
        blob.DownloadToStream(fs, null, null, oc);
        Console.WriteLine("\t Blob downloaded toofile: {0}", downloadToPath);
    }
}
catch (StorageException storageException)
{
    Console.WriteLine("Storage exception {0} occurred", storageException.Message);
    // Multiple results may exist due tooclient side retry logic - each retried operation will have a unique ServiceRequestId
    foreach (var result in oc.RequestResults)
    {
            Console.WriteLine("HttpStatus: {0}, ServiceRequestId {1}", result.HttpStatusCode, result.ServiceRequestID);
    }
}
```

### <a name="timestamps"></a>Tidsstämplar
Du kan också använda tidsstämplar toolocate relaterade loggposter, men tänk på alla klockavvikelser mellan hello klient och server som kan finnas. Du bör söka plus eller minus 15 minuter för serversidan poster baserat på hello tidsstämpeln på klientens hello-matchning. Kom ihåg att hello blob-metadata för hello blob som innehåller mått anger hello tidsintervall för hello mätvärden som är lagrad i hello blob; Detta är användbart om du har många mått BLOB för hello samma minut eller timme.

## <a name="troubleshooting-guidance"></a>Riktlinjer för felsökning
Det här avsnittet hjälper dig med hello diagnos och felsökning av några vanliga problem med hello ditt program kan uppstå när du använder hello Azure storage-tjänster. Använd hello listan nedan toolocate hello information relevanta tooyour specifikt problem.

**Felsökning av beslutsträdet**

---
Relaterar problemet toohello prestanda på en av hello storage-tjänster?

* [mätvärdena visar AverageE2ELatency hög och låg AverageServerLatency]
* [Mätvärdena visar låg AverageE2ELatency och låg AverageServerLatency men hello-klienten har hög fördröjning]
* [Mätningar visar ett högt värde för AverageServerLatency]
* [Du upplever oväntade fördröjningar i en köad meddelandeleverans]

---
Relaterar problemet toohello tillgängligheten för en av hello storage-tjänster?

* [mätvärdena visar en ökning i PercentThrottlingError]
* [mätvärdena visar en ökning i PercentTimeoutError]
* [Mätningar visar en ökning i PercentNetworkError]

---
 Klientprogrammet tar emot en HTTP-svar som 4XX (till exempel 404) från en lagringstjänsten?

* [hello klienten tar emot HTTP 403 (förbjuden) meddelanden]
* [hello klienten tar emot HTTP 404 (inget hittas) meddelanden]
* [hello klienten tar emot HTTP 409 (konflikt) meddelanden]

---
[mätvärdena visar låg PercentSuccess eller analytics loggposter innehålla åtgärder med status för ClientOtherErrors]

---
[Kapacitetsdata visar en oväntad ökning i kapacitetsförbrukning för lagring]

---
[Det uppstår oväntade omstarter av virtuella datorer som har ett stort antal anslutna virtuella hårddiskar]

---
[Problemet uppstår från att använda hello storage-emulatorn för utveckling eller testning]

---
[Det uppstår problem med att installera hello Azure SDK för .NET]

---
[Du har ett annat problem med en storage-tjänst]

---
### <a name="metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency"></a>Mätvärdena visar AverageE2ELatency hög och låg AverageServerLatency
Hej bilden nedan från hello [Azure-portalen](https://portal.azure.com) övervakning verktyget visar ett exempel där hello **AverageE2ELatency** är avsevärt högre än hello **AverageServerLatency**.

![][4]

Observera att hello lagringstjänsten endast beräknar hello mått **AverageE2ELatency** för lyckade begäranden och till skillnad från **AverageServerLatency**, innehåller hello tid hello klienten tar toosend hello data och ta emot bekräftelse från hello storage-tjänst. Därför skillnad mellan **AverageE2ELatency** och **AverageServerLatency** kan vara antingen på grund av toohello klienten program som långsamma toorespond eller på grund av tooconditions hello nätverket.

> [!NOTE]
> Du kan också visa **E2ELatency** och **ServerLatency** för enskilda lagringsåtgärder i hello lagring loggning logga data.
> 
> 

#### <a name="investigating-client-performance-issues"></a>Undersöka prestandaproblem för klienten
Möjliga orsaker till hello klienten svarar långsamt innehåller med ett begränsat antal tillgängliga anslutningar eller -trådar, eller ha ont om resurser, t.ex CPU, minne eller nätverket bandbredd. Du kan tooresolve hello problemet genom att ändra hello klienten kod toobe effektivare (till exempel med hjälp av asynkrona anrop toohello storage service) eller genom att använda en större virtuell dator (med flera kärnor och mer minne).

För hello tabell och kön tjänster hello Nagle algoritmen kan också orsakas av hög **AverageE2ELatency** jämfört för**AverageServerLatency**: Mer information finns i hello efter [Nagle's Algoritmen är inte egna gentemot små begäran](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx). Du kan inaktivera hello Nagle algoritmen i kod med hjälp av hello **ServicePointManager** klassen i hello **System.Net** namnområde. Du bör göra detta innan du gör någon anrop toohello tabell eller queue-tjänster i ditt program eftersom det inte påverkar anslutningar som redan är öppna. hello följande exempel kommer från hello **Application_Start** metod i en arbetsroll.

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);
ServicePoint tableServicePoint = ServicePointManager.FindServicePoint(storageAccount.TableEndpoint);
tableServicePoint.UseNagleAlgorithm = false;
ServicePoint queueServicePoint = ServicePointManager.FindServicePoint(storageAccount.QueueEndpoint);
queueServicePoint.UseNagleAlgorithm = false;
```

Du bör kontrollera hello klientsidan loggar toosee hur många begäranden klientprogrammet skickar och Sök efter allmän .NET relaterade flaskhalsar i din klient, till exempel CPU, .NET skräpinsamling, belastningen på nätverket eller minne. Som en startpunkt för felsökning av .NET-klientprogram finns [felsökning och spårning Profiling](http://msdn.microsoft.com/library/7fe0dd2y).

#### <a name="investigating-network-latency-issues"></a>Undersöka problem med nätverkssvarstiden
Fördröjningar som slutpunkt till slutpunkt på grund av hello nätverk är vanligtvis på grund av tootransient villkor. Du kan undersöka både tillfälliga och permanenta nätverksproblem, till exempel ignorerade paket med hjälp av verktyg som Wireshark eller Microsoft Message Analyzer.

Mer information om hur du använder Wireshark tootroubleshoot nätverksproblem finns ”[bilaga 2: använda Wireshark toocapture nätverkstrafik]”.

Mer information om hur du använder Microsoft Message Analyzer tootroubleshoot nätverksproblem finns ”[tillägg 3: använda Microsoft Message Analyzer toocapture nätverkstrafik]”.

### <a name="metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency"></a>Mätvärdena visar låg AverageE2ELatency och låg AverageServerLatency men hello-klienten har hög fördröjning
I det här scenariot beror hello antagligen en fördröjning i hello lagringsbegäranden når hello storage-tjänst. Du bör undersöka varför begäranden från hello klient inte gör det via toohello blob-tjänsten.

Hello-klienten att begäranden skickas en möjlig orsak är att det finns ett begränsat antal tillgängliga anslutningar eller -trådar.

Du bör också kontrollera om hello klienten är utför upprepade försök och undersöka hello orsak hello fall. toodetermine om hello klienten utför flera försök, kan du:

* Granska loggarna för hello Storage Analytics. Om flera försök sker, ser du flera åtgärder med samma ID för klientbegäran hello men med annan server begär ID: N.
* Granska loggarna för hello-klienten. Utförlig loggning indikerar att ett nytt försök har genomförts.
* Felsöka din kod och kontrollera hello egenskaper för hello **OperationContext** objektet som är associerat med hello-begäran. Om hello åtgärden har igen hello **RequestResults** egenskap innehåller flera unika serverbegäran ID: N. Du kan också kontrollera hello start- och sluttider för varje begäran. Mer information finns i hello kodexempel i hello avsnittet [Server begäran-ID].

Om det inte finns några problem i hello-klienten, bör du undersöka potentiella nätverksproblem, till exempel paketförlust. Du kan använda verktyg som Wireshark eller Microsoft Message Analyzer tooinvestigate nätverksproblem.

Mer information om hur du använder Wireshark tootroubleshoot nätverksproblem finns ”[bilaga 2: använda Wireshark toocapture nätverkstrafik]”.

Mer information om hur du använder Microsoft Message Analyzer tootroubleshoot nätverksproblem finns ”[tillägg 3: använda Microsoft Message Analyzer toocapture nätverkstrafik]”.

### <a name="metrics-show-high-AverageServerLatency"></a>Mätvärdena visar hög AverageServerLatency
I fallet hello hög **AverageServerLatency** för blob-hämtningsbegäranden, bör du använda hello lagring loggning loggar toosee om upprepade autentiseringsbegäranden hello samma blob (eller uppsättning blobbar). Blob-överföringar, bör du undersöka vilka block storlek hello klienten använder (till exempel block som är mindre än 64 kB kan resultera i kostnaderna om hello läser finns också i mindre än 64 kB blocken), och om flera klienter överför blockerar toohello samma BLOB-parallellt. Du bör också kontrollera hello per minut mätvärden för toppar i hello antal begäranden som resulterar i mer än hello per andra skalbarhetsmål: också se ”[mätvärdena visar en ökning i PercentTimeoutError]”.

Om du ser högt **AverageServerLatency** för blob-hämtningsbegäranden när det upprepas begäranden hello samma blob eller uppsättning blobbar, sedan ska du cache-lagra dessa blobar med hjälp av Azure Cache eller hello Azure Content Delivery Network (CDN). Du kan förbättra hello dataflöde med hjälp av en större blockstorlek för överföringsbegäranden. Frågor tootables är det också möjligt tooimplement klientcachelagring på klienter som utför hello samma fråga åtgärder och hello där data inte ändras ofta.

Hög **AverageServerLatency** värden kan också vara ett symtom på dåligt utformad tabeller eller frågor att resultera i genomsökningen eller som följer hello Lägg till/lägga ett mönster. Se ”[mätvärdena visar en ökning i PercentThrottlingError]” mer information.

> [!NOTE]
> Du hittar en omfattande checklista prestanda checklista här: [Microsoft Azure Storage prestanda och skalbarhet checklista](storage-performance-checklist.md).
> 
> 

### <a name="you-are-experiencing-unexpected-delays-in-message-delivery"></a>Det uppstår oväntade fördröjningar i leverans av meddelanden i en kö
Om det uppstår en fördröjning mellan hello tid ett program lägger till ett meddelande tooa kön och hello tid blir tillgängliga tooread från hello kö och du bör vidta följande steg toodiagnose hello problemet hello:

* Kontrollera att programmet hello har lagt till hello toohello kön. Kontrollera att programmet hello inte försöker på nytt hello **AddMessage** metoden flera gånger innan du lyckas. hello Storage-klientbibliotek loggar visas alla upprepade försök av lagringsåtgärder.
* Kontrollera att det finns ingen klocka skeva mellan hello arbetsroll som lägger till hello meddelandekö toohello och hello arbetsroll som läser hello-meddelande från hello kö som gör det visas som om det uppstår en fördröjning i bearbetningen.
* Kontrollera om hello arbetsroll som läser hälsningsmeddelande från hello kö skadad. Om en kö klient anropar hello **GetMessage** metoden misslyckas men toorespond med en bekräftelse, hello-meddelande är osynliga för hello kön tills hello **invisibilityTimeout** period har löpt ut. Nu blir hello-meddelande tillgängliga för bearbetning av igen.
* Kontrollera om hello Kölängd ökar med tiden. Detta kan inträffa om du inte har tillräcklig arbetare-tillgängliga tooprocess alla hello meddelanden att andra anställda avyttring hello kön. Du bör också kontrollera hello mått toosee om delete-begäranden skickas och hello har status Created antalet på meddelanden som kan tyda på upprepade misslyckade försök toodelete hello-meddelande.
* Granska hello lagring loggning loggarna för alla åtgärder i kön som har högre än förväntat **E2ELatency** och **ServerLatency** värden under en längre tid än vanligt.

### <a name="metrics-show-an-increase-in-PercentThrottlingError"></a>Mätvärdena visar en ökning i PercentThrottlingError
Bandbreddsbegränsning fel uppstår när du överskrider hello skalbarhetsmål för storage-tjänst. hello storage-tjänst har den här tooensure som ingen enskild klient eller innehavare kan använda hello service på hello bekostnad av andra. Mer information finns i [Azure Storage skalbarhets- och prestandamål](storage-scalability-targets.md) detaljer om skalbarhetsmål för storage-konton och prestandamål för partitioner i storage-konton.

Om hello **PercentThrottlingError** måttet visar en ökning i hello procentandelen förfrågningar som misslyckas med felet bandbreddsbegränsning måste du tooinvestigate en av två scenarier:

* [Tillfällig ökning PercentThrottlingError]
* [Permanent ökning PercentThrottlingError fel]

En ökning av **PercentThrottlingError** ofta uppstår på hello samma tid som en ökning av hello antalet förfrågningar om lagring eller när du först läsa testa ditt program. Detta kan också visa sig hello-klienten som ”503 servern upptagen” eller ”tidsgräns för 500 åtgärd” HTTP statusmeddelanden från lagringsåtgärder.

#### <a name="transient-increase-in-PercentThrottlingError"></a>Tillfällig ökning PercentThrottlingError
Om du ser toppar i hello värdet av **PercentThrottlingError** som sammanfaller med perioder med hög aktivitet för hello program bör du implementera ett exponentiell (inte linjär) tillbaka av strategi för nya försök i din klient: detta minskar hello omedelbar belastningen på hello partition och att ditt program toosmooth ut toppar i trafiken. Mer information om hur tooimplement försök principer med hjälp av hello Storage-klientbiblioteket finns [Microsoft.WindowsAzure.Storage.RetryPolicies Namespace](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.retrypolicies.aspx).

> [!NOTE]
> Du kan också se toppar i hello värdet av **PercentThrottlingError** som inte krockar med perioder med hög aktivitet för programmet hello: hello här beror antagligen flytta partitioner tooimprove belastningen hello storage-tjänst belastningsutjämning.
> 
> 

#### <a name="permanent-increase-in-PercentThrottlingError"></a>Permanent ökning PercentThrottlingError fel
Om du ser ett konsekvent högt värde för **PercentThrottlingError** efter en permanent ökning i din transaktionsvolymer eller när du utför din första last testar för ditt program, måste du tooevaluate hur ditt program använder partitioner för lagring och om den närmar sig hello skalbarhetsmål för ett lagringskonto. Till exempel om du ser begränsning fel på en kö (som räknas som en enda partition), bör sedan du använda ytterligare köer toospread hello transaktioner över flera partitioner. Om du ser begränsning fel på en tabell, måste tooconsider med hjälp av en annan partitionering schemat toospread transaktioner över flera partitioner med hjälp av ett brett spektrum av nyckelvärden för partitionen. En vanlig orsak till det här problemet är hello lägga/lägga till ett mönster där du väljer hello datum som hello partitionsnyckel och alla data på en viss dag skrivs tooone partition: belastning detta kan resultera i en flaskhals för skrivning. Du bör du överväga en annan partitionering design, eller så kan du utvärdera om med blob storage kan vara en bättre lösning. Du bör också kontrollera om hello begränsning uppstår till följd av toppar i trafiken och undersöka sätt av Utjämning mönstret för förfrågningar.

Om du distribuerar dina transaktioner över flera partitioner, måste du fortfarande vara medveten om hello skalbarhet gränserna för hello storage-konto. Till exempel om du har använt tio köer varje bearbetning hello högst 2 000 1KB meddelanden per sekund, kommer du att vid hello övergripande gränsen för 20 000 meddelanden per sekund för hello storage-konto. Om du behöver tooprocess mer än 20 000 enheter per sekund, bör du använda flera lagringskonton. Du bör också ha i åtanke att hello storleken på dina önskemål och entiteter har påverkas när hello lagringstjänsten begränsar klienterna: Om du har större begäranden och enheter kan du kanske att begränsas snabbare.

Ineffektiv frågedesigntillståndet kan också medföra toohit hello skalbarhetsbegränsningar för tabellpartitioner. Till exempel måste en fråga med ett filter som endast väljer en procent av hello entiteter i en partition men som söker igenom alla hello entiteter i en partition tooaccess varje entitet. Varje entitet läsa räknas som en hello sammanlagda antalet transaktioner i den aktuella partitionen; Därför kan du enkelt kan nå hello skalbarhetsmål.

> [!NOTE]
> Din prestandatestning bör Visa inga ineffektiv frågan Designer i ditt program.
> 
> 

### <a name="metrics-show-an-increase-in-PercentTimeoutError"></a>Mätvärdena visar en ökning i PercentTimeoutError
Din mätvärdena visar en ökning av **PercentTimeoutError** för en storage-tjänster. Vid hello samma tid, hello klienten tar emot en stor volym med ”tidsgräns för 500 åtgärd” HTTP statusmeddelanden från lagringsåtgärder.

> [!NOTE]
> Du kan se timeout-fel tillfälligt som hello lagringstjänsten läsa in saldon begäranden genom att flytta en partition tooa ny server.
> 
> 

Hej **PercentTimeoutError** mått är en sammanställning av hello följande mått: **ClientTimeoutError**, **AnonymousClientTimeoutError**,  **SASClientTimeoutError**, **ServerTimeoutError**, **AnonymousServerTimeoutError**, och **SASServerTimeoutError**.

Hej server-timeout orsakas av ett fel på hello-server. Hej klienttimeouter bero på att en åtgärd på servern hello har överskridit hello tidsgränsen som anges av hello-klienten. en klient som använder hello Storage-klientbibliotek kan till exempel ange en tidsgräns för en åtgärd med hjälp av hello **ServerTimeout** -egenskapen för hello **QueueRequestOptions** klass.

Server-timeout indikera ett problem med hello storage-tjänst som kräver ytterligare undersökning. Du kan använda toosee mått om du träffa hello skalbarhetsbegränsningar för hello-tjänsten och tooidentify alla toppar i trafik som kan orsaka det här problemet. Om hello problemet är tillfälligt, kanske på grund av tooload-utjämnande aktivitet i hello-tjänsten. Om hello problemet inte orsakas av programmet träffa hello skalbarhetsbegränsningar för hello-tjänsten är beständig, ska du höja ett supportproblem. För klient-timeout måste du bestämma om hello timeout tooan lämpligt värde i hello-klienten och antingen ändra hello timeout-värde som hello klienten eller undersöka hur du kan förbättra prestanda hello hello åtgärder i hello lagringstjänsten för exempel genom att optimera dina frågor för tabellen eller minska hello storleken på dina meddelanden.

### <a name="metrics-show-an-increase-in-PercentNetworkError"></a>Mätvärdena visar en ökning i PercentNetworkError
Din mätvärdena visar en ökning av **PercentNetworkError** för en storage-tjänster. Hej **PercentNetworkError** mått är en sammanställning av hello följande mått: **NetworkError**, **AnonymousNetworkError**, och  **SASNetworkError**. Dessa visas när hello lagringstjänsten uppstår ett nätverksfel uppstod när hello klienten gör en begäran för lagring.

hello vanligaste orsaken till felet är en klient kopplar från innan tidsgränsen upphör att gälla i hello storage-tjänst. Du bör undersöka hello kod i din klient toounderstand när och varför hello klienten kopplas från hello storage-tjänst. Du kan också använda Wireshark, Microsoft Message Analyzer eller Tcping tooinvestigate problem med nätverksanslutningen hello-klient. Dessa verktyg beskrivs i hello [tilläggen].

### <a name="the-client-is-receiving-403-messages"></a>hello klienten tar emot HTTP 403 (förbjuden) meddelanden
Om klientprogrammet har egna HTTP 403 (förbjuden) fel är en trolig orsak att hello-klienten använder en utgången delade signatur åtkomst (SAS) när den skickar en begäran om lagring (även om andra möjliga orsaker inkluderar klockan skeva ogiltig nycklar och tom rubriker). Om en utgången SAS-nyckel är hello orsak, visas inte några poster i hello serversidan lagring logga data för loggning. hello visar följande tabell ett exempel från hello klientsidan loggen genereras av hello Storage-klientbiblioteket som illustrerar det här problemet inträffar:

| Källa | Detaljnivå | Detaljnivå | Id för klientbegäran | Åtgärden text |
| --- | --- | --- | --- | --- |
| Microsoft.WindowsAzure.Storage |Information |3 |85d077ab-... |Starta åtgärden med platsen primära per plats läge PrimaryOnly. |
| Microsoft.WindowsAzure.Storage |Information |3 |85d077ab-... |Starta synkron begära toohttps://domemaildist.blob.core.windows.netazureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp;sr = c&amp;si = mypolicy&amp;sig = OFnd4Rd7z01fIvh % 2BmcR6zbudIH2F5Ikm % 2FyhNYZEmJNQ % 3D&amp;api-version = 2014-02-14. |
| Microsoft.WindowsAzure.Storage |Information |3 |85d077ab-... |Väntar på svar. |
| Microsoft.WindowsAzure.Storage |Varning |2 |85d077ab-... |Ett undantag uppstod under väntan på svar: hello fjärrservern returnerade ett fel: (403) nekad... |
| Microsoft.WindowsAzure.Storage |Information |3 |85d077ab-... |Svar mottogs. Statuskod = 403 begäran-ID = 9d67c64a-64ed-4b0d-9515-3b14bbcdc63d, Content-MD5 = ETag =. |
| Microsoft.WindowsAzure.Storage |Varning |2 |85d077ab-... |Ett undantag uppstod under åtgärden hello: hello fjärrservern returnerade ett fel: (403) nekad... |
| Microsoft.WindowsAzure.Storage |Information |3 |85d077ab-... |Kontrollerar om hello åtgärden bör provas igen. Antal försök = 0, HTTP-statuskod = 403 undantag = hello fjärrservern returnerade ett fel: (403) nekad... |
| Microsoft.WindowsAzure.Storage |Information |3 |85d077ab-... |hello nästa plats har angetts tooPrimary, baserat på hello plats läge. |
| Microsoft.WindowsAzure.Storage |Fel |1 |85d077ab-... |Återförsöksprincipen tillät inte för ett nytt försök. Misslyckas med hello fjärrservern returnerade ett fel: (403) förbjuden. |

Du bör undersöka varför hello SAS-token upphör att gälla innan hello klienten skickar hello token toohello server i det här scenariot:

* Normalt bör du inte ange en starttid som när du skapar en SAS för en klient toouse omedelbart. Om det finns mindre hello klockan skillnaderna mellan hello värden genererar hello SAS med aktuell tid och hello lagringstjänsten, så är det möjligt för hello storage service tooreceive en SAS inte är giltigt ännu.
* Du bör inte ange en mycket kort förfallotid på en SAS. Igen, liten klocka skillnaderna mellan hello värden genererar hello SAS och hello lagringstjänsten kan leda tooa SAS uppenbarligen ut tidigare än förväntat.
* Hello version parameter i hello SAS-nyckeln (till exempel **SA = 2015-04-05**) matchar hello version av hello Storage-klientbiblioteket som du använder? Vi rekommenderar att du alltid använder hello senaste versionen av hello [Storage-klientbibliotek](https://www.nuget.org/packages/WindowsAzure.Storage/).
* Om du återskapar dina åtkomstnycklar för lagring kan detta ogiltigförklara eventuella befintliga SAS-token. Detta kan vara ett problem om du generera en SAS-token med en lång förfallotiden för klienten program toocache.

Om du använder hello Storage-klientbibliotek toogenerate SAS-token, är det enkelt toobuild en giltig token. Men om du använder hello Storage REST API och hur du skapar hello SAS-token manuellt du noga läsa hello avsnittet [delegera åtkomst med en signatur för delad åtkomst](http://msdn.microsoft.com/library/azure/ee395415.aspx).

### <a name="the-client-is-receiving-404-messages"></a>hello klienten tar emot HTTP 404 (inget hittas) meddelanden
Om hello klientprogrammet tar emot en HTTP 404 (inget hittas)-meddelande från hello-servern, innebär detta att hello objektet hello klienten skulle toouse (till exempel en entitet, tabell, blob, behållare eller kön) finns inte i hello storage-tjänst. Det finns ett antal möjliga orsaker till detta, exempelvis:

* [hello-klient eller en annan process som tidigare har tagit bort hello-objektet]
* [Ett problem med auktorisering delade signatur åtkomst (SAS)]
* [Klientens JavaScript-kod har inte behörighet tooaccess hello objekt]
* [Nätverksfel]

#### <a name="client-previously-deleted-the-object"></a>hello-klient eller en annan process som tidigare har tagit bort hello-objektet
I scenarier där hello klienten försöker tooread, uppdatera eller ta bort data i en storage-tjänst är det vanligtvis loggar enkelt tooidentify i hello serversidan en tidigare åtgärd som bort hello aktuella objektet från hello storage-tjänst. Mycket ofta visar hello loggdata som en annan användare eller process borttagna hello-objektet. I hello serversidan lagring loggning loggfilen hello åtgärden typ och begärt Objektnyckel kolumner visar när en klient bort ett objekt.

I hello scenario där en klient försöker tooinsert ett objekt, kanske den inte visar sig omedelbart anledningen till detta resulterar i ett HTTP 404 (inget hittas) svar med tanke på att hello klienten skapar ett nytt objekt. Om hello-klienten skapar en blob som den måste vara kan toofind hello blob-behållaren om hello-klienten skapar ett meddelande måste det vara kan toofind en kö, och om hello klienten är att lägga till en rad måste den vara kan toofind hello tabell.

Du kan använda loggen, hello klientens hello Storage-klientbibliotek toogain en mer detaljerad förståelse för när hello klient skickar specifika begäranden toohello storage-tjänst.

hello illustrerar följande klientsidan loggen genereras av hello Storage-klientbiblioteket hello problem när hello-klienten inte kan hitta hello behållare för hello blob den skapar. Den här loggfilen innehåller information om följande lagringsåtgärder hello:

| ID för begäran | Åtgärd |
| --- | --- |
| 07b26a5d-... |**DeleteIfExists** metoden toodelete hello blob-behållare. Observera att den här åtgärden innehåller en **HEAD** begära toocheck hello befintliga hello behållare. |
| e2d06d78... |**CreateIfNotExists** metoden toocreate hello blob-behållare. Observera att den här åtgärden innehåller en **HEAD** begäran som kontrollerar om hello hello behållaren finns. Hej **HEAD** returnerar ett 404 meddelande men fortsätter. |
| de8b1c3c-... |**UploadFromStream** metoden toocreate hello blob. Hej **PLACERA** begäran misslyckas med ett 404-meddelande |

Loggposter:

| ID för begäran | Åtgärden Text |
| --- | --- |
| 07b26a5d-... |Starta en synkron begäran toohttps://domemaildist.blob.core.windows.net/azuremmblobcontainer. |
| 07b26a5d-... |StringToSign = HEAD...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 Jun 2014 10:33:11 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| 07b26a5d-... |Väntar på svar. |
| 07b26a5d-... |Svar mottogs. Statuskod = 200, ID för begäran = eeead849... Content-MD5 = ETag = &quot;0x8D14D2DC63D059B&quot;. |
| 07b26a5d-... |Svarshuvuden har bearbetats, fortsätter med hello resten av hello igen. |
| 07b26a5d-... |Hämtar brödtext för svar. |
| 07b26a5d-... |Åtgärden har slutförts. |
| 07b26a5d-... |Starta en synkron begäran toohttps://domemaildist.blob.core.windows.net/azuremmblobcontainer. |
| 07b26a5d-... |StringToSign = DELETE...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 Jun 2014 12:10:33 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| 07b26a5d-... |Väntar på svar. |
| 07b26a5d-... |Svar mottogs. Statuskod = 202, ID för begäran = 6ab2a4cf-..., Content-MD5 = ETag =. |
| 07b26a5d-... |Svarshuvuden har bearbetats, fortsätter med hello resten av hello igen. |
| 07b26a5d-... |Hämtar brödtext för svar. |
| 07b26a5d-... |Åtgärden har slutförts. |
| e2d06d78-... |Starta en asynkron begäran toohttps://domemaildist.blob.core.windows.net/azuremmblobcontainer.</td> |
| e2d06d78-... |StringToSign = HEAD...x-ms-client-request-id:e2d06d78-...x-ms-date:Tue, 03 Jun 2014 12:10:33 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| e2d06d78-... |Väntar på svar. |
| de8b1c3c-... |Starta en synkron begäran toohttps://domemaildist.blob.core.windows.net/azuremmblobcontainer/blobCreated.txt. |
| de8b1c3c-... |StringToSign = PUT... 64.qCmF+TQLPhq/YYK50mP9ZQ==...x-MS-BLOB-Type:BlockBlob.x-MS-Client-Request-ID:de8b1c3c-...x-MS-Date:TUE 03 Jun 2014 12:10:33 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer/blobCreated.txt. |
| de8b1c3c-... |Förbereder data för toowrite begäran. |
| e2d06d78-... |Ett undantag uppstod under väntan på svar: hello fjärrservern returnerade ett fel: (404) gick inte att hitta... |
| e2d06d78-... |Svar mottogs. Statuskod = 404 begäran-ID = 353ae3bc-..., Content-MD5 = ETag =. |
| e2d06d78-... |Svarshuvuden har bearbetats, fortsätter med hello resten av hello igen. |
| e2d06d78-... |Hämtar brödtext för svar. |
| e2d06d78-... |Åtgärden har slutförts. |
| e2d06d78-... |Starta en asynkron begäran toohttps://domemaildist.blob.core.windows.net/azuremmblobcontainer. |
| e2d06d78-... |StringToSign = PUT... 0...x-MS-Client-Request-ID:e2d06d78-...x-MS-Date:TUE 03 Jun 2014 12:10:33 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| e2d06d78-... |Väntar på svar. |
| de8b1c3c-... |Begäran när data skrevs. |
| de8b1c3c-... |Väntar på svar. |
| e2d06d78-... |Ett undantag uppstod under väntan på svar: hello fjärrservern returnerade ett fel: (409) konflikt... |
| e2d06d78-... |Svar mottogs. Statuskod = 409, ID för begäran = c27da20e-..., Content-MD5 = ETag =. |
| e2d06d78-... |Hämtar fel svarstexten. |
| de8b1c3c-... |Ett undantag uppstod under väntan på svar: hello fjärrservern returnerade ett fel: (404) gick inte att hitta... |
| de8b1c3c-... |Svar mottogs. Statuskod = 404 begäran-ID = 0eaeab3e-..., Content-MD5 = ETag =. |
| de8b1c3c-... |Ett undantag uppstod under åtgärden hello: hello fjärrservern returnerade ett fel: (404) gick inte att hitta... |
| de8b1c3c-... |Återförsöksprincipen tillät inte för ett nytt försök. Misslyckas med hello fjärrservern returnerade ett fel: (404) gick inte att hitta... |
| e2d06d78-... |Återförsöksprincipen tillät inte för ett nytt försök. Misslyckas med hello fjärrservern returnerade ett fel: (409) konflikt... |

I det här exemplet visar hello loggen hello klienten interfoliering begäranden från hello **CreateIfNotExists** metod (begäran-id e2d06d78...) med hello begäranden från hello **UploadFromStream** metod ( de8b1c3c...); Detta beror på att hello klientprogram anropar asynkront dessa metoder. Du bör ändra hello asynkrona koden i hello klienten tooensure att skapas hello behållare innan du försöker tooupload alla data tooa blob i behållaren. Helst bör du skapa alla behållare i förväg.

#### <a name="SAS-authorization-issue"></a>Ett problem med auktorisering delade signatur åtkomst (SAS)
Om hello klientprogrammet försöker toouse en SAS-nyckel som inte innehåller hello behörighet för åtgärden hello, returnerar en HTTP 404 (inget hittas) meddelande toohello klient hello storage-tjänst. Vid hello samma tid, ser du även ett noll-värde för **SASAuthorizationError** i hello mått.

hello följande tabell visar ett serversidan loggmeddelande exempel från hello lagring loggning loggfilen:

| Namn | Värde |
| --- | --- |
| Starttid för begäran | 2014-05-30T06:17:48.4473697Z |
| Åtgärdstyp     | GetBlobProperties            |
| Status för tjänstbegäran     | SASAuthorizationError        |
| HTTP-statuskod   | 404                          |
| Autentiseringstyp| SAS                          |
| Typ av tjänst       | Blob                         |
| URL-begäran        | https://domemaildist.BLOB.Core.Windows.NET/azureimblobcontainer/blobCreatedViaSAS.txt |
| nbsp;              |   ? SA = 2014-02-14 & sr = c & si = mypolicy & sig = XXXXX&;api-version = 2014-02-14 |
| Huvudet i begäran-id  | a1f348d5-8032-4912-93EF-b393e5252a3b |
| ID för klientbegäran  | 2d064953-8436-4ee0-aa0c-65cb874f7929 |


Du bör undersöka varför ditt klientprogram försöker tooperform en åtgärd som det inte har beviljats behörigheter för.

#### <a name="JavaScript-code-does-not-have-permission"></a>Klientens JavaScript-kod har inte behörighet tooaccess hello objekt
Om du använder en JavaScript-klient och hello lagringstjänsten returnerar HTTP 404-meddelanden, Sök efter hello följande JavaScript-fel i hello webbläsare:

```
SEC7120: Origin http://localhost:56309 not found in Access-Control-Allow-Origin header.
SCRIPT7002: XMLHttpRequest: Network Error 0x80070005, Access is denied.
```

> [!NOTE]
> Du kan använda hello F12 utvecklingsverktygen i Internet Explorer tootrace hello meddelanden som utbyts mellan hello webbläsare och hello lagringstjänsten när du felsöker problem med klientens JavaScript.
> 
> 

Dessa fel inträffa hello webbläsare implementerar hello [samma ursprung princip](http://www.w3.org/Security/wiki/Same_Origin_Policy) säkerhetsbegränsningar som förhindrar att en webbsida anropar en API i en annan domän än hello domän hello sidan kommer från.

toowork runt hello JavaScript problemet kan du konfigurera Cross Origin Resource Sharing (CORS) för hello storage service hello klient har åtkomst till. Mer information finns i [Cross-Origin Resource Sharing (CORS) stöd för Azure Storage-tjänster](http://msdn.microsoft.com/library/azure/dn535601.aspx).

hello följande kodexempel visar hur tooconfigure din blob-tjänst tooallow JavaScript körs i hello Contoso-domänen tooaccess en blobb i ditt blob storage-tjänst:

```csharp
CloudBlobClient client = new CloudBlobClient(blobEndpoint, new StorageCredentials(accountName, accountKey));
// Set hello service properties.
ServiceProperties sp = client.GetServiceProperties();
sp.DefaultServiceVersion = "2013-08-15";
CorsRule cr = new CorsRule();
cr.AllowedHeaders.Add("*");
cr.AllowedMethods = CorsHttpMethods.Get | CorsHttpMethods.Put;
cr.AllowedOrigins.Add("http://www.contoso.com");
cr.ExposedHeaders.Add("x-ms-*");
cr.MaxAgeInSeconds = 5;
sp.Cors.CorsRules.Clear();
sp.Cors.CorsRules.Add(cr);
client.SetServiceProperties(sp);
```

#### <a name="network-failure"></a>Nätverksfel
I vissa fall kan förlorade nätverkspaket leda toohello lagringstjänsten returnera HTTP 404-meddelanden toohello klienten. Till exempel när klientprogrammet tas bort en entitet från tabelltjänsten hello finns hello klienten utlösa en lagring undantag reporting ett ”HTTP 404 (inget hittas)” statusmeddelande från hello tabelltjänsten. När du undersöker hello tabellen i hello table storage-tjänsten finns hello-tjänsten tog bort hello entitet som begärs.

hello undantagsinformation hello-klienten innehåller hello begäran-id (7e84f12d...) tilldelas av hello tabelltjänsten för hello begäran: du kan använda denna information toolocate hello begäran detaljer i hello serversidan lagring loggarna genom att söka i hello  **huvud för begäran-id** kolumn i hello loggfilen. Du kan också använda hello mått tooidentify när fel som detta inträffa och sök sedan hello loggfiler baserat på hello hello tidsintervallen registreras det här felet. Den här loggen posten visas som hello delete misslyckades med ett statusmeddelande för ”HTTP (404) klient fel”. hello samma loggpost även hello begärande-id har genererats av hello klienten i hello **client-request-id** kolumn (813ea74f...).

hello serversidan loggen innehåller även en annan transaktion med hello samma **client-request-id** värde (813ea74f...) för en lyckad borttagningsåtgärden för hello samma entitet och hello från samma klient. Borttagningsåtgärden lyckas ta ägde rum precis innan hello misslyckade raderingsbegäran.

hello troligaste orsaken till det här scenariot är att hello klienten har skickat en begäran om borttagning för hello entiteten toohello tabell-tjänsten som har slutförts men tog inte emot en bekräftelse från hello server (kanske på grund av tooa tillfälliga nätverksproblem). hello klienten sedan automatiskt igen hello åtgärden (med hjälp av hello samma **client-request-id**), och den här nya försöket misslyckades eftersom hello entiteten hade redan tagits bort.

Om det här problemet uppstår ofta bör du undersöka varför hello klienten inte tooreceive bekräftelser från hello tabelltjänsten. Om hello problemet är tillfälligt, bör du trap hello ”HTTP (404) kunde inte hittas” fel och logga in hello klienten, men Tillåt hello klienten toocontinue.

### <a name="the-client-is-receiving-409-messages"></a>hello klienten tar emot HTTP 409 (konflikt) meddelanden
hello i tabellen nedan visas ett utdrag ur hello serversidan loggen för två Klientåtgärder: **DeleteIfExists** följt av omedelbart **CreateIfNotExists** med hello samma blob-behållarnamn. Observera att varje klientåtgärden resulterar i två begäranden skickas toohello server först en **GetContainerProperties** toocheck begäran om hello-behållaren finns, följt av hello **DeleteContainer** eller  **CreateContainer** begäran.

| tidsstämpel | Åtgärd | Resultat | Behållarens namn | Id för klientbegäran |
| --- | --- | --- | --- | --- |
| 05:10:13.7167225 |GetContainerProperties |200 |mmcont |c9f52c89-... |
| 05:10:13.8167325 |DeleteContainer |202 |mmcont |c9f52c89-... |
| 05:10:13.8987407 |GetContainerProperties |404 |mmcont |bc881924-... |
| 05:10:14.2147723 |CreateContainer |409 |mmcont |bc881924-... |

hello koden i klientprogrammet för hello tar bort och sedan omedelbart återskapar en blob-behållare med hello samma namn: hello **CreateIfNotExists** metod (klienten begär ID bc881924-...) så småningom misslyckas med hello HTTP 409 (konflikt) fel. När en klient tar bort blob-behållare, tabeller eller köer som det finns en kort period innan hello namn blir tillgänglig igen.

hello klientprogram ska använda unikt behållarnamn när du skapar den nya behållare om hello ta bort/återskapa mönstret är vanligt.

### <a name="metrics-show-low-percent-success"></a>Mätvärdena visar låg PercentSuccess eller analytics loggposter innehålla åtgärder med transaktionsstatus för ClientOtherErrors
Hej **PercentSuccess** mått fångar hello procent av åtgärder som har genomförts baserat på HTTP-statuskod. Åtgärder med statuskoder av 2XX räknas som slutförd medan åtgärder med statuskoder i 3XX, 4XX och 5XX intervall räknas som misslyckades och lägre hello **PercentSucess** värde. I hello serversidan lagring loggfiler åtgärderna registreras med transaktionsstatus **ClientOtherErrors**.

Det är viktigt toonote att dessa åtgärder har slutförts och därför inte påverkar andra mått, till exempel tillgänglighet. Exempel på åtgärder som köras men som kan leda till misslyckade HTTP-statuskoder är:

* **ResourceNotFound** (inte att hitta 404), till exempel från GET begäran tooa blob som inte finns.
* **ResouceAlreadyExists** (konflikt 409), till exempel från ett **CreateIfNotExist** åtgärden där hello resursen finns redan.
* **ConditionNotMet** (inte ändrade 304), till exempel från en villkorlig åtgärd, till exempel när en klient skickar en **ETag** värde och en HTTP- **If-None-Match** huvud toorequest om en avbildning Det har uppdaterats sedan hello senaste åtgärden.

Du hittar en lista över vanliga felkoder för REST-API som hello lagringstjänster returnerar på hello sidan [vanliga felkoder för REST-API](http://msdn.microsoft.com/library/azure/dd179357.aspx).

### <a name="capacity-metrics-show-an-unexpected-increase"></a>Kapacitetsdata visar en oväntad ökning i kapacitetsförbrukning för lagring
Om du ser plötslig oväntade ändringar i kapacitetsförbrukning i ditt lagringskonto, du kan undersöka hello orsaker genom att studera din mått för tillgänglighet; till exempel kan en ökning i hello antal misslyckade begäranden om borttagning leda tooan öka hello mängd blob-lagring som du använder som programmet specifika rensningsåtgärder så kan du ha förväntats toobe Frigör utrymme inte kanske fungerar som förväntat (för exempelvis eftersom hello SAS-token som används för att frigöra utrymme har upphört att gälla).

### <a name="you-are-experiencing-unexpected-reboots"></a>Det uppstår oväntade omstarter av Azure virtuella datorer som har ett stort antal anslutna virtuella hårddiskar
Om Azure virtuell dator (VM) har ett stort antal anslutna virtuella hårddiskar som tillhör hello samma lagringskonto kan överstiga hello skalbarhetsmål för en enskild lagringskontot orsakar hello VM toofail. Du bör kontrollera hello minut mätvärden för hello storage-konto (**TotalRequests**/**TotalIngress**/**TotalEgress**) för toppar som överstiger hello skalbarhetsmål för ett lagringskonto. Hello i avsnittet ”[mätvärdena visar en ökning i PercentThrottlingError]” för stöd för att fastställa om begränsning har inträffat i ditt lagringskonto.

I allmänhet varje enskild indata eller utdata-åtgärden på en VHD från en virtuell dator översätter för**hämta sidan** eller **placera sidan** åtgärder på hello underliggande sidblob. Därför kan du använda hello beräknade IOPS för din miljö tootune hur många virtuella hårddiskar kan i ett enda lagringskonto baserat på hello specifikt beteende för programmet. Vi rekommenderar inte att ha fler än 40 diskar i ett enda storage-konto. Se [Azure Storage skalbarhets- och prestandamål](storage-scalability-targets.md) mer information om hello aktuella skalbarhetsmål för storage-konton, i synnerhet hello totala begäran hastighet och den totala bandbredden för hello typ av lagringskonto du använder .
Om du överstiger hello skalbarhetsmål för ditt lagringskonto, bör du placera de virtuella hårddiskarna i flera olika konton tooreduce hello lagringsaktiviteten i varje enskilt konto.

### <a name="your-issue-arises-from-using-the-storage-emulator"></a>Problemet uppstår från att använda hello storage-emulatorn för utveckling eller testning
Du vanligtvis använder hello storage-emulatorn under utvecklingen och testa tooavoid hello krav för Azure storage-konto. hello vanliga problem som kan uppstå när du använder hello storage-emulatorn är:

* [Funktionen ”X” fungerar inte i hello storage-emulatorn]
* [Fel ”hello-värdet för en hello HTTP-huvuden är inte i rätt format för hello” när med hello storage-emulatorn]
* [Körs hello storage-emulatorn kräver administratörsbehörighet]

#### <a name="feature-X-is-not-working"></a>Funktionen ”X” fungerar inte i hello storage-emulatorn
hello storage-emulatorn stöder inte alla hello funktioner i hello Azure storage-tjänster som hello filtjänst. Mer information finns i [Använd hello Azure Storage-emulatorn för utveckling och testning](storage-use-emulator.md).

För de funktioner som hello lagring emulatorn inte stöd för använder hello Azure storage-tjänst i molnet hello.

#### <a name="error-HTTP-header-not-correct-format"></a>Fel ”hello-värdet för en hello HTTP-huvuden är inte i rätt format för hello” när med hello storage-emulatorn
Du testar programmet som använder hello Storage-klientbibliotek mot hello lokal lagring emulatorn och metoden samtal som **CreateIfNotExists** misslyckas med felet hello meddelandet ”hello-värdet för en hello HTTP-huvuden är inte i hello rätt format ”. Detta anger att hello version av hello storage-emulatorn som du använder stöder inte hello version av du använder hello-storage-klientbiblioteket. hello Storage-klientbibliotek lägger till hello sidhuvud **x-ms-version** tooall hello begär den gör. Om hello storage-emulatorn inte känner igen hello värdet i hello **x-ms-version** rubrik som är den hello avslår.

Du kan använda hello lagring Biblioteksklient loggar toosee hello värdet för hello **x-ms-versionshuvud** som skickas. Du kan också se hello värdet för hello **x-ms-versionshuvud** om du använder Fiddler tootrace hello begäranden från klientprogrammet.

Detta inträffar vanligtvis om du installerar och använder hello senaste versionen av hello Storage-klientbibliotek utan att uppdatera hello storage-emulatorn. Du bör installera hello senaste versionen av hello storage-emulatorn eller använda molnlagring i stället för hello-emulatorn för utveckling och testning.

#### <a name="storage-emulator-requires-administrative-privileges"></a>Körs hello storage-emulatorn kräver administratörsbehörighet
Du tillfrågas om autentiseringsuppgifter när du kör hello storage-emulatorn. Det här inträffar bara när du initierar hello storage-emulatorn för hello första gången. När du har initierat hello storage-emulatorn, behöver du inte administrativa privilegier toorun den igen.

Mer information finns i [Använd hello Azure Storage-emulatorn för utveckling och testning](storage-use-emulator.md). Observera att du kan också initiera hello storage-emulatorn i Visual Studio, vilket även kräver administratörsbehörighet.

### <a name="you-are-encountering-problems-installing-the-Windows-Azure-SDK"></a>Det uppstår problem med att installera hello Azure SDK för .NET
När du försöker tooinstall hello SDK, misslyckas försöker tooinstall hello lagringsemulatorn på den lokala datorn. hello installationsloggen innehåller något av följande meddelanden hello:

* CAQuietExec: Fel: Det gick inte tooaccess SQL-instans
* CAQuietExec: Fel: Det gick inte toocreate databas

hello orsaken är ett problem med en befintlig LocalDB-installation. Som standard används hello storage-emulatorn LocalDB toopersist data när den simulerar hello Azure storage-tjänster. Du kan återställa din LocalDB-instans genom att köra följande kommandon i ett Kommandotolk fönster innan du försöker tooinstall hello SDK hello.

```
sqllocaldb stop v11.0
sqllocaldb delete v11.0
delete %USERPROFILE%\WAStorageEmulatorDb3*.*
sqllocaldb create v11.0
```

Hej **ta bort** kommandot tar bort eventuella gamla databasfiler från en tidigare installation av hello storage-emulatorn.

### <a name="you-have-a-different-issue-with-a-storage-service"></a>Du har ett annat problem med en storage-tjänst
Om hello tidigare felsökningsavsnitt innehåller inte inkluderar hello problem som du har med en storage-tjänst, bör du vidta hello följande metod toodiagnosing och felsöka problemet.

* Kontrollera din mått toosee om det finns ändringar från din förväntade baslinjen beteende. Från hello statistik och, du kan toodetermine om hello problemet är tillfälligt eller permanent och vilka lagringsåtgärder hello problemet påverkar.
* Du kan använda hello mått information toohelp du söka efter loggdata serversidan för mer detaljerad information om fel som uppstår. Den här informationen kan hjälpa dig att felsöka och lösa hello problemet.
* Om hello information i hello serversidan loggar inte är tillräckligt tootroubleshoot hello problemet har, kan du använda hello Storage-klientbibliotek klientsidan loggar tooinvestigate hello beteendet klientprogrammet och verktyg som Fiddler, Wireshark, Microsoft Message Analyzer tooinvestigate och nätverket.

Mer information om hur du använder Fiddler finns ”[bilaga 1: med Fiddler toocapture HTTP och HTTPS-trafik]”.

Mer information om hur du använder Wireshark finns ”[bilaga 2: använda Wireshark toocapture nätverkstrafik]”.

Mer information om hur du använder Microsoft Message Analyzer finns ”[tillägg 3: använda Microsoft Message Analyzer toocapture nätverkstrafik]”.

## <a name="appendices"></a>Tilläggen
hello tilläggen beskrivs flera verktyg som kan vara praktiskt när du diagnostiserar och felsökning av problem med Azure Storage (och andra tjänster). Dessa verktyg är inte en del av Azure Storage och vissa är produkter från tredje part. Hello-verktyg som beskrivs i de här tilläggen omfattas inte av några supportavtal som du kan ha med Microsoft Azure eller Azure Storage som sådana och därför som en del av din utvärdering av du bör undersöka hello licensiering och supportalternativ finns i hello leverantörer av dessa verktyg.

### <a name="appendix-1"></a>Bilaga 1: Med Fiddler toocapture HTTP och HTTPS-trafik
[Fiddler](http://www.telerik.com/fiddler) är användbart för att analysera hello HTTP och HTTPS-trafik mellan klientprogrammet och hello Azure storage-tjänst som du använder.

> [!NOTE]
> Fiddler kan avkoda HTTPS-trafik. Du bör läsa hello Fiddler dokumentationen noggrant toounderstand hur det fungerar, och toounderstand hello säkerhetsaspekter.
> 
> 

Den här bilagan innehåller en kort genomgång av hur tooconfigure Fiddler toocapture trafik mellan hello lokala datorn där du har installerat Fiddler och hello Azure storage-tjänster.

När du har öppnat Fiddler börjar samla in HTTP och HTTPS-trafik på den lokala datorn. hello nedan följer några användbara kommandon för att styra Fiddler:

* Stoppa och starta fånga in trafik. Hello Huvudmeny, gå för**filen** och klicka sedan på **fånga in trafik** tootoggle avbildning på och stänga av.
* Spara insamlade trafikinformation. Hello Huvudmeny, gå för**filen**, klickar du på **spara**, och klicka sedan på **alla sessioner**: Detta gör att du toosave hello trafik i en Session arkivfilen. Du kan läsa in ett Session-Arkiv senare för analys, eller skicka den om begärt tooMicrosoft support.

toolimit hello mängden trafik som samlar in Fiddler, du kan använda filter som du konfigurerar i hello **filter** fliken hello följande skärmbild visar ett filter som samlar in endast trafik som skickas toohello  **contosoemaildist.Table.Core.Windows.NET** lagring slutpunkt:

![][5]

### <a name="appendix-2"></a>Bilaga 2: Använda Wireshark toocapture nätverkstrafik
[Wireshark](http://www.wireshark.org/) är en nätverksprotokollanalysator som gör att du tooview detaljerad Paketinformation om för en mängd olika nätverksprotokoll.

hello följande procedur visar hur toocapture detaljerad paketinformationen för trafik från hello lokal dator där du installerade Wireshark toohello tabelltjänsten i Azure storage-konto.

1. Starta Wireshark på din lokala dator.
2. I hello **starta** avsnitt, Välj hello lokalt nätverksgränssnitt eller gränssnitt som är anslutna toohello internet.
3. Klicka på **avbilda alternativ**.
4. Lägg till ett filter toohello **Capture Filter** textruta. Till exempel **värd contosoemaildist.table.core.windows.net** konfigurerar Wireshark toocapture endast paket skickas tooor från hello tabelltjänst i hello **contosoemaildist** lagring konto. Kolla in hello [fullständig lista över avbilda filter](http://wiki.wireshark.org/CaptureFilters).
   
   ![][6]
5. Klicka på **starta**. Wireshark avbildar nu alla hello paket skicka tooor från hello tabelltjänst som du använder klientprogrammet på den lokala datorn.
6. När du är klar på hello huvudmenyn Klicka **avbilda** och sedan **stoppa**.
7. toosave hello infångade data i en Wireshark Capture-fil på hello huvudmenyn Klicka **filen** och sedan **spara**.

WireShark markerar du eventuella fel som finns i hello **packetlist** fönster. Du kan också använda hello **Expert Info** fönstret (klicka på **analysera**, sedan **Expert Info**) tooview en sammanfattning av fel och varningar.

![][7]

Valt tooview hello TCP-data som hello programnivå ser den genom att högerklicka på hello TCP-data och välja **följer TCP-ström**. Detta är särskilt användbart om du har hämtat dina dump utan ett filter för avbildning. Mer information finns i [följande TCP-strömmar](http://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowTCPSection.html).

![][8]

> [!NOTE]
> Mer information om hur du använder Wireshark finns hello [Wireshark användarhandboken](http://www.wireshark.org/docs/wsug_html_chunked).
> 
> 

### <a name="appendix-3"></a>Tillägg 3: Använda Microsoft Message Analyzer toocapture nätverkstrafik
Du kan använda Microsoft Message Analyzer toocapture HTTP och HTTPS-trafik i ett liknande sätt tooFiddler och avbilda nätverkstrafik på ett liknande sätt tooWireshark.

#### <a name="configure-a-web-tracing-session-using-microsoft-message-analyzer"></a>Konfigurera en session för spårning av webbprogram med hjälp av Microsoft Message Analyzer
tooconfigure en spårning webbsessionen för HTTP och HTTPS-trafik med hjälp av Microsoft Message Analyzer kör hello Microsoft Message Analyzer program och klicka sedan på hello **filen** -menyn klickar du på **avbilda/Trace**. Markera i hello lista över tillgängliga trace scenarier, **webbproxy**. I hello **scenariot spårningskonfiguration** panelen i hello **HostnameFilter** textruta lägga till hello namnen på dina slutpunkter för lagring (du kan slå upp dessa namn i hello [Azure-portalen](https://portal.azure.com)). Till exempel om hello namnet på ditt Azure storage-konto är **contosodata**, bör du lägga till följande toohello hello **HostnameFilter** textruta:

```
contosodata.blob.core.windows.net contosodata.table.core.windows.net contosodata.queue.core.windows.net
```

> [!NOTE]
> Blanksteg avgränsar hello värdnamn.
> 
> 

När du är klar toostart insamling av spårningsdata klickar du på hello **starta med** knappen.

Mer information om hello Microsoft Message Analyzer **webbproxy** spåra, se [Provider för Microsoft-PEF-WebProxy](http://technet.microsoft.com/library/jj674814.aspx).

hello inbyggda **webbproxy** spårning i Microsoft Message Analyzer baseras på Fiddler; den kan samla in klientsidans HTTPS-trafik och visa okrypterade HTTPS-meddelanden. Hej **webbproxy** spåra fungerar genom att konfigurera en lokal proxyserver för alla HTTP och HTTPS-trafik som ger den åtkomst toounencrypted meddelanden.

#### <a name="diagnosing-network-issues-using-microsoft-message-analyzer"></a>Diagnostisera problem med Microsoft Message Analyzer
Dessutom toousing hello Microsoft Message Analyzer **webbproxy** trace toocapture information om Hej HTTP/HTTPs-trafik mellan hello klientprogrammet och hello lagringstjänsten, du kan också använda hello inbyggda  **Lokala länknivå** spårningsinformation toocapture nätverk paket. Det här kan du toocapture data liknande toothat som du kan avbilda med Wireshark och diagnostisera nätverk problem som tappade paket.

hello följande skärmbild visar ett exempel **lokala länknivå** spårningen med några **informationsmeddelande** meddelanden i hello **DiagnosisTypes** kolumn. Klicka på en ikon i hello **DiagnosisTypes** kolumnen visar hello information om hello-meddelande. I det här exemplet hello-server som skickats om meddelandet #305 eftersom den inte fick en bekräftelse från hello klient:

![][9]

När du skapar hello spårningssessionen i Microsoft Message Analyzer kan du ange filter tooreduce hello mycket brus i hello spårning. På hello **avbilda / spåra** sida där du definierar hello spårningen, klicka på hello **konfigurera** länka bredvid för**Microsoft-Windows-NDIS-PacketCapture**. hello följande skärmbild visar en konfiguration som filtrerar TCP-trafik för hello IP-adresser i tre lagringstjänster:

![][10]

Läs mer om hello Microsoft Message Analyzer lokala länknivå trace [Provider för Microsoft-PEF-NDIS-PacketCapture](http://technet.microsoft.com/library/jj659264.aspx).

### <a name="appendix-4"></a>Tillägg 4: Använda Excel tooview mått och loggar data
Många verktyg kan du toodownload hello Storage Metrics data från Azure-tabellagring i en avgränsad format som gör det enkelt tooload hello data till Excel för visning och analys. Lagring loggningsdata från Azure blob storage finns redan i en avgränsad format som du kan läsa in i Excel. Dock behöver du tooadd lämplig kolumnrubrikerna i hello information [Storage Analytics loggformatet](http://msdn.microsoft.com/library/azure/hh343259.aspx) och [Storage Analytics mätvärden tabellschemat](http://msdn.microsoft.com/library/azure/hh343264.aspx).

tooimport lagring logga data till Excel när du hämta det från blob storage:

* På hello **Data** -menyn klickar du på **från Text**.
* Bläddra toohello loggfilen tooview och på **importera**.
* Steg 1 av hello på **guiden**väljer **avgränsad**.

Steg 1 av hello på **guiden**väljer **semikolon** som hello endast avgränsare och välj dubbla citattecken som hello **textavgränsare**. Klicka på **Slutför** och välj där tooplace hello data i din arbetsbok.

### <a name="appendix-5"></a>Tillägg 5: Övervaka med Programinsikter för Visual Studio Team Services
Du kan också använda funktionen för hello Application Insights för Visual Studio Team Services som en del av prestanda och tillgänglighetsövervakning. Det här verktyget kan:

* Kontrollera att webbtjänsten är tillgänglig och svarstid. Om din app är en webbplats eller en enhetsapp som använder en webbtjänst kan det Testa URL: en med några minuters mellanrum från platser runt hello world och att du vet om det finns ett problem.
* Snabbt diagnostisera eventuella problem med prestanda eller undantag i webbtjänsten. Ta reda på om CPU eller andra resurser är att sträckas ut, hämta stackspår från undantag och enkelt söka igenom loggspårningar. Om hello appens prestandaförsämring under godtagbara gränser, vi kan skicka ett e-postmeddelande. Du kan övervaka både .NET och Java-webbtjänster.

Du hittar mer information i [vad är Application Insights?](../../application-insights/app-insights-overview.md).

<!--Anchors-->
[Introduktion]: #introduction
[Hur den här guiden är organiserat]: #how-this-guide-is-organized

[övervakning lagringstjänsten]: #monitoring-your-storage-service
[Övervakning av tjänstens hälsa]: #monitoring-service-health
[Övervakning av kapacitet]: #monitoring-capacity
[Övervaka tillgänglighet]: #monitoring-availability
[Övervaka prestanda]: #monitoring-performance

[diagnostisera problem med lagring]: #diagnosing-storage-issues
[Hälsotillståndsproblem med tjänsten för]: #service-health-issues
[Prestandaproblem]: #performance-issues
[Diagnostisera fel]: #diagnosing-errors
[Storage-emulatorn problem]: #storage-emulator-issues
[Loggningsverktyg för lagring]: #storage-logging-tools
[Med hjälp av verktyg för loggning]: #using-network-logging-tools

[slutpunkt till slutpunkt spårning]: #end-to-end-tracing
[Korrelerar loggdata]: #correlating-log-data
[ID för klientbegäran]: #client-request-id
[Server begäran-ID]: #server-request-id
[Tidsstämplar]: #timestamps

[felsökningsanvisningar]: #troubleshooting-guidance
[mätvärdena visar AverageE2ELatency hög och låg AverageServerLatency]: #metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency
[Mätvärdena visar låg AverageE2ELatency och låg AverageServerLatency men hello-klienten har hög fördröjning]: #metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency
[Mätningar visar ett högt värde för AverageServerLatency]: #metrics-show-high-AverageServerLatency
[Du upplever oväntade fördröjningar i en köad meddelandeleverans]: #you-are-experiencing-unexpected-delays-in-message-delivery

[mätvärdena visar en ökning i PercentThrottlingError]: #metrics-show-an-increase-in-PercentThrottlingError
[Tillfällig ökning PercentThrottlingError]: #transient-increase-in-PercentThrottlingError
[Permanent ökning PercentThrottlingError fel]: #permanent-increase-in-PercentThrottlingError
[mätvärdena visar en ökning i PercentTimeoutError]: #metrics-show-an-increase-in-PercentTimeoutError
[Mätningar visar en ökning i PercentNetworkError]: #metrics-show-an-increase-in-PercentNetworkError

[hello klienten tar emot HTTP 403 (förbjuden) meddelanden]: #the-client-is-receiving-403-messages
[hello klienten tar emot HTTP 404 (inget hittas) meddelanden]: #the-client-is-receiving-404-messages
[hello-klient eller en annan process som tidigare har tagit bort hello-objektet]: #client-previously-deleted-the-object
[Ett problem med auktorisering delade signatur åtkomst (SAS)]: #SAS-authorization-issue
[Klientens JavaScript-kod har inte behörighet tooaccess hello objekt]: #JavaScript-code-does-not-have-permission
[Nätverksfel]: #network-failure
[hello klienten tar emot HTTP 409 (konflikt) meddelanden]: #the-client-is-receiving-409-messages

[mätvärdena visar låg PercentSuccess eller analytics loggposter innehålla åtgärder med status för ClientOtherErrors]: #metrics-show-low-percent-success
[Kapacitetsdata visar en oväntad ökning i kapacitetsförbrukning för lagring]: #capacity-metrics-show-an-unexpected-increase
[Det uppstår oväntade omstarter av virtuella datorer som har ett stort antal anslutna virtuella hårddiskar]: #you-are-experiencing-unexpected-reboots
[Problemet uppstår från att använda hello storage-emulatorn för utveckling eller testning]: #your-issue-arises-from-using-the-storage-emulator
[Funktionen ”X” fungerar inte i hello storage-emulatorn]: #feature-X-is-not-working
[Fel ”hello-värdet för en hello HTTP-huvuden är inte i rätt format för hello” när med hello storage-emulatorn]: #error-HTTP-header-not-correct-format
[Körs hello storage-emulatorn kräver administratörsbehörighet]: #storage-emulator-requires-administrative-privileges
[Det uppstår problem med att installera hello Azure SDK för .NET]: #you-are-encountering-problems-installing-the-Windows-Azure-SDK
[Du har ett annat problem med en storage-tjänst]: #you-have-a-different-issue-with-a-storage-service

[tilläggen]: #appendices
[bilaga 1: med Fiddler toocapture HTTP och HTTPS-trafik]: #appendix-1
[bilaga 2: använda Wireshark toocapture nätverkstrafik]: #appendix-2
[tillägg 3: använda Microsoft Message Analyzer toocapture nätverkstrafik]: #appendix-3
[Tillägg 4: Använda Excel tooview mått och loggar data]: #appendix-4
[Tillägg 5: Övervaka med Programinsikter för Visual Studio Team Services]: #appendix-5

<!--Image references-->
[1]: ./media/storage-monitoring-diagnosing-troubleshooting/overview.png
[3]: ./media/storage-monitoring-diagnosing-troubleshooting/hour-minute-metrics.png
[4]: ./media/storage-monitoring-diagnosing-troubleshooting/high-e2e-latency.png
[5]: ./media/storage-monitoring-diagnosing-troubleshooting/fiddler-screenshot.png
[6]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-1.png
[7]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-2.png
[8]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-3.png
[9]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-1.png
[10]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-2.png
