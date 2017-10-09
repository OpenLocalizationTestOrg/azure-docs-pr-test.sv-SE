---
title: aaaIntroduction tooAzure Storage | Microsoft Docs
description: "En översikt över Azure Storage, Microsofts onlinelagring i hello molnet. Lär dig hur toouse hello bästa tillgängliga lösningen för molnlagring i dina program."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: a4a1bc58-ea14-4bf5-b040-f85114edc1f1
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/26/2017
ms.author: marsma
ms.openlocfilehash: dec8280c77f4b23df4c2a471e1d755e365c14e58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toomicrosoft-azure-storage"></a>Introduktion tooMicrosoft Azure Storage

## <a name="overview"></a>Översikt
Azure Storage är hello molnlagringslösningen för moderna program som förlitar sig på hållbarhet, tillgänglighet och skalbarhet toomeet hello kundernas behov. I den här artikeln kan utvecklare, IT-proffs och beslutsfattare lära sig mer om:

* Vad Azure Storage är och hur du kan dra nytta av det i dina moln-, mobil-, server- och skrivbordsprogram.
* Vilka typer av data som du kan lagra med hello Azure Storage-tjänster: blob-data (objekt), NoSQL-tabelldata, Kömeddelanden och filresurser.
* Hur tooyour åtkomst till data i Azure Storage hanteras
* Hur dina Azure Storage-data blir hållbara genom redundans och replikering.
* Där toogo nästa toobuild ditt första Azure Storage-program

<!-- after our quick starts are available, replace this link with a link tooone of those. 
Had tooremove this article, it refers toohello VS quickstarts, and they've stopped publishing them. Robin --> 
<!-- tooget up and running with Azure Storage quickly, see [Get started with Azure Storage in five minutes](storage-getting-started-guide.md). -->

Mer information om verktyg, bibliotek och andra resurser för att arbeta med Azure Storage finns i [Nästa steg](#next-steps) nedan.

## <a name="what-is-azure-storage"></a>Vad är Azure Storage?
Molntjänster möjliggör nya scenarier för program som kräver datalagring med hög tillgänglighet, skalbarhet och hållbarhet – vilket är exakt varför Microsoft utvecklade Azure Storage. I tillägg toomaking det möjligt för utvecklare toobuild storskaliga program toosupport nya scenarier, Azure Storage innehåller också hello lagringsgrunden för Azure Virtual Machines, en ytterligare ett bevis tooits stabilitet.

Azure Storage är extremt skalbart, så att du kan lagra och bearbeta hundratals terabyte data toosupport hello stordatascenarier krävs av vetenskapsprogram, ekonomisk analys, medieprogram och program. Eller så kan du lagra hello små mängder data som krävs för en liten företagswebbplats. Oavsett dina behov, betalar bara för hello data du lagrar. Azure Storage lagrar för närvarande flera biljoner unika kundobjekt och hanterar många miljoner förfrågningar per sekund i genomsnitt.

Azure Storage är Elastiskt, så att du kan skapa program för en stor global målgrupp och skala programmen efter behov – både vad gäller hello mängden lagrade data och hello antalet förfrågningar som görs mot. Du betalar endast för det du använder, och bara när du använder det.

Azure Storage använder ett system för automatisk partitionering som automatiskt belastningsutjämnar dina data baserat på trafiken. Det innebär att när hello kraven på ditt program ökar, tilldelar Azure Storage automatiskt hello lämpliga resurser för toomeet dem.

Azure Storage kan nås från var som helst i hello world från alla slags program, oavsett om de körs i hello molnet på hello skrivbordet, på en lokal server eller på en mobil eller surfplatta. Du kan använda Azure Storage i mobila scenarier där hello programmet lagrar en delmängd av data på hello enhet och synkroniseras med en fullständig uppsättning data som lagras i hello molnet.

Azure Storage har stöd för klienter med en rad olika operativsystem (inklusive Windows och Linux) och många olika programmeringsspråk (inklusive .NET, Java, Node.js, Python, Ruby, PHP och C++ samt programmeringsspråk för mobilappar) för en smidig utveckling. Azure Storage exponerar även dataresurser via enkla REST API: er, som är tillgängliga tooany klienten kan skicka och ta emot data via HTTP eller HTTPS.

Azure Premium Storage ger stöd för diskar med låg fördröjning och höga prestanda i I/O-intensiva arbetsbelastningar som körs på Azure Virtual Machines. Med Azure Premium Storage kan du bifoga flera beständiga data diskar tooa virtuell dator och konfigurerar dem toomeet prestandakraven. Varje datadisk backas upp av en SSD-disk i Premium Storage för maximala I/O-prestanda. Mer information finns i [Premium Storage: Lagring med höga prestanda för arbetsbelastningar på virtuella datorer i Azure](storage-premium-storage.md).

## <a name="introducing-hello-azure-storage-services"></a>Introduktion till hello Azure Storage-tjänster
Azure storage tillhandahåller hello följande fyra tjänster: Blob storage, Table storage, Queue storage och File storage.

* Blob Storage lagrar ostrukturerade objektdata. En blobb kan bestå av vilka slags textdata eller binära data som helst, till exempel ett dokument, en mediefil eller ett installationsprogram. BLOB storage är också hänvisade tooas objektlagring.
* Table Storage lagrar strukturerade datauppsättningar. Table storage är en NoSQL-nyckelattribut-databasen, som möjliggör snabb utveckling och snabb åtkomst toolarge mängder data.
* Queue Storage erbjuder tillförlitlig meddelandehantering för arbetsflödesbearbetning och för kommunikation mellan komponenter i molntjänster.
* File Storage erbjuder delad lagring för äldre program som använder hello SMB-standardprotokollet. Azure virtuella datorer och molntjänster kan dela fildata över programkomponenter via monterade resurser och lokala program kan komma åt fildata på en resurs via hello File service REST API.

Ett Azure storage-konto är ett säkert konto som ger dig åtkomst till tooservices i Azure Storage. Ditt lagringskonto tillhandahåller hello unika namnrymden för dina lagringsresurser. hello bilden nedan visar hello relationer mellan hello Azure storage-resurser i ett lagringskonto:

![Azure Storage-resurser](./media/storage-introduction/storage-concepts.png)

[!INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

[!INCLUDE [storage-versions-include](../../includes/storage-versions-include.md)]

## <a name="blob-storage"></a>Blob Storage
För användare med stora mängder Ostrukturerade objektet data toostore i hello molnet erbjuder Blob storage en kostnadseffektiv och skalbar lösning. Du kan använda Blob storage toostore innehåll som:

* Dokument
* Sociala data, till exempel foton, videor, musik och bloggar
* Säkerhetskopior av filer, datorer, databaser och enheter
* Bilder och text för webbprogram
* Konfigurationsdata för molnprogram
* Stordata, till exempel loggar och andra stora datauppsättningar

Blobbar ordnas i behållare. Behållare ger också ett praktiskt sätt tooassign säkerhet principer toogroups av objekt. Ett lagringskonto kan innehålla valfritt antal behållare och en behållare kan innehålla valfritt antal blobbar, upp toohello 500 TB kapacitetsgränsen för hello storage-konto.

Blob Storage erbjuder tre typer av blobbar: blockblobbar, tilläggsblobbar och sidblobbar (diskar).

* Blockblobbar är optimerade för direktuppspelning och lagring av molnobjekt och är ett bra alternativ för att lagra dokument, mediefiler, säkerhetskopior osv.
* Lägg till blobbar är liknande tooblock blobbar, men är optimerade för tilläggsåtgärder. En tilläggsblobb kan bara uppdateras genom att lägga till ett nytt block toohello slut. Lägg till blobbar är ett bra alternativ för scenarier som loggning, där nya data måste toobe skrivs endast toohello slutet av hello-blob.
* Sidblobbar är optimerade för att representera IaaS-diskar och stöder slumpmässiga skrivningar och kan vara upp too1 TB i storlek. En nätverksansluten IaaS-disk för en virtuell Azure-dator är en VHD som lagras som en sidblobb.

För mycket stora datamängder där nätverksbegränsningar gör att överföra eller hämta tooBlob datalagring över hello överföring orealistiskt du levererar en hårddisk tooMicrosoft tooimport eller exportera data direkt från hello datacenter. Se [använda hello Microsoft Azure Import/Export Service tooTransfer Data tooBlob lagring](storage-import-export-service.md).

## <a name="table-storage"></a>Table Storage

[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

Moderna program behöver ofta datalager med bättre skalbarhet och flexibilitet än tidigare versioner av den programvara som krävs. Table storage erbjuder hög tillgänglighet mycket skalbar lagring, så att programmet kan anpassas automatiskt toomeet användarens behov. Table Storage är Microsofts nyckel- och attributdatabas för NoSQL. Tjänsten har en schemalös design, vilket skiljer den från traditionella relationsdatabaser. Med ett schemalöst datalager är det enkelt tooadapt dina data efter hello behov utvecklas. Table storage är enkelt toouse så att utvecklare kan skapa program snabbt. Åtkomst toodata är snabb och kostnadseffektiv för alla typer av program.  Kostnaden för Table Storage är normalt sett betydligt lägre än för motsvarande volymer med traditionell SQL.

Table Storage är en nyckel- och attributdatabas, vilket innebär att varje värde i en tabell lagras med ett typbestämt egenskapsnamn. hello egenskapsnamnet kan användas för att filtrera och ange urvalskriterier. En uppsättning egenskaper och deras värden utgör en entitet. Eftersom Table storage är schemalöst kan två entiteter i hello samma tabell kan innehålla olika samlingar egenskaper, och dessa egenskaper kan vara av olika typer.

Du kan använda Table storage toostore flexibla datauppsättningar, till exempel användardata för webbprogram, adressböcker, enhetsinformation och andra typer av metadata som din tjänst kräver.  Du kan lagra valfritt antal enheter i en tabell och ett lagringskonto kan innehålla valfritt antal tabeller, upp toohello kapacitetsgränsen för hello storage-konto.

Som Blobbar och köer kan utvecklare hantera och komma åt Table storage med hjälp av REST-standardprotokoll, men tabellen lagring också stöder en delmängd av hello OData-protokollet, vilket förenklar avancerade frågefunktioner och aktivera JSON- och AtomPub (XML-baserade) format.

För dagens Internetbaserade program erbjuder NoSQL-databaser som Table storage ett populärt alternativ tootraditional relationsdatabaser.

## <a name="queue-storage"></a>Queue Storage
När program utformas för skalning är programkomponenterna ofta fristående, så att de kan skalas oberoende av varandra. Queue storage tillhandahåller en tillförlitlig meddelandelösning för asynkron kommunikation mellan programkomponenter, oavsett om de körs i hello molnet på hello skrivbordet, på en lokal server eller på en mobil enhet. Queue Storage har också stöd för hantering av asynkrona åtgärder och utveckling av processarbetsflöden.

Ett lagringskonto kan innehålla valfritt antal köer. En kö kan innehålla valfritt antal meddelanden, upp toohello kapacitetsgränsen för hello storage-konto. Enskilda meddelanden kan vara upp too64 KB i storlek.

## <a name="file-storage"></a>File Storage
hello Azure Files service kan du tooset in högtillgänglig nätverksfilresurser som kan nås via Server Message Block (SMB) hello standardprotokoll. Att hello innebär att flera virtuella datorer kan dela samma filer med läs- och skrivbehörighet. Du kan också läsa hello filer hello REST-gränssnittet eller hello storage, klientbiblioteken.

En sak som särskiljer Azure File storage från filer på en filresurs som företagets är att du kan komma åt hello filer överallt i hello world med en URL som pekar toohello filen och innehåller en signatur (SAS) delad åtkomst-token. Du kan generera SAS-token de kan specifika tooa privata tillgång för en viss tidsperiod.

Filresurser kan användas för många vanliga scenarier:

* Många lokala program använder filresurser. Den här funktionen gör det enklare toomigrate programmen som delar data tooAzure. Om du monterar hello filen resursen toohello samma enhetsbeteckning som hello lokalt program använder, hello en del av ditt program som har åtkomst till filresursen för hello bör arbeta tillsammans med minimala eventuella ändringar.

* Konfigurationsfiler kan lagras på en filresurs och nås från flera virtuella datorer. Verktyg och hjälpmedel som används av flera utvecklare i en grupp kan lagras på en filresurs, se till att alla kan hitta dem, och att de använder hello samma version.

* Diagnostiska loggar, mätvärden och krascher Dumpar är tre exempel på data som kan skrivas tooa filresursen och bearbetas eller analyseras senare.

Vid denna tidpunkt, Active Directory-baserad autentisering och åtkomstkontroll-listorna (ACL) stöds inte, men de kommer att någon gång hello framtida. Hej lagringskontouppgifter är används tooprovide autentisering för åtkomst toohello filresurs. Detta innebär att vem som helst med hello resurs monterade har fullständig läsning och skrivning åtkomst toohello resursen.

## <a name="access-tooblob-table-queue-and-file-resources"></a>Åtkomst tooBlob, tabell-, kö- och filen resurser
Som standard kan endast hello lagringskontoägaren komma åt resurser i hello storage-konto. För hello säkerheten för dina data, måste alla begäranden som görs mot resurser i ditt konto autentiseras. Autentiseringen använder en modell med delade nycklar. Blobbar kan också vara konfigurerade toosupport anonym autentisering.

Ditt lagringskonto tilldelas två privata åtkomstnycklar när det skapas som används för autentisering. Med två nycklar säkerställer att programmet fortfarande är tillgänglig när du regelbundet återskapar hello nycklar som säkerhet nyckelhantering vanligt.

Om du behöver tooallow användare kontrollerad åtkomst tooyour lagringsresurser, kan du skapa en signatur för delad åtkomst. En signatur för delad åtkomst (SAS) är en token som kan vara tillagda tooa URL som aktiverar delegerad åtkomst tooa storage-resursen. Alla som har hello token kan komma åt hello-resurs som pekar toowith hello behörigheter anger, för hello tidsperiod som den är giltig. Från och med version 2015-04-05 stöder Azure Storage två typer av signaturer för delad åtkomst: tjänst-SAS och konto-SAS.

hello service SAS delegater åtkomst tooa resursen i en av hello lagringstjänster: hello Blob, kön, tabell eller fil-tjänsten.

En konto-SAS delegerar åtkomst tooresources i en eller flera av hello lagringstjänster. Du kan delegera åtkomst tooservice åtgärder på rapportnivå som inte är tillgängliga med en tjänst-SAS. Du kan också delegera åtkomst tooread-, Skriv- och delete-åtgärder på blob-behållare, tabeller, köer och filresurser som inte tillåts med en tjänst-SAS.

Slutligen kan du även ange att en behållare och dess blobbar, eller en specifik blobb, ska vara offentligt tillgängliga. När du anger att en behållare eller blobb är offentlig kan alla läsa den anonymt; ingen autentisering krävs.  Offentliga behållare och blobbar är användbara för att exponera resurser, till exempel media och dokument som finns på webbplatser.  Du kan cachelagra blobbdata som används av webbplatser med hello Azure CDN toodecrease Nätverksfördröjningen för en global publik.

Mer information om signaturer för delad åtkomst finns i [Använda signaturer för delad åtkomst (SAS)](storage-dotnet-shared-access-signature-part-1.md). Se [Hantera anonym läsbehörighet toocontainers och blobbar](storage-manage-access-to-resources.md) och [autentisering för hello Azure Storage-tjänster](https://msdn.microsoft.com/library/azure/dd179428.aspx) mer information om säker åtkomst tooyour storage-konto.

## <a name="replication-for-durability-and-high-availability"></a>Replikering för hållbarhet och hög tillgänglighet
hello data i din Microsoft Azure storage-konto är alltid replikeras tooensure hållbarhet och hög tillgänglighet. Replikering kopierar dina data, antingen inom hello samma Datacenter eller tooa andra datacenter, beroende på vilket replikeringsalternativ du väljer. Replikering skyddar dina data och bevara dina program drifttid i hello händelse av tillfälliga maskinvarufel. Om dina data replikeras tooa andra datacenter, som också skyddar dina data mot ett oåterkalleligt fel i hello primär plats.

Replikeringen garanterar att ditt lagringskonto uppfyller hello [servicenivåer serviceavtalet (SLA) för lagring](https://azure.microsoft.com/support/legal/sla/storage/) även i hello ansikte fel. Se hello SLA information om Azure Storage garanterar för hållbarhet och tillgänglighet.

När du skapar ett lagringskonto kan välja du något av följande replikeringsalternativ hello:

* **Lokalt redundant lagring (LRS).** Med lokalt redundant lagring underhålls tre kopior av dina data. LRS replikeras tre gånger på ett datacenter i en region. LRS skyddar dina data mot normala maskinvarufel, men inte från hello fel i ett datacenter.

    LRS erbjuds med rabatt. För maximal hållbarhet rekommenderar vi att du använder geo-redundant lagring, som beskrivs nedan.
* **Zonredundant lagring (ZRS).** Med zonredundant lagring underhålls tre kopior av dina data. ZRS replikeras tre gånger mellan två toothree anläggningar, antingen i en enda region eller mellan två regioner, vilket ger högre hållbarhet än LRS. ZRS garanterar att dina data skyddas inom en enskild region.

    ZRS ger en högre nivå av hållbarhet än LRS. För maximal hållbarhet rekommenderar vi dock att du använder geo-redundant lagring, som beskrivs nedan.

  > [!NOTE]
  > ZRS är för närvarande endast tillgängligt för blockblobbar, och stöds endast för version 2014-02-14 och senare.
  >
  > När du har skapat ditt lagringskonto och valt ZRS kan du inte konvertera den toouse tooany andra typ av replikering eller tvärtom.
  >
  >
* **Geo-redundant lagring (GRS)**. Med GRS underhålls sex kopior av dina data. Dina data med GRS replikeras tre gånger inom hello primär region och också replikeras tre gånger i en sekundär region hundratals mil bort från hello primära regionen, vilket ger hello högsta nivån av hållbarhet. I hello händelse av ett fel vid hello primära regionen kommer Azure Storage redundans toohello sekundär region. GRS garanterar att dina data skyddas i två olika områden.

    Information om primära och sekundära kopplingar efter region finns i [Azure-regioner](https://azure.microsoft.com/regions/).
* **Geo-redundant lagring med läsbehörighet (RA-GRS)**. Geo-redundant lagring med läsbehörighet replikerar data tooa sekundära var du befinner dig och ger även läsåtkomst tooyour data på hello sekundär plats. Geo-redundant lagring med läsbehörighet kan du tooaccess dina data från primär hello eller hello sekundär plats i hello händelse att en plats blir otillgänglig. Geo-redundant lagring med läsbehörighet är hello standardalternativet för ditt lagringskonto som standard när du skapar den.

  > [!IMPORTANT]
  > Du kan ändra hur dina data replikeras när ditt lagringskonto har skapats, såvida du inte valde ZRS när du skapade hello-konto. Observera dock att du kan innebära en ytterligare engångskostnader för dataöverföring om du växlar från LRS tooGRS eller RA-GRS.
  >
  >

Mer information om lagringsreplikeringsalternativ finns i [Azure Storage-replikering](storage-redundancy.md).

Information om priser för replikering av lagringskonton finns i [Priser för Azure Storage](https://azure.microsoft.com/pricing/details/storage/). Mer information om vilka tjänster som är tillgängliga i varje region finns i [Azure-regioner](https://azure.microsoft.com/regions/#services).

Arkitekturinformation om hållbarhet med Azure Storage finns i dokumentet [SOSP Paper - Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).

## <a name="transferring-data-tooand-from-azure-storage"></a>Överföra data tooand från Azure Storage
Du kan använda hello AzCopy kommandoradsverktyget toocopy blob-, fil- och tabelldata inom ditt lagringskonto eller mellan lagringskonton. Se [överföra data med kommandoradsverktyget Azcopy hello](storage-use-azcopy.md) för mer information.

AzCopy är byggt på hello [Azure Data Movement Library](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/), som är tillgängliga i förhandsversionen.

hello Azure Import/Export service ger ett sätt tooimport blob-data till eller exportera blob-data från ditt lagringskonto via en hårddisk som skickat toohello Azure-datacenter. Läs mer om hello Import/Export service [använda hello Microsoft Azure Import/Export Service tooTransfer Data tooBlob lagring](storage-import-export-service.md).

## <a name="pricing"></a>Prissättning
[!INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

## <a name="storage-apis-libraries-and-tools"></a>API:er, bibliotek och verktyg för Azure Storage
Azure Storage-resurser kan nås med alla språk som kan skicka HTTP/HTTPS-förfrågningar. Dessutom erbjuder Azure Storage programmeringsbibliotek för flera populära språk. Dessa bibliotek förenklar många aspekter av arbetet med Azure Storage genom att hantera information som till exempel synkrona och asynkrona anrop, massbearbetning av åtgärder, undantagshantering, automatiska omförsök, funktionsbeteenden och så vidare. Bibliotek är tillgängliga för hello följande språk och plattformar, med andra i hello pipeline:

### <a name="azure-storage-data-services"></a>Azure Storage Data Services
* [REST-API för Storage Services](http://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Storage-klientbibliotek för .NET, Windows Phone och Windows Runtime](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [Storage-klientbibliotek för C++](https://github.com/Azure/azure-storage-cpp)
* [Storage-klientbibliotek för Java/Android](https://azure.microsoft.com/develop/java/)
* [Storage-klientbibliotek för Node.js](http://dl.windowsazure.com/nodestoragedocs/index.html)
* [Storage-klientbibliotek för PHP](https://azure.microsoft.com/develop/php/)
* [Storage-klientbibliotek för Ruby](https://azure.microsoft.com/develop/ruby/)
* [Storage-klientbibliotek för Python](https://azure.microsoft.com/develop/python/)
* [Storage-cmdletar för PowerShell 1.0](/powershell/module/azurerm.storage/#storage)

### <a name="azure-storage-management-services"></a>Azure Storage Management Services
* [Referens för REST-API för Storage Resource Provider](/rest/api/storagerp/)
* [Klientbibliotek för Storage Resource Provider för .NET](/dotnet/api/microsoft.azure.management.storage)
* [Cmdlets för Storage Resource Provider för PowerShell 1.0](/powershell/module/azure.storage)
* [REST-API för Service Management för Storage (klassisk)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### <a name="azure-storage-data-movement-services"></a>Azure Storage Data Movement Services
* [REST-API för Storage Import/Export Service](storage-import-export-service.md)
* [Klientbibliotek för Storage Data Movement för .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### <a name="tools-and-utilities"></a>Verktyg och hjälpmedel
* [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en gratis, fristående app från Microsoft som möjliggör toowork visuellt med Azure Storage-data i Windows, macOS och Linux.
* [Azure Storage-klientverktyg](storage-explorers.md)
* [SDK:er och verktyg för Azure](https://azure.microsoft.com/tools/)
* [Azure Storage-emulator](http://www.microsoft.com/download/details.aspx?id=43709)
* [Azure PowerShell](/powershell/azure/overview)
* [Kommandoradsverktyget AzCopy](http://aka.ms/downloadazcopy)

## <a name="next-steps"></a>Nästa steg
toolearn mer om Azure Storage, utforska gärna dessa resurser:

### <a name="documentation"></a>Dokumentation
* [Azure Storage-dokumentation](https://azure.microsoft.com/documentation/services/storage/)
* [Skapa ett lagringskonto](storage-create-storage-account.md)

<!-- after our quick starts are available, replace this link with a link tooone of those. 
Had tooremove this article, it refers toohello VS quickstarts, and they've stopped publishing them. Robin --> 
<!--* [Get started with Azure Storage in five minutes](storage-getting-started-guide.md)
-->

### <a name="for-administrators"></a>För administratörer
* [Använd Azure PowerShell med Azure Storage](storage-powershell-guide-full.md)
* [Använda Azure CLI med Azure Storage](storage-azure-cli.md)

### <a name="for-net-developers"></a>För .NET-utvecklare
* [Komma igång med Azure Blob Storage med hjälp av .NET](storage-dotnet-how-to-use-blobs.md)
* [Komma igång med Azure Table Storage med hjälp av .NET](storage-dotnet-how-to-use-tables.md)
* [Komma igång med Azure Queue Storage med hjälp av .NET](storage-dotnet-how-to-use-queues.md)
* [Komma igång med Azure File Storage i Windows](storage-dotnet-how-to-use-files.md)

### <a name="for-javaandroid-developers"></a>För Java-/Android-utvecklare
* [Hur toouse Blob storage från Java](storage-java-how-to-use-blob-storage.md)
* [Hur toouse Table storage från Java](storage-java-how-to-use-table-storage.md)
* [Hur toouse Queue storage från Java](storage-java-how-to-use-queue-storage.md)
* [Hur toouse File storage från Java](storage-java-how-to-use-file-storage.md)

### <a name="for-nodejs-developers"></a>För Node.js-utvecklare
* [Hur toouse Blob storage från Node.js](storage-nodejs-how-to-use-blob-storage.md)
* [Hur toouse Table storage från Node.js](storage-nodejs-how-to-use-table-storage.md)
* [Hur toouse Queue storage från Node.js](storage-nodejs-how-to-use-queues.md)

### <a name="for-php-developers"></a>För PHP-utvecklare
* [Hur toouse Blob storage från PHP](storage-php-how-to-use-blobs.md)
* [Hur toouse Table storage från PHP](storage-php-how-to-use-table-storage.md)
* [Hur toouse Queue storage från PHP](storage-php-how-to-use-queues.md)

### <a name="for-ruby-developers"></a>För Ruby-utvecklare
* [Hur toouse Blob storage från Ruby](storage-ruby-how-to-use-blob-storage.md)
* [Hur toouse Table storage från Ruby](storage-ruby-how-to-use-table-storage.md)
* [Hur toouse Queue storage från Ruby](storage-ruby-how-to-use-queue-storage.md)

### <a name="for-python-developers"></a>För Python-utvecklare
* [Hur toouse Blob storage från Python](storage-python-how-to-use-blob-storage.md)
* [Hur toouse Table storage från Python](storage-python-how-to-use-table-storage.md)
* [Hur toouse Queue storage från Python](storage-python-how-to-use-queue-storage.md)
* [Hur toouse File storage från Python](storage-python-how-to-use-file-storage.md)
