---
title: aaaManaging samtidighet i Microsoft Azure Storage
description: "Hur toomanage samtidighet för hello Blob, kön, tabell, och Filtjänster"
services: storage
documentationcenter: 
author: jasontang501
manager: tadb
editor: tysonn
ms.assetid: cc6429c4-23ee-46e3-b22d-50dd68bd4680
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/11/2017
ms.author: jasontang501
ms.openlocfilehash: 277fbbb880906da6be67b2267ed5c8e457455bd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-concurrency-in-microsoft-azure-storage"></a>Hantera samtidighet i Microsoft Azure Storage
## <a name="overview"></a>Översikt
Moderna Internet-baserade program har vanligtvis flera användare visa och uppdatera data samtidigt. Detta kräver programmet utvecklare toothink noggrant om hur tooprovide en förutsägbar uppleva tootheir slutanvändare, särskilt för scenarier där flera användare kan uppdatera hello samma data. Det finns tre huvudsakliga databassamtidighet strategier beaktas vanligtvis utvecklare:  

1. Optimistisk samtidighet – ett program som utför en uppdatering som en del av dess uppdatering verifierar om hello data har ändrats sedan programmet hello senast läsa data. Till exempel om två användare som visar en wiki-sida gör en uppdatering toohello sidan samma hello wiki-plattformen måste se till att andra update hello inte över första uppdatering av hello- och båda användare att förstå om uppdateringen lyckades eller inte. Den här strategin används oftast i webbprogram.
2. Sämsta samtidighet – ett program som söker tooperform en uppdatering tar ett lås på ett objekt som förhindrar att andra användare uppdaterar hello data förrän hello Lås släpps. Till exempel i över-/ underordnade data replikering scenario där endast hello master utför uppdateringar innehåller hello master vanligtvis ett exklusivt lås för en längre tid på hello data tooensure utan någon annan kan uppdatera den.
3. Den senaste skrivaren wins – en metod som gör att alla update operations tooproceed utan att verifiera om något annat program har uppdaterats hello data eftersom programmet hello först läsa hello data. Den här strategin (eller brist på en formell strategi) används vanligtvis när data är partitionerad så att det finns inga sannolikheten att flera användare kommer åt hello samma data. Det kan också vara användbara där tillfällig dataströmmar bearbetas.  

Den här artikeln innehåller en översikt över hur hello Azure Storage-plattformen förenklar utvecklingen som har förstklassigt stöd för alla tre strategierna samtidighet.  

## <a name="azure-storage--simplifies-cloud-development"></a>Azure-lagring – förenklar utvecklingen i molnet
hello Azure storage-tjänst stöder alla tre strategier, men det är olika i dess möjlighet tooprovide fullständigt stöd för bästa och sämsta samtidighet eftersom den var utformad tooembrace en stark konsekvens-modell som garanterar att om Hej Storage service incheckningar data infoga eller uppdatera alla ytterligare åtkomst till toothat data kommer att se hello uppdatera senaste åtgärden. Storage-plattformar som använder en modell för slutliga konsekvensen har en fördröjning mellan när en skrivning utförs av en användare och när hello uppdaterade data kan ses av andra användare således vilket komplicerar utvecklingen av klientprogram i ordning tooprevent inkonsekvenser från påverkar slutanvändarna.  

Dessutom tooselecting en lämplig samtidighet strategi utvecklare bör också känna till hur en plattform för lagring isolerar ändringar – särskilt ändringar toohello samma objekt över transaktioner. hello Azure storage-tjänst använder ögonblicksbilden isolering tooallow läsa operations toohappen samtidigt skrivåtgärder inom en partition. Till skillnad från andra isoleringsnivåer garanterar ögonblicksbildisolering att alla Läs finns en programkonsekvent ögonblicksbild av hello data även när uppdateringar sker – i princip genom att returnera hello senast allokerat värdena medan en uppdateringstransaktion bearbetas.  

## <a name="managing-concurrency-in-blob-storage"></a>Hantera samtidighet i Blob storage
Du kan välja toouse antingen bästa eller sämsta samtidighet modeller toomanage åtkomst tooblobs och behållare i hello blob-tjänsten. Om du inte uttryckligen anger en strategi senast skriver wins är hello standard.  

