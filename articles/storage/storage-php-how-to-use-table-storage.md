---
title: "aaaHow toouse table storage från PHP | Microsoft Docs"
description: "Lär dig hur toouse hello tabelltjänsten från PHP toocreate och ta bort en tabell,- och insert-, delete- och hello frågetabellen."
services: storage
documentationcenter: php
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 1e57f371-6208-4753-b2a0-05db4aede8e3
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: 1e1036118e208280b4c205da7d7eea61e79359c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-table-storage-from-php"></a>Hur toouse table storage från PHP
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Översikt
Den här guiden visar hur tooperform vanliga scenarier med hjälp av hello Azure Table-tjänsten. hello exemplen är skrivna i PHP och använder hello [Azure SDK för PHP][download]. hello beskrivs scenarier där **skapar och tar bort en tabell och lägga till, ta bort och frågar entiteter i en tabell**. Mer information om hello Azure Table-tjänsten finns hello [nästa steg](#next-steps) avsnitt.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Skapa en PHP-program
hello endast hello är ett krav för att skapa en PHP-program som ansluter till hello Azure Table tjänst refererar till klasser i hello Azure SDK för PHP från inom din kod. Du kan använda alla development tools toocreate ditt program, inklusive anteckningar.

I den här guiden kan du använda tabellen tjänstens funktioner som kan anropas från ett PHP-program lokalt eller i koden körs i en Azure-webbroll, en arbetsroll eller en webbplats.

## <a name="get-hello-azure-client-libraries"></a>Hämta hello Azure-klientbibliotek
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-tooaccess-hello-table-service"></a>Konfigurera program för tooaccess hello tabelltjänst
toouse hello Azure Table service API: er, måste du:

1. Referens hello bandladdaren fil med hello [require_once] [ require_once] -instruktionen och
2. Referera till alla klasser som du kan använda.

hello följande exempel visas hur tooinclude hello bandladdaren fil- och hello **ServicesBuilder** klass.

> [!NOTE]
> hello exemplen i den här artikeln förutsätter att du har installerat hello PHP-klientbibliotek för Azure via Composer. Om du har installerat hello bibliotek manuellt måste tooreference hello <code>WindowsAzure.php</code> bandladdaren filen.
>
>

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

I hello exemplen nedan hello `require_once` instruktionen visas alltid, men bara hello klasser krävs för hello exempel tooexecute refererar till.

## <a name="set-up-an-azure-storage-connection"></a>Skapa en Azure storage-anslutning
tooinstantiate klienten en Azure Table-tjänsten måste du först ha en giltig anslutningssträng. hello-formatet för hello anslutningssträng för tabellen är:

För att komma åt en live-tjänst:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

För att komma åt hello emulatorn lagring:

```php
UseDevelopmentStorage=true
```

toocreate alla Azure-tjänst-klienter, behöver du toouse hello **ServicesBuilder** klass. Du kan:

* Skicka hello anslutning direkt string tooit eller
* Använd hello **CloudConfigurationManager (CCM)** toocheck flera externa datakällor för hello anslutningssträngen:
  * som standard levereras med stöd för en extern källa - miljövariabler
  * Du kan lägga till nya källor genom att utöka hello **ConnectionStringSource** klass

Hello-exempel som beskrivs här skickas hello anslutningssträngen direkt.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);
```

## <a name="create-a-table"></a>Skapa en tabell
En **TableRestProxy** objekt kan du skapa en tabell med hello **createTable** metod. När du skapar en tabell kan ange du hello tabellen service-timeout. (Mer information om hello tabellen service-timeout finns [inställningen tidsgränser för tabellen tjänståtgärder][table-service-timeouts].)

```php
require_once 'vendor\autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    // Create table.
    $tableRestProxy->createTable("mytable");
}
catch(ServiceException $e){
    $code = $e->getCode();
    $error_message = $e->getMessage();
    // Handle exception based on error codes and messages.
    // Error codes and messages can be found here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
}
```

Mer information om begränsningar för tabellnamn finns [hello förstå tabelltjänst-datamodellen][table-data-model].

## <a name="add-an-entity-tooa-table"></a>Lägg till en entitet tooa tabell
tooadd en entitet tooa tabell, skapa en ny **entiteten** objekt och skickar den för**TableRestProxy -> insertEntity**. Observera att när du skapar en entitet, måste du ange en `PartitionKey` och `RowKey`. Dessa är hello unika identifierare för en entitet och är värden som kan efterfrågas mycket snabbare än andra entitetsegenskaper. hello används `PartitionKey` tooautomatically distribuera hello tabellentiteter över många lagringsnoder. Entiteter med hello samma `PartitionKey` lagras på hello samma nod. (Åtgärder på flera entiteter som lagras på samma nod utför hello bättre än på entiteter som lagras på olika noder.) Hej `RowKey` är hello unikt ID för en entitet i en partition.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$entity = new Entity();
$entity->setPartitionKey("tasksSeattle");
$entity->setRowKey("1");
$entity->addProperty("Description", null, "Take out hello trash.");
$entity->addProperty("DueDate",
                        EdmType::DATETIME,
                        new DateTime("2012-11-05T08:15:00-08:00"));
$entity->addProperty("Location", EdmType::STRING, "Home");

try{
    $tableRestProxy->insertEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
}
```

