---
title: "aaaHow toouse Azure Table Storage från Ruby | Microsoft Docs"
description: Lagra strukturerade data i hello molnet med Azure Table storage, en NoSQL-databas.
services: cosmos-db
documentationcenter: ruby
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 047cd9ff-17d3-4c15-9284-1b5cc61a3224
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.openlocfilehash: 2f9eb5a9160b551d6d1d198869787070c402b1d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-table-storage-from-ruby"></a><span data-ttu-id="9852d-103">Hur toouse Azure Table Storage från Ruby</span><span class="sxs-lookup"><span data-stu-id="9852d-103">How toouse Azure Table Storage from Ruby</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="9852d-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="9852d-104">Overview</span></span>
<span data-ttu-id="9852d-105">Den här guiden visar hur tooperform vanliga scenarier med hjälp av hello Azure Table-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9852d-105">This guide shows you how tooperform common scenarios using hello Azure Table service.</span></span> <span data-ttu-id="9852d-106">hello exempel skrivs med hello Ruby API.</span><span class="sxs-lookup"><span data-stu-id="9852d-106">hello samples are written using hello Ruby API.</span></span> <span data-ttu-id="9852d-107">hello beskrivs scenarier där **skapar och tar bort en tabell, lägga till och fråga entiteter i en tabell**.</span><span class="sxs-lookup"><span data-stu-id="9852d-107">hello scenarios covered include **creating and deleting a table, inserting and querying entities in a table**.</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a><span data-ttu-id="9852d-108">Skapa ett Ruby-program</span><span class="sxs-lookup"><span data-stu-id="9852d-108">Create a Ruby application</span></span>
<span data-ttu-id="9852d-109">Anvisningar hur toocreate Ruby program, se [Ruby på spår webbprogram på en Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="9852d-109">For instructions how toocreate a Ruby application, see [Ruby on Rails Web application on an Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span></span>

## <a name="configure-your-application-tooaccess-storage"></a><span data-ttu-id="9852d-110">Konfigurera ditt program tooaccess lagring</span><span class="sxs-lookup"><span data-stu-id="9852d-110">Configure your application tooaccess Storage</span></span>
<span data-ttu-id="9852d-111">toouse Azure Storage, behöver du toodownload och Använd hello Ruby azure-paketet som innehåller en uppsättning bekvämlighet bibliotek som kommunicerar med hello Storage REST-tjänster.</span><span class="sxs-lookup"><span data-stu-id="9852d-111">toouse Azure Storage, you need toodownload and use hello Ruby azure package which includes a set of convenience libraries that communicate with hello Storage REST services.</span></span>

### <a name="use-rubygems-tooobtain-hello-package"></a><span data-ttu-id="9852d-112">Använd RubyGems tooobtain hello-paket</span><span class="sxs-lookup"><span data-stu-id="9852d-112">Use RubyGems tooobtain hello package</span></span>
1. <span data-ttu-id="9852d-113">Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac), eller **Bash** (Unix).</span><span class="sxs-lookup"><span data-stu-id="9852d-113">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix).</span></span>
2. <span data-ttu-id="9852d-114">Typen **symbolen installera azure** i hello kommandot fönstret tooinstall hello symbolen och beroenden.</span><span class="sxs-lookup"><span data-stu-id="9852d-114">Type **gem install azure** in hello command window tooinstall hello gem and dependencies.</span></span>

