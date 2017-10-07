---
title: "aaaHow toouse Azure Table storage från Node.js | Microsoft Docs"
description: Lagra strukturerade data i hello molnet med Azure Table storage, en NoSQL-databas.
services: cosmos-db
documentationcenter: nodejs
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: fc2e33d2-c5da-4861-8503-53fdc25750de
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.openlocfilehash: 21022491a9a21a5365628de93582ea3a325ed869
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-table-storage-from-nodejs"></a>Hur toouse Azure Table storage från Node.js
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Översikt
Det här avsnittet visar hur tooperform vanliga scenarier med hjälp av hello Azure Table-tjänsten i ett Node.js-program.

hello kodexempel i det här avsnittet förutsätter att du redan har ett Node.js-program. Information om hur toocreate ett Node.js-program i Azure, se några av följande avsnitt:

* [Skapa en Node.js-webbapp i Azure App Service](../app-service-web/app-service-web-get-started-nodejs.md)
* [Skapa och distribuera en Node.js web app tooAzure med WebMatrix](../app-service-web/web-sites-nodejs-use-webmatrix.md)
* [Skapa och distribuera en Node.js-programmet tooan Azure Cloud Service](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (med Windows PowerShell)

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="configure-your-application-tooaccess-azure-storage"></a>Konfigurera ditt program tooaccess Azure Storage
toouse Azure Storage, behöver du hello Azure Storage SDK för Node.js som innehåller en uppsättning bekvämlighet bibliotek som kommunicerar med hello storage REST-tjänster.

### <a name="use-node-package-manager-npm-tooinstall-hello-package"></a>Använd noden Package Manager (NPM) tooinstall hello-paket
1. Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac), eller **Bash** (Unix) och gå toohello mappen där du skapade ditt program.
2. Typen **npm installera azure-lagring** i hello kommandofönstret. Utdata från kommandot hello är liknande toohello följande exempel.

       azure-storage@0.5.0 node_modules\azure-storage
       +-- extend@1.2.1
       +-- xmlbuilder@0.4.3
       +-- mime@1.2.11
       +-- node-uuid@1.4.3
       +-- validator@3.22.2
       +-- underscore@1.4.4
       +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
       +-- xml2js@0.2.7 (sax@0.5.2)
       +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
3. Du kan köra hello manuellt **ls** kommandot tooverify som en **nod\_moduler** mappen har skapats. I mappen hittar du hello **azure storage** paket som innehåller hello bibliotek som du behöver tooaccess lagring.

### <a name="import-hello-package"></a>Importera hello-paket
Lägg till följande kod toohello överkant hello hello **server.js** filen i ditt program:

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a>Skapa en Azure Storage-anslutning
hello azure modulen läser hello miljövariabler AZURE\_lagring\_konto och AZURE\_lagring\_åtkomst\_nyckel eller AZURE\_lagring\_anslutning \_Sträng för information som krävs för tooconnect tooyour Azure storage-konto. Om de här miljövariablerna inte har angetts måste du ange hello kontoinformation när du anropar **TableService**.