### <a name="optimistic-concurrency-for-blobs-and-containers"></a>Optimistisk samtidighet för blobbar och behållare
hello lagringstjänsten tilldelar ett identifierare tooevery objekt som lagras. Den här identifieraren uppdateras varje gång en update-åtgärd utförs på ett objekt. hello identifierare returneras toohello klienten inom ramen för en HTTP GET-svar med hello ETag (entitetstaggen)-huvudet som har definierats inom hello HTTP-protokollet. En användare som utför en uppdatering på sådana objekt kan skicka i hello ursprungliga ETag tillsammans med ett villkorat huvud tooensure som en uppdatering görs bara om ett visst villkor har uppfyllts – i det här fallet hello villkoret är ett ”If-Match”-huvud som kräver hello lagring Tjänsten tooensure hello värde hello ETag som anges i begäran om uppdatering hello är hello samma som lagras i hello Storage-tjänst.  

hello beskrivning av den här processen är följande:  

1. Hämta en blob från hello lagringstjänsten, hello svaret innehåller ett ETag-rubriken i HTTP-värde som anger hello nuvarande version av hello objekt i hello storage-tjänst.
2. När du uppdaterar hello blob inkluderar hello ETag-värde som du fick i steg 1 i hello **If-Match** villkorat huvud för hello-begäran som du skickar toohello service.
3. hello tjänsten jämför hello ETag-värde i hello begäran med hello aktuella ETag-värde hello-blob.
4. Om hello aktuella ETag-värde hello blob är en annan version än hello ETag i hello **If-Match** villkorat huvud i begäran hello hello-tjänsten returnerar en 412 fel toohello klient. Detta anger toohello klienten en annan process har uppdaterat hello blob eftersom hello klienten hämtades.
5. Om hello aktuella hello ETag hello blob-värdet är samma version som hello ETag i hello **If-Match** villkorat huvud i begäran hello hello-tjänsten utför hello begärde åtgärden och uppdateringar hello hello blob aktuella ETag-värde tooshow att den har skapat en ny version.  

hello följande C# utdrag (med hello Storage-klientbiblioteket 4.2.0) visar ett enkelt exempel på hur tooconstruct en **If-Match AccessCondition** baserat på hello ETag-värde som öppnas från hello egenskaperna för en blob som var tidigare hämtade eller infogade. Därefter använder hello **AccessCondition** objekt när den uppdaterar hello blob: hello **AccessCondition** objekt lägger till hello **If-Match** huvud toohello begäran. Om en annan process har uppdaterat hello blob, returnerar hello blob-tjänsten ett statusmeddelande HTTP 412 (villkor misslyckades). Du kan hämta hello fullständigt exempel här: [hantera samtidighet med hjälp av Azure Storage](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).  

```csharp
// Retrieve hello ETag from hello newly created blob
// Etag is already populated as UploadText should cause a PUT Blob call
// toostorage blob service which returns hello etag in response.
string orignalETag = blockBlob.Properties.ETag;

// This code simulates an update by a third party.
string helloText = "Blob updated by a third party.";

// No etag, provided so orignal blob is overwritten (thus generating a new etag)
blockBlob.UploadText(helloText);
Console.WriteLine("Blob updated. Updated ETag = {0}",
blockBlob.Properties.ETag);

// Now try tooupdate hello blob using hello orignal ETag provided when hello blob was created
try
{
    Console.WriteLine("Trying tooupdate blob using orignal etag toogenerate if-match access condition");
    blockBlob.UploadText(helloText,accessCondition:
    AccessCondition.GenerateIfMatchCondition(orignalETag));
}
catch (StorageException ex)
{
    if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
    {
        Console.WriteLine("Precondition failure as expected. Blob's orignal etag no longer matches");
        // TODO: client can decide on how it wants toohandle hello 3rd party updated content.
    }
    else
        throw;
}  
```

