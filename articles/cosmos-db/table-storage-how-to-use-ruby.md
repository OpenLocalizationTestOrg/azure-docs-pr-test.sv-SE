---
title: "Hur du använder Azure Table Storage från Ruby | Microsoft Docs"
description: "Lagra strukturerade data i molnet med hjälp av Azure Table Storage, en NoSQL-databas."
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
ms.openlocfilehash: 372bc89f75ad4730f0defbf9d6f9f041ae5ce1bf
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-azure-table-storage-from-ruby"></a><span data-ttu-id="281bf-103">Hur du använder Azure Table Storage från Ruby</span><span class="sxs-lookup"><span data-stu-id="281bf-103">How to use Azure Table Storage from Ruby</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="281bf-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="281bf-104">Overview</span></span>
<span data-ttu-id="281bf-105">Den här guiden visar hur du utför vanliga scenarier med hjälp av tjänsten Azure Table.</span><span class="sxs-lookup"><span data-stu-id="281bf-105">This guide shows you how to perform common scenarios using the Azure Table service.</span></span> <span data-ttu-id="281bf-106">Exemplen är skrivna med hjälp av Ruby-API.</span><span class="sxs-lookup"><span data-stu-id="281bf-106">The samples are written using the Ruby API.</span></span> <span data-ttu-id="281bf-107">Scenarier som tas upp inkluderar **skapar och tar bort en tabell, lägga till och fråga entiteter i en tabell**.</span><span class="sxs-lookup"><span data-stu-id="281bf-107">The scenarios covered include **creating and deleting a table, inserting and querying entities in a table**.</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a><span data-ttu-id="281bf-108">Skapa ett Ruby-program</span><span class="sxs-lookup"><span data-stu-id="281bf-108">Create a Ruby application</span></span>
<span data-ttu-id="281bf-109">Mer information hur du skapar ett Ruby program finns [Ruby på spår webbprogram på en Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="281bf-109">For instructions how to create a Ruby application, see [Ruby on Rails Web application on an Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span></span>

## <a name="configure-your-application-to-access-storage"></a><span data-ttu-id="281bf-110">Konfigurera ditt program åtkomst till lagring</span><span class="sxs-lookup"><span data-stu-id="281bf-110">Configure your application to access Storage</span></span>
<span data-ttu-id="281bf-111">För att använda Azure Storage, måste du hämta och använda Ruby azure-paketet som innehåller en uppsättning bekvämlighet bibliotek som kommunicerar med Storage REST-tjänster.</span><span class="sxs-lookup"><span data-stu-id="281bf-111">To use Azure Storage, you need to download and use the Ruby azure package which includes a set of convenience libraries that communicate with the Storage REST services.</span></span>

### <a name="use-rubygems-to-obtain-the-package"></a><span data-ttu-id="281bf-112">Använda RubyGems för att hämta paketet</span><span class="sxs-lookup"><span data-stu-id="281bf-112">Use RubyGems to obtain the package</span></span>
1. <span data-ttu-id="281bf-113">Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac), eller **Bash** (Unix).</span><span class="sxs-lookup"><span data-stu-id="281bf-113">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix).</span></span>
2. <span data-ttu-id="281bf-114">Typen **symbolen installera azure** i kommandofönstret att installera symbolen och beroenden.</span><span class="sxs-lookup"><span data-stu-id="281bf-114">Type **gem install azure** in the command window to install the gem and dependencies.</span></span>

### <a name="import-the-package"></a><span data-ttu-id="281bf-115">Importera paketet</span><span class="sxs-lookup"><span data-stu-id="281bf-115">Import the package</span></span>
<span data-ttu-id="281bf-116">Använda valfri textredigerare, lägger du till följande upp i filen Ruby där du tänker använda lagring:</span><span class="sxs-lookup"><span data-stu-id="281bf-116">Use your favorite text editor, add the following to the top of the Ruby file where you intend to use Storage:</span></span>

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="281bf-117">Skapa en Azure Storage-anslutning</span><span class="sxs-lookup"><span data-stu-id="281bf-117">Set up an Azure Storage connection</span></span>
<span data-ttu-id="281bf-118">Azure-modulen läses miljövariablerna **AZURE\_lagring\_konto** och **AZURE\_lagring\_åtkomst\_NYCKELN**information som krävs för att ansluta till ditt Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="281bf-118">The azure module will read the environment variables **AZURE\_STORAGE\_ACCOUNT** and **AZURE\_STORAGE\_ACCESS\_KEY** for information required to connect to your Azure Storage account.</span></span> <span data-ttu-id="281bf-119">Om de här miljövariablerna inte har angetts måste du ange kontoinformation innan du använder **Azure::TableService** med följande kod:</span><span class="sxs-lookup"><span data-stu-id="281bf-119">If these environment variables are not set, you must specify the account information before using **Azure::TableService** with the following code:</span></span>

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

