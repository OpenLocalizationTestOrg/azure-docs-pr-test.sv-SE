---
title: "aaaHow toouse Blob storage från Node.js | Microsoft Docs"
description: Lagra Ostrukturerade data i hello moln med Azure Blob storage (objektlagring).
services: storage
documentationcenter: nodejs
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 8b0df222-1ca8-4967-8248-6d6d720947b8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: 572e7fc9f7b19ff01720a7cadd495c809ed49fb2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-nodejs"></a>Hur toouse Blob storage från Node.js
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a>Översikt
Den här artikeln beskrivs hur du tooperform vanliga scenarier med Blob storage. hello exempel skrivs via hello Node.js API. hello-scenarier som tas upp är hur tooupload, lista, hämta och ta bort blobbar.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Skapa ett Node.js-program
Anvisningar för hur toocreate ett Node.js-program finns i [skapa en Node.js-webbapp i Azure App Service], [skapa och distribuera en Node.js-programmet tooan Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) --med Windows PowerShell eller [bygga och distribuera en Node.js web app tooAzure med hjälp av Web Matrix](https://www.microsoft.com/web/webmatrix/).

## <a name="configure-your-application-tooaccess-storage"></a>Konfigurera programmet tooaccess lagringen
toouse Azure storage, behöver du hello Azure Storage SDK för Node.js som innehåller en uppsättning bekvämlighet bibliotek som kommunicerar med hello storage REST-tjänster.

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a>Använd noden Package Manager (NPM) tooobtain hello-paket
1. Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac), eller **Bash** (Unix) toonavigate toohello mappen där du skapade ditt exempel programmet.
2. Typen **npm installera azure-lagring** i hello kommandofönstret. Utdata från kommandot hello är liknande toohello följande kodexempel.

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
3. Du kan köra hello manuellt **ls** kommandot tooverify som en **nod\_moduler** mappen har skapats. Hitta hello inuti den mappen **azure storage** paket som innehåller hello bibliotek som du behöver tooaccess lagring.

### <a name="import-hello-package"></a>Importera hello-paket
Använda anteckningar eller något annat textredigeringsprogram Lägg till följande toohello överkant hello hello **server.js** -filen för programmet hello där du ska toouse lagring:

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a>Skapa en Azure Storage-anslutning
hello Azure-modulen läses hello miljövariabler `AZURE_STORAGE_ACCOUNT` och `AZURE_STORAGE_ACCESS_KEY`, eller `AZURE_STORAGE_CONNECTION_STRING`, information krävs tooconnect tooyour Azure storage-konto. Om de här miljövariablerna inte har angetts måste du ange hello kontoinformation när du anropar **createBlobService**.

Ett exempel på inställningen hello miljövariabler i hello [Azure-portalen](https://portal.azure.com) för en Azure webbapp, se [Node.js web app med hello Azure Tabelltjänsten](../../app-service-web/storage-nodejs-use-table-storage-web-site.md).

## <a name="create-a-container"></a>Skapa en behållare
Hej **BlobService** objekt kan du arbeta med behållare och blobbar. hello följande kod skapar en **BlobService** objekt. Lägg till följande hello hello övre delen av **server.js**:

```nodejs
var blobSvc = azure.createBlobService();
```

> [!NOTE]
> Du kan komma åt en blob anonymt med **createBlobServiceAnonymous** och ge hello värdadress. Till exempel använda `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.
>
>

[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

toocreate en ny behållare använder **createContainerIfNotExists**. hello skapar följande exempel en ny behållare med namnet 'minbehållare':

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
    if(!error){
      // Container exists and is private
    }
});
```

Om hello behållaren nyligen har skapat `result.created` är true. Om det redan finns hello behållaren `result.created` är false. `response`innehåller information om hello operationen, inklusive hello ETag-information för hello behållare.

### <a name="container-security"></a>Behållaren säkerhet
Som standard nya behållare är privata och går inte att ansluta anonymt. toomake hello behållare offentliga så att du kan komma åt den anonymt, du kan ange åtkomstnivån hello behållare för**blob** eller **behållare**.

* **BLOB** -tillåter anonym läsbehörighet tooblob innehållet och metadata i den här behållaren, men inte toocontainer metadata, till exempel visar en lista över alla blobbar i en behållare
* **behållaren** -tillåter anonym läsbehörighet tooblob innehållet och metadata som metadata för behållaren

hello följande kodexempel visar inställningen hello åtkomstnivå för**blob**:

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
    if(!error){
      // Container exists and allows
      // anonymous read access tooblob
      // content and metadata within this container
    }
});
```

Du kan också ändra hello åtkomstnivån för en behållare med hjälp av **setContainerAcl** toospecify hello åtkomstnivå. hello följande kod exempel ändringar hello åtkomst nivå toocontainer:

```nodejs
blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
  if(!error){
    // Container access level set too'container'
  }
});
```

hello innehåller information om hello operationen, inklusive hello aktuella **ETag** för hello behållare.

### <a name="filters"></a>Filter
Du kan använda valfri filtrering operations toooperations utföras med hjälp av **BlobService**. Filtrering operations kan innehålla loggning, försöker automatiskt igen och så vidare. Filtren är objekt som implementerar en metod med hello signatur:

```nodejs
function handle (requestOptions, next)
```

När du har gjort dess förbearbetning på hello begäran alternativ måste hello-metoden toocall ”nästa” Skicka en motringning med hello följande signatur:

```nodejs
function (returnObject, finalCallback, next)
```

I den här motringning och efter bearbetning hello returnObject (hello svar från hello begäran toohello server), hello-återanrop måste tooeither anropa sedan om den finns toocontinue bearbetning filter eller helt enkelt anropa finalCallback tooend hello service anrop.

Två filter som implementerar logik som medföljer hello Azure SDK för Node.js, **ExponentialRetryPolicyFilter** och **LinearRetryPolicyFilter**. hello följande skapar en **BlobService** objekt som använder hello **ExponentialRetryPolicyFilter**:

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var blobSvc = azure.createBlobService().withFilter(retryOperations);
```

