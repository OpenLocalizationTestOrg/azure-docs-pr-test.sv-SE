---
title: "Kom igång med Azure-tabellagring och Visual Studio anslutna tjänster (ASP.NET) | Microsoft Docs"
description: "Hur du kommer igång med Azure-tabellagring i ASP.NET-projekt i Visual Studio efter anslutning till ett lagringskonto med hjälp av Visual Studio anslutna Services"
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
ms.openlocfilehash: d9cb32483d3f582bbeb0ccc6a204a8b6d9ea5c96
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-azure-table-storage-and-visual-studio-connected-services-aspnet"></a><span data-ttu-id="81dc9-103">Kom igång med Azure-tabellagring och Visual Studio anslutna tjänster (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="81dc9-103">Get started with Azure table storage and Visual Studio Connected Services (ASP.NET)</span></span>
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a><span data-ttu-id="81dc9-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="81dc9-104">Overview</span></span>

<span data-ttu-id="81dc9-105">Azure Table storage kan du lagra stora mängder strukturerade data.</span><span class="sxs-lookup"><span data-stu-id="81dc9-105">Azure Table storage enables you to store large amounts of structured data.</span></span> <span data-ttu-id="81dc9-106">Tjänsten är en NoSQL-datalager som tar emot autentiserade anrop inuti och utanför Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="81dc9-106">The service is a NoSQL datastore that accepts authenticated calls from inside and outside the Azure cloud.</span></span> <span data-ttu-id="81dc9-107">Azure-tabeller passar utmärkt för att lagra strukturerade, icke-relationella data.</span><span class="sxs-lookup"><span data-stu-id="81dc9-107">Azure tables are ideal for storing structured, non-relational data.</span></span>

<span data-ttu-id="81dc9-108">Den här kursen visar hur du skriver ASP.NET-kod för några vanliga scenarier med hjälp av Azure table storage entiteter.</span><span class="sxs-lookup"><span data-stu-id="81dc9-108">This tutorial shows how to write ASP.NET code for some common scenarios using Azure table storage entities.</span></span> <span data-ttu-id="81dc9-109">Dessa scenarier som inkluderar att skapa en tabell och lägga till, fråga och ta bort tabellentiteter.</span><span class="sxs-lookup"><span data-stu-id="81dc9-109">These scenarios include creating a table, and adding, querying, and deleting table entities.</span></span> 

##<a name="prerequisites"></a><span data-ttu-id="81dc9-110">Krav</span><span class="sxs-lookup"><span data-stu-id="81dc9-110">Prerequisites</span></span>

