---
title: "aaaHow toouse Azure Table Storage från Ruby | Microsoft Docs"
description: Lagra strukturerade data i hello molnet med Azure Table storage, en NoSQL-databas.
services: storage
documentationcenter: ruby
author: mmacy
manager: timlt
editor: 
ms.assetid: 047cd9ff-17d3-4c15-9284-1b5cc61a3224
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: 9c77ff9f384a776c9bc075b60b351685c61acc36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-table-storage-from-ruby"></a>Hur toouse Azure Table Storage från Ruby
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Översikt
Den här guiden visar hur tooperform vanliga scenarier med hjälp av hello Azure Table-tjänsten. hello exempel skrivs med hello Ruby API. hello beskrivs scenarier där **skapar och tar bort en tabell, lägga till och fråga entiteter i en tabell**.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Skapa ett Ruby-program
Anvisningar hur toocreate Ruby program, se [Ruby på spår webbprogram på en Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-tooaccess-storage"></a>Konfigurera ditt program tooaccess lagring
toouse Azure Storage, behöver du toodownload och Använd hello Ruby azure-paketet som innehåller en uppsättning bekvämlighet bibliotek som kommunicerar med hello Storage REST-tjänster.

### <a name="use-rubygems-tooobtain-hello-package"></a>Använd RubyGems tooobtain hello-paket
1. Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac), eller **Bash** (Unix).
2. Typen **symbolen installera azure** i hello kommandot fönstret tooinstall hello symbolen och beroenden.

### <a name="import-hello-package"></a>Importera hello-paket
Använd valfri textredigerare, lägga till hello följande toohello överkant hello Ruby filen där du ska toouse lagring:

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a>Skapa en Azure Storage-anslutning
hello azure-modulen läses hello miljövariabler **AZURE\_lagring\_konto** och **AZURE\_lagring\_åtkomst\_NYCKELN**information krävs tooconnect tooyour Azure Storage-konto. Om de här miljövariablerna inte har angetts måste du ange hello kontoinformation innan du använder **Azure::TableService** med hello följande kod:

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

tooobtain dessa värden från en klassiska eller Resource Manager lagring konto i hello Azure-portalen:

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Navigera toohello storage-konto som du vill toouse.
3. I hello inställningsbladet på hello höger klickar du på **åtkomstnycklar**.
4. I hello åtkomst bladet nycklar som visas, visas hello åtkomstnyckel 1 och åtkomstnyckel 2. Du kan använda någon av dessa.
5. Klicka på hello kopiera ikonen toocopy hello viktiga toohello Urklipp.

tooobtain värdena från klassiska lagring konto i hello klassiska Azure-portalen:

1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Navigera toohello storage-konto som du vill toouse.
3. Klicka på **hantera ÅTKOMSTNYCKLAR** längst hello hello navigeringsfönstret.
4. I hello popup-fönstret visas hello lagringskontonamn, primärnyckeln och sekundära åtkomstnyckeln. Du kan använda hello primära eller hello sekundära för åtkomst till nyckeln.
5. Klicka på hello kopiera ikonen toocopy hello viktiga toohello Urklipp.

## <a name="create-a-table"></a>Skapa en tabell
Hej **Azure::TableService** objekt kan du arbeta med tabeller och de entiteter. toocreate en tabell, använder hello **skapa\_table()** metod. hello följande exempel skapar en tabell eller utskrifter hello fel om sådana finns.

```ruby
azure_table_service = Azure::TableService.new
begin
    azure_table_service.create_table("testtable")
rescue
    puts $!
end
```

## <a name="add-an-entity-tooa-table"></a>Lägg till en entitet tooa tabell
tooadd en entitet, först skapa en hash-objekt som definierar egenskaper för enhet. Observera att för varje entitet måste du ange en **PartitionKey** och **RowKey**. Dessa är hello unika identifierare för dina enheter och är värden som kan efterfrågas mycket snabbare än dina andra egenskaper. Azure Storage använder **PartitionKey** tooautomatically distribuera hello tabellentiteter över många lagringsnoder. Entiteter med hello samma **PartitionKey** lagras på hello samma nod. Hej **RowKey** är hello unikt ID för hello entiteten inom hello-partition som den tillhör.

```ruby
entity = { "content" => "test entity",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.insert_entity("testtable", entity)
```

