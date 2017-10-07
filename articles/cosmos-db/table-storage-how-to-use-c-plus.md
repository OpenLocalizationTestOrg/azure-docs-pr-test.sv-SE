---
title: aaaHow toouse Table storage (C++) | Microsoft Docs
description: Lagra strukturerade data i hello molnet med Azure Table storage, en NoSQL-databas.
services: cosmos-db
documentationcenter: .net
author: mimig1
manager: jahogg
editor: tysonn
ms.assetid: f191f308-e4b2-4de9-85cb-551b82b1ea7c
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: mimig
ms.openlocfilehash: 8096fe531427ba4858f7f4cb7f664f941892d1c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-table-storage-from-c"></a><span data-ttu-id="68a46-103">Hur toouse Table storage från C++</span><span class="sxs-lookup"><span data-stu-id="68a46-103">How toouse Table storage from C++</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="68a46-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="68a46-104">Overview</span></span>
<span data-ttu-id="68a46-105">Den här guiden visar hur tooperform vanliga scenarier med hjälp av hello Azure Table storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="68a46-105">This guide will show you how tooperform common scenarios by using hello Azure Table storage service.</span></span> <span data-ttu-id="68a46-106">hello exemplen är skrivna i C++ och använda hello [Azure Storage-klientbibliotek för C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="68a46-106">hello samples are written in C++ and use hello [Azure Storage Client Library for C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md).</span></span> <span data-ttu-id="68a46-107">hello beskrivs scenarier där **skapa och ta bort en tabell** och **arbeta med tabellentiteter**.</span><span class="sxs-lookup"><span data-stu-id="68a46-107">hello scenarios covered include **creating and deleting a table** and **working with table entities**.</span></span>

> [!NOTE]
> <span data-ttu-id="68a46-108">Den här guiden riktad mot hello Azure Storage-klientbibliotek för C++ version 1.0.0 och senare.</span><span class="sxs-lookup"><span data-stu-id="68a46-108">This guide targets hello Azure Storage Client Library for C++ version 1.0.0 and above.</span></span> <span data-ttu-id="68a46-109">hello rekommenderas version är Storage-klientbibliotek 2.2.0, som är tillgängliga via [NuGet](http://www.nuget.org/packages/wastorage) eller [GitHub](https://github.com/Azure/azure-storage-cpp/).</span><span class="sxs-lookup"><span data-stu-id="68a46-109">hello recommended version is Storage Client Library 2.2.0, which is available via [NuGet](http://www.nuget.org/packages/wastorage) or [GitHub](https://github.com/Azure/azure-storage-cpp/).</span></span>
> 
> 

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a><span data-ttu-id="68a46-110">Skapa ett C++-program</span><span class="sxs-lookup"><span data-stu-id="68a46-110">Create a C++ application</span></span>
<span data-ttu-id="68a46-111">I den här guiden använder lagringsfunktioner som kan köras i ett C++-program.</span><span class="sxs-lookup"><span data-stu-id="68a46-111">In this guide, you will use storage features that can be run within a C++ application.</span></span> <span data-ttu-id="68a46-112">toodo så behöver du tooinstall hello Azure Storage-klientbibliotek för C++ och skapa ett Azure storage-konto i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="68a46-112">toodo so, you will need tooinstall hello Azure Storage Client Library for C++ and create an Azure storage account in your Azure subscription.</span></span>  

<span data-ttu-id="68a46-113">tooinstall hello Azure Storage-klientbibliotek för C++ kan du använda hello följande metoder:</span><span class="sxs-lookup"><span data-stu-id="68a46-113">tooinstall hello Azure Storage Client Library for C++, you can use hello following methods:</span></span>

* <span data-ttu-id="68a46-114">**Linux:** Följ instruktionerna på hello hello [Azure Storage-klientbibliotek för C++ viktigt](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) sidan.</span><span class="sxs-lookup"><span data-stu-id="68a46-114">**Linux:** Follow hello instructions given on hello [Azure Storage Client Library for C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) page.</span></span>  
* <span data-ttu-id="68a46-115">**Windows:** i Visual Studio klickar du på **Verktyg > NuGet Package Manager > Package Manager-konsolen**.</span><span class="sxs-lookup"><span data-stu-id="68a46-115">**Windows:** In Visual Studio, click **Tools > NuGet Package Manager > Package Manager Console**.</span></span> <span data-ttu-id="68a46-116">Typen hello följande kommando i hello [NuGet Package Manager-konsolen](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) och tryck på RETUR.</span><span class="sxs-lookup"><span data-stu-id="68a46-116">Type hello following command into hello [NuGet Package Manager console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) and press Enter.</span></span>  
  
     <span data-ttu-id="68a46-117">Install-Package wastorage</span><span class="sxs-lookup"><span data-stu-id="68a46-117">Install-Package wastorage</span></span>

## <a name="configure-your-application-tooaccess-table-storage"></a><span data-ttu-id="68a46-118">Konfigurera ditt program tooaccess tabellagring</span><span class="sxs-lookup"><span data-stu-id="68a46-118">Configure your application tooaccess Table storage</span></span>
<span data-ttu-id="68a46-119">Lägga till följande hello innehålla instruktioner toohello överkant hello C++ filen där du vill toouse hello Azure storage-API: er tooaccess tabeller:</span><span class="sxs-lookup"><span data-stu-id="68a46-119">Add hello following include statements toohello top of hello C++ file where you want toouse hello Azure storage APIs tooaccess tables:</span></span>  

```cpp
#include <was/storage_account.h>
#include <was/table.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="68a46-120">Ställ in en anslutningssträng för Azure storage</span><span class="sxs-lookup"><span data-stu-id="68a46-120">Set up an Azure storage connection string</span></span>
<span data-ttu-id="68a46-121">Ett Azure storage-klienten använder en lagring sträng toostore slutpunkter och autentiseringsuppgifter för åtkomst till data management services.</span><span class="sxs-lookup"><span data-stu-id="68a46-121">An Azure storage client uses a storage connection string toostore endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="68a46-122">Du måste ange hello lagringsanslutningssträngen i hello följande format när du kör ett klientprogram.</span><span class="sxs-lookup"><span data-stu-id="68a46-122">When running a client application, you must provide hello storage connection string in hello following format.</span></span> <span data-ttu-id="68a46-123">Använd hello namnet på ditt konto och hello lagring lagringsåtkomstnyckel för hello storage-konto som anges i hello [Azure Portal](https://portal.azure.com) för hello *AccountName* och *AccountKey* värden.</span><span class="sxs-lookup"><span data-stu-id="68a46-123">Use hello name of your storage account and hello storage access key for hello storage account listed in hello [Azure Portal](https://portal.azure.com) for hello *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="68a46-124">Information om lagringskonton och snabbtangenterna finns [om Azure storage-konton](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="68a46-124">For information on storage accounts and access keys, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="68a46-125">Det här exemplet visar hur du kan deklarera en anslutningssträng för statiska fält toohold hello:</span><span class="sxs-lookup"><span data-stu-id="68a46-125">This example shows how you can declare a static field toohold hello connection string:</span></span>  

```cpp
// Define hello connection string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

<span data-ttu-id="68a46-126">tootest ditt program i din lokala Windows-baserad dator kan du använda hello Azure [lagringsemulatorn](../storage/common/storage-use-emulator.md) som installeras med hello [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="68a46-126">tootest your application in your local Windows-based computer, you can use hello Azure [storage emulator](../storage/common/storage-use-emulator.md) that is installed with hello [Azure SDK](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="68a46-127">hello storage-emulatorn är ett verktyg som simulerar hello Azure Blob, köer och tabellen tjänster som är tillgängliga på utvecklingsdatorn lokala.</span><span class="sxs-lookup"><span data-stu-id="68a46-127">hello storage emulator is a utility that simulates hello Azure Blob, Queue, and Table services available on your local development machine.</span></span> <span data-ttu-id="68a46-128">hello följande exempel visas hur du deklarera ett statiskt fält toohold hello anslutning sträng tooyour lokala storage-emulatorn:</span><span class="sxs-lookup"><span data-stu-id="68a46-128">hello following example shows how you can declare a static field toohold hello connection string tooyour local storage emulator:</span></span>  

```cpp
// Define hello connection string with Azure storage emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

<span data-ttu-id="68a46-129">toostart hello Azure storage-emulatorn, klicka på hello **starta** eller tryck på knappen hello Windows-tangenten.</span><span class="sxs-lookup"><span data-stu-id="68a46-129">toostart hello Azure storage emulator, click hello **Start** button or press hello Windows key.</span></span> <span data-ttu-id="68a46-130">Börja skriva **Azure Storage-emulatorn**, och välj sedan **Microsoft Azure Storage-emulatorn** hello listan med program.</span><span class="sxs-lookup"><span data-stu-id="68a46-130">Begin typing **Azure Storage Emulator**, and then select **Microsoft Azure Storage Emulator** from hello list of applications.</span></span>  

<span data-ttu-id="68a46-131">hello förutsätter följande exempel att du har använt någon av dessa två metoder tooget hello lagringsanslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="68a46-131">hello following samples assume that you have used one of these two methods tooget hello storage connection string.</span></span>  

## <a name="retrieve-your-connection-string"></a><span data-ttu-id="68a46-132">Hämta anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="68a46-132">Retrieve your connection string</span></span>
<span data-ttu-id="68a46-133">Du kan använda hello **cloud_storage_account** klassen toorepresent kontoinformationen lagring.</span><span class="sxs-lookup"><span data-stu-id="68a46-133">You can use hello **cloud_storage_account** class toorepresent your storage account information.</span></span> <span data-ttu-id="68a46-134">tooretrieve lagringen kontoinformation från hello anslutningssträngen för lagring kan du använda hello parse-metod.</span><span class="sxs-lookup"><span data-stu-id="68a46-134">tooretrieve your storage account information from hello storage connection string, you can use hello parse method.</span></span>

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

<span data-ttu-id="68a46-135">Sedan hämta en referens tooa **cloud_table_client** class, som gör det möjligt att hämta referensobjekt för tabeller och entiteter som lagras i hello Table storage-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="68a46-135">Next, get a reference tooa **cloud_table_client** class, as it lets you get reference objects for tables and entities stored within hello Table storage service.</span></span> <span data-ttu-id="68a46-136">hello följande kod skapar en **cloud_table_client** objektet med hjälp av hello konto lagringsobjekt vi hämta ovan:</span><span class="sxs-lookup"><span data-stu-id="68a46-136">hello following code creates a **cloud_table_client** object by using hello storage account object we retrieved above:</span></span>  

```cpp
// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();
```

## <a name="create-a-table"></a><span data-ttu-id="68a46-137">Skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="68a46-137">Create a table</span></span>
<span data-ttu-id="68a46-138">En **cloud_table_client** objekt kan du få referensobjekt för tabeller och enheter.</span><span class="sxs-lookup"><span data-stu-id="68a46-138">A **cloud_table_client** object lets you get reference objects for tables and entities.</span></span> <span data-ttu-id="68a46-139">hello följande kod skapar en **cloud_table_client** objekt och använder den toocreate en ny tabell.</span><span class="sxs-lookup"><span data-stu-id="68a46-139">hello following code creates a **cloud_table_client** object and uses it toocreate a new table.</span></span>

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);  

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Retrieve a reference tooa table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create hello table if it doesn't exist.
table.create_if_not_exists();  
```

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="68a46-140">Lägg till en entitet tooa tabell</span><span class="sxs-lookup"><span data-stu-id="68a46-140">Add an entity tooa table</span></span>
<span data-ttu-id="68a46-141">tooadd en entitet tooa tabell, skapa en ny **table_entity** objekt och skickar den för**table_operation::insert_entity**.</span><span class="sxs-lookup"><span data-stu-id="68a46-141">tooadd an entity tooa table, create a new **table_entity** object and pass it too**table_operation::insert_entity**.</span></span> <span data-ttu-id="68a46-142">hello använder följande kod hello kundens förnamn som hello radnyckel och efternamn som partitionsnyckel hello.</span><span class="sxs-lookup"><span data-stu-id="68a46-142">hello following code uses hello customer's first name as hello row key and last name as hello partition key.</span></span> <span data-ttu-id="68a46-143">Tillsammans identifiera en entitets partition och radnyckel hello entiteten i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="68a46-143">Together, an entity's partition and row key uniquely identify hello entity in hello table.</span></span> <span data-ttu-id="68a46-144">Entiteter med samma partitionsnyckel kan frågas snabbare än med olika hello partitions nycklar, men med olika partitionsnycklar möjliggör större parallell åtgärd skalbarhet.</span><span class="sxs-lookup"><span data-stu-id="68a46-144">Entities with hello same partition key can be queried faster than those with different partition keys, but using diverse partition keys allows for greater parallel operation scalability.</span></span> <span data-ttu-id="68a46-145">Mer information finns i [Microsoft Azure storage checklistan för prestanda och skalbarhet](../storage/common/storage-performance-checklist.md).</span><span class="sxs-lookup"><span data-stu-id="68a46-145">For more information, see [Microsoft Azure storage performance and scalability checklist](../storage/common/storage-performance-checklist.md).</span></span>

<span data-ttu-id="68a46-146">hello följande kod skapar en ny instans av **table_entity** med vissa toobe för kund-data som lagras.</span><span class="sxs-lookup"><span data-stu-id="68a46-146">hello following code creates a new instance of **table_entity** with some customer data toobe stored.</span></span> <span data-ttu-id="68a46-147">Hej kod nästa anrop **table_operation::insert_entity** toocreate en **table_operation** objektet tooinsert en entitet i en tabell och associerats hello ny tabell entitet med den.</span><span class="sxs-lookup"><span data-stu-id="68a46-147">hello code next calls **table_operation::insert_entity** toocreate a **table_operation** object tooinsert an entity into a table, and associates hello new table entity with it.</span></span> <span data-ttu-id="68a46-148">Slutligen hello koden anropar hello execute-metoden på hello **cloud_table** objekt.</span><span class="sxs-lookup"><span data-stu-id="68a46-148">Finally, hello code calls hello execute method on hello **cloud_table** object.</span></span> <span data-ttu-id="68a46-149">Och hello nya **table_operation** skickar en begäran toohello tabellen service tooinsert hello nya kundentiteten i hello ”personer”-tabellen.</span><span class="sxs-lookup"><span data-stu-id="68a46-149">And hello new **table_operation** sends a request toohello Table service tooinsert hello new customer entity into hello "people" table.</span></span>  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Retrieve a reference tooa table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create hello table if it doesn't exist.
table.create_if_not_exists();

// Create a new customer entity.
azure::storage::table_entity customer1(U("Harp"), U("Walter"));

azure::storage::table_entity::properties_type& properties = customer1.properties();
properties.reserve(2);
properties[U("Email")] = azure::storage::entity_property(U("Walter@contoso.com"));

properties[U("Phone")] = azure::storage::entity_property(U("425-555-0101"));

// Create hello table operation that inserts hello customer entity.
azure::storage::table_operation insert_operation = azure::storage::table_operation::insert_entity(customer1);

// Execute hello insert operation.
azure::storage::table_result insert_result = table.execute(insert_operation);
```

## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="68a46-150">Infoga en batch med entiteter</span><span class="sxs-lookup"><span data-stu-id="68a46-150">Insert a batch of entities</span></span>
<span data-ttu-id="68a46-151">Du kan infoga en batch med entiteter toohello tabelltjänsten i samma skrivåtgärd.</span><span class="sxs-lookup"><span data-stu-id="68a46-151">You can insert a batch of entities toohello Table service in one write operation.</span></span> <span data-ttu-id="68a46-152">hello följande kod skapar en **table_batch_operation** objekt och lägger sedan till tre Infoga operations tooit.</span><span class="sxs-lookup"><span data-stu-id="68a46-152">hello following code creates a **table_batch_operation** object, and then adds three insert operations tooit.</span></span> <span data-ttu-id="68a46-153">Varje insert-åtgärden har lagts till genom att skapa en ny enhetsobjekt dess inställningsvärden och sedan anropa hello Infoga metoden på hello **table_batch_operation** objekt tooassociate hello entitet med en ny infoga igen.</span><span class="sxs-lookup"><span data-stu-id="68a46-153">Each insert operation is added by creating a new entity object, setting its values, and then calling hello insert method on hello **table_batch_operation** object tooassociate hello entity with a new insert operation.</span></span> <span data-ttu-id="68a46-154">Sedan **cloud_table.execute** tooexecute hello åtgärden anropas.</span><span class="sxs-lookup"><span data-stu-id="68a46-154">Then, **cloud_table.execute** is called tooexecute hello operation.</span></span>  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Define a batch operation.
azure::storage::table_batch_operation batch_operation;

// Create a customer entity and add it toohello table.
azure::storage::table_entity customer1(U("Smith"), U("Jeff"));

azure::storage::table_entity::properties_type& properties1 = customer1.properties();
properties1.reserve(2);
properties1[U("Email")] = azure::storage::entity_property(U("Jeff@contoso.com"));
properties1[U("Phone")] = azure::storage::entity_property(U("425-555-0104"));

// Create another customer entity and add it toohello table.
azure::storage::table_entity customer2(U("Smith"), U("Ben"));

azure::storage::table_entity::properties_type& properties2 = customer2.properties();
properties2.reserve(2);
properties2[U("Email")] = azure::storage::entity_property(U("Ben@contoso.com"));
properties2[U("Phone")] = azure::storage::entity_property(U("425-555-0102"));

// Create a third customer entity tooadd toohello table.
azure::storage::table_entity customer3(U("Smith"), U("Denise"));

azure::storage::table_entity::properties_type& properties3 = customer3.properties();
properties3.reserve(2);
properties3[U("Email")] = azure::storage::entity_property(U("Denise@contoso.com"));
properties3[U("Phone")] = azure::storage::entity_property(U("425-555-0103"));

// Add customer entities toohello batch insert operation.
batch_operation.insert_or_replace_entity(customer1);
batch_operation.insert_or_replace_entity(customer2);
batch_operation.insert_or_replace_entity(customer3);

// Execute hello batch operation.
std::vector<azure::storage::table_result> results = table.execute_batch(batch_operation);
```

<span data-ttu-id="68a46-155">Vissa saker toonote om batchåtgärder:</span><span class="sxs-lookup"><span data-stu-id="68a46-155">Some things toonote on batch operations:</span></span>  

* <span data-ttu-id="68a46-156">Du kan utföra in too100 insert, delete, merge, Ersätt, insert eller dokument och infoga eller ersätta åtgärder i vilken kombination som helst i en enskild batch.</span><span class="sxs-lookup"><span data-stu-id="68a46-156">You can perform up too100 insert, delete, merge, replace, insert-or-merge, and insert-or-replace operations in any combination in a single batch.</span></span>  
* <span data-ttu-id="68a46-157">En batchåtgärd kan ha en hämta-åtgärd om är det enda hello-åtgärd i hello batch.</span><span class="sxs-lookup"><span data-stu-id="68a46-157">A batch operation can have a retrieve operation, if it is hello only operation in hello batch.</span></span>  
* <span data-ttu-id="68a46-158">Alla entiteter i samma batchåtgärd måste ha hello samma partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="68a46-158">All entities in a single batch operation must have hello same partition key.</span></span>  
* <span data-ttu-id="68a46-159">En batchåtgärd är begränsad tooa 4 MB nyttolast.</span><span class="sxs-lookup"><span data-stu-id="68a46-159">A batch operation is limited tooa 4-MB data payload.</span></span>  

## <a name="retrieve-all-entities-in-a-partition"></a><span data-ttu-id="68a46-160">Hämta alla entiteter i en partition</span><span class="sxs-lookup"><span data-stu-id="68a46-160">Retrieve all entities in a partition</span></span>
<span data-ttu-id="68a46-161">tooquery en tabell efter alla entiteter i en partition, Använd en **table_query** objekt.</span><span class="sxs-lookup"><span data-stu-id="68a46-161">tooquery a table for all entities in a partition, use a **table_query** object.</span></span> <span data-ttu-id="68a46-162">hello anges följande kodexempel ett filter för entiteter där Partitionsnyckeln hello är ”Smith”.</span><span class="sxs-lookup"><span data-stu-id="68a46-162">hello following code example specifies a filter for entities where 'Smith' is hello partition key.</span></span> <span data-ttu-id="68a46-163">Det här exemplet skriver ut hello fälten för varje entitet i hello frågan resultat toohello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="68a46-163">This example prints hello fields of each entity in hello query results toohello console.</span></span>  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Construct hello query operation for all customer entities where PartitionKey="Smith".
azure::storage::table_query query;

query.set_filter_string(azure::storage::table_query::generate_filter_condition(U("PartitionKey"), azure::storage::query_comparison_operator::equal, U("Smith")));

// Execute hello query.
azure::storage::table_query_iterator it = table.execute_query(query);

// Print hello fields for each customer.
azure::storage::table_query_iterator end_of_results;
for (; it != end_of_results; ++it)
{
    const azure::storage::table_entity::properties_type& properties = it->properties();

    std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
}  
```

<span data-ttu-id="68a46-164">hello frågan i det här exemplet ger alla hello entiteter som matchar hello filtervillkor.</span><span class="sxs-lookup"><span data-stu-id="68a46-164">hello query in this example brings all hello entities that match hello filter criteria.</span></span> <span data-ttu-id="68a46-165">Om du har stora tabeller och behöver toodownload hello tabellentiteter ofta, rekommenderar vi att du lagrar data i Azure storage-blobbar i stället.</span><span class="sxs-lookup"><span data-stu-id="68a46-165">If you have large tables and need toodownload hello table entities often, we recommend that you store your data in Azure storage blobs instead.</span></span>

## <a name="retrieve-a-range-of-entities-in-a-partition"></a><span data-ttu-id="68a46-166">Hämta ett intervall med enheter i en partition</span><span class="sxs-lookup"><span data-stu-id="68a46-166">Retrieve a range of entities in a partition</span></span>
<span data-ttu-id="68a46-167">Om du inte vill tooquery alla hello entiteter i en partition kan ange du ett intervall genom att kombinera hello partitionsnyckelfiltret med ett radnyckelfilter.</span><span class="sxs-lookup"><span data-stu-id="68a46-167">If you don't want tooquery all hello entities in a partition, you can specify a range by combining hello partition key filter with a row key filter.</span></span> <span data-ttu-id="68a46-168">hello följande kodexempel används två filter tooget alla entiteter i partitionen ”Smith” där hello Radnyckeln (Förnamn) börjar med en bokstav före ”E” i alfabetet hello och skriver sedan ut hello frågeresultat.</span><span class="sxs-lookup"><span data-stu-id="68a46-168">hello following code example uses two filters tooget all entities in partition 'Smith' where hello row key (first name) starts with a letter earlier than 'E' in hello alphabet and then prints hello query results.</span></span>  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create hello table query.
azure::storage::table_query query;

query.set_filter_string(azure::storage::table_query::combine_filter_conditions(
    azure::storage::table_query::generate_filter_condition(U("PartitionKey"),
    azure::storage::query_comparison_operator::equal, U("Smith")),
    azure::storage::query_logical_operator::op_and,
    azure::storage::table_query::generate_filter_condition(U("RowKey"), azure::storage::query_comparison_operator::less_than, U("E"))));

// Execute hello query.
azure::storage::table_query_iterator it = table.execute_query(query);

// Loop through hello results, displaying information about hello entity.
azure::storage::table_query_iterator end_of_results;
for (; it != end_of_results; ++it)
{
    const azure::storage::table_entity::properties_type& properties = it->properties();

    std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
}  
```

## <a name="retrieve-a-single-entity"></a><span data-ttu-id="68a46-169">Hämta en enda entitet</span><span class="sxs-lookup"><span data-stu-id="68a46-169">Retrieve a single entity</span></span>
<span data-ttu-id="68a46-170">Du kan skriva en fråga tooretrieve en enda, specifik entitet.</span><span class="sxs-lookup"><span data-stu-id="68a46-170">You can write a query tooretrieve a single, specific entity.</span></span> <span data-ttu-id="68a46-171">hello följande kod används **table_operation::retrieve_entity** toospecify hello kunden Jeff Smith.</span><span class="sxs-lookup"><span data-stu-id="68a46-171">hello following code uses **table_operation::retrieve_entity** toospecify hello customer 'Jeff Smith'.</span></span> <span data-ttu-id="68a46-172">Den här metoden returnerar endast en entitet i stället för en samling och hello värdet som returneras i **table_result**.</span><span class="sxs-lookup"><span data-stu-id="68a46-172">This method returns just one entity, rather than a collection, and hello returned value is in **table_result**.</span></span> <span data-ttu-id="68a46-173">Ange både partitions- och nycklar i en fråga är hello snabbaste sättet tooretrieve en enda entitet från tabelltjänsten hello.</span><span class="sxs-lookup"><span data-stu-id="68a46-173">Specifying both partition and row keys in a query is hello fastest way tooretrieve a single entity from hello Table service.</span></span>  

```cpp
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Retrieve hello entity with partition key of "Smith" and row key of "Jeff".
azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

// Output hello entity.
azure::storage::table_entity entity = retrieve_result.entity();
const azure::storage::table_entity::properties_type& properties = entity.properties();

std::wcout << U("PartitionKey: ") << entity.partition_key() << U(", RowKey: ") << entity.row_key()
    << U(", Property1: ") << properties.at(U("Email")).string_value()
    << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
```

## <a name="replace-an-entity"></a><span data-ttu-id="68a46-174">Ersätta en entitet</span><span class="sxs-lookup"><span data-stu-id="68a46-174">Replace an entity</span></span>
<span data-ttu-id="68a46-175">Hämta den från tabelltjänsten hello tooreplace en entitet, ändra hello enhetsobjekt och sedan tillbaka toohello tabelltjänsten spara hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="68a46-175">tooreplace an entity, retrieve it from hello Table service, modify hello entity object, and then save hello changes back toohello Table service.</span></span> <span data-ttu-id="68a46-176">hello ändrar följande kod en befintlig kunds telefonnummer och e-postadress.</span><span class="sxs-lookup"><span data-stu-id="68a46-176">hello following code changes an existing customer's phone number and email address.</span></span> <span data-ttu-id="68a46-177">Istället för att ringa **table_operation::insert_entity**använder den här koden **table_operation::replace_entity**.</span><span class="sxs-lookup"><span data-stu-id="68a46-177">Instead of calling **table_operation::insert_entity**, this code uses **table_operation::replace_entity**.</span></span> <span data-ttu-id="68a46-178">Detta medför hello entiteten toobe ersätts helt på hello-servern om hello entiteten på hello-servern har ändrats sedan den hämtades, i så fall hello åtgärden misslyckas.</span><span class="sxs-lookup"><span data-stu-id="68a46-178">This causes hello entity toobe fully replaced on hello server, unless hello entity on hello server has changed since it was retrieved, in which case hello operation will fail.</span></span> <span data-ttu-id="68a46-179">Det här felet är tooprevent ditt program oavsiktligt skriver över en ändring som gjorts mellan hello hämtning och uppdatering av en annan komponent i ditt program.</span><span class="sxs-lookup"><span data-stu-id="68a46-179">This failure is tooprevent your application from inadvertently overwriting a change made between hello retrieval and update by another component of your application.</span></span> <span data-ttu-id="68a46-180">hello korrekt hantering av det här felet är tooretrieve hello entiteten igen, gör dina ändringar (om det är fortfarande giltigt) och sedan köra en **table_operation::replace_entity** igen.</span><span class="sxs-lookup"><span data-stu-id="68a46-180">hello proper handling of this failure is tooretrieve hello entity again, make your changes (if still valid), and then perform another **table_operation::replace_entity** operation.</span></span> <span data-ttu-id="68a46-181">hello nästa avsnitt visar hur toooverride problemet.</span><span class="sxs-lookup"><span data-stu-id="68a46-181">hello next section will show you how toooverride this behavior.</span></span>  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Replace an entity.
azure::storage::table_entity entity_to_replace(U("Smith"), U("Jeff"));
azure::storage::table_entity::properties_type& properties_to_replace = entity_to_replace.properties();
properties_to_replace.reserve(2);

// Specify a new phone number.
properties_to_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0106"));

// Specify a new email address.
properties_to_replace[U("Email")] = azure::storage::entity_property(U("JeffS@contoso.com"));

// Create an operation tooreplace hello entity.
azure::storage::table_operation replace_operation = azure::storage::table_operation::replace_entity(entity_to_replace);

// Submit hello operation toohello Table service.
azure::storage::table_result replace_result = table.execute(replace_operation);
```

## <a name="insert-or-replace-an-entity"></a><span data-ttu-id="68a46-182">Infoga eller ersätta en entitet</span><span class="sxs-lookup"><span data-stu-id="68a46-182">Insert-or-replace an entity</span></span>
<span data-ttu-id="68a46-183">**table_operation::replace_entity** åtgärder misslyckas om hello entiteten har ändrats sedan den hämtades från hello-servern.</span><span class="sxs-lookup"><span data-stu-id="68a46-183">**table_operation::replace_entity** operations will fail if hello entity has been changed since it was retrieved from hello server.</span></span> <span data-ttu-id="68a46-184">Dessutom måste du hämta hello entitet från hello servern först för **table_operation::replace_entity** toobe lyckades.</span><span class="sxs-lookup"><span data-stu-id="68a46-184">Furthermore, you must retrieve hello entity from hello server first in order for **table_operation::replace_entity** toobe successful.</span></span> <span data-ttu-id="68a46-185">Ibland, men du inte vet om hello entiteten finns på hello-servern och hello aktuella värden som lagras i den är irrelevanta – dina uppdateringar ska skriva över alla.</span><span class="sxs-lookup"><span data-stu-id="68a46-185">Sometimes, however, you don't know if hello entity exists on hello server and hello current values stored in it are irrelevant—your update should overwrite them all.</span></span> <span data-ttu-id="68a46-186">tooaccomplish, använder du en **table_operation::insert_or_replace_entity** igen.</span><span class="sxs-lookup"><span data-stu-id="68a46-186">tooaccomplish this, you would use a **table_operation::insert_or_replace_entity** operation.</span></span> <span data-ttu-id="68a46-187">Den här åtgärden infogar hello entiteten om den finns inte eller ersätter den om den finns, oavsett när hello senaste uppdateringen gjordes.</span><span class="sxs-lookup"><span data-stu-id="68a46-187">This operation inserts hello entity if it doesn't exist, or replaces it if it does, regardless of when hello last update was made.</span></span> <span data-ttu-id="68a46-188">I följande kodexempel hello, hello kundentiteten för Jeff Smith hämtas fortfarande, men den sparas sedan tillbaka toohello servern via **table_operation::insert_or_replace_entity**.</span><span class="sxs-lookup"><span data-stu-id="68a46-188">In hello following code example, hello customer entity for Jeff Smith is still retrieved, but it is then saved back toohello server via **table_operation::insert_or_replace_entity**.</span></span> <span data-ttu-id="68a46-189">Uppdateringar som görs toohello entiteten mellan hello hämtningen och uppdateringen igen skrivs över.</span><span class="sxs-lookup"><span data-stu-id="68a46-189">Any updates made toohello entity between hello retrieval and update operation will be overwritten.</span></span>  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Insert-or-replace an entity.
azure::storage::table_entity entity_to_insert_or_replace(U("Smith"), U("Jeff"));
azure::storage::table_entity::properties_type& properties_to_insert_or_replace = entity_to_insert_or_replace.properties();

properties_to_insert_or_replace.reserve(2);

// Specify a phone number.
properties_to_insert_or_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0107"));

// Specify an email address.
properties_to_insert_or_replace[U("Email")] = azure::storage::entity_property(U("Jeffsm@contoso.com"));

// Create an operation tooinsert-or-replace hello entity.
azure::storage::table_operation insert_or_replace_operation = azure::storage::table_operation::insert_or_replace_entity(entity_to_insert_or_replace);

// Submit hello operation toohello Table service.
azure::storage::table_result insert_or_replace_result = table.execute(insert_or_replace_operation);
```

## <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="68a46-190">Fråga en deluppsättning entitetsegenskaper</span><span class="sxs-lookup"><span data-stu-id="68a46-190">Query a subset of entity properties</span></span>
<span data-ttu-id="68a46-191">En tooa frågetabellen kan hämta bara några få egenskaper från en entitet.</span><span class="sxs-lookup"><span data-stu-id="68a46-191">A query tooa table can retrieve just a few properties from an entity.</span></span> <span data-ttu-id="68a46-192">hello frågan i hello följande kod använder hello **table_query::set_select_columns** metoden tooreturn endast hello e-postadresserna för entiteter i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="68a46-192">hello query in hello following code uses hello **table_query::set_select_columns** method tooreturn only hello email addresses of entities in hello table.</span></span>  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Define hello query, and select only hello Email property.
azure::storage::table_query query;
std::vector<utility::string_t> columns;

columns.push_back(U("Email"));
query.set_select_columns(columns);

// Execute hello query.
azure::storage::table_query_iterator it = table.execute_query(query);

// Display hello results.
azure::storage::table_query_iterator end_of_results;
for (; it != end_of_results; ++it)
{
    std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key();

    const azure::storage::table_entity::properties_type& properties = it->properties();
    for (auto prop_it = properties.begin(); prop_it != properties.end(); ++prop_it)
    {
        std::wcout << ", " << prop_it->first << ": " << prop_it->second.str();
    }

    std::wcout << std::endl;
}
```

> [!NOTE]
> <span data-ttu-id="68a46-193">Frågar några egenskaper från en entitet är effektivare än att hämta alla egenskaper.</span><span class="sxs-lookup"><span data-stu-id="68a46-193">Querying a few properties from an entity is a more efficient operation than retrieving all properties.</span></span>
> 
> 

## <a name="delete-an-entity"></a><span data-ttu-id="68a46-194">Ta bort en entitet</span><span class="sxs-lookup"><span data-stu-id="68a46-194">Delete an entity</span></span>
<span data-ttu-id="68a46-195">Du kan enkelt ta bort en enhet när du har hämtat den.</span><span class="sxs-lookup"><span data-stu-id="68a46-195">You can easily delete an entity after you have retrieved it.</span></span> <span data-ttu-id="68a46-196">När hello entitet hämtas anropa **table_operation::delete_entity** med hello entiteten toodelete.</span><span class="sxs-lookup"><span data-stu-id="68a46-196">Once hello entity is retrieved, call **table_operation::delete_entity** with hello entity toodelete.</span></span> <span data-ttu-id="68a46-197">Anropa hello **cloud_table.execute** metod.</span><span class="sxs-lookup"><span data-stu-id="68a46-197">Then call hello **cloud_table.execute** method.</span></span> <span data-ttu-id="68a46-198">hello följande kod hämtar och tar bort en entitet med en partitionsnyckel för ”Smith” och en radnyckel av ”Jeff”.</span><span class="sxs-lookup"><span data-stu-id="68a46-198">hello following code retrieves and deletes an entity with a partition key of "Smith" and a row key of "Jeff".</span></span>  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create an operation tooretrieve hello entity with partition key of "Smith" and row key of "Jeff".
azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

// Create an operation toodelete hello entity.
azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

// Submit hello delete operation toohello Table service.
azure::storage::table_result delete_result = table.execute(delete_operation);  
```

## <a name="delete-a-table"></a><span data-ttu-id="68a46-199">Ta bort en tabell</span><span class="sxs-lookup"><span data-stu-id="68a46-199">Delete a table</span></span>
<span data-ttu-id="68a46-200">Följande kodexempel hello tar slutligen bort en tabell från ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="68a46-200">Finally, hello following code example deletes a table from a storage account.</span></span> <span data-ttu-id="68a46-201">En tabell som har tagits bort kommer att vara otillgänglig toobe återskapas under en tidsperiod efter hello borttagning.</span><span class="sxs-lookup"><span data-stu-id="68a46-201">A table that has been deleted will be unavailable toobe re-created for a period of time following hello deletion.</span></span>  

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for hello table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create an operation tooretrieve hello entity with partition key of "Smith" and row key of "Jeff".
azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

// Create an operation toodelete hello entity.
azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

// Submit hello delete operation toohello Table service.
azure::storage::table_result delete_result = table.execute(delete_operation);
```

## <a name="next-steps"></a><span data-ttu-id="68a46-202">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="68a46-202">Next steps</span></span>
<span data-ttu-id="68a46-203">Nu när du har lärt dig hello grunderna i table storage, följa dessa länkar toolearn mer om Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="68a46-203">Now that you've learned hello basics of table storage, follow these links toolearn more about Azure Storage:</span></span>  

* <span data-ttu-id="68a46-204">[Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en gratis, fristående app från Microsoft som möjliggör toowork visuellt med Azure Storage-data i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="68a46-204">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* [<span data-ttu-id="68a46-205">Hur toouse Blob storage från C++</span><span class="sxs-lookup"><span data-stu-id="68a46-205">How toouse Blob storage from C++</span></span>](../storage/blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [<span data-ttu-id="68a46-206">Hur toouse Queue storage från C++</span><span class="sxs-lookup"><span data-stu-id="68a46-206">How toouse Queue storage from C++</span></span>](../storage/queues/storage-c-plus-plus-how-to-use-queues.md)
* [<span data-ttu-id="68a46-207">Visa en lista med Azure Storage-resurser i C++</span><span class="sxs-lookup"><span data-stu-id="68a46-207">List Azure Storage resources in C++</span></span>](../storage/common/storage-c-plus-plus-enumeration.md)
* [<span data-ttu-id="68a46-208">Storage-klientbibliotek för C++-referens</span><span class="sxs-lookup"><span data-stu-id="68a46-208">Storage Client Library for C++ reference</span></span>](http://azure.github.io/azure-storage-cpp)
* [<span data-ttu-id="68a46-209">Azure Storage-dokumentation</span><span class="sxs-lookup"><span data-stu-id="68a46-209">Azure Storage documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)