## <a name="upload-a-blob-into-a-container"></a>Ladda upp en blobb till en behållare
Det finns tre typer av blobbar: blockblobbar, sidblobbar och tilläggsblobbar. Blockblobbar kan du toomore effektivt överför stora mängder data. Lägg till blobbar som är optimerade för tilläggsåtgärder. Sidblobbar är optimerade för läs-/ skrivåtgärder. Mer information finns i [förstå Blockblobbar, Tilläggsblobbar och Sidblobbar](http://msdn.microsoft.com/library/azure/ee691964.aspx).

### <a name="block-blobs"></a>Blockblob-objekt
tooupload data tooa blockblob, Använd hello följande:

* **createBlockBlobFromLocalFile** – skapar en ny block-blob och överför hello innehållet i en fil
* **createBlockBlobFromStream** – skapar en ny block-blob och överför hello innehållet i en dataström
* **createBlockBlobFromText** – skapar en ny block-blob och överför hello innehållet i en sträng
* **createWriteStreamToBlockBlob** -ger en skrivning dataströmmen tooa block-blob

hello följande kodexempel överför hello innehållet i hello **test.txt** filen till **minblobb**.

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

Hej `result` returneras av dessa metoder innehåller information om hello-åtgärden, till exempel hello **ETag** av hello-blob.

### <a name="append-blobs"></a>Tilläggsblobbar
tooupload data tooa nya bifoga blob, använder hello följande:

* **createAppendBlobFromLocalFile** – skapar en ny tilläggsblobb och överför hello innehållet i en fil
* **createAppendBlobFromStream** – skapar en ny tilläggsblobb och överför hello innehållet i en dataström
* **createAppendBlobFromText** – skapar en ny tilläggsblobb och överför hello innehållet i en sträng
* **createWriteStreamToNewAppendBlob** – skapar en ny tilläggsblobb och ger en dataström toowrite tooit

hello följande kodexempel överför hello innehållet i hello **test.txt** filen till **myappendblob**.

```nodejs
blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

tooappend ett block tooan befintliga Lägg blob, Använd hello följande:

* **appendFromLocalFile** -Lägg till hello innehållet i en fil tooan befintliga bifoga blob
* **appendFromStream** -Lägg till hello innehållet för en dataström tooan befintliga bifoga blob
* **appendFromText** -Lägg till hello innehållet i en sträng tooan befintliga bifoga blob
* **appendBlockFromStream** -Lägg till hello innehållet för en dataström tooan befintliga bifoga blob
* **appendBlockFromText** -Lägg till hello innehållet i en sträng tooan befintliga bifoga blob

> [!NOTE]
> appendFromXXX API: er kommer att göra vissa klientsidan validering toofail snabb tooavoid onödiga server-anrop. appendBlockFromXXX inte.
>
>

hello följande kodexempel överför hello innehållet i hello **test.txt** filen till **myappendblob**.

```nodejs
blobSvc.appendFromText('mycontainer', 'myappendblob', 'text toobe appended', function(error, result, response){
  if(!error){
    // text appended
  }
});
```

### <a name="page-blobs"></a>Sidblobbar
tooupload data tooa sidblob, Använd hello följande:

* **createPageBlob** -skapar en ny sidblob för en viss längd
* **createPageBlobFromLocalFile** – skapar en ny sidblob och överför hello innehållet i en fil
* **createPageBlobFromStream** – skapar en ny sidblob och överför hello innehållet i en dataström
* **createWriteStreamToExistingPageBlob** -ger en skrivning dataströmmen tooan befintliga sidblob
* **createWriteStreamToNewPageBlob** – skapar en ny sidblob och ger en dataström toowrite tooit

hello följande kodexempel överför hello innehållet i hello **test.txt** filen till **mypageblob**.

```nodejs
blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

> [!NOTE]
> Sidblobbar består av 512 byte sidor. Du får ett fel när överföra data med en storlek som inte är en multipel av 512.
>
>

## <a name="list-hello-blobs-in-a-container"></a>Lista hello blobbar i en behållare
toolist hello blobbar i en behållare använder hello **listBlobsSegmented** metod. Om du vill tooreturn blobbar med ett specifikt prefix använda **listBlobsSegmentedWithPrefix**.

```nodejs
blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
  if(!error){
      // result.entries contains hello entries
      // If not all blobs were returned, result.continuationToken has hello continuation token.
  }
});
```

Hej `result` innehåller en `entries` samling, som är en matris med objekt som beskriver varje blob. Om alla BLOB inte kan returneras, hello `result` innehåller också en `continuationToken`, som du kan använda som hello andra parameter tooretrieve ytterligare poster.

## <a name="download-blobs"></a>Ladda ned blobbar
toodownload data från blob använder hello följande:

* **getBlobToLocalFile** -skriver hello blob innehållet toofile
* **getBlobToStream** -skriver hello blob innehållet tooa dataström
* **getBlobToText** -skriver hello blobbinnehållet tooa strängen
* **createReadStream** -ger en dataström tooread från hello blob

hello följande kodexempel visas hur du använder **getBlobToStream** toodownload hello innehållet i hello **minblobb** blob och lagra den toohello **output.txt** filen med hjälp av en dataströmmen:

```nodejs
var fs = require('fs');
blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
  if(!error){
    // blob retrieved
  }
});
```

Hej `result` innehåller information om hello blob, inklusive **ETag** information.

## <a name="delete-a-blob"></a>Ta bort en blob
Slutligen toodelete blob anropa **deleteBlob**. Hej följande kodexempel borttagningar hello blob med namnet **minblobb**.

```nodejs
blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
  if(!error){
    // Blob has been deleted
  }
});
```

## <a name="concurrent-access"></a>Samtidig åtkomst
toosupport samtidig åtkomst tooa blob från flera klienter eller flera processinstanser, som du kan använda **ETags** eller **lån**.

* **ETag** -ger ett sätt toodetect som hello blob eller behållare har ändrats av en annan process
* **Lånet** – ger en sätt tooobtain exklusiv, förnyas, skrivning eller ta bort åtkomst tooa blob för en viss tidsperiod

### <a name="etag"></a>ETag
Använd ETags om du behöver tooallow flera klienter eller instanser toowrite toohello block Blob eller sidan Blob samtidigt. hello kan ETag du toodetermine om hello behållare eller blobb har ändrats sedan du först läsa eller skapades, vilket gör att du tooavoid skriver över ändringar som har genomförts av en annan klient eller process.

Du kan ange ETag villkor med hjälp av valfri hello `options.accessConditions` parameter. hello följande kodexempel endast överför hello **test.txt** -filen om hello blob som redan finns och har hello ETag-värde som innehåller `etagToMatch`.

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
    if(!error){
    // file uploaded
  }
});
```

När du använder ETags är hello allmänna mönster:

1. Hämta hello ETag hello följd av att skapa, listor eller hämta-åtgärden.
2. Utföra en åtgärd som kontrollerar den hello ETag-värde inte har ändrats.

Detta anger att en annan klient eller instans ändrats hello blob eller behållaren eftersom du har köpt hello ETag-värde om hello-värdet har ändrats.

### <a name="lease"></a>Lån
Du kan skaffa ett nytt lån med hjälp av hello **acquireLease** -metoden att ange hello blob eller behållare som du vill tooobtain ett lån på. Till exempel följande kod hello får ett lån på **minblobb**.

```nodejs
blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
  if(!error) {
    console.log('leaseId: ' + result.id);
  }
});
```

Efterföljande åtgärder på **minblobb** måste ange hello `options.leaseId` parameter. hello lån ID returneras som `result.id` från **acquireLease**.

> [!NOTE]
> Som standard är hello lånetid oändlig. Du kan ange en icke oändliga varaktigheten (mellan 15 och 60 sekunder) genom att tillhandahålla hello `options.leaseDuration` parameter.
>
>

Använd tooremove ett lån **releaseLease**. toobreak ett lån men förhindra andra från att erhålla ett nytt lån till hello ursprungliga varaktigheten har gått ut, Använd **breakLease**.

## <a name="work-with-shared-access-signatures"></a>Arbeta med signaturer för delad åtkomst
Signaturer för delad åtkomst (SAS) är ett säkert sätt tooprovide detaljerade åtkomst tooblobs och behållare utan att erbjuda dina lagringskontonamn eller nycklar. Signaturer för delad åtkomst är ofta använda tooprovide begränsad åtkomst tooyour data, till exempel att tillåta att en mobil app tooaccess blobbar.

> [!NOTE]
> Medan du kan också tillåta anonym åtkomst tooblobs, kan signaturer för delad åtkomst du tooprovide mer kontrollerad åtkomst som du måste skapa hello SAS.
>
>

En betrodda program, till exempel en molnbaserad tjänst genererar signaturer för delad åtkomst med hjälp av hello **generateSharedAccessSignature** av hello **BlobService**, och ger den tooan betrodd eller delvis betrodda program, till exempel en mobil app. Delad åtkomst signaturer genereras med hjälp av en princip som beskriver hello start- och slutdatum under vilka hello signaturer för delad åtkomst är giltiga samt hello åt nivån beviljade toohello delad åtkomst signaturer innehavaren.

hello följande kodexempel genererar en ny princip för delad åtkomst som tillåter hello delad åtkomst signaturer innehavaren tooperform läsåtgärder på hello **minblobb** blob och upphör att gälla 100 minuter efter hello när det skapas.

```nodejs
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
    Start: startDate,
    Expiry: expiryDate
  },
};

var blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', 'myblob', sharedAccessPolicy);
var host = blobSvc.host;
```

Observera att information om hello värden måste anges också som krävs när hello delad åtkomst signaturer innehavaren försöker tooaccess hello behållare.

hello klientprogrammet sedan använder signaturer för delad åtkomst med **BlobServiceWithSAS** tooperform åtgärder mot hello-blob. hello följande hämtar information **minblobb**.

```nodejs
var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
  if(!error) {
    // retrieved info
  }
});
```

Eftersom hello delade åtkomstsignaturer genererades med skrivskyddad åtkomst om ett försök görs toomodify hello blob, returneras ett fel.

### <a name="access-control-lists"></a>Listor för åtkomstkontroll
Du kan också använda en åtkomstkontrollista tooset hello åtkomst principer för åtkomstkontroll för SAS. Detta är användbart om du vill tooallow flera klienter tooaccess en behållare men ange olika principer för varje klient.

En ACL implementeras med hjälp av en matris med principer för åtkomst med ett ID som är associerade med varje princip. Följande kodexempel hello definierar två principer, en för 'Användare1' och en för 'användare2':

```nodejs
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.WRITE,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

följande kod exempel hämtar hello hello aktuella ACL för **minbehållare**, och lägger sedan till hello nya principer med hjälp av **setBlobAcl**. Den här metoden kan:

```nodejs
var extend = require('extend');
blobSvc.getBlobAcl('mycontainer', function(error, result, response) {
  if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    blobSvc.setBlobAcl('mycontainer', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

En gång hello ACL anges, du kan sedan skapa signaturer för delad åtkomst baserat på hello-ID för en princip. Följande kodexempel hello skapar nya signaturer för delad åtkomst för 'användare2':

```nodejs
blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });
```

## <a name="next-steps"></a>Nästa steg
Mer information finns i följande resurser hello.

* [Azure Storage SDK för noden API-referens] [Azure Storage SDK för noden API-referens]
* [Azure Storage-teamets blogg] [Azure Storage-teamets blogg]
* [Azure Storage SDK: N för noden] [ Azure Storage SDK for Node] databasen på GitHub
* [Node.js Developer Center](https://azure.microsoft.com/develop/nodejs/)
* [Överföra data med hello kommandoradsverktyget azcopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node

[Node.js-webbapp med hjälp av hello Azure Table-tjänsten](../../app-service-web/storage-nodejs-use-table-storage-web-site.md)    
[Skapa och distribuera en Node.js web app tooAzure med hjälp av Web Matrix]: https://www.microsoft.com/web/webmatrix/  
[Hello med hjälp av REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx [Azure portal]: https://portal.azure.com [skapa och distribuera en Node.js-programmet tooan Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) [Azure Storage-teamets blogg]: http:// blogs.msdn.com/b/windowsazurestorage/ [Azure Storage SDK för noden API-referens]: http://dl.windowsazure.com/nodestoragedocs/index.html
