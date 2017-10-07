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
# <a name="how-toouse-table-storage-in-python"></a>Hur toouse tabellagring i Python

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

Den här guiden visar hur tooperform vanliga Azure Table storage scenarier i Python med hello [Microsoft Azure Storage SDK för Python](https://github.com/Azure/azure-storage-python). hello Lär dig bland annat hur du skapar och tar bort en tabell och infoga fråga entiteter.

När du arbetar via hello scenarier i den här kursen får du vill toorefer toohello [Azure Storage SDK för Python API-referens för](https://azure-storage.readthedocs.io/en/latest/index.html).

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="install-hello-azure-storage-sdk-for-python"></a>Installera hello Azure Storage SDK för Python

När du har skapat ett lagringskonto, nästa steg är tooinstall hello [Microsoft Azure Storage SDK för Python](https://github.com/Azure/azure-storage-python). För information om hur du installerar hello SDK, se toohello [README.rst](https://github.com/Azure/azure-storage-python/blob/master/README.rst) filen i hello Storage SDK för Python lagringsplats på GitHub.

## <a name="create-a-table"></a>Skapa en tabell

toowork med hello Azure Table-tjänsten i Python, måste du importera hello [TableService] [ py_TableService] modul. Eftersom du arbetar med tabellentiteter, måste du också hello [entiteten] [ py_Entity] klass. Lägg till den här koden hello övre delen din Python filen tooimport båda:

```python
from azure.storage.table import TableService, Entity
```

Skapa en [TableService] [ py_TableService] objekt som passerar i din namn och åtkomstnyckel för lagring. Ersätt `myaccount` och `mykey` med kontonamnet och nyckeln och anropet [create_table] [ py_create_table] toocreate hello tabell i Azure Storage.

```python
table_service = TableService(account_name='myaccount', account_key='mykey')

table_service.create_table('tasktable')
```

## <a name="add-an-entity-tooa-table"></a>Lägg till en entitet tooa tabell

tooadd en entitet du först skapa ett objekt som representerar din enhet, sedan pass hello objektet toohello [TableService][py_TableService].[ insert_entity] [ py_insert_entity] metod. hello enhetsobjekt kan vara en ordlista eller ett objekt av typen [entiteten][py_Entity], och definierar egenskapsnamn och värden för din enhet. Varje entitet måste innehålla hello krävs [PartitionKey och RowKey](#partitionkey-and-rowkey) egenskaper i tillägg tooany andra egenskaper du definierar för hello entitet.

Det här exemplet skapar ett katalogobjekt som representerar en entitet skickar sedan den toohello [insert_entity] [ py_insert_entity] metoden tooadd den toohello tabellen:

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out hello trash', 'priority' : 200}
table_service.insert_entity('tasktable', task)
```

Det här exemplet skapas en [entiteten] [ py_Entity] objekt och skickar den toohello [insert_entity] [ py_insert_entity] metoden tooadd den toohello tabellen:

```python
task = Entity()
task.PartitionKey = 'tasksSeattle'
task.RowKey = '002'
task.description = 'Wash hello car'
task.priority = 100
table_service.insert_entity('tasktable', task)
```

### <a name="partitionkey-and-rowkey"></a>PartitionKey och RowKey

Du måste ange både en **PartitionKey** och en **RowKey** egenskap för varje entitet. Dessa är hello unika identifierare för dina enheter, som tillsammans de bildar hello primärnyckel för en entitet. Du kan fråga med hjälp av dessa värden som är mycket snabbare än du kan fråga andra Entitetsegenskaper eftersom endast dessa egenskaper indexeras.

Hej tabell tjänsten använder **PartitionKey** toointelligently distribuerar tabellentiteter över lagringsnoder. Enheter som har hello samma **PartitionKey** lagras på hello samma nod. **RowKey** är hello unikt ID för hello entiteten inom hello-partition som den tillhör.

## <a name="update-an-entity"></a>Uppdatera en entitet

tooupdate alla egenskapsvärden för en entitet, anropa hello [update_entity] [ py_update_entity] metod. Det här exemplet illustrerar hur tooreplace en befintlig entitet med en uppdaterad version:

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out hello garbage', 'priority' : 250}
table_service.update_entity('tasktable', task)
```

Om hello entitet som ska uppdateras inte redan finns kommer hello uppdateringen misslyckas. Om du vill toostore en entitet om den finns eller inte använda [insert_or_replace_entity][py_insert_or_replace_entity]. I följande exempel hello, ersätter hello första anropet befintliga hello-entiteten. hello andra anropet infogar en ny entitet, eftersom ingen entitet med hello angetts PartitionKey och RowKey finns i hello tabellen.

```python
# Replace hello entity created earlier
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out hello garbage again', 'priority' : 250}
table_service.insert_or_replace_entity('tasktable', task)

# Insert a new entity
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '003', 'description' : 'Buy detergent', 'priority' : 300}
table_service.insert_or_replace_entity('tasktable', task)
```

> [!TIP]
> Hej [update_entity] [ py_update_entity] metoden ersätter alla egenskaper och värdena för en befintlig enhet som du kan också använda tooremove egenskaper från en befintlig entitet. Du kan använda hello [merge_entity] [ py_merge_entity] metoden tooupdate en befintlig entitet med nya eller ändrade egenskapsvärden utan att helt ersätta hello entitet.

## <a name="modify-multiple-entities"></a>Ändra flera entiteter

tooensure Hej atomiska bearbetningen av en begäran av hello tabelltjänsten, kan du skicka flera åtgärder tillsammans i en batch. Använd först hello [TableBatch] [ py_TableBatch] klassen tooadd flera åtgärder tooa enskild batch. Därefter anropar [TableService][py_TableService].[ commit_batch] [ py_commit_batch] toosubmit hello åtgärder i en atomisk åtgärd. Alla entiteter toobe ändras i batchen måste vara i hello samma partition.

Det här exemplet lägger till två entiteter tillsammans i en batch:

```python
from azure.storage.table import TableBatch
batch = TableBatch()
task004 = {'PartitionKey': 'tasksSeattle', 'RowKey': '004', 'description' : 'Go grocery shopping', 'priority' : 400}
task005 = {'PartitionKey': 'tasksSeattle', 'RowKey': '005', 'description' : 'Clean hello bathroom', 'priority' : 100}
batch.insert_entity(task004)
batch.insert_entity(task005)
table_service.commit_batch('tasktable', batch)
```

Batchar kan också användas med hello kontexten manager syntax:

```python
task006 = {'PartitionKey': 'tasksSeattle', 'RowKey': '006', 'description' : 'Go grocery shopping', 'priority' : 400}
task007 = {'PartitionKey': 'tasksSeattle', 'RowKey': '007', 'description' : 'Clean hello bathroom', 'priority' : 100}

with table_service.batch('tasktable') as batch:
    batch.insert_entity(task006)
    batch.insert_entity(task007)
```

## <a name="query-for-an-entity"></a>Frågan för en entitet

tooquery för en entitet i en tabell, skickar dess PartitionKey och RowKey toohello [TableService][py_TableService].[ get_entity] [ py_get_entity] metod.

```python
task = table_service.get_entity('tasktable', 'tasksSeattle', '001')
print(task.description)
print(task.priority)
```

## <a name="query-a-set-of-entities"></a>Fråga en uppsättning enheter

Du kan fråga efter en uppsättning enheter genom att ange en filtreringssträng med hello **filter** parameter. Det här exemplet hittar du alla aktiviteter i Seattle genom att använda ett filter på PartitionKey:

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
for task in tasks:
    print(task.description)
    print(task.priority)
```

## <a name="query-a-subset-of-entity-properties"></a>Fråga en deluppsättning entitetsegenskaper

Du kan också begränsa vilka egenskaper som returneras för varje entitet i en fråga. Den här tekniken, kallad *projektion*, minskar bandbredden och kan förbättra frågeprestanda, särskilt för stora entiteter eller resultatmängder. Använd hello **Välj** parameter- och pass hello namnen på hello egenskaper som ska returneras toohello klienten.

hello-fråga i hello följande kod returnerar bara hello beskrivningar av entiteter i hello tabell.

> [!NOTE]
> hello följande fragment fungerar bara mot hello Azure Storage. Det stöds inte av hello storage-emulatorn.

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
for task in tasks:
    print(task.description)
```

## <a name="delete-an-entity"></a>Ta bort en entitet

Ta bort en enhet genom att ange dess PartitionKey och RowKey toohello [delete_entity] [ py_delete_entity] metod.

```python
table_service.delete_entity('tasktable', 'tasksSeattle', '001')
```

## <a name="delete-a-table"></a>Ta bort en tabell

Om du inte längre behöver en tabell eller någon av hello enheter inom det anropa hello [delete_table] [ py_delete_table] metoden toopermanently ta bort hello tabell från Azure Storage.

```python
table_service.delete_table('tasktable')
```

## <a name="next-steps"></a>Nästa steg

* [Azure Storage-SDK för Python API-referens](https://azure-storage.readthedocs.io/en/latest/index.html)
* [Azure Storage SDK för Python](https://github.com/Azure/azure-storage-python)
* [Python Developer Center](https://azure.microsoft.com/develop/python/)
* [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md): ett kostnadsfritt, plattformsoberoende program för att visuellt arbeta med Azure Storage-data i Windows, macOS och Linux.

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
