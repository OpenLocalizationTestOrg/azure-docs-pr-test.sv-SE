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
# <a name="how-toouse-table-storage-from-java"></a>Hur toouse Table storage från Java
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Översikt
Den här guiden visar hur tooperform vanliga scenarier med hjälp av hello Azure Table storage-tjänst. hello exemplen är skrivna i Java och använder hello [Azure Storage SDK för Java][Azure Storage SDK for Java]. hello beskrivs scenarier där **skapar**, **lista**, och **bort** tabeller, samt **infoga**,  **fråga**, **ändra**, och **bort** entiteter i en tabell. Mer information om tabeller finns hello [nästa steg](#Next-Steps) avsnitt.

Obs: En SDK är tillgänglig för utvecklare som använder Azure Storage på Android-enheter. Mer information finns i hello [Azure Storage SDK för Android][Azure Storage SDK for Android].

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Skapa ett Java-program
I den här guiden använder lagringsfunktioner som kan köras i en Java-program lokalt eller i kod som körs i en webbroll eller worker-rollen i Azure.

toodo så behöver du tooinstall hello Java Development Kit (JDK) och skapa ett Azure storage-konto i din Azure-prenumeration. När du har gjort det, måste tooverify utvecklingssystemet uppfyller minimikraven för hello och beroenden som är listade i hello [Azure Storage SDK för Java] [ Azure Storage SDK for Java] databasen på GitHub. Om datorn uppfyller dessa krav, kan du följa hello instruktioner för att hämta och installera hello Azure Storage-biblioteken för Java på datorn från den lagringsplatsen. När du har slutfört uppgifterna, kommer du att kunna toocreate ett Java-program som använder hello exemplen i den här artikeln.

## <a name="configure-your-application-tooaccess-table-storage"></a>Konfigurera ditt program tooaccess tabellagring
Lägg till hello efter import instruktioner toohello överkant hello Java filen där du vill att toouse Microsoft Azure storage-API: er tooaccess tabeller:

```java
// Include hello following imports toouse table APIs
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.table.*;
import com.microsoft.azure.storage.table.TableQuery.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a>Ställ in en anslutningssträng för Azure storage
Ett Azure storage-klienten använder en lagring sträng toostore slutpunkter och autentiseringsuppgifter för åtkomst till data management services. När den körs i ett klientprogram, måste du ange hello lagringsanslutningssträngen i hello följande format, med hello namnet på ditt lagringskonto och hello primärnyckeln för hello storage-konto som anges i hello [Azure-portalen](https://portal.azure.com)för hello *AccountName* och *AccountKey* värden. Det här exemplet visar hur du kan deklarera en anslutningssträng för statiska fält toohold hello:

```java
// Define hello connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

I ett program som körs inom en roll i Microsoft Azure, kan den här strängen lagras i hello tjänstkonfigurationsfilen *ServiceConfiguration.cscfg*, och kan nås med ett samtal toohello  **RoleEnvironment.getConfigurationSettings** metod. Här är ett exempel på hello anslutningssträngen från en **inställningen** element med namnet *StorageConnectionString* i konfigurationsfilen för hello-tjänsten:

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

hello förutsätter följande exempel att du har använt någon av dessa två metoder tooget hello lagringsanslutningssträng.

## <a name="how-to-create-a-table"></a>Så här: skapa en tabell
En **CloudTableClient** objekt kan du få referensobjekt för tabeller och enheter. hello följande kod skapar en **CloudTableClient** objekt och använder den toocreate en ny **CloudTable** -objektet som representerar en tabell med namnet ”personer”. (Observera: det finns andra sätt toocreate **CloudStorageAccount** objekt; mer information finns i **CloudStorageAccount** i hello [referens för Azure Storage Client SDK].)

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

## <a name="how-to-list-hello-tables"></a>Så här: Visa hello register
tooget en lista med tabeller, anrop hello **CloudTableClient.listTables()** metoden tooretrieve en iterable lista över tabellnamn.

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

## <a name="how-to-add-an-entity-tooa-table"></a>Så här: Lägg till en entitet tooa tabell
Entiteter mappar tooJava objekt med hjälp av en anpassad klass som implementerar **TableEntity**. För enkelhetens skull hello **TableServiceEntity** klassen implementerar **TableEntity** och använder reflektion toomap egenskaper toogetter och set-metoder med namnet för hello egenskaper. tooadd en entitet tooa tabell först skapa en klass som definierar hello egenskaperna för entiteten. hello definierar följande kod en entitetsklass som använder hello kundens förnamn som hello radnyckel och efternamn som partitionsnyckel hello. Tillsammans identifiera en entitets partition och radnyckel hello entiteten i hello tabell. Entiteter med samma partitionsnyckel kan frågas snabbare än de som har olika partitionsnycklar hello.

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

Tabellåtgärder som rör entiteter kräver en **TableOperation** objekt. Det här objektet definierar hello åtgärden toobe utförs på en enhet som kan köras med en **CloudTable** objekt. hello följande kod skapar en ny instans av hello **CustomerEntity** klass med vissa toobe för kund-data som lagras. Hej kod nästa anrop **TableOperation.insertOrReplace** toocreate en **TableOperation** objektet tooinsert en entitet i en tabell och associerats hello nya **CustomerEntity**med den. Slutligen hello koden anropar hello **köra** metod på hello **CloudTable** objekt som anger hello ”personer” tabellen och nya hello **TableOperation**, som sedan skickar en begära toohello storage service tooinsert hello nya kundentiteten i hello ”personer” tabell eller ersätta hello entiteten om den redan finns.

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

## <a name="how-to-insert-a-batch-of-entities"></a>Så här: Infoga en batch med entiteter
Du kan infoga en batch med entiteter toohello tabelltjänsten i samma skrivåtgärd. hello följande kod skapar en **TableBatchOperation** objekt och sedan lägger till tre Infoga operations tooit. Varje infogningen har lagts till genom att skapa ett nytt enhetsobjekt, dess inställningsvärden och sedan anropa hello **infoga** metod på hello **TableBatchOperation** tooassociate hello entitet med ett nytt objekt Infoga igen. Hej koden anropar **köra** på hello **CloudTable** objekt som anger hello ”personer” tabell och hello **TableBatchOperation** -objekt som skickar hello batch för tabellen Operations toohello lagringstjänsten i en enskild begäran.

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

Vissa saker toonote om batchåtgärder:

* Du kan utföra in too100 insert-, delete, merge, Ersätt, insert eller sammanfoga, och infoga eller ersätta åtgärder i vilken kombination som helst i en enskild batch.
* En batchåtgärd kan ha en hämta-åtgärd om är det enda hello-åtgärd i hello batch.
* Alla entiteter i samma batchåtgärd måste ha hello samma partitionsnyckel.
* En batchåtgärd är begränsad tooa 4MB nyttolast.

## <a name="how-to-retrieve-all-entities-in-a-partition"></a>Så här: hämta alla entiteter i en partition
tooquery en tabell för entiteter i en partition som du kan använda en **TableQuery**. Anropa **TableQuery.from** toocreate en fråga på en viss tabell som returnerar en angiven resultattyp. hello anger följande kod ett filter för entiteter där Partitionsnyckeln hello är ”Smith”. **TableQuery.generateFilterCondition** är en hjälpmetod toocreate filter för frågor. Anropa **där** för hello-referens som returneras av hello **TableQuery.from** toohello för metoden tooapply hello filterfråga. När hello-frågan körs med ett anrop för**köra** på hello **CloudTable** objekt returneras en **Iterator** med hello **CustomerEntity**leda typ har angetts. Du kan sedan använda hello **Iterator** returneras i en för varje loop tooconsume hello resultat. Den här koden skriver ut hello fälten för varje entitet i hello frågan resultat toohello-konsolen.

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

## <a name="how-to-retrieve-a-range-of-entities-in-a-partition"></a>Så här: hämta ett intervall med entiteter i en partition
Om du inte vill tooquery alla hello entiteter i en partition kan ange du ett intervall med jämförelseoperatorer i ett filter. Hej följande kod kombinerar två filtrerar tooget alla entiteter i partitionen ”Smith” där hello Radnyckeln (Förnamn) börjar med en bokstav in too'E' i hello alfabetet. Sedan hello frågeresultatet skrivs ut. Om du använder hello entiteter tillagda toohello tabellen i hello batch infoga i den här guiden kan bara två entiteter returneras nu (Ben och Denise Smith). Jeff Smith ingår inte.

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

## <a name="how-to-retrieve-a-single-entity"></a>Så här: hämta en enda entitet
Du kan skriva en fråga tooretrieve en enda, specifik entitet. hello följande koden anropar **TableOperation.retrieve** med partition nyckel och raden nyckelparametrar toospecify hello kunden ”Jeff Smith”, istället för att skapa en **TableQuery** och använda filter toodo hello samma sak. När den exekveras, hämta hello åtgärden returnerar endast en entitet i stället för en samling. Hej **getResultAsType** metoden kastar hello toohello resultattyp hello tilldelning mål, en **CustomerEntity** objekt. Om den här typen inte är kompatibel med hello typ har angetts för hello fråga genereras ett undantagsfel. Ett null-värde returneras om ingen enhet har en exakt partition och radnyckel matchar. Ange både partitions- och nycklar i en fråga är hello snabbaste sättet tooretrieve en enda entitet från tabelltjänsten hello.

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

## <a name="how-to-modify-an-entity"></a>Så här: ändra en entitet
toomodify en entitet hämtar du den från tabelltjänsten hello, gör ändringar toohello enhetsobjekt och spara hello ändringar tillbaka toohello tabelltjänsten med en ersättning eller merge-åtgärd. hello ändrar följande kod en befintlig kunds telefonnummer. Istället för att ringa **TableOperation.insert** som vi gjorde tooinsert den här koden anropar **TableOperation.replace**. Hej **CloudTable.execute** metodanrop hello tabelltjänsten och hello entiteten ersätts om ett annat program ändrade den hello tidpunkt eftersom hämta av det här programmet. När det sker ett undantag och hello entitet måste hämtas, ändra och spara igen. Optimistisk samtidighet gör mönstret är vanligt i ett distribuerat storage-system.

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

## <a name="how-to-query-a-subset-of-entity-properties"></a>Så här: fråga en deluppsättning Entitetsegenskaper
En tooa frågetabellen kan hämta bara några få egenskaper från en entitet. Den här tekniken, kallad projektion, minskar bandbredden och kan förbättra frågeprestanda, i synnerhet för stora entiteter. hello frågan i hello följande kod använder hello **Välj** metoden tooreturn endast hello e-postadresserna för entiteter i hello tabell. hello resultat är planerad för en samling **sträng** med hello hjälp av en **EntityResolver**, som hello typkonvertering på hello entiteter som returnerades från hello-servern. Du kan lära dig mer om projektion i [Azure-tabeller: Introducing Upsert and Query Projection][Azure Tables: Introducing Upsert and Query Projection]. Observera att projektion inte stöds på hello lokala lagringsemulatorn, så den här koden körs bara när du använder ett konto på hello tabelltjänsten.

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

## <a name="how-to-insert-or-replace-an-entity"></a>Så här: infoga eller ersätta en entitet
Ofta vill du tooadd en entitet tooa tabell utan att känna till om det finns redan i hello tabellen. En insert-eller ersätta åtgärd kan du toomake en enskild begäran som infogar hello entiteten om den inte finns eller ersätta hello befintliga något om det inte. Skapa i föregående exempel hello följande kod infogar eller ersätter hello entiteten för ”Viktor Harp”. När du har skapat en ny entitet den här koden anropar hello **TableOperation.insertOrReplace** metod. Den här koden anropar sedan **köra** på hello **CloudTable** objekt med hello tabell och hello infoga eller ersätta Tabellåtgärd som hello parametrar. tooupdate bara en del av en entitet hello **TableOperation.insertOrMerge** metoden kan användas i stället. Observera att infoga eller ersätta inte stöds på hello lokala lagringsemulatorn, så den här koden körs bara när du använder ett konto på hello tabelltjänsten. Du kan lära dig mer om Infoga eller ersätta och infoga eller dokument i den här [Azure-tabeller: Introducing Upsert and Query Projection][Azure Tables: Introducing Upsert and Query Projection].

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

## <a name="how-to-delete-an-entity"></a>Så här: ta bort en entitet
Du kan enkelt ta bort en enhet när du har hämtat den. När hello entitet hämtas anropa **TableOperation.delete** med hello entiteten toodelete. Sedan anropar **köra** på hello **CloudTable** objekt. hello följande kod hämtar och tar bort en kundentitet.

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

## <a name="how-to-delete-a-table"></a>Så här: ta bort en tabell
Slutligen hello följande kod tar bort en tabell från ett lagringskonto. En tabell som har tagits bort kommer att vara otillgänglig toobe återskapas under en tidsperiod efter hello borttagning, vanligtvis mindre än 40 sekunder.

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

## <a name="next-steps"></a>Nästa steg

* [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en gratis, fristående app från Microsoft som möjliggör toowork visuellt med Azure Storage-data i Windows, macOS och Linux.
* [Azure Storage SDK för Java][Azure Storage SDK for Java]
* [Referens för Azure Storage Client SDK][referens för Azure Storage Client SDK]
* [Azure Storage REST API][Azure Storage REST API]
* [Azure Storage-teamets blogg][Azure Storage Team Blog]

Mer information finns i [Azure för Java-utvecklare](/java/azure).

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[referens för Azure Storage Client SDK]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure Tables: Introducing Upsert and Query Projection]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