Ett exempel på inställningen hello miljövariabler i hello [Azure-portalen](https://portal.azure.com) en Azure-webbplats finns [Node.js web app med hello Azure Tabelltjänsten](../app-service-web/storage-nodejs-use-table-storage-web-site.md).

## <a name="create-a-table"></a>Skapa en tabell
hello följande kod skapar en **TableService** objekt och använder den toocreate en ny tabell. Lägg till följande hello hello övre delen av **server.js**.

```nodejs
var tableSvc = azure.createTableService();
```

Hej anrop för**createTableIfNotExists** skapa en ny tabell med hello angivna namnet om det inte redan finns. hello skapas följande exempel en ny tabell med namnet ”mytable” som prefix om det inte redan finns:

```nodejs
tableSvc.createTableIfNotExists('mytable', function(error, result, response){
  if(!error){
    // Table exists or created
  }
});
```

Hej `result.created` blir `true` om en ny tabell skapas och `false` om hello tabellen redan finns. Hej `response` innehåller information om hello-begäran.

### <a name="filters"></a>Filter
Valfria filtrering åtgärder kan vara tillämpade toooperations utförs med hjälp av **TableService**. Filtrering operations kan innehålla loggning, försöker automatiskt igen och så vidare. Filtren är objekt som implementerar en metod med hello signatur:

```nodejs
function handle (requestOptions, next)
```

När du har gjort dess förbearbetning på hello begäran alternativ måste hello-metoden toocall ”nästa” Skicka en motringning med hello följande signatur:

```nodejs
function (returnObject, finalCallback, next)
```

I den här motringning och efter bearbetning hello returnObject (hello svar från hello begäran toohello server), hello-återanrop måste tooeither anropa sedan om den finns toocontinue bearbetning filter eller helt enkelt anropa finalCallback annars tooend hello Service-anrop.

Två filter som implementerar logik som medföljer hello Azure SDK för Node.js, **ExponentialRetryPolicyFilter** och **LinearRetryPolicyFilter**. hello följande skapar en **TableService** objekt som använder hello **ExponentialRetryPolicyFilter**:

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var tableSvc = azure.createTableService().withFilter(retryOperations);
```

## <a name="add-an-entity-tooa-table"></a>Lägg till en entitet tooa tabell
tooadd en entitet först skapa ett objekt som definierar egenskaper för enhet. Alla enheter måste innehålla en **PartitionKey** och **RowKey**, som är unika identifierare för hello entitet.

* **PartitionKey** -avgör hello-partition som hello entitet lagras i
* **RowKey** - unikt identifierar hello entiteten inom hello partition

Båda **PartitionKey** och **RowKey** måste vara strängvärden. Mer information finns i [hello förstå tabelltjänst-datamodellen](http://msdn.microsoft.com/library/azure/dd179338.aspx).

hello följande är ett exempel för att definiera en entitet. Observera att **dueDate** definieras som en typ av **Edm.DateTime**. Att ange hello-typen är valfri och typer kommer att härleda om inget anges.

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'take out hello trash'},
  dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
};
```

> [!NOTE]
> Det finns också en **tidsstämpel** för varje post som anges av Azure när en entitet infogas eller uppdateras.
>
>

Du kan också använda hello **entityGenerator** toocreate entiteter. hello följande exempel skapar hello samma aktivitet entitet med hello **entityGenerator**.

```nodejs
var entGen = azure.TableUtilities.entityGenerator;
var task = {
  PartitionKey: entGen.String('hometasks'),
  RowKey: entGen.String('1'),
  description: entGen.String('take out hello trash'),
  dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
};
```

tooadd en entitet tooyour tabell skicka hello entitet objektet toohello **insertEntity** metod.

```nodejs
tableSvc.insertEntity('mytable',task, function (error, result, response) {
  if(!error){
    // Entity inserted
  }
});
```

Om hello åtgärden lyckas `result` innehåller hello [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) av hello infogad post och `response` innehåller information om hello igen.

Exempelsvar:

```nodejs
{ '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }
```

> [!NOTE]
> Som standard **insertEntity** returnerar inte hello infogas entiteten som en del av hello `response` information. Om du planerar att andra åtgärder pågår på den här entiteten eller vill toocache hello information, kan det vara användbart toohave returnerade som en del av hello `result`. Du kan göra detta genom att aktivera **echoContent** på följande sätt:
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`
>
>

## <a name="update-an-entity"></a>Uppdatera en entitet
Det finns flera metoder tillgängliga tooupdate en befintlig entitet:

* **replaceEntity** -uppdaterar en befintlig entitet genom att ersätta den
* **mergeEntity** -uppdaterar en befintlig entitet genom att använda nya egenskapsvärden i hello befintlig entitet
* **insertOrReplaceEntity** -uppdaterar en befintlig entitet genom att ersätta den. Om inga entiteten finns kommer en ny att infogas
* **insertOrMergeEntity** -uppdaterar en befintlig entitet genom att använda nya egenskapsvärden i hello befintliga. Om inga entiteten finns kommer en ny att infogas

hello exemplet nedan visar uppdaterar en entitet med **replaceEntity**:

```nodejs
tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
  if(!error) {
    // Entity updated
  }
});
```

> [!NOTE]
> Som standard kontrollerar uppdaterar en entitet inte toosee om hello data uppdateras tidigare har ändrats av en annan process. toosupport samtidiga uppdateringar:
>
> 1. Hämta hello ETag för hello-objekt som håller på att uppdateras. Det här felet returneras som en del av hello `response` för varje entitet-relaterade åtgärden och kan hämtas via `response['.metadata'].etag`.
> 2. När du utför en update-åtgärd på en entitet, Lägg till hämta information om hello ETag tidigare toohello ny entitet. Exempel:
>
>       entity2 [.metadata] .etag = currentEtag;
> 3. Utföra hello uppdateringen. Om hello entiteten har ändrats sedan du hämtade hello ETag-värde, till exempel en annan instans av programmet, en `error` returneras om inte uppfylldes hello update villkoret i hello-begäran.
>
>

Med **replaceEntity** och **mergeEntity**om hello entitet som ska uppdateras inte finns, hello update-åtgärden kommer att misslyckas. Om du inte vill toostore en entitet oavsett om den redan finns, använder du därför **insertOrReplaceEntity** eller **insertOrMergeEntity**.

Hej `result` för lyckade uppdatering operations innehåller hello **Etag** av hello uppdatera entiteten.

## <a name="work-with-groups-of-entities"></a>Arbeta med grupper av entiteter
Ibland blir meningsfullt toosubmit flera åtgärder tillsammans i en batch tooensure atomiska bearbetning av hello-servern. tooaccomplish som använder hello **TableBatch** klassen toocreate en batch och sedan använda hello **executeBatch** metod för **TableService** tooperform hello batchar åtgärder.

 hello som följande exempel visar hur du skickar två entiteter i en batch:

```nodejs
var task1 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'Take out hello trash'},
  dueDate: {'_':new Date(2015, 6, 20)}
};
var task2 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '2'},
  description: {'_':'Wash hello dishes'},
  dueDate: {'_':new Date(2015, 6, 20)}
};

