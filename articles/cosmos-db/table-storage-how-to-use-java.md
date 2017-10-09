---
title: "aaaHow toouse Table storage från Java | Microsoft Docs"
description: Lagra strukturerade data i hello molnet med Azure Table storage, en NoSQL-databas.
services: cosmos-db
documentationcenter: java
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 45145189-e67f-4ca6-b15d-43af7bfd3f97
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.openlocfilehash: 20d03e867219cc254da8dad37cf3cf61bca65671
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-table-storage-from-java"></a><span data-ttu-id="0d498-103">Hur toouse Table storage från Java</span><span class="sxs-lookup"><span data-stu-id="0d498-103">How toouse Table storage from Java</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="0d498-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="0d498-104">Overview</span></span>
<span data-ttu-id="0d498-105">Den här guiden visar hur tooperform vanliga scenarier med hjälp av hello Azure Table storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="0d498-105">This guide will show you how tooperform common scenarios using hello Azure Table storage service.</span></span> <span data-ttu-id="0d498-106">hello exemplen är skrivna i Java och använder hello [Azure Storage SDK för Java][Azure Storage SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="0d498-106">hello samples are written in Java and use hello [Azure Storage SDK for Java][Azure Storage SDK for Java].</span></span> <span data-ttu-id="0d498-107">hello beskrivs scenarier där **skapar**, **lista**, och **bort** tabeller, samt **infoga**,  **fråga**, **ändra**, och **bort** entiteter i en tabell.</span><span class="sxs-lookup"><span data-stu-id="0d498-107">hello scenarios covered include **creating**, **listing**, and **deleting** tables, as well as **inserting**, **querying**, **modifying**, and **deleting** entities in a table.</span></span> <span data-ttu-id="0d498-108">Mer information om tabeller finns hello [nästa steg](#Next-Steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0d498-108">For more information on tables, see hello [Next steps](#Next-Steps) section.</span></span>

<span data-ttu-id="0d498-109">Obs: En SDK är tillgänglig för utvecklare som använder Azure Storage på Android-enheter.</span><span class="sxs-lookup"><span data-stu-id="0d498-109">Note: An SDK is available for developers who are using Azure Storage on Android devices.</span></span> <span data-ttu-id="0d498-110">Mer information finns i hello [Azure Storage SDK för Android][Azure Storage SDK for Android].</span><span class="sxs-lookup"><span data-stu-id="0d498-110">For more information, see hello [Azure Storage SDK for Android][Azure Storage SDK for Android].</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a><span data-ttu-id="0d498-111">Skapa ett Java-program</span><span class="sxs-lookup"><span data-stu-id="0d498-111">Create a Java application</span></span>
<span data-ttu-id="0d498-112">I den här guiden använder lagringsfunktioner som kan köras i en Java-program lokalt eller i kod som körs i en webbroll eller worker-rollen i Azure.</span><span class="sxs-lookup"><span data-stu-id="0d498-112">In this guide, you will use storage features which can be run within a Java application locally, or in code running within a web role or worker role in Azure.</span></span>

<span data-ttu-id="0d498-113">toodo så behöver du tooinstall hello Java Development Kit (JDK) och skapa ett Azure storage-konto i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0d498-113">toodo so, you will need tooinstall hello Java Development Kit (JDK) and create an Azure storage account in your Azure subscription.</span></span> <span data-ttu-id="0d498-114">När du har gjort det, måste tooverify utvecklingssystemet uppfyller minimikraven för hello och beroenden som är listade i hello [Azure Storage SDK för Java] [ Azure Storage SDK for Java] databasen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="0d498-114">Once you have done so, you will need tooverify that your development system meets hello minimum requirements and dependencies which are listed in hello [Azure Storage SDK for Java][Azure Storage SDK for Java] repository on GitHub.</span></span> <span data-ttu-id="0d498-115">Om datorn uppfyller dessa krav, kan du följa hello instruktioner för att hämta och installera hello Azure Storage-biblioteken för Java på datorn från den lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="0d498-115">If your system meets those requirements, you can follow hello instructions for downloading and installing hello Azure Storage Libraries for Java on your system from that repository.</span></span> <span data-ttu-id="0d498-116">När du har slutfört uppgifterna, kommer du att kunna toocreate ett Java-program som använder hello exemplen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="0d498-116">Once you have completed those tasks, you will be able toocreate a Java application which uses hello examples in this article.</span></span>

## <a name="configure-your-application-tooaccess-table-storage"></a><span data-ttu-id="0d498-117">Konfigurera ditt program tooaccess tabellagring</span><span class="sxs-lookup"><span data-stu-id="0d498-117">Configure your application tooaccess table storage</span></span>
<span data-ttu-id="0d498-118">Lägg till hello efter import instruktioner toohello överkant hello Java filen där du vill att toouse Microsoft Azure storage-API: er tooaccess tabeller:</span><span class="sxs-lookup"><span data-stu-id="0d498-118">Add hello following import statements toohello top of hello Java file where you want toouse Microsoft Azure storage APIs tooaccess tables:</span></span>

```java
// Include hello following imports toouse table APIs
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.table.*;
import com.microsoft.azure.storage.table.TableQuery.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="0d498-119">Ställ in en anslutningssträng för Azure storage</span><span class="sxs-lookup"><span data-stu-id="0d498-119">Set up an Azure storage connection string</span></span>
<span data-ttu-id="0d498-120">Ett Azure storage-klienten använder en lagring sträng toostore slutpunkter och autentiseringsuppgifter för åtkomst till data management services.</span><span class="sxs-lookup"><span data-stu-id="0d498-120">An Azure storage client uses a storage connection string toostore endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="0d498-121">När den körs i ett klientprogram, måste du ange hello lagringsanslutningssträngen i hello följande format, med hello namnet på ditt lagringskonto och hello primärnyckeln för hello storage-konto som anges i hello [Azure-portalen](https://portal.azure.com)för hello *AccountName* och *AccountKey* värden.</span><span class="sxs-lookup"><span data-stu-id="0d498-121">When running in a client application, you must provide hello storage connection string in hello following format, using hello name of your storage account and hello Primary access key for hello storage account listed in hello [Azure portal](https://portal.azure.com) for hello *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="0d498-122">Det här exemplet visar hur du kan deklarera en anslutningssträng för statiska fält toohold hello:</span><span class="sxs-lookup"><span data-stu-id="0d498-122">This example shows how you can declare a static field toohold hello connection string:</span></span>

```java
// Define hello connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

<span data-ttu-id="0d498-123">I ett program som körs inom en roll i Microsoft Azure, kan den här strängen lagras i hello tjänstkonfigurationsfilen *ServiceConfiguration.cscfg*, och kan nås med ett samtal toohello  **RoleEnvironment.getConfigurationSettings** metod.</span><span class="sxs-lookup"><span data-stu-id="0d498-123">In an application running within a role in Microsoft Azure, this string can be stored in hello service configuration file, *ServiceConfiguration.cscfg*, and can be accessed with a call toohello **RoleEnvironment.getConfigurationSettings** method.</span></span> <span data-ttu-id="0d498-124">Här är ett exempel på hello anslutningssträngen från en **inställningen** element med namnet *StorageConnectionString* i konfigurationsfilen för hello-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="0d498-124">Here's an example of getting hello connection string from a **Setting** element named *StorageConnectionString* in hello service configuration file:</span></span>

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

<span data-ttu-id="0d498-125">hello förutsätter följande exempel att du har använt någon av dessa två metoder tooget hello lagringsanslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="0d498-125">hello following samples assume that you have used one of these two methods tooget hello storage connection string.</span></span>

## <a name="how-to-create-a-table"></a><span data-ttu-id="0d498-126">Så här: skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="0d498-126">How to: Create a table</span></span>
<span data-ttu-id="0d498-127">En **CloudTableClient** objekt kan du få referensobjekt för tabeller och enheter.</span><span class="sxs-lookup"><span data-stu-id="0d498-127">A **CloudTableClient** object lets you get reference objects for tables and entities.</span></span> <span data-ttu-id="0d498-128">hello följande kod skapar en **CloudTableClient** objekt och använder den toocreate en ny **CloudTable** -objektet som representerar en tabell med namnet ”personer”.</span><span class="sxs-lookup"><span data-stu-id="0d498-128">hello following code creates a **CloudTableClient** object and uses it toocreate a new **CloudTable** object which represents a table named "people".</span></span> <span data-ttu-id="0d498-129">(Observera: det finns andra sätt toocreate **CloudStorageAccount** objekt; mer information finns i **CloudStorageAccount** i hello [referens för Azure Storage Client SDK].)</span><span class="sxs-lookup"><span data-stu-id="0d498-129">(Note: There are additional ways toocreate **CloudStorageAccount** objects; for more information, see **CloudStorageAccount** in hello [Azure Storage Client SDK Reference].)</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create hello table if it doesn't exist.
    String tableName = "people";
    CloudTable cloudTable = tableClient.getTableReference(tableName);
    cloudTable.createIfNotExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-list-hello-tables"></a><span data-ttu-id="0d498-130">Så här: Visa hello register</span><span class="sxs-lookup"><span data-stu-id="0d498-130">How to: List hello tables</span></span>
<span data-ttu-id="0d498-131">tooget en lista med tabeller, anrop hello **CloudTableClient.listTables()** metoden tooretrieve en iterable lista över tabellnamn.</span><span class="sxs-lookup"><span data-stu-id="0d498-131">tooget a list of tables, call hello **CloudTableClient.listTables()** method tooretrieve an iterable list of table names.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Loop through hello collection of table names.
    for (String table : tableClient.listTables())
    {
        // Output each table name.
        System.out.println(table);
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-add-an-entity-tooa-table"></a><span data-ttu-id="0d498-132">Så här: Lägg till en entitet tooa tabell</span><span class="sxs-lookup"><span data-stu-id="0d498-132">How to: Add an entity tooa table</span></span>
<span data-ttu-id="0d498-133">Entiteter mappar tooJava objekt med hjälp av en anpassad klass som implementerar **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="0d498-133">Entities map tooJava objects using a custom class implementing **TableEntity**.</span></span> <span data-ttu-id="0d498-134">För enkelhetens skull hello **TableServiceEntity** klassen implementerar **TableEntity** och använder reflektion toomap egenskaper toogetter och set-metoder med namnet för hello egenskaper.</span><span class="sxs-lookup"><span data-stu-id="0d498-134">For convenience, hello **TableServiceEntity** class implements **TableEntity** and uses reflection toomap properties toogetter and setter methods named for hello properties.</span></span> <span data-ttu-id="0d498-135">tooadd en entitet tooa tabell först skapa en klass som definierar hello egenskaperna för entiteten.</span><span class="sxs-lookup"><span data-stu-id="0d498-135">tooadd an entity tooa table, first create a class that defines hello properties of your entity.</span></span> <span data-ttu-id="0d498-136">hello definierar följande kod en entitetsklass som använder hello kundens förnamn som hello radnyckel och efternamn som partitionsnyckel hello.</span><span class="sxs-lookup"><span data-stu-id="0d498-136">hello following code defines an entity class which uses hello customer's first name as hello row key, and last name as hello partition key.</span></span> <span data-ttu-id="0d498-137">Tillsammans identifiera en entitets partition och radnyckel hello entiteten i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="0d498-137">Together, an entity's partition and row key uniquely identify hello entity in hello table.</span></span> <span data-ttu-id="0d498-138">Entiteter med samma partitionsnyckel kan frågas snabbare än de som har olika partitionsnycklar hello.</span><span class="sxs-lookup"><span data-stu-id="0d498-138">Entities with hello same partition key can be queried faster than those with different partition keys.</span></span>

```java
public class CustomerEntity extends TableServiceEntity {
    public CustomerEntity(String lastName, String firstName) {
        this.partitionKey = lastName;
        this.rowKey = firstName;
    }

    public CustomerEntity() { }

    String email;
    String phoneNumber;

    public String getEmail() {
        return this.email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getPhoneNumber() {
        return this.phoneNumber;
    }

    public void setPhoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
    }
}
```

<span data-ttu-id="0d498-139">Tabellåtgärder som rör entiteter kräver en **TableOperation** objekt.</span><span class="sxs-lookup"><span data-stu-id="0d498-139">Table operations involving entities require a **TableOperation** object.</span></span> <span data-ttu-id="0d498-140">Det här objektet definierar hello åtgärden toobe utförs på en enhet som kan köras med en **CloudTable** objekt.</span><span class="sxs-lookup"><span data-stu-id="0d498-140">This object defines hello operation toobe performed on an entity, which can be executed with a **CloudTable** object.</span></span> <span data-ttu-id="0d498-141">hello följande kod skapar en ny instans av hello **CustomerEntity** klass med vissa toobe för kund-data som lagras.</span><span class="sxs-lookup"><span data-stu-id="0d498-141">hello following code creates a new instance of hello **CustomerEntity** class with some customer data toobe stored.</span></span> <span data-ttu-id="0d498-142">Hej kod nästa anrop **TableOperation.insertOrReplace** toocreate en **TableOperation** objektet tooinsert en entitet i en tabell och associerats hello nya **CustomerEntity**med den.</span><span class="sxs-lookup"><span data-stu-id="0d498-142">hello code next calls **TableOperation.insertOrReplace** toocreate a **TableOperation** object tooinsert an entity into a table, and associates hello new **CustomerEntity** with it.</span></span> <span data-ttu-id="0d498-143">Slutligen hello koden anropar hello **köra** metod på hello **CloudTable** objekt som anger hello ”personer” tabellen och nya hello **TableOperation**, som sedan skickar en begära toohello storage service tooinsert hello nya kundentiteten i hello ”personer” tabell eller ersätta hello entiteten om den redan finns.</span><span class="sxs-lookup"><span data-stu-id="0d498-143">Finally, hello code calls hello **execute** method on hello **CloudTable** object, specifying hello "people" table and hello new **TableOperation**, which then sends a request toohello storage service tooinsert hello new customer entity into hello "people" table, or replace hello entity if it already exists.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.setEmail("Walter@contoso.com");
    customer1.setPhoneNumber("425-555-0101");

    // Create an operation tooadd hello new customer toohello people table.
    TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

    // Submit hello operation toohello table service.
    cloudTable.execute(insertCustomer1);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-insert-a-batch-of-entities"></a><span data-ttu-id="0d498-144">Så här: Infoga en batch med entiteter</span><span class="sxs-lookup"><span data-stu-id="0d498-144">How to: Insert a batch of entities</span></span>
<span data-ttu-id="0d498-145">Du kan infoga en batch med entiteter toohello tabelltjänsten i samma skrivåtgärd.</span><span class="sxs-lookup"><span data-stu-id="0d498-145">You can insert a batch of entities toohello table service in one write operation.</span></span> <span data-ttu-id="0d498-146">hello följande kod skapar en **TableBatchOperation** objekt och sedan lägger till tre Infoga operations tooit.</span><span class="sxs-lookup"><span data-stu-id="0d498-146">hello following code creates a **TableBatchOperation** object, then adds three insert operations tooit.</span></span> <span data-ttu-id="0d498-147">Varje infogningen har lagts till genom att skapa ett nytt enhetsobjekt, dess inställningsvärden och sedan anropa hello **infoga** metod på hello **TableBatchOperation** tooassociate hello entitet med ett nytt objekt Infoga igen.</span><span class="sxs-lookup"><span data-stu-id="0d498-147">Each insert operation is added by creating a new entity object, setting its values, and then calling hello **insert** method on hello **TableBatchOperation** object tooassociate hello entity with a new insert operation.</span></span> <span data-ttu-id="0d498-148">Hej koden anropar **köra** på hello **CloudTable** objekt som anger hello ”personer” tabell och hello **TableBatchOperation** -objekt som skickar hello batch för tabellen Operations toohello lagringstjänsten i en enskild begäran.</span><span class="sxs-lookup"><span data-stu-id="0d498-148">Then hello code calls **execute** on hello **CloudTable** object, specifying hello "people" table and hello **TableBatchOperation** object, which sends hello batch of table operations toohello storage service in a single request.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Define a batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a customer entity tooadd toohello table.
    CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
    customer.setEmail("Jeff@contoso.com");
    customer.setPhoneNumber("425-555-0104");
    batchOperation.insertOrReplace(customer);

    // Create another customer entity tooadd toohello table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.setEmail("Ben@contoso.com");
    customer2.setPhoneNumber("425-555-0102");
    batchOperation.insertOrReplace(customer2);

    // Create a third customer entity tooadd toohello table.
    CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
    customer3.setEmail("Denise@contoso.com");
    customer3.setPhoneNumber("425-555-0103");
    batchOperation.insertOrReplace(customer3);

    // Execute hello batch of operations on hello "people" table.
    cloudTable.execute(batchOperation);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

<span data-ttu-id="0d498-149">Vissa saker toonote om batchåtgärder:</span><span class="sxs-lookup"><span data-stu-id="0d498-149">Some things toonote on batch operations:</span></span>

* <span data-ttu-id="0d498-150">Du kan utföra in too100 insert-, delete, merge, Ersätt, insert eller sammanfoga, och infoga eller ersätta åtgärder i vilken kombination som helst i en enskild batch.</span><span class="sxs-lookup"><span data-stu-id="0d498-150">You can perform up too100 insert, delete, merge, replace, insert or merge, and insert or replace operations in any combination in a single batch.</span></span>
* <span data-ttu-id="0d498-151">En batchåtgärd kan ha en hämta-åtgärd om är det enda hello-åtgärd i hello batch.</span><span class="sxs-lookup"><span data-stu-id="0d498-151">A batch operation can have a retrieve operation, if it is hello only operation in hello batch.</span></span>
* <span data-ttu-id="0d498-152">Alla entiteter i samma batchåtgärd måste ha hello samma partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="0d498-152">All entities in a single batch operation must have hello same partition key.</span></span>
* <span data-ttu-id="0d498-153">En batchåtgärd är begränsad tooa 4MB nyttolast.</span><span class="sxs-lookup"><span data-stu-id="0d498-153">A batch operation is limited tooa 4MB data payload.</span></span>

## <a name="how-to-retrieve-all-entities-in-a-partition"></a><span data-ttu-id="0d498-154">Så här: hämta alla entiteter i en partition</span><span class="sxs-lookup"><span data-stu-id="0d498-154">How to: Retrieve all entities in a partition</span></span>
<span data-ttu-id="0d498-155">tooquery en tabell för entiteter i en partition som du kan använda en **TableQuery**.</span><span class="sxs-lookup"><span data-stu-id="0d498-155">tooquery a table for entities in a partition, you can use a **TableQuery**.</span></span> <span data-ttu-id="0d498-156">Anropa **TableQuery.from** toocreate en fråga på en viss tabell som returnerar en angiven resultattyp.</span><span class="sxs-lookup"><span data-stu-id="0d498-156">Call **TableQuery.from** toocreate a query on a particular table that returns a specified result type.</span></span> <span data-ttu-id="0d498-157">hello anger följande kod ett filter för entiteter där Partitionsnyckeln hello är ”Smith”.</span><span class="sxs-lookup"><span data-stu-id="0d498-157">hello following code specifies a filter for entities where 'Smith' is hello partition key.</span></span> <span data-ttu-id="0d498-158">**TableQuery.generateFilterCondition** är en hjälpmetod toocreate filter för frågor.</span><span class="sxs-lookup"><span data-stu-id="0d498-158">**TableQuery.generateFilterCondition** is a helper method toocreate filters for queries.</span></span> <span data-ttu-id="0d498-159">Anropa **där** för hello-referens som returneras av hello **TableQuery.from** toohello för metoden tooapply hello filterfråga.</span><span class="sxs-lookup"><span data-stu-id="0d498-159">Call **where** on hello reference returned by hello **TableQuery.from** method tooapply hello filter toohello query.</span></span> <span data-ttu-id="0d498-160">När hello-frågan körs med ett anrop för**köra** på hello **CloudTable** objekt returneras en **Iterator** med hello **CustomerEntity**leda typ har angetts.</span><span class="sxs-lookup"><span data-stu-id="0d498-160">When hello query is executed with a call too**execute** on hello **CloudTable** object, it returns an **Iterator** with hello **CustomerEntity** result type specified.</span></span> <span data-ttu-id="0d498-161">Du kan sedan använda hello **Iterator** returneras i en för varje loop tooconsume hello resultat.</span><span class="sxs-lookup"><span data-stu-id="0d498-161">You can then use hello **Iterator** returned in a for each loop tooconsume hello results.</span></span> <span data-ttu-id="0d498-162">Den här koden skriver ut hello fälten för varje entitet i hello frågan resultat toohello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="0d498-162">This code prints hello fields of each entity in hello query results toohello console.</span></span>

```java
try
{
    // Define constants for filters.
    final String PARTITION_KEY = "PartitionKey";
    final String ROW_KEY = "RowKey";
    final String TIMESTAMP = "Timestamp";

    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a filter condition where hello partition key is "Smith".
    String partitionFilter = TableQuery.generateFilterCondition(
        PARTITION_KEY,
        QueryComparisons.EQUAL,
        "Smith");

    // Specify a partition query, using "Smith" as hello partition key filter.
    TableQuery<CustomerEntity> partitionQuery =
        TableQuery.from(CustomerEntity.class)
        .where(partitionFilter);

    // Loop through hello results, displaying information about hello entity.
    for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
        System.out.println(entity.getPartitionKey() +
            " " + entity.getRowKey() +
            "\t" + entity.getEmail() +
            "\t" + entity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-retrieve-a-range-of-entities-in-a-partition"></a><span data-ttu-id="0d498-163">Så här: hämta ett intervall med entiteter i en partition</span><span class="sxs-lookup"><span data-stu-id="0d498-163">How to: Retrieve a range of entities in a partition</span></span>
<span data-ttu-id="0d498-164">Om du inte vill tooquery alla hello entiteter i en partition kan ange du ett intervall med jämförelseoperatorer i ett filter.</span><span class="sxs-lookup"><span data-stu-id="0d498-164">If you don't want tooquery all hello entities in a partition, you can specify a range by using comparison operators in a filter.</span></span> <span data-ttu-id="0d498-165">Hej följande kod kombinerar två filtrerar tooget alla entiteter i partitionen ”Smith” där hello Radnyckeln (Förnamn) börjar med en bokstav in too'E' i hello alfabetet.</span><span class="sxs-lookup"><span data-stu-id="0d498-165">hello following code combines two filters tooget all entities in partition "Smith" where hello row key (first name) starts with a letter up too'E' in hello alphabet.</span></span> <span data-ttu-id="0d498-166">Sedan hello frågeresultatet skrivs ut.</span><span class="sxs-lookup"><span data-stu-id="0d498-166">Then it prints hello query results.</span></span> <span data-ttu-id="0d498-167">Om du använder hello entiteter tillagda toohello tabellen i hello batch infoga i den här guiden kan bara två entiteter returneras nu (Ben och Denise Smith). Jeff Smith ingår inte.</span><span class="sxs-lookup"><span data-stu-id="0d498-167">If you use hello entities added toohello table in hello batch insert section of this guide, only two entities are returned this time (Ben and Denise Smith); Jeff Smith is not included.</span></span>

```java
try
{
    // Define constants for filters.
    final String PARTITION_KEY = "PartitionKey";
    final String ROW_KEY = "RowKey";
    final String TIMESTAMP = "Timestamp";

    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a filter condition where hello partition key is "Smith".
    String partitionFilter = TableQuery.generateFilterCondition(
        PARTITION_KEY,
        QueryComparisons.EQUAL,
        "Smith");

    // Create a filter condition where hello row key is less than hello letter "E".
    String rowFilter = TableQuery.generateFilterCondition(
        ROW_KEY,
        QueryComparisons.LESS_THAN,
        "E");

    // Combine hello two conditions into a filter expression.
    String combinedFilter = TableQuery.combineFilters(partitionFilter,
        Operators.AND, rowFilter);

    // Specify a range query, using "Smith" as hello partition key,
    // with hello row key being up toohello letter "E".
    TableQuery<CustomerEntity> rangeQuery =
        TableQuery.from(CustomerEntity.class)
        .where(combinedFilter);

    // Loop through hello results, displaying information about hello entity
    for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
        System.out.println(entity.getPartitionKey() +
            " " + entity.getRowKey() +
            "\t" + entity.getEmail() +
            "\t" + entity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-retrieve-a-single-entity"></a><span data-ttu-id="0d498-168">Så här: hämta en enda entitet</span><span class="sxs-lookup"><span data-stu-id="0d498-168">How to: Retrieve a single entity</span></span>
<span data-ttu-id="0d498-169">Du kan skriva en fråga tooretrieve en enda, specifik entitet.</span><span class="sxs-lookup"><span data-stu-id="0d498-169">You can write a query tooretrieve a single, specific entity.</span></span> <span data-ttu-id="0d498-170">hello följande koden anropar **TableOperation.retrieve** med partition nyckel och raden nyckelparametrar toospecify hello kunden ”Jeff Smith”, istället för att skapa en **TableQuery** och använda filter toodo hello samma sak.</span><span class="sxs-lookup"><span data-stu-id="0d498-170">hello following code calls **TableOperation.retrieve** with partition key and row key parameters toospecify hello customer "Jeff Smith", instead of creating a **TableQuery** and using filters toodo hello same thing.</span></span> <span data-ttu-id="0d498-171">När den exekveras, hämta hello åtgärden returnerar endast en entitet i stället för en samling.</span><span class="sxs-lookup"><span data-stu-id="0d498-171">When executed, hello retrieve operation returns just one entity, rather than a collection.</span></span> <span data-ttu-id="0d498-172">Hej **getResultAsType** metoden kastar hello toohello resultattyp hello tilldelning mål, en **CustomerEntity** objekt.</span><span class="sxs-lookup"><span data-stu-id="0d498-172">hello **getResultAsType** method casts hello result toohello type of hello assignment target, a **CustomerEntity** object.</span></span> <span data-ttu-id="0d498-173">Om den här typen inte är kompatibel med hello typ har angetts för hello fråga genereras ett undantagsfel.</span><span class="sxs-lookup"><span data-stu-id="0d498-173">If this type is not compatible with hello type specified for hello query, an exception will be thrown.</span></span> <span data-ttu-id="0d498-174">Ett null-värde returneras om ingen enhet har en exakt partition och radnyckel matchar.</span><span class="sxs-lookup"><span data-stu-id="0d498-174">A null value is returned if no entity has an exact partition and row key match.</span></span> <span data-ttu-id="0d498-175">Ange både partitions- och nycklar i en fråga är hello snabbaste sättet tooretrieve en enda entitet från tabelltjänsten hello.</span><span class="sxs-lookup"><span data-stu-id="0d498-175">Specifying both partition and row keys in a query is hello fastest way tooretrieve a single entity from hello Table service.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Retrieve hello entity with partition key of "Smith" and row key of "Jeff"
    TableOperation retrieveSmithJeff =
        TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Submit hello operation toohello table service and get hello specific entity.
    CustomerEntity specificEntity =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Output hello entity.
    if (specificEntity != null)
    {
        System.out.println(specificEntity.getPartitionKey() +
            " " + specificEntity.getRowKey() +
            "\t" + specificEntity.getEmail() +
            "\t" + specificEntity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-modify-an-entity"></a><span data-ttu-id="0d498-176">Så här: ändra en entitet</span><span class="sxs-lookup"><span data-stu-id="0d498-176">How to: Modify an entity</span></span>
<span data-ttu-id="0d498-177">toomodify en entitet hämtar du den från tabelltjänsten hello, gör ändringar toohello enhetsobjekt och spara hello ändringar tillbaka toohello tabelltjänsten med en ersättning eller merge-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0d498-177">toomodify an entity, retrieve it from hello table service, make changes toohello entity object, and save hello changes back toohello table service with a replace or merge operation.</span></span> <span data-ttu-id="0d498-178">hello ändrar följande kod en befintlig kunds telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="0d498-178">hello following code changes an existing customer's phone number.</span></span> <span data-ttu-id="0d498-179">Istället för att ringa **TableOperation.insert** som vi gjorde tooinsert den här koden anropar **TableOperation.replace**.</span><span class="sxs-lookup"><span data-stu-id="0d498-179">Instead of calling **TableOperation.insert** like we did tooinsert, this code calls **TableOperation.replace**.</span></span> <span data-ttu-id="0d498-180">Hej **CloudTable.execute** metodanrop hello tabelltjänsten och hello entiteten ersätts om ett annat program ändrade den hello tidpunkt eftersom hämta av det här programmet.</span><span class="sxs-lookup"><span data-stu-id="0d498-180">hello **CloudTable.execute** method calls hello table service, and hello entity is replaced, unless another application changed it in hello time since this application retrieved it.</span></span> <span data-ttu-id="0d498-181">När det sker ett undantag och hello entitet måste hämtas, ändra och spara igen.</span><span class="sxs-lookup"><span data-stu-id="0d498-181">When that happens, an exception is thrown, and hello entity must be retrieved, modified, and saved again.</span></span> <span data-ttu-id="0d498-182">Optimistisk samtidighet gör mönstret är vanligt i ett distribuerat storage-system.</span><span class="sxs-lookup"><span data-stu-id="0d498-182">This optimistic concurrency retry pattern is common in a distributed storage system.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Retrieve hello entity with partition key of "Smith" and row key of "Jeff".
    TableOperation retrieveSmithJeff =
        TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Submit hello operation toohello table service and get hello specific entity.
    CustomerEntity specificEntity =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Specify a new phone number.
    specificEntity.setPhoneNumber("425-555-0105");

    // Create an operation tooreplace hello entity.
    TableOperation replaceEntity = TableOperation.replace(specificEntity);

    // Submit hello operation toohello table service.
    cloudTable.execute(replaceEntity);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-query-a-subset-of-entity-properties"></a><span data-ttu-id="0d498-183">Så här: fråga en deluppsättning Entitetsegenskaper</span><span class="sxs-lookup"><span data-stu-id="0d498-183">How to: Query a subset of entity properties</span></span>
<span data-ttu-id="0d498-184">En tooa frågetabellen kan hämta bara några få egenskaper från en entitet.</span><span class="sxs-lookup"><span data-stu-id="0d498-184">A query tooa table can retrieve just a few properties from an entity.</span></span> <span data-ttu-id="0d498-185">Den här tekniken, kallad projektion, minskar bandbredden och kan förbättra frågeprestanda, i synnerhet för stora entiteter.</span><span class="sxs-lookup"><span data-stu-id="0d498-185">This technique, called projection, reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="0d498-186">hello frågan i hello följande kod använder hello **Välj** metoden tooreturn endast hello e-postadresserna för entiteter i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="0d498-186">hello query in hello following code uses hello **select** method tooreturn only hello email addresses of entities in hello table.</span></span> <span data-ttu-id="0d498-187">hello resultat är planerad för en samling **sträng** med hello hjälp av en **EntityResolver**, som hello typkonvertering på hello entiteter som returnerades från hello-servern.</span><span class="sxs-lookup"><span data-stu-id="0d498-187">hello results are projected into a collection of **String** with hello help of an **EntityResolver**, which does hello type conversion on hello entities returned from hello server.</span></span> <span data-ttu-id="0d498-188">Du kan lära dig mer om projektion i [Azure-tabeller: Introducing Upsert and Query Projection][Azure Tables: Introducing Upsert and Query Projection].</span><span class="sxs-lookup"><span data-stu-id="0d498-188">You can learn more about projection in [Azure Tables: Introducing Upsert and Query Projection][Azure Tables: Introducing Upsert and Query Projection].</span></span> <span data-ttu-id="0d498-189">Observera att projektion inte stöds på hello lokala lagringsemulatorn, så den här koden körs bara när du använder ett konto på hello tabelltjänsten.</span><span class="sxs-lookup"><span data-stu-id="0d498-189">Note that projection is not supported on hello local storage emulator, so this code runs only when using an account on hello table service.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Define a projection query that retrieves only hello Email property
    TableQuery<CustomerEntity> projectionQuery =
        TableQuery.from(CustomerEntity.class)
        .select(new String[] {"Email"});

    // Define a Entity resolver tooproject hello entity toohello Email value.
    EntityResolver<String> emailResolver = new EntityResolver<String>() {
        @Override
        public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
            return properties.get("Email").getValueAsString();
        }
    };

    // Loop through hello results, displaying hello Email values.
    for (String projectedString :
        cloudTable.execute(projectionQuery, emailResolver)) {
            System.out.println(projectedString);
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-insert-or-replace-an-entity"></a><span data-ttu-id="0d498-190">Så här: infoga eller ersätta en entitet</span><span class="sxs-lookup"><span data-stu-id="0d498-190">How to: Insert or Replace an entity</span></span>
<span data-ttu-id="0d498-191">Ofta vill du tooadd en entitet tooa tabell utan att känna till om det finns redan i hello tabellen.</span><span class="sxs-lookup"><span data-stu-id="0d498-191">Often you want tooadd an entity tooa table without knowing if it already exists in hello table.</span></span> <span data-ttu-id="0d498-192">En insert-eller ersätta åtgärd kan du toomake en enskild begäran som infogar hello entiteten om den inte finns eller ersätta hello befintliga något om det inte.</span><span class="sxs-lookup"><span data-stu-id="0d498-192">An insert-or-replace operation allows you toomake a single request which will insert hello entity if it does not exist or replace hello existing one if it does.</span></span> <span data-ttu-id="0d498-193">Skapa i föregående exempel hello följande kod infogar eller ersätter hello entiteten för ”Viktor Harp”.</span><span class="sxs-lookup"><span data-stu-id="0d498-193">Building on prior examples, hello following code inserts or replaces hello entity for "Walter Harp".</span></span> <span data-ttu-id="0d498-194">När du har skapat en ny entitet den här koden anropar hello **TableOperation.insertOrReplace** metod.</span><span class="sxs-lookup"><span data-stu-id="0d498-194">After creating a new entity, this code calls hello **TableOperation.insertOrReplace** method.</span></span> <span data-ttu-id="0d498-195">Den här koden anropar sedan **köra** på hello **CloudTable** objekt med hello tabell och hello infoga eller ersätta Tabellåtgärd som hello parametrar.</span><span class="sxs-lookup"><span data-stu-id="0d498-195">This code then calls **execute** on hello **CloudTable** object with hello table and hello insert or replace table operation as hello parameters.</span></span> <span data-ttu-id="0d498-196">tooupdate bara en del av en entitet hello **TableOperation.insertOrMerge** metoden kan användas i stället.</span><span class="sxs-lookup"><span data-stu-id="0d498-196">tooupdate only part of an entity, hello **TableOperation.insertOrMerge** method can be used instead.</span></span> <span data-ttu-id="0d498-197">Observera att infoga eller ersätta inte stöds på hello lokala lagringsemulatorn, så den här koden körs bara när du använder ett konto på hello tabelltjänsten.</span><span class="sxs-lookup"><span data-stu-id="0d498-197">Note that insert-or-replace is not supported on hello local storage emulator, so this code runs only when using an account on hello table service.</span></span> <span data-ttu-id="0d498-198">Du kan lära dig mer om Infoga eller ersätta och infoga eller dokument i den här [Azure-tabeller: Introducing Upsert and Query Projection][Azure Tables: Introducing Upsert and Query Projection].</span><span class="sxs-lookup"><span data-stu-id="0d498-198">You can learn more about insert-or-replace and insert-or-merge in this [Azure Tables: Introducing Upsert and Query Projection][Azure Tables: Introducing Upsert and Query Projection].</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
    customer5.setEmail("Walter@contoso.com");
    customer5.setPhoneNumber("425-555-0106");

    // Create an operation tooadd hello new customer toohello people table.
    TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

    // Submit hello operation toohello table service.
    cloudTable.execute(insertCustomer5);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-an-entity"></a><span data-ttu-id="0d498-199">Så här: ta bort en entitet</span><span class="sxs-lookup"><span data-stu-id="0d498-199">How to: Delete an entity</span></span>
<span data-ttu-id="0d498-200">Du kan enkelt ta bort en enhet när du har hämtat den.</span><span class="sxs-lookup"><span data-stu-id="0d498-200">You can easily delete an entity after you have retrieved it.</span></span> <span data-ttu-id="0d498-201">När hello entitet hämtas anropa **TableOperation.delete** med hello entiteten toodelete.</span><span class="sxs-lookup"><span data-stu-id="0d498-201">Once hello entity is retrieved, call **TableOperation.delete** with hello entity toodelete.</span></span> <span data-ttu-id="0d498-202">Sedan anropar **köra** på hello **CloudTable** objekt.</span><span class="sxs-lookup"><span data-stu-id="0d498-202">Then call **execute** on hello **CloudTable** object.</span></span> <span data-ttu-id="0d498-203">hello följande kod hämtar och tar bort en kundentitet.</span><span class="sxs-lookup"><span data-stu-id="0d498-203">hello following code retrieves and deletes a customer entity.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create an operation tooretrieve hello entity with partition key of "Smith" and row key of "Jeff".
    TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Retrieve hello entity with partition key of "Smith" and row key of "Jeff".
    CustomerEntity entitySmithJeff =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Create an operation toodelete hello entity.
    TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

    // Submit hello delete operation toohello table service.
    cloudTable.execute(deleteSmithJeff);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-a-table"></a><span data-ttu-id="0d498-204">Så här: ta bort en tabell</span><span class="sxs-lookup"><span data-stu-id="0d498-204">How to: Delete a table</span></span>
<span data-ttu-id="0d498-205">Slutligen hello följande kod tar bort en tabell från ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="0d498-205">Finally, hello following code deletes a table from a storage account.</span></span> <span data-ttu-id="0d498-206">En tabell som har tagits bort kommer att vara otillgänglig toobe återskapas under en tidsperiod efter hello borttagning, vanligtvis mindre än 40 sekunder.</span><span class="sxs-lookup"><span data-stu-id="0d498-206">A table which has been deleted will be unavailable toobe recreated for a period of time following hello deletion, usually less than forty seconds.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Delete hello table and all its data if it exists.
    CloudTable cloudTable = tableClient.getTableReference("people");
    cloudTable.deleteIfExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```
[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="next-steps"></a><span data-ttu-id="0d498-207">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0d498-207">Next steps</span></span>

* <span data-ttu-id="0d498-208">[Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en gratis, fristående app från Microsoft som möjliggör toowork visuellt med Azure Storage-data i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="0d498-208">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="0d498-209">[Azure Storage SDK för Java][Azure Storage SDK for Java]</span><span class="sxs-lookup"><span data-stu-id="0d498-209">[Azure Storage SDK for Java][Azure Storage SDK for Java]</span></span>
* <span data-ttu-id="0d498-210">[Referens för Azure Storage Client SDK][referens för Azure Storage Client SDK]</span><span class="sxs-lookup"><span data-stu-id="0d498-210">[Azure Storage Client SDK Reference][Azure Storage Client SDK Reference]</span></span>
* <span data-ttu-id="0d498-211">[Azure Storage REST API][Azure Storage REST API]</span><span class="sxs-lookup"><span data-stu-id="0d498-211">[Azure Storage REST API][Azure Storage REST API]</span></span>
* <span data-ttu-id="0d498-212">[Azure Storage-teamets blogg][Azure Storage Team Blog]</span><span class="sxs-lookup"><span data-stu-id="0d498-212">[Azure Storage Team Blog][Azure Storage Team Blog]</span></span>

<span data-ttu-id="0d498-213">Mer information finns i [Azure för Java-utvecklare](/java/azure).</span><span class="sxs-lookup"><span data-stu-id="0d498-213">For more information, visit [Azure for Java developers](/java/azure).</span></span>

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[referens för Azure Storage Client SDK]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure Tables: Introducing Upsert and Query Projection]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