hello lagringstjänsten omfattar också stöd för ytterligare villkorlig huvuden som **If-Modified-Since**, **If-Unmodified-Since** och **If-None-Match** samt kombinationer av dessa. Mer information finns i [ange villkorlig huvuden för Blob-tjänståtgärder](http://msdn.microsoft.com/library/azure/dd179371.aspx) på MSDN.  

hello följande tabell sammanfattas hello behållaren åtgärder som accepterar villkorlig huvuden som **If-Match** hello begäran och som returnerar ett ETag-värde i hello svar.  

| Åtgärd | Returnerar behållare ETag-värde | Accepterar villkorlig rubriker |
|:--- |:--- |:--- |
| Skapa en behållare |Ja |Nej |
| Hämta egenskaper för behållaren |Ja |Nej |
| Hämta Metadata för behållaren |Ja |Nej |
| Ange Metadata för behållaren |Ja |Ja |
| Hämta behållar-ACL |Ja |Nej |
| Ange behållar-ACL |Ja |Ja (*) |
| Ta bort behållaren |Nej |Ja |
| Lånet behållare |Ja |Ja |
| Lista över Blobbar |Nej |Nej |

(*) hello behörigheter som definieras av SetContainerACL cachelagras och uppdateringar toothese behörighet ta 30 sekunder toopropagate under vilka uppdateringar inte är garanterat toobe konsekvent.  

hello följande tabell sammanfattas hello blob-åtgärder som accepterar villkorlig huvuden som **If-Match** hello begäran och som returnerar ett ETag-värde i hello svar.

| Åtgärd | Returnerar ETag-värde | Accepterar villkorlig rubriker |
|:--- |:--- |:--- |
| Placera Blob |Ja |Ja |
| Hämta Blob |Ja |Ja |
| Hämta Blob-egenskaper |Ja |Ja |
| Ange Blob-egenskaper |Ja |Ja |
| Hämta Blobbmetadata |Ja |Ja |
| Ange Blobbmetadata |Ja |Ja |
| Lånet Blob (*) |Ja |Ja |
| Snapshot-Blob |Ja |Ja |
| Kopiera Blob |Ja |Ja (för käll- och blob) |
| Avbryt kopiera Blob |Nej |Nej |
| Ta bort blobben |Nej |Ja |
| Placera Block |Nej |Nej |
| Placera Blockeringslista |Ja |Ja |
| Hämta listan över blockerade |Ja |Nej |
| Placera sida |Ja |Ja |
| Get-sidintervall |Ja |Ja |

(*) Lånet Blob ändras inte hello ETag på en blob.  

### <a name="pessimistic-concurrency-for-blobs"></a>Sämsta samtidighet för blobbar
toolock en blob för exklusiv användning, kan du hämta en [lån](http://msdn.microsoft.com/library/azure/ee691972.aspx) på den. När du skaffa en leasing anger du hur länge du behöver hello lån: Detta kan vara för mellan 15 too60 sekunder eller oändlig vilka belopp tooan exklusivt lås. Du kan förnya ett begränsat lån tooextend den, och du kan släppa ett lån när du är klar med den. hello blob-tjänsten Frigör ändligt lån automatiskt när de upphör att gälla.  

Lån Aktivera synkronisering av olika strategier toobe stöds, inklusive exklusiv skrivning / delade skrivskyddade, exklusiv skrivning / exklusiv Läs- och delade skrivning / läsa samtidigt. Om ett lån finns tillämpar hello lagringstjänsten exklusiv skrivningar (put, ange och delete-åtgärder) men säkerställt ensamrätt för läsåtgärder kräver hello developer tooensure att alla program använder ett lån-ID och att endast en klient i taget har ett giltigt lease-ID. Läs-och skrivåtgärder som inte innehåller ett lån ID resultat i delade läsningar.  

hello visar följande C# fragment ett exempel för att erhålla ett exklusiv adresslån i 30 sekunder på en blob, uppdatera hello innehållet i hello blob och släppa hello lån. Om det finns redan ett giltigt lån på hello blob när du försöker tooacquire ett nytt lån, returnerar hello blob-tjänsten ett resultat för ”HTTP (409) konflikt” status. hello fragment nedan använder en **AccessCondition** objekt tooencapsulate hello lease-information när den är en begäran tooupdate hello blob med hello storage-tjänst.  Du kan hämta hello fullständigt exempel här: [hantera samtidighet med hjälp av Azure Storage](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

```csharp
// Acquire lease for 15 seconds
string lease = blockBlob.AcquireLease(TimeSpan.FromSeconds(15), null);
Console.WriteLine("Blob lease acquired. Lease = {0}", lease);

// Update blob using lease. This operation will succeed
const string helloText = "Blob updated";
var accessCondition = AccessCondition.GenerateLeaseCondition(lease);
blockBlob.UploadText(helloText, accessCondition: accessCondition);
Console.WriteLine("Blob updated using an exclusive lease");

//Simulate third party update tooblob without lease
try
{
    // Below operation will fail as no valid lease provided
    Console.WriteLine("Trying tooupdate blob without valid lease");
    blockBlob.UploadText("Update without lease, will fail");
}
catch (StorageException ex)
{
    if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
        Console.WriteLine("Precondition failure as expected. Blob's lease does not match");
    else
        throw;
}  
```

Om du försöker en skrivåtgärd i en blob-lånade utan att skicka hello lån ID misslyckas hello begäran 412 fel. Observera att om hello lånet går ut innan du anropar hello **UploadText** metoden men du fortfarande skicka hello lease-ID, hello begäran också misslyckas med en **412** fel. Mer information om hur du hanterar lånetid för förfallodatum och lease-ID: n finns hello [lån Blob](http://msdn.microsoft.com/library/azure/ee691972.aspx) REST-dokumentationen.  

hello kan följande blob-åtgärder använda lån toomanage sämsta samtidighet:  

* Placera Blob
* Hämta Blob
* Hämta Blob-egenskaper
* Ange Blob-egenskaper
* Hämta Blobbmetadata
* Ange Blobbmetadata
* Ta bort blobben
* Placera Block
* Placera Blockeringslista
* Hämta listan över blockerade
* Placera sida
* Get-sidintervall
* Ögonblicksbild Blob - lån-ID: T är valfri om ett lån finns
* Kopiera Blob - lån ID krävs om det finns ett lån på hello mål-blob
* Avbryt kopiera Blob - lån ID krävs om det finns en oändlig lån på hello mål-blob
* Lånet Blob  

### <a name="pessimistic-concurrency-for-containers"></a>Sämsta samtidighet för behållare
Lån på behållare aktivera hello samma synkronisering strategier toobe stöds som på blobbar (exklusiv Skriv- / delade skrivskyddade, exklusiv skrivning / exklusiv Läs- och delade skrivning / samtidigt läsa) men till skillnad från BLOB storage-tjänst för hello endast tillämpar ensamrätt på delete-åtgärder. en klient måste innehålla hello aktiv livslängd ID med hello raderingsbegäran toodelete en behållare med en aktiv livslängd. Alla andra åtgärder i behållaren lyckas på lånade behållaren utan att inkludera hello lån ID då de delas åtgärder. Om ensamrätt av update (put eller set) eller läsåtgärder krävs ska utvecklare Kontrollera alla klienter använder ett lån-ID och som endast en klient i taget har ett giltigt lease-ID.  

hello kan följande åtgärder på en behållare använda lån toomanage sämsta samtidighet:  

* Ta bort behållaren
* Hämta egenskaper för behållaren
* Hämta Metadata för behållaren
* Ange Metadata för behållaren
* Hämta behållar-ACL
* Ange behållar-ACL
* Lånet behållare  

Mer information finns i:  

* [Ange villkorlig huvuden för Blob-tjänståtgärder](http://msdn.microsoft.com/library/azure/dd179371.aspx)
* [Lånet behållare](http://msdn.microsoft.com/library/azure/jj159103.aspx)
* [Lånet Blob](http://msdn.microsoft.com/library/azure/ee691972.aspx)

## <a name="managing-concurrency-in-hello-table-service"></a>Hantera samtidighet i hello tabelltjänst
Hej tabelltjänsten använder Optimistisk samtidighet kontrollerar som hello standardbeteendet när du arbetar med entiteter, till skillnad från hello blob-tjänsten där du måste uttryckligen välja tooperform Optimistisk samtidighet kontroller. hello andra skillnaden mellan hello tabell och blob-tjänster är att du kan endast hantera hello samtidighet beteendet för entiteter medan hello blob-tjänsten kan du hantera hello samtidighet på både behållare och blobbar.  

Du kan använda hello ETag-värde som du får när hello tabelltjänsten returnerar en entitet toouse Optimistisk samtidighet och toocheck om en annan process har ändrat en entitet eftersom hämtades från hello table storage-tjänsten. hello beskrivning av den här processen är följande:  

1. Hämta en entitet från hello table storage-tjänsten, hello svaret innehåller ett ETag-värde som anger hello aktuella identifierare som är associerad med den entiteten i hello storage-tjänst.
2. När du uppdaterar hello entiteten innehåller hello ETag-värde som du fick i steg 1 i hello obligatoriska **If-Match** huvudet i hello-begäran som du skickar toohello service.
3. hello tjänsten jämför hello ETag-värde i hello begäran med hello aktuella ETag-värde hello entitet.
4. Om hello aktuella ETag-värde hello entitet skiljer sig hello ETag i hello obligatoriska **If-Match** huvudet i begäran hello hello-tjänsten returnerar en 412 fel toohello klient. Detta anger toohello klienten en annan process har uppdaterat hello entiteten eftersom hello klienten hämtades.
5. Om hello aktuella ETag-värde hello entiteten är hello samma som hello ETag i hello obligatoriska **If-Match** huvudet i begäran hello eller hello **If-Match** huvudet innehåller hello jokertecknet (*) hello-tjänsten Utför hello begärde åtgärden och uppdateringar hello aktuella ETag-värde hello entiteten tooshow den har uppdaterats.  

Observera att hello tabelltjänsten till skillnad från hello blob-tjänsten kräver hello klienten tooinclude en **If-Match** huvudet i begäranden om att uppdatera. Det är dock möjligt tooforce en ovillkorlig uppdatera (senaste skrivaren WINS-strategi) och kringgå samtidighet kontrollerar om hello klienten anger hello **If-Match** huvud jokertecken toohello (*) i hello-begäran.  

hello följande C# fragment visar en kundentitet som skapades tidigare antingen eller hämtas med deras e-postadress som uppdateras. hello inledande infoga eller hämta åtgärden lagrar hello ETag-värde i hello kunden objekt och eftersom hello används hello samma objektinstansen när den körs hello ersättningsåtgärden, skickar automatiskt hello ETag-värdet tillbaka toohello tabellen service aktiverar hello service toocheck för samtidighet överträdelser. Om en annan process har uppdaterat hello entiteten i table storage, returnerar hello-tjänsten ett statusmeddelande HTTP 412 (villkor misslyckades).  Du kan hämta hello fullständigt exempel här: [hantera samtidighet med hjälp av Azure Storage](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

```csharp
try
{
    customer.Email = "updatedEmail@contoso.org";
    TableOperation replaceCustomer = TableOperation.Replace(customer);
    customerTable.Execute(replaceCustomer);
    Console.WriteLine("Replace operation succeeded.");
}
catch (StorageException ex)
{
    if (ex.RequestInformation.HttpStatusCode == 412)
        Console.WriteLine("Optimistic concurrency violation – entity has changed since it was retrieved.");
    else
        throw;
}  
```

tooexplicitly inaktivera hello simultankontroll, bör du ange hello **ETag** -egenskapen för hello **medarbetare** objekt för ”*” innan du kan köra hello ersättningsåtgärden.  

```csharp
customer.ETag = "*";  
```

hello följande tabell visas hur hello tabellåtgärder entitet använder ETag-värden:

| Åtgärd | Returnerar ETag-värde | Kräver If-Match huvudet i begäran |
|:--- |:--- |:--- |
| Fråga entiteter |Ja |Nej |
| Infoga entitet |Ja |Nej |
| Uppdatera entitet |Ja |Ja |
| Sammanfoga entitet |Ja |Ja |
| Ta bort entiteten |Nej |Ja |
| Infoga eller ersätta en entitet |Ja |Nej |
| Infoga- eller Merge-entitet |Ja |Nej |

Observera att hello **infoga eller ersätta entiteten** och **Insert- eller Merge-entiteten** operations vill *inte* utföra alla kontroller av samtidighet eftersom de inte skicka en toohello för ETag-värde tabelltjänsten.  

I allmänhet utvecklare som använder tabeller bör förlitar sig på Optimistisk samtidighet när du utvecklar skalbara program. Om sämsta låsning krävs en metod som utvecklare vidta när åtkomst till tabeller är tooassign avsedda blob för varje tabell och försök tootake ett lån på hello blob innan du arbetar hello tabell. Den här metoden kräver hello programmet tooensure alla dataåtkomst sökvägar hämta hello lån tidigare toooperating för hello tabellen. Observera även att hello minsta lånetid är 15 sekunder som kräver noggrant övervägande för skalbarhet.  

Mer information finns i:  

* [Åtgärder på enheter](http://msdn.microsoft.com/library/azure/dd179375.aspx)  

## <a name="managing-concurrency-in-hello-queue-service"></a>Hantera samtidighet i hello kötjänsten
Ett scenario som är ett problem i hello kö service i vilket concurrency är där flera klienter hämtar meddelanden från en kö. När ett meddelande har hämtats från hello kö innehåller hello svaret hello-meddelande och ett pop inleverans-värde som är nödvändiga toodelete hello-meddelande. hello-meddelande tas inte bort automatiskt från hello kön, men när den har hämtats, det är inte synliga tooother klienter för hello tidsintervall som anges av hello visibilitytimeout-parametern. hello-klienten som hämtar hello-meddelande är förväntade toodelete hello-meddelande när den har bearbetats och innan hello som anges av hello TimeNextVisible element hello svar, som beräknas utifrån hello värdet för hello visibilitytimeout parameter. hello-värdet för visibilitytimeout läggs toohello tid vid vilken hello-meddelande är hämtade toodetermine hello värdet för TimeNextVisible.  

hello kötjänsten har inte stöd för bästa eller sämsta samtidighet och för den här orsak klienter bearbetar meddelanden som hämtas från en kö ska kontrollera meddelanden bearbetas på ett idempotent sätt. En senaste skrivaren WINS-strategi används för update-åtgärder som till exempel SetQueueServiceProperties, SetQueueMetaData, SetQueueACL och UpdateMessage.  

Mer information finns i:  

* [Kön Service REST-API](http://msdn.microsoft.com/library/azure/dd179363.aspx)
* [Hämta meddelanden](http://msdn.microsoft.com/library/azure/dd179474.aspx)  

## <a name="managing-concurrency-in-hello-file-service"></a>Hantera samtidighet i hello Filtjänst
Hej filtjänst kan nås med hjälp av två olika protokollslutpunkterna – SMB- och REST. hello REST-tjänst har inte stöd för antingen optimistisk låsning eller sämsta låser och alla uppdateringar följer en senaste skrivaren WINS-strategi. SMB-klienter som montera filresurser kan utnyttja filen låsning mekanismer toomanage åtkomst tooshared systemfiler – inklusive hello möjlighet tooperform sämsta låsning. När en SMB-klienten öppnar en fil, anger både hello filåtkomst och dela läge. En filåtkomst alternativet för ”skriva” eller ”läsa/skriva” tillsammans med en filresurs läge ”NONE” leder hello-fil som låst av en SMB-klient tills hello-filen är stängd. Om REST-åtgärden utförs på en fil där en SMB-klienten har hello filen låst returnerar hello REST-tjänst statuskod 409 (konflikt) med felkoden SharingViolation.  

När en SMB-klienten öppnar en fil att ta bort, markerar hello-fil som väntar på borttagning förrän alla andra SMB-klienten med öppna referenser i filen är stängd. När en fil är markerad som väntar på borttagning, returneras alla REST-åtgärden på filen statuskod 409 (konflikt) med felkoden SMBDeletePending. Statuskod 404 (inget hittas) returneras inte eftersom det är möjligt för hello SMB-klienten tooremove hello väntande borttagning flaggan tidigare tooclosing hello-filen. Statuskod 404 (inget hittas) förväntas med andra ord bara när hello-filen har tagits bort. Observera att när en fil är i en SMB väntetillstånd delete kan den inte inkluderas i hello lista filer resultat. Observera att hello REST ta bort fil- och REST ta bort åtgärder genomförs automatiskt och inte leder till väntande bort raderas även tillstånd.  

Mer information finns i:  

* [Hantera filen låser](http://msdn.microsoft.com/library/azure/dn194265.aspx)  

## <a name="summary-and-next-steps"></a>Sammanfattning och nästa steg.
hello utformats Microsoft Azure Storage-tjänsten har för toomeet hello mest komplexa Onlineprogram hello behov utan att utvecklare toocompromise eller Tänk om viktiga designbeslut antaganden som samtidighet och data konsekvent att de kommer tootake för beviljas.  

Utför exempelprogram som refereras till i den här bloggen för hello:  

* [Hantera samtidighet med hjälp av Azure Storage - exempelprogram](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114)  

Mer information om Azure Storage finns:  

* [Startsida för Microsoft Azure Storage](https://azure.microsoft.com/services/storage/)
* [Introduktion tooAzure lagring](storage-introduction.md)
* Komma igång för lagring [Blob](storage-dotnet-how-to-use-blobs.md), [tabell](storage-dotnet-how-to-use-tables.md), [köer](storage-dotnet-how-to-use-queues.md), och [filer](storage-dotnet-how-to-use-files.md)
* Lagringsarkitektur – [Azure Storage: en högtillgänglig Cloud Storage Service with Strong Consistency](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