var batch = new azure.TableBatch();

batch.insertEntity(task1, {echoContent: true});
batch.insertEntity(task2, {echoContent: true});

tableSvc.executeBatch('mytable', batch, function (error, result, response) {
  if(!error) {
    // Batch completed
  }
});
```

För lyckad batchåtgärder `result` innehåller information för varje åtgärd i hello batch.

### <a name="work-with-batched-operations"></a>Arbeta med batch-åtgärder
Åtgärder för att lägga till tooa batch kan kontrolleras genom att visa hello `operations` egenskapen. Du kan också använda hello följande metoder toowork med åtgärder:

* **Rensa** -rensar alla åtgärder från en grupp
* **getOperations** -hämtar en åtgärd från hello batch
* **hasOperations** -returnerar true om hello batch innehåller åtgärder
* **removeOperations** -tar bort en åtgärd
* **storlek** -returnerar hello antalet åtgärder i hello batch

## <a name="retrieve-an-entity-by-key"></a>Hämta en entitet med nyckeln
en specifik enhet baserat på hello tooreturn **PartitionKey** och **RowKey**, använda hello **retrieveEntity** metod.

```nodejs
tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
  if(!error){
    // result contains hello entity
  }
});
```

När den här åtgärden är klar, `result` innehåller hello entitet.

## <a name="query-a-set-of-entities"></a>Fråga en uppsättning enheter
tooquery en tabell, använder hello **TableQuery** objekt toobuild upp ett frågeuttryck med hello följande villkor:

* **Välj** -hello fält toobe har returnerats från hello fråga
* **där** - hello där satsen

  * **och** – en `and` where-villkor
  * **eller** – en `or` where-villkor
* **TOP** -hello antalet objekt toofetch

hello skapas följande exempel en fråga som returnerar hello översta fem objekt med en PartitionKey 'hometasks'.

```nodejs
var query = new azure.TableQuery()
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

Eftersom **Välj** inte används, alla fält som ska returneras. tooperform hello frågan mot en tabell, Använd **queryEntities**. hello används följande exempel den här frågan tooreturn entiteter från ”mytable” som prefix.

```nodejs
tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
  if(!error) {
    // query was successful
  }
});
```

Om detta lyckas `result.entries` innehåller en matris med entiteter som matchar hello fråga. Om hello frågan var tooreturn alla entiteter `result.continuationToken` kommer att vara icke -*null* och kan användas som hello tredje parametern för **queryEntities** tooretrieve resultat. Hello första fråga, Använd *null* för hello tredje parameter.

### <a name="query-a-subset-of-entity-properties"></a>Fråga en deluppsättning entitetsegenskaper
En tooa frågetabellen kan hämta bara några fält från en entitet.
Detta minskar bandbredden och kan förbättra frågeprestanda, särskilt för stora entiteter. Använd hello **Välj** -satsen och pass hello namnen på hello fält toobe returneras. Till exempel hello följande fråga returnerar endast hello **beskrivning** och **dueDate** fält.

```nodejs
var query = new azure.TableQuery()
  .select(['description', 'dueDate'])
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

## <a name="delete-an-entity"></a>Ta bort en entitet
Du kan ta bort en entitet med dess partition och raden nycklar. I det här exemplet hello **task1** objektet innehåller hello **RowKey** och **PartitionKey** värdena för hello entiteten toobe tas bort. Sedan hello objektet skickas toohello **deleteEntity** metod.

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'}
};

tableSvc.deleteEntity('mytable', task, function(error, response){
  if(!error) {
    // Entity deleted
  }
});
```