* [<span data-ttu-id="81dc9-111">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="81dc9-111">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="81dc9-112">Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="81dc9-112">Azure storage account</span></span>](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a><span data-ttu-id="81dc9-113">Skapa en MVC-enhet</span><span class="sxs-lookup"><span data-stu-id="81dc9-113">Create an MVC controller</span></span> 

1. <span data-ttu-id="81dc9-114">I den **Solution Explorer**, högerklicka på **domänkontrollanter**, och på snabbmenyn Välj **Lägg till -> styrenhet**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-114">In the **Solution Explorer**, right-click **Controllers**, and, from the context menu, select **Add->Controller**.</span></span>

    ![Lägg till en domänkontrollant i en ASP.NET MVC-app](./media/vs-storage-aspnet-getting-started-tables/add-controller-menu.png)

1. <span data-ttu-id="81dc9-116">På den **Lägg till Kodskelett** markerar **MVC 5 styrenhet – tom**, och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-116">On the **Add Scaffold** dialog, select **MVC 5 Controller - Empty**, and select **Add**.</span></span>

    ![Ange typ av MVC-domänkontrollant](./media/vs-storage-aspnet-getting-started-tables/add-controller.png)

1. <span data-ttu-id="81dc9-118">På den **Lägg till styrenhet** dialogrutan namn styrenheten *TablesController*, och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-118">On the **Add Controller** dialog, name the controller *TablesController*, and select **Add**.</span></span>

    ![Namnet på MVC-enhet](./media/vs-storage-aspnet-getting-started-tables/add-controller-name.png)

1. <span data-ttu-id="81dc9-120">Lägg till följande *med* direktiven till den `TablesController.cs` filen:</span><span class="sxs-lookup"><span data-stu-id="81dc9-120">Add the following *using* directives to the `TablesController.cs` file:</span></span>

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ```

### <a name="create-a-model-class"></a><span data-ttu-id="81dc9-121">Skapa en modellklass</span><span class="sxs-lookup"><span data-stu-id="81dc9-121">Create a model class</span></span>

<span data-ttu-id="81dc9-122">Många av exemplen i den här artikeln används en **TableEntity**-härledd klass som kallas **CustomerEntity**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-122">Many of the examples in this article use a **TableEntity**-derived class called **CustomerEntity**.</span></span> <span data-ttu-id="81dc9-123">Följande steg när du går igenom deklarera den här klassen som en modellklass:</span><span class="sxs-lookup"><span data-stu-id="81dc9-123">The following steps guide you through declaring this class as a model class:</span></span>

1. <span data-ttu-id="81dc9-124">I den **Solution Explorer**, högerklicka på **modeller**, och på snabbmenyn Välj **klassen -> Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-124">In the **Solution Explorer**, right-click **Models**, and, from the context menu, select **Add->Class**.</span></span>

1. <span data-ttu-id="81dc9-125">På den **Lägg till nytt objekt** dialogrutan namn klassen **CustomerEntity**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-125">On the **Add New Item** dialog, name the class, **CustomerEntity**.</span></span>

1. <span data-ttu-id="81dc9-126">Öppna den `CustomerEntity.cs` filen och Lägg till följande **med** direktiv:</span><span class="sxs-lookup"><span data-stu-id="81dc9-126">Open the `CustomerEntity.cs` file, and add the following **using** directive:</span></span>

    ```csharp
    using Microsoft.WindowsAzure.Storage.Table;
    ```

1. <span data-ttu-id="81dc9-127">Ändra klassen så att när du är klar klassen har deklarerats som i följande kod.</span><span class="sxs-lookup"><span data-stu-id="81dc9-127">Modify the class so that, when finished, the class is declared as in the following code.</span></span> <span data-ttu-id="81dc9-128">Klassen deklarerar en entitetsklass som kallas **CustomerEntity** som använder kundens förnamn som radnyckel och efternamn som partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="81dc9-128">The class declares an entity class called **CustomerEntity** that uses the customer's first name as the row key and last name as the partition key.</span></span>

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

## <a name="create-a-table"></a><span data-ttu-id="81dc9-129">Skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="81dc9-129">Create a table</span></span>

<span data-ttu-id="81dc9-130">Följande steg visar hur du skapar en tabell:</span><span class="sxs-lookup"><span data-stu-id="81dc9-130">The following steps illustrate how to create a table:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="81dc9-131">Det här avsnittet förutsätter att du har slutfört stegen i [Konfigurera utvecklingsmiljön](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="81dc9-131">This section assumes you have completed the steps in [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="81dc9-132">Öppna filen `TablesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="81dc9-132">Open the `TablesController.cs` file.</span></span>

1. <span data-ttu-id="81dc9-133">Lägg till en metod som kallas **CreateTable** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-133">Add a method called **CreateTable** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult CreateTable()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="81dc9-134">I den **CreateTable** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="81dc9-134">Within the **CreateTable** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="81dc9-135">Använda följande kod för att hämta anslutningssträngen för lagring och information om lagringskonto från Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* till namnet på Azure storage-konto du försöker komma åt.)</span><span class="sxs-lookup"><span data-stu-id="81dc9-135">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="81dc9-136">Hämta en **CloudTableClient** -objektet representerar en tabelltjänstens klient.</span><span class="sxs-lookup"><span data-stu-id="81dc9-136">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="81dc9-137">Hämta en **CloudTable** objekt som representerar en referens till önskad tabellens namn.</span><span class="sxs-lookup"><span data-stu-id="81dc9-137">Get a **CloudTable** object that represents a reference to the desired table name.</span></span> <span data-ttu-id="81dc9-138">Den **CloudTableClient.GetTableReference** inte gör en begäran mot tabellagring.</span><span class="sxs-lookup"><span data-stu-id="81dc9-138">The **CloudTableClient.GetTableReference** method does not make a request against table storage.</span></span> <span data-ttu-id="81dc9-139">Referensen returneras eller inte finns i tabellen.</span><span class="sxs-lookup"><span data-stu-id="81dc9-139">The reference is returned whether or not the table exists.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="81dc9-140">Anropa den **CloudTable.CreateIfNotExists** metod för att skapa tabellen om det inte finns ännu.</span><span class="sxs-lookup"><span data-stu-id="81dc9-140">Call the **CloudTable.CreateIfNotExists** method to create the table if it does not yet exist.</span></span> <span data-ttu-id="81dc9-141">Den **CloudTable.CreateIfNotExists** metoden returnerar **SANT** om tabellen inte finns och har skapats.</span><span class="sxs-lookup"><span data-stu-id="81dc9-141">The **CloudTable.CreateIfNotExists** method returns **true** if the table does not exist, and is successfully created.</span></span> <span data-ttu-id="81dc9-142">Annars **FALSKT** returneras.</span><span class="sxs-lookup"><span data-stu-id="81dc9-142">Otherwise, **false** is returned.</span></span>    

    ```csharp
    ViewBag.Success = table.CreateIfNotExists();
    ```

1. <span data-ttu-id="81dc9-143">Uppdatering av **ViewBag** med namnet på tabellen.</span><span class="sxs-lookup"><span data-stu-id="81dc9-143">Update the **ViewBag** with the name of the table.</span></span>

    ```csharp
    ViewBag.TableName = table.Name;
    ```

1. <span data-ttu-id="81dc9-144">I den **Solution Explorer**, expandera den **vyer** mappen, högerklicka på **tabeller**, och på snabbmenyn väljer **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-144">In the **Solution Explorer**, expand the **Views** folder, right-click **Tables**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="81dc9-145">På den **Lägg till vy** dialogrutan Ange **CreateTable** för namn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-145">On the **Add View** dialog, enter **CreateTable** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="81dc9-146">Öppna `CreateTable.cshtml`, och ändra den så att det ser ut som följande kodavsnitt:</span><span class="sxs-lookup"><span data-stu-id="81dc9-146">Open `CreateTable.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Create Table";
    }
    
    <h2>Create Table results</h2>

    Creation of @ViewBag.TableName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. <span data-ttu-id="81dc9-147">I den **Solution Explorer**, expandera den **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="81dc9-147">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="81dc9-148">Efter senast **Html.ActionLink**, Lägg till följande **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="81dc9-148">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Create table", "CreateTable", "Tables")</li>
    ```

1. <span data-ttu-id="81dc9-149">Kör programmet och välj **Skapa tabell** att se resultatet liknar följande Skärmdump:</span><span class="sxs-lookup"><span data-stu-id="81dc9-149">Run the application, and select **Create table** to see results similar to the following screen shot:</span></span>
  
    ![Skapa tabell](./media/vs-storage-aspnet-getting-started-tables/create-table-results.png)

    <span data-ttu-id="81dc9-151">Som nämnts tidigare i **CloudTable.CreateIfNotExists** metoden returnerar **SANT** endast när tabellen finns inte och har skapats.</span><span class="sxs-lookup"><span data-stu-id="81dc9-151">As mentioned previously, the **CloudTable.CreateIfNotExists** method returns **true** only when the table doesn't exist and is created.</span></span> <span data-ttu-id="81dc9-152">Om du kör appen när tabellen finns metoden returnerar därför **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-152">Therefore, if you run the app when the table exists, the method returns **false**.</span></span> <span data-ttu-id="81dc9-153">Om du vill köra appen flera gånger, måste du ta bort tabellen innan du kör appen igen.</span><span class="sxs-lookup"><span data-stu-id="81dc9-153">To run the app multiple times, you must delete the table before running the app again.</span></span> <span data-ttu-id="81dc9-154">Ta bort tabellen kan göras den **CloudTable.Delete** metod.</span><span class="sxs-lookup"><span data-stu-id="81dc9-154">Deleting the table can be done via the **CloudTable.Delete** method.</span></span> <span data-ttu-id="81dc9-155">Du kan också ta bort en tabell med hjälp av den [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040) eller [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="81dc9-155">You can also delete the table using the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) or the [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>  

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="81dc9-156">Lägga till en entitet i en tabell</span><span class="sxs-lookup"><span data-stu-id="81dc9-156">Add an entity to a table</span></span>

<span data-ttu-id="81dc9-157">*Entiteter* mappas till C\# objekt med hjälp av en anpassad klass som härleds från **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-157">*Entities* map to C\# objects by using a custom class derived from **TableEntity**.</span></span> <span data-ttu-id="81dc9-158">Om du vill lägga till en entitet i en tabell skapar du en klass som definierar egenskaperna för entiteten.</span><span class="sxs-lookup"><span data-stu-id="81dc9-158">To add an entity to a table, create a class that defines the properties of your entity.</span></span> <span data-ttu-id="81dc9-159">I det här avsnittet visas hur du definierar en entitetsklass som använder kundens förnamn som radnyckel och efternamn som partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="81dc9-159">In this section, you'll see how to define an entity class that uses the customer's first name as the row key and last name as the partition key.</span></span> <span data-ttu-id="81dc9-160">Tillsammans identifierar en entitets partition och radnyckel entiteten i tabellen unikt.</span><span class="sxs-lookup"><span data-stu-id="81dc9-160">Together, an entity's partition and row key uniquely identify the entity in the table.</span></span> <span data-ttu-id="81dc9-161">Det går snabbare att fråga entiteter med samma partitionsnyckel än entiteter som har olika partitionsnycklar, men skalbarheten och möjligheten att utföra parallella åtgärder är större med olika partitionsnycklar.</span><span class="sxs-lookup"><span data-stu-id="81dc9-161">Entities with the same partition key can be queried faster than entities with different partition keys, but using diverse partition keys allows for greater scalability of parallel operations.</span></span> <span data-ttu-id="81dc9-162">Egenskapen måste vara en offentlig egenskap för en typ som stöds som exponerar både inställningen och hämtar värden för alla egenskaper som ska lagras i tabelltjänsten.</span><span class="sxs-lookup"><span data-stu-id="81dc9-162">For any property that should be stored in the table service, the property must be a public property of a supported type that exposes both setting and retrieving values.</span></span>
<span data-ttu-id="81dc9-163">Enhetsklassen *måste* deklarera en offentlig parameterlös konstruktor.</span><span class="sxs-lookup"><span data-stu-id="81dc9-163">The entity class *must* declare a public parameter-less constructor.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="81dc9-164">Det här avsnittet förutsätter att du har slutfört stegen i [Konfigurera utvecklingsmiljön](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="81dc9-164">This section assumes you have completed the steps in [Set up the development environment](#set-up-the-development-environment).</span></span>

1. <span data-ttu-id="81dc9-165">Öppna filen `TablesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="81dc9-165">Open the `TablesController.cs` file.</span></span>

1. <span data-ttu-id="81dc9-166">Lägg till följande direktiv så att koden i den `TablesController.cs` filen kan komma åt den **CustomerEntity** klass:</span><span class="sxs-lookup"><span data-stu-id="81dc9-166">Add the following directive so that the code in the `TablesController.cs` file can access the **CustomerEntity** class:</span></span>

    ```csharp
    using StorageAspnet.Models;
    ```

1. <span data-ttu-id="81dc9-167">Lägg till en metod som kallas **AddEntity** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-167">Add a method called **AddEntity** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult AddEntity()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="81dc9-168">I den **AddEntity** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="81dc9-168">Within the **AddEntity** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="81dc9-169">Använda följande kod för att hämta anslutningssträngen för lagring och information om lagringskonto från Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* till namnet på Azure storage-konto du försöker komma åt.)</span><span class="sxs-lookup"><span data-stu-id="81dc9-169">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="81dc9-170">Hämta en **CloudTableClient** -objektet representerar en tabelltjänstens klient.</span><span class="sxs-lookup"><span data-stu-id="81dc9-170">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="81dc9-171">Hämta en **CloudTable** objekt som representerar en referens till tabellen som du ska lägga till den nya entiteten.</span><span class="sxs-lookup"><span data-stu-id="81dc9-171">Get a **CloudTable** object that represents a reference to the table to which you are going to add the new entity.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="81dc9-172">Skapa en instans av och initiera den **CustomerEntity** klass.</span><span class="sxs-lookup"><span data-stu-id="81dc9-172">Instantiate and initialize the **CustomerEntity** class.</span></span>

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    ```

1. <span data-ttu-id="81dc9-173">Skapa en **TableOperation** objekt som infogar kundentiteten.</span><span class="sxs-lookup"><span data-stu-id="81dc9-173">Create a **TableOperation** object that inserts the customer entity.</span></span>

    ```csharp
    TableOperation insertOperation = TableOperation.Insert(customer1);
    ```

1. <span data-ttu-id="81dc9-174">Köra insert-åtgärden genom att anropa den **CloudTable.Execute** metod.</span><span class="sxs-lookup"><span data-stu-id="81dc9-174">Execute the insert operation by calling the **CloudTable.Execute** method.</span></span> <span data-ttu-id="81dc9-175">Du kan kontrollera resultatet av åtgärden genom att kontrollera den **TableResult.HttpStatusCode** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="81dc9-175">You can verify the result of the operation by inspecting the **TableResult.HttpStatusCode** property.</span></span> <span data-ttu-id="81dc9-176">Statuskoden 2xx anger den åtgärd som begärs av klienten har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="81dc9-176">A status code of 2xx indicates the action requested by the client was processed successfully.</span></span> <span data-ttu-id="81dc9-177">Lyckad infogningar av nya enheter resulterar i en HTTP-statuskod 204, vilket innebär att åtgärden bearbetades och servern returnerade exempelvis inte allt innehåll.</span><span class="sxs-lookup"><span data-stu-id="81dc9-177">For example, successful insertions of new entities results in an HTTP status code of 204, meaning that the operation was successfully processed and the server did not return any content.</span></span>

    ```csharp
    TableResult result = table.Execute(insertOperation);
    ```

1. <span data-ttu-id="81dc9-178">Uppdatering av **ViewBag** med tabellnamnet och resultatet av insert-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="81dc9-178">Update the **ViewBag** with the table name, and the results of the insert operation.</span></span>

    ```csharp
    ViewBag.TableName = table.Name;
    ViewBag.Result = result.HttpStatusCode;
    ```

1. <span data-ttu-id="81dc9-179">I den **Solution Explorer**, expandera den **vyer** mappen, högerklicka på **tabeller**, och på snabbmenyn väljer **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-179">In the **Solution Explorer**, expand the **Views** folder, right-click **Tables**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="81dc9-180">På den **Lägg till vy** dialogrutan Ange **AddEntity** för namn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-180">On the **Add View** dialog, enter **AddEntity** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="81dc9-181">Öppna `AddEntity.cshtml`, och ändra den så att det ser ut som följande kodavsnitt:</span><span class="sxs-lookup"><span data-stu-id="81dc9-181">Open `AddEntity.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Add entity";
    }
    
    <h2>Add entity results</h2>

    Insert of entity into @ViewBag.TableName @(ViewBag.Result == 204 ? "succeeded" : "failed")
    ```
1. <span data-ttu-id="81dc9-182">I den **Solution Explorer**, expandera den **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="81dc9-182">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="81dc9-183">Efter senast **Html.ActionLink**, Lägg till följande **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="81dc9-183">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Add entity", "AddEntity", "Tables")</li>
    ```

1. <span data-ttu-id="81dc9-184">Kör programmet och välj **lägga till enheten** att se resultatet liknar följande Skärmdump:</span><span class="sxs-lookup"><span data-stu-id="81dc9-184">Run the application, and select **Add entity** to see results similar to the following screen shot:</span></span>
  
    ![Lägga till entitet](./media/vs-storage-aspnet-getting-started-tables/add-entity-results.png)

    <span data-ttu-id="81dc9-186">Du kan kontrollera att entiteten har lagts till genom att följa stegen i avsnittet [hämta en enda entitet](#get-a-single-entity).</span><span class="sxs-lookup"><span data-stu-id="81dc9-186">You can verify that the entity was added by following the steps in the section, [Get a single entity](#get-a-single-entity).</span></span> <span data-ttu-id="81dc9-187">Du kan också använda den [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) att visa alla entiteter för tabeller.</span><span class="sxs-lookup"><span data-stu-id="81dc9-187">You can also use the [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) to view all the entities for your tables.</span></span>

## <a name="add-a-batch-of-entities-to-a-table"></a><span data-ttu-id="81dc9-188">Lägg till en batch med entiteter i en tabell</span><span class="sxs-lookup"><span data-stu-id="81dc9-188">Add a batch of entities to a table</span></span>

<span data-ttu-id="81dc9-189">Förutom att kunna [lägga till en entitet i en tabell en i taget](#add-an-entity-to-a-table), du kan också lägga till entiteter i en batch.</span><span class="sxs-lookup"><span data-stu-id="81dc9-189">In addition to being able to [add an entity to a table one at a time](#add-an-entity-to-a-table), you can also add entities in batch.</span></span> <span data-ttu-id="81dc9-190">Lägger till enheter i batch minskar antalet turer mellan din kod och tjänsten Azure-tabellen.</span><span class="sxs-lookup"><span data-stu-id="81dc9-190">Adding entities in batch reduces the number of round-trips between your code and the Azure table service.</span></span> <span data-ttu-id="81dc9-191">Följande steg visar hur du lägger till flera enheter till en tabell med en enda infoga igen:</span><span class="sxs-lookup"><span data-stu-id="81dc9-191">The following steps illustrate how to add multiple entities to a table with a single insert operation:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="81dc9-192">Det här avsnittet förutsätter att du har slutfört stegen i [Konfigurera utvecklingsmiljön](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="81dc9-192">This section assumes you have completed the steps in [Set up the development environment](#set-up-the-development-environment).</span></span>

1. <span data-ttu-id="81dc9-193">Öppna filen `TablesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="81dc9-193">Open the `TablesController.cs` file.</span></span>

1. <span data-ttu-id="81dc9-194">Lägg till en metod som kallas **AddEntities** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-194">Add a method called **AddEntities** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult AddEntities()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="81dc9-195">I den **AddEntities** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="81dc9-195">Within the **AddEntities** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="81dc9-196">Använda följande kod för att hämta anslutningssträngen för lagring och information om lagringskonto från Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* till namnet på Azure storage-konto du försöker komma åt.)</span><span class="sxs-lookup"><span data-stu-id="81dc9-196">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="81dc9-197">Hämta en **CloudTableClient** -objektet representerar en tabelltjänstens klient.</span><span class="sxs-lookup"><span data-stu-id="81dc9-197">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="81dc9-198">Hämta en **CloudTable** objekt som representerar en referens till tabellen som du ska lägga till nya enheter.</span><span class="sxs-lookup"><span data-stu-id="81dc9-198">Get a **CloudTable** object that represents a reference to the table to which you are going to add the new entities.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="81dc9-199">Skapa en instans av vissa kundobjekt baserat på den **CustomerEntity** modellklass som visas i avsnittet [lägga till en entitet i en tabell](#add-an-entity-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="81dc9-199">Instantiate some customer objects based on the **CustomerEntity** model class presented in the section, [Add an entity to a table](#add-an-entity-to-a-table).</span></span>

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";

    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    ```

1. <span data-ttu-id="81dc9-200">Hämta en **TableBatchOperation** objekt.</span><span class="sxs-lookup"><span data-stu-id="81dc9-200">Get a **TableBatchOperation** object.</span></span>

    ```csharp
    TableBatchOperation batchOperation = new TableBatchOperation();
    ```

1. <span data-ttu-id="81dc9-201">Lägga till enheter i objektet batch insert-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="81dc9-201">Add entities to the batch insert operation object.</span></span>

    ```csharp
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);
    ```

1. <span data-ttu-id="81dc9-202">Köra batch insert-åtgärden genom att anropa den **CloudTable.ExecuteBatch** metod.</span><span class="sxs-lookup"><span data-stu-id="81dc9-202">Execute the batch insert operation by calling the **CloudTable.ExecuteBatch** method.</span></span>   

    ```csharp
    IList<TableResult> results = table.ExecuteBatch(batchOperation);
    ```

1. <span data-ttu-id="81dc9-203">Den **CloudTable.ExecuteBatch** metoden returnerar en lista över **TableResult** objekt där varje **TableResult** objekt kan undersökas för att fastställa lyckad eller misslyckad för varje enskild transaktion.</span><span class="sxs-lookup"><span data-stu-id="81dc9-203">The **CloudTable.ExecuteBatch** method returns a list of **TableResult** objects where each **TableResult** object can be examined to determine the success or failure of each individual operation.</span></span> <span data-ttu-id="81dc9-204">I det här exemplet skickar listan till en vy och låt den vy som visar resultatet av varje åtgärd.</span><span class="sxs-lookup"><span data-stu-id="81dc9-204">For this example, pass the list to a view and let the view display the results of each operation.</span></span> 
 
    ```csharp
    return View(results);
    ```

1. <span data-ttu-id="81dc9-205">I den **Solution Explorer**, expandera den **vyer** mappen, högerklicka på **tabeller**, och på snabbmenyn väljer **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-205">In the **Solution Explorer**, expand the **Views** folder, right-click **Tables**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="81dc9-206">På den **Lägg till vy** dialogrutan Ange **AddEntities** för namn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-206">On the **Add View** dialog, enter **AddEntities** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="81dc9-207">Öppna `AddEntities.cshtml`, och ändra den så att det ser ut ungefär så här.</span><span class="sxs-lookup"><span data-stu-id="81dc9-207">Open `AddEntities.cshtml`, and modify it so that it looks like the following.</span></span>

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

1. <span data-ttu-id="81dc9-208">I den **Solution Explorer**, expandera den **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="81dc9-208">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="81dc9-209">Efter senast **Html.ActionLink**, Lägg till följande **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="81dc9-209">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Add entities", "AddEntities", "Tables")</li>
    ```

1. <span data-ttu-id="81dc9-210">Kör programmet och välj **lägga till enheter** att se resultatet liknar följande Skärmdump:</span><span class="sxs-lookup"><span data-stu-id="81dc9-210">Run the application, and select **Add entities** to see results similar to the following screen shot:</span></span>
  
    ![Lägg till entiteter](./media/vs-storage-aspnet-getting-started-tables/add-entities-results.png)

    <span data-ttu-id="81dc9-212">Du kan kontrollera att entiteten har lagts till genom att följa stegen i avsnittet [hämta en enda entitet](#get-a-single-entity).</span><span class="sxs-lookup"><span data-stu-id="81dc9-212">You can verify that the entity was added by following the steps in the section, [Get a single entity](#get-a-single-entity).</span></span> <span data-ttu-id="81dc9-213">Du kan också använda den [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) att visa alla entiteter för tabeller.</span><span class="sxs-lookup"><span data-stu-id="81dc9-213">You can also use the [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) to view all the entities for your tables.</span></span>

## <a name="get-a-single-entity"></a><span data-ttu-id="81dc9-214">Hämta en enda entitet</span><span class="sxs-lookup"><span data-stu-id="81dc9-214">Get a single entity</span></span>

<span data-ttu-id="81dc9-215">Det här avsnittet beskriver hur du får en enda entitet från en tabell med hjälp av den entitets radnyckel och partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="81dc9-215">This section illustrates how to get a single entity from a table using the entity's row key and partition key.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="81dc9-216">Det här avsnittet förutsätter att du har slutfört stegen i [Konfigurera utvecklingsmiljön](#set-up-the-development-environment), och använder data från [lägga till en batch med entiteter i en tabell](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="81dc9-216">This section assumes you have completed the steps in [Set up the development environment](#set-up-the-development-environment), and uses data from [Add a batch of entities to a table](#add-a-batch-of-entities-to-a-table).</span></span> 

1. <span data-ttu-id="81dc9-217">Öppna filen `TablesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="81dc9-217">Open the `TablesController.cs` file.</span></span>

1. <span data-ttu-id="81dc9-218">Lägg till en metod som kallas **GetSingle** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-218">Add a method called **GetSingle** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult GetSingle()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="81dc9-219">I den **GetSingle** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="81dc9-219">Within the **GetSingle** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="81dc9-220">Använda följande kod för att hämta anslutningssträngen för lagring och information om lagringskonto från Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* till namnet på Azure storage-konto du försöker komma åt.)</span><span class="sxs-lookup"><span data-stu-id="81dc9-220">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="81dc9-221">Hämta en **CloudTableClient** -objektet representerar en tabelltjänstens klient.</span><span class="sxs-lookup"><span data-stu-id="81dc9-221">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="81dc9-222">Hämta en **CloudTable** objekt som representerar en referens till tabellen från vilken du hämtar entiteten.</span><span class="sxs-lookup"><span data-stu-id="81dc9-222">Get a **CloudTable** object that represents a reference to the table from which you are retrieving the entity.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="81dc9-223">Skapa ett objekt för hämta åtgärd som tar ett entitetsobjekt som härletts från **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-223">Create a retrieve operation object that takes an entity object derived from **TableEntity**.</span></span> <span data-ttu-id="81dc9-224">Den första parametern är den *partitionKey*, och den andra parametern är den *rowKey*.</span><span class="sxs-lookup"><span data-stu-id="81dc9-224">The first parameter is the *partitionKey*, and the second parameter is the *rowKey*.</span></span> <span data-ttu-id="81dc9-225">Med hjälp av den **CustomerEntity** klass och data som anges i avsnittet [lägga till en batch med entiteter i en tabell](#add-a-batch-of-entities-to-a-table), följande kodavsnitt frågar tabellen för en **CustomerEntity** entitet med en *partitionKey* värdet för ”Smith” och en *rowKey* värdet för ”Ben”:</span><span class="sxs-lookup"><span data-stu-id="81dc9-225">Using the **CustomerEntity** class and data presented in the section [Add a batch of entities to a table](#add-a-batch-of-entities-to-a-table), the following code snippet queries the table for a **CustomerEntity** entity with a *partitionKey* value of "Smith" and a *rowKey* value of "Ben":</span></span>

    ```csharp
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");
    ```

1. <span data-ttu-id="81dc9-226">Köra hämtningen.</span><span class="sxs-lookup"><span data-stu-id="81dc9-226">Execute the retrieve operation.</span></span>   

    ```csharp
    TableResult result = table.Execute(retrieveOperation);
    ```

1. <span data-ttu-id="81dc9-227">Skicka resultatet till vyn för visning.</span><span class="sxs-lookup"><span data-stu-id="81dc9-227">Pass the result to the view for display.</span></span>

    ```csharp
    return View(result);
    ```

1. <span data-ttu-id="81dc9-228">I den **Solution Explorer**, expandera den **vyer** mappen, högerklicka på **tabeller**, och på snabbmenyn väljer **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-228">In the **Solution Explorer**, expand the **Views** folder, right-click **Tables**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="81dc9-229">På den **Lägg till vy** dialogrutan Ange **GetSingle** för namn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-229">On the **Add View** dialog, enter **GetSingle** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="81dc9-230">Öppna `GetSingle.cshtml`, och ändra den så att det ser ut som följande kodavsnitt:</span><span class="sxs-lookup"><span data-stu-id="81dc9-230">Open `GetSingle.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

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

1. <span data-ttu-id="81dc9-231">I den **Solution Explorer**, expandera den **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="81dc9-231">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="81dc9-232">Efter senast **Html.ActionLink**, Lägg till följande **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="81dc9-232">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Get single", "GetSingle", "Tables")</li>
    ```

1. <span data-ttu-id="81dc9-233">Kör programmet och välj **hämta enda** att se resultatet liknar följande Skärmdump:</span><span class="sxs-lookup"><span data-stu-id="81dc9-233">Run the application, and select **Get Single** to see results similar to the following screen shot:</span></span>
  
    ![Hämta enda](./media/vs-storage-aspnet-getting-started-tables/get-single-results.png)

## <a name="get-all-entities-in-a-partition"></a><span data-ttu-id="81dc9-235">Hämta alla entiteter i en partition</span><span class="sxs-lookup"><span data-stu-id="81dc9-235">Get all entities in a partition</span></span>

<span data-ttu-id="81dc9-236">Som nämnts i avsnittet [lägga till en entitet i en tabell](#add-an-entity-to-a-table), identifiera kombinationen av en partition och en rad för en entitet i en tabell.</span><span class="sxs-lookup"><span data-stu-id="81dc9-236">As mentioned in the section, [Add an entity to a table](#add-an-entity-to-a-table), the combination of a partition and a row key uniquely identify an entity in a table.</span></span> <span data-ttu-id="81dc9-237">Entiteter med samma partitionsnyckel kan frågas snabbare än entiteter med olika partitionsnycklar.</span><span class="sxs-lookup"><span data-stu-id="81dc9-237">Entities with the same partition key can be queried faster than entities with different partition keys.</span></span> <span data-ttu-id="81dc9-238">Det här avsnittet visar hur du kan fråga en tabell efter alla entiteter från en angiven partition.</span><span class="sxs-lookup"><span data-stu-id="81dc9-238">This section illustrates how to query a table for all the entities from a specified partition.</span></span>  

> [!NOTE]
> 
> <span data-ttu-id="81dc9-239">Det här avsnittet förutsätter att du har slutfört stegen i [Konfigurera utvecklingsmiljön](#set-up-the-development-environment), och använder data från [lägga till en batch med entiteter i en tabell](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="81dc9-239">This section assumes you have completed the steps in [Set up the development environment](#set-up-the-development-environment), and uses data from [Add a batch of entities to a table](#add-a-batch-of-entities-to-a-table).</span></span> 

1. <span data-ttu-id="81dc9-240">Öppna filen `TablesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="81dc9-240">Open the `TablesController.cs` file.</span></span>

1. <span data-ttu-id="81dc9-241">Lägg till en metod som kallas **GetPartition** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-241">Add a method called **GetPartition** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult GetPartition()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="81dc9-242">I den **GetPartition** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="81dc9-242">Within the **GetPartition** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="81dc9-243">Använda följande kod för att hämta anslutningssträngen för lagring och information om lagringskonto från Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* till namnet på Azure storage-konto du försöker komma åt.)</span><span class="sxs-lookup"><span data-stu-id="81dc9-243">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="81dc9-244">Hämta en **CloudTableClient** -objektet representerar en tabelltjänstens klient.</span><span class="sxs-lookup"><span data-stu-id="81dc9-244">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="81dc9-245">Hämta en **CloudTable** objekt som representerar en referens till tabellen från vilken du hämtar entiteterna.</span><span class="sxs-lookup"><span data-stu-id="81dc9-245">Get a **CloudTable** object that represents a reference to the table from which you are retrieving the entities.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="81dc9-246">Skapa en instans av en **TableQuery** objekt som anger frågan i den **där** satsen.</span><span class="sxs-lookup"><span data-stu-id="81dc9-246">Instantiate a **TableQuery** object specifying the query in the **Where** clause.</span></span> <span data-ttu-id="81dc9-247">Med hjälp av den **CustomerEntity** klass och data som anges i avsnittet [lägga till en batch med entiteter i en tabell](#add-a-batch-of-entities-to-a-table), följande kodavsnitt frågar tabellen för alla enheter där den **PartitionKey** (kundens efternamn) har värdet ”Smith”:</span><span class="sxs-lookup"><span data-stu-id="81dc9-247">Using the **CustomerEntity** class and data presented in the section [Add a batch of entities to a table](#add-a-batch-of-entities-to-a-table), the following code snippet queries the table for a all entities where the **PartitionKey** (customer's last name) has a value of "Smith":</span></span>

    ```csharp
    TableQuery<CustomerEntity> query = 
        new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));
    ```

1. <span data-ttu-id="81dc9-248">I en slinga anropa den **CloudTable.ExecuteQuerySegmented** metoden skickar frågeobjektet du instansierad i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="81dc9-248">Within a loop, call the **CloudTable.ExecuteQuerySegmented** method passing the query object you instantiated in the previous step.</span></span>  <span data-ttu-id="81dc9-249">Den **CloudTable.ExecuteQuerySegmented** metoden returnerar en **TableContinuationToken** objekt som - när **null** -anger att det inte finns några fler enheter att hämta.</span><span class="sxs-lookup"><span data-stu-id="81dc9-249">The **CloudTable.ExecuteQuerySegmented** method returns a **TableContinuationToken** object that - when **null** - indicates that there are no more entities to retrieve.</span></span> <span data-ttu-id="81dc9-250">Använd en annan loop inom loopen, och iterera över returnerade entiteter.</span><span class="sxs-lookup"><span data-stu-id="81dc9-250">Within the loop, use another loop to iterate over the returned entities.</span></span> <span data-ttu-id="81dc9-251">I följande kodexempel läggs varje returnerade entitet till en lista.</span><span class="sxs-lookup"><span data-stu-id="81dc9-251">In the following code example, each returned entity is added to a list.</span></span> <span data-ttu-id="81dc9-252">När loopen avslutas listan skickas till en vy för visning:</span><span class="sxs-lookup"><span data-stu-id="81dc9-252">Once the loop ends, the list is passed to a view for display:</span></span> 

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

1. <span data-ttu-id="81dc9-253">I den **Solution Explorer**, expandera den **vyer** mappen, högerklicka på **tabeller**, och på snabbmenyn väljer **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-253">In the **Solution Explorer**, expand the **Views** folder, right-click **Tables**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="81dc9-254">På den **Lägg till vy** dialogrutan Ange **GetPartition** för namn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-254">On the **Add View** dialog, enter **GetPartition** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="81dc9-255">Öppna `GetPartition.cshtml`, och ändra den så att det ser ut som följande kodavsnitt:</span><span class="sxs-lookup"><span data-stu-id="81dc9-255">Open `GetPartition.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

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

1. <span data-ttu-id="81dc9-256">I den **Solution Explorer**, expandera den **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="81dc9-256">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="81dc9-257">Efter senast **Html.ActionLink**, Lägg till följande **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="81dc9-257">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Get partition", "GetPartition", "Tables")</li>
    ```

1. <span data-ttu-id="81dc9-258">Kör programmet och välj **hämta partitionen** att se resultatet liknar följande Skärmdump:</span><span class="sxs-lookup"><span data-stu-id="81dc9-258">Run the application, and select **Get Partition** to see results similar to the following screen shot:</span></span>
  
    ![Hämta Partition](./media/vs-storage-aspnet-getting-started-tables/get-partition-results.png)

## <a name="delete-an-entity"></a><span data-ttu-id="81dc9-260">Ta bort en entitet</span><span class="sxs-lookup"><span data-stu-id="81dc9-260">Delete an entity</span></span>

<span data-ttu-id="81dc9-261">Det här avsnittet beskriver hur du tar bort en entitet från en tabell.</span><span class="sxs-lookup"><span data-stu-id="81dc9-261">This section illustrates how to delete an entity from a table.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="81dc9-262">Det här avsnittet förutsätter att du har slutfört stegen i [Konfigurera utvecklingsmiljön](#set-up-the-development-environment), och använder data från [lägga till en batch med entiteter i en tabell](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="81dc9-262">This section assumes you have completed the steps in [Set up the development environment](#set-up-the-development-environment), and uses data from [Add a batch of entities to a table](#add-a-batch-of-entities-to-a-table).</span></span> 

1. <span data-ttu-id="81dc9-263">Öppna filen `TablesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="81dc9-263">Open the `TablesController.cs` file.</span></span>

1. <span data-ttu-id="81dc9-264">Lägg till en metod som kallas **DeleteEntity** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-264">Add a method called **DeleteEntity** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult DeleteEntity()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="81dc9-265">I den **DeleteEntity** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="81dc9-265">Within the **DeleteEntity** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="81dc9-266">Använda följande kod för att hämta anslutningssträngen för lagring och information om lagringskonto från Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* till namnet på Azure storage-konto du försöker komma åt.)</span><span class="sxs-lookup"><span data-stu-id="81dc9-266">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="81dc9-267">Hämta en **CloudTableClient** -objektet representerar en tabelltjänstens klient.</span><span class="sxs-lookup"><span data-stu-id="81dc9-267">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="81dc9-268">Hämta en **CloudTable** objekt som representerar en referens till tabellen från vilken du vill ta bort entiteten.</span><span class="sxs-lookup"><span data-stu-id="81dc9-268">Get a **CloudTable** object that represents a reference to the table from which you are deleting the entity.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="81dc9-269">Skapa åtgärden ta bort objekt som tar ett entitetsobjekt som härletts från **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-269">Create a delete operation object that takes an entity object derived from **TableEntity**.</span></span> <span data-ttu-id="81dc9-270">I det här fallet används den **CustomerEntity** klass och data som anges i avsnittet [lägga till en batch med entiteter i en tabell](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="81dc9-270">In this case, we use the **CustomerEntity** class and data presented in the section [Add a batch of entities to a table](#add-a-batch-of-entities-to-a-table).</span></span> <span data-ttu-id="81dc9-271">Entitetens **ETag** måste anges till ett giltigt värde.</span><span class="sxs-lookup"><span data-stu-id="81dc9-271">The entity's **ETag** must be set to a valid value.</span></span>  

    ```csharp
    TableOperation deleteOperation = 
        TableOperation.Delete(new CustomerEntity("Smith", "Ben") { ETag = "*" } );
    ```

1. <span data-ttu-id="81dc9-272">Köra delete-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="81dc9-272">Execute the delete operation.</span></span>   

    ```csharp
    TableResult result = table.Execute(deleteOperation);
    ```

1. <span data-ttu-id="81dc9-273">Skicka resultatet till vyn för visning.</span><span class="sxs-lookup"><span data-stu-id="81dc9-273">Pass the result to the view for display.</span></span>

    ```csharp
    return View(result);
    ```

1. <span data-ttu-id="81dc9-274">I den **Solution Explorer**, expandera den **vyer** mappen, högerklicka på **tabeller**, och på snabbmenyn väljer **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-274">In the **Solution Explorer**, expand the **Views** folder, right-click **Tables**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="81dc9-275">På den **Lägg till vy** dialogrutan Ange **DeleteEntity** för namn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="81dc9-275">On the **Add View** dialog, enter **DeleteEntity** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="81dc9-276">Öppna `DeleteEntity.cshtml`, och ändra den så att det ser ut som följande kodavsnitt:</span><span class="sxs-lookup"><span data-stu-id="81dc9-276">Open `DeleteEntity.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

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

1. <span data-ttu-id="81dc9-277">I den **Solution Explorer**, expandera den **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="81dc9-277">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="81dc9-278">Efter senast **Html.ActionLink**, Lägg till följande **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="81dc9-278">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Delete entity", "DeleteEntity", "Tables")</li>
    ```

1. <span data-ttu-id="81dc9-279">Kör programmet och välj **ta bort entiteten** att se resultatet liknar följande Skärmdump:</span><span class="sxs-lookup"><span data-stu-id="81dc9-279">Run the application, and select **Delete entity** to see results similar to the following screen shot:</span></span>
  
    ![Hämta enda](./media/vs-storage-aspnet-getting-started-tables/delete-entity-results.png)

## <a name="next-steps"></a><span data-ttu-id="81dc9-281">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="81dc9-281">Next steps</span></span>
<span data-ttu-id="81dc9-282">Visa fler funktionsguider och lär dig mer om andra alternativ för att lagra data i Azure.</span><span class="sxs-lookup"><span data-stu-id="81dc9-282">View more feature guides to learn about additional options for storing data in Azure.</span></span>

  * [<span data-ttu-id="81dc9-283">Kom igång med Azure blob storage och Visual Studio anslutna tjänster (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="81dc9-283">Get started with Azure blob storage and Visual Studio Connected Services (ASP.NET)</span></span>](./vs-storage-aspnet-getting-started-blobs.md)
  * [<span data-ttu-id="81dc9-284">Kom igång med Azure queue storage- och Visual Studio anslutna tjänster (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="81dc9-284">Get started with Azure queue storage and Visual Studio Connected Services (ASP.NET)</span></span>](./vs-storage-aspnet-getting-started-queues.md)