### <a name="import-hello-package"></a><span data-ttu-id="9852d-115">Importera hello-paket</span><span class="sxs-lookup"><span data-stu-id="9852d-115">Import hello package</span></span>
<span data-ttu-id="9852d-116">Använd valfri textredigerare, lägga till hello följande toohello överkant hello Ruby filen där du ska toouse lagring:</span><span class="sxs-lookup"><span data-stu-id="9852d-116">Use your favorite text editor, add hello following toohello top of hello Ruby file where you intend toouse Storage:</span></span>

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="9852d-117">Skapa en Azure Storage-anslutning</span><span class="sxs-lookup"><span data-stu-id="9852d-117">Set up an Azure Storage connection</span></span>
<span data-ttu-id="9852d-118">hello azure-modulen läses hello miljövariabler **AZURE\_lagring\_konto** och **AZURE\_lagring\_åtkomst\_NYCKELN**information krävs tooconnect tooyour Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="9852d-118">hello azure module will read hello environment variables **AZURE\_STORAGE\_ACCOUNT** and **AZURE\_STORAGE\_ACCESS\_KEY** for information required tooconnect tooyour Azure Storage account.</span></span> <span data-ttu-id="9852d-119">Om de här miljövariablerna inte har angetts måste du ange hello kontoinformation innan du använder **Azure::TableService** med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="9852d-119">If these environment variables are not set, you must specify hello account information before using **Azure::TableService** with hello following code:</span></span>

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

<span data-ttu-id="9852d-120">tooobtain dessa värden från en klassiska eller Resource Manager lagring konto i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="9852d-120">tooobtain these values from a classic or Resource Manager storage account in hello Azure portal:</span></span>

1. <span data-ttu-id="9852d-121">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9852d-121">Log in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="9852d-122">Navigera toohello storage-konto som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="9852d-122">Navigate toohello storage account you want toouse.</span></span>
3. <span data-ttu-id="9852d-123">I hello inställningsbladet på hello höger klickar du på **åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="9852d-123">In hello Settings blade on hello right, click **Access Keys**.</span></span>
4. <span data-ttu-id="9852d-124">I hello åtkomst bladet nycklar som visas, visas hello åtkomstnyckel 1 och åtkomstnyckel 2.</span><span class="sxs-lookup"><span data-stu-id="9852d-124">In hello Access keys blade that appears, you'll see hello access key 1 and access key 2.</span></span> <span data-ttu-id="9852d-125">Du kan använda någon av dessa.</span><span class="sxs-lookup"><span data-stu-id="9852d-125">You can use either of these.</span></span>
5. <span data-ttu-id="9852d-126">Klicka på hello kopiera ikonen toocopy hello viktiga toohello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="9852d-126">Click hello copy icon toocopy hello key toohello clipboard.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="9852d-127">Skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="9852d-127">Create a table</span></span>
<span data-ttu-id="9852d-128">Hej **Azure::TableService** objekt kan du arbeta med tabeller och de entiteter.</span><span class="sxs-lookup"><span data-stu-id="9852d-128">hello **Azure::TableService** object lets you work with tables and entities.</span></span> <span data-ttu-id="9852d-129">toocreate en tabell, använder hello **skapa\_table()** metod.</span><span class="sxs-lookup"><span data-stu-id="9852d-129">toocreate a table, use hello **create\_table()** method.</span></span> <span data-ttu-id="9852d-130">hello följande exempel skapar en tabell eller utskrifter hello fel om sådana finns.</span><span class="sxs-lookup"><span data-stu-id="9852d-130">hello following example creates a table or prints hello error if there is any.</span></span>

```ruby
azure_table_service = Azure::TableService.new
begin
    azure_table_service.create_table("testtable")
rescue
    puts $!
end
```

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="9852d-131">Lägg till en entitet tooa tabell</span><span class="sxs-lookup"><span data-stu-id="9852d-131">Add an entity tooa table</span></span>
<span data-ttu-id="9852d-132">tooadd en entitet, först skapa en hash-objekt som definierar egenskaper för enhet.</span><span class="sxs-lookup"><span data-stu-id="9852d-132">tooadd an entity, first create a hash object that defines your entity properties.</span></span> <span data-ttu-id="9852d-133">Observera att för varje entitet måste du ange en **PartitionKey** och **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="9852d-133">Note that for every entity you must specify a **PartitionKey** and **RowKey**.</span></span> <span data-ttu-id="9852d-134">Dessa är hello unika identifierare för dina enheter och är värden som kan efterfrågas mycket snabbare än dina andra egenskaper.</span><span class="sxs-lookup"><span data-stu-id="9852d-134">These are hello unique identifiers of your entities, and are values that can be queried much faster than your other properties.</span></span> <span data-ttu-id="9852d-135">Azure Storage använder **PartitionKey** tooautomatically distribuera hello tabellentiteter över många lagringsnoder.</span><span class="sxs-lookup"><span data-stu-id="9852d-135">Azure Storage uses **PartitionKey** tooautomatically distribute hello table's entities over many storage nodes.</span></span> <span data-ttu-id="9852d-136">Entiteter med hello samma **PartitionKey** lagras på hello samma nod.</span><span class="sxs-lookup"><span data-stu-id="9852d-136">Entities with hello same **PartitionKey** are stored on hello same node.</span></span> <span data-ttu-id="9852d-137">Hej **RowKey** är hello unikt ID för hello entiteten inom hello-partition som den tillhör.</span><span class="sxs-lookup"><span data-stu-id="9852d-137">hello **RowKey** is hello unique ID of hello entity within hello partition it belongs to.</span></span>

```ruby
entity = { "content" => "test entity",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.insert_entity("testtable", entity)
```

## <a name="update-an-entity"></a><span data-ttu-id="9852d-138">Uppdatera en entitet</span><span class="sxs-lookup"><span data-stu-id="9852d-138">Update an entity</span></span>
<span data-ttu-id="9852d-139">Det finns flera metoder tillgängliga tooupdate en befintlig entitet:</span><span class="sxs-lookup"><span data-stu-id="9852d-139">There are multiple methods available tooupdate an existing entity:</span></span>

* <span data-ttu-id="9852d-140">**Uppdatera\_entity():** uppdatera en befintlig entitet genom att ersätta den.</span><span class="sxs-lookup"><span data-stu-id="9852d-140">**update\_entity():** Update an existing entity by replacing it.</span></span>
* <span data-ttu-id="9852d-141">**merge\_entity():** uppdaterar en befintlig entitet genom att använda nya egenskapsvärden i hello befintlig entitet.</span><span class="sxs-lookup"><span data-stu-id="9852d-141">**merge\_entity():** Updates an existing entity by merging new property values into hello existing entity.</span></span>
* <span data-ttu-id="9852d-142">**Infoga\_eller\_merge\_entity():** uppdaterar en befintlig entitet genom att ersätta den.</span><span class="sxs-lookup"><span data-stu-id="9852d-142">**insert\_or\_merge\_entity():** Updates an existing entity by replacing it.</span></span> <span data-ttu-id="9852d-143">Om inga entiteten finns infogas en ny:</span><span class="sxs-lookup"><span data-stu-id="9852d-143">If no entity exists, a new one will be inserted:</span></span>
* <span data-ttu-id="9852d-144">**Infoga\_eller\_ersätta\_entity():** uppdaterar en befintlig entitet genom att använda nya egenskapsvärden i hello befintlig entitet.</span><span class="sxs-lookup"><span data-stu-id="9852d-144">**insert\_or\_replace\_entity():** Updates an existing entity by merging new property values into hello existing entity.</span></span> <span data-ttu-id="9852d-145">Om inga entiteten finns infogas en ny.</span><span class="sxs-lookup"><span data-stu-id="9852d-145">If no entity exists, a new one will be inserted.</span></span>

<span data-ttu-id="9852d-146">hello exemplet nedan visar uppdaterar en entitet med **uppdatera\_entity()**:</span><span class="sxs-lookup"><span data-stu-id="9852d-146">hello following example demonstrates updating an entity using **update\_entity()**:</span></span>

```ruby
entity = { "content" => "test entity with updated content",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.update_entity("testtable", entity)
```