Information om Tabellegenskaper och typer finns [hello förstå tabelltjänst-datamodellen][table-data-model].

Hej **TableRestProxy** klass erbjuder två alternativa metoder för att lägga till entiteter: **insertOrMergeEntity** och **insertOrReplaceEntity**. toouse metoderna, skapa en ny **entiteten** och skicka den som en parameter tooeither metod. Varje metod infogar hello entiteten om den inte finns. Om hello entitet redan **insertOrMergeEntity** uppdaterar egenskapsvärden om hello egenskaper redan finns och lägger till nya egenskaper om det inte finns, medan **insertOrReplaceEntity** helt ersätter en befintlig entitet. följande exempel visar hur hello toouse **insertOrMergeEntity**. Om hello entitet med `PartitionKey` ”tasksSeattle” och `RowKey` ”1” inte redan finns, kommer att infogas. Men om det har tidigare infogats (som visas i hello-exemplet ovan), hello `DueDate` egenskapen kommer att uppdateras och hello `Status` egenskapen kommer att läggas till. Hej `Description` och `Location` egenskaper har också uppdaterats, men med värden som effektivt låt dem vara oförändrad. Om dessa senare två egenskaper inte har lagts till som visas i hello exempel, men fanns på hello målentiteten, skulle befintliga värden ändras inte.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

//Create new entity.
$entity = new Entity();

// PartitionKey and RowKey are required.
$entity->setPartitionKey("tasksSeattle");
$entity->setRowKey("1");

// If entity exists, existing properties are updated with new values and
// new properties are added. Missing properties are unchanged.
$entity->addProperty("Description", null, "Take out hello trash.");
$entity->addProperty("DueDate", EdmType::DATETIME, new DateTime()); // Modified hello DueDate field.
$entity->addProperty("Location", EdmType::STRING, "Home");
$entity->addProperty("Status", EdmType::STRING, "Complete"); // Added Status field.

