---
title: aaaHow toouse Azure Table storage med Python | Microsoft Docs
description: Lagra strukturerade data i hello molnet med Azure Table storage, en NoSQL-databas.
services: cosmos-db
documentationcenter: python
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 7ddb9f3e-4e6d-4103-96e6-f0351d69a17b
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/16/2017
ms.author: mimig
ms.openlocfilehash: 3382fcd5667a93d5533b5f8fad1d3d1c27f23482
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-table-storage-in-python"></a><span data-ttu-id="9c560-103">Hur toouse tabellagring i Python</span><span class="sxs-lookup"><span data-stu-id="9c560-103">How toouse Table storage in Python</span></span>

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

<span data-ttu-id="9c560-104">Den här guiden visar hur tooperform vanliga Azure Table storage scenarier i Python med hello [Microsoft Azure Storage SDK för Python](https://github.com/Azure/azure-storage-python).</span><span class="sxs-lookup"><span data-stu-id="9c560-104">This guide shows you how tooperform common Azure Table storage scenarios in Python using hello [Microsoft Azure Storage SDK for Python](https://github.com/Azure/azure-storage-python).</span></span> <span data-ttu-id="9c560-105">hello Lär dig bland annat hur du skapar och tar bort en tabell och infoga fråga entiteter.</span><span class="sxs-lookup"><span data-stu-id="9c560-105">hello scenarios covered include creating and deleting a table, and inserting and querying entities.</span></span>

<span data-ttu-id="9c560-106">När du arbetar via hello scenarier i den här kursen får du vill toorefer toohello [Azure Storage SDK för Python API-referens för](https://azure-storage.readthedocs.io/en/latest/index.html).</span><span class="sxs-lookup"><span data-stu-id="9c560-106">While working through hello scenarios in this tutorial, you may wish toorefer toohello [Azure Storage SDK for Python API reference](https://azure-storage.readthedocs.io/en/latest/index.html).</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="install-hello-azure-storage-sdk-for-python"></a><span data-ttu-id="9c560-107">Installera hello Azure Storage SDK för Python</span><span class="sxs-lookup"><span data-stu-id="9c560-107">Install hello Azure Storage SDK for Python</span></span>

<span data-ttu-id="9c560-108">När du har skapat ett lagringskonto, nästa steg är tooinstall hello [Microsoft Azure Storage SDK för Python](https://github.com/Azure/azure-storage-python).</span><span class="sxs-lookup"><span data-stu-id="9c560-108">Once you've created a storage account, your next step is tooinstall hello [Microsoft Azure Storage SDK for Python](https://github.com/Azure/azure-storage-python).</span></span> <span data-ttu-id="9c560-109">För information om hur du installerar hello SDK, se toohello [README.rst](https://github.com/Azure/azure-storage-python/blob/master/README.rst) filen i hello Storage SDK för Python lagringsplats på GitHub.</span><span class="sxs-lookup"><span data-stu-id="9c560-109">For details on installing hello SDK, refer toohello [README.rst](https://github.com/Azure/azure-storage-python/blob/master/README.rst) file in hello Storage SDK for Python repository on GitHub.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="9c560-110">Skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="9c560-110">Create a table</span></span>

<span data-ttu-id="9c560-111">toowork med hello Azure Table-tjänsten i Python, måste du importera hello [TableService] [ py_TableService] modul.</span><span class="sxs-lookup"><span data-stu-id="9c560-111">toowork with hello Azure Table service in Python, you must import hello [TableService][py_TableService] module.</span></span> <span data-ttu-id="9c560-112">Eftersom du arbetar med tabellentiteter, måste du också hello [entiteten] [ py_Entity] klass.</span><span class="sxs-lookup"><span data-stu-id="9c560-112">Since you'll be working with Table entities, you also need hello [Entity][py_Entity] class.</span></span> <span data-ttu-id="9c560-113">Lägg till den här koden hello övre delen din Python filen tooimport båda:</span><span class="sxs-lookup"><span data-stu-id="9c560-113">Add this code near hello top your Python file tooimport both:</span></span>

```python
from azure.storage.table import TableService, Entity
```

<span data-ttu-id="9c560-114">Skapa en [TableService] [ py_TableService] objekt som passerar i din namn och åtkomstnyckel för lagring.</span><span class="sxs-lookup"><span data-stu-id="9c560-114">Create a [TableService][py_TableService] object, passing in your storage account name and account key.</span></span> <span data-ttu-id="9c560-115">Ersätt `myaccount` och `mykey` med kontonamnet och nyckeln och anropet [create_table] [ py_create_table] toocreate hello tabell i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="9c560-115">Replace `myaccount` and `mykey` with your account name and key, and call [create_table][py_create_table] toocreate hello table in Azure Storage.</span></span>

```python
table_service = TableService(account_name='myaccount', account_key='mykey')

table_service.create_table('tasktable')
```

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="9c560-116">Lägg till en entitet tooa tabell</span><span class="sxs-lookup"><span data-stu-id="9c560-116">Add an entity tooa table</span></span>

<span data-ttu-id="9c560-117">tooadd en entitet du först skapa ett objekt som representerar din enhet, sedan pass hello objektet toohello [TableService][py_TableService].[ insert_entity] [ py_insert_entity] metod.</span><span class="sxs-lookup"><span data-stu-id="9c560-117">tooadd an entity, you first create an object that represents your entity, then pass hello object toohello [TableService][py_TableService].[insert_entity][py_insert_entity] method.</span></span> <span data-ttu-id="9c560-118">hello enhetsobjekt kan vara en ordlista eller ett objekt av typen [entiteten][py_Entity], och definierar egenskapsnamn och värden för din enhet.</span><span class="sxs-lookup"><span data-stu-id="9c560-118">hello entity object can be a dictionary or an object of type [Entity][py_Entity], and defines your entity's property names and values.</span></span> <span data-ttu-id="9c560-119">Varje entitet måste innehålla hello krävs [PartitionKey och RowKey](#partitionkey-and-rowkey) egenskaper i tillägg tooany andra egenskaper du definierar för hello entitet.</span><span class="sxs-lookup"><span data-stu-id="9c560-119">Every entity must include hello required [PartitionKey and RowKey](#partitionkey-and-rowkey) properties, in addition tooany other properties you define for hello entity.</span></span>

<span data-ttu-id="9c560-120">Det här exemplet skapar ett katalogobjekt som representerar en entitet skickar sedan den toohello [insert_entity] [ py_insert_entity] metoden tooadd den toohello tabellen:</span><span class="sxs-lookup"><span data-stu-id="9c560-120">This example creates a dictionary object representing an entity, then passes it toohello [insert_entity][py_insert_entity] method tooadd it toohello table:</span></span>

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out hello trash', 'priority' : 200}
table_service.insert_entity('tasktable', task)
```

<span data-ttu-id="9c560-121">Det här exemplet skapas en [entiteten] [ py_Entity] objekt och skickar den toohello [insert_entity] [ py_insert_entity] metoden tooadd den toohello tabellen:</span><span class="sxs-lookup"><span data-stu-id="9c560-121">This example creates an [Entity][py_Entity] object, then passes it toohello [insert_entity][py_insert_entity] method tooadd it toohello table:</span></span>

```python
task = Entity()
task.PartitionKey = 'tasksSeattle'
task.RowKey = '002'
task.description = 'Wash hello car'
task.priority = 100
table_service.insert_entity('tasktable', task)
```

### <a name="partitionkey-and-rowkey"></a><span data-ttu-id="9c560-122">PartitionKey och RowKey</span><span class="sxs-lookup"><span data-stu-id="9c560-122">PartitionKey and RowKey</span></span>

<span data-ttu-id="9c560-123">Du måste ange både en **PartitionKey** och en **RowKey** egenskap för varje entitet.</span><span class="sxs-lookup"><span data-stu-id="9c560-123">You must specify both a **PartitionKey** and a **RowKey** property for every entity.</span></span> <span data-ttu-id="9c560-124">Dessa är hello unika identifierare för dina enheter, som tillsammans de bildar hello primärnyckel för en entitet.</span><span class="sxs-lookup"><span data-stu-id="9c560-124">These are hello unique identifiers of your entities, as together they form hello primary key of an entity.</span></span> <span data-ttu-id="9c560-125">Du kan fråga med hjälp av dessa värden som är mycket snabbare än du kan fråga andra Entitetsegenskaper eftersom endast dessa egenskaper indexeras.</span><span class="sxs-lookup"><span data-stu-id="9c560-125">You can query using these values much faster than you can query any other entity properties because only these properties are indexed.</span></span>

<span data-ttu-id="9c560-126">Hej tabell tjänsten använder **PartitionKey** toointelligently distribuerar tabellentiteter över lagringsnoder.</span><span class="sxs-lookup"><span data-stu-id="9c560-126">hello Table service uses **PartitionKey** toointelligently distribute table entities across storage nodes.</span></span> <span data-ttu-id="9c560-127">Enheter som har hello samma **PartitionKey** lagras på hello samma nod.</span><span class="sxs-lookup"><span data-stu-id="9c560-127">Entities that have hello same  **PartitionKey** are stored on hello same node.</span></span> <span data-ttu-id="9c560-128">**RowKey** är hello unikt ID för hello entiteten inom hello-partition som den tillhör.</span><span class="sxs-lookup"><span data-stu-id="9c560-128">**RowKey** is hello unique ID of hello entity within hello partition it belongs to.</span></span>

## <a name="update-an-entity"></a><span data-ttu-id="9c560-129">Uppdatera en entitet</span><span class="sxs-lookup"><span data-stu-id="9c560-129">Update an entity</span></span>

<span data-ttu-id="9c560-130">tooupdate alla egenskapsvärden för en entitet, anropa hello [update_entity] [ py_update_entity] metod.</span><span class="sxs-lookup"><span data-stu-id="9c560-130">tooupdate all of an entity's property values, call hello [update_entity][py_update_entity] method.</span></span> <span data-ttu-id="9c560-131">Det här exemplet illustrerar hur tooreplace en befintlig entitet med en uppdaterad version:</span><span class="sxs-lookup"><span data-stu-id="9c560-131">This example shows how tooreplace an existing entity with an updated version:</span></span>

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out hello garbage', 'priority' : 250}
table_service.update_entity('tasktable', task)
```

<span data-ttu-id="9c560-132">Om hello entitet som ska uppdateras inte redan finns kommer hello uppdateringen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="9c560-132">If hello entity that is being updated doesn't already exist, then hello update operation will fail.</span></span> <span data-ttu-id="9c560-133">Om du vill toostore en entitet om den finns eller inte använda [insert_or_replace_entity][py_insert_or_replace_entity].</span><span class="sxs-lookup"><span data-stu-id="9c560-133">If you want toostore an entity whether it exists or not, use [insert_or_replace_entity][py_insert_or_replace_entity].</span></span> <span data-ttu-id="9c560-134">I följande exempel hello, ersätter hello första anropet befintliga hello-entiteten.</span><span class="sxs-lookup"><span data-stu-id="9c560-134">In hello following example, hello first call will replace hello existing entity.</span></span> <span data-ttu-id="9c560-135">hello andra anropet infogar en ny entitet, eftersom ingen entitet med hello angetts PartitionKey och RowKey finns i hello tabellen.</span><span class="sxs-lookup"><span data-stu-id="9c560-135">hello second call will insert a new entity, since no entity with hello specified PartitionKey and RowKey exists in hello table.</span></span>

```python
# Replace hello entity created earlier
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out hello garbage again', 'priority' : 250}
table_service.insert_or_replace_entity('tasktable', task)

# Insert a new entity
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '003', 'description' : 'Buy detergent', 'priority' : 300}
table_service.insert_or_replace_entity('tasktable', task)
```

> [!TIP]
> <span data-ttu-id="9c560-136">Hej [update_entity] [ py_update_entity] metoden ersätter alla egenskaper och värdena för en befintlig enhet som du kan också använda tooremove egenskaper från en befintlig entitet.</span><span class="sxs-lookup"><span data-stu-id="9c560-136">hello [update_entity][py_update_entity] method replaces all properties and values of an existing entity, which you can also use tooremove properties from an existing entity.</span></span> <span data-ttu-id="9c560-137">Du kan använda hello [merge_entity] [ py_merge_entity] metoden tooupdate en befintlig entitet med nya eller ändrade egenskapsvärden utan att helt ersätta hello entitet.</span><span class="sxs-lookup"><span data-stu-id="9c560-137">You can use hello [merge_entity][py_merge_entity] method tooupdate an existing entity with new or modified property values without completely replacing hello entity.</span></span>

## <a name="modify-multiple-entities"></a><span data-ttu-id="9c560-138">Ändra flera entiteter</span><span class="sxs-lookup"><span data-stu-id="9c560-138">Modify multiple entities</span></span>

<span data-ttu-id="9c560-139">tooensure Hej atomiska bearbetningen av en begäran av hello tabelltjänsten, kan du skicka flera åtgärder tillsammans i en batch.</span><span class="sxs-lookup"><span data-stu-id="9c560-139">tooensure hello atomic processing of a request by hello Table service, you can submit multiple operations together in a batch.</span></span> <span data-ttu-id="9c560-140">Använd först hello [TableBatch] [ py_TableBatch] klassen tooadd flera åtgärder tooa enskild batch.</span><span class="sxs-lookup"><span data-stu-id="9c560-140">First, use hello [TableBatch][py_TableBatch] class tooadd multiple operations tooa single batch.</span></span> <span data-ttu-id="9c560-141">Därefter anropar [TableService][py_TableService].[ commit_batch] [ py_commit_batch] toosubmit hello åtgärder i en atomisk åtgärd.</span><span class="sxs-lookup"><span data-stu-id="9c560-141">Next, call [TableService][py_TableService].[commit_batch][py_commit_batch] toosubmit hello operations in an atomic operation.</span></span> <span data-ttu-id="9c560-142">Alla entiteter toobe ändras i batchen måste vara i hello samma partition.</span><span class="sxs-lookup"><span data-stu-id="9c560-142">All entities toobe modified in batch must be in hello same partition.</span></span>

<span data-ttu-id="9c560-143">Det här exemplet lägger till två entiteter tillsammans i en batch:</span><span class="sxs-lookup"><span data-stu-id="9c560-143">This example adds two entities together in a batch:</span></span>

```python
from azure.storage.table import TableBatch
batch = TableBatch()
task004 = {'PartitionKey': 'tasksSeattle', 'RowKey': '004', 'description' : 'Go grocery shopping', 'priority' : 400}
task005 = {'PartitionKey': 'tasksSeattle', 'RowKey': '005', 'description' : 'Clean hello bathroom', 'priority' : 100}
batch.insert_entity(task004)
batch.insert_entity(task005)
table_service.commit_batch('tasktable', batch)
```

<span data-ttu-id="9c560-144">Batchar kan också användas med hello kontexten manager syntax:</span><span class="sxs-lookup"><span data-stu-id="9c560-144">Batches can also be used with hello context manager syntax:</span></span>

```python
task006 = {'PartitionKey': 'tasksSeattle', 'RowKey': '006', 'description' : 'Go grocery shopping', 'priority' : 400}
task007 = {'PartitionKey': 'tasksSeattle', 'RowKey': '007', 'description' : 'Clean hello bathroom', 'priority' : 100}

with table_service.batch('tasktable') as batch:
    batch.insert_entity(task006)
    batch.insert_entity(task007)
```

## <a name="query-for-an-entity"></a><span data-ttu-id="9c560-145">Frågan för en entitet</span><span class="sxs-lookup"><span data-stu-id="9c560-145">Query for an entity</span></span>

<span data-ttu-id="9c560-146">tooquery för en entitet i en tabell, skickar dess PartitionKey och RowKey toohello [TableService][py_TableService].[ get_entity] [ py_get_entity] metod.</span><span class="sxs-lookup"><span data-stu-id="9c560-146">tooquery for an entity in a table, pass its PartitionKey and RowKey toohello [TableService][py_TableService].[get_entity][py_get_entity] method.</span></span>

```python
task = table_service.get_entity('tasktable', 'tasksSeattle', '001')
print(task.description)
print(task.priority)
```

## <a name="query-a-set-of-entities"></a><span data-ttu-id="9c560-147">Fråga en uppsättning enheter</span><span class="sxs-lookup"><span data-stu-id="9c560-147">Query a set of entities</span></span>

<span data-ttu-id="9c560-148">Du kan fråga efter en uppsättning enheter genom att ange en filtreringssträng med hello **filter** parameter.</span><span class="sxs-lookup"><span data-stu-id="9c560-148">You can query for a set of entities by supplying a filter string with hello **filter** parameter.</span></span> <span data-ttu-id="9c560-149">Det här exemplet hittar du alla aktiviteter i Seattle genom att använda ett filter på PartitionKey:</span><span class="sxs-lookup"><span data-stu-id="9c560-149">This example finds all tasks in Seattle by applying a filter on PartitionKey:</span></span>

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
for task in tasks:
    print(task.description)
    print(task.priority)
```

## <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="9c560-150">Fråga en deluppsättning entitetsegenskaper</span><span class="sxs-lookup"><span data-stu-id="9c560-150">Query a subset of entity properties</span></span>

<span data-ttu-id="9c560-151">Du kan också begränsa vilka egenskaper som returneras för varje entitet i en fråga.</span><span class="sxs-lookup"><span data-stu-id="9c560-151">You can also restrict which properties are returned for each entity in a query.</span></span> <span data-ttu-id="9c560-152">Den här tekniken, kallad *projektion*, minskar bandbredden och kan förbättra frågeprestanda, särskilt för stora entiteter eller resultatmängder.</span><span class="sxs-lookup"><span data-stu-id="9c560-152">This technique, called *projection*, reduces bandwidth and can improve query performance, especially for large entities or result sets.</span></span> <span data-ttu-id="9c560-153">Använd hello **Välj** parameter- och pass hello namnen på hello egenskaper som ska returneras toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="9c560-153">Use hello **select** parameter and pass hello names of hello properties you want returned toohello client.</span></span>

<span data-ttu-id="9c560-154">hello-fråga i hello följande kod returnerar bara hello beskrivningar av entiteter i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="9c560-154">hello query in hello following code returns only hello descriptions of entities in hello table.</span></span>

> [!NOTE]
> <span data-ttu-id="9c560-155">hello följande fragment fungerar bara mot hello Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="9c560-155">hello following snippet works only against hello Azure Storage.</span></span> <span data-ttu-id="9c560-156">Det stöds inte av hello storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="9c560-156">It is not supported by hello storage emulator.</span></span>

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
for task in tasks:
    print(task.description)
```

## <a name="delete-an-entity"></a><span data-ttu-id="9c560-157">Ta bort en entitet</span><span class="sxs-lookup"><span data-stu-id="9c560-157">Delete an entity</span></span>

<span data-ttu-id="9c560-158">Ta bort en enhet genom att ange dess PartitionKey och RowKey toohello [delete_entity] [ py_delete_entity] metod.</span><span class="sxs-lookup"><span data-stu-id="9c560-158">Delete an entity by passing its PartitionKey and RowKey toohello [delete_entity][py_delete_entity] method.</span></span>

```python
table_service.delete_entity('tasktable', 'tasksSeattle', '001')
```

## <a name="delete-a-table"></a><span data-ttu-id="9c560-159">Ta bort en tabell</span><span class="sxs-lookup"><span data-stu-id="9c560-159">Delete a table</span></span>

<span data-ttu-id="9c560-160">Om du inte längre behöver en tabell eller någon av hello enheter inom det anropa hello [delete_table] [ py_delete_table] metoden toopermanently ta bort hello tabell från Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="9c560-160">If you no longer need a table or any of hello entities within it, call hello [delete_table][py_delete_table] method toopermanently delete hello table from Azure Storage.</span></span>

```python
table_service.delete_table('tasktable')
```

## <a name="next-steps"></a><span data-ttu-id="9c560-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9c560-161">Next steps</span></span>

* [<span data-ttu-id="9c560-162">Azure Storage-SDK för Python API-referens</span><span class="sxs-lookup"><span data-stu-id="9c560-162">Azure Storage SDK for Python API reference</span></span>](https://azure-storage.readthedocs.io/en/latest/index.html)
* [<span data-ttu-id="9c560-163">Azure Storage SDK för Python</span><span class="sxs-lookup"><span data-stu-id="9c560-163">Azure Storage SDK for Python</span></span>](https://github.com/Azure/azure-storage-python)
* [<span data-ttu-id="9c560-164">Python Developer Center</span><span class="sxs-lookup"><span data-stu-id="9c560-164">Python Developer Center</span></span>](https://azure.microsoft.com/develop/python/)
* <span data-ttu-id="9c560-165">[Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md): ett kostnadsfritt, plattformsoberoende program för att visuellt arbeta med Azure Storage-data i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="9c560-165">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md): A free, cross-platform application for working visually with Azure Storage data on Windows, macOS, and Linux.</span></span>

[py_commit_batch]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.commit_batch
[py_create_table]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.create_table
[py_delete_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.delete_entity
[py_delete_table]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.delete_table
[py_Entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.models.html#azure.storage.table.models.Entity
[py_get_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.get_entity
[py_insert_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.insert_entity
[py_insert_or_replace_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.insert_or_replace_entity
[py_merge_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.merge_entity
[py_update_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.update_entity
[py_TableService]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html
[py_TableBatch]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tablebatch.html#azure.storage.table.tablebatch.TableBatch