<span data-ttu-id="9852d-147">Med **uppdatera\_entity()** och **merge\_entity()**om hello-enhet som du uppdaterar inte finns hello update-åtgärden kommer att misslyckas.</span><span class="sxs-lookup"><span data-stu-id="9852d-147">With **update\_entity()** and **merge\_entity()**, if hello entity that you are updating doesn't exist then hello update operation will fail.</span></span> <span data-ttu-id="9852d-148">Om du vill toostore en entitet oavsett om det redan finns kan du bör därför i stället använda **infoga\_eller\_ersätta\_entity()** eller **infoga\_eller \_merge\_entity()**.</span><span class="sxs-lookup"><span data-stu-id="9852d-148">Therefore if you wish toostore an entity regardless of whether it already exists, you should instead use **insert\_or\_replace\_entity()** or **insert\_or\_merge\_entity()**.</span></span>

## <a name="work-with-groups-of-entities"></a><span data-ttu-id="9852d-149">Arbeta med grupper av entiteter</span><span class="sxs-lookup"><span data-stu-id="9852d-149">Work with groups of entities</span></span>
<span data-ttu-id="9852d-150">Ibland blir meningsfullt toosubmit flera åtgärder tillsammans i en batch tooensure atomiska bearbetning av hello-servern.</span><span class="sxs-lookup"><span data-stu-id="9852d-150">Sometimes it makes sense toosubmit multiple operations together in a batch tooensure atomic processing by hello server.</span></span> <span data-ttu-id="9852d-151">tooaccomplish att du först skapa en **Batch** objekt och sedan använda hello **köra\_batch()** metod på **TableService**.</span><span class="sxs-lookup"><span data-stu-id="9852d-151">tooaccomplish that, you first create a **Batch** object and then use hello **execute\_batch()** method on **TableService**.</span></span> <span data-ttu-id="9852d-152">hello visar exemplet nedan skickar två entiteter med RowKey 2 och 3 i en batch.</span><span class="sxs-lookup"><span data-stu-id="9852d-152">hello following example demonstrates submitting two entities with RowKey 2 and 3 in a batch.</span></span> <span data-ttu-id="9852d-153">Observera att den endast fungerar för entiteter med hello samma PartitionKey.</span><span class="sxs-lookup"><span data-stu-id="9852d-153">Notice that it only works for entities with hello same PartitionKey.</span></span>

```ruby
azure_table_service = Azure::TableService.new
batch = Azure::Storage::Table::Batch.new("testtable",
    "test-partition-key") do
    insert "2", { "content" => "new content 2" }
    insert "3", { "content" => "new content 3" }
end
results = azure_table_service.execute_batch(batch)
```

## <a name="query-for-an-entity"></a><span data-ttu-id="9852d-154">Frågan för en entitet</span><span class="sxs-lookup"><span data-stu-id="9852d-154">Query for an entity</span></span>
<span data-ttu-id="9852d-155">tooquery en entitet i en tabell, Använd hello **hämta\_entity()** metoden genom att skicka hello tabellnamn **PartitionKey** och **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="9852d-155">tooquery an entity in a table, use hello **get\_entity()** method, by passing hello table name, **PartitionKey** and **RowKey**.</span></span>

```ruby
result = azure_table_service.get_entity("testtable", "test-partition-key",
    "1")
```