> [!NOTE]
> Överväg att använda ETags när du tar bort objekt, tooensure hello objektet inte har ändrats av en annan process. Se [uppdatera en entitet](#update-an-entity) information om hur du använder ETags.
>
>

## <a name="delete-a-table"></a>Ta bort en tabell
följande kod hello tar bort en tabell från ett lagringskonto.

```nodejs
tableSvc.deleteTable('mytable', function(error, response){
    if(!error){
        // Table deleted
    }
});
```

Om du är osäker på om det finns hello tabell använder **deleteTableIfExists**.

## <a name="use-continuation-tokens"></a>Använd fortsättning token
När du frågar tabeller för stora mängder resultat, leta efter fortsättning token. Det kan finnas stora mängder data tillgängliga för din fråga att du inte kanske vet om du inte skapa toorecognize när en fortsättningstoken är närvarande.

hello resultat objekt returnerades vid fråga entiteter anger en `continuationToken` egenskapen när dessa token finns. Du kan sedan använda den när du utför en fråga toocontinue toomove över hello partition och tabellen entiteter.

När du frågar, kan en continuationToken-parameter tillhandahållas mellan hello frågan objektinstansen och hello Återanropsfunktionen:

```nodejs
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

Om du inspektera hello `continuationToken` objekt, hittar du egenskaper som `nextPartitionKey`, `nextRowKey` och `targetLocation`, vilket kan vara används tooiterate via alla hello resultat.

Det finns också ett exempel på en fortsättning inom hello Azure Storage Node.js lagringsplatsen på GitHub. Leta efter `examples/samples/continuationsample.js`.

## <a name="work-with-shared-access-signatures"></a>Arbeta med signaturer för delad åtkomst
Signaturer för delad åtkomst (SAS) är ett säkert sätt tooprovide detaljerade åtkomst tootables utan att erbjuda dina lagringskontonamn eller nycklar. Säkerhetsassociationer är ofta använda tooprovide begränsad åtkomst tooyour data, till exempel att tillåta att en mobil app tooquery poster.

En betrodda program, till exempel en molnbaserad tjänst genererar en SAS med hello **generateSharedAccessSignature** av hello **TableService**, och ger den tooan inte är betrodd eller delvis betrodda program till exempel en mobil app. hello SAS genereras med en princip som beskriver hello start- och slutdatum under vilka hello SAS är giltigt, samt hello åtkomst nivån beviljade toohello SAS innehavaren.

hello följande exempel skapar en ny princip för delad åtkomst som gör att hello SAS innehavaren tooquery (r) hello tabell och upphör att gälla 100 minuter efter hello när det skapas.

```nodejs
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
    Start: startDate,
    Expiry: expiryDate
  },
};

var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
var host = tableSvc.host;
```

Observera att information om hello värden måste anges också som krävs när hello SAS innehavaren försöker tooaccess hello tabell.

Hej klientprogrammet och sedan använder hello SAS med **TableServiceWithSAS** tooperform åtgärder mot hello tabell. följande exempel hello ansluter toohello tabell och utför en fråga.

```nodejs
var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
var query = azure.TableQuery()
  .where('PartitionKey eq ?', 'hometasks');

sharedTableService.queryEntities(query, null, function(error, result, response) {
  if(!error) {
    // result contains hello entities
  }
});
```

Eftersom hello SAS genererades med endast fråga åtkomst, om ett försök gjordes tooinsert, uppdatera eller ta bort entiteter, returneras ett fel.

### <a name="access-control-lists"></a>Åtkomstkontrollistor
Du kan också använda en åtkomstkontrollista (ACL) tooset hello åtkomstprincip för en SAS. Detta är användbart om du vill tooallow flera klienter tooaccess hello tabell, men ange olika principer för varje klient.

En ACL implementeras med hjälp av en matris med principer för åtkomst med ett ID som är associerade med varje princip. hello följande exempel definierar två principer, en för 'Användare1' och en för 'användare2':

```nodejs
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

följande exempel hämtar hello hello aktuella ACL för hello **hometasks** tabell och lägger sedan till hello nya principer med hjälp av **setTableAcl**. Den här metoden kan:

```nodejs
var extend = require('extend');
tableSvc.getTableAcl('hometasks', function(error, result, response) {
if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    tableSvc.setTableAcl('hometasks', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

En gång hello ACL har ställts in, du kan sedan skapa en SAS baserat på hello-ID för en princip. hello följande exempel skapas en ny SAS för 'användare2':

```nodejs
tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });
```

## <a name="next-steps"></a>Nästa steg
Mer information finns i följande resurser hello.

* [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en gratis, fristående app från Microsoft som möjliggör toowork visuellt med Azure Storage-data i Windows, macOS och Linux.
* [Azure Storage SDK: N för noden](https://github.com/Azure/azure-storage-node) databasen på GitHub.
* [Node.js Developer Center](/develop/nodejs/)
* [Skapa och distribuera en Node.js-programmet tooan Azure-webbplats](../app-service-web/app-service-web-get-started-nodejs.md)
