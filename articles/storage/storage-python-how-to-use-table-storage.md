---
title: "Hur du använder Azure Table storage med Python | Microsoft Docs"
description: "Lagra strukturerade data i molnet med hjälp av Azure Table Storage, en NoSQL-databas."
services: storage
documentationcenter: python
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 7ddb9f3e-4e6d-4103-96e6-f0351d69a17b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/16/2017
ms.author: marsma
ms.openlocfilehash: c310a52182bbc3cf44ed4dc6a04e97aa59200a64
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-table-storage-in-python"></a><span data-ttu-id="bae6c-103">Använda Table storage i Python</span><span class="sxs-lookup"><span data-stu-id="bae6c-103">How to use Table storage in Python</span></span>

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

<span data-ttu-id="bae6c-104">Den här guiden visar hur du utför vanliga scenarier för Azure Table storage i Python med hjälp av den [Microsoft Azure Storage SDK för Python](https://github.com/Azure/azure-storage-python).</span><span class="sxs-lookup"><span data-stu-id="bae6c-104">This guide shows you how to perform common Azure Table storage scenarios in Python using the [Microsoft Azure Storage SDK for Python](https://github.com/Azure/azure-storage-python).</span></span> <span data-ttu-id="bae6c-105">Scenarier som tas upp är hur du skapar och tar bort en tabell och infoga fråga entiteter.</span><span class="sxs-lookup"><span data-stu-id="bae6c-105">The scenarios covered include creating and deleting a table, and inserting and querying entities.</span></span>

<span data-ttu-id="bae6c-106">När du arbetar via scenarier i den här kursen kan du referera till den [Azure Storage SDK för Python API-referens för](https://azure-storage.readthedocs.io/en/latest/index.html).</span><span class="sxs-lookup"><span data-stu-id="bae6c-106">While working through the scenarios in this tutorial, you may wish to refer to the [Azure Storage SDK for Python API reference](https://azure-storage.readthedocs.io/en/latest/index.html).</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="install-the-azure-storage-sdk-for-python"></a><span data-ttu-id="bae6c-107">Installera Azure Storage SDK för Python</span><span class="sxs-lookup"><span data-stu-id="bae6c-107">Install the Azure Storage SDK for Python</span></span>

<span data-ttu-id="bae6c-108">När du har skapat ett lagringskonto, nästa steg är att installera den [Microsoft Azure Storage SDK för Python](https://github.com/Azure/azure-storage-python).</span><span class="sxs-lookup"><span data-stu-id="bae6c-108">Once you've created a storage account, your next step is to install the [Microsoft Azure Storage SDK for Python](https://github.com/Azure/azure-storage-python).</span></span> <span data-ttu-id="bae6c-109">Mer information om hur du installerar SDK finns i den [README.rst](https://github.com/Azure/azure-storage-python/blob/master/README.rst) filen i Storage SDK: N för Python lagringsplats på GitHub.</span><span class="sxs-lookup"><span data-stu-id="bae6c-109">For details on installing the SDK, refer to the [README.rst](https://github.com/Azure/azure-storage-python/blob/master/README.rst) file in the Storage SDK for Python repository on GitHub.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="bae6c-110">Skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="bae6c-110">Create a table</span></span>

<span data-ttu-id="bae6c-111">Om du vill arbeta med Azure Table-tjänsten i Python, måste du importera den [TableService] [ py_TableService] modul.</span><span class="sxs-lookup"><span data-stu-id="bae6c-111">To work with the Azure Table service in Python, you must import the [TableService][py_TableService] module.</span></span> <span data-ttu-id="bae6c-112">Eftersom du arbetar med tabellentiteter, behöver du också den [entiteten] [ py_Entity] klass.</span><span class="sxs-lookup"><span data-stu-id="bae6c-112">Since you'll be working with Table entities, you also need the [Entity][py_Entity] class.</span></span> <span data-ttu-id="bae6c-113">Lägg till den här koden längst upp Python-fil om du vill importera båda:</span><span class="sxs-lookup"><span data-stu-id="bae6c-113">Add this code near the top your Python file to import both:</span></span>

```python
from azure.storage.table import TableService, Entity
```

<span data-ttu-id="bae6c-114">Skapa en [TableService] [ py_TableService] objekt som passerar i din namn och åtkomstnyckel för lagring.</span><span class="sxs-lookup"><span data-stu-id="bae6c-114">Create a [TableService][py_TableService] object, passing in your storage account name and account key.</span></span> <span data-ttu-id="bae6c-115">Ersätt `myaccount` och `mykey` med kontonamnet och nyckeln och anropet [create_table] [ py_create_table] att skapa tabellen i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="bae6c-115">Replace `myaccount` and `mykey` with your account name and key, and call [create_table][py_create_table] to create the table in Azure Storage.</span></span>

```python
table_service = TableService(account_name='myaccount', account_key='mykey')

table_service.create_table('tasktable')
```

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="bae6c-116">Lägga till en entitet i en tabell</span><span class="sxs-lookup"><span data-stu-id="bae6c-116">Add an entity to a table</span></span>

<span data-ttu-id="bae6c-117">Om du vill lägga till en enhet måste du först skapa ett objekt som representerar din enhet och sedan skicka objekt till den [TableService][py_TableService].[ insert_entity] [ py_insert_entity] metod.</span><span class="sxs-lookup"><span data-stu-id="bae6c-117">To add an entity, you first create an object that represents your entity, then pass the object to the [TableService][py_TableService].[insert_entity][py_insert_entity] method.</span></span> <span data-ttu-id="bae6c-118">Entitetsobjektet kan vara en ordlista eller ett objekt av typen [entiteten][py_Entity], och definierar egenskapsnamn och värden för din enhet.</span><span class="sxs-lookup"><span data-stu-id="bae6c-118">The entity object can be a dictionary or an object of type [Entity][py_Entity], and defines your entity's property names and values.</span></span> <span data-ttu-id="bae6c-119">Varje entitet måste innehålla de nödvändiga [PartitionKey och RowKey](#partitionkey-and-rowkey) egenskaper, utöver andra egenskaper som du definierar för entiteten.</span><span class="sxs-lookup"><span data-stu-id="bae6c-119">Every entity must include the required [PartitionKey and RowKey](#partitionkey-and-rowkey) properties, in addition to any other properties you define for the entity.</span></span>

<span data-ttu-id="bae6c-120">Det här exemplet skapar ett katalogobjekt som representerar en entitet sedan skickar det till den [insert_entity] [ py_insert_entity] metod för att lägga till den i tabellen:</span><span class="sxs-lookup"><span data-stu-id="bae6c-120">This example creates a dictionary object representing an entity, then passes it to the [insert_entity][py_insert_entity] method to add it to the table:</span></span>

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the trash', 'priority' : 200}
table_service.insert_entity('tasktable', task)
```

<span data-ttu-id="bae6c-121">Det här exemplet skapas en [entiteten] [ py_Entity] objekt och skickar det till den [insert_entity] [ py_insert_entity] metod för att lägga till den i tabellen:</span><span class="sxs-lookup"><span data-stu-id="bae6c-121">This example creates an [Entity][py_Entity] object, then passes it to the [insert_entity][py_insert_entity] method to add it to the table:</span></span>

```python
task = Entity()
task.PartitionKey = 'tasksSeattle'
task.RowKey = '002'
task.description = 'Wash the car'
task.priority = 100
table_service.insert_entity('tasktable', task)
```

### <a name="partitionkey-and-rowkey"></a><span data-ttu-id="bae6c-122">PartitionKey och RowKey</span><span class="sxs-lookup"><span data-stu-id="bae6c-122">PartitionKey and RowKey</span></span>

<span data-ttu-id="bae6c-123">Du måste ange både en **PartitionKey** och en **RowKey** egenskap för varje entitet.</span><span class="sxs-lookup"><span data-stu-id="bae6c-123">You must specify both a **PartitionKey** and a **RowKey** property for every entity.</span></span> <span data-ttu-id="bae6c-124">Dessa är de unika identifierarna på entiteter, som tillsammans de bildar primärnyckel för en entitet.</span><span class="sxs-lookup"><span data-stu-id="bae6c-124">These are the unique identifiers of your entities, as together they form the primary key of an entity.</span></span> <span data-ttu-id="bae6c-125">Du kan fråga med hjälp av dessa värden som är mycket snabbare än du kan fråga andra Entitetsegenskaper eftersom endast dessa egenskaper indexeras.</span><span class="sxs-lookup"><span data-stu-id="bae6c-125">You can query using these values much faster than you can query any other entity properties because only these properties are indexed.</span></span>

<span data-ttu-id="bae6c-126">Tjänsten använder tabellen **PartitionKey** Intelligent distribuerar tabellentiteter över lagringsnoder.</span><span class="sxs-lookup"><span data-stu-id="bae6c-126">The Table service uses **PartitionKey** to intelligently distribute table entities across storage nodes.</span></span> <span data-ttu-id="bae6c-127">Enheter som har samma **PartitionKey** lagras på samma nod.</span><span class="sxs-lookup"><span data-stu-id="bae6c-127">Entities that have the same  **PartitionKey** are stored on the same node.</span></span> <span data-ttu-id="bae6c-128">**RowKey** är unikt ID för entiteten i partitionen som den tillhör.</span><span class="sxs-lookup"><span data-stu-id="bae6c-128">**RowKey** is the unique ID of the entity within the partition it belongs to.</span></span>

## <a name="update-an-entity"></a><span data-ttu-id="bae6c-129">Uppdatera en entitet</span><span class="sxs-lookup"><span data-stu-id="bae6c-129">Update an entity</span></span>

<span data-ttu-id="bae6c-130">Om du vill uppdatera alla egenskapsvärden för en entitet, anropa den [update_entity] [ py_update_entity] metod.</span><span class="sxs-lookup"><span data-stu-id="bae6c-130">To update all of an entity's property values, call the [update_entity][py_update_entity] method.</span></span> <span data-ttu-id="bae6c-131">Det här exemplet visar hur du ersätter en befintlig entitet med en uppdaterad version:</span><span class="sxs-lookup"><span data-stu-id="bae6c-131">This example shows how to replace an existing entity with an updated version:</span></span>

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the garbage', 'priority' : 250}
table_service.update_entity('tasktable', task)
```

<span data-ttu-id="bae6c-132">Uppdateringen misslyckas om entiteten som ska uppdateras inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="bae6c-132">If the entity that is being updated doesn't already exist, then the update operation will fail.</span></span> <span data-ttu-id="bae6c-133">Om du vill lagra en entitet om den finns eller inte kan använda [insert_or_replace_entity][py_insert_or_replace_entity].</span><span class="sxs-lookup"><span data-stu-id="bae6c-133">If you want to store an entity whether it exists or not, use [insert_or_replace_entity][py_insert_or_replace_entity].</span></span> <span data-ttu-id="bae6c-134">I följande exempel är ersätter det första anropet befintliga entiteten.</span><span class="sxs-lookup"><span data-stu-id="bae6c-134">In the following example, the first call will replace the existing entity.</span></span> <span data-ttu-id="bae6c-135">Det andra anropet ska infoga en ny enhet, eftersom ingen entitet med angivna PartitionKey och RowKey finns i tabellen.</span><span class="sxs-lookup"><span data-stu-id="bae6c-135">The second call will insert a new entity, since no entity with the specified PartitionKey and RowKey exists in the table.</span></span>

```python
# Replace the entity created earlier
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the garbage again', 'priority' : 250}
table_service.insert_or_replace_entity('tasktable', task)

# Insert a new entity
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '003', 'description' : 'Buy detergent', 'priority' : 300}
table_service.insert_or_replace_entity('tasktable', task)
```

> [!TIP]
> <span data-ttu-id="bae6c-136">Den [update_entity] [ py_update_entity] metoden ersätter alla egenskaper och värden för en befintlig enhet där du kan också ta bort egenskaper från en befintlig entitet.</span><span class="sxs-lookup"><span data-stu-id="bae6c-136">The [update_entity][py_update_entity] method replaces all properties and values of an existing entity, which you can also use to remove properties from an existing entity.</span></span> <span data-ttu-id="bae6c-137">Du kan använda den [merge_entity] [ py_merge_entity] metod för att uppdatera en befintlig entitet med nya eller ändrade egenskapsvärden utan att helt ersätta entiteten.</span><span class="sxs-lookup"><span data-stu-id="bae6c-137">You can use the [merge_entity][py_merge_entity] method to update an existing entity with new or modified property values without completely replacing the entity.</span></span>

## <a name="modify-multiple-entities"></a><span data-ttu-id="bae6c-138">Ändra flera entiteter</span><span class="sxs-lookup"><span data-stu-id="bae6c-138">Modify multiple entities</span></span>

<span data-ttu-id="bae6c-139">Du kan skicka flera åtgärder tillsammans i en grupp för att säkerställa atomiska bearbetningen av en begäran av tabelltjänsten.</span><span class="sxs-lookup"><span data-stu-id="bae6c-139">To ensure the atomic processing of a request by the Table service, you can submit multiple operations together in a batch.</span></span> <span data-ttu-id="bae6c-140">Använd först den [TableBatch] [ py_TableBatch] klassen om du vill lägga till flera åtgärder i en enskild batch.</span><span class="sxs-lookup"><span data-stu-id="bae6c-140">First, use the [TableBatch][py_TableBatch] class to add multiple operations to a single batch.</span></span> <span data-ttu-id="bae6c-141">Därefter anropar [TableService][py_TableService].[ commit_batch] [ py_commit_batch] att skicka åtgärder i en atomisk åtgärd.</span><span class="sxs-lookup"><span data-stu-id="bae6c-141">Next, call [TableService][py_TableService].[commit_batch][py_commit_batch] to submit the operations in an atomic operation.</span></span> <span data-ttu-id="bae6c-142">Alla enheter som ska ändras i batchen måste finnas i samma partition.</span><span class="sxs-lookup"><span data-stu-id="bae6c-142">All entities to be modified in batch must be in the same partition.</span></span>

<span data-ttu-id="bae6c-143">Det här exemplet lägger till två entiteter tillsammans i en batch:</span><span class="sxs-lookup"><span data-stu-id="bae6c-143">This example adds two entities together in a batch:</span></span>

```python
from azure.storage.table import TableBatch
batch = TableBatch()
task004 = {'PartitionKey': 'tasksSeattle', 'RowKey': '004', 'description' : 'Go grocery shopping', 'priority' : 400}
task005 = {'PartitionKey': 'tasksSeattle', 'RowKey': '005', 'description' : 'Clean the bathroom', 'priority' : 100}
batch.insert_entity(task004)
batch.insert_entity(task005)
table_service.commit_batch('tasktable', batch)
```

<span data-ttu-id="bae6c-144">Batchar kan också användas med kontexten manager syntax:</span><span class="sxs-lookup"><span data-stu-id="bae6c-144">Batches can also be used with the context manager syntax:</span></span>

```python
task006 = {'PartitionKey': 'tasksSeattle', 'RowKey': '006', 'description' : 'Go grocery shopping', 'priority' : 400}
task007 = {'PartitionKey': 'tasksSeattle', 'RowKey': '007', 'description' : 'Clean the bathroom', 'priority' : 100}

with table_service.batch('tasktable') as batch:
    batch.insert_entity(task006)
    batch.insert_entity(task007)
```

## <a name="query-for-an-entity"></a><span data-ttu-id="bae6c-145">Frågan för en entitet</span><span class="sxs-lookup"><span data-stu-id="bae6c-145">Query for an entity</span></span>

<span data-ttu-id="bae6c-146">Om du vill fråga för en entitet i en tabell, skickar dess PartitionKey och RowKey till den [TableService][py_TableService].[ get_entity] [ py_get_entity] metod.</span><span class="sxs-lookup"><span data-stu-id="bae6c-146">To query for an entity in a table, pass its PartitionKey and RowKey to the [TableService][py_TableService].[get_entity][py_get_entity] method.</span></span>

```python
task = table_service.get_entity('tasktable', 'tasksSeattle', '001')
print(task.description)
print(task.priority)
```

## <a name="query-a-set-of-entities"></a><span data-ttu-id="bae6c-147">Fråga en uppsättning enheter</span><span class="sxs-lookup"><span data-stu-id="bae6c-147">Query a set of entities</span></span>

<span data-ttu-id="bae6c-148">Du kan fråga efter en uppsättning enheter genom att ange en filtreringssträng med den **filter** parameter.</span><span class="sxs-lookup"><span data-stu-id="bae6c-148">You can query for a set of entities by supplying a filter string with the **filter** parameter.</span></span> <span data-ttu-id="bae6c-149">Det här exemplet hittar du alla aktiviteter i Seattle genom att använda ett filter på PartitionKey:</span><span class="sxs-lookup"><span data-stu-id="bae6c-149">This example finds all tasks in Seattle by applying a filter on PartitionKey:</span></span>

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
for task in tasks:
    print(task.description)
    print(task.priority)
```

## <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="bae6c-150">Fråga en deluppsättning entitetsegenskaper</span><span class="sxs-lookup"><span data-stu-id="bae6c-150">Query a subset of entity properties</span></span>

<span data-ttu-id="bae6c-151">Du kan också begränsa vilka egenskaper som returneras för varje entitet i en fråga.</span><span class="sxs-lookup"><span data-stu-id="bae6c-151">You can also restrict which properties are returned for each entity in a query.</span></span> <span data-ttu-id="bae6c-152">Den här tekniken, kallad *projektion*, minskar bandbredden och kan förbättra frågeprestanda, särskilt för stora entiteter eller resultatmängder.</span><span class="sxs-lookup"><span data-stu-id="bae6c-152">This technique, called *projection*, reduces bandwidth and can improve query performance, especially for large entities or result sets.</span></span> <span data-ttu-id="bae6c-153">Använd den **Välj** parameter- och pass namnen på egenskaperna som returneras till klienten.</span><span class="sxs-lookup"><span data-stu-id="bae6c-153">Use the **select** parameter and pass the names of the properties you want returned to the client.</span></span>

<span data-ttu-id="bae6c-154">Frågan i följande kod returnerar bara beskrivningar av entiteter i tabellen.</span><span class="sxs-lookup"><span data-stu-id="bae6c-154">The query in the following code returns only the descriptions of entities in the table.</span></span>

> [!NOTE]
> <span data-ttu-id="bae6c-155">Följande kodavsnitt fungerar bara mot Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="bae6c-155">The following snippet works only against the Azure Storage.</span></span> <span data-ttu-id="bae6c-156">Det stöds inte av storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="bae6c-156">It is not supported by the storage emulator.</span></span>

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
for task in tasks:
    print(task.description)
```

## <a name="delete-an-entity"></a><span data-ttu-id="bae6c-157">Ta bort en entitet</span><span class="sxs-lookup"><span data-stu-id="bae6c-157">Delete an entity</span></span>

<span data-ttu-id="bae6c-158">Ta bort en enhet genom att ange dess PartitionKey och RowKey till den [delete_entity] [ py_delete_entity] metod.</span><span class="sxs-lookup"><span data-stu-id="bae6c-158">Delete an entity by passing its PartitionKey and RowKey to the [delete_entity][py_delete_entity] method.</span></span>

```python
table_service.delete_entity('tasktable', 'tasksSeattle', '001')
```

## <a name="delete-a-table"></a><span data-ttu-id="bae6c-159">Ta bort en tabell</span><span class="sxs-lookup"><span data-stu-id="bae6c-159">Delete a table</span></span>

<span data-ttu-id="bae6c-160">Om du inte längre behöver en tabell eller någon av enheterna i den anropar den [delete_table] [ py_delete_table] metod för att ta bort tabellen från Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="bae6c-160">If you no longer need a table or any of the entities within it, call the [delete_table][py_delete_table] method to permanently delete the table from Azure Storage.</span></span>

```python
table_service.delete_table('tasktable')
```

## <a name="next-steps"></a><span data-ttu-id="bae6c-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bae6c-161">Next steps</span></span>

* [<span data-ttu-id="bae6c-162">Azure Storage-SDK för Python API-referens</span><span class="sxs-lookup"><span data-stu-id="bae6c-162">Azure Storage SDK for Python API reference</span></span>](https://azure-storage.readthedocs.io/en/latest/index.html)
* [<span data-ttu-id="bae6c-163">Azure Storage SDK för Python</span><span class="sxs-lookup"><span data-stu-id="bae6c-163">Azure Storage SDK for Python</span></span>](https://github.com/Azure/azure-storage-python)
* [<span data-ttu-id="bae6c-164">Python Developer Center</span><span class="sxs-lookup"><span data-stu-id="bae6c-164">Python Developer Center</span></span>](https://azure.microsoft.com/develop/python/)
* <span data-ttu-id="bae6c-165">[Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md): ett kostnadsfritt, plattformsoberoende program för att visuellt arbeta med Azure Storage-data i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="bae6c-165">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md): A free, cross-platform application for working visually with Azure Storage data on Windows, macOS, and Linux.</span></span>

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