## <a name="update-an-entity"></a>Uppdatera en entitet
Det finns flera metoder tillgängliga tooupdate en befintlig entitet:

* **Uppdatera\_entity():** uppdatera en befintlig entitet genom att ersätta den.
* **merge\_entity():** uppdaterar en befintlig entitet genom att använda nya egenskapsvärden i hello befintlig entitet.
* **Infoga\_eller\_merge\_entity():** uppdaterar en befintlig entitet genom att ersätta den. Om inga entiteten finns infogas en ny:
* **Infoga\_eller\_ersätta\_entity():** uppdaterar en befintlig entitet genom att använda nya egenskapsvärden i hello befintlig entitet. Om inga entiteten finns infogas en ny.

hello exemplet nedan visar uppdaterar en entitet med **uppdatera\_entity()**:

```ruby
entity = { "content" => "test entity with updated content",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.update_entity("testtable", entity)
```

Med **uppdatera\_entity()** och **merge\_entity()**om hello-enhet som du uppdaterar inte finns hello update-åtgärden kommer att misslyckas. Om du vill toostore en entitet oavsett om det redan finns kan du bör därför i stället använda **infoga\_eller\_ersätta\_entity()** eller **infoga\_eller \_merge\_entity()**.

## <a name="work-with-groups-of-entities"></a>Arbeta med grupper av entiteter
Ibland blir meningsfullt toosubmit flera åtgärder tillsammans i en batch tooensure atomiska bearbetning av hello-servern. tooaccomplish att du först skapa en **Batch** objekt och sedan använda hello **köra\_batch()** metod på **TableService**. hello visar exemplet nedan skickar två entiteter med RowKey 2 och 3 i en batch. Observera att den endast fungerar för entiteter med hello samma PartitionKey.

```ruby
azure_table_service = Azure::TableService.new
batch = Azure::Storage::Table::Batch.new("testtable",
    "test-partition-key") do
    insert "2", { "content" => "new content 2" }
    insert "3", { "content" => "new content 3" }
end
results = azure_table_service.execute_batch(batch)
```

## <a name="query-for-an-entity"></a>Frågan för en entitet
tooquery en entitet i en tabell, Använd hello **hämta\_entity()** metoden genom att skicka hello tabellnamn **PartitionKey** och **RowKey**.

```ruby
result = azure_table_service.get_entity("testtable", "test-partition-key",
    "1")
```

## <a name="query-a-set-of-entities"></a>Fråga en uppsättning enheter
tooquery en uppsättning enheter i en tabell, skapa en fråga hash-objekt och använda hello **frågan\_entities()** metod. hello exemplet nedan visar hämtar alla hello entiteter med hello samma **PartitionKey**:

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'" }
result, token = azure_table_service.query_entities("testtable", query)
```

> [!NOTE]
> Om hello resultatet är för stort för en enskild fråga tooreturn en fortsättningstoken returneras som du kan använda tooretrieve på de efterföljande sidorna.
>
>

## <a name="query-a-subset-of-entity-properties"></a>Fråga en deluppsättning entitetsegenskaper
En tooa frågetabellen kan hämta bara några få egenskaper från en entitet. Den här tekniken, kallad ”projektion”, minskar bandbredden och kan förbättra frågeprestanda, särskilt för stora entiteter. Använd hello select-satsen och pass hello namnen på hello egenskaper som du vill att toobring över toohello.

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'",
    :select => ["content"] }
result, token = azure_table_service.query_entities("testtable", query)
```

## <a name="delete-an-entity"></a>Ta bort en entitet
toodelete en enhet använder hello **ta bort\_entity()** metod. Du måste toopass i hello namnet på hello-tabell som innehåller hello entitetstyper, hello PartitionKey och RowKey av hello entitet.

```ruby
azure_table_service.delete_entity("testtable", "test-partition-key", "1")
```

## <a name="delete-a-table"></a>Ta bort en tabell
toodelete en tabell, använder hello **ta bort\_table()** metod och skicka hello namnet på hello önskad toodelete tabell.

```ruby
azure_table_service.delete_table("testtable")
```

## <a name="next-steps"></a>Nästa steg

* [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en gratis, fristående app från Microsoft som möjliggör toowork visuellt med Azure Storage-data i Windows, macOS och Linux.
* [Azure SDK för Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) databasen på GitHub

