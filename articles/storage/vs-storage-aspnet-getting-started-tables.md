---
title: "aaaGet igång med Azure-tabellagring och Visual Studio anslutna tjänster (ASP.NET) | Microsoft Docs"
description: "Hur tooget igång med Azure-tabellagring i ASP.NET-projekt i Visual Studio när du har anslutit tooa lagringskonto med hjälp av Visual Studio anslutna Services"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: af81a326-18f4-4449-bc0d-e96fba27c1f8
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: tarcher
ms.openlocfilehash: e7ed17098c8742954972dc9e1b50eca77221e327
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-table-storage-and-visual-studio-connected-services-aspnet"></a>Kom igång med Azure-tabellagring och Visual Studio anslutna tjänster (ASP.NET)
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Översikt

Azure Table storage kan du toostore stora mängder strukturerade data. hello-tjänsten är en NoSQL-datalager som tar emot autentiserade anrop inuti och utanför hello Azure-molnet. Azure-tabeller passar utmärkt för att lagra strukturerade, icke-relationella data.

Den här kursen visar hur toowrite ASP.NET kod för några vanliga scenarier med hjälp av Azure table storage entiteter. Dessa scenarier som inkluderar att skapa en tabell och lägga till, fråga och ta bort tabellentiteter. 

##<a name="prerequisites"></a>Krav

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure Storage-konto](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>Skapa en MVC-enhet 

1. I hello **Solution Explorer**, högerklicka på **domänkontrollanter**, hello snabbmenyn, Välj **Lägg till -> Controller**.

    ![Lägg till en domänkontrollant tooan ASP.NET MVC-app](./media/vs-storage-aspnet-getting-started-tables/add-controller-menu.png)

1. På hello **Lägg till Kodskelett** markerar **MVC 5 styrenhet – tom**, och välj **Lägg till**.

    ![Ange typ av MVC-domänkontrollant](./media/vs-storage-aspnet-getting-started-tables/add-controller.png)

1. På hello **Lägg till styrenhet** dialogrutan, namnet hello controller *TablesController*, och välj **Lägg till**.

    ![Namnet hello MVC-enhet](./media/vs-storage-aspnet-getting-started-tables/add-controller-name.png)

1. Lägg till följande hello *med* direktiven toohello `TablesController.cs` fil:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ```

### <a name="create-a-model-class"></a>Skapa en modellklass

Många av hello exemplen i den här artikeln används en **TableEntity**-härledd klass som kallas **CustomerEntity**. hello följande steg när du går igenom deklarera den här klassen som en modellklass:

1. I hello **Solution Explorer**, högerklicka på **modeller**, hello snabbmenyn, Välj **Lägg till -> klassen**.

1. På hello **Lägg till nytt objekt** dialogrutan, namnet hello klassen **CustomerEntity**.

1. Öppna hello `CustomerEntity.cs` filen och Lägg till följande hello **med** direktiv:

    ```csharp
    using Microsoft.WindowsAzure.Storage.Table;
    ```

1. Ändra hello klassen så att när du är klar hello klass har deklarerats som hello följande kod. hello klassen deklarerar en entitetsklass som kallas **CustomerEntity** som använder hello kundens förnamn som hello radnyckel och efternamn som partitionsnyckel hello.

    ```csharp
    public class CustomerEntity : TableEntity
    {
        public CustomerEntity(string lastName, string firstName)
        {
            this.PartitionKey = lastName;
            this.RowKey = firstName;
        }

        public CustomerEntity() { }

        public string Email { get; set; }
    }
    ```

## <a name="create-a-table"></a>Skapa en tabell

hello följande steg visar hur toocreate en tabell:

> [!NOTE]
> 
> Det här avsnittet förutsätter att du har slutfört hello stegen i [ställa in hello utvecklingsmiljö](#set-up-the-development-environment). 

1. Öppna hello `TablesController.cs` fil.

1. Lägg till en metod som kallas **CreateTable** som returnerar en **ActionResult**.

    ```csharp
    public ActionResult CreateTable()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. Inom hello **CreateTable** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Hämta en **CloudTableClient** -objektet representerar en tabelltjänstens klient.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Hämta en **CloudTable** objekt som representerar ett referens toohello önskade tabellnamn. Hej **CloudTableClient.GetTableReference** inte gör en begäran mot tabellagring. hello referens returneras hello tabellen finns eller inte. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Anropa hello **CloudTable.CreateIfNotExists** metoden toocreate hello tabell om det inte finns ännu. Hej **CloudTable.CreateIfNotExists** metoden returnerar **SANT** om hello tabellen finns inte och har skapats. Annars **FALSKT** returneras.    

    ```csharp
    ViewBag.Success = table.CreateIfNotExists();
    ```

1. Uppdatera hello **ViewBag** med hello namnet på hello tabell.

    ```csharp
    ViewBag.TableName = table.Name;
    ```

1. I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **tabeller**, och hello snabbmenyn, Välj **Lägg till -> Visa**.

1. På hello **Lägg till vy** dialogrutan Ange **CreateTable** hello vynamn och välj **Lägg till**.

1. Öppna `CreateTable.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:

    ```csharp
    @{
        ViewBag.Title = "Create Table";
    }
    
    <h2>Create Table results</h2>

    Creation of @ViewBag.TableName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.

1. Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create table", "CreateTable", "Tables")</li>
    ```

1. Kör hello programmet och välj **Skapa tabell** toosee resulterar liknande toohello följande skärmbild:
  
    ![Skapa tabell](./media/vs-storage-aspnet-getting-started-tables/create-table-results.png)

    Som tidigare nämnts hello **CloudTable.CreateIfNotExists** metoden returnerar **SANT** endast när hello tabellen finns inte och har skapats. Om du kör hello app när hello tabellen finns hello-metoden returnerar därför **FALSKT**. toorun hello app flera gånger, måste du ta bort hello tabell innan du kör hello appen igen. Radera hello registret kan göras via hello **CloudTable.Delete** metod. Du kan också ta bort hello tabellen med hjälp av hello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040) eller hello [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md).  

## <a name="add-an-entity-tooa-table"></a>Lägg till en entitet tooa tabell

*Entiteter* mappa tooC\# objekt med hjälp av en anpassad klass som härleds från **TableEntity**. tooadd en entitet tooa tabell, skapa en klass som definierar hello egenskaperna för entiteten. I det här avsnittet visas hur toodefine en entitetsklass som använder hello kundens förnamn som hello radnyckel och efternamn som partitionsnyckel hello. Tillsammans identifiera en entitets partition och radnyckel hello entiteten i hello tabell. Det går snabbare att fråga entiteter med samma partitionsnyckel än entiteter som har olika partitionsnycklar, men skalbarheten och möjligheten att utföra parallella åtgärder är större med olika partitionsnycklar. För alla egenskaper som ska lagras i tabelltjänsten hello måste hello-egenskapen vara en offentlig egenskap för en typ som stöds som exponerar både inställningen och hämtar värden.
Hej enhetsklassen *måste* deklarera en offentlig parameterlös konstruktor.

> [!NOTE]
> 
> Det här avsnittet förutsätter att du har slutfört hello stegen i [ställa in hello utvecklingsmiljö](#set-up-the-development-environment).

1. Öppna hello `TablesController.cs` fil.

1. Lägg till följande direktiv så som hello koden i hello hello `TablesController.cs` filen kan komma åt hello **CustomerEntity** klass:

    ```csharp
    using StorageAspnet.Models;
    ```

1. Lägg till en metod som kallas **AddEntity** som returnerar en **ActionResult**.

    ```csharp
    public ActionResult AddEntity()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. Inom hello **AddEntity** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Hämta en **CloudTableClient** -objektet representerar en tabelltjänstens klient.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Hämta en **CloudTable** objekt som representerar en referens toohello tabell toowhich ska tooadd hello ny entitet. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Skapa en instans av och initiera hello **CustomerEntity** klass.

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    ```

1. Skapa en **TableOperation** objekt som infogar kundentiteten hello.

    ```csharp
    TableOperation insertOperation = TableOperation.Insert(customer1);
    ```

1. Köra hello insert-åtgärden genom att anropa hello **CloudTable.Execute** metod. Du kan verifiera hello resultatet av hello genom att inspektera hello **TableResult.HttpStatusCode** egenskapen. Statuskoden 2xx anger hello åtgärden som begärs av hello-klient har bearbetats. Till exempel lyckad infogningar nya resulterar i en HTTP-statuskod 204, vilket innebär att hello-åtgärden bearbetades och hello servern returnerade inte något innehåll.

    ```csharp
    TableResult result = table.Execute(insertOperation);
    ```

1. Uppdatera hello **ViewBag** med hello tabellnamn och hello resultatet av hello insert-åtgärden.

    ```csharp
    ViewBag.TableName = table.Name;
    ViewBag.Result = result.HttpStatusCode;
    ```

1. I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **tabeller**, och hello snabbmenyn, Välj **Lägg till -> Visa**.

1. På hello **Lägg till vy** dialogrutan Ange **AddEntity** hello vynamn och välj **Lägg till**.

1. Öppna `AddEntity.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:

    ```csharp
    @{
        ViewBag.Title = "Add entity";
    }
    
    <h2>Add entity results</h2>

    Insert of entity into @ViewBag.TableName @(ViewBag.Result == 204 ? "succeeded" : "failed")
    ```
1. I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.

1. Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Add entity", "AddEntity", "Tables")</li>
    ```

1. Kör hello programmet och välj **lägga till enheten** toosee resulterar liknande toohello följande skärmbild:
  
    ![Lägga till entitet](./media/vs-storage-aspnet-getting-started-tables/add-entity-results.png)

    Du kan verifiera att hello entiteten har lagts till genom att följa stegen hello hello under [hämta en enda entitet](#get-a-single-entity). Du kan också använda hello [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooview alla hello entiteter för tabeller.

## <a name="add-a-batch-of-entities-tooa-table"></a>Lägg till en batch med entiteter tooa tabell

I tillägg toobeing kan för[lägga till en entitet tooa tabell en i taget](#add-an-entity-to-a-table), du kan också lägga till entiteter i en batch. Lägger till enheter i batch minskar hello antalet turer mellan kod och hello Azure tabelltjänsten. hello följande steg visar hur tooadd flera entiteter tooa tabell med en enda infogningen:

> [!NOTE]
> 
> Det här avsnittet förutsätter att du har slutfört hello stegen i [ställa in hello utvecklingsmiljö](#set-up-the-development-environment).

1. Öppna hello `TablesController.cs` fil.

1. Lägg till en metod som kallas **AddEntities** som returnerar en **ActionResult**.

    ```csharp
    public ActionResult AddEntities()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. Inom hello **AddEntities** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Hämta en **CloudTableClient** -objektet representerar en tabelltjänstens klient.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Hämta en **CloudTable** objekt som representerar en referens toohello tabellen toowhich du är pågående tooadd hello nya entiteter. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Skapa en instans av vissa kundobjekt baserat på hello **CustomerEntity** modellklass som visas i avsnittet hello, [Lägg till en entitet tooa tabell](#add-an-entity-to-a-table).

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";

    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    ```

1. Hämta en **TableBatchOperation** objekt.

    ```csharp
    TableBatchOperation batchOperation = new TableBatchOperation();
    ```

1. Lägga till entiteter toohello batch insert-åtgärden objekt.

    ```csharp
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);
    ```

1. Köra hello batch insert-åtgärden genom att anropa hello **CloudTable.ExecuteBatch** metod.   

    ```csharp
    IList<TableResult> results = table.ExecuteBatch(batchOperation);
    ```

1. Hej **CloudTable.ExecuteBatch** metoden returnerar en lista över **TableResult** objekt där varje **TableResult** objekt kan vara undersökas toodetermine hello lyckats eller misslyckats för varje enskild transaktion. I det här exemplet skickar hello tooa listvyn och låt hello vy som visar hello resultaten av varje åtgärd. 
 
    ```csharp
    return View(results);
    ```

1. I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **tabeller**, och hello snabbmenyn, Välj **Lägg till -> Visa**.

1. På hello **Lägg till vy** dialogrutan Ange **AddEntities** hello vynamn och välj **Lägg till**.

1. Öppna `AddEntities.cshtml`, och ändra den så att det ser ut som följande hello.

    ```csharp
    @model IEnumerable<Microsoft.WindowsAzure.Storage.Table.TableResult>
    @{
        ViewBag.Title = "AddEntities";
    }
    
    <h2>Add-entities results</h2>
    
    <table border="1">
        <tr>
            <th>First name</th>
            <th>Last name</th>
            <th>HTTP result</th>
        </tr>
        @foreach (var result in Model)
        {
        <tr>
            <td>@((result.Result as StorageAspnet.Models.CustomerEntity).RowKey)</td>
            <td>@((result.Result as StorageAspnet.Models.CustomerEntity).PartitionKey)</td>
            <td>@result.HttpStatusCode</td>
        </tr>
        }
    </table>
    ```

1. I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.

1. Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Add entities", "AddEntities", "Tables")</li>
    ```

1. Kör hello programmet och välj **lägga till enheter** toosee resulterar liknande toohello följande skärmbild:
  
    ![Lägg till entiteter](./media/vs-storage-aspnet-getting-started-tables/add-entities-results.png)

    Du kan verifiera att hello entiteten har lagts till genom att följa stegen hello hello under [hämta en enda entitet](#get-a-single-entity). Du kan också använda hello [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooview alla hello entiteter för tabeller.

## <a name="get-a-single-entity"></a>Hämta en enda entitet

Detta avsnitt visar hur tooget en enda entitet från en tabell med hjälp av hello entitets radnyckel och partitionsnyckel. 

> [!NOTE]
> 
> Det här avsnittet förutsätter att du har slutfört hello stegen i [ställa in hello utvecklingsmiljö](#set-up-the-development-environment), och använder data från [lägga till en batch med entiteter tooa tabellen](#add-a-batch-of-entities-to-a-table). 

1. Öppna hello `TablesController.cs` fil.

1. Lägg till en metod som kallas **GetSingle** som returnerar en **ActionResult**.

    ```csharp
    public ActionResult GetSingle()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. Inom hello **GetSingle** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Hämta en **CloudTableClient** -objektet representerar en tabelltjänstens klient.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Hämta en **CloudTable** objekt som representerar en toohello tabell från vilken du hämtar hello entitet. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Skapa ett objekt för hämta åtgärd som tar ett entitetsobjekt som härletts från **TableEntity**. hello första parametern är hello *partitionKey*, och andra hello-parametern är hello *rowKey*. Med hjälp av hello **CustomerEntity** klass och data som visas i avsnittet hello [lägga till en batch med entiteter tooa tabell](#add-a-batch-of-entities-to-a-table), hello följande kodfragment frågor hello tabellen för en **CustomerEntity** entitet med en *partitionKey* värdet för ”Smith” och en *rowKey* värdet för ”Ben”:

    ```csharp
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");
    ```

1. Köra hello hämtningen.   

    ```csharp
    TableResult result = table.Execute(retrieveOperation);
    ```

1. Skicka hello resultatet toohello vy för visning.

    ```csharp
    return View(result);
    ```

1. I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **tabeller**, och hello snabbmenyn, Välj **Lägg till -> Visa**.

1. På hello **Lägg till vy** dialogrutan Ange **GetSingle** hello vynamn och välj **Lägg till**.

1. Öppna `GetSingle.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:

    ```csharp
    @model Microsoft.WindowsAzure.Storage.Table.TableResult
    @{
        ViewBag.Title = "GetSingle";
    }
    
    <h2>Get Single results</h2>
    
    <table border="1">
        <tr>
            <th>HTTP result</th>
            <th>First name</th>
            <th>Last name</th>
            <th>Email</th>
        </tr>
        <tr>
            <td>@Model.HttpStatusCode</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).RowKey)</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).PartitionKey)</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).Email)</td>
        </tr>
    </table>
    ```

1. I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.

1. Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Get single", "GetSingle", "Tables")</li>
    ```

1. Kör hello programmet och välj **hämta enda** toosee resulterar liknande toohello följande skärmbild:
  
    ![Hämta enda](./media/vs-storage-aspnet-getting-started-tables/get-single-results.png)

## <a name="get-all-entities-in-a-partition"></a>Hämta alla entiteter i en partition

Som tidigare nämnts hello under [lägga till en entitet tooa tabell](#add-an-entity-to-a-table), identifiera hello kombination av en partition och en rad för en entitet i en tabell. Entiteter med samma partitionsnyckel kan frågas snabbare än entiteter med olika partitionsnycklar. Detta avsnitt visar hur tooquery en tabell för alla hello entiteter från en angiven partition.  

> [!NOTE]
> 
> Det här avsnittet förutsätter att du har slutfört hello stegen i [ställa in hello utvecklingsmiljö](#set-up-the-development-environment), och använder data från [lägga till en batch med entiteter tooa tabellen](#add-a-batch-of-entities-to-a-table). 

1. Öppna hello `TablesController.cs` fil.

1. Lägg till en metod som kallas **GetPartition** som returnerar en **ActionResult**.

    ```csharp
    public ActionResult GetPartition()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. Inom hello **GetPartition** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Hämta en **CloudTableClient** -objektet representerar en tabelltjänstens klient.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Hämta en **CloudTable** objekt som representerar en toohello tabell från vilken du hämtar hello entiteter. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Skapa en instans av en **TableQuery** objekt som anger hello frågan i hello **där** satsen. Med hjälp av hello **CustomerEntity** klass och data som visas i avsnittet hello [lägga till en batch med entiteter tooa tabell](#add-a-batch-of-entities-to-a-table), hello följande kod fragment frågor hello tabellen för alla enheter där hello  **PartitionKey** (kundens efternamn) har värdet ”Smith”:

    ```csharp
    TableQuery<CustomerEntity> query = 
        new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));
    ```

1. I en slinga anropa hello **CloudTable.ExecuteQuerySegmented** metoden skicka hello frågeobjekt du instansierad i hello föregående steg.  Hej **CloudTable.ExecuteQuerySegmented** metoden returnerar en **TableContinuationToken** objekt som - när **null** -anger att det inte finns några fler entiteter tooretrieve. Använda en annan loop tooiterate över hello returnerade enheter inom hello loop. I följande kodexempel hello, läggs varje returnerade entitet tooa lista. En gång Hej loopen avslutas, hello listan skickas tooa vy för visning: 

    ```csharp
    List<CustomerEntity> customers = new List<CustomerEntity>();
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity> resultSegment = table.ExecuteQuerySegmented(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity customer in resultSegment.Results)
        {
            customers.Add(customer);
        }
    } while (token != null);

    return View(customers);
    ```

1. I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **tabeller**, och hello snabbmenyn, Välj **Lägg till -> Visa**.

1. På hello **Lägg till vy** dialogrutan Ange **GetPartition** hello vynamn och välj **Lägg till**.

1. Öppna `GetPartition.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:

    ```csharp
    @model IEnumerable<StorageAspnet.Models.CustomerEntity>
    @{
        ViewBag.Title = "GetPartition";
    }
    
    <h2>Get Partition results</h2>
    
    <table border="1">
        <tr>
            <th>First name</th>
            <th>Last name</th>
            <th>Email</th>
        </tr>
        @foreach (var customer in Model)
        {
        <tr>
            <td>@(customer.RowKey)</td>
            <td>@(customer.PartitionKey)</td>
            <td>@(customer.Email)</td>
        </tr>
        }
    </table>
    ```

1. I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.

1. Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Get partition", "GetPartition", "Tables")</li>
    ```

1. Kör hello programmet och välj **hämta partitionen** toosee resulterar liknande toohello följande skärmbild:
  
    ![Hämta Partition](./media/vs-storage-aspnet-getting-started-tables/get-partition-results.png)

## <a name="delete-an-entity"></a>Ta bort en entitet

Detta avsnitt visar hur toodelete en entitet från en tabell.

> [!NOTE]
> 
> Det här avsnittet förutsätter att du har slutfört hello stegen i [ställa in hello utvecklingsmiljö](#set-up-the-development-environment), och använder data från [lägga till en batch med entiteter tooa tabellen](#add-a-batch-of-entities-to-a-table). 

1. Öppna hello `TablesController.cs` fil.

1. Lägg till en metod som kallas **DeleteEntity** som returnerar en **ActionResult**.

    ```csharp
    public ActionResult DeleteEntity()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. Inom hello **DeleteEntity** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Hämta en **CloudTableClient** -objektet representerar en tabelltjänstens klient.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Hämta en **CloudTable** objekt som representerar en toohello tabell från vilken du vill ta bort hello entitet. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Skapa åtgärden ta bort objekt som tar ett entitetsobjekt som härletts från **TableEntity**. I detta fall kan vi använda hello **CustomerEntity** klass och data som visas i avsnittet hello [lägga till en batch med entiteter tooa tabellen](#add-a-batch-of-entities-to-a-table). Hej entitetens **ETag** tooa giltigt värde måste anges.  

    ```csharp
    TableOperation deleteOperation = 
        TableOperation.Delete(new CustomerEntity("Smith", "Ben") { ETag = "*" } );
    ```

1. Köra hello borttagningsåtgärd.   

    ```csharp
    TableResult result = table.Execute(deleteOperation);
    ```

1. Skicka hello resultatet toohello vy för visning.

    ```csharp
    return View(result);
    ```

1. I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **tabeller**, och hello snabbmenyn, Välj **Lägg till -> Visa**.

1. På hello **Lägg till vy** dialogrutan Ange **DeleteEntity** hello vynamn och välj **Lägg till**.

1. Öppna `DeleteEntity.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:

    ```csharp
    @model Microsoft.WindowsAzure.Storage.Table.TableResult
    @{
        ViewBag.Title = "DeleteEntity";
    }
    
    <h2>Delete Entity results</h2>
    
    <table border="1">
        <tr>
            <th>First name</th>
            <th>Last name</th>
            <th>HTTP result</th>
        </tr>
        <tr>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).RowKey)</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).PartitionKey)</td>
            <td>@Model.HttpStatusCode</td>
        </tr>
    </table>

    ```

1. I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.

1. Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete entity", "DeleteEntity", "Tables")</li>
    ```

1. Kör hello programmet och välj **ta bort entiteten** toosee resulterar liknande toohello följande skärmbild:
  
    ![Hämta enda](./media/vs-storage-aspnet-getting-started-tables/delete-entity-results.png)

## <a name="next-steps"></a>Nästa steg
Visa mer funktionen guider toolearn om ytterligare alternativ för att lagra data i Azure.

  * [Kom igång med Azure blob storage och Visual Studio anslutna tjänster (ASP.NET)](./vs-storage-aspnet-getting-started-blobs.md)
  * [Kom igång med Azure queue storage- och Visual Studio anslutna tjänster (ASP.NET)](./vs-storage-aspnet-getting-started-queues.md)
