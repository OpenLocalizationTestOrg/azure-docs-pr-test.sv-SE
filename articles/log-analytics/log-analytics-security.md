---
title: "aaaLog Analytics datasäkerhet | Microsoft Docs"
description: "Läs mer om hur logganalys skyddar din integritet och skyddar dina data."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: a33bb05d-b310-4f2c-8f76-f627e600c8e7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: magoedte
ms.openlocfilehash: 130b59f22fc3dd249f32717367cc62ea25c55a21
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-data-security"></a>Logga Analytics datasäkerhet
Microsoft allokerat tooprotecting din integritet och skydda dina data, samtidigt som de levererar programvaror och tjänster som hjälper dig att hantera hello IT-infrastrukturen i organisationen. Vi förstår att när du ge dina data tooothers förtroendet kräver omfattande säkerhet. Microsoft följer riktlinjer för toostrict efterlevnad och säkerhet – från kodning toooperating en tjänst.

Säkra och skydda data är högsta prioritet på Microsoft. Kontakta oss med frågor, förslag och problem om något av följande information, inklusive vår säkerhetsprinciper på hello [Azure supportalternativ](http://azure.microsoft.com/support/options/).

Den här artikeln förklarar hur data som samlas in, bearbetas och skyddas av logganalys i hello Operations Management Suite (OMS). Du kan använda agenter tooconnect toohello-webbtjänsten, använda användningsdata för System Center Operations Manager toocollect eller hämta data från Azure-diagnostik för användning av logganalys. hello insamlade data skickas över hello Internet med hjälp av certifikatbaserad autentisering och SSL-3 toohello logganalys-tjänsten, som finns i Microsoft Azure. Data komprimeras av hello agent innan den skickas.

hello logganalys-tjänsten hanterar molnbaserade data på ett säkert sätt med hjälp av hello följande metoder:

* Dataavgränsning
* datalagring
* Fysisk säkerhet
* Hantering av incidenter
* Kompatibilitet
* standarder Säkerhetscertifieringar

## <a name="data-segregation"></a>Dataavgränsning
Kundinformation lagras logiskt separat på varje komponent i hela hello OMS-tjänsten. Alla data taggas efter organisation. Den här taggning kvarstår under hello data livscykel och den tillämpas på varje nivå av hello-tjänsten. Varje kund har en dedikerad Azure blob där hello långsiktiga data

## <a name="data-retention"></a>Datakvarhållning
Indexerade Sök loggdata lagras och behålls bl.a tooyour priser plan. Mer information finns i [Log Analytics priser](https://azure.microsoft.com/pricing/details/log-analytics/).

Microsoft tar bort kundinformation 30 dagar efter hello OMS-arbetsytan har stängts. Microsoft tar också bort hello Azure Storage-konto där hello data finns. Inga fysiska enheter förstörs när kundens data tas bort.

hello följande tabell listar några av hello tillgängliga lösningar i OMS och exempel hello typer av information som samlas in.

| **Lösning** | **Datatyper** |
| --- | --- |
| Konfigurationsbedömning |Konfigurationsdata, metadata och systemtillstånd |
| Kapacitetsplanering |Prestandadata och metadata |
| Programvara mot skadlig kod |Konfigurationsdata och metadata |
| Systemuppdateringsbedömning |Metadata och tillståndsdata |
| Logghantering |Användardefinierade händelseloggar, Windows-händelseloggar och/eller IIS-loggar |
| Spårning av ändringar |Programvaruinventering och metadata för Windows-tjänst |
| SQL- och Active Directory-utvärdering |WMI-data, registerdata, prestandadata och SQL Server dynamiska hanteringsvyn visa resultat |

hello följande tabell visas exempel på datatyper:

| **Datatyp** | **Fält** |
| --- | --- |
| Varning |Varna namn, Aviseringsbeskrivningen, BaseManagedEntityId, Problem-ID, IsMonitorAlert, RuleId, ResolutionState, prioritet, allvarlighetsgrad, kategori, ägare, ResolvedBy, TimeRaised, TimeAdded, LastModified, LastModifiedBy, LastModifiedExceptRepeatCount, TimeResolved, TimeResolutionStateLastModified, TimeResolutionStateLastModifiedInDB, RepeatCount |
| Konfiguration |CustomerID, AgentID, ID för entiteterna, ManagedTypeID, ManagedTypePropertyID, CurrentValue, ChangeDate |
| Händelse |Händelse-ID, EventOriginalID, BaseManagedEntityInternalId, RuleId, PublisherId, PublisherName, FullNumber, antalet, kategori, ChannelLevel, LoggingComputer, EventData, EventParameters, TimeGenerated, TimeAdded <br>**Obs:** när du skriver händelser med anpassade fält i händelseloggen för toohello OMS samlar in dem. |
| Metadata |BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, nätverksnamn, IP-adress, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-adress, NetbiosDomainName, LogicalProcessors, DNS-namn, DisplayName, DomainDnsName, ActiveDirectorySite, huvudkontot, OffsetInMinuteFromGreenwichTime |
| Prestanda |Objektnamn, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded |
| Status |StateChangeEventId, StateId, NewHealthState, OldHealthState, kontext, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified |

## <a name="physical-security"></a>Fysisk säkerhet
hello logganalys i OMS-tjänsten är bemannat av Microsoft-Personal och alla aktiviteter loggas och kan granskas. hello-tjänsten körs helt i Azure och uppfyller hello Azure gemensamma engineering kriterier. Du kan visa information om hello fysiska säkerheten för Azure tillgångar på sidan 18 i hello [översikt över Microsoft Azure-säkerhet](http://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf). Fysisk åtkomst rättigheter toosecure områden ändras inom en arbetsdag för alla som har ansvar för hello OMS-tjänsten, inklusive överföring och avslutning inte längre. Du kan läsa om hello globala fysisk infrastruktur vi använder på [Microsoft Datacenters](https://www.microsoft.com/en-us/server-cloud/cloud-os/global-datacenters.aspx).

## <a name="incident-management"></a>Hantering av incidenter
OMS har en process för hantering av incidenter som följer alla Microsoft-tjänster. toosummarize, vi:

* Använd en delad ansvar modell där en del av säkerhet ansvar tillhör tooMicrosoft och en del tillhör toohello kunden
* Hantera Azure säkerhetsincidenter
  * Starta en undersökning vid identifiering av en incident
  * Utvärdera hello inverkan och allvarlighetsgrad för en incident av en medlem i gruppen på anrop incidenter. Baserat på bevis hello assessment kan eller inte kan resultera i ytterligare eskalering toohello security response-teamet.
  * Felsöka en incident av säkerhet svar experter tooconduct hello tekniska eller kriminaltekniska undersökningar, identifiera strategier för inneslutning, begränsning och lösning. Om hello säkerhetsteam anser att kundinformation som har blivit exponerade tooan olagligt eller obehöriga börjar enskilda, parallell körning av hello kunden Incident Notification processen parallellt.  
  * Hålla och återställa från hello incident. hello incidenter team skapar en återställning plan toomitigate hello problemet. Kris inneslutning steg, till exempel sätta i karantän berörda system kan uppstå omedelbart och parallellt med diagnos. Längre sikt åtgärder planeras som inträffar efter hello omedelbar risk har passerat.  
  * Stäng hello incident och genomföra en post före. hello incidenter team skapas en post före som visar hello information om hello incident, med hello avsikt toorevise principer, procedurer och processer tooprevent återkommer hello-händelse.
* Meddela kunder för säkerhetsincidenter
  * Fastställa hello omfattning berörda kunder och tooprovide vem som helst som påverkas som detaljerad ett meddelande som möjligt
  * Skapa ett meddelande tooprovide kunder med tillräckligt detaljerad information så att de kan utföra en undersökning på deras syfte och uppfylla sina åtaganden de har tootheir slutanvändare när inte fördröja process för hello-meddelande.
  * Bekräfta och deklarera hello incident, vid behov.
  * Meddela kunder med en incident meddelande utan orimligt fördröjning och i enlighet med ett åtagande lag eller avtal. Meddelanden om säkerhetsincidenter levereras tooone eller flera av kundens administratörer på något sätt Microsoft väljer, inklusive via e-post.
* Genomföra team beredskap och utbildning
  * Microsoft-Personal är obligatoriska toocomplete säkerhet och utbildning som hjälper dem att tooidentify och rapportera misstänkta säkerhetsproblem.  
  * Operatorer som arbetar på hello Microsoft Azure-tjänsten har ytterligare utbildning skyldigheter kring deras åtkomst toosensitive system som värd för kundens data.
  * Microsoft security response-personal få särskilda utbildning för deras roller

Om en kund dataförlust, meddelar vi varje kund inom en dag. Kunden dataförlust har aldrig uppstod med OMS. Dessutom upprätthåller vi kopior av data som har skapats och den distribueras geografiskt.

Mer information om hur Microsoft hanterar toosecurity incidenter finns [Microsoft Azure Security Response i hello molnet](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/1/Microsoft Azure Security Response in hello cloud.pdf).

## <a name="compliance"></a>Efterlevnad
Hej OMS software development och tjänsten grupps informationssäkerhet och styrning programmet stöder affärskraven och följer toolaws och -förordningar enligt beskrivningen i [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) och [Microsoft Trust Center kompatibilitet](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx). Hur OMS upprättar säkerhetskrav, identifierar säkerhetsåtgärder, hanterar och övervakar risker beskrivs också det. Årligen, vi granska principer, standarder och procedurer som riktlinjer.

Varje medlem i gruppen OMS utveckling får formella säkerhetsutbildning. Internt, använder vi ett system för version för programutveckling. Varje projekt program är skyddat av hello systemet för versionskontroll.

Microsoft har ett team för säkerhet och efterlevnad som övervakar och utvärderar alla tjänster i Microsoft. Information security polis utgör hello team och de är inte kopplat till hello tekniska avdelningar som utvecklar OMS. hello har sina egna management kedjan och utföra oberoende bedömning av produkter och tjänster tooensure säkerhet och efterlevnad.

Microsofts styrelse meddelas av en årlig rapport om alla information säkerhetsprogram på Microsoft.

hello OMS programvara utvecklings- och teamet arbetar aktivt tillsammans med hello Microsoft Legal och kompatibiliteten team och andra industry partner tooacquire olika certifikat.

## <a name="certifications-and-attestations"></a>Certifikat och intyg
OMS logganalys hello på uppfyller följande krav:

* [ISO/IEC 27001](http://www.iso.org/iso/home/standards/management-standards/iso27001.htm)
* [ISO/IEC 27018:2014](http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498)
* [ISO 22301](https://azure.microsoft.com/en-us/blog/iso22301/)
* [Payment Card (PCI kompatibla) Datasäkerhetsstandard (PCI DSS)](https://www.microsoft.com/en-us/TrustCenter/Compliance/PCI) av hello PCI Security Standards Council.
* [Tjänsten organisation kontroller (SOC) 1 typen 1 och SOC 2 typ 1](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2) kompatibla
* [HIPAA och HITECH](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA) för företag som har ett HIPAA Business associera avtal
* Windows vanliga tekniker villkor
* Microsoft Trustworthy Computing
* Hello-komponenter som använder OMS följa tooAzure efterföljandekrav som en Azure-tjänst. Du kan läsa mer i [Microsoft förtroende Center kompatibilitet](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx).

> [!NOTE]
> I vissa certifieringar/intyg logganalys anges under dess tidigare namnet på *åtgärdsinformation*.
>
>


## <a name="cloud-computing-security-data-flow"></a>Molntjänster dataflöde för säkerhet
hello följande diagram visar en moln-säkerhetsarkitekturen som hello flödet av information från ditt företag och hur den är skyddad som flyttar toohello logganalys-tjänsten som setts av du slutligen i hello OMS-portalen. Mer information om varje steg följer hello diagram.

![Bild av OMS-datainsamling och säkerhet](./media/log-analytics-security/log-analytics-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. Registrera dig för Log Analytics och samla in data
Din organisation toosend data tooLog Analytics konfigurerar du Windows-agenter, agenter som körs på virtuella Azure-datorer eller OMS-Agent för Linux. Om du använder Operations Manager-agenter kan du använda en konfigurationsguide i hello Operations-konsolen tooconfigure dem. Användare (som kan vara dig, andra enskilda användare eller en grupp människor) skapa en eller flera OMS-konton (OMS arbetsytor) och registrera agenter med någon av hello följande konton:

* [Organisations-ID](../active-directory/sign-up-organization.md)
* [Microsoft-konto – Outlook, Office Live, MSN](http://www.microsoft.com/account/default.aspx)

En OMS-arbetsyta är där data som samlas in, aggregeras, analyseras och visas. En arbetsyta används i huvudsak som en innebär toopartition data och varje arbetsyta är unika. Du kanske exempelvis vill toohave produktionsdata hanteras med en OMS-arbetsytan och testdata som hanteras med en annan arbetsyta. Arbetsytor kan också hjälpa en administratör användaren åtkomst toohello kontrolldata. Varje arbetsyta kan ha flera användarkonton som är kopplade till den, och varje användarkonto som har åtkomst till flera arbetsytor i OMS. Du skapar arbetsytor baserat på datacenter region. Varje arbetsyta är replikerade tooother datacenter i hello region, främst för OMS tjänstetillgänglighet.

För Operations Manager upprättar varje hanteringsgrupp för Operations Manager en anslutning med hello logganalys-tjänsten när hello konfigurationsguiden är klar. Du kan sedan använda hello guiden Lägg till datorer toochoose vilka datorer i hello management group tillåts toosend toohello datatjänst. För andra typer av agenten ansluter varje på ett säkert sätt toohello OMS-tjänsten.

All kommunikation mellan anslutna system och hello logganalys-tjänsten är krypterad.  hello TLS används-protokollet (HTTPS) för kryptering.  hello Microsoft SDL-processen följs tooensure logganalys är uppdaterad med hello senaste utvecklingen av kryptografiska protokoll.

Varje typ av agenten samlar in data för Log Analytics. hello data som samlas in som beror på hello olika typer av lösningar som används. Du kan se en översikt över datainsamling vid [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md). Dessutom finns mer detaljerad samlingsinformation för de flesta lösningar. En lösning är ett paket med fördefinierade vyer, log sökfrågor, regler för insamling av data och standardbearbetningslogiken. Endast administratörer kan använda logganalys tooimport en lösning. När hello lösningen importeras, är det flyttade toohello Operations Manager-hanteringsservrar (om sådan används) och sedan tooany agenter som du har valt. Därefter kan samla hello agenter hello data.

## <a name="2-send-data-from-agents"></a>2. Skicka data från agenter
Du kan registrera alla typer av agenten med en nyckel för registrering och upprätta en säker anslutning mellan hello agent och hello Log Analytics-tjänsten med hjälp av certifikatbaserad autentisering och SSL med port 443. OMS använder en hemlig store toogenerate och underhålla nycklar. Privata nycklar roteras efter 90 dagar och lagras i Azure och hanteras av hello Azure åtgärder som följer strikta regler och kompatibilitet metoder.

Med Operations Manager kan du registrera en arbetsyta med hello Log Analytics-tjänsten och en säker HTTPS-anslutning upprättas mellan hello Operations Manager-hanteringsservern.

Windows-agenter som körs på virtuella Azure-datorer, är en skrivskyddad lagringsnyckel används tooread diagnostiska händelser i Azure-tabeller.

Om någon agent är toocommunicate toohello service av någon anledning, hello insamlade data lagras lokalt i en tillfällig cache och hello hanteringsservern försöker tooresend hello data var 8: e minut i två timmar. hello agent cachelagrade data skyddas av hello operativsystemets Arkiv för autentiseringsuppgifter. Om hello kunde inte behandla hello data efter två timmar, köar hello agenter hello data. Om hello kön är full, startar OMS släppa datatyper, från och med prestandadata. hello agent Kögränsen är en registernyckel, så du kan ändra den, om det behövs. Insamlade data komprimeras och skickas toohello tjänsten, kringgå lokala databaser så inte lägger till någon belastningen toothem. När hello insamlade data skickas, den tas bort från hello cache.

Enligt beskrivningen ovan, skickas data från dina agenter via SSL tooMicrosoft Azure-datacenter. Du kan även använda ExpressRoute tooprovide ytterligare säkerhet för hello data. ExpressRoute är ett sätt toodirectly ansluta tooAzure från ditt befintliga WAN-nätverk som flera protokoll etikett växling (MPLS) VPN, som tillhandahålls av en nätverkstjänstleverantören. Mer information finns i [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

## <a name="3-hello-log-analytics-service-receives-and-processes-data"></a>3. hello logganalys-tjänsten som tar emot och bearbetar data
hello logganalys-tjänsten gör att inkommande data från en betrodd källa genom att verifiera certifikat och hello dataintegritet med Azure-autentisering. hello obearbetat rådata lagras som en blob i [Microsoft Azure Storage](../storage/common/storage-introduction.md) och inte är krypterad. Dock har varje Azure storage-blob en unik uppsättning nycklar, som är tillgänglig endast toothat användare. hello typ av data som lagras beror på hello olika typer av lösningar som har importerats och används toocollect data. Hello logganalys-tjänsten bearbetar sedan hello rådata för hello Azure storage blob.

## <a name="4-use-log-analytics-tooaccess-hello-data"></a>4. Använda logganalys tooaccess hello data
Du kan logga in tooLog Analytics i hello OMS-portalen med hjälp av hello organisationskonto eller Microsoft-konto som du skapat tidigare. All trafik mellan hello OMS-portalen och logganalys i OMS skickas via en säker kanal för HTTPS. När du använder hello OMS-portalen, ett sessions-ID genereras på hello användarklient (webbläsare) och data lagras i ett lokalt cacheminne tills hello sessionen avslutas. När avslutas, raderas hello cache. Klientsidans cookies, som inte innehåller personligt identifierbar information, tas inte bort automatiskt. Sessionscookies är markerad HTTPOnly och skyddas. Efter en förbestämd inaktiv period avbryts hello OMS-portalen sessionen.

Använd hello OMS-portalen och du kan exportera data tooa CSV-fil och du kan komma åt data med hjälp av API: er Search. Exportera CSV-fil är begränsad too50, 000 rader per export- och API-data är begränsade too5, 000 rader per sökning.

## <a name="next-steps"></a>Nästa steg
* [Kom igång med logganalys](log-analytics-get-started.md) toolearn mer om logganalys och komma igång i minuter.
