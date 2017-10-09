---
title: aaaIntroduction tooAzure Storage | Microsoft Docs
description: Introduktion tooAzure lagring, Microsofts datalagring i hello molnet.
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: a4a1bc58-ea14-4bf5-b040-f85114edc1f1
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/09/2017
ms.author: robinsh
ms.openlocfilehash: f61324f98d0a8eb24023e4344acdb4ca58bb27f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
<!-- this is hello same version that is in hello MVC branch -->
# <a name="introduction-toomicrosoft-azure-storage"></a>Introduktion tooMicrosoft Azure Storage

Microsoft Azure Storage är en Microsoft-hanterad molntjänst som tillhandahåller lagring som är mycket tillgänglig, säker, beständig, skalbar och redundant. Microsoft tar hand om underhåll och hanterar kritiska problem åt dig. 

Azure Storage består av tre datatjänster: Blob Storage, File Storage och Queue Storage. BLOB storage stöder både standard- och premium-lagring med premium-lagring med hjälp av endast SSD för hello snabbaste prestanda möjligt. En annan funktion är lågfrekvent, så att du toostorage stora mängder data som sällan används för en lägre kostnad.

I den här artikeln får du lära dig om hello följande:
* hello Azure Storage-tjänster
* hello typerna av lagringskonton
* åtkomst till dina blobar, köer och filer
* kryptering
* replikering 
* överföring av data till och från lagring
* Hej många lagringsklientbiblioteken tillgängliga. 


<!-- RE-ENABLE THESE AFTER MVC GOES LIVE 
tooget up and running with Azure Storage quickly, check out one of hello following Quickstarts:
* [Create a storage account using PowerShell](storage-quick-create-storage-account-powershell.md)
* [Create a storage account using CLI](storage-quick-create-storage-account-cli.md)
-->


## <a name="introducing-hello-azure-storage-services"></a>Introduktion till hello Azure Storage-tjänster

toouse någon av hello tjänster som tillhandahålls av Azure Storage - Blob storage File storage och Queue storage--du först skapa ett lagringskonto och du kan sedan överföra data till och från en specifik tjänst i detta lagringskonto. 

## <a name="blob-storage"></a>Blob Storage

Blobbar är i princip filer som de du lagrar på en dator (eller surfplatta, mobil enhet och så vidare). De kan vara bilder, Microsoft Excel-filer, HTML-filer, virtuella hårddiskar (VHD), stordata, till exempel loggar, databassäkerhetskopior – nästan allt. Blobbar som lagras i behållare som är liknande toofolders. 

När du lagrar filer i Blob storage, du kan komma åt dem från valfri plats i hello world med hjälp av URL: er, hello REST-gränssnittet eller en av lagringsklientbiblioteken för hello Azure SDK. Det finns lagringsklientbibliotek för flera språk, bland annat Node.js, Java, PHP, Ruby, Python och .NET. 

Det finns tre typer av blobbar – blockblobbar, tilläggsblobbar och sidblobbar (används för VHD-filer).

* Blockblobbar är används toohold vanliga filer upp tooabout 4,7 TB. 
* Sidblobbar är filer som används toohold direktåtkomst in too8 TB i storlek. Dessa används för hello VHD-filer som stöder virtuella datorer.
* Lägg till BLOB består av block som hello blockblobbar, men är optimerade för tilläggsåtgärder. Dessa används för sådant som att logga information toohello samma blob från flera virtuella datorer.

För mycket stora datamängder där nätverksbegränsningar gör att överföra eller hämta tooBlob datalagring över hello överföring orealistiskt du levererar en uppsättning hårddiskar tooMicrosoft tooimport eller exportera data direkt från hello datacenter. Se [använda hello Microsoft Azure Import/Export Service tooTransfer Data tooBlob lagring](../storage-import-export-service.md).

## <a name="file-storage"></a>File Storage

hello Azure Files service kan du tooset in högtillgänglig nätverksfilresurser som kan nås via Server Message Block (SMB) hello standardprotokoll. Att hello innebär att flera virtuella datorer kan dela samma filer med läs- och skrivbehörighet. Du kan också läsa hello filer hello REST-gränssnittet eller hello storage, klientbiblioteken. 

En sak som särskiljer Azure File storage från filer på en filresurs som företagets är att du kan komma åt hello filer överallt i hello world med en URL som pekar toohello filen och innehåller en signatur (SAS) delad åtkomst-token. Du kan generera SAS-token de kan specifika tooa privata tillgång för en viss tidsperiod. 

