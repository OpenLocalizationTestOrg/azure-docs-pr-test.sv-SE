---
title: "aaaClient-Side Encryption med Python för Microsoft Azure Storage | Microsoft Docs"
description: "hello Azure Storage-klientbibliotek för Python stöder kryptering på klientsidan för maximal säkerhet för dina Azure Storage-program."
services: storage
documentationcenter: python
author: lakasa
manager: jahogg
editor: tysonn
ms.assetid: f9bf7981-9948-4f83-8931-b15679a09b8a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/11/2017
ms.author: lakasa
ms.openlocfilehash: d2e943977322b97b777369508b957a1b2cbaa4e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="client-side-encryption-with-python-for-microsoft-azure-storage"></a>Klientsidans kryptering med Python för Microsoft Azure Storage
[!INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Översikt
Hej [Azure Storage-klientbibliotek för Python](https://pypi.python.org/pypi/azure-storage) har stöd för kryptering av data i klientprogram före överföringen tooAzure lagring och dekryptera data vid hämtning av toohello klienten.

> [!NOTE]
> hello Azure Storage Python-bibliotek finns i förhandsgranskningen.
> 
> 

## <a name="encryption-and-decryption-via-hello-envelope-technique"></a>Kryptering och dekryptering via hello kuvert teknik
hello processer för kryptering och dekryptering Följ hello kuvert tekniken.

### <a name="encryption-via-hello-envelope-technique"></a>Kryptering via hello kuvert teknik
Kryptering via hello kuvert tekniken fungerar hello följande sätt:

1. hello Azure storage-klientbibliotek genererar en innehåll krypteringsnyckel (CEK), vilket är en gång Använd symmetrisk nyckel.
2. Användardata krypteras med den här CEK.
3. Hej kapslas CEK sedan (krypterade) med hjälp av krypteringsnyckeln för hello nyckel (KEK). Hej KEK identifieras med en nyckelidentifierare och kan vara ett asymmetriskt nyckelpar eller en symmetrisk nyckel som hanteras lokalt.
   hello storage-klientbibliotek själva har aldrig åtkomst tooKEK. hello biblioteket anropar hello nyckeln wrapping algoritmen som tillhandahålls av hello KEK. Användarna kan välja toouse anpassade providers för viktiga radbrytning/uppackning om så önskas.
4. hello krypterade data har sedan överföra toohello Azure Storage-tjänst. hello omsluten nyckel tillsammans med vissa ytterligare krypteringsmetadata lagras som metadata (på en blob) eller interpolerade med hello krypterade data (Kömeddelanden och tabellentiteter).

### <a name="decryption-via-hello-envelope-technique"></a>Dekryptering via hello kuvert teknik
Dekryptering via hello kuvert tekniken fungerar hello följande sätt:

1. hello klientbiblioteket förutsätter hello användaren hanterar hello nyckelkryptering nyckel (KEK) lokalt. hello användaren behöver inte tooknow hello viss nyckel som används för kryptering. I stället kan en nyckel matchare som löser olika nyckelidentifierare tookeys ställa in och användas.
2. hello klientbiblioteket hämtar hello krypterade data tillsammans med kryptering material som är lagrad på hello-tjänsten.
3. hello omsluten innehåll krypteringsnyckel (CEK) är okrypterad (dekrypterade) med hjälp av hello nyckelkryptering nyckel (KEK). Här igen, har hello klientbiblioteket inte åtkomst till tooKEK. Den anropar bara hello anpassad provider borttagning av algoritmen.
4. hello innehåll (CEK) kan sedan använda toodecrypt hello krypterade användardata.

## <a name="encryption-mechanism"></a>Krypteringsalgoritm
hello storage-klientbiblioteket använder [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) i ordning tooencrypt användardata. Mer specifikt [Cipher Block Chaining (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) läge med AES. Varje tjänst fungerar olika, så vi upp dem här.

### <a name="blobs"></a>Blobar
hello klientbiblioteket stöder för närvarande kryptering av hela blobbar. Mer specifikt kryptering stöds när användare använder hello **skapa*** metoder. För hämtade filer, både fullständig och intervallet nedladdningar som stöds och parallellisering för både ladda upp och hämta är tillgänglig.

Under krypteringen hello klientbiblioteket Generera en slumpmässig initieringen Vector (IV) av 16 byte, tillsammans med en slumpmässig innehåll krypteringsnyckel (CEK) 32 byte och utföra kuvert kryptering av hello blob-data med den här informationen. hello omsluten CEK och vissa ytterligare krypteringsmetadata och sedan lagras som blob metadata tillsammans med hello krypterade blob på hello-tjänsten.

> [!WARNING]
> Om du redigerar eller överför din egen metadata för hello blob måste tooensure att dessa metadata bevaras. Om du överför nya metadata utan metadata hello omsluten CEK, IV och andra metadata går förlorade och hello blob-innehåll aldrig kommer att hämtas igen.
> 
> 

Hämtar en krypterad blob omfattar hämtar hello innehåll för hello hela blob med hello **hämta*** bekvämlighet metoder. hello omsluten CEK oförpackade och användas tillsammans med hello IV (som lagras som blobbmetadata i det här fallet) tooreturn hello dekrypteras data toohello användare.

Hämta ett godtyckligt intervall (**hämta*** metoder med intervallet parametrar skickades) i hello krypterade blob innebär att justera hello-intervall som tillhandahålls av användare i ordning tooget en liten mängd ytterligare data som kan användas toosuccessfully dekryptera hello begärt intervall.

Blockblobbar och sidblobbar kan bara vara krypterad/dekrypteras med hjälp av det här schemat. Det finns inget stöd för kryptering av tilläggsblobar.

### <a name="queues"></a>Köer
Eftersom Kömeddelanden kan vara i valfritt format, definierar ett anpassat format som innehåller hello initieringen Vector (IV) och hello krypterat innehåll krypteringsnyckel (CEK) i hello meddelandetext hello klientbiblioteket.

Under krypteringen hello klientbiblioteket genererar en slumpmässig IV av 16 byte tillsammans med en slumpmässig CEK 32 byte och utför kuvert kryptering av meddelandetexten för hello kön med den här informationen. hello omsluten CEK och vissa ytterligare krypteringsmetadata läggs sedan till toohello krypterade kömeddelande. Ändrade meddelandet (se nedan) lagras på hello-tjänsten.

```
<MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>
```

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
3. hello omsluten CEK och vissa ytterligare krypteringsmetadata lagras sedan som ytterligare två reserverade egenskaper. hello reserverade först egenskapen (\_ClientEncryptionMetadata1) är en strängegenskap som innehåller hello information om IV, version och omslutna nyckel. Hej andra reserverade egenskapen (\_ClientEncryptionMetadata2) är en binär egenskap som innehåller hello information om hello egenskaper som är krypterade. informationen i den här andra egenskapen hello (\_ClientEncryptionMetadata2) krypteras.
4. På grund av toothese ytterligare reserverade egenskaper som krävs för kryptering, har användare nu endast 250 anpassade egenskaper i stället för 252. hello total storlek på hello entitet måste vara mindre än 1MB.
   
   Observera att endast egenskaperna för anslutningssträngen kan vara krypterad. Om andra typer av egenskaper toobe krypterad, måste de vara konverterade toostrings. hello krypterade strängar lagras på hello-tjänsten som binära egenskaper och de konverteras tillbaka toostrings (rådata strängar, inte EntityProperties med typen EdmType.STRING) efter dekrypteringen.
   
   För tabeller, dessutom toohello krypteringsprincipen användare måste ange hello egenskaper toobe krypteras. Detta kan göras genom att antingen lagra dessa egenskaper i TableEntity-objekt med hello typen set tooEdmType.STRING och kryptera encryption_resolver_function för set-tootrue eller inställningen hello på hello tableservice objekt. En kryptering lösare är en funktion som tar en partitionsnyckel, radnyckel och egenskapsnamn och returnerar ett booleskt värde som anger om egenskapen ska krypteras. Under krypteringen använder hello klientbiblioteket denna information toodecide om en egenskap som ska krypteras vid skrivning till toohello överföring. Det ger också hello ombud hello möjlighet till logik runt hur egenskaperna är krypterade. (Till exempel om X, sedan kryptera egenskapen A; annars kryptera egenskaper A och b) Observera att det är inte nödvändigt tooprovide denna information vid läsning eller fråga entiteter.

### <a name="batch-operations"></a>Batchåtgärder
En krypteringsprincipen gäller tooall raderna i hello batchen. hello klientbiblioteket genererar internt en ny slumpmässiga IV och slumpmässiga CEK per rad i hello batch. Användare kan också välja tooencrypt olika egenskaper för varje åtgärd i hello batch genom att definiera problemet i hello kryptering lösare.
Om en batch har skapats som en kontext manager hello tableservice batch() med metoden, blir automatiskt hello tableservice krypteringsprincipen tillämpade toohello batch. Om en batch har skapats explicit genom att anropa konstruktorn för hello, skickas hello krypteringsprincipen som en parameter och vänster oförändrade under hello livstid hello batch.
Observera att entiteter har krypterats som de infogas i hello batch med hello batch krypteringsprincipen (entiteter inte är krypterade när hello genomför hello batch med hello tableservice krypteringsprincipen).

### <a name="queries"></a>Frågor
tooperform fråga åtgärder, måste du ange en nyckel matchare som kan tooresolve alla hello nycklar i hello resultatuppsättningen. Om en entitet i frågeresultatet hello inte kan matchas tooa provider, genereras hello klientbiblioteket ett fel. För en fråga som utför server side projektioner hello klientbiblioteket läggs hello särskilda kryptering metadataegenskaper (\_ClientEncryptionMetadata1 och \_ClientEncryptionMetadata2) som standard toohello valda kolumner .

> [!IMPORTANT]
> Tänk på dessa viktiga punkter när du använder kryptering på klientsidan:
> 
> * När läsning från eller skriva tooan krypterad blob, Använd hela blob överför kommandon och intervall/hela blob download kommandon. Undvik att skriva tooan krypterade blob med hjälp av protokollåtgärder som till exempel placera Block, placera Blockeringslista, skriva sidor eller rensa sidor. Annars kan du skadad hello krypterade blob och att den inte kan läsas.
> * För tabeller finns en liknande begränsning. Vara försiktig toonot uppdatera krypterade egenskaper utan att uppdatera metadata för hello-kryptering.
> * Om du anger metadata för hello krypterade blob, kan du skriva över hello krypteringsrelaterade metadata som krävs för dekryptering, eftersom inställningen metadata inte är additiva. Detta gäller även för ögonblicksbilder; Undvik att ange metadata när du skapar en ögonblicksbild av en krypterad blob. Om du måste ange metadata, vara säker på att toocall hello **get_blob_metadata** metoden första tooget hello aktuella krypteringsmetadata och undvika samtidiga skrivningar metadata anges.
> * Aktivera hello **require_encryption** flaggan på hello serviceobjektet för användare som ska fungerar bara med krypterade data. Mer information finns nedan.
> 
> 

hello storage-klientbibliotek förväntar hello angetts KEK och viktiga matcharen tooimplement hello följande gränssnitt. [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) stöd för hantering av Python KEK väntar och kommer att integreras i det här biblioteket när du är klar.

## <a name="client-api--interface"></a>Programmeringsgränssnitt mot klienten / gränssnitt
När ett service lagringsobjekt (d.v.s. blockblobservice) har skapats, hello användare kan tilldela värden toohello fält som utgör en krypteringsprincipen: key_encryption_key key_resolver_function, och require_encryption. Användare kan ange endast en KEK endast en matchare eller båda. key_encryption_key är hello grundläggande nyckeltyp som identifieras med en nyckelidentifierare och som ger hello logik för radbrytning/uppackning. key_resolver_function är används tooresolve en nyckel under hello dekrypteringsprocess. Returnerar en giltig KEK ges en nyckelidentifierare. Detta ger användarna hello möjlighet toochoose mellan flera nycklar som hanteras på flera platser.

Hej KEK måste implementera hello följande metoder toosuccessfully kryptera data:

* wrap_key(cek): radbryter hello anges med en algoritm för hello användares val CEK (byte). Returnerar hello omslutas nyckel.
* get_key_wrap_algorithm(): returnerar hello algoritmen används toowrap nycklar.
* get_kid(): returnerar hello sträng nyckel-id för den här KEK.
  Hej KEK måste implementera hello följande metoder toosuccessfully dekryptera data:
* unwrap_key (cek, algoritm): returnerar hello oförpackade form av hello anges CEK med hello sträng anges algoritm.
* get_kid(): returnerar en sträng nyckel-id för den här KEK.

hello viktiga lösare måste minst implementera en metod som, anges en nyckel-id, returnerar hello motsvarande KEK implementerar hello-gränssnittet ovan. Den här metoden är toobe tilldelade toohello key_resolver_function egenskap hello serviceobjektet.

* Hello-nyckeln används alltid för kryptering, och hello frånvaron av en nyckel resulterar i ett fel.
* För dekryptering:
  
  * hello viktiga matcharen anropas om tooget hello nyckel har angetts. Om hello matcharen har angetts men inte har en mappning för hello nyckelidentifierare, returneras ett fel.
  * Om DNS-matchare inte har angetts, men en nyckel har angetts, används hello nyckeln om dess identifierare matchar nyckelidentifierare hello krävs. Ett fel returneras om hello-ID inte matchar.
    
    Hej kryptering prover i azure.storage.samples <fix URL>demonstrera en mer detaljerad slutpunkt till slutpunkt för BLOB, köer och tabeller.
      Exempel implementeringar av hello KEK och viktiga matchning finns i hello exempelfiler som KeyWrapper och KeyResolver respektive.

### <a name="requireencryption-mode"></a>RequireEncryption läge
Användare kan välja att ett läge där alla överföringar och hämtningsbara filer måste vara krypterad. I det här läget misslyckas försök tooupload data utan en kryptering principen eller hämta data som inte är krypterad på hello-tjänsten på hello-klienten. Hej **require_encryption** flaggan på hello objektet kontrollerar detta beteende.

### <a name="blob-service-encryption"></a>Kryptering för BLOB-tjänst
Ange hello kryptering principfält för hello blockblobservice objekt. Allt annat ska hanteras av hello klientbiblioteket internt.

```python
# Create hello KEK used for encryption.
# KeyWrapper is hello provided sample implementation, but hello user may use their own object as long as it implements hello interface above.
kek = KeyWrapper('local:key1') # Key identifier

# Create hello key resolver used for decryption.
# KeyResolver is hello provided sample implementation, but hello user may use whatever implementation they choose so long as hello function set on hello service object behaves appropriately.
key_resolver = KeyResolver()
key_resolver.put_key(kek)

# Set hello KEK and key resolver on hello service object.
my_block_blob_service.key_encryption_key = kek
my_block_blob_service.key_resolver_funcion = key_resolver.resolve_key

# Upload hello encrypted contents toohello blob.
my_block_blob_service.create_blob_from_stream(container_name, blob_name, stream)

# Download and decrypt hello encrypted contents from hello blob.
blob = my_block_blob_service.get_blob_to_bytes(container_name, blob_name)
```

### <a name="queue-service-encryption"></a>Kön service-kryptering
Ange hello kryptering principfält för hello queueservice objekt. Allt annat ska hanteras av hello klientbiblioteket internt.

```python
# Create hello KEK used for encryption.
# KeyWrapper is hello provided sample implementation, but hello user may use their own object as long as it implements hello interface above.
kek = KeyWrapper('local:key1') # Key identifier

# Create hello key resolver used for decryption.
# KeyResolver is hello provided sample implementation, but hello user may use whatever implementation they choose so long as hello function set on hello service object behaves appropriately.
key_resolver = KeyResolver()
key_resolver.put_key(kek)

# Set hello KEK and key resolver on hello service object.
my_queue_service.key_encryption_key = kek
my_queue_service.key_resolver_funcion = key_resolver.resolve_key

# Add message
my_queue_service.put_message(queue_name, content)

# Retrieve message
retrieved_message_list = my_queue_service.get_messages(queue_name)
```

### <a name="table-service-encryption"></a>Tabellen service-kryptering
Dessutom toocreating en krypteringsprincip för och ange värdet på begäran alternativ, måste du antingen ange en **encryption_resolver_function** på hello **tableservice**, eller ange hello kryptera attributet på Hej EntityProperty.

### <a name="using-hello-resolver"></a>Hello resolvern

```python
# Create hello KEK used for encryption.
# KeyWrapper is hello provided sample implementation, but hello user may use their own object as long as it implements hello interface above.
kek = KeyWrapper('local:key1') # Key identifier

# Create hello key resolver used for decryption.
# KeyResolver is hello provided sample implementation, but hello user may use whatever implementation they choose so long as hello function set on hello service object behaves appropriately.
key_resolver = KeyResolver()
key_resolver.put_key(kek)

# Define hello encryption resolver_function.
def my_encryption_resolver(pk, rk, property_name):
        if property_name == 'foo':
                return True
        return False

# Set hello KEK and key resolver on hello service object.
my_table_service.key_encryption_key = kek
my_table_service.key_resolver_funcion = key_resolver.resolve_key
my_table_service.encryption_resolver_function = my_encryption_resolver

# Insert Entity
my_table_service.insert_entity(table_name, entity)

# Retrieve Entity
# Note: No need toospecify an encryption resolver for retrieve, but it is harmless tooleave hello property set.
my_table_service.get_entity(table_name, entity['PartitionKey'], entity['RowKey'])
```

### <a name="using-attributes"></a>Använda attribut
Som nämnts ovan är en egenskap kan markeras för kryptering genom att lagra det i ett EntityProperty objekt och ange hello kryptera fältet.

```python
encrypted_property_1 = EntityProperty(EdmType.STRING, value, encrypt=True)
```

## <a name="encryption-and-performance"></a>Kryptering och prestanda
Observera att kryptera din lagring data resulterar i ytterligare prestanda försämras. hello innehåll nyckel och IV måste skapas själva hello innehållet måste krypteras och ytterligare metadata måste vara formaterad och har överförts. Det här arbetet varierar beroende på hello mängden data som krypteras. Vi rekommenderar att kunder alltid testa sina program för prestanda under utveckling.

## <a name="next-steps"></a>Nästa steg
* Hämta hello [Azure Storage-klientbibliotek för Java PyPi paketet](https://pypi.python.org/pypi/azure-storage)
* Hämta hello [Azure Storage-klientbibliotek för Python källkod från GitHub](https://github.com/Azure/azure-storage-python)