<span data-ttu-id="281bf-120">Du kan hämta dessa värden från en klassiska eller Resource Manager storage-konto i Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="281bf-120">To obtain these values from a classic or Resource Manager storage account in the Azure portal:</span></span>

1. <span data-ttu-id="281bf-121">Logga in på [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="281bf-121">Log in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="281bf-122">Navigera till storage-konto som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="281bf-122">Navigate to the storage account you want to use.</span></span>
3. <span data-ttu-id="281bf-123">I bladet inställningar till höger klickar du på **åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="281bf-123">In the Settings blade on the right, click **Access Keys**.</span></span>
4. <span data-ttu-id="281bf-124">I åtkomst bladet nycklar som visas, visas den åtkomstnyckel 1 och åtkomstnyckel 2.</span><span class="sxs-lookup"><span data-stu-id="281bf-124">In the Access keys blade that appears, you'll see the access key 1 and access key 2.</span></span> <span data-ttu-id="281bf-125">Du kan använda någon av dessa.</span><span class="sxs-lookup"><span data-stu-id="281bf-125">You can use either of these.</span></span>
5. <span data-ttu-id="281bf-126">Klicka på Kopiera-ikonen för att kopiera nyckeln till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="281bf-126">Click the copy icon to copy the key to the clipboard.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="281bf-127">Skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="281bf-127">Create a table</span></span>
<span data-ttu-id="281bf-128">Den **Azure::TableService** objekt kan du arbeta med tabeller och de entiteter.</span><span class="sxs-lookup"><span data-stu-id="281bf-128">The **Azure::TableService** object lets you work with tables and entities.</span></span> <span data-ttu-id="281bf-129">Du kan skapa en tabell med de **skapa\_table()** metod.</span><span class="sxs-lookup"><span data-stu-id="281bf-129">To create a table, use the **create\_table()** method.</span></span> <span data-ttu-id="281bf-130">I följande exempel skapar en tabell eller skriver ut felet om sådana finns.</span><span class="sxs-lookup"><span data-stu-id="281bf-130">The following example creates a table or prints the error if there is any.</span></span>

```ruby
azure_table_service = Azure::TableService.new
begin
    azure_table_service.create_table("testtable")
rescue
    puts $!
end
```

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="281bf-131">Lägga till en entitet i en tabell</span><span class="sxs-lookup"><span data-stu-id="281bf-131">Add an entity to a table</span></span>
<span data-ttu-id="281bf-132">Om du vill lägga till en entitet, först skapa en hash-objekt som definierar egenskaper för enhet.</span><span class="sxs-lookup"><span data-stu-id="281bf-132">To add an entity, first create a hash object that defines your entity properties.</span></span> <span data-ttu-id="281bf-133">Observera att för varje entitet måste du ange en **PartitionKey** och **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="281bf-133">Note that for every entity you must specify a **PartitionKey** and **RowKey**.</span></span> <span data-ttu-id="281bf-134">Dessa är de unika identifierarna för dina enheter och är värden som kan efterfrågas mycket snabbare än dina andra egenskaper.</span><span class="sxs-lookup"><span data-stu-id="281bf-134">These are the unique identifiers of your entities, and are values that can be queried much faster than your other properties.</span></span> <span data-ttu-id="281bf-135">Azure Storage använder **PartitionKey** att automatiskt distribuera tabellens entiteter över många lagringsnoder.</span><span class="sxs-lookup"><span data-stu-id="281bf-135">Azure Storage uses **PartitionKey** to automatically distribute the table's entities over many storage nodes.</span></span> <span data-ttu-id="281bf-136">Entiteter med samma **PartitionKey** lagras på samma nod.</span><span class="sxs-lookup"><span data-stu-id="281bf-136">Entities with the same **PartitionKey** are stored on the same node.</span></span> <span data-ttu-id="281bf-137">Den **RowKey** är unikt ID för entiteten i partitionen som den tillhör.</span><span class="sxs-lookup"><span data-stu-id="281bf-137">The **RowKey** is the unique ID of the entity within the partition it belongs to.</span></span>

```ruby
entity = { "content" => "test entity",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.insert_entity("testtable", entity)
```

## <a name="update-an-entity"></a><span data-ttu-id="281bf-138">Uppdatera en entitet</span><span class="sxs-lookup"><span data-stu-id="281bf-138">Update an entity</span></span>
<span data-ttu-id="281bf-139">Det finns flera metoder för att uppdatera en befintlig entitet:</span><span class="sxs-lookup"><span data-stu-id="281bf-139">There are multiple methods available to update an existing entity:</span></span>

* <span data-ttu-id="281bf-140">**Uppdatera\_entity():** uppdatera en befintlig entitet genom att ersätta den.</span><span class="sxs-lookup"><span data-stu-id="281bf-140">**update\_entity():** Update an existing entity by replacing it.</span></span>
* <span data-ttu-id="281bf-141">**merge\_entity():** uppdaterar en befintlig entitet genom att använda nya egenskapsvärden i befintliga entiteten.</span><span class="sxs-lookup"><span data-stu-id="281bf-141">**merge\_entity():** Updates an existing entity by merging new property values into the existing entity.</span></span>
* <span data-ttu-id="281bf-142">**Infoga\_eller\_merge\_entity():** uppdaterar en befintlig entitet genom att ersätta den.</span><span class="sxs-lookup"><span data-stu-id="281bf-142">**insert\_or\_merge\_entity():** Updates an existing entity by replacing it.</span></span> <span data-ttu-id="281bf-143">Om inga entiteten finns infogas en ny:</span><span class="sxs-lookup"><span data-stu-id="281bf-143">If no entity exists, a new one will be inserted:</span></span>
* <span data-ttu-id="281bf-144">**Infoga\_eller\_ersätta\_entity():** uppdaterar en befintlig entitet genom att använda nya egenskapsvärden i befintliga entiteten.</span><span class="sxs-lookup"><span data-stu-id="281bf-144">**insert\_or\_replace\_entity():** Updates an existing entity by merging new property values into the existing entity.</span></span> <span data-ttu-id="281bf-145">Om inga entiteten finns infogas en ny.</span><span class="sxs-lookup"><span data-stu-id="281bf-145">If no entity exists, a new one will be inserted.</span></span>

<span data-ttu-id="281bf-146">Exemplet nedan visar att uppdatera en entitet med **uppdatera\_entity()**:</span><span class="sxs-lookup"><span data-stu-id="281bf-146">The following example demonstrates updating an entity using **update\_entity()**:</span></span>

```ruby
entity = { "content" => "test entity with updated content",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.update_entity("testtable", entity)
```

<span data-ttu-id="281bf-147">Med **uppdatera\_entity()** och **merge\_entity()**, om den enhet som du uppdaterar inte finns uppdateringen kommer att misslyckas.</span><span class="sxs-lookup"><span data-stu-id="281bf-147">With **update\_entity()** and **merge\_entity()**, if the entity that you are updating doesn't exist then the update operation will fail.</span></span> <span data-ttu-id="281bf-148">Därför om du vill lagra en entitet oavsett om den redan finns, bör du istället använda **infoga\_eller\_ersätta\_entity()** eller **infoga\_eller \_merge\_entity()**.</span><span class="sxs-lookup"><span data-stu-id="281bf-148">Therefore if you wish to store an entity regardless of whether it already exists, you should instead use **insert\_or\_replace\_entity()** or **insert\_or\_merge\_entity()**.</span></span>

## <a name="work-with-groups-of-entities"></a><span data-ttu-id="281bf-149">Arbeta med grupper av entiteter</span><span class="sxs-lookup"><span data-stu-id="281bf-149">Work with groups of entities</span></span>
<span data-ttu-id="281bf-150">Ibland är det praktiskt att skicka flera åtgärder tillsammans i en grupp så atomiska bearbetning av servern.</span><span class="sxs-lookup"><span data-stu-id="281bf-150">Sometimes it makes sense to submit multiple operations together in a batch to ensure atomic processing by the server.</span></span> <span data-ttu-id="281bf-151">För att åstadkomma som måste du först skapa en **Batch** objekt och sedan använda den **köra\_batch()** metod på **TableService**.</span><span class="sxs-lookup"><span data-stu-id="281bf-151">To accomplish that, you first create a **Batch** object and then use the **execute\_batch()** method on **TableService**.</span></span> <span data-ttu-id="281bf-152">Exemplet nedan visar skickar två entiteter med RowKey 2 och 3 i en batch.</span><span class="sxs-lookup"><span data-stu-id="281bf-152">The following example demonstrates submitting two entities with RowKey 2 and 3 in a batch.</span></span> <span data-ttu-id="281bf-153">Observera att den endast fungerar för entiteter med samma PartitionKey.</span><span class="sxs-lookup"><span data-stu-id="281bf-153">Notice that it only works for entities with the same PartitionKey.</span></span>

```ruby
azure_table_service = Azure::TableService.new
batch = Azure::Storage::Table::Batch.new("testtable",
    "test-partition-key") do
    insert "2", { "content" => "new content 2" }
    insert "3", { "content" => "new content 3" }
end
results = azure_table_service.execute_batch(batch)
```

## <a name="query-for-an-entity"></a><span data-ttu-id="281bf-154">Frågan för en entitet</span><span class="sxs-lookup"><span data-stu-id="281bf-154">Query for an entity</span></span>
<span data-ttu-id="281bf-155">Om du vill fråga en entitet i en tabell, använder den **hämta\_entity()** metoden genom att skicka namnet på tabellen **PartitionKey** och **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="281bf-155">To query an entity in a table, use the **get\_entity()** method, by passing the table name, **PartitionKey** and **RowKey**.</span></span>

```ruby
result = azure_table_service.get_entity("testtable", "test-partition-key",
    "1")
```

## <a name="query-a-set-of-entities"></a><span data-ttu-id="281bf-156">Fråga en uppsättning enheter</span><span class="sxs-lookup"><span data-stu-id="281bf-156">Query a set of entities</span></span>
<span data-ttu-id="281bf-157">Om du vill fråga en uppsättning enheter i en tabell, skapa en fråga hash-objekt och använda den **frågan\_entities()** metod.</span><span class="sxs-lookup"><span data-stu-id="281bf-157">To query a set of entities in a table, create a query hash object and use the **query\_entities()** method.</span></span> <span data-ttu-id="281bf-158">Exemplet nedan visar att hämta alla entiteter med samma **PartitionKey**:</span><span class="sxs-lookup"><span data-stu-id="281bf-158">The following example demonstrates getting all the entities with the same **PartitionKey**:</span></span>

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'" }
result, token = azure_table_service.query_entities("testtable", query)
```

> [!NOTE]
> <span data-ttu-id="281bf-159">Om resultatet är för stort för en enskild fråga att returnera en fortsättningstoken returneras som du kan använda för att hämta de efterföljande sidorna.</span><span class="sxs-lookup"><span data-stu-id="281bf-159">If the result set is too large for a single query to return, a continuation token will be returned which you can use to retrieve subsequent pages.</span></span>
>
>

## <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="281bf-160">Fråga en deluppsättning entitetsegenskaper</span><span class="sxs-lookup"><span data-stu-id="281bf-160">Query a subset of entity properties</span></span>
<span data-ttu-id="281bf-161">En fråga till en tabell kan hämta bara några få egenskaper från en entitet.</span><span class="sxs-lookup"><span data-stu-id="281bf-161">A query to a table can retrieve just a few properties from an entity.</span></span> <span data-ttu-id="281bf-162">Den här tekniken, kallad ”projektion”, minskar bandbredden och kan förbättra frågeprestanda, särskilt för stora entiteter.</span><span class="sxs-lookup"><span data-stu-id="281bf-162">This technique, called "projection", reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="281bf-163">Använda select-satsen och ange namnen på de egenskaper som du vill ta till klienten.</span><span class="sxs-lookup"><span data-stu-id="281bf-163">Use the select clause and pass the names of the properties you would like to bring over to the client.</span></span>

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'",
    :select => ["content"] }
result, token = azure_table_service.query_entities("testtable", query)
```

## <a name="delete-an-entity"></a><span data-ttu-id="281bf-164">Ta bort en entitet</span><span class="sxs-lookup"><span data-stu-id="281bf-164">Delete an entity</span></span>
<span data-ttu-id="281bf-165">Om du vill ta bort en entitet, Använd den **ta bort\_entity()** metod.</span><span class="sxs-lookup"><span data-stu-id="281bf-165">To delete an entity, use the **delete\_entity()** method.</span></span> <span data-ttu-id="281bf-166">Du måste ange namnet på den tabell som innehåller entiteten PartitionKey och RowKey för entiteten.</span><span class="sxs-lookup"><span data-stu-id="281bf-166">You need to pass in the name of the table which contains the entity, the PartitionKey and RowKey of the entity.</span></span>

```ruby
azure_table_service.delete_entity("testtable", "test-partition-key", "1")
```

## <a name="delete-a-table"></a><span data-ttu-id="281bf-167">Ta bort en tabell</span><span class="sxs-lookup"><span data-stu-id="281bf-167">Delete a table</span></span>
<span data-ttu-id="281bf-168">Om du vill ta bort en tabell, använder den **ta bort\_table()** metod och pass i tabellen som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="281bf-168">To delete a table, use the **delete\_table()** method and pass in the name of the table you want to delete.</span></span>

```ruby
azure_table_service.delete_table("testtable")
```

## <a name="next-steps"></a><span data-ttu-id="281bf-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="281bf-169">Next steps</span></span>

* <span data-ttu-id="281bf-170">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en kostnadsfri, fristående app från Microsoft som gör det möjligt att arbeta visuellt med Azure Storage-data i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="281bf-170">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you to work visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="281bf-171">[Azure SDK för Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) databasen på GitHub</span><span class="sxs-lookup"><span data-stu-id="281bf-171">[Azure SDK for Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) repository on GitHub</span></span>