## <a name="query-a-set-of-entities"></a><span data-ttu-id="9852d-156">Fråga en uppsättning enheter</span><span class="sxs-lookup"><span data-stu-id="9852d-156">Query a set of entities</span></span>
<span data-ttu-id="9852d-157">tooquery en uppsättning enheter i en tabell, skapa en fråga hash-objekt och använda hello **frågan\_entities()** metod.</span><span class="sxs-lookup"><span data-stu-id="9852d-157">tooquery a set of entities in a table, create a query hash object and use hello **query\_entities()** method.</span></span> <span data-ttu-id="9852d-158">hello exemplet nedan visar hämtar alla hello entiteter med hello samma **PartitionKey**:</span><span class="sxs-lookup"><span data-stu-id="9852d-158">hello following example demonstrates getting all hello entities with hello same **PartitionKey**:</span></span>

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'" }
result, token = azure_table_service.query_entities("testtable", query)
```

> [!NOTE]
> <span data-ttu-id="9852d-159">Om hello resultatet är för stort för en enskild fråga tooreturn en fortsättningstoken returneras som du kan använda tooretrieve på de efterföljande sidorna.</span><span class="sxs-lookup"><span data-stu-id="9852d-159">If hello result set is too large for a single query tooreturn, a continuation token will be returned which you can use tooretrieve subsequent pages.</span></span>
>
>

## <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="9852d-160">Fråga en deluppsättning entitetsegenskaper</span><span class="sxs-lookup"><span data-stu-id="9852d-160">Query a subset of entity properties</span></span>
<span data-ttu-id="9852d-161">En tooa frågetabellen kan hämta bara några få egenskaper från en entitet.</span><span class="sxs-lookup"><span data-stu-id="9852d-161">A query tooa table can retrieve just a few properties from an entity.</span></span> <span data-ttu-id="9852d-162">Den här tekniken, kallad ”projektion”, minskar bandbredden och kan förbättra frågeprestanda, särskilt för stora entiteter.</span><span class="sxs-lookup"><span data-stu-id="9852d-162">This technique, called "projection", reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="9852d-163">Använd hello select-satsen och pass hello namnen på hello egenskaper som du vill att toobring över toohello.</span><span class="sxs-lookup"><span data-stu-id="9852d-163">Use hello select clause and pass hello names of hello properties you would like toobring over toohello client.</span></span>

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'",
    :select => ["content"] }
result, token = azure_table_service.query_entities("testtable", query)
```

## <a name="delete-an-entity"></a><span data-ttu-id="9852d-164">Ta bort en entitet</span><span class="sxs-lookup"><span data-stu-id="9852d-164">Delete an entity</span></span>
<span data-ttu-id="9852d-165">toodelete en enhet använder hello **ta bort\_entity()** metod.</span><span class="sxs-lookup"><span data-stu-id="9852d-165">toodelete an entity, use hello **delete\_entity()** method.</span></span> <span data-ttu-id="9852d-166">Du måste toopass i hello namnet på hello-tabell som innehåller hello entitetstyper, hello PartitionKey och RowKey av hello entitet.</span><span class="sxs-lookup"><span data-stu-id="9852d-166">You need toopass in hello name of hello table which contains hello entity, hello PartitionKey and RowKey of hello entity.</span></span>

```ruby
azure_table_service.delete_entity("testtable", "test-partition-key", "1")
```

## <a name="delete-a-table"></a><span data-ttu-id="9852d-167">Ta bort en tabell</span><span class="sxs-lookup"><span data-stu-id="9852d-167">Delete a table</span></span>
<span data-ttu-id="9852d-168">toodelete en tabell, använder hello **ta bort\_table()** metod och skicka hello namnet på hello önskad toodelete tabell.</span><span class="sxs-lookup"><span data-stu-id="9852d-168">toodelete a table, use hello **delete\_table()** method and pass in hello name of hello table you want toodelete.</span></span>

```ruby
azure_table_service.delete_table("testtable")
```

## <a name="next-steps"></a><span data-ttu-id="9852d-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9852d-169">Next steps</span></span>

* <span data-ttu-id="9852d-170">[Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en gratis, fristående app från Microsoft som möjliggör toowork visuellt med Azure Storage-data i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="9852d-170">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="9852d-171">[Azure SDK för Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) databasen på GitHub</span><span class="sxs-lookup"><span data-stu-id="9852d-171">[Azure SDK for Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) repository on GitHub</span></span>

