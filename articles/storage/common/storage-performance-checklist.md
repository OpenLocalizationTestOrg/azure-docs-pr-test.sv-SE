---
title: "aaaAzure lagring checklistan för prestanda och skalbarhet | Microsoft Docs"
description: "En checklista beprövade metoder för användning med Azure Storage i utvecklar performant program."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 959d831b-a4fd-4634-a646-0d2c0c462ef8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: c2970c055d460070288d1810e4a77a7f056a4137
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-storage-performance-and-scalability-checklist"></a>Prestanda och skalbarhetschecklista för Microsoft Azure Storage
## <a name="overview"></a>Översikt
Microsoft har utvecklat ett antal beprövade metoder för att använda dessa tjänster på ett sätt som performant sedan hello versionen av hello Microsoft Azure Storage-tjänster, och den här artikeln fungerar tooconsolidate hello viktigaste av dem i en lista med checklista-format. den här artikeln hello avsikt är toohelp programutvecklare kontrollera de använder beprövade metoder med Azure Storage och toohelp dem identifiera andra beprövade metoder som de bör införandet. Den här artikeln använder inte toocover var möjligt optimering av prestanda och skalbarhet – det utesluter som är litet i deras inverkan eller inte brett tillämpas. toohello mån hello programmets beteende kan förutsägas under design, är det användbart tookeep dessa i något tidigt tooavoid design som ska köras i prestandaproblem.  

Alla utvecklare av serverprogram med hjälp av Azure Storage bör ta hello tid tooread den här artikeln och kontrollera att hans eller hennes program följer varje hello beprövade metoder som anges nedan.  

## <a name="checklist"></a>Checklista
Den här artikeln organiserar hello beprövade metoder i hello efter grupper. Beprövade metoder som gäller för:  

* Alla Azure Storage-tjänster (blobbar, tabeller, köer och filer)
* Blobar
* Tabeller
* Köer  

