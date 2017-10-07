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
# <a name="how-toouse-table-storage-from-c"></a>Hur toouse Table storage från C++
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Översikt
Den här guiden visar hur tooperform vanliga scenarier med hjälp av hello Azure Table storage-tjänst. hello exemplen är skrivna i C++ och använda hello [Azure Storage-klientbibliotek för C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md). hello beskrivs scenarier där **skapa och ta bort en tabell** och **arbeta med tabellentiteter**.

> [!NOTE]
> Den här guiden riktad mot hello Azure Storage-klientbibliotek för C++ version 1.0.0 och senare. hello rekommenderas version är Storage-klientbibliotek 2.2.0, som är tillgängliga via [NuGet](http://www.nuget.org/packages/wastorage) eller [GitHub](https://github.com/Azure/azure-storage-cpp/).
> 
> 

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Skapa ett C++-program
I den här guiden använder lagringsfunktioner som kan köras i ett C++-program. toodo så behöver du tooinstall hello Azure Storage-klientbibliotek för C++ och skapa ett Azure storage-konto i din Azure-prenumeration.  

tooinstall hello Azure Storage-klientbibliotek för C++ kan du använda hello följande metoder:

* **Linux:** Följ instruktionerna på hello hello [Azure Storage-klientbibliotek för C++ viktigt](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) sidan.  
* **Windows:** i Visual Studio klickar du på **Verktyg > NuGet Package Manager > Package Manager-konsolen**. Typen hello följande kommando i hello [NuGet Package Manager-konsolen](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) och tryck på RETUR.  
  
     Install-Package wastorage

## <a name="configure-your-application-tooaccess-table-storage"></a>Konfigurera ditt program tooaccess tabellagring
Lägga till följande hello innehålla instruktioner toohello överkant hello C++ filen där du vill toouse hello Azure storage-API: er tooaccess tabeller:  

```cpp
#include <was/storage_account.h>
#include <was/table.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a>Ställ in en anslutningssträng för Azure storage
Ett Azure storage-klienten använder en lagring sträng toostore slutpunkter och autentiseringsuppgifter för åtkomst till data management services. Du måste ange hello lagringsanslutningssträngen i hello följande format när du kör ett klientprogram. Använd hello namnet på ditt konto och hello lagring lagringsåtkomstnyckel för hello storage-konto som anges i hello [Azure Portal](https://portal.azure.com) för hello *AccountName* och *AccountKey* värden. Information om lagringskonton och snabbtangenterna finns [om Azure storage-konton](../storage/common/storage-create-storage-account.md). Det här exemplet visar hur du kan deklarera en anslutningssträng för statiska fält toohold hello:  

```cpp
// Define hello connection string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

tootest ditt program i din lokala Windows-baserad dator kan du använda hello Azure [lagringsemulatorn](../storage/common/storage-use-emulator.md) som installeras med hello [Azure SDK](https://azure.microsoft.com/downloads/). hello storage-emulatorn är ett verktyg som simulerar hello Azure Blob, köer och tabellen tjänster som är tillgängliga på utvecklingsdatorn lokala. hello följande exempel visas hur du deklarera ett statiskt fält toohold hello anslutning sträng tooyour lokala storage-emulatorn:  

```cpp
// Define hello connection string with Azure storage emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

toostart hello Azure storage-emulatorn, klicka på hello **starta** eller tryck på knappen hello Windows-tangenten. Börja skriva **Azure Storage-emulatorn**, och välj sedan **Microsoft Azure Storage-emulatorn** hello listan med program.  

hello förutsätter följande exempel att du har använt någon av dessa två metoder tooget hello lagringsanslutningssträng.  

## <a name="retrieve-your-connection-string"></a>Hämta anslutningssträngen
Du kan använda hello **cloud_storage_account** klassen toorepresent kontoinformationen lagring. tooretrieve lagringen kontoinformation från hello anslutningssträngen för lagring kan du använda hello parse-metod.

```cpp
// Retrieve hello storage account from hello connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

Sedan hämta en referens tooa **cloud_table_client** class, som gör det möjligt att hämta referensobjekt för tabeller och entiteter som lagras i hello Table storage-tjänsten. hello följande kod skapar en **cloud_table_client** objektet med hjälp av hello konto lagringsobjekt vi hämta ovan:  

```cpp
// Create hello table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();
```

## <a name="create-a-table"></a>Skapa en tabell
En **cloud_table_client** objekt kan du få referensobjekt för tabeller och enheter. hello följande kod skapar en **cloud_table_client** objekt och använder den toocreate en ny tabell.

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

## <a name="add-an-entity-tooa-table"></a>Lägg till en entitet tooa tabell
tooadd en entitet tooa tabell, skapa en ny **table_entity** objekt och skickar den för**table_operation::insert_entity**. hello använder följande kod hello kundens förnamn som hello radnyckel och efternamn som partitionsnyckel hello. Tillsammans identifiera en entitets partition och radnyckel hello entiteten i hello tabell. Entiteter med samma partitionsnyckel kan frågas snabbare än med olika hello partitions nycklar, men med olika partitionsnycklar möjliggör större parallell åtgärd skalbarhet. Mer information finns i [Microsoft Azure storage checklistan för prestanda och skalbarhet](../storage/common/storage-performance-checklist.md).

hello följande kod skapar en ny instans av **table_entity** med vissa toobe för kund-data som lagras. Hej kod nästa anrop **table_operation::insert_entity** toocreate en **table_operation** objektet tooinsert en entitet i en tabell och associerats hello ny tabell entitet med den. Slutligen hello koden anropar hello execute-metoden på hello **cloud_table** objekt. Och hello nya **table_operation** skickar en begäran toohello tabellen service tooinsert hello nya kundentiteten i hello ”personer”-tabellen.  

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

## <a name="insert-a-batch-of-entities"></a>Infoga en batch med entiteter
Du kan infoga en batch med entiteter toohello tabelltjänsten i samma skrivåtgärd. hello följande kod skapar en **table_batch_operation** objekt och lägger sedan till tre Infoga operations tooit. Varje insert-åtgärden har lagts till genom att skapa en ny enhetsobjekt dess inställningsvärden och sedan anropa hello Infoga metoden på hello **table_batch_operation** objekt tooassociate hello entitet med en ny infoga igen. Sedan **cloud_table.execute** tooexecute hello åtgärden anropas.  

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

Vissa saker toonote om batchåtgärder:  

* Du kan utföra in too100 insert, delete, merge, Ersätt, insert eller dokument och infoga eller ersätta åtgärder i vilken kombination som helst i en enskild batch.  
* En batchåtgärd kan ha en hämta-åtgärd om är det enda hello-åtgärd i hello batch.  
* Alla entiteter i samma batchåtgärd måste ha hello samma partitionsnyckel.  
* En batchåtgärd är begränsad tooa 4 MB nyttolast.  

## <a name="retrieve-all-entities-in-a-partition"></a>Hämta alla entiteter i en partition
tooquery en tabell efter alla entiteter i en partition, Använd en **table_query** objekt. hello anges följande kodexempel ett filter för entiteter där Partitionsnyckeln hello är ”Smith”. Det här exemplet skriver ut hello fälten för varje entitet i hello frågan resultat toohello-konsolen.  

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

hello frågan i det här exemplet ger alla hello entiteter som matchar hello filtervillkor. Om du har stora tabeller och behöver toodownload hello tabellentiteter ofta, rekommenderar vi att du lagrar data i Azure storage-blobbar i stället.

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Hämta ett intervall med enheter i en partition
Om du inte vill tooquery alla hello entiteter i en partition kan ange du ett intervall genom att kombinera hello partitionsnyckelfiltret med ett radnyckelfilter. hello följande kodexempel används två filter tooget alla entiteter i partitionen ”Smith” där hello Radnyckeln (Förnamn) börjar med en bokstav före ”E” i alfabetet hello och skriver sedan ut hello frågeresultat.  

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

## <a name="retrieve-a-single-entity"></a>Hämta en enda entitet
Du kan skriva en fråga tooretrieve en enda, specifik entitet. hello följande kod används **table_operation::retrieve_entity** toospecify hello kunden Jeff Smith. Den här metoden returnerar endast en entitet i stället för en samling och hello värdet som returneras i **table_result**. Ange både partitions- och nycklar i en fråga är hello snabbaste sättet tooretrieve en enda entitet från tabelltjänsten hello.  

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

## <a name="replace-an-entity"></a>Ersätta en entitet
Hämta den från tabelltjänsten hello tooreplace en entitet, ändra hello enhetsobjekt och sedan tillbaka toohello tabelltjänsten spara hello ändringar. hello ändrar följande kod en befintlig kunds telefonnummer och e-postadress. Istället för att ringa **table_operation::insert_entity**använder den här koden **table_operation::replace_entity**. Detta medför hello entiteten toobe ersätts helt på hello-servern om hello entiteten på hello-servern har ändrats sedan den hämtades, i så fall hello åtgärden misslyckas. Det här felet är tooprevent ditt program oavsiktligt skriver över en ändring som gjorts mellan hello hämtning och uppdatering av en annan komponent i ditt program. hello korrekt hantering av det här felet är tooretrieve hello entiteten igen, gör dina ändringar (om det är fortfarande giltigt) och sedan köra en **table_operation::replace_entity** igen. hello nästa avsnitt visar hur toooverride problemet.  

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

## <a name="insert-or-replace-an-entity"></a>Infoga eller ersätta en entitet
**table_operation::replace_entity** åtgärder misslyckas om hello entiteten har ändrats sedan den hämtades från hello-servern. Dessutom måste du hämta hello entitet från hello servern först för **table_operation::replace_entity** toobe lyckades. Ibland, men du inte vet om hello entiteten finns på hello-servern och hello aktuella värden som lagras i den är irrelevanta – dina uppdateringar ska skriva över alla. tooaccomplish, använder du en **table_operation::insert_or_replace_entity** igen. Den här åtgärden infogar hello entiteten om den finns inte eller ersätter den om den finns, oavsett när hello senaste uppdateringen gjordes. I följande kodexempel hello, hello kundentiteten för Jeff Smith hämtas fortfarande, men den sparas sedan tillbaka toohello servern via **table_operation::insert_or_replace_entity**. Uppdateringar som görs toohello entiteten mellan hello hämtningen och uppdateringen igen skrivs över.  

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

## <a name="query-a-subset-of-entity-properties"></a>Fråga en deluppsättning entitetsegenskaper
En tooa frågetabellen kan hämta bara några få egenskaper från en entitet. hello frågan i hello följande kod använder hello **table_query::set_select_columns** metoden tooreturn endast hello e-postadresserna för entiteter i hello tabell.  

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
> Frågar några egenskaper från en entitet är effektivare än att hämta alla egenskaper.
> 
> 

## <a name="delete-an-entity"></a>Ta bort en entitet
Du kan enkelt ta bort en enhet när du har hämtat den. När hello entitet hämtas anropa **table_operation::delete_entity** med hello entiteten toodelete. Anropa hello **cloud_table.execute** metod. hello följande kod hämtar och tar bort en entitet med en partitionsnyckel för ”Smith” och en radnyckel av ”Jeff”.  

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

## <a name="delete-a-table"></a>Ta bort en tabell
Följande kodexempel hello tar slutligen bort en tabell från ett lagringskonto. En tabell som har tagits bort kommer att vara otillgänglig toobe återskapas under en tidsperiod efter hello borttagning.  

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

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig hello grunderna i table storage, följa dessa länkar toolearn mer om Azure Storage:  

* [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en gratis, fristående app från Microsoft som möjliggör toowork visuellt med Azure Storage-data i Windows, macOS och Linux.
* [Hur toouse Blob storage från C++](../storage/blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [Hur toouse Queue storage från C++](../storage/queues/storage-c-plus-plus-how-to-use-queues.md)
* [Visa en lista med Azure Storage-resurser i C++](../storage/common/storage-c-plus-plus-enumeration.md)
* [Storage-klientbibliotek för C++-referens](http://azure.github.io/azure-storage-cpp)
* [Azure Storage-dokumentation](https://azure.microsoft.com/documentation/services/storage/)