Filresurser kan användas för många vanliga scenarier: 

* Många lokala program använder filresurser. Den här funktionen gör det enklare toomigrate programmen som delar data tooAzure. Om du monterar hello filen resursen toohello samma enhetsbeteckning som hello lokalt program använder, hello en del av ditt program som har åtkomst till filresursen för hello bör arbeta tillsammans med minimala eventuella ändringar.

* Konfigurationsfiler kan lagras på en filresurs och nås från flera virtuella datorer. Verktyg och hjälpmedel som används av flera utvecklare i en grupp kan lagras på en filresurs, se till att alla kan hitta dem, och att de använder hello samma version.

* Diagnostiska loggar, mätvärden och krascher Dumpar är tre exempel på data som kan skrivas tooa filresursen och bearbetas eller analyseras senare.

Vid denna tidpunkt, Active Directory-baserad autentisering och åtkomstkontroll-listorna (ACL) stöds inte, men de kommer att någon gång hello framtida. Hej lagringskontouppgifter är används tooprovide autentisering för åtkomst toohello filresurs. Detta innebär att vem som helst med hello resurs monterade har fullständig läsning och skrivning åtkomst toohello resursen.

## <a name="queue-storage"></a>Queue Storage

hello Azure-kötjänsten är används toostore och hämta meddelanden. Kömeddelanden kan vara upp too64 KB i storlek och en kö kan innehålla miljontals meddelanden. Köer är vanligen används toostore listor över meddelanden toobe bearbetas asynkront. 

Anta exempelvis att du vill att kunder toobe kan tooupload bilder, och du vill toocreate miniatyrbilder för varje bild. Du kan ha kunden vänta tills du toocreate hello miniatyrer under överföring hello bilder. Ett alternativ är toouse en kö. Skriva en meddelandekö toohello när hello kunden har slutförts hans överföringen. Sedan har en Azure-funktion hämta hello-meddelande från kön hello och skapa hello miniatyrerna. Var och en av hello delar av denna bearbetning kan skalas separat, vilket ger dig större kontroll när inställningen för din användning.

<!-- this bookmark is used by other articles; you'll need tooupdate them before this goes into production ROBIN-->
## <a name="table-storage"></a>Table Storage
<!-- add a link toohello old table storage toothis paragraph once it's moved -->
Standard Azure Table Storage är nu en del av Cosmos DB. Dessutom finns Premium Tables för Azure Table Storage, som erbjuder tabeller för optimerat dataflöde, global distribution och automatiska sekundära index. toolearn mer och prova hello nya bästa upplevelsen checka ut [Azure Cosmos DB: tabellen API](https://aka.ms/premiumtables).

## <a name="disk-storage"></a>Disklagring

hello Azure Storage team även äger diskar, som innehåller alla hello hanterade och ohanterade disk funktioner som används av virtuella datorer. Mer information om dessa funktioner finns i hello [dokumentation för Compute tjänsten](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).

## <a name="types-of-storage-accounts"></a>Typer av lagringskonton 

Den här tabellen visar hello olika typer av lagringskonton och vilka objekt som kan användas med varje.

|**Typ av lagringskonto**|**Allmän Standard**|**Allmän Premium**|**Blob Storage, frekvent och lågfrekvent åtkomstnivå**|
|-----|-----|-----|-----|
|**Tjänster som stöds**| Blob-, fil- och kötjänster | Blob-tjänst | Blob-tjänst|
|**Typer av blobbar som stöds**|Blockblobbar, sidblobbar och tilläggsblobbar | Sidblobbar | Blockblobbar och tilläggsblobbar|

### <a name="general-purpose-storage-accounts"></a>Allmänna lagringskonton

Det finns två typer av allmänna lagringskonton. 

#### <a name="standard-storage"></a>Standard Storage 

hello används mest storage-konton är standard storage-konton som kan användas för alla typer av data. Standard storage-konton använder magnetiska media toostore data.

#### <a name="premium-storage"></a>Premium Storage

Premium Storage ger högpresterande lagring för sidblobbar, som främst används för VHD-filer. Premium-lagringskonton använder SSD toostore data. Microsoft rekommenderar att du använder Premium Storage för alla dina virtuella datorer.

### <a name="blob-storage-accounts"></a>Blob Storage-konton

hello Blob Storage-konto är ett specialiserat lagringskonto används toostore blockblobbar och tilläggsblobbar. Du kan inte lagra sidblobbar i dessa konton, och därför kan du inte lagra VHD-filer. Dessa konton kan du tooset tooHot en åtkomst-nivå eller lågfrekvent; hello nivå kan ändras när som helst. 

hello frekvent åtkomstnivå används för filer som används ofta--du betalar högre kostnad för lagring, men hello kostnaden för att komma åt hello blobbar är mycket lägre. Du betalar högre kostnad för att komma åt hello BLOB för blobbar som lagras i hello kall åtkomstnivå, men hello kostnaden för lagring är mycket lägre.

## <a name="accessing-your-blobs-files-and-queues"></a>Åtkomst till dina blobbar, filer och köer

Varje lagringskonto har två autentiseringsnycklar, och båda kan användas för alla åtgärder. Det finns två nycklar så att du kan rulla över hello nycklar ibland tooenhance säkerhet. Det är viktigt att dessa nycklar hållas säkra eftersom innehar tillsammans med hello kontonamn tillåter obegränsad åtkomst tooall data i hello storage-konto. 

Det här avsnittet ser två sätt toosecure hello storage-konto och dess data. Detaljerad information om hur du skyddar ditt lagringskonto och dina data finns hello [säkerhetsguiden för Azure Storage](storage-security-guide.md).

### <a name="securing-access-toostorage-accounts-using-azure-ad"></a>Skydda åtkomsten toostorage konton med hjälp av Azure AD

Enkelriktade toosecure åtkomst tooyour storage-data är genom att kontrollera åtkomst toohello lagringskontonycklar. Du kan tilldela roller toousers, grupper eller program med Resource Manager rollbaserad åtkomstkontroll (RBAC). Rollerna är knutna tooa specifik uppsättning åtgärder som tillåts eller inte tillåts. Med RBAC hanterar toogrant åtkomst tooa lagringskonto endast hello hanteringsåtgärder för detta lagringskonto, till exempel ändra åtkomstnivå för hello. Du kan inte använda RBAC toogrant access toodata-objekt som en specifik behållare eller filresurs. Du kan dock använda RBAC toogrant åtkomst toohello lagringskontonycklar, som sedan kan använda tooread hello-dataobjekt. 

### <a name="securing-access-using-shared-access-signatures"></a>Skydda åtkomst med hjälp av signaturer för delad åtkomst 

Du kan använda signaturer för delad åtkomst och lagras åtkomst principer toosecure dataobjekt. En signatur för delad åtkomst (SAS) är en sträng som innehåller en säkerhetstoken som kan vara ansluten toohello URI för en tillgång som gör att du toodelegate åtkomst toospecific lagringsobjekt och toospecify begränsningarna, t.ex behörigheter och hello tidsvärdet mängd åtkomst. Den här funktionen har omfattande möjligheter. Detaljerad information finns för[med delad åtkomst signaturer (SAS)](storage-dotnet-shared-access-signature-part-1.md).

### <a name="public-access-tooblobs"></a>Allmän åtkomst tooblobs

hello Blob-tjänsten kan du tooprovide offentlig åtkomst tooa behållaren och dess blobbar, eller en specifik blobb. När du anger att en behållare eller blobb är offentlig kan alla läsa den anonymt; ingen autentisering krävs. Ett exempel på när du vill ha toodo detta är när du har en webbplats som använder bilder, video eller dokument från Blob storage. Mer information finns i [Hantera anonym läsbehörighet toocontainers och blobbar](../blobs/storage-manage-access-to-resources.md) 

## <a name="encryption"></a>Kryptering

Det finns några grundläggande typer av kryptering för hello Storage-tjänster. 

### <a name="encryption-at-rest"></a>Vilande kryptering 

Du kan aktivera kryptering för lagring-tjänsten (SSE) på antingen hello filer service (förhandsgranskning) eller hello Blob-tjänsten för Azure storage-konto. Om aktiverad, krypteras alla data som skrivs toohello specifik tjänst innan skrivs. När du läser hello data dekrypteras innan returneras. 

### <a name="client-side-encryption"></a>Kryptering av klientsidan

hello storage, klientbiblioteken har metoder som du kan anropa tooprogrammatically kryptera data innan den skickas över hello överföring från hello klienten tooAzure. De lagras krypterade, vilket innebär att de också är krypterade i vila. När du läser hello data tillbaka dekryptera hello information när du har fått den. 

### <a name="encryption-in-transit-with-azure-file-shares"></a>Kryptering under överföring med Azure-filresurser

Mer information om signaturer för delad åtkomst finns i [Använda signaturer för delad åtkomst (SAS)](../storage-dotnet-shared-access-signature-part-1.md). Se [Hantera anonym läsbehörighet toocontainers och blobbar](../blobs/storage-manage-access-to-resources.md) och [autentisering för hello Azure Storage-tjänster](https://msdn.microsoft.com/library/azure/dd179428.aspx) mer information om säker åtkomst tooyour storage-konto.

Mer information om hur du skyddar ditt lagringskonto och kryptering finns hello [säkerhetsguiden för Azure Storage](storage-security-guide.md).

## <a name="replication"></a>Replikering

I ordning tooensure att dina data skyddas, Azure Storage har hello möjlighet tookeep (och hantera) flera kopior av dina data. Det kallas replikering, eller ibland redundans. När du konfigurerar ditt lagringskonto väljer du replikeringstyp. I de flesta fall kan den här inställningen ändras när hello storage-konto har konfigurerats. 

Alla lagringskonton har **lokalt redundant lagring (LRS)**. Det innebär tre kopior av dina data hanteras av Azure Storage i hello Datacenter anges när hello storage-konto har ställts in. När ändringarna har bekräftats tooone kopiera, hello andra två kopior uppdateras innan det returneras lyckades. Det innebär att hello tre repliker alltid är synkroniserade. Dessutom hello tre kopior av finns i separata feldomäner och uppgradera domäner, vilket innebär att dina data är tillgängliga även om en lagringsnod hålla dina data misslyckas eller uppdateras tas offline toobe. 

**Lokalt redundant lagring (LRS)**

Som det står ovan har du med LRS tre kopior av dina data i ett enda datacenter. Detta hanterar hello problemet med data blir tillgängliga om en lagringsnod misslyckas eller tas offline toobe uppdaterats, men inte hello fallet med ett hela datacentret blir otillgänglig.

**Zonredundant lagring (ZRS)**

Zonredundant lagring (ZRS) underhålls hello tre lokala kopior av dina data och en annan uppsättning tre kopior av dina data. hello replikeras andra uppsättning tre kopior asynkront mellan datacenter inom en eller två regioner. Observera att ZRS endast är tillgängligt för blockblobbar i allmänna lagringskonton. När du har skapat ditt lagringskonto och valt ZRS kan du också kan inte konvertera det toouse tooany andra typ av replikering eller tvärtom.

ZRS-konton ger högre hållbarhet än LRS, men ZRS-konton har inte mått- eller loggningsfunktioner. 

**Geo-redundant lagring (GRS)**

GEO-redundant lagring (GRS) upprätthåller hello tre lokala kopior av dina data i en primär region och en annan uppsättning tre kopior av dina data i en sekundär region hundratals mil bort från hello primär region. Azure Storage hello händelse av ett fel vid hello primära region inte över toohello sekundär region. 

**Geo-redundant lagring med läsbehörighet (RA-GRS)** 

Geo-redundant lagring med läsbehörighet är exakt samma sätt som GRS förutom att du får tillgång till läsåtkomst toohello data hello sekundär plats. Om hello primära Datacenter blir tillfälligt otillgänglig, kan du fortsätta tooread hello data från hello sekundär plats. Det kan vara mycket användbart. Du kan till exempel ha ett webbprogram som ändrar till skrivskyddat läge och pekar toohello sekundär kopia, så att vissa åtkomst trots att uppdateringar inte är tillgängliga. 

> [!IMPORTANT]
> Du kan ändra hur dina data replikeras när ditt lagringskonto har skapats, såvida du inte valde ZRS när du skapade hello-konto. Observera dock att du kan innebära en ytterligare engångskostnader för dataöverföring om du växlar från LRS tooGRS eller RA-GRS.
>

Mer information om replikering finns i [Azure Storage-replikering](storage-redundancy.md).

Disaster recovery information finns i [vilka toodo om ett Azure Storage-avbrott inträffar](storage-disaster-recovery-guidance.md).

Ett exempel på hur tooleverage RA-GRS lagring tooensure hög tillgänglighet, se [skapar hög tillgängliga program med hjälp av RA-GRS](storage-designing-ha-apps-with-ragrs.md).

## <a name="transferring-data-tooand-from-azure-storage"></a>Överföra data tooand från Azure Storage

Du kan använda hello AzCopy kommandoradsverktyget toocopy blob och fildata i ditt lagringskonto eller mellan lagringskonton. Se en av hello följande artiklar för att få hjälp:

* [Överföra data med AzCopy för Windows](storage-use-azcopy.md)
* [Överföra data med AzCopy för Linux](storage-use-azcopy-linux.md)

AzCopy är byggt på hello [Azure Data Movement Library](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/), som är tillgängliga i förhandsversionen.

hello Azure Import/Export service kan vara används tooimport eller export stora mängder tooor för blob-data från ditt lagringskonto. Du förbereda e flera hårddiskar tooan Azure-datacenter, där de överför hello data till och från hello hårddiskar och skickar hello hårddiskar tillbaka tooyou. Läs mer om hello Import/Export service [använda hello Microsoft Azure Import/Export Service tooTransfer Data tooBlob lagring](../storage-import-export-service.md).

## <a name="pricing"></a>Prissättning

Detaljerad information om priser för Azure Storage finns hello [priser sidan](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="storage-apis-libraries-and-tools"></a>API:er, bibliotek och verktyg för Azure Storage
Azure Storage-resurser kan nås med alla språk som kan skicka HTTP/HTTPS-förfrågningar. Dessutom erbjuder Azure Storage programmeringsbibliotek för flera populära språk. Dessa bibliotek förenklar många aspekter av arbetet med Azure Storage genom att hantera information som till exempel synkrona och asynkrona anrop, massbearbetning av åtgärder, undantagshantering, automatiska omförsök, funktionsbeteenden och så vidare. Bibliotek är tillgängliga för hello följande språk och plattformar, med andra i hello pipeline:

### <a name="azure-storage-data-services"></a>Azure Storage Data Services
* [REST-API för Storage Services](/rest/api/storageservices/)
* [Storage-klientbibliotek för .NET](https://docs.microsoft.com/dotnet/api/?view=azurestorage-8.1.1)
* [Storage-klientbibliotek för C++](https://github.com/Azure/azure-storage-cpp)
* [Storage-klientbibliotek för Java/Android](https://azure.microsoft.com/develop/java/)
* [Storage-klientbibliotek för Node.js](http://dl.windowsazure.com/nodestoragedocs/index.html)
* [Storage-klientbibliotek för PHP](https://azure.microsoft.com/develop/php/)
* [Storage-klientbibliotek för Python](https://azure.microsoft.com/develop/python/)
* [Storage-klientbibliotek för Ruby](https://azure.microsoft.com/develop/ruby/)
* [Storage-cmdletar för PowerShell](/powershell/module/azure.storage/?view=azurermps-4.1.0&viewFallbackFrom=azurermps-4.0.0)
* [Storage-kommandon för CLI 2.0](/cli/azure/storage)

## <a name="next-steps"></a>Nästa steg

* [Mer information om Blob Storage](../blobs/storage-blobs-introduction.md)
* [Mer information om File Storage](../storage-files-introduction.md)
* [Mer information om Queue Storage](../queues/storage-queues-introduction.md)

<!-- RE-ENABLE THESE AFTER MVC GOES LIVE 
tooget up and running with Azure Storage quickly, check out one of hello following Quickstarts:
* [Create a storage account using PowerShell](storage-quick-create-storage-account-powershell.md)
* [Create a storage account using CLI](storage-quick-create-storage-account-cli.md)
-->

<!-- FIGURE OUT WHAT tooDO WITH ALL THESE LINKS.

Azure Storage resources can be accessed by any language that can make HTTP/HTTPS requests. Additionally, Azure Storage offers programming libraries for several popular languages. These libraries simplify many aspects of working with Azure Storage by handling details such as synchronous and asynchronous invocation, batching of operations, exception management, automatic retries, operational behavior and so forth. Libraries are currently available for hello following languages and platforms, with others in hello pipeline:

### Azure Storage data services
* [Storage Services REST API](https://docs.microsoft.com/rest/api/storageservices/)
* [Storage Client Library for .NET](https://docs.microsoft.com/dotnet/api/?view=azurestorage-8.1.1)
* [Storage Client Library for C++](https://github.com/Azure/azure-storage-cpp)
* [Storage Client Library for Java/Android](https://azure.microsoft.com/develop/java/)
* [Storage Client Library for Node.js](http://dl.windowsazure.com/nodestoragedocs/index.html)
* [Storage Client Library for PHP](https://azure.microsoft.com/develop/php/)
* [Storage Client Library for Python](https://azure.microsoft.com/develop/python/)
* [Storage Client Library for Ruby](https://azure.microsoft.com/develop/ruby/)
* [Storage Cmdlets for PowerShell](/powershell/module/azure.storage/?view=azurermps-4.1.0&viewFallbackFrom=azurermps-4.0.0)

### Azure Storage management services
* [Storage Resource Provider REST API Reference](/rest/api/storagerp/)
* [Storage Resource Provider Client Library for .NET](/dotnet/api/microsoft.azure.management.storage)
* [Storage Resource Provider Cmdlets for PowerShell 1.0](/powershell/module/azure.storage)
* [Storage Service Management REST API (Classic)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### Azure Storage data movement services
* [Storage Import/Export Service REST API](../storage-import-export-service.md)
* [Storage Data Movement Client Library for .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### Tools and utilities
* [Microsoft Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.
* [Azure Storage Client Tools](../storage-explorers.md)
* [Azure SDKs and Tools](https://azure.microsoft.com/tools/)
* [Azure Storage Emulator](http://www.microsoft.com/download/details.aspx?id=43709)
* [Azure PowerShell](/powershell/azure/overview)
* [AzCopy Command-Line Utility](http://aka.ms/downloadazcopy)

## Next steps
toolearn more about Azure Storage, explore these resources:

### Documentation
* [Azure Storage Documentation](https://azure.microsoft.com/documentation/services/storage/)
* [Create a storage account](../storage-create-storage-account.md)

<!-- after our quick starts are available, replace this link with a link tooone of those. 
Had tooremove this article, it refers toohello VS quickstarts, and they've stopped publishing them. Robin --> 
<!--* [Get started with Azure Storage in five minutes](storage-getting-started-guide.md)
-->

### <a name="for-administrators"></a>För administratörer
* [Använd Azure PowerShell med Azure Storage](storage-powershell-guide-full.md)
* [Använda Azure CLI med Azure Storage](../storage-azure-cli.md)

### <a name="for-net-developers"></a>För .NET-utvecklare
* [Komma igång med Azure Blob Storage med hjälp av .NET](../blobs/storage-dotnet-how-to-use-blobs.md)
* [Komma igång med Azure Table Storage med hjälp av .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Komma igång med Azure Queue Storage med hjälp av .NET](../storage-dotnet-how-to-use-queues.md)
* [Komma igång med Azure File Storage i Windows](../storage-dotnet-how-to-use-files.md)

### <a name="for-javaandroid-developers"></a>För Java-/Android-utvecklare
* [Hur toouse Blob storage från Java](../blobs/storage-java-how-to-use-blob-storage.md)
* [Hur toouse Table storage från Java](../../cosmos-db/table-storage-how-to-use-java.md)
* [Hur toouse Queue storage från Java](../storage-java-how-to-use-queue-storage.md)
* [Hur toouse File storage från Java](../storage-java-how-to-use-file-storage.md)

### <a name="for-nodejs-developers"></a>För Node.js-utvecklare
* [Hur toouse Blob storage från Node.js](../blobs/storage-nodejs-how-to-use-blob-storage.md)
* [Hur toouse Table storage från Node.js](../../cosmos-db/table-storage-how-to-use-nodejs.md)
* [Hur toouse Queue storage från Node.js](../storage-nodejs-how-to-use-queues.md)

### <a name="for-php-developers"></a>För PHP-utvecklare
* [Hur toouse Blob storage från PHP](../blobs/storage-php-how-to-use-blobs.md)
* [Hur toouse Table storage från PHP](../../cosmos-db/table-storage-how-to-use-php.md)
* [Hur toouse Queue storage från PHP](../storage-php-how-to-use-queues.md)

### <a name="for-ruby-developers"></a>För Ruby-utvecklare
* [Hur toouse Blob storage från Ruby](../blobs/storage-ruby-how-to-use-blob-storage.md)
* [Hur toouse Table storage från Ruby](../../cosmos-db/table-storage-how-to-use-ruby.md)
* [Hur toouse Queue storage från Ruby](../storage-ruby-how-to-use-queue-storage.md)

### <a name="for-python-developers"></a>För Python-utvecklare
* [Hur toouse Blob storage från Python](../blobs/storage-python-how-to-use-blob-storage.md)
* [Hur toouse Table storage från Python](../../cosmos-db/table-storage-how-to-use-python.md)
* [Hur toouse Queue storage från Python](../storage-python-how-to-use-queue-storage.md)   
* [Hur toouse File storage från Python](../storage-python-how-to-use-file-storage.md) 
-->