| Klart | Område | Kategori | Fråga |
| --- | --- | --- | --- |
| &nbsp; | Alla tjänster |Skalbarhetsmål |[Närmar programmet utformats-tooavoid hello skalbarhetsmål?](#subheading1) |
| &nbsp; | Alla tjänster |Skalbarhetsmål |[Är din naming convention utformats tooenable bättre belastningsutjämning?](#subheading47) |
| &nbsp; | Alla tjänster |Nätverk |[Har sida klientenheter tillräckligt hög bandbredd och låg latens tooachieve hello prestanda som krävs?](#subheading2) |
| &nbsp; | Alla tjänster |Nätverk |[Har sida klientenheter en tillräckligt hög kvalitet länk?](#subheading3) |
| &nbsp; | Alla tjänster |Nätverk |[Hello klientprogrammet finns ”” hello storage-konto?](#subheading4) |
| &nbsp; | Alla tjänster |Innehållsdistribution |[Använder du en CDN för innehållsdistribution?](#subheading5) |
| &nbsp; | Alla tjänster |En klient direkt åtkomst |[Använder du SAS och CORS tooallow direktåtkomst toostorage i stället för proxy?](#subheading6) |
| &nbsp; | Alla tjänster |Cachelagring |[Är programmet cachelagring data används flera gånger och ändringar sällan?](#subheading7) |
| &nbsp; | Alla tjänster |Cachelagring |[Tillämpningsprogrammet är grupperade uppdateringar (cachelagring på klientsidan för dem och sedan ladda upp i större mängder)?](#subheading8) |
| &nbsp; | Alla tjänster |.NET-konfiguration |[Har du konfigurerat din klient toouse ett tillräckligt antal samtidiga anslutningar?](#subheading9) |
| &nbsp; | Alla tjänster |.NET-konfiguration |[Har du konfigurerat .NET toouse ett tillräckligt antal trådar?](#subheading10) |
| &nbsp; | Alla tjänster |.NET-konfiguration |[Du använder .NET 4.5 eller senare som har förbättrats skräpinsamling?](#subheading11) |
| &nbsp; | Alla tjänster |Parallellitet |[Har du sett till att parallellitet begränsas på lämpligt sätt så att du inte överlagra din klientfunktioner eller hello skalbarhetsmål?](#subheading12) |
| &nbsp; | Alla tjänster |Verktyg |[Tillhandahålls du använder hello senaste versionen av Microsoft klientbibliotek och verktyg?](#subheading13) |
| &nbsp; | Alla tjänster |Antal försök |[Är du med hjälp av en exponentiell backoff försök principer för begränsning av fel och tidsgränser?](#subheading14) |
| &nbsp; | Alla tjänster |Antal försök |[Är programmet Undvik nya försök för icke-återförsökbart fel?](#subheading15) |
| &nbsp; | Blobar |Skalbarhetsmål |[Har du ett stort antal klienter som ansluter till ett enda objekt samtidigt?](#subheading46) |
| &nbsp; | Blobar |Skalbarhetsmål |[Är programmet dig inom mål för hello-bandbredd eller åtgärder skalbarhet för en enda blob?](#subheading16) |
| &nbsp; | Blobar |Kopiera BLOB |[Vill du kopiera BLOB på ett effektivt sätt?](#subheading17) |
| &nbsp; | Blobar |Kopiera BLOB |[Använder du AzCopy för bulk kopior av blobbar?](#subheading18) |
| &nbsp; | Blobar |Kopiera BLOB |[Använder du Azure Import/Export tootransfer mycket stora mängder data?](#subheading19) |
| &nbsp; | Blobar |Använda Metadata |[Lagrar du vanliga metadata om blobbar i sina metadata?](#subheading20) |
| &nbsp; | Blobar |Snabb överföring |[När du försöker tooupload en blob snabbt du överför block parallellt?](#subheading21) |
| &nbsp; | Blobar |Snabb överföring |[När du försöker tooupload många blobbar snabbt, du överför blobbar parallellt?](#subheading22) |
| &nbsp; | Blobar |Korrigera Blobbtypen |[Du använder sidblobbar eller blockblobbar när det är lämpligt?](#subheading23) |
| &nbsp; | Tabeller |Skalbarhetsmål |[Är du närmar sig hello skalbarhetsmål för entiteter per sekund?](#subheading24) |
| &nbsp; | Tabeller |Konfiguration |[Använder du JSON för tabell-begäranden?](#subheading25) |
| &nbsp; | Tabeller |Konfiguration |[Har du inaktiverat Nagle tooimprove hello prestanda för små begäran?](#subheading26) |
| &nbsp; | Tabeller |Tabeller och partitioner |[Har du rätt partitionerat data?](#subheading27) |
| &nbsp; | Tabeller |Varm partitioner |[Är du undvika Lägg endast och endast lägga mönster?](#subheading28) |
| &nbsp; | Tabeller |Varm partitioner |[Infogningar/uppdateringar är fördelade på flera partitioner?](#subheading29) |
| &nbsp; | Tabeller |Frågeomfattningen |[Har du skapat din schemat tooallow för punkt frågor toobe används i de flesta fall och tabellen frågor toobe användas sparsamt?](#subheading30) |
| &nbsp; | Tabeller |Frågan densitet |[Gör dina frågor vanligtvis endast genomsökning och returnerar rader som ska använda för ditt program?](#subheading31) |
| &nbsp; | Tabeller |Begränsa returnerade Data |[Använder du filtrera tooavoid returnerar entiteter som inte behövs?](#subheading32) |
| &nbsp; | Tabeller |Begränsa returnerade Data |[Använder du projektion tooavoid returnerar egenskaper som inte behövs?](#subheading33) |
| &nbsp; | Tabeller |Denormalization |[Har du Avnormaliserade dina data så att du undviker ineffektiva frågor eller flera läsbegäranden vid tooget data?](#subheading34) |
| &nbsp; | Tabeller |Insert/Update/Delete |[Är du batchbearbetning begäranden som behöver toobe transaktionella eller kan göras på hello samma tid tooreduce turer?](#subheading35) |
| &nbsp; | Tabeller |Insert/Update/Delete |[Är du undvika att du hämtar en entitet bara toodetermine om toocall infoga eller uppdatera?](#subheading36) |
| &nbsp; | Tabeller |Insert/Update/Delete |[Har du funderat lagra serien med data som hämtas ofta tillsammans i en enda enhet som egenskaper i stället för flera enheter?](#subheading37) |
| &nbsp; | Tabeller |Insert/Update/Delete |[För enheter som hämtas alltid tillsammans och kan skrivas i batchar (t.ex. serien tidsdata), har du funderat med blobbar i stället för tabeller?](#subheading38) |
| &nbsp; | Köer |Skalbarhetsmål |[Är du närmar sig hello skalbarhetsmål för meddelanden per sekund?](#subheading39) |
| &nbsp; | Köer |Konfiguration |[Har du inaktiverat Nagle tooimprove hello prestanda för små begäran?](#subheading40) |
| &nbsp; | Köer |Meddelandestorlek |[Är dina meddelanden compact tooimprove hello prestanda för hello kön?](#subheading41) |
| &nbsp; | Köer |Hämta bulk |[Hämtar du flera meddelanden i en enda åtgärd ”hämta”?](#subheading42) |
| &nbsp; | Köer |Avsökningsfrekvens |[Är du avsökning ofta tillräckligt med tooreduce hello som uppfattade svarstiden för ditt program?](#subheading43) |
| &nbsp; | Köer |Uppdatera meddelande |[Du använder UpdateMessage toostore förlopp i behandlar meddelanden att undvika att hela tooreprocess hello-meddelande om ett fel inträffar?](#subheading44) |
| &nbsp; | Köer |Arkitektur |[Du använder köer toomake Mer skalbar hela programmet genom att hålla tidskrävande arbetsbelastningar utanför hello kritiska och skala sedan oberoende av varandra?](#subheading45) |

## <a name="allservices"></a>Alla tjänster
Det här avsnittet innehåller beprövade metoder som är tillämpliga toohello användningen av hello Azure Storage-tjänster (blobbar, tabeller, köer eller filer).  

### <a name="subheading1"></a>Skalbarhetsmål
Varje hello Azure Storage-tjänster har skalbarhetsmål för kapacitet (GB), Transaktionshastighet och bandbredd. Om ditt program närmar sig eller överskrider något hello skalbarhetsmål, stöta ökad transaktion fördröjningar eller begränsning. När en lagringstjänsten begränsar ditt program, börjar hello service tooreturn ”503 servern upptagen” eller ”tidsgräns för 500 åtgärd” felkoder för vissa lagringstransaktioner. Det här avsnittet beskrivs särskilt båda hello allmänna inriktning toodealing med skalbarhetsmål och skalbarhetsmål för bandbredd. Senare avsnitt som handlar om enskilda lagringstjänster beskrivs skalbarhetsmål hello kontexten för den specifika tjänsten:  

* [BLOB-bandbredd och begäranden per sekund](#subheading16)
* [Tabellentiteter per sekund](#subheading24)
* [Kömeddelanden per sekund](#subheading39)  

#### <a name="sub1bandwidth"></a>Bandbredd skalbarhet mål för alla tjänster
När hello skrivning, hello bandbredd mål i hello USA för en geo-redundant lagring (GRS)-konto är 10 Gigabit per sekund (Gbps) för inkommande trafik (data skickas toohello storage-konto) och 20 Gbit/s för utgående trafik (data som skickas från hello storage-konto). För ett lokalt redundant lagringskonto (LRS) hello-gränserna är högre – 20 Gbit/s för ingående och 30 Gbit/s för utgående trafik.  Internationella bandbreddsgränser kan vara lägre och finns på vår [skalbarhet mål sidan](http://msdn.microsoft.com/library/azure/dn249410.aspx).  Mer information om hello lagringsalternativ för redundans, se hello länkar i [användbara resurser](#sub1useful) nedan.  

#### <a name="what-toodo-when-approaching-a-scalability-target"></a>Vilka toodo när närmar sig ett mål för skalbarhet
Om ditt program närmar sig hello skalbarhetsmål för ett enda lagringskonto, bör du införandet av en av följande metoder hello:  

* Rapportdefinitionselementet hello arbetsbelastning som gör att din ansökan tooapproach eller överskrida hello skalbarhet mål. Kan du utforma den annorlunda toouse mindre bandbredd eller kapacitet eller färre transaktioner?
* Om ett program måste överskrider en hello skalbarhetsmål, bör du skapa flera lagringskonton och partition dina programdata mellan dessa flera lagringskonton. Om du använder det här mönstret sedan vara säker på att toodesign ditt program så att du kan lägga till fler lagringskonton i hello framtida för belastningsutjämning. Varje Azure-prenumeration kan ha upp too100 storage-konton vid tiden för skrivning.  Storage-konton har också utan kostnad än förbrukningen termer data som lagras, transaktioner som har gjorts eller data som överförs.
* Om ditt program träffar hello bandbredd mål, Överväg att komprimera data i hello tooreduce hello bandbredd som krävs toosend hello data toohello lagring-klienttjänsten.  Observera att även om detta kan spara bandbredd och förbättra nätverkets prestanda, den kan också innehålla några negativa konsekvenser.  Du bör utvärdera hello prestandapåverkan detta på grund av toohello ytterligare bearbetningskrav för att komprimera och expandera data i hello-klienten. Dessutom kan kan lagra komprimerade data göra det svårare tootroubleshoot problem Eftersom det kan vara svårare tooview lagrade data med standardverktyg.
* Om ditt program träffar hello skalbarhetsmål, kontrollera att du använder en exponentiell backoff för återförsök (se [återförsök](#subheading14)).  Det är bättre toomake säker hello skalbarhetsmål hanterar aldrig (med hjälp av en av hello ovan metoder), men det säkerställer att programmet hålla inte bara du försöker snabbt, vilket gör hello begränsning worse.  

#### <a name="useful-resources"></a>Användbara resurser
hello följande länkar innehåller ytterligare information om skalbarhetsmål:

* Se [Azure Storage skalbarhets- och prestandamål](storage-scalability-targets.md) information om skalbarhetsmål.
* Se [Azure Storage-replikering](storage-redundancy.md) och hello blogginlägget [Azure lagringsalternativ för redundans och Geo-Redundant lagring med läsbehörighet](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx) information om lagringsalternativ för redundans.
* Aktuell information om priser för Azure-tjänster finns [priser för Azure](https://azure.microsoft.com/pricing/overview/).  

### <a name="subheading47"></a>Namngivningskonventionen för partition
Azure Storage använder ett intervall-baserade partitionering schemat tooscale och Läs in saldo hello system. hello Partitionsnyckeln är används toopartition data till intervall och dessa områden är belastningsutjämnad över hello system. Det innebär att namngivningskonventioner, till exempel lexikala ordning (t.ex. msftpayroll, msftperformance, msftemployees osv.) eller med tidsstämplar (log20160101, log20160102, log20160102 osv.) kan låna ut sig själva toohello partitioner som potentiellt är placerade på hello samma partition server, tills en åtgärd för belastningsutjämning delar ut dem i mindre intervall. Alla blobbar i en behållare kan till exempel hanteras av en enskild server förrän hello belastningen på dessa blobbar kräver ytterligare omfördelning av hello partition intervall. På samma sätt kan en grupp med lågt belastade konton med namnen i lexikala ordning kan hanteras av en enskild server förrän hello läsa in på en eller alla dessa konton behöver dem toobe dela över flera servrar i partitioner. Varje belastningsutjämningsregel åtgärden kan påverka hello svarstid för anrop av lagring under hello-åtgärd. hello systemets möjlighet toohandle en plötslig burst av trafik tooa partition begränsas av hello skalbarhet för en enskild partition server förrän hello belastningsutjämning åtgärden sparkar i och balanserar hello partitionsnyckelintervallet.  

Du kan följa vissa tooreduce hello frekvensen bästa praxis för dessa åtgärder.  

* Granska hello namngivningskonvention som du använder för konton, behållare, blobbar, tabeller och köer, nära. Du skapar prefix kontonamn med ett 3-siffriga hash-värde med hjälp av en hash-funktionen som bäst passar dina behov.  
* Om du ordna data med tidsstämplar eller numerisk identifierare har du inte använder en append-only (eller endast lägga) trafikmönster tooensure. Dessa mönster lämpar sig inte för ett intervall-baserade partitionering system och gick lead tooall hello trafik kommer tooa enskild partition och begränsande hello system från effektivt belastningsutjämning. Om du har dagligen dirigeras åtgärder som använder ett blob-objekt med en tidsstämpel, till exempel ÅÅÅÅMMDD sedan alla hello trafik för som dagliga driften är exempelvis tooa objekt som hanteras av en enda partition-servern. Titta på om hello per blob gränser och partition begränsar uppfyller dina behov och kan du dela den här åtgärden i flera blobbar om det behövs. På samma sätt om du sparar tid series-data i tabeller all trafik som hello kunde dirigeras toohello sista delen av hello viktiga namnområde. Om du måste använda tidsstämplar eller numeriska ID, prefix hello-id med 3-siffriga hash eller hello gäller tidsstämplar prefix hello sekunder del av hello tid, till exempel ssyyyymmdd. Visa och undersöka åtgärder utförs regelbundet, välja en hash-funktionen som begränsar antalet frågor. I annat fall kan en slumpmässig prefixet vara tillräckligt.  
* Mer information om hello partitioneringsschema som används i Azure Storage läsa hello SOSP paper [här](http://sigops.org/sosp/sosp11/current/2011-Cascais/printable/11-calder.pdf).

### <a name="networking"></a>Nätverk
Hello API-anrop frågan, ha ofta hello fysiskt nätverksbegränsningar av programmet hello en betydande inverkan på prestanda. hello följande beskriver några begränsningar kan användarna få.  

#### <a name="client-network-capability"></a>Klienten nätverkskapacitet
##### <a name="subheading2"></a>Dataflöde
Bandbredd är hello problemet ofta hello funktionerna i hello-klienten. Till exempel när ett enda storage-konto kan hantera 10 Gbit/s eller flera av ingång (se [bandbredd skalbarhetsmål](#sub1bandwidth)), hello nätverkshastigheten i en instans av ”liten” Azure-Arbetsroll kan endast cirka 100 Mbit/s. Större Azure-instanser har nätverkskort med större kapacitet, så bör du använda en större instans eller flera Virtuella datorer om du behöver högre gränser i nätverket från en enda dator. Om du ansluter till en Storage-tjänst från ett på lokala program hello samma regel och använder sedan: Förstå hello nätverksfunktioner för hello klientenheter och hello network connectivity toohello Azure-lagringsplats och antingen förbättra dem efter behov eller utforma din programmet toowork inom deras funktioner.  

##### <a name="subheading3"></a>Länkkvalitet
Precis som med alla nätverksanvändning Tänk på att nätverksförhållanden ledde till ett fel och paketförlust kommer långsamma effektiva genomflöde.  WireShark eller NetMon hjälp kan lösa problemet.  

##### <a name="useful-resources"></a>Användbara resurser
Mer information om storlekar för virtuella datorer och allokerad bandbredd finns [Windows VM-storlekar](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) eller [Linux VM-storlekar](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

#### <a name="subheading4"></a>Plats
I en distribuerad miljö ger placera hello klienten nära toohello server i hello bästa prestanda. För åtkomst till Azure Storage med hello lägsta svarstid hello bästa platsen för din klient är inom hello samma Azure-region. Till exempel om du har en Azure-webbplats som använder Azure Storage kan du söka efter dem båda inom en enskild region (till exempel oss Väst eller Asien/Stillahavsområdet, sydost). Detta minskar hello latens och hello kostnad – samtidigt hello skrivning bandbreddsanvändning inom en enskild region är ledig.  

Om klienten program inte finns i Azure (till exempel appar för mobila enheter eller lokala företagstjänster), sedan igen placerar hello storage-konto i en region nära toohello enheter som kommer att använda det, kommer vanligtvis minska svarstiden. Om klienterna distribueras brett (till exempel, en del i Nordamerika och vissa i Europa), så bör du använda flera lagringskonton: placerad i Nordamerika region och en i en europeisk region. Detta hjälper tooreduce svarstid för användare i båda regioner. Den här metoden är oftast enklare tooimplement om hello hello programmet datalager specifika tooindividual användare och kräver inte replikerar data mellan lagringskonton.  En CDN rekommenderas för bred innehållsdistribution – hello nästa avsnitt innehåller mer information.  

### <a name="subheading5"></a>Innehållsdistribution
Ibland kan ett program måste tooserve hello samma innehåll toomany användare (t.ex. en produkt demonstrera video används i hello hemsidan för en webbplats), finns i antingen hello samma eller i flera områden. I det här scenariot bör du använda en innehåll innehållsleveransnätverk (CDN), till exempel Azure CDN och hello CDN använder Azure storage som hello ursprung hello data. Till skillnad från ett Azure Storage-konto som finns i en region och som det går inte att leverera innehåll med låg latens tooother regioner använder Azure CDN-servrar i flera Datacenter hello världen. Dessutom kan en CDN vanligtvis stöder mycket högre utgång gränser än ett enda storage-konto.  

Mer information om Azure CDN finns [Azure CDN](https://azure.microsoft.com/services/cdn/).  

### <a name="subheading6"></a>Med hjälp av SAS och CORS
När du behöver tooauthorize kod exempelvis JavaScript i webbläsare för en användare eller en mobiltelefon tooaccess AppData i Azure Storage, en metod som är toouse ett program i webbroll som en proxyserver: hello användaren autentiseras med hello webbrollen, som i sin tur verifierar med hello storage-tjänst. På så sätt kan undvika du att exponera dina lagringskontonycklar på osäker enheter. Men placerar detta en stor belastning på hello webbroll eftersom alla hello data överförs mellan hello användarens enhet och hello lagringstjänsten måste passera hello webbroll. Du kan undvika att använda en webbroll som en proxy för hello lagring med hjälp av delad åtkomst signaturer (SAS), ibland tillsammans med rubriker om Cross-Origin Resource Sharing (CORS). Med SAS kan låta du dina användares enhet toomake begäranden direkt tooa storage-tjänsten med hjälp av en begränsad åtkomst-token. Till exempel om en användare tooupload ett foto tooyour program, kan web-roll generera och skicka toohello användarens enhet en SAS-token som beviljar behörighet toowrite tooa specifika blob eller behållare för hello nästa 30 minuter (efter vilken hello SAS-token upphör att gälla).

En webbläsare kan normalt inte JavaScript på en sida som en webbplats på en domän tooperform specifika åtgärder som till exempel en ”PLACERA” tooanother domän som värd. Till exempel om du har en webbroll på ”contosomarketing.cloudapp.net” och vill toouse klienten sida JavaScript tooupload en blob tooyour storage-konto på ”contosoproducts.blob.core.windows.net” Hej webbläsarens ”samma ursprung policy” kommer förbjuda detta åtgärden. CORS är en webbläsarfunktion som tillåter hello domän (i det här fallet hello lagringskontot) toocommunicate toohello webbläsare som begäran har sitt ursprung i hello källdomänen (i det här fallet hello-webbroll).  

Båda dessa tekniker kan hjälpa dig att undvika onödiga belastning (och flaskhalsar) på ditt webbprogram.  

#### <a name="useful-resources"></a>Användbara resurser
Mer information om SAS finns [signaturer för delad åtkomst, del 1: Förstå hello SAS-modellen](../storage-dotnet-shared-access-signature-part-1.md).  

Läs mer om CORS [Cross-Origin Resource Sharing (CORS) stöd för hello Azure Storage-tjänster](http://msdn.microsoft.com/library/azure/dn535601.aspx).  

### <a name="caching"></a>Cachelagring
#### <a name="subheading7"></a>Hämtning av Data
I allmänhet är hämtar data från en tjänst när bättre än tas den två gånger. Överväg att hello exempel på en MVC-webbapp körs i en webbroll som redan har hämtats 50MB blob från hello storage service tooserve som innehåll tooa användare. hello programmet kan sedan hämta samma blobben varje gång en användare begär den eller det kan cachelagra den lokalt toodisk och återanvändning cachelagrade hello-version för efterföljande användarförfrågningar. Dessutom, när en användare begär hello data, hello program kan problemet får med ett villkorat huvud för ändringstid som skulle undvika hello hela blob om den inte har ändrats. Du kan använda den här samma mönster tooworking med tabellentiteter.  

I vissa fall kan du välja att programmet kan anta att hello blob förblir giltig under en kort period efter hämtning av den och att under denna period hello program inte behöver toocheck om hello-blob har ändrats.

Konfiguration, sökning och andra data som används alltid av programmet hello är bra kandidater för cachelagring.  

Ett exempel på hur tooget en blob egenskaper toodiscover hello senast ändrad datum med .NET, finns i [Set och hämta egenskaper och Metadata](../blobs/storage-properties-metadata.md). Mer information om nedladdningar av villkorlig finns [villkorligt uppdatera en lokal kopia av en Blob](http://msdn.microsoft.com/library/azure/dd179371.aspx).  

#### <a name="subheading8"></a>Ladda upp Data i batchar
I vissa program, kan aggregera data lokalt och sedan regelbundet ladda upp den i en grupp i stället för att överföra varje datadel omedelbart. Till exempel ett webbprogram ha en loggfil över aktiviteter: hello program kan antingen ladda upp information om varje aktivitet som det sker som en tabell-enhet (som kräver många lagringsåtgärder) eller den kan spara aktiviteten information tooa lokala loggfil, och sedan ladda upp alla aktivitetsinformation regelbundet som en fil tooa blob. Om varje loggpost är 1KB i storlek, kan du överföra tusentalsavgränsare i en enda transaktion för ”placera Blob” (du kan överföra en blob för in too64MB i storlek i en enda transaktion). Naturligtvis om hello lokal dator kraschar tidigare toohello överför potentiellt förlorar du vissa loggdata: hello programutvecklaren måste utforma hello möjlighet till klientenheten eller Överför fel.  Om hello aktivitetsdata måste toobe hämtas för timespans (inte bara enkel aktivitet), bör blobbar över tabeller.

### <a name="net-configuration"></a>.NET-konfiguration
Om du använder hello .NET Framework, innehåller det här avsnittet flera konfigurationsinställningar som du kan använda toomake betydande prestandaförbättringar för snabbt.  Om du använder andra språk, kontrollera toosee om liknande koncept som gäller i ditt valda språk.  

#### <a name="subheading9"></a>Öka Standardgränsen för anslutning
I .NET, ökar hello följande kod hello anslutning Standardgränsen (vilket är normalt 2 i en klientmiljö med eller 10 i en servermiljö) too100. Normalt bör du ange hello värdet tooapproximately hello antalet trådar som används av ditt program.  

```csharp
ServicePointManager.DefaultConnectionLimit = 100; //(Or More)  
```

Du måste ange hello anslutningsgränsen innan du öppnar alla anslutningar.  

Andra programmeringsspråk, finns det språket dokumentationen toodetermine hur tooset hello anslutning begränsa.  

Mer information finns i blogginlägget hello [Web Services: samtidiga anslutningar](http://blogs.msdn.com/b/darrenj/archive/2005/03/07/386655.aspx).  

#### <a name="subheading10"></a>Öka ThreadPool Min Threads om synkron kod med asynkrona uppgifter
Den här koden kommer att öka hello tråd pool min trådar:  

```csharp
ThreadPool.SetMinThreads(100,100); //(Determine hello right number for your application)  
```

Mer information finns i [ThreadPool.SetMinThreads metoden](http://msdn.microsoft.com/library/system.threading.threadpool.setminthreads%28v=vs.110%29.aspx).  

#### <a name="subheading11"></a>Dra nytta av .NET 4.5 skräpinsamling
Använd .NET 4.5 eller senare för hello klienten program tootake nytta av bättre prestanda i server skräpinsamling.

Mer information finns i artikeln hello [en översikt av prestandaförbättringarna i .NET 4.5](http://msdn.microsoft.com/magazine/hh882452.aspx).  

### <a name="subheading12"></a>Unbounded parallellitet
Parallellitet kan vara bra prestanda, vara försiktig med att använda flera partitioner unbounded parallellitet (ingen gräns för hello antalet trådar och/eller parallella begäranden) tooupload eller hämta data med flera personer tooaccess (behållare, köer, eller tabell partitioner) i hello samma lagringskonto eller tooaccess flera objekt i hello samma partition. Om hello parallellitet unbounded programmet överskrider hello klienten enhetens kapacitet eller hello storage-konto skalbarhetsmål ledde till ett längre svarstider och begränsning.  

### <a name="subheading13"></a>Verktyg och Storage-klientbibliotek
Använd alltid hello-klientbibliotek för senaste Microsoft tillhandahåller och verktyg. Vid hello tidpunkt som skrivs finns klientbibliotek för .NET, Windows Phone, Windows Runtime, Java och C++ samt preview bibliotek för andra språk. Dessutom har Microsoft släppt PowerShell-cmdlets och Azure CLI-kommandona för att arbeta med Azure Storage. Microsoft aktivt utvecklar dessa verktyg med prestanda, för att hålla dem in toodate med hello senaste service-versioner och garanterar de hanterar många hello internt beprövade metoder för prestanda.  

### <a name="retries"></a>Antal försök
#### <a name="subheading14"></a>Begränsning/ServerBusy
I vissa fall kan hello lagringstjänsten begränsa tillämpningsprogrammet eller kan bara tooserve hello begäran på grund av toosome övergående tillstånd och returnerar meddelandet ”503 servern upptagen” eller ”tidsgräns för 500”.  Detta kan inträffa om tillämpningsprogrammet närmar sig något hello skalbarhetsmål, eller om hello system ombalansering din partitionerade data tooallow för högre genomströmning.  hello klientprogrammet bör normalt gör hello-åtgärden som orsakar ett sådant fel: försöker hello samma begäran senare kan genomföras. Men göra om hello lagringstjänsten begränsning är ditt program eftersom det överskrider skalbarhetsmål eller om hello-tjänsten kunde tooserve hello begäran av någon anledning, aggressivt återförsök oftast hello problemet worse. Därför bör du använda en exponentiell undantagsläge (hello klienten bibliotek toothis standardbeteendet). Programmet kan t.ex, försök igen efter 2 sekunder sedan 4 sekunder sedan 10 sekunder sedan 30 sekunder och ge helt. Detta resulterar i ditt program avsevärt minska belastningen på hello-tjänsten i stället exacerbating eventuella problem.  

Observera att anslutningsfel kan göras omedelbart, eftersom de inte hello resultatet av begränsning och förväntade toobe tillfälligt.  

#### <a name="subheading15"></a>Icke-Återförsökbart fel
hello klientbibliotek är medveten om vilka fel kan retry och som inte är. Tänk dock på om du skriver egen kod mot hello storage REST-API, det finns några fel som du inte bör försöka: till exempel 400 (felaktig begäran) svaret anger att hello klientprogrammet har skickat en begäran inte kunde bearbetas eftersom den har inte en förväntad form. Skicka om begäran resulterar hello samma svar varje gång, så det finns ingen anledning du försöker den. Om du skriver egen kod mot hello storage REST API vara medveten om vilka hello felkoder medelvärde och hello tooretry på rätt sätt (eller inte) för var och en av dem.  

#### <a name="useful-resources"></a>Användbara resurser
Mer information om lagring felkoder finns [Status och felkoder](http://msdn.microsoft.com/library/azure/dd179382.aspx) på hello Microsoft Azure-webbplatsen.  

## <a name="blobs"></a>Blobar
I tillägg toohello beprövade metoder för [alla tjänster](#allservices) som beskrivs ovan, hello beprövade metoder när det gäller specifikt toohello blob-tjänsten.  

### <a name="blob-specific-scalability-targets"></a>BLOB-specifika skalbarhetsmål
#### <a name="subheading46"></a>Flera klienter som ansluter till ett enda objekt samtidigt
Om du har ett stort antal klienter som ansluter till ett enda objekt samtidigt behöver tooconsider per objekt och lagring skalbarhetsmål för lagringskontot. hello exakta antalet klienter som kan komma åt ett objekt ska variera beroende på faktorer som hello antalet klienter som begär hello objekt samtidigt, hello storleken för hello-objektet, nätverk villkor osv.

Om hello-objekt kan distribueras via en CDN, till exempel bilder eller videofiler hanteras från en webbplats och du bör använda en CDN. Se [här](#subheading5).

I andra scenarier, till exempel vetenskapliga simulering där hello data är konfidentiellt har du två alternativ. hello är först toostagger din arbetsbelastning åtkomst sådana som hello objektet används under en period om jämfört används samtidigt. Alternativt kan du tillfälligt kopiera hello objektet toomultiple storage-konton vilket ökar hello totala IOPS per objekt och över storage-konton. Begränsad testning påträffades att cirka 25 virtuella datorer kan hämta en 100GB blob parallellt (varje virtuell dator parallelizing hello download med 32 trådar) samtidigt. Om du har 100 klienter behöver tooaccess hello objekt först kopiera den tooa andra storage-konto har hello första 50 virtuella datorer åtkomst hello första blob och hello 50 andra virtuella datorer åtkomst hello andra blob. Resultatet varierar beroende på din program-beteende så bör du testa under design. 

#### <a name="subheading16"></a>Bandbredd och -åtgärder per Blob
Du kan läsa eller skriva tooa enda blob in tooa högst 60 MB per sekund (detta är ungefär 480 Mbit/s som överskrider hello funktioner för flera nätverk för klient-sida (inklusive hello fysiska nätverkskortet på hello klientenhet). Dessutom stöder en enda blob upp too500 begäranden per sekund. Om du har flera klienter som behöver tooread hello samma blob och kanske överskrider gränserna, bör du använda en CDN för att distribuera hello-blob.  

Mer information om mål-genomströmning för BLOB finns [Azure Storage skalbarhets- och prestandamål](storage-scalability-targets.md).  

### <a name="copying-and-moving-blobs"></a>Kopiera och flytta Blobbar
#### <a name="subheading17"></a>Kopiera Blob
hello storage REST API version 2012-02-12 introduceras hello användbar möjlighet toocopy blobbar över konton: ett klientprogram kan instruera hello storage service toocopy en blob från en annan källa (eventuellt i ett annat lagringskonto) och därefter låta hello tjänsten kopiera hello asynkront. Detta kan avsevärt minska hello bandbredd som krävs för programmet hello när du migrerar data från andra lagringskonton eftersom du inte behöver toodownload och ladda upp hello data.  

En fråga, men är att, när du kopierar mellan lagringskonton finns det ingen garanti för tid för när hello kopiera slutförs. Om ditt program måste toocomplete en blob kopiera snabbt under din kontroll, kanske bättre toocopy hello blob genom att hämta den tooa VM och överföra den toohello mål.  Se till att utförs hello kopia för fullständig förutsägbarhet i så fall av en virtuell dator som körs i hello samma Azure-region, annars nätverksförhållanden kan (och troligtvis kommer) påverkar din kopia prestanda.  Dessutom kan du övervaka hello förloppet för en asynkron kopia programmässigt.  

Observera att kopierar inom samma lagringskonto själva vanligtvis avslutad snabbt hello.  

Mer information finns i [kopiera Blob](http://msdn.microsoft.com/library/azure/dd894037.aspx).  

#### <a name="subheading18"></a>Använda AzCopy
hello Azure Storage har släppts kommandoradsverktyget ”AzCopy” som är avsedda toohelp med bulk överför många BLOB till, från och mellan lagringskonton.  Det här verktyget är optimerad för det här scenariot och uppnå hög överföringshastighet.  Används rekommenderas för bulk-överföringen, hämtning och kopiera scenarier. Mer om den toolearn och ladda ned det, se [överföra data med kommandoradsverktyget Azcopy hello](storage-use-azcopy.md).  

#### <a name="subheading19"></a>Tjänsten Azure Import/Export
Hello Azure Storage erbjuder mycket stora volymer av data (mer än 1TB) hello Import/Export service som gör det möjligt att överföra och ladda ned från blob storage med leverans hårddiskar.  Du kan publicera dina data på en hårddisk och skickar den tooMicrosoft för överföring eller skicka en tom hårddisk tooMicrosoft toodownload data.  Mer information finns i [använda hello Microsoft Azure Import/Export Service tooTransfer Data tooBlob lagring](../storage-import-export-service.md).  Det kan vara mycket effektivare än att överföra/hämta volymen av data över hello nätverk.  

### <a name="subheading20"></a>Använda metadata
hello blob-tjänsten stöder head-begäranden som kan innehålla metadata om hello-blob. Om ditt program behövs hello EXIF data från ett foto, kan den hämta hello foto och extrahera det. toosave bandbredd och förbättra prestanda, programmet kan lagra hello EXIF data i hello blob metadata när programmet hello upp hello foto: du kan sedan hämta hello EXIF data i metadata med hjälp av endast en HEAD-begäran, spara betydande bandbredd och hello bearbetningstid behövs tooextract hello EXIF-data varje gång hello är läsåtgärder. Detta kan vara användbart i scenarier där du behöver bara hello metadata och inte hello fullständiga innehållet på en blob.  Observera att endast 8 KB metadata kan lagras per blob (hello tjänsten inte accepterar en begäran toostore större än), så om hello data inte ryms i storleken inte får vara toouse kan den här metoden.  

Ett exempel på hur tooget en blob-metadata med hjälp av .NET, se [Set och hämta egenskaper och Metadata](../blobs/storage-properties-metadata.md).  

### <a name="uploading-fast"></a>Snabb överföring
tooupload blobbar snabb hello första fråga tooanswer är: är du överföra en blob eller många?  Använd hello nedan vägledning toodetermine hello rätt metod toouse beroende på ditt scenario.  

#### <a name="subheading21"></a>Ladda upp en stor blob snabbt
tooupload som en enda stor blob snabbt klientprogrammet ladda upp dess block eller sidor parallellt (är uppmärksam på hello skalbarhetsmål för enskilda blobbar och hello storage-konto som helhet).  Observera att hello officiella Microsoft RTM Storage-klientbibliotek (.NET, Java) hello möjlighet toodo detta.  Använd hello under angivna objektegenskaper tooset hello nivå av samtidighet för varje hello bibliotek:  

* .NET: Ange ParallelOperationThreadCount på en BlobRequestOptions objektet toobe används.
* Java/Android: Använda BlobRequestOptions.setConcurrentRequestCount()
* Node.js: Använd parallelOperationThreadCount antingen hello begäran alternativ eller hello blob-tjänsten.
* C++: Använd hello blob_request_options::set_parallelism_factor metoden.

#### <a name="subheading22"></a>Överför många blobbar snabbt
tooupload många blobbar snabbt överföra blobbar parallellt. Det här är snabbare än att överföra enda blobbar i taget med parallellt block överföringar eftersom det sprids hello överför över flera partitioner i hello storage-tjänst. En enda blob stöder bara en genomströmning på 60 MB per sekund (cirka 480 Mbit/s). När hello skrivning, USA-baserade LRS-konto har stöd för upp too20 Gbit/s-ingång som är större än hello dataflöde som stöds av en enskild blob.  [AzCopy](#subheading18) utför överföringar parallellt som standard och rekommenderas för det här scenariot.  

### <a name="subheading23"></a>Att välja hello rätt typ av blob
Azure Storage stöder två typer av blob: *sidan* blobbar och *block* blobbar. För en given användningsscenariot påverkar ditt val av blobbtypen hello prestanda och skalbarhet i lösningen. Blockblobbar är lämplig när du vill tooupload stora mängder data effektivt: till exempel ett klientprogram kan behöva tooupload foton eller video tooblob lagring. Sidblobbar är lämplig om hello programmet behöver tooperform slumpmässiga skrivningar på hello data: till exempel Azure virtuella hårddiskarna lagras som sidblobar.  

Mer information finns i [förstå Blockblobbar, Tilläggsblobbar och Sidblobbar](http://msdn.microsoft.com/library/azure/ee691964.aspx).  

## <a name="tables"></a>Tabeller
I tillägg toohello beprövade metoder för [alla tjänster](#allservices) som beskrivs ovan, hello beprövade metoder när det gäller specifikt toohello tabelltjänsten.  

### <a name="subheading24"></a>Tabell-specifika skalbarhetsmål
Tillägg toohello bandbreddsbegränsningar av en hela lagringskontot ha tabeller hello efter specifika skalbarhetsgränsen.  Observera att hello system kommer belastningsutjämna som ökar din trafik, men om trafiken har plötslig belastning, du får inte vara kan tooget volymen genomströmning omedelbart.  Om mönstret har belastning, förväntat toosee begränsning och/eller timeout under hello burst som hello lagringstjänsten automatiskt belastningsutjämning i tabellen.  Långsamt vanligtvis av dig har bättre resultat som den ger hello system tid tooload saldo på lämpligt sätt.  

#### <a name="entities-per-second-account"></a>Entiteter per sekund (konto)
Hej skalbarhetsgränsen för att komma åt tabeller är upp too20 000 enheter (1KB som är varje) per sekund för ett konto.  I allmänhet varje entitet som infogas, uppdateras, tas bort eller genomsöks antal mot det här målet.  Infoga en batch som innehåller 100 entiteter skulle så räknas som 100 entiteter.  En fråga som genomsöker 1000 entiteter och returnerar 5 räknas som 1000 enheter.  

#### <a name="entities-per-second-partition"></a>Entiteter per sekund (Partition)
Inom en enskild partition hello skalbarhet mål för att komma åt tabeller är 2 000 enheter (1KB som är varje) per sekund, med hjälp av hello samma inventering som beskrivs i föregående avsnitt i hello.  

### <a name="configuration"></a>Konfiguration
Det här avsnittet innehåller flera snabb konfigurationsinställningar som du kan använda toomake betydande prestandaförbättringar i hello tabelltjänsten:  

#### <a name="subheading25"></a>Använd JSON
Från och med version av storage service 2013-08-15, stöder hello tabelltjänsten användningen av JSON i stället för hello XML-baserade AtomPub-format för att överföra tabelldata. Detta kan minska nyttolast storlekar som 75% och förbättrar hello prestandan för din app.

Mer information finns i hello efter [Microsoft Azure-tabeller: introduktion till JSON](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/05/windows-azure-tables-introducing-json.aspx) och [Nyttolastformat för tabellen tjänståtgärder](http://msdn.microsoft.com/library/azure/dn535600.aspx).

#### <a name="subheading26"></a>Nagle ut
Nagles algoritmen implementeras över TCP/IP-nätverk som ett sätt tooimprove nätverkets prestanda. Det är dock inte optimalt i samtliga fall (till exempel interaktiva miljöer). Nagles algoritm har en negativ inverkan på prestandan för hello begäranden toohello tabell och kön tjänster för Azure Storage, och bör du inaktivera den om möjligt.  

Mer information finns i vår blogginlägget [Nagles algoritmen är inte egna gentemot små begäran](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx), som förklarar varför Nagles algoritmen dåligt samverkar med tabell- och köegenskaper begäranden och visar hur toodisable den i din klient programmet.  

### <a name="schema"></a>Schemat
Hur du representera och fråga data är hello största en faktor som påverkar hello prestanda för hello tabelltjänsten. Varje program är olika, beskrivs i det här avsnittet några allmänna beprövade metoder som relaterar till:  

* Tabelldesign
* Effektiva frågor
* Effektiv uppdateringar  

#### <a name="subheading27"></a>Tabeller och partitioner
Tabeller är indelade i partitioner. Varje entitet som lagras i en partition resurser hello samma partitionsnyckel och har en unik rad viktiga tooidentify det i den aktuella partitionen. Partitioner ger fördelar, men också introducera skalbarhetsbegränsningar.  

* Fördelar: Du kan uppdatera entiteter i hello samma partition i en enda, atomic, batch-transaktion som innehåller too100 separat lagringsåtgärder (högst 4MB, total storlek). Under förutsättning att hello samma nummer för entiteter toobe hämtas, du kan också fråga data i en enda partition effektivare än data sträcker sig över flera partitioner (även om läsa på ytterligare rekommendationer för frågar tabelldata).
* Skalbarhetsgränsen: åtkomst tooentities lagras i en partition kan inte vara belastningsutjämnad eftersom partitioner stöder atomiska Batchtransaktioner. Därför är hello skalbarhet mål för en enskild tabell partition lägre än för hello tabelltjänsten som helhet.  

På grund av dessa tabeller och egenskaper partitioner, bör du anta hello följande principer:  

* Data som ditt klientprogram uppdateras ofta eller frågas i samma logiska arbetsenheten måste finnas i hello hello samma partition.  Det kan vara eftersom programmet är sammanställa skrivningar eller eftersom du vill tootake nytta av atomiska batchåtgärder.  Dessutom kan data i en partition effektivare efterfrågas i en enskild fråga än data över partitioner.
* Data som ditt klientprogram inte infoga/uppdatera eller fråga i hello samma logiska arbetsenheten (enskild fråga eller gruppuppdatering) måste finnas i separata partitioner.  Ett viktigt är att det finns ingen gräns toohello antal partitionsnycklar i en tabell med miljontals partitionsnycklar är inte ett problem och påverkar inte prestanda.  Till exempel om ditt program är en populär webbplats med användarinloggning, kan med hjälp av hello användar-Id som hello partitionsnyckel vara ett bra alternativ.  

#### <a name="hot-partitions"></a>Varm partitioner
En varm partition är en som tar emot oproportionerligt procent av hello trafik tooan konto och kan inte vara belastningsutjämnade eftersom det är en enskild partition.  I allmänhet skapas varm partitioner på två sätt:  

##### <a name="subheading28"></a>Lägg endast till och lägga endast mönster
Hej ”Lägg till bara” mönster är ett där alla (eller nästan alla) av hello trafik tooa angivna PK ökar och minskar bl.a toohello aktuell tid.  Ett exempel är om ditt program används hello aktuellt datum som en partitionsnyckel för loggdata.  Detta resulterar i alla hello infogningar ska toohello sista partition i tabellen och hello går inte att läsa belastningsutjämna eftersom alla hello skrivningar kommer toohello slutet av tabellen.  Om hello mängden trafik toothat partition överskrider hello partition nivå skalbarhet mål, resulterar det i begränsning.  Det är bättre tooensure som trafiken skickas toomultiple partitioner tooenable belastningen saldo hello begäranden i tabellen.  

##### <a name="subheading29"></a>Hög trafik Data
Om din partitionering schema resulterar i en enda partition som har bara data som används mycket mer än andra partitioner, kan du också se begränsning när den aktuella partitionen närmar sig hello skalbarhet mål för en enskild partition.  Det är bättre toomake till att ingen enskild partition scheme resultaten partitions närmar sig hello skalbarhetsmål.  

#### <a name="querying"></a>Fråga
Det här avsnittet beskrivs beprövade metoder för att fråga efter hello tabelltjänsten.  

##### <a name="subheading30"></a>Frågeomfattningen
Det finns flera sätt toospecify hello mängd entiteter tooquery.  hello följer en beskrivning av hello används för var och en.  

I allmänhet undvika genomsökningar (frågor som är större än en enda enhet), men om du måste skannar försök tooorganize dina data så att dina skanningar hämta hello data som du behöver, utan genomsökning eller returnera stora mängder enheter inte behöver du.  

###### <a name="point-queries"></a>Punkt-frågor
En punkt-fråga hämtar exakt en enhet. Det gör du genom att ange både hello partitionsnyckel och radnyckel av hello entiteten tooretrieve. De här frågorna är mycket effektivt och du bör använda dem om möjligt.  

###### <a name="partition-queries"></a>Partitionen frågor
En partitionsfrågan är en fråga som hämtar en datamängd som delar en gemensam partitionsnyckel. Normalt anger hello frågan ett intervall av nyckelvärden för raden eller ett intervall med värden för vissa entitetsegenskap i tillägget tooa partitionsnyckel. Dessa är mindre effektiva än punkt frågor och bör användas sparsamt.  

###### <a name="table-queries"></a>Tabell frågor
En tabellfråga är en fråga som hämtar en uppsättning enheter som inte delar en gemensam partitionsnyckel. Dessa frågor är inte effektivt och du bör undvika dem om möjligt.  

##### <a name="subheading31"></a>Frågan densitet
En annan nyckelfaktor i frågan effektivitet är hello antal entiteter som returneras som jämfört med toohello antal entiteter skannade toofind hello returnerade värdet. Om ditt program utför en tabellfråga med ett filter för ett värde för egenskapen att bara 1% av hello data resurser hello fråga genomsöks 100 entiteter för var en entitet som returneras. Hej skalbarhetsmål för tabellen beskrivs tidigare alla relatera toohello antal entiteter som genomsöks, och inte hello antal entiteter som returneras: densitet frågor kan enkelt orsaka hello tabellen service toothrottle ditt program eftersom den måste söka så många entiteter tooretrieve hello entitet du letar efter.  I avsnittet hello nedan på [denormalization](#subheading34) för mer information om hur tooavoid detta.  

##### <a name="limiting-hello-amount-of-data-returned"></a>Begränsa hello belopp av returnerade Data
###### <a name="subheading32"></a>Filtrering
Om du vet att en fråga returnerar enheter som du inte behöver i hello klientprogrammet, Överväg att använda filter tooreduce hello storleken hello returnerade uppsättningen. Vid hello entiteter inte returnerades toohello klienten fortfarande räknas in hello skalbarhetsgränser, programmets prestanda kommer förbättra på grund av hello minskas nätverket nyttolastens storlek och hello minskat antal entiteter som klientprogrammet måste bearbeta .  Se över anteckning på [frågan densitet](#subheading31), men – hello skalbarhetsmål relaterar toohello antal entiteter som genomsöks, så att en fråga som filtrerar ut många entiteter fortfarande kan resultera i begränsning, även om några enheter returneras.  

###### <a name="subheading33"></a>Projektion
Om klientprogrammet måste begränsad uppsättning egenskaper från hello entiteter i tabellen, kan du använda projektion toolimit hello storleken på hello returnerade datauppsättning. Precis som med filtrering, hjälper detta tooreduce nätverksbelastningen och bearbetning av klienten.  

##### <a name="subheading34"></a>Denormalization
Till skillnad från arbete med relationsdatabaser leda hello beprövade metoder för att effektivt frågar tabelldata toodenormalizing dina data. Det vill säga dupliceras hello samma data i flera enheter (en för varje nyckel kan du använda toofind hello data) toominimize hello antal entiteter som en fråga måste du skanna toofind hello informationen hello klienten måste i stället tooscan stort antal entiteter toofind hello data måste för ditt program.  På en webbplats för e-handel, kan du exempelvis vill toofind en order både av hello kund-ID (mig kundens order) och efter datum hello (mig order på ett datum).  I Table Storage är det bästa toostore hello entitet (eller en referens tooit) två gånger – en gång med tabellnamnet och PK RK toofacilitate söka efter kund-ID, en gång toofacilitate söka efter hello datum.  

#### <a name="insertupdatedelete"></a>Insert/Update/Delete
Det här avsnittet beskrivs beprövade metoder för att ändra entiteter som lagras i tabelltjänsten hello.  

##### <a name="subheading35"></a>Batchbearbetning
Batchtransaktioner är kända som entiteten grupp transaktioner (ETG) i Azure Storage; alla hello åtgärder i en ETG måste finnas på en partition i en tabell. Använd om möjligt ETGs tooperform infogningar, uppdateringar och borttagningar i batchar. Detta minskar hello antalet sändningar till klienten toohello programservern, minskar hello antal fakturerbar transaktioner (en ETG räknas som en enda transaktion för fakturering och kan innehålla upp too100 lagringsåtgärder) och aktiverar atomiska uppdateringar (alla åtgärder lyckas eller misslyckas inom en ETG alla). Miljöer med hög fördröjning till exempel från mobila enheter kommer avsevärt fördelarna med att använda ETGs.  

##### <a name="subheading36"></a>Upsert
Användningstabell **Upsert** åtgärder när så är möjligt. Det finns två typer av **Upsert**, som kan vara effektivare än traditionella **infoga** och **uppdatering** åtgärder:  

* **InsertOrMerge**: Använd det här när du vill tooupload en delmängd av hello Entitetsegenskaper, men är osäker på om hello entiteten finns redan. Om hello entiteten finns det här anropet uppdaterar hello egenskaper som ingår i hello **Upsert** åtgärd och lämnar alla befintliga egenskaper som de är, om hello entiteten finns inte, infogas hello ny entitet. Detta är liknande toousing projektion i en fråga i den behöver du bara tooupload hello egenskaper som ändrar.
* **InsertOrReplace**: Använd den här när du vill tooupload en helt ny enhet men du är osäker på om den redan finns. Du bör endast använda detta när du vet att hello nyligen upp entiteten är helt korrekt eftersom det helt skrivs över hello gamla entitet. Till exempel önskade tooupdate hello entitet som lagrar användarens aktuella plats oavsett om programmet hello tidigare lagras lokaliseringsuppgifter för hello användaren. hello ny plats entitet är klar och du behöver inte någon information från tidigare entiteter.

##### <a name="subheading37"></a>Lagra dataserien i en enda entitet
Ibland kan ett program lagrar en serie med data som ofta måste tooretrieve samtidigt: till exempel ett program kan spåra processoranvändning över tid i ordning tooplot ett rullande diagram över hello data från hello senaste 24 timmarna. En metod är toohave en tabell entitet per timme, med varje entitet som representerar en specifik timme och lagrar hello CPU-användning för den timmen. tooplot informationen hello programmet måste tooretrieve hello innehavare hello data från hello de senaste 24 timmarna.  

Du kan också programmet kan lagra hello CPU-användning för varje timme som en separat av en enda entitet: tooupdate varje timme, ditt program kan använda en enda **InsertOrMerge Upsert** anropa tooupdate hello värdet för hello den senaste timmen. tooplot hello data hello räcker tooretrieve en enda entitet i stället för 24, för en mycket effektivt fråga (se ovan diskussion på [fråga scope](#subheading30)).

##### <a name="subheading38"></a>Lagra strukturerade data i BLOB
Ibland strukturerade data känns som det ska gå i tabeller, men intervallen för entiteter hämtas alltid tillsammans och kan batch infogas.  Ett bra exempel på detta är en loggfil.  I det här fallet kan du batch-flera minuters loggar, infoga dem och sedan du hämtar alltid flera minuter loggar i taget samt.  I det här fallet för prestanda är det bättre toouse blobbar i stället för tabeller, eftersom du kan avsevärt minska hello antalet objekt skrivs/returnerade, samt vanligtvis hello antalet begäranden som behöver.  

## <a name="queues"></a>Köer
I tillägg toohello beprövade metoder för [alla tjänster](#allservices) som beskrivs ovan, hello beprövade metoder när det gäller specifikt toohello kötjänsten.  

### <a name="subheading39"></a>Skalbarhetsgränser
En enskild kö kan bearbeta ungefär 2 000 meddelanden (1KB som är varje) per sekund (varje AddMessage och GetMessage DeleteMessage antal som ett meddelande här). Om detta inte är tillräckligt för ditt program bör du använda flera köer och sprids dem hälsningsmeddelande.  

Visa den aktuella skalbarhetsmål på [Azure Storage skalbarhets- och prestandamål](storage-scalability-targets.md).  

### <a name="subheading40"></a>Nagle ut
Avsnittet hello på konfigurationen som beskrivs hello Nagle algoritmen – hello Nagle algoritmen är vanligtvis bra för hello prestanda för kön begäranden och bör du inaktivera den.  

### <a name="subheading41"></a>Meddelandestorlek
Kön prestanda och skalbarhet minskas när meddelandet storlek ökar. Du bör placera endast hello information hello mottagaren måste i ett meddelande.  

### <a name="subheading42"></a>Batch-hämtning
Du kan hämta in too32 meddelanden från en kö i en enda åtgärd. Detta kan minska hello antalet turer fram och tillbaka från hello klientprogrammet, vilket är särskilt användbart för företagsmiljöer, t.ex mobila enheter med hög latens.  

### <a name="subheading43"></a>Avsökningsintervall för kön
De flesta program att söka efter meddelanden från en kö som kan vara något av hello största källor till transaktioner för programmet. Välj din avsökningsintervallet klokt: avsökning för ofta kan leda till att dina program tooapproach hello skalbarhetsmål för hello kö. På 200 000 transaktioner för $0,01 (för närvarande hello skrivning) är en enskild processor avsökning när varje sekund för en månad skulle kosta mindre än 15 cent så kostnaden dock inte vanligtvis en faktor som påverkar ditt val av avsökningsintervallet.  

Uppdaterade kostnadsinformation finns [priser för Azure Storage](https://azure.microsoft.com/pricing/details/storage/).  

### <a name="subheading44"></a>UpdateMessage
Du kan använda **UpdateMessage** tooincrease hello osynlighet timeout eller tooupdate statusinformation för ett meddelande. Detta är kraftfulla, Kom ihåg att varje **UpdateMessage** åtgärden räknar mot hello skalbarhet mål. Det kan dock vara ett mycket effektivare sätt än med ett arbetsflöde som skickar ett jobb från en kö toohello därefter som varje steg i hello jobbet har slutförts. Med hjälp av hello **UpdateMessage** åtgärden gör att din ansökan toosave jobbet tillstånd toohello hälsningsmeddelande och sedan fortsätta arbeta i stället för nytt queuing hello-meddelande för hello nästa steg i hello jobb varje gång ett steg har slutförts.  

Mer information finns i artikeln hello [så här: ändra hello innehållet i ett meddelande i kön](../queues/storage-dotnet-how-to-use-queues.md#change-the-contents-of-a-queued-message).  

### <a name="subheading45"></a>Programarkitektur
Du bör använda köer toomake din skalbar arkitektur för programmet. hello nedan visas några metoder som du kan använda köer toomake programmet mer skalbar:  

* Du kan använda köer toocreate eftersläpningar arbete för bearbetning och Utjämna arbetsbelastningar i ditt program. Du kan till exempel köa begäranden från användare tooperform processorn arbetar, till exempel storleksändring överförda bilder.
* Du kan använda köer toodecouple delar av ditt program så att du kan skala oberoende av varandra. Till exempel placera en webbklientdel undersökningsresultat från användare i en kö för senare analys och lagring. Du kan lägga till flera worker-rollen instanser tooprocess hello kön data vid behov.  

## <a name="conclusion"></a>Slutsats
Den här artikeln beskrivs några av de vanligaste hello beprövade metoder för att optimera prestanda när du använder Azure Storage. Vi uppmuntra varje program developer tooassess sina program mot varje hello ovan praxis och Överväg fungerar på hello rekommendationer tooget utmärkt prestanda för de program som använder Azure Storage.