try    {
    // Calling insertOrReplaceEntity, instead of insertOrMergeEntity as shown,
    // would simply replace hello entity with PartitionKey "tasksSeattle" and RowKey "1".
    $tableRestProxy->insertOrMergeEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="retrieve-a-single-entity"></a>Hämta en enda entitet
Hej **TableRestProxy -> getEntity** metod kan du tooretrieve en enda enhet genom att fråga om dess `PartitionKey` och `RowKey`. I hello exemplet nedan hello partitionsnyckel `tasksSeattle` och radnyckel `1` skickas toohello **getEntity** metod.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entity = $result->getEntity();

echo $entity->getPartitionKey().":".$entity->getRowKey();
```

## <a name="retrieve-all-entities-in-a-partition"></a>Hämta alla entiteter i en partition
Entiteten frågor har skapats med hjälp av filter (Mer information finns i [frågor till tabeller och de entiteter][filters]). tooretrieve alla entiteter i partitionen, använder hello filter ”PartitionKey eq *partitionsnamn*”. följande exempel visar hur hello tooretrieve alla entiteter i hello `tasksSeattle` partition genom att skicka ett filter toohello **queryEntities** metod.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$filter = "PartitionKey eq 'tasksSeattle'";

try    {
    $result = $tableRestProxy->queryEntities("mytable", $filter);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entities = $result->getEntities();

foreach($entities as $entity){
    echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
}
```

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a>Hämta en delmängd av entiteter i en partition
Hej samma mönster som används i hello föregående exempel kan vara används tooretrieve en delmängd av entiteter i en partition. hello delmängd av du hämta entiteter bestäms av hello filter som du använder (Mer information finns i [frågor till tabeller och de entiteter][filters]) .hello som följande exempel visar hur toouse tooretrieve ett filter alla entiteter med ett visst `Location` och en `DueDate` mindre än det angivna datumet.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$filter = "Location eq 'Office' and DueDate lt '2012-11-5'";

try    {
    $result = $tableRestProxy->queryEntities("mytable", $filter);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entities = $result->getEntities();

foreach($entities as $entity){
    echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
}
```

## <a name="retrieve-a-subset-of-entity-properties"></a>Hämta en deluppsättning Entitetsegenskaper
En fråga kan hämta en deluppsättning entitetsegenskaper. Den här tekniken, kallad *projektion*, minskar bandbredden och kan förbättra frågeprestanda, särskilt för stora entiteter. toospecify en egenskapen toobe hämtas, skicka hello namnet på hello egenskapen toohello **addSelectField ->** metod. Du kan anropa metoden flera gånger tooadd fler egenskaper. Efter körning **TableRestProxy -> queryEntities**, hello returnerade entiteter har endast hello valda egenskaper. (Om du vill tooreturn en delmängd av tabellentiteter kan använda ett filter som visas i hello frågor ovan.)

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\QueryEntitiesOptions;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$options = new QueryEntitiesOptions();
$options->addSelectField("Description");

try    {
    $result = $tableRestProxy->queryEntities("mytable", $options);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

// All entities in hello table are returned, regardless of whether
// they have hello Description field.
// toolimit hello results returned, use a filter.
$entities = $result->getEntities();

foreach($entities as $entity){
    $description = $entity->getProperty("Description")->getValue();
    echo $description."<br />";
}
```

## <a name="update-an-entity"></a>Uppdatera en entitet
En befintlig entitet kan uppdateras med hjälp av hello **entitet -> setProperty** och **entitet -> addProperty** metoder i hello entitet och sedan anropar **TableRestProxy -> updateEntity** . hello följande exempel hämtar en entitet, ändrar en egenskap, tar bort en annan egenskap och lägger till en ny egenskap. Observera att du tar bort en egenskap genom att ange värdet för**null**.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);

$entity = $result->getEntity();

$entity->setPropertyValue("DueDate", new DateTime()); //Modified DueDate.

$entity->setPropertyValue("Location", null); //Removed Location.

$entity->addProperty("Status", EdmType::STRING, "In progress"); //Added Status.

try    {
    $tableRestProxy->updateEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="delete-an-entity"></a>Ta bort en entitet
toodelete en entitet skicka hello tabellnamn och hello entitet `PartitionKey` och `RowKey` toohello **TableRestProxy -> deleteEntity** metod.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    // Delete entity.
    $tableRestProxy->deleteEntity("mytable", "tasksSeattle", "2");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Observera att du kan ange hello Etag för en entitet toobe bort med hjälp av hello samtidighet kontroller **DeleteEntityOptions -> setEtag** metod och skicka hello **DeleteEntityOptions** objekt för**deleteEntity** som en fjärde parameter.

## <a name="batch-table-operations"></a>Batchtabeller
Hej **TableRestProxy -> batch** metod kan du tooexecute flera åtgärder i en enskild begäran. hello mönster här måste du lägga till åtgärder för**BatchRequest** objekt och sedan överföra hello **BatchRequest** objekt toohello **TableRestProxy -> batch** metod. tooadd en åtgärden tooa **BatchRequest** objekt, kan du anropa någon av följande metoder flera gånger hello:

* **addInsertEntity** (lägger till en insertEntity åtgärd)
* **addUpdateEntity** (lägger till en updateEntity åtgärd)
* **addMergeEntity** (lägger till en åtgärd för mergeEntity)
* **addInsertOrReplaceEntity** (lägger till en insertOrReplaceEntity åtgärd)
* **addInsertOrMergeEntity** (lägger till en insertOrMergeEntity åtgärd)
* **addDeleteEntity** (lägger till en åtgärd för deleteEntity)

följande exempel visar hur hello tooexecute **insertEntity** och **deleteEntity** åtgärder i en enskild begäran:

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;
use MicrosoftAzure\Storage\Table\Models\BatchOperations;

    // Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

// Create list of batch operation.
$operations = new BatchOperations();

$entity1 = new Entity();
$entity1->setPartitionKey("tasksSeattle");
$entity1->setRowKey("2");
$entity1->addProperty("Description", null, "Clean roof gutters.");
$entity1->addProperty("DueDate",
                        EdmType::DATETIME,
                        new DateTime("2012-11-05T08:15:00-08:00"));
$entity1->addProperty("Location", EdmType::STRING, "Home");

// Add operation toolist of batch operations.
$operations->addInsertEntity("mytable", $entity1);

// Add operation toolist of batch operations.
$operations->addDeleteEntity("mytable", "tasksSeattle", "1");

try    {
    $tableRestProxy->batch($operations);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Mer information om batchbearbetning tabellåtgärder finns [utför entitet gruppera transaktioner][entity-group-transactions].

## <a name="delete-a-table"></a>Ta bort en tabell
Slutligen toodelete en tabell, skicka hello tabellen namnet toohello **TableRestProxy -> deleteTable** metod.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    // Delete table.
    $tableRestProxy->deleteTable("mytable");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig hello grunderna i hello Azure Table-tjänsten, följa dessa länkar toolearn om mer komplexa lagringsuppgifter.

* [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en gratis, fristående app från Microsoft som möjliggör toowork visuellt med Azure Storage-data i Windows, macOS och Linux.

* [PHP Developer Center](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx
