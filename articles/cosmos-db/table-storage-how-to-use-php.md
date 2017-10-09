---
title: "aaaHow toouse table storage från PHP | Microsoft Docs"
description: "Lär dig hur toouse hello tabelltjänsten från PHP toocreate och ta bort en tabell,- och insert-, delete- och hello frågetabellen."
services: cosmos-db
documentationcenter: php
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 1e57f371-6208-4753-b2a0-05db4aede8e3
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.openlocfilehash: 5b7c92221069d1c2a6ca951c06ae8eea8bb8478c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-table-storage-from-php"></a><span data-ttu-id="22a47-103">Hur toouse table storage från PHP</span><span class="sxs-lookup"><span data-stu-id="22a47-103">How toouse table storage from PHP</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="22a47-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="22a47-104">Overview</span></span>
<span data-ttu-id="22a47-105">Den här guiden visar hur tooperform vanliga scenarier med hjälp av hello Azure Table-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="22a47-105">This guide shows you how tooperform common scenarios using hello Azure Table service.</span></span> <span data-ttu-id="22a47-106">hello exemplen är skrivna i PHP och använder hello [Azure SDK för PHP][download].</span><span class="sxs-lookup"><span data-stu-id="22a47-106">hello samples are written in PHP and use hello [Azure SDK for PHP][download].</span></span> <span data-ttu-id="22a47-107">hello beskrivs scenarier där **skapar och tar bort en tabell och lägga till, ta bort och frågar entiteter i en tabell**.</span><span class="sxs-lookup"><span data-stu-id="22a47-107">hello scenarios covered include **creating and deleting a table, and inserting, deleting, and querying entities in a table**.</span></span> <span data-ttu-id="22a47-108">Mer information om hello Azure Table-tjänsten finns hello [nästa steg](#next-steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="22a47-108">For more information on hello Azure Table service, see hello [Next steps](#next-steps) section.</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a><span data-ttu-id="22a47-109">Skapa en PHP-program</span><span class="sxs-lookup"><span data-stu-id="22a47-109">Create a PHP application</span></span>
<span data-ttu-id="22a47-110">hello endast hello är ett krav för att skapa en PHP-program som ansluter till hello Azure Table tjänst refererar till klasser i hello Azure SDK för PHP från inom din kod.</span><span class="sxs-lookup"><span data-stu-id="22a47-110">hello only requirement for creating a PHP application that accesses hello Azure Table service is hello referencing of classes in hello Azure SDK for PHP from within your code.</span></span> <span data-ttu-id="22a47-111">Du kan använda alla development tools toocreate ditt program, inklusive anteckningar.</span><span class="sxs-lookup"><span data-stu-id="22a47-111">You can use any development tools toocreate your application, including Notepad.</span></span>

<span data-ttu-id="22a47-112">I den här guiden kan du använda tabellen tjänstens funktioner som kan anropas från ett PHP-program lokalt eller i koden körs i en Azure-webbroll, en arbetsroll eller en webbplats.</span><span class="sxs-lookup"><span data-stu-id="22a47-112">In this guide, you use Table service features which can be called from within a PHP application locally, or in code running within an Azure web role, worker role, or website.</span></span>

## <a name="get-hello-azure-client-libraries"></a><span data-ttu-id="22a47-113">Hämta hello Azure-klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="22a47-113">Get hello Azure Client Libraries</span></span>
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-tooaccess-hello-table-service"></a><span data-ttu-id="22a47-114">Konfigurera program för tooaccess hello tabelltjänst</span><span class="sxs-lookup"><span data-stu-id="22a47-114">Configure your application tooaccess hello Table service</span></span>
<span data-ttu-id="22a47-115">toouse hello Azure Table service API: er, måste du:</span><span class="sxs-lookup"><span data-stu-id="22a47-115">toouse hello Azure Table service APIs, you need to:</span></span>

1. <span data-ttu-id="22a47-116">Referens hello bandladdaren fil med hello [require_once] [ require_once] -instruktionen och</span><span class="sxs-lookup"><span data-stu-id="22a47-116">Reference hello autoloader file using hello [require_once][require_once] statement, and</span></span>
2. <span data-ttu-id="22a47-117">Referera till alla klasser som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="22a47-117">Reference any classes you might use.</span></span>

<span data-ttu-id="22a47-118">hello följande exempel visas hur tooinclude hello bandladdaren fil- och hello **ServicesBuilder** klass.</span><span class="sxs-lookup"><span data-stu-id="22a47-118">hello following example shows how tooinclude hello autoloader file and reference hello **ServicesBuilder** class.</span></span>

> [!NOTE]
> <span data-ttu-id="22a47-119">hello exemplen i den här artikeln förutsätter att du har installerat hello PHP-klientbibliotek för Azure via Composer.</span><span class="sxs-lookup"><span data-stu-id="22a47-119">hello examples in this article assume you have installed hello PHP Client Libraries for Azure via Composer.</span></span> <span data-ttu-id="22a47-120">Om du har installerat hello bibliotek manuellt måste tooreference hello <code>WindowsAzure.php</code> bandladdaren filen.</span><span class="sxs-lookup"><span data-stu-id="22a47-120">If you installed hello libraries manually, you need tooreference hello <code>WindowsAzure.php</code> autoloader file.</span></span>
>
>

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

<span data-ttu-id="22a47-121">I hello exemplen nedan hello `require_once` instruktionen visas alltid, men bara hello klasser krävs för hello exempel tooexecute refererar till.</span><span class="sxs-lookup"><span data-stu-id="22a47-121">In hello examples below, hello `require_once` statement is always shown, but only hello classes necessary for hello example tooexecute are referenced.</span></span>

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="22a47-122">Skapa en Azure storage-anslutning</span><span class="sxs-lookup"><span data-stu-id="22a47-122">Set up an Azure storage connection</span></span>
<span data-ttu-id="22a47-123">tooinstantiate klienten en Azure Table-tjänsten måste du först ha en giltig anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="22a47-123">tooinstantiate an Azure Table service client, you must first have a valid connection string.</span></span> <span data-ttu-id="22a47-124">hello-formatet för hello anslutningssträng för tabellen är:</span><span class="sxs-lookup"><span data-stu-id="22a47-124">hello format for hello Table service connection string is:</span></span>

<span data-ttu-id="22a47-125">För att komma åt en live-tjänst:</span><span class="sxs-lookup"><span data-stu-id="22a47-125">For accessing a live service:</span></span>

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

<span data-ttu-id="22a47-126">För att komma åt hello emulatorn lagring:</span><span class="sxs-lookup"><span data-stu-id="22a47-126">For accessing hello emulator storage:</span></span>

```php
UseDevelopmentStorage=true
```

<span data-ttu-id="22a47-127">toocreate alla Azure-tjänst-klienter, behöver du toouse hello **ServicesBuilder** klass.</span><span class="sxs-lookup"><span data-stu-id="22a47-127">toocreate any Azure service client, you need toouse hello **ServicesBuilder** class.</span></span> <span data-ttu-id="22a47-128">Du kan:</span><span class="sxs-lookup"><span data-stu-id="22a47-128">You can:</span></span>

* <span data-ttu-id="22a47-129">Skicka hello anslutning direkt string tooit eller</span><span class="sxs-lookup"><span data-stu-id="22a47-129">pass hello connection string directly tooit or</span></span>
* <span data-ttu-id="22a47-130">Använd hello **CloudConfigurationManager (CCM)** toocheck flera externa datakällor för hello anslutningssträngen:</span><span class="sxs-lookup"><span data-stu-id="22a47-130">use hello **CloudConfigurationManager (CCM)** toocheck multiple external sources for hello connection string:</span></span>
  * <span data-ttu-id="22a47-131">som standard levereras med stöd för en extern källa - miljövariabler</span><span class="sxs-lookup"><span data-stu-id="22a47-131">by default, it comes with support for one external source - environmental variables</span></span>
  * <span data-ttu-id="22a47-132">Du kan lägga till nya källor genom att utöka hello **ConnectionStringSource** klass</span><span class="sxs-lookup"><span data-stu-id="22a47-132">you can add new sources by extending hello **ConnectionStringSource** class</span></span>

<span data-ttu-id="22a47-133">Hello-exempel som beskrivs här skickas hello anslutningssträngen direkt.</span><span class="sxs-lookup"><span data-stu-id="22a47-133">For hello examples outlined here, hello connection string will be passed directly.</span></span>

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);
```

## <a name="create-a-table"></a><span data-ttu-id="22a47-134">Skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="22a47-134">Create a table</span></span>
<span data-ttu-id="22a47-135">En **TableRestProxy** objekt kan du skapa en tabell med hello **createTable** metod.</span><span class="sxs-lookup"><span data-stu-id="22a47-135">A **TableRestProxy** object lets you create a table with hello **createTable** method.</span></span> <span data-ttu-id="22a47-136">När du skapar en tabell kan ange du hello tabellen service-timeout.</span><span class="sxs-lookup"><span data-stu-id="22a47-136">When creating a table, you can set hello Table service timeout.</span></span> <span data-ttu-id="22a47-137">(Mer information om hello tabellen service-timeout finns [inställningen tidsgränser för tabellen tjänståtgärder][table-service-timeouts].)</span><span class="sxs-lookup"><span data-stu-id="22a47-137">(For more information about hello Table service timeout, see [Setting Timeouts for Table Service Operations][table-service-timeouts].)</span></span>

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

<span data-ttu-id="22a47-138">Mer information om begränsningar för tabellnamn finns [hello förstå tabelltjänst-datamodellen][table-data-model].</span><span class="sxs-lookup"><span data-stu-id="22a47-138">For information about restrictions on table names, see [Understanding hello Table Service Data Model][table-data-model].</span></span>

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="22a47-139">Lägg till en entitet tooa tabell</span><span class="sxs-lookup"><span data-stu-id="22a47-139">Add an entity tooa table</span></span>
<span data-ttu-id="22a47-140">tooadd en entitet tooa tabell, skapa en ny **entiteten** objekt och skickar den för**TableRestProxy -> insertEntity**.</span><span class="sxs-lookup"><span data-stu-id="22a47-140">tooadd an entity tooa table, create a new **Entity** object and pass it too**TableRestProxy->insertEntity**.</span></span> <span data-ttu-id="22a47-141">Observera att när du skapar en entitet, måste du ange en `PartitionKey` och `RowKey`.</span><span class="sxs-lookup"><span data-stu-id="22a47-141">Note that when you create an entity, you must specify a `PartitionKey` and `RowKey`.</span></span> <span data-ttu-id="22a47-142">Dessa är hello unika identifierare för en entitet och är värden som kan efterfrågas mycket snabbare än andra entitetsegenskaper.</span><span class="sxs-lookup"><span data-stu-id="22a47-142">These are hello unique identifiers for an entity and are values that can be queried much faster than other entity properties.</span></span> <span data-ttu-id="22a47-143">hello används `PartitionKey` tooautomatically distribuera hello tabellentiteter över många lagringsnoder.</span><span class="sxs-lookup"><span data-stu-id="22a47-143">hello system uses `PartitionKey` tooautomatically distribute hello table's entities over many storage nodes.</span></span> <span data-ttu-id="22a47-144">Entiteter med hello samma `PartitionKey` lagras på hello samma nod.</span><span class="sxs-lookup"><span data-stu-id="22a47-144">Entities with hello same `PartitionKey` are stored on hello same node.</span></span> <span data-ttu-id="22a47-145">(Åtgärder på flera entiteter som lagras på samma nod utför hello bättre än på entiteter som lagras på olika noder.) Hej `RowKey` är hello unikt ID för en entitet i en partition.</span><span class="sxs-lookup"><span data-stu-id="22a47-145">(Operations on multiple entities stored on hello same node perform better than on entities stored across different nodes.) hello `RowKey` is hello unique ID of an entity within a partition.</span></span>

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

<span data-ttu-id="22a47-146">Information om Tabellegenskaper och typer finns [hello förstå tabelltjänst-datamodellen][table-data-model].</span><span class="sxs-lookup"><span data-stu-id="22a47-146">For information about Table properties and types, see [Understanding hello Table Service Data Model][table-data-model].</span></span>

<span data-ttu-id="22a47-147">Hej **TableRestProxy** klass erbjuder två alternativa metoder för att lägga till entiteter: **insertOrMergeEntity** och **insertOrReplaceEntity**.</span><span class="sxs-lookup"><span data-stu-id="22a47-147">hello **TableRestProxy** class offers two alternative methods for inserting entities: **insertOrMergeEntity** and **insertOrReplaceEntity**.</span></span> <span data-ttu-id="22a47-148">toouse metoderna, skapa en ny **entiteten** och skicka den som en parameter tooeither metod.</span><span class="sxs-lookup"><span data-stu-id="22a47-148">toouse these methods, create a new **Entity** and pass it as a parameter tooeither method.</span></span> <span data-ttu-id="22a47-149">Varje metod infogar hello entiteten om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="22a47-149">Each method will insert hello entity if it does not exist.</span></span> <span data-ttu-id="22a47-150">Om hello entitet redan **insertOrMergeEntity** uppdaterar egenskapsvärden om hello egenskaper redan finns och lägger till nya egenskaper om det inte finns, medan **insertOrReplaceEntity** helt ersätter en befintlig entitet.</span><span class="sxs-lookup"><span data-stu-id="22a47-150">If hello entity already exists, **insertOrMergeEntity** updates property values if hello properties already exist and adds new properties if they do not exist, while **insertOrReplaceEntity** completely replaces an existing entity.</span></span> <span data-ttu-id="22a47-151">följande exempel visar hur hello toouse **insertOrMergeEntity**.</span><span class="sxs-lookup"><span data-stu-id="22a47-151">hello following example shows how toouse **insertOrMergeEntity**.</span></span> <span data-ttu-id="22a47-152">Om hello entitet med `PartitionKey` ”tasksSeattle” och `RowKey` ”1” inte redan finns, kommer att infogas.</span><span class="sxs-lookup"><span data-stu-id="22a47-152">If hello entity with `PartitionKey` "tasksSeattle" and `RowKey` "1" does not already exist, it will be inserted.</span></span> <span data-ttu-id="22a47-153">Men om det har tidigare infogats (som visas i hello-exemplet ovan), hello `DueDate` egenskapen kommer att uppdateras och hello `Status` egenskapen kommer att läggas till.</span><span class="sxs-lookup"><span data-stu-id="22a47-153">However, if it has previously been inserted (as shown in hello example above), hello `DueDate` property will be updated, and hello `Status` property will be added.</span></span> <span data-ttu-id="22a47-154">Hej `Description` och `Location` egenskaper har också uppdaterats, men med värden som effektivt låt dem vara oförändrad.</span><span class="sxs-lookup"><span data-stu-id="22a47-154">hello `Description` and `Location` properties are also updated, but with values that effectively leave them unchanged.</span></span> <span data-ttu-id="22a47-155">Om dessa senare två egenskaper inte har lagts till som visas i hello exempel, men fanns på hello målentiteten, skulle befintliga värden ändras inte.</span><span class="sxs-lookup"><span data-stu-id="22a47-155">If these latter two properties were not added as shown in hello example, but existed on hello target entity, their existing values would remain unchanged.</span></span>

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

## <a name="retrieve-a-single-entity"></a><span data-ttu-id="22a47-156">Hämta en enda entitet</span><span class="sxs-lookup"><span data-stu-id="22a47-156">Retrieve a single entity</span></span>
<span data-ttu-id="22a47-157">Hej **TableRestProxy -> getEntity** metod kan du tooretrieve en enda enhet genom att fråga om dess `PartitionKey` och `RowKey`.</span><span class="sxs-lookup"><span data-stu-id="22a47-157">hello **TableRestProxy->getEntity** method allows you tooretrieve a single entity by querying for its `PartitionKey` and `RowKey`.</span></span> <span data-ttu-id="22a47-158">I hello exemplet nedan hello partitionsnyckel `tasksSeattle` och radnyckel `1` skickas toohello **getEntity** metod.</span><span class="sxs-lookup"><span data-stu-id="22a47-158">In hello example below, hello partition key `tasksSeattle` and row key `1` are passed toohello **getEntity** method.</span></span>

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

## <a name="retrieve-all-entities-in-a-partition"></a><span data-ttu-id="22a47-159">Hämta alla entiteter i en partition</span><span class="sxs-lookup"><span data-stu-id="22a47-159">Retrieve all entities in a partition</span></span>
<span data-ttu-id="22a47-160">Entiteten frågor har skapats med hjälp av filter (Mer information finns i [frågor till tabeller och de entiteter][filters]).</span><span class="sxs-lookup"><span data-stu-id="22a47-160">Entity queries are constructed using filters (for more information, see [Querying Tables and Entities][filters]).</span></span> <span data-ttu-id="22a47-161">tooretrieve alla entiteter i partitionen, använder hello filter ”PartitionKey eq *partitionsnamn*”.</span><span class="sxs-lookup"><span data-stu-id="22a47-161">tooretrieve all entities in partition, use hello filter "PartitionKey eq *partition_name*".</span></span> <span data-ttu-id="22a47-162">följande exempel visar hur hello tooretrieve alla entiteter i hello `tasksSeattle` partition genom att skicka ett filter toohello **queryEntities** metod.</span><span class="sxs-lookup"><span data-stu-id="22a47-162">hello following example shows how tooretrieve all entities in hello `tasksSeattle` partition by passing a filter toohello **queryEntities** method.</span></span>

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

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a><span data-ttu-id="22a47-163">Hämta en delmängd av entiteter i en partition</span><span class="sxs-lookup"><span data-stu-id="22a47-163">Retrieve a subset of entities in a partition</span></span>
<span data-ttu-id="22a47-164">Hej samma mönster som används i hello föregående exempel kan vara används tooretrieve en delmängd av entiteter i en partition.</span><span class="sxs-lookup"><span data-stu-id="22a47-164">hello same pattern used in hello previous example can be used tooretrieve any subset of entities in a partition.</span></span> <span data-ttu-id="22a47-165">hello delmängd av du hämta entiteter bestäms av hello filter som du använder (Mer information finns i [frågor till tabeller och de entiteter][filters]) .hello som följande exempel visar hur toouse tooretrieve ett filter alla entiteter med ett visst `Location` och en `DueDate` mindre än det angivna datumet.</span><span class="sxs-lookup"><span data-stu-id="22a47-165">hello subset of entities you retrieve are determined by hello filter you use (for more information, see [Querying Tables and Entities][filters]).hello following example shows how toouse a filter tooretrieve all entities with a specific `Location` and a `DueDate` less than a specified date.</span></span>

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

## <a name="retrieve-a-subset-of-entity-properties"></a><span data-ttu-id="22a47-166">Hämta en deluppsättning Entitetsegenskaper</span><span class="sxs-lookup"><span data-stu-id="22a47-166">Retrieve a subset of entity properties</span></span>
<span data-ttu-id="22a47-167">En fråga kan hämta en deluppsättning entitetsegenskaper.</span><span class="sxs-lookup"><span data-stu-id="22a47-167">A query can retrieve a subset of entity properties.</span></span> <span data-ttu-id="22a47-168">Den här tekniken, kallad *projektion*, minskar bandbredden och kan förbättra frågeprestanda, särskilt för stora entiteter.</span><span class="sxs-lookup"><span data-stu-id="22a47-168">This technique, called *projection*, reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="22a47-169">toospecify en egenskapen toobe hämtas, skicka hello namnet på hello egenskapen toohello **addSelectField ->** metod.</span><span class="sxs-lookup"><span data-stu-id="22a47-169">toospecify a property toobe retrieved, pass hello name of hello property toohello **Query->addSelectField** method.</span></span> <span data-ttu-id="22a47-170">Du kan anropa metoden flera gånger tooadd fler egenskaper.</span><span class="sxs-lookup"><span data-stu-id="22a47-170">You can call this method multiple times tooadd more properties.</span></span> <span data-ttu-id="22a47-171">Efter körning **TableRestProxy -> queryEntities**, hello returnerade entiteter har endast hello valda egenskaper.</span><span class="sxs-lookup"><span data-stu-id="22a47-171">After executing **TableRestProxy->queryEntities**, hello returned entities will only have hello selected properties.</span></span> <span data-ttu-id="22a47-172">(Om du vill tooreturn en delmängd av tabellentiteter kan använda ett filter som visas i hello frågor ovan.)</span><span class="sxs-lookup"><span data-stu-id="22a47-172">(If you want tooreturn a subset of Table entities, use a filter as shown in hello queries above.)</span></span>

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

## <a name="update-an-entity"></a><span data-ttu-id="22a47-173">Uppdatera en entitet</span><span class="sxs-lookup"><span data-stu-id="22a47-173">Update an entity</span></span>
<span data-ttu-id="22a47-174">En befintlig entitet kan uppdateras med hjälp av hello **entitet -> setProperty** och **entitet -> addProperty** metoder i hello entitet och sedan anropar **TableRestProxy -> updateEntity** .</span><span class="sxs-lookup"><span data-stu-id="22a47-174">An existing entity can be updated by using hello **Entity->setProperty** and **Entity->addProperty** methods on hello entity, and then calling **TableRestProxy->updateEntity**.</span></span> <span data-ttu-id="22a47-175">hello följande exempel hämtar en entitet, ändrar en egenskap, tar bort en annan egenskap och lägger till en ny egenskap.</span><span class="sxs-lookup"><span data-stu-id="22a47-175">hello following example retrieves an entity, modifies one property, removes another property, and adds a new property.</span></span> <span data-ttu-id="22a47-176">Observera att du tar bort en egenskap genom att ange värdet för**null**.</span><span class="sxs-lookup"><span data-stu-id="22a47-176">Note that you can remove a property by setting its value too**null**.</span></span>

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

## <a name="delete-an-entity"></a><span data-ttu-id="22a47-177">Ta bort en entitet</span><span class="sxs-lookup"><span data-stu-id="22a47-177">Delete an entity</span></span>
<span data-ttu-id="22a47-178">toodelete en entitet skicka hello tabellnamn och hello entitet `PartitionKey` och `RowKey` toohello **TableRestProxy -> deleteEntity** metod.</span><span class="sxs-lookup"><span data-stu-id="22a47-178">toodelete an entity, pass hello table name, and hello entity's `PartitionKey` and `RowKey` toohello **TableRestProxy->deleteEntity** method.</span></span>

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

<span data-ttu-id="22a47-179">Observera att du kan ange hello Etag för en entitet toobe bort med hjälp av hello samtidighet kontroller **DeleteEntityOptions -> setEtag** metod och skicka hello **DeleteEntityOptions** objekt för**deleteEntity** som en fjärde parameter.</span><span class="sxs-lookup"><span data-stu-id="22a47-179">Note that for concurrency checks, you can set hello Etag for an entity toobe deleted by using hello **DeleteEntityOptions->setEtag** method and passing hello **DeleteEntityOptions** object too**deleteEntity** as a fourth parameter.</span></span>

## <a name="batch-table-operations"></a><span data-ttu-id="22a47-180">Batchtabeller</span><span class="sxs-lookup"><span data-stu-id="22a47-180">Batch table operations</span></span>
<span data-ttu-id="22a47-181">Hej **TableRestProxy -> batch** metod kan du tooexecute flera åtgärder i en enskild begäran.</span><span class="sxs-lookup"><span data-stu-id="22a47-181">hello **TableRestProxy->batch** method allows you tooexecute multiple operations in a single request.</span></span> <span data-ttu-id="22a47-182">hello mönster här måste du lägga till åtgärder för**BatchRequest** objekt och sedan överföra hello **BatchRequest** objekt toohello **TableRestProxy -> batch** metod.</span><span class="sxs-lookup"><span data-stu-id="22a47-182">hello pattern here involves adding operations too**BatchRequest** object and then passing hello **BatchRequest** object toohello **TableRestProxy->batch** method.</span></span> <span data-ttu-id="22a47-183">tooadd en åtgärden tooa **BatchRequest** objekt, kan du anropa någon av följande metoder flera gånger hello:</span><span class="sxs-lookup"><span data-stu-id="22a47-183">tooadd an operation tooa **BatchRequest** object, you can call any of hello following methods multiple times:</span></span>

* <span data-ttu-id="22a47-184">**addInsertEntity** (lägger till en insertEntity åtgärd)</span><span class="sxs-lookup"><span data-stu-id="22a47-184">**addInsertEntity** (adds an insertEntity operation)</span></span>
* <span data-ttu-id="22a47-185">**addUpdateEntity** (lägger till en updateEntity åtgärd)</span><span class="sxs-lookup"><span data-stu-id="22a47-185">**addUpdateEntity** (adds an updateEntity operation)</span></span>
* <span data-ttu-id="22a47-186">**addMergeEntity** (lägger till en åtgärd för mergeEntity)</span><span class="sxs-lookup"><span data-stu-id="22a47-186">**addMergeEntity** (adds a mergeEntity operation)</span></span>
* <span data-ttu-id="22a47-187">**addInsertOrReplaceEntity** (lägger till en insertOrReplaceEntity åtgärd)</span><span class="sxs-lookup"><span data-stu-id="22a47-187">**addInsertOrReplaceEntity** (adds an insertOrReplaceEntity operation)</span></span>
* <span data-ttu-id="22a47-188">**addInsertOrMergeEntity** (lägger till en insertOrMergeEntity åtgärd)</span><span class="sxs-lookup"><span data-stu-id="22a47-188">**addInsertOrMergeEntity** (adds an insertOrMergeEntity operation)</span></span>
* <span data-ttu-id="22a47-189">**addDeleteEntity** (lägger till en åtgärd för deleteEntity)</span><span class="sxs-lookup"><span data-stu-id="22a47-189">**addDeleteEntity** (adds a deleteEntity operation)</span></span>

<span data-ttu-id="22a47-190">följande exempel visar hur hello tooexecute **insertEntity** och **deleteEntity** åtgärder i en enskild begäran:</span><span class="sxs-lookup"><span data-stu-id="22a47-190">hello following example shows how tooexecute **insertEntity** and **deleteEntity** operations in a single request:</span></span>

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

<span data-ttu-id="22a47-191">Mer information om batchbearbetning tabellåtgärder finns [utför entitet gruppera transaktioner][entity-group-transactions].</span><span class="sxs-lookup"><span data-stu-id="22a47-191">For more information about batching Table operations, see [Performing Entity Group Transactions][entity-group-transactions].</span></span>

## <a name="delete-a-table"></a><span data-ttu-id="22a47-192">Ta bort en tabell</span><span class="sxs-lookup"><span data-stu-id="22a47-192">Delete a table</span></span>
<span data-ttu-id="22a47-193">Slutligen toodelete en tabell, skicka hello tabellen namnet toohello **TableRestProxy -> deleteTable** metod.</span><span class="sxs-lookup"><span data-stu-id="22a47-193">Finally, toodelete a table, pass hello table name toohello **TableRestProxy->deleteTable** method.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="22a47-194">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="22a47-194">Next steps</span></span>
<span data-ttu-id="22a47-195">Nu när du har lärt dig hello grunderna i hello Azure Table-tjänsten, följa dessa länkar toolearn om mer komplexa lagringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="22a47-195">Now that you've learned hello basics of hello Azure Table service, follow these links toolearn about more complex storage tasks.</span></span>

* <span data-ttu-id="22a47-196">[Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en gratis, fristående app från Microsoft som möjliggör toowork visuellt med Azure Storage-data i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="22a47-196">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.</span></span>

* <span data-ttu-id="22a47-197">[PHP Developer Center](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="22a47-197">[PHP Developer Center](/develop/php/).</span></span>

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx
