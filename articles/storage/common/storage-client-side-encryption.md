---
title: "Kryptering av aaaClient på klientsidan med .NET för Microsoft Azure Storage | Microsoft Docs"
description: "hello Azure Storage-klientbibliotek för .NET stöder kryptering på klientsidan och integrering med Azure Key Vault för maximal säkerhet för dina Azure Storage-program."
services: storage
documentationcenter: .net
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: becfccca-510a-479e-a798-2044becd9a64
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: c09246f43801e17aff96ea453182d11ffbf5e420
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="client-side-encryption-and-azure-key-vault-for-microsoft-azure-storage"></a>Kryptering på klientsidan och Azure Key Vault för Microsoft Azure Storage
[!INCLUDE [storage-selector-client-side-encryption-include](../../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Översikt
Hej [Azure Storage-klientbibliotek för .NET-Nuget-paketet](https://www.nuget.org/packages/WindowsAzure.Storage) har stöd för kryptering av data i klientprogram före överföringen tooAzure lagring och dekryptera data vid hämtning av toohello klienten. hello biblioteket stöder även integrering med [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) storage-konto för hantering av nycklar.

En stegvis självstudiekurs som leder dig genom processen hello kryptera blobar med hjälp av kryptering på klientsidan och Azure Key Vault finns [kryptera och dekryptera blobbar i Microsoft Azure Storage med hjälp av Azure Key Vault](../blobs/storage-encrypt-decrypt-blobs-key-vault.md).

Kryptering på klientsidan med Java, se [klientsidan kryptering med Java för Microsoft Azure Storage](storage-client-side-encryption-java.md).

## <a name="encryption-and-decryption-via-hello-envelope-technique"></a>Kryptering och dekryptering via hello kuvert teknik
hello processer för kryptering och dekryptering Följ hello kuvert tekniken.

### <a name="encryption-via-hello-envelope-technique"></a>Kryptering via hello kuvert teknik
Kryptering via hello kuvert tekniken fungerar hello följande sätt:

1. hello Azure storage-klientbibliotek genererar en innehåll krypteringsnyckel (CEK), vilket är en gång Använd symmetrisk nyckel.
2. Användardata krypteras med den här CEK.
3. Hej kapslas CEK sedan (krypterade) med hjälp av krypteringsnyckeln för hello nyckel (KEK). Hej KEK identifieras med en nyckelidentifierare och kan vara ett asymmetriskt nyckelpar eller en symmetrisk nyckel och kan hanteras lokalt eller lagras i Azure Key valv.
   
    hello storage-klientbibliotek själva har aldrig åtkomst tooKEK. hello biblioteket anropar hello viktiga radbrytning algoritmen som tillhandahålls av Key Vault. Användarna kan välja toouse anpassade providers för viktiga radbrytning/uppackning om så önskas.

4. hello krypterade data har sedan överföra toohello Azure Storage-tjänst. hello omsluten nyckel tillsammans med vissa ytterligare krypteringsmetadata lagras som metadata (på en blob) eller interpolerade med hello krypterade data (Kömeddelanden och tabellentiteter).

### <a name="decryption-via-hello-envelope-technique"></a>Dekryptering via hello kuvert teknik
Dekryptering via hello kuvert tekniken fungerar hello följande sätt:

1. hello klientbiblioteket förutsätter hello användaren hanterar hello nyckelkryptering nyckel (KEK) lokalt eller i Azure Key valv. hello användaren behöver inte tooknow hello viss nyckel som används för kryptering. I stället kan en nyckel matchare som löser olika nyckelidentifierare tookeys ställa in och användas.
2. hello klientbiblioteket hämtar hello krypterade data tillsammans med kryptering material som är lagrad på hello-tjänsten.
3. hello omsluten innehåll krypteringsnyckel (CEK) är okrypterad (dekrypterade) med hjälp av hello nyckelkryptering nyckel (KEK). Här igen, har hello klientbiblioteket inte åtkomst till tooKEK. Den anropar bara hello anpassade eller borttagning av algoritmen för Key Vault-providern.
4. hello innehåll (CEK) kan sedan använda toodecrypt hello krypterade användardata.

## <a name="encryption-mechanism"></a>Krypteringsalgoritm
hello storage-klientbiblioteket använder [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) i ordning tooencrypt användardata. Mer specifikt [Cipher Block Chaining (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) läge med AES. Varje tjänst fungerar olika, så vi upp dem här.

### <a name="blobs"></a>Blobar
hello klientbiblioteket stöder för närvarande kryptering av hela blobbar. Mer specifikt kryptering stöds när användare använder hello **UploadFrom*** metoder eller hello **OpenWrite** metod. För hämtade filer, både fullständig och intervallet hämtningsbara filer stöds.

Under krypteringen hello klientbiblioteket Generera en slumpmässig initieringen Vector (IV) av 16 byte, tillsammans med en slumpmässig innehåll krypteringsnyckel (CEK) 32 byte och utföra kuvert kryptering av hello blob-data med den här informationen. hello omsluten CEK och vissa ytterligare krypteringsmetadata och sedan lagras som blob metadata tillsammans med hello krypterade blob på hello-tjänsten.

> [!WARNING]
> Om du redigerar eller överför din egen metadata för hello blob måste tooensure att dessa metadata bevaras. Om du överför nya metadata utan metadata hello omsluten CEK, IV och andra metadata går förlorade och hello blob-innehåll aldrig kommer att hämtas igen.
> 
> 

Hämtar en krypterad blob omfattar hämtar hello innehåll för hello hela blob med hello **DownloadTo***/**BlobReadStream** bekvämlighet -metoderna. hello omsluten CEK oförpackade och användas tillsammans med hello IV (som lagras som blobbmetadata i det här fallet) tooreturn hello dekrypteras data toohello användare.

Hämta ett godtyckligt intervall (**DownloadRange*** metoder) i hello krypterade blob innebär att justera hello intervallet anges av användare i ordning tooget en liten mängd ytterligare data som kan använda toosuccessfully dekryptera hello begärt intervall.

Alla blob-typer (blockblobbar, sidblobbar och tilläggsblobbar) kan krypterade/dekrypteras med hjälp av det här schemat.

### <a name="queues"></a>Köer
Eftersom Kömeddelanden kan vara i valfritt format, definierar ett anpassat format som innehåller hello initieringen Vector (IV) och hello krypterat innehåll krypteringsnyckel (CEK) i hello meddelandetext hello klientbiblioteket.

Under krypteringen hello klientbiblioteket genererar en slumpmässig IV av 16 byte tillsammans med en slumpmässig CEK 32 byte och utför kuvert kryptering av meddelandetexten för hello kön med den här informationen. hello omsluten CEK och vissa ytterligare krypteringsmetadata läggs sedan till toohello krypterade kömeddelande. Ändrade meddelandet (se nedan) lagras på hello-tjänsten.

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

Under dekrypteringen, hello omsluten nyckeln hämtas från kön hälsningsmeddelande och oförpackade. hello IV är också extraheras från kön hälsningsmeddelande och används tillsammans med hello oförpackade viktiga toodecrypt hello kön meddelandedata. Observera att hello krypteringsmetadata är liten (under 500 byte), så medan den räknas mot hello 64KB-gränsen för ett kömeddelande hello påverkan ska hanteras.

### <a name="tables"></a>Tabeller
Hej klienten biblioteket har stöd för kryptering av Entitetsegenskaper för insert och ersätt-åtgärder.

> [!NOTE]
> Koppling stöds inte för närvarande. Eftersom en delmängd av egenskaper kan har krypterats tidigare med hjälp av en annan nyckel leder bara sammanslagning hello nya egenskaper och uppdaterar hello metadata till dataförlust. Sammanslagning av antingen kräver att göra extra service anropar tooread hello befintlig entitet från hello-tjänsten eller med en ny nyckel för varje egenskap som lämpar sig inte av prestandaskäl.
> 
> 

Tabell datakryptering fungerar på följande sätt:  

1. Användare ange hello egenskaper toobe krypteras.
2. hello klientbiblioteket genererar en slumpmässig initieringen Vector (IV) av 16 byte tillsammans med en slumpmässig innehåll krypteringsnyckel (CEK) 32 byte för varje entitet och utför kuvert kryptering på hello enskilda egenskaper toobe krypterats med som en ny IV per Egenskapen. hello krypterad egenskap lagras som binära data.
3. hello omsluten CEK och vissa ytterligare krypteringsmetadata lagras sedan som ytterligare två reserverade egenskaper. hello första reserverade egenskapen (_ClientEncryptionMetadata1) är en strängegenskap som innehåller hello information om IV, version och omslutna nyckel. hello andra reserverade egenskapen (_ClientEncryptionMetadata2) är en binär egenskap som innehåller hello information om hello egenskaper som är krypterade. hello informationen i andra egenskapen (_ClientEncryptionMetadata2) är krypterad.
4. På grund av toothese ytterligare reserverade egenskaper som krävs för kryptering, har användare nu endast 250 anpassade egenskaper i stället för 252. hello total storlek på hello entitet måste vara mindre än 1 MB.

Observera att endast egenskaperna för anslutningssträngen kan vara krypterad. Om andra typer av egenskaper toobe krypterad, måste de vara konverterade toostrings. hello krypterade strängar lagras på hello-tjänsten som binära egenskaper och de konverteras tillbaka toostrings efter dekrypteringen.

För tabeller, dessutom toohello krypteringsprincipen användare måste ange hello egenskaper toobe krypteras. Detta kan göras genom att antingen att ange attributet [EncryptProperty] (för POCO entiteter som är härledda från TableEntity) eller en kryptering matcharen begäran alternativ. En kryptering Konfliktlösaren är en delegat som tar en partitionsnyckel, radnyckel och egenskapsnamn och returnerar ett booleskt värde som anger om egenskapen ska krypteras. Under krypteringen använder hello klientbiblioteket denna information toodecide om en egenskap som ska krypteras vid skrivning till toohello överföring. Det ger också hello ombud hello möjlighet till logik runt hur egenskaperna är krypterade. (Till exempel om X, sedan kryptera egenskapen A; annars kryptera egenskaper A och b) Observera att det är inte nödvändigt tooprovide denna information vid läsning eller fråga entiteter.

### <a name="batch-operations"></a>Batchåtgärder
I batch-åtgärder kommer hello samma KEK att används över alla hello rader i den batchåtgärd eftersom hello klientbiblioteket kan bara ett alternativ objekt (och därför en princip/KEK) per batchåtgärd. Dock genererar hello klientbiblioteket internt en ny slumpmässiga IV och slumpmässiga CEK per rad i hello batch. Användare kan också välja tooencrypt olika egenskaper för varje åtgärd i hello batch genom att definiera problemet i hello kryptering lösare.

### <a name="queries"></a>Frågor
tooperform fråga åtgärder, måste du ange en nyckel matchare som kan tooresolve alla hello nycklar i hello resultatuppsättningen. Om en entitet i frågeresultatet hello inte kan matchas tooa provider, genereras hello klientbiblioteket ett fel. För en fråga som utför serversidan projektioner hello klientbiblioteket lägger till hello särskilda kryptering metadataegenskaper (_ClientEncryptionMetadata1 och _ClientEncryptionMetadata2) som standard toohello valda kolumner.

## <a name="azure-key-vault"></a>Azure Key Vault
Azure Key Vault hjälper dig att skydda krypteringsnycklar och hemligheter som används av molnprogram och molntjänster. Med hjälp av Azure Key Vault kan kryptera användare nycklar och hemligheter (till exempel autentiseringsnycklar, lagringskontonycklar, datakrypteringsnycklar. PFX-filer och lösenord) med hjälp av nycklar som skyddas av maskinvarusäkerhetsmoduler (HSM). Mer information finns i [vad är Azure Key Vault?](../../key-vault/key-vault-whatis.md).

hello storage-klientbiblioteket använder hello Key Vault-Kärnbibliotek i ordning tooprovide ett gemensamt ramverk i Azure för hantering av nycklar. Användare kan också få hello fördelen med hello Key Vault-tillägg-biblioteket. hello tillägg biblioteket innehåller användbara funktioner runt enkel och sömlös Symmetric/RSA lokala och viktiga molntjänstleverantörer samt med aggregering och cachelagring.

### <a name="interface-and-dependencies"></a>Gränssnittet och beroenden
Det finns tre Key Vault-paket:

* Microsoft.Azure.KeyVault.Core innehåller hello IKey och IKeyResolver. Det är ett litet paket utan beroenden. hello storage-klientbibliotek för .NET definierar det som ett beroende.
* Microsoft.Azure.KeyVault innehåller hello Key Vault REST-klient.
* Microsoft.Azure.KeyVault.Extensions innehåller tilläggskod som innehåller implementeringar av kryptografiska algoritmer och en RSAKey och en SymmetricKey. Det beror på hello kärnor och KeyVault namnområden och ger funktioner toodefine en sammanställd matcharen (om användarna vill toouse flera viktiga providers) och en cachelagring viktiga matchare. Även om hello storage-klientbibliotek inte direkt beror på det här paketet om användaren vill toouse Azure Key Vault toostore sina nycklar eller toouse hello Key Vault tillägg tooconsume hello lokala och molnet kryptografiproviders, måste de det här paketet.

Key Vault är utformat för högt värde huvudnycklar och bandbreddsbegränsning gränser för Key Vault är utformad med detta i åtanke. När du utför kryptering på klientsidan med Key Vault är hello önskade modellen toouse symmetriska huvudnycklar lagras som hemligheter i Nyckelvalvet och cachelagras lokalt. Användarna måste göra hello följande:

1. Skapa en hemlighet offline och överför den tooKey valvet.
2. Använd hello hemlighet grundläggande identifierare som en parameter tooresolve hello nuvarande version av hello hemligheten för kryptering och cachelagra informationen lokalt. Använd CachingKeyResolver för cachelagring; användare har inte förväntad tooimplement sina egna cachelagring logik.
3. Använd hello cachelagring matcharen som indata när du skapar hello krypteringsprincipen.

Mer information om användning av Key Vault finns i hello [kryptering kodexempel](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples).

## <a name="best-practices"></a>Bästa praxis
Stöd för kryptering är endast tillgänglig i hello storage-klientbibliotek för .NET. Windows Phone och Windows Runtime stöder inte kryptering för närvarande.

> [!IMPORTANT]
> Tänk på dessa viktiga punkter när du använder kryptering på klientsidan:
> 
> * När läsning från eller skriva tooan krypterad blob, Använd hela blob överför kommandon och intervall/hela blob download kommandon. Undvik att skriva tooan krypterade blob med hjälp av protokollåtgärder som till exempel placera Block, placera Blockeringslista, skriva sidor, rensa sidor eller lägga till Block. Annars kan du skadad hello krypterade blob och att den inte kan läsas.
> * För tabeller finns en liknande begränsning. Vara försiktig toonot uppdatera krypterade egenskaper utan att uppdatera metadata för hello-kryptering.
> * Om du anger metadata för hello krypterade blob, kan du skriva över hello krypteringsrelaterade metadata som krävs för dekryptering, eftersom inställningen metadata inte är additiva. Detta gäller även för ögonblicksbilder; Undvik att ange metadata när du skapar en ögonblicksbild av en krypterad blob. Om du måste ange metadata, vara säker på att toocall hello **FetchAttributes** metoden första tooget hello aktuella krypteringsmetadata och undvika samtidiga skrivningar metadata anges.
> * Aktivera hello **RequireEncryption** egenskap i hello begäran standardalternativen för användare som ska fungerar bara med krypterade data. Mer information finns nedan.
> 
> 

## <a name="client-api--interface"></a>Programmeringsgränssnitt mot klienten / gränssnitt
När du skapar en EncryptionPolicy objekt, kan användare ange endast en nyckel (implementera IKey), bara en matchare (implementera IKeyResolver) eller båda. IKey är hello grundläggande nyckeltyp som identifieras med en nyckelidentifierare och som ger hello logik för radbrytning/uppackning. IKeyResolver är används tooresolve en nyckel under hello dekrypteringsprocess. Den definierar en ResolveKey-metod som returnerar ett IKey ges en nyckelidentifierare. Detta ger användarna hello möjlighet toochoose mellan flera nycklar som hanteras på flera platser.

* Hello-nyckeln används alltid för kryptering, och hello frånvaron av en nyckel resulterar i ett fel.
* För dekryptering:
  * hello viktiga matcharen anropas om tooget hello nyckel har angetts. Om hello matcharen har angetts men inte har en mappning för hello nyckelidentifierare, returneras ett fel.
  * Om DNS-matchare inte har angetts, men en nyckel har angetts, används hello nyckeln om dess identifierare matchar nyckelidentifierare hello krävs. Ett fel returneras om hello-ID inte matchar.

Hej [kryptering prover](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples) demonstrera en mer detaljerad slutpunkt till slutpunkt för BLOB, köer och tabeller, tillsammans med Key Vault-integrering.

### <a name="requireencryption-mode"></a>RequireEncryption läge
Användare kan välja att ett läge där alla överföringar och hämtningsbara filer måste vara krypterad. I det här läget misslyckas försök tooupload data utan en kryptering principen eller hämta data som inte är krypterad på hello-tjänsten på hello-klienten. Hej **RequireEncryption** hello Begäranobjektet alternativ bestämmer det här beteendet. Om ditt program kommer att kryptera alla objekt som lagras i Azure Storage, så du kan ange hello **RequireEncryption** egenskapen hello begäran standardalternativ för hello serviceobjektet för klienten. Till exempel **CloudBlobClient.DefaultRequestOptions.RequireEncryption** för**SANT** toorequire kryptering för alla blob åtgärder utföras genom att klientobjekt.

### <a name="blob-service-encryption"></a>Kryptering för BLOB-tjänst
Skapa en **BlobEncryptionPolicy** objekt och anger det i alternativen för hello (per API eller på en klientnivå med hjälp av **DefaultRequestOptions**). Allt annat ska hanteras av hello klientbiblioteket internt.

```csharp
// Create hello IKey used for encryption.
 RsaKey key = new RsaKey("private:key1" /* key identifier */);

 // Create hello encryption policy toobe used for upload and download.
 BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

 // Set hello encryption policy on hello request options.
 BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

 // Upload hello encrypted contents toohello blob.
 blob.UploadFromStream(stream, size, null, options, null);

 // Download and decrypt hello encrypted contents from hello blob.
 MemoryStream outputStream = new MemoryStream();
 blob.DownloadToStream(outputStream, null, options, null);
```

### <a name="queue-service-encryption"></a>Kön service-kryptering
Skapa en **QueueEncryptionPolicy** objekt och anger det i alternativen för hello (per API eller på en klientnivå med hjälp av **DefaultRequestOptions**). Allt annat ska hanteras av hello klientbiblioteket internt.

```csharp
// Create hello IKey used for encryption.
 RsaKey key = new RsaKey("private:key1" /* key identifier */);

 // Create hello encryption policy toobe used for upload and download.
 QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

 // Add message
 QueueRequestOptions options = new QueueRequestOptions() { EncryptionPolicy = policy };
 queue.AddMessage(message, null, null, options, null);

 // Retrieve message
 CloudQueueMessage retrMessage = queue.GetMessage(null, options, null);
```

### <a name="table-service-encryption"></a>Tabellen service-kryptering
Dessutom toocreating en krypteringsprincip för och ange värdet på begäran alternativ, måste du antingen ange en **EncryptionResolver** i **TableRequestOptions**, eller ange hello [EncryptProperty]-attribut på hello entitet.

#### <a name="using-hello-resolver"></a>Hello resolvern

```csharp
// Create hello IKey used for encryption.
 RsaKey key = new RsaKey("private:key1" /* key identifier */);

 // Create hello encryption policy toobe used for upload and download.
 TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);

 TableRequestOptions options = new TableRequestOptions()
 {
    EncryptionResolver = (pk, rk, propName) =>
     {
        if (propName == "foo")
         {
            return true;
         }
         return false;
     },
     EncryptionPolicy = policy
 };

 // Insert Entity
 currentTable.Execute(TableOperation.Insert(ent), options, null);

 // Retrieve Entity
 // No need toospecify an encryption resolver for retrieve
 TableRequestOptions retrieveOptions = new TableRequestOptions()
 {
    EncryptionPolicy = policy
 };

 TableOperation operation = TableOperation.Retrieve(ent.PartitionKey, ent.RowKey);
 TableResult result = currentTable.Execute(operation, retrieveOptions, null);
```

#### <a name="using-attributes"></a>Använda attribut
Som nämnts ovan, om hello entitet implementerar TableEntity sedan hello egenskaper kan vara dekorerad med hello [EncryptProperty]-attributet i stället för att ange hello **EncryptionResolver**.

```csharp
[EncryptProperty]
 public string EncryptedProperty1 { get; set; }
```

## <a name="encryption-and-performance"></a>Kryptering och prestanda
Observera att kryptera din lagring data resulterar i ytterligare prestanda försämras. hello innehåll nyckel och IV måste skapas själva hello innehållet måste krypteras och ytterligare metadata måste vara formaterad och har överförts. Det här arbetet varierar beroende på hello mängden data som krypteras. Vi rekommenderar att kunder alltid testa sina program för prestanda under utveckling.

## <a name="next-steps"></a>Nästa steg
* [Självstudier: Kryptera och dekryptera blobbar i Microsoft Azure Storage med hjälp av Azure Key Vault](../blobs/storage-encrypt-decrypt-blobs-key-vault.md)
* Hämta hello [Azure Storage-klientbibliotek för .NET-NuGet-paketet](https://www.nuget.org/packages/WindowsAzure.Storage)
* Hämta hello Azure Key Vault NuGet [Core](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core/), [klienten](http://www.nuget.org/packages/Microsoft.Azure.KeyVault/), och [tillägg](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions/) paket  
* Besök hello [Azure Key Vault-dokumentationen](../../key-vault/key-vault-whatis.md)
