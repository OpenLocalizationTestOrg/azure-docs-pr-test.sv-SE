---
title: "aaaGet igång med Azure-tabellagring och Visual Studio anslutna tjänster (ASP.NET) | Microsoft Docs"
description: "Hur tooget igång med Azure-tabellagring i ASP.NET-projekt i Visual Studio när du har anslutit tooa lagringskonto med hjälp av Visual Studio anslutna Services"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: af81a326-18f4-4449-bc0d-e96fba27c1f8
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: kraigb
ms.openlocfilehash: 04f79db7aad60ca51c3c866da1f4b01d9e11ac52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-table-storage-and-visual-studio-connected-services-aspnet"></a><span data-ttu-id="a7b62-103">Kom igång med Azure-tabellagring och Visual Studio anslutna tjänster (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="a7b62-103">Get started with Azure table storage and Visual Studio Connected Services (ASP.NET)</span></span>
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a><span data-ttu-id="a7b62-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="a7b62-104">Overview</span></span>

<span data-ttu-id="a7b62-105">Azure Table storage kan du toostore stora mängder strukturerade data.</span><span class="sxs-lookup"><span data-stu-id="a7b62-105">Azure Table storage enables you toostore large amounts of structured data.</span></span> <span data-ttu-id="a7b62-106">hello-tjänsten är en NoSQL-datalager som tar emot autentiserade anrop inuti och utanför hello Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="a7b62-106">hello service is a NoSQL datastore that accepts authenticated calls from inside and outside hello Azure cloud.</span></span> <span data-ttu-id="a7b62-107">Azure-tabeller passar utmärkt för att lagra strukturerade, icke-relationella data.</span><span class="sxs-lookup"><span data-stu-id="a7b62-107">Azure tables are ideal for storing structured, non-relational data.</span></span>

<span data-ttu-id="a7b62-108">Den här kursen visar hur toowrite ASP.NET kod för några vanliga scenarier med hjälp av Azure table storage entiteter.</span><span class="sxs-lookup"><span data-stu-id="a7b62-108">This tutorial shows how toowrite ASP.NET code for some common scenarios using Azure table storage entities.</span></span> <span data-ttu-id="a7b62-109">Dessa scenarier som inkluderar att skapa en tabell och lägga till, fråga och ta bort tabellentiteter.</span><span class="sxs-lookup"><span data-stu-id="a7b62-109">These scenarios include creating a table, and adding, querying, and deleting table entities.</span></span> 

##<a name="prerequisites"></a><span data-ttu-id="a7b62-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a7b62-110">Prerequisites</span></span>

* [<span data-ttu-id="a7b62-111">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a7b62-111">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="a7b62-112">Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="a7b62-112">Azure storage account</span></span>](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a><span data-ttu-id="a7b62-113">Skapa en MVC-enhet</span><span class="sxs-lookup"><span data-stu-id="a7b62-113">Create an MVC controller</span></span> 

1. <span data-ttu-id="a7b62-114">I hello **Solution Explorer**, högerklicka på **domänkontrollanter**, hello snabbmenyn, Välj **Lägg till -> Controller**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-114">In hello **Solution Explorer**, right-click **Controllers**, and, from hello context menu, select **Add->Controller**.</span></span>

    ![Lägg till en domänkontrollant tooan ASP.NET MVC-app](./media/vs-storage-aspnet-getting-started-tables/add-controller-menu.png)

1. <span data-ttu-id="a7b62-116">På hello **Lägg till Kodskelett** markerar **MVC 5 styrenhet – tom**, och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-116">On hello **Add Scaffold** dialog, select **MVC 5 Controller - Empty**, and select **Add**.</span></span>

    ![Ange typ av MVC-domänkontrollant](./media/vs-storage-aspnet-getting-started-tables/add-controller.png)

1. <span data-ttu-id="a7b62-118">På hello **Lägg till styrenhet** dialogrutan, namnet hello controller *TablesController*, och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-118">On hello **Add Controller** dialog, name hello controller *TablesController*, and select **Add**.</span></span>

    ![Namnet hello MVC-enhet](./media/vs-storage-aspnet-getting-started-tables/add-controller-name.png)

1. <span data-ttu-id="a7b62-120">Lägg till följande hello *med* direktiven toohello `TablesController.cs` fil:</span><span class="sxs-lookup"><span data-stu-id="a7b62-120">Add hello following *using* directives toohello `TablesController.cs` file:</span></span>

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ```

### <a name="create-a-model-class"></a><span data-ttu-id="a7b62-121">Skapa en modellklass</span><span class="sxs-lookup"><span data-stu-id="a7b62-121">Create a model class</span></span>

<span data-ttu-id="a7b62-122">Många av hello exemplen i den här artikeln används en **TableEntity**-härledd klass som kallas **CustomerEntity**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-122">Many of hello examples in this article use a **TableEntity**-derived class called **CustomerEntity**.</span></span> <span data-ttu-id="a7b62-123">hello följande steg när du går igenom deklarera den här klassen som en modellklass:</span><span class="sxs-lookup"><span data-stu-id="a7b62-123">hello following steps guide you through declaring this class as a model class:</span></span>

1. <span data-ttu-id="a7b62-124">I hello **Solution Explorer**, högerklicka på **modeller**, hello snabbmenyn, Välj **Lägg till -> klassen**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-124">In hello **Solution Explorer**, right-click **Models**, and, from hello context menu, select **Add->Class**.</span></span>

1. <span data-ttu-id="a7b62-125">På hello **Lägg till nytt objekt** dialogrutan, namnet hello klassen **CustomerEntity**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-125">On hello **Add New Item** dialog, name hello class, **CustomerEntity**.</span></span>

1. <span data-ttu-id="a7b62-126">Öppna hello `CustomerEntity.cs` filen och Lägg till följande hello **med** direktiv:</span><span class="sxs-lookup"><span data-stu-id="a7b62-126">Open hello `CustomerEntity.cs` file, and add hello following **using** directive:</span></span>

    ```csharp
    using Microsoft.WindowsAzure.Storage.Table;
    ```

1. <span data-ttu-id="a7b62-127">Ändra hello klassen så att när du är klar hello klass har deklarerats som hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="a7b62-127">Modify hello class so that, when finished, hello class is declared as in hello following code.</span></span> <span data-ttu-id="a7b62-128">hello klassen deklarerar en entitetsklass som kallas **CustomerEntity** som använder hello kundens förnamn som hello radnyckel och efternamn som partitionsnyckel hello.</span><span class="sxs-lookup"><span data-stu-id="a7b62-128">hello class declares an entity class called **CustomerEntity** that uses hello customer's first name as hello row key and last name as hello partition key.</span></span>

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

## <a name="create-a-table"></a><span data-ttu-id="a7b62-129">Skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="a7b62-129">Create a table</span></span>

<span data-ttu-id="a7b62-130">hello följande steg visar hur toocreate en tabell:</span><span class="sxs-lookup"><span data-stu-id="a7b62-130">hello following steps illustrate how toocreate a table:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="a7b62-131">Det här avsnittet förutsätter att du har slutfört hello stegen i [ställa in hello utvecklingsmiljö](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="a7b62-131">This section assumes you have completed hello steps in [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="a7b62-132">Öppna hello `TablesController.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="a7b62-132">Open hello `TablesController.cs` file.</span></span>

1. <span data-ttu-id="a7b62-133">Lägg till en metod som kallas **CreateTable** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-133">Add a method called **CreateTable** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult CreateTable()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="a7b62-134">Inom hello **CreateTable** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="a7b62-134">Within hello **CreateTable** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="a7b62-135">Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)</span><span class="sxs-lookup"><span data-stu-id="a7b62-135">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="a7b62-136">Hämta en **CloudTableClient** -objektet representerar en tabelltjänstens klient.</span><span class="sxs-lookup"><span data-stu-id="a7b62-136">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="a7b62-137">Hämta en **CloudTable** objekt som representerar ett referens toohello önskade tabellnamn.</span><span class="sxs-lookup"><span data-stu-id="a7b62-137">Get a **CloudTable** object that represents a reference toohello desired table name.</span></span> <span data-ttu-id="a7b62-138">Hej **CloudTableClient.GetTableReference** inte gör en begäran mot tabellagring.</span><span class="sxs-lookup"><span data-stu-id="a7b62-138">hello **CloudTableClient.GetTableReference** method does not make a request against table storage.</span></span> <span data-ttu-id="a7b62-139">hello referens returneras hello tabellen finns eller inte.</span><span class="sxs-lookup"><span data-stu-id="a7b62-139">hello reference is returned whether or not hello table exists.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="a7b62-140">Anropa hello **CloudTable.CreateIfNotExists** metoden toocreate hello tabell om det inte finns ännu.</span><span class="sxs-lookup"><span data-stu-id="a7b62-140">Call hello **CloudTable.CreateIfNotExists** method toocreate hello table if it does not yet exist.</span></span> <span data-ttu-id="a7b62-141">Hej **CloudTable.CreateIfNotExists** metoden returnerar **SANT** om hello tabellen finns inte och har skapats.</span><span class="sxs-lookup"><span data-stu-id="a7b62-141">hello **CloudTable.CreateIfNotExists** method returns **true** if hello table does not exist, and is successfully created.</span></span> <span data-ttu-id="a7b62-142">Annars **FALSKT** returneras.</span><span class="sxs-lookup"><span data-stu-id="a7b62-142">Otherwise, **false** is returned.</span></span>    

    ```csharp
    ViewBag.Success = table.CreateIfNotExists();
    ```

1. <span data-ttu-id="a7b62-143">Uppdatera hello **ViewBag** med hello namnet på hello tabell.</span><span class="sxs-lookup"><span data-stu-id="a7b62-143">Update hello **ViewBag** with hello name of hello table.</span></span>

    ```csharp
    ViewBag.TableName = table.Name;
    ```

1. <span data-ttu-id="a7b62-144">I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **tabeller**, och hello snabbmenyn, Välj **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-144">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Tables**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="a7b62-145">På hello **Lägg till vy** dialogrutan Ange **CreateTable** hello vynamn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-145">On hello **Add View** dialog, enter **CreateTable** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="a7b62-146">Öppna `CreateTable.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="a7b62-146">Open `CreateTable.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Create Table";
    }
    
    <h2>Create Table results</h2>

    Creation of @ViewBag.TableName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. <span data-ttu-id="a7b62-147">I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="a7b62-147">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="a7b62-148">Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="a7b62-148">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Create table", "CreateTable", "Tables")</li>
    ```

1. <span data-ttu-id="a7b62-149">Kör hello programmet och välj **Skapa tabell** toosee resulterar liknande toohello följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="a7b62-149">Run hello application, and select **Create table** toosee results similar toohello following screen shot:</span></span>
  
    ![Skapa tabell](./media/vs-storage-aspnet-getting-started-tables/create-table-results.png)

    <span data-ttu-id="a7b62-151">Som tidigare nämnts hello **CloudTable.CreateIfNotExists** metoden returnerar **SANT** endast när hello tabellen finns inte och har skapats.</span><span class="sxs-lookup"><span data-stu-id="a7b62-151">As mentioned previously, hello **CloudTable.CreateIfNotExists** method returns **true** only when hello table doesn't exist and is created.</span></span> <span data-ttu-id="a7b62-152">Om du kör hello app när hello tabellen finns hello-metoden returnerar därför **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-152">Therefore, if you run hello app when hello table exists, hello method returns **false**.</span></span> <span data-ttu-id="a7b62-153">toorun hello app flera gånger, måste du ta bort hello tabell innan du kör hello appen igen.</span><span class="sxs-lookup"><span data-stu-id="a7b62-153">toorun hello app multiple times, you must delete hello table before running hello app again.</span></span> <span data-ttu-id="a7b62-154">Radera hello registret kan göras via hello **CloudTable.Delete** metod.</span><span class="sxs-lookup"><span data-stu-id="a7b62-154">Deleting hello table can be done via hello **CloudTable.Delete** method.</span></span> <span data-ttu-id="a7b62-155">Du kan också ta bort hello tabellen med hjälp av hello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040) eller hello [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="a7b62-155">You can also delete hello table using hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) or hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>  

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="a7b62-156">Lägg till en entitet tooa tabell</span><span class="sxs-lookup"><span data-stu-id="a7b62-156">Add an entity tooa table</span></span>

<span data-ttu-id="a7b62-157">*Entiteter* mappa tooC\# objekt med hjälp av en anpassad klass som härleds från **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-157">*Entities* map tooC\# objects by using a custom class derived from **TableEntity**.</span></span> <span data-ttu-id="a7b62-158">tooadd en entitet tooa tabell, skapa en klass som definierar hello egenskaperna för entiteten.</span><span class="sxs-lookup"><span data-stu-id="a7b62-158">tooadd an entity tooa table, create a class that defines hello properties of your entity.</span></span> <span data-ttu-id="a7b62-159">I det här avsnittet visas hur toodefine en entitetsklass som använder hello kundens förnamn som hello radnyckel och efternamn som partitionsnyckel hello.</span><span class="sxs-lookup"><span data-stu-id="a7b62-159">In this section, you'll see how toodefine an entity class that uses hello customer's first name as hello row key and last name as hello partition key.</span></span> <span data-ttu-id="a7b62-160">Tillsammans identifiera en entitets partition och radnyckel hello entiteten i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="a7b62-160">Together, an entity's partition and row key uniquely identify hello entity in hello table.</span></span> <span data-ttu-id="a7b62-161">Det går snabbare att fråga entiteter med samma partitionsnyckel än entiteter som har olika partitionsnycklar, men skalbarheten och möjligheten att utföra parallella åtgärder är större med olika partitionsnycklar.</span><span class="sxs-lookup"><span data-stu-id="a7b62-161">Entities with the same partition key can be queried faster than entities with different partition keys, but using diverse partition keys allows for greater scalability of parallel operations.</span></span> <span data-ttu-id="a7b62-162">För alla egenskaper som ska lagras i tabelltjänsten hello måste hello-egenskapen vara en offentlig egenskap för en typ som stöds som exponerar både inställningen och hämtar värden.</span><span class="sxs-lookup"><span data-stu-id="a7b62-162">For any property that should be stored in hello table service, hello property must be a public property of a supported type that exposes both setting and retrieving values.</span></span>
<span data-ttu-id="a7b62-163">Hej enhetsklassen *måste* deklarera en offentlig parameterlös konstruktor.</span><span class="sxs-lookup"><span data-stu-id="a7b62-163">hello entity class *must* declare a public parameter-less constructor.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="a7b62-164">Det här avsnittet förutsätter att du har slutfört hello stegen i [ställa in hello utvecklingsmiljö](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="a7b62-164">This section assumes you have completed hello steps in [Set up hello development environment](#set-up-the-development-environment).</span></span>

1. <span data-ttu-id="a7b62-165">Öppna hello `TablesController.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="a7b62-165">Open hello `TablesController.cs` file.</span></span>

1. <span data-ttu-id="a7b62-166">Lägg till följande direktiv så som hello koden i hello hello `TablesController.cs` filen kan komma åt hello **CustomerEntity** klass:</span><span class="sxs-lookup"><span data-stu-id="a7b62-166">Add hello following directive so that hello code in hello `TablesController.cs` file can access hello **CustomerEntity** class:</span></span>

    ```csharp
    using StorageAspnet.Models;
    ```

1. <span data-ttu-id="a7b62-167">Lägg till en metod som kallas **AddEntity** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-167">Add a method called **AddEntity** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult AddEntity()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="a7b62-168">Inom hello **AddEntity** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="a7b62-168">Within hello **AddEntity** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="a7b62-169">Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)</span><span class="sxs-lookup"><span data-stu-id="a7b62-169">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="a7b62-170">Hämta en **CloudTableClient** -objektet representerar en tabelltjänstens klient.</span><span class="sxs-lookup"><span data-stu-id="a7b62-170">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="a7b62-171">Hämta en **CloudTable** objekt som representerar en referens toohello tabell toowhich ska tooadd hello ny entitet.</span><span class="sxs-lookup"><span data-stu-id="a7b62-171">Get a **CloudTable** object that represents a reference toohello table toowhich you are going tooadd hello new entity.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="a7b62-172">Skapa en instans av och initiera hello **CustomerEntity** klass.</span><span class="sxs-lookup"><span data-stu-id="a7b62-172">Instantiate and initialize hello **CustomerEntity** class.</span></span>

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    ```

1. <span data-ttu-id="a7b62-173">Skapa en **TableOperation** objekt som infogar kundentiteten hello.</span><span class="sxs-lookup"><span data-stu-id="a7b62-173">Create a **TableOperation** object that inserts hello customer entity.</span></span>

    ```csharp
    TableOperation insertOperation = TableOperation.Insert(customer1);
    ```

1. <span data-ttu-id="a7b62-174">Köra hello insert-åtgärden genom att anropa hello **CloudTable.Execute** metod.</span><span class="sxs-lookup"><span data-stu-id="a7b62-174">Execute hello insert operation by calling hello **CloudTable.Execute** method.</span></span> <span data-ttu-id="a7b62-175">Du kan verifiera hello resultatet av hello genom att inspektera hello **TableResult.HttpStatusCode** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="a7b62-175">You can verify hello result of hello operation by inspecting hello **TableResult.HttpStatusCode** property.</span></span> <span data-ttu-id="a7b62-176">Statuskoden 2xx anger hello åtgärden som begärs av hello-klient har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="a7b62-176">A status code of 2xx indicates hello action requested by hello client was processed successfully.</span></span> <span data-ttu-id="a7b62-177">Till exempel lyckad infogningar nya resulterar i en HTTP-statuskod 204, vilket innebär att hello-åtgärden bearbetades och hello servern returnerade inte något innehåll.</span><span class="sxs-lookup"><span data-stu-id="a7b62-177">For example, successful insertions of new entities results in an HTTP status code of 204, meaning that hello operation was successfully processed and hello server did not return any content.</span></span>

    ```csharp
    TableResult result = table.Execute(insertOperation);
    ```

1. <span data-ttu-id="a7b62-178">Uppdatera hello **ViewBag** med hello tabellnamn och hello resultatet av hello insert-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a7b62-178">Update hello **ViewBag** with hello table name, and hello results of hello insert operation.</span></span>

    ```csharp
    ViewBag.TableName = table.Name;
    ViewBag.Result = result.HttpStatusCode;
    ```

1. <span data-ttu-id="a7b62-179">I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **tabeller**, och hello snabbmenyn, Välj **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-179">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Tables**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="a7b62-180">På hello **Lägg till vy** dialogrutan Ange **AddEntity** hello vynamn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-180">On hello **Add View** dialog, enter **AddEntity** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="a7b62-181">Öppna `AddEntity.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="a7b62-181">Open `AddEntity.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Add entity";
    }
    
    <h2>Add entity results</h2>

    Insert of entity into @ViewBag.TableName @(ViewBag.Result == 204 ? "succeeded" : "failed")
    ```
1. <span data-ttu-id="a7b62-182">I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="a7b62-182">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="a7b62-183">Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="a7b62-183">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Add entity", "AddEntity", "Tables")</li>
    ```

1. <span data-ttu-id="a7b62-184">Kör hello programmet och välj **lägga till enheten** toosee resulterar liknande toohello följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="a7b62-184">Run hello application, and select **Add entity** toosee results similar toohello following screen shot:</span></span>
  
    ![Lägga till entitet](./media/vs-storage-aspnet-getting-started-tables/add-entity-results.png)

    <span data-ttu-id="a7b62-186">Du kan verifiera att hello entiteten har lagts till genom att följa stegen hello hello under [hämta en enda entitet](#get-a-single-entity).</span><span class="sxs-lookup"><span data-stu-id="a7b62-186">You can verify that hello entity was added by following hello steps in hello section, [Get a single entity](#get-a-single-entity).</span></span> <span data-ttu-id="a7b62-187">Du kan också använda hello [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooview alla hello entiteter för tabeller.</span><span class="sxs-lookup"><span data-stu-id="a7b62-187">You can also use hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooview all hello entities for your tables.</span></span>

## <a name="add-a-batch-of-entities-tooa-table"></a><span data-ttu-id="a7b62-188">Lägg till en batch med entiteter tooa tabell</span><span class="sxs-lookup"><span data-stu-id="a7b62-188">Add a batch of entities tooa table</span></span>

<span data-ttu-id="a7b62-189">I tillägg toobeing kan för[lägga till en entitet tooa tabell en i taget](#add-an-entity-to-a-table), du kan också lägga till entiteter i en batch.</span><span class="sxs-lookup"><span data-stu-id="a7b62-189">In addition toobeing able too[add an entity tooa table one at a time](#add-an-entity-to-a-table), you can also add entities in batch.</span></span> <span data-ttu-id="a7b62-190">Lägger till enheter i batch minskar hello antalet turer mellan kod och hello Azure tabelltjänsten.</span><span class="sxs-lookup"><span data-stu-id="a7b62-190">Adding entities in batch reduces hello number of round-trips between your code and hello Azure table service.</span></span> <span data-ttu-id="a7b62-191">hello följande steg visar hur tooadd flera entiteter tooa tabell med en enda infogningen:</span><span class="sxs-lookup"><span data-stu-id="a7b62-191">hello following steps illustrate how tooadd multiple entities tooa table with a single insert operation:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="a7b62-192">Det här avsnittet förutsätter att du har slutfört hello stegen i [ställa in hello utvecklingsmiljö](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="a7b62-192">This section assumes you have completed hello steps in [Set up hello development environment](#set-up-the-development-environment).</span></span>

1. <span data-ttu-id="a7b62-193">Öppna hello `TablesController.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="a7b62-193">Open hello `TablesController.cs` file.</span></span>

1. <span data-ttu-id="a7b62-194">Lägg till en metod som kallas **AddEntities** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-194">Add a method called **AddEntities** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult AddEntities()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="a7b62-195">Inom hello **AddEntities** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="a7b62-195">Within hello **AddEntities** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="a7b62-196">Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)</span><span class="sxs-lookup"><span data-stu-id="a7b62-196">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="a7b62-197">Hämta en **CloudTableClient** -objektet representerar en tabelltjänstens klient.</span><span class="sxs-lookup"><span data-stu-id="a7b62-197">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="a7b62-198">Hämta en **CloudTable** objekt som representerar en referens toohello tabellen toowhich du är pågående tooadd hello nya entiteter.</span><span class="sxs-lookup"><span data-stu-id="a7b62-198">Get a **CloudTable** object that represents a reference toohello table toowhich you are going tooadd hello new entities.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="a7b62-199">Skapa en instans av vissa kundobjekt baserat på hello **CustomerEntity** modellklass som visas i avsnittet hello, [Lägg till en entitet tooa tabell](#add-an-entity-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="a7b62-199">Instantiate some customer objects based on hello **CustomerEntity** model class presented in hello section, [Add an entity tooa table](#add-an-entity-to-a-table).</span></span>

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";

    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    ```

1. <span data-ttu-id="a7b62-200">Hämta en **TableBatchOperation** objekt.</span><span class="sxs-lookup"><span data-stu-id="a7b62-200">Get a **TableBatchOperation** object.</span></span>

    ```csharp
    TableBatchOperation batchOperation = new TableBatchOperation();
    ```

1. <span data-ttu-id="a7b62-201">Lägga till entiteter toohello batch insert-åtgärden objekt.</span><span class="sxs-lookup"><span data-stu-id="a7b62-201">Add entities toohello batch insert operation object.</span></span>

    ```csharp
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);
    ```

1. <span data-ttu-id="a7b62-202">Köra hello batch insert-åtgärden genom att anropa hello **CloudTable.ExecuteBatch** metod.</span><span class="sxs-lookup"><span data-stu-id="a7b62-202">Execute hello batch insert operation by calling hello **CloudTable.ExecuteBatch** method.</span></span>   

    ```csharp
    IList<TableResult> results = table.ExecuteBatch(batchOperation);
    ```

1. <span data-ttu-id="a7b62-203">Hej **CloudTable.ExecuteBatch** metoden returnerar en lista över **TableResult** objekt där varje **TableResult** objekt kan vara undersökas toodetermine hello lyckats eller misslyckats för varje enskild transaktion.</span><span class="sxs-lookup"><span data-stu-id="a7b62-203">hello **CloudTable.ExecuteBatch** method returns a list of **TableResult** objects where each **TableResult** object can be examined toodetermine hello success or failure of each individual operation.</span></span> <span data-ttu-id="a7b62-204">I det här exemplet skickar hello tooa listvyn och låt hello vy som visar hello resultaten av varje åtgärd.</span><span class="sxs-lookup"><span data-stu-id="a7b62-204">For this example, pass hello list tooa view and let hello view display hello results of each operation.</span></span> 
 
    ```csharp
    return View(results);
    ```

1. <span data-ttu-id="a7b62-205">I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **tabeller**, och hello snabbmenyn, Välj **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-205">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Tables**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="a7b62-206">På hello **Lägg till vy** dialogrutan Ange **AddEntities** hello vynamn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-206">On hello **Add View** dialog, enter **AddEntities** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="a7b62-207">Öppna `AddEntities.cshtml`, och ändra den så att det ser ut som följande hello.</span><span class="sxs-lookup"><span data-stu-id="a7b62-207">Open `AddEntities.cshtml`, and modify it so that it looks like hello following.</span></span>

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

1. <span data-ttu-id="a7b62-208">I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="a7b62-208">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="a7b62-209">Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="a7b62-209">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Add entities", "AddEntities", "Tables")</li>
    ```

1. <span data-ttu-id="a7b62-210">Kör hello programmet och välj **lägga till enheter** toosee resulterar liknande toohello följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="a7b62-210">Run hello application, and select **Add entities** toosee results similar toohello following screen shot:</span></span>
  
    ![Lägg till entiteter](./media/vs-storage-aspnet-getting-started-tables/add-entities-results.png)

    <span data-ttu-id="a7b62-212">Du kan verifiera att hello entiteten har lagts till genom att följa stegen hello hello under [hämta en enda entitet](#get-a-single-entity).</span><span class="sxs-lookup"><span data-stu-id="a7b62-212">You can verify that hello entity was added by following hello steps in hello section, [Get a single entity](#get-a-single-entity).</span></span> <span data-ttu-id="a7b62-213">Du kan också använda hello [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooview alla hello entiteter för tabeller.</span><span class="sxs-lookup"><span data-stu-id="a7b62-213">You can also use hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooview all hello entities for your tables.</span></span>

## <a name="get-a-single-entity"></a><span data-ttu-id="a7b62-214">Hämta en enda entitet</span><span class="sxs-lookup"><span data-stu-id="a7b62-214">Get a single entity</span></span>

<span data-ttu-id="a7b62-215">Detta avsnitt visar hur tooget en enda entitet från en tabell med hjälp av hello entitets radnyckel och partitionsnyckel.</span><span class="sxs-lookup"><span data-stu-id="a7b62-215">This section illustrates how tooget a single entity from a table using hello entity's row key and partition key.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="a7b62-216">Det här avsnittet förutsätter att du har slutfört hello stegen i [ställa in hello utvecklingsmiljö](#set-up-the-development-environment), och använder data från [lägga till en batch med entiteter tooa tabellen](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="a7b62-216">This section assumes you have completed hello steps in [Set up hello development environment](#set-up-the-development-environment), and uses data from [Add a batch of entities tooa table](#add-a-batch-of-entities-to-a-table).</span></span> 

1. <span data-ttu-id="a7b62-217">Öppna hello `TablesController.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="a7b62-217">Open hello `TablesController.cs` file.</span></span>

1. <span data-ttu-id="a7b62-218">Lägg till en metod som kallas **GetSingle** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-218">Add a method called **GetSingle** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult GetSingle()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="a7b62-219">Inom hello **GetSingle** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="a7b62-219">Within hello **GetSingle** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="a7b62-220">Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)</span><span class="sxs-lookup"><span data-stu-id="a7b62-220">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="a7b62-221">Hämta en **CloudTableClient** -objektet representerar en tabelltjänstens klient.</span><span class="sxs-lookup"><span data-stu-id="a7b62-221">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="a7b62-222">Hämta en **CloudTable** objekt som representerar en toohello tabell från vilken du hämtar hello entitet.</span><span class="sxs-lookup"><span data-stu-id="a7b62-222">Get a **CloudTable** object that represents a reference toohello table from which you are retrieving hello entity.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="a7b62-223">Skapa ett objekt för hämta åtgärd som tar ett entitetsobjekt som härletts från **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-223">Create a retrieve operation object that takes an entity object derived from **TableEntity**.</span></span> <span data-ttu-id="a7b62-224">hello första parametern är hello *partitionKey*, och andra hello-parametern är hello *rowKey*.</span><span class="sxs-lookup"><span data-stu-id="a7b62-224">hello first parameter is hello *partitionKey*, and hello second parameter is hello *rowKey*.</span></span> <span data-ttu-id="a7b62-225">Med hjälp av hello **CustomerEntity** klass och data som visas i avsnittet hello [lägga till en batch med entiteter tooa tabell](#add-a-batch-of-entities-to-a-table), hello följande kodfragment frågor hello tabellen för en **CustomerEntity** entitet med en *partitionKey* värdet för ”Smith” och en *rowKey* värdet för ”Ben”:</span><span class="sxs-lookup"><span data-stu-id="a7b62-225">Using hello **CustomerEntity** class and data presented in hello section [Add a batch of entities tooa table](#add-a-batch-of-entities-to-a-table), hello following code snippet queries hello table for a **CustomerEntity** entity with a *partitionKey* value of "Smith" and a *rowKey* value of "Ben":</span></span>

    ```csharp
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");
    ```

1. <span data-ttu-id="a7b62-226">Köra hello hämtningen.</span><span class="sxs-lookup"><span data-stu-id="a7b62-226">Execute hello retrieve operation.</span></span>   

    ```csharp
    TableResult result = table.Execute(retrieveOperation);
    ```

1. <span data-ttu-id="a7b62-227">Skicka hello resultatet toohello vy för visning.</span><span class="sxs-lookup"><span data-stu-id="a7b62-227">Pass hello result toohello view for display.</span></span>

    ```csharp
    return View(result);
    ```

1. <span data-ttu-id="a7b62-228">I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **tabeller**, och hello snabbmenyn, Välj **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-228">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Tables**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="a7b62-229">På hello **Lägg till vy** dialogrutan Ange **GetSingle** hello vynamn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-229">On hello **Add View** dialog, enter **GetSingle** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="a7b62-230">Öppna `GetSingle.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="a7b62-230">Open `GetSingle.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

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

1. <span data-ttu-id="a7b62-231">I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="a7b62-231">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="a7b62-232">Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="a7b62-232">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Get single", "GetSingle", "Tables")</li>
    ```

1. <span data-ttu-id="a7b62-233">Kör hello programmet och välj **hämta enda** toosee resulterar liknande toohello följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="a7b62-233">Run hello application, and select **Get Single** toosee results similar toohello following screen shot:</span></span>
  
    ![Hämta enda](./media/vs-storage-aspnet-getting-started-tables/get-single-results.png)

## <a name="get-all-entities-in-a-partition"></a><span data-ttu-id="a7b62-235">Hämta alla entiteter i en partition</span><span class="sxs-lookup"><span data-stu-id="a7b62-235">Get all entities in a partition</span></span>

<span data-ttu-id="a7b62-236">Som tidigare nämnts hello under [lägga till en entitet tooa tabell](#add-an-entity-to-a-table), identifiera hello kombination av en partition och en rad för en entitet i en tabell.</span><span class="sxs-lookup"><span data-stu-id="a7b62-236">As mentioned in hello section, [Add an entity tooa table](#add-an-entity-to-a-table), hello combination of a partition and a row key uniquely identify an entity in a table.</span></span> <span data-ttu-id="a7b62-237">Entiteter med samma partitionsnyckel kan frågas snabbare än entiteter med olika partitionsnycklar.</span><span class="sxs-lookup"><span data-stu-id="a7b62-237">Entities with the same partition key can be queried faster than entities with different partition keys.</span></span> <span data-ttu-id="a7b62-238">Detta avsnitt visar hur tooquery en tabell för alla hello entiteter från en angiven partition.</span><span class="sxs-lookup"><span data-stu-id="a7b62-238">This section illustrates how tooquery a table for all hello entities from a specified partition.</span></span>  

> [!NOTE]
> 
> <span data-ttu-id="a7b62-239">Det här avsnittet förutsätter att du har slutfört hello stegen i [ställa in hello utvecklingsmiljö](#set-up-the-development-environment), och använder data från [lägga till en batch med entiteter tooa tabellen](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="a7b62-239">This section assumes you have completed hello steps in [Set up hello development environment](#set-up-the-development-environment), and uses data from [Add a batch of entities tooa table](#add-a-batch-of-entities-to-a-table).</span></span> 

1. <span data-ttu-id="a7b62-240">Öppna hello `TablesController.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="a7b62-240">Open hello `TablesController.cs` file.</span></span>

1. <span data-ttu-id="a7b62-241">Lägg till en metod som kallas **GetPartition** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-241">Add a method called **GetPartition** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult GetPartition()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="a7b62-242">Inom hello **GetPartition** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="a7b62-242">Within hello **GetPartition** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="a7b62-243">Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)</span><span class="sxs-lookup"><span data-stu-id="a7b62-243">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="a7b62-244">Hämta en **CloudTableClient** -objektet representerar en tabelltjänstens klient.</span><span class="sxs-lookup"><span data-stu-id="a7b62-244">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="a7b62-245">Hämta en **CloudTable** objekt som representerar en toohello tabell från vilken du hämtar hello entiteter.</span><span class="sxs-lookup"><span data-stu-id="a7b62-245">Get a **CloudTable** object that represents a reference toohello table from which you are retrieving hello entities.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="a7b62-246">Skapa en instans av en **TableQuery** objekt som anger hello frågan i hello **där** satsen.</span><span class="sxs-lookup"><span data-stu-id="a7b62-246">Instantiate a **TableQuery** object specifying hello query in hello **Where** clause.</span></span> <span data-ttu-id="a7b62-247">Med hjälp av hello **CustomerEntity** klass och data som visas i avsnittet hello [lägga till en batch med entiteter tooa tabell](#add-a-batch-of-entities-to-a-table), hello följande kod fragment frågor hello tabellen för alla enheter där hello  **PartitionKey** (kundens efternamn) har värdet ”Smith”:</span><span class="sxs-lookup"><span data-stu-id="a7b62-247">Using hello **CustomerEntity** class and data presented in hello section [Add a batch of entities tooa table](#add-a-batch-of-entities-to-a-table), hello following code snippet queries hello table for a all entities where hello **PartitionKey** (customer's last name) has a value of "Smith":</span></span>

    ```csharp
    TableQuery<CustomerEntity> query = 
        new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));
    ```

1. <span data-ttu-id="a7b62-248">I en slinga anropa hello **CloudTable.ExecuteQuerySegmented** metoden skicka hello frågeobjekt du instansierad i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="a7b62-248">Within a loop, call hello **CloudTable.ExecuteQuerySegmented** method passing hello query object you instantiated in hello previous step.</span></span>  <span data-ttu-id="a7b62-249">Hej **CloudTable.ExecuteQuerySegmented** metoden returnerar en **TableContinuationToken** objekt som - när **null** -anger att det inte finns några fler entiteter tooretrieve.</span><span class="sxs-lookup"><span data-stu-id="a7b62-249">hello **CloudTable.ExecuteQuerySegmented** method returns a **TableContinuationToken** object that - when **null** - indicates that there are no more entities tooretrieve.</span></span> <span data-ttu-id="a7b62-250">Använda en annan loop tooiterate över hello returnerade enheter inom hello loop.</span><span class="sxs-lookup"><span data-stu-id="a7b62-250">Within hello loop, use another loop tooiterate over hello returned entities.</span></span> <span data-ttu-id="a7b62-251">I följande kodexempel hello, läggs varje returnerade entitet tooa lista.</span><span class="sxs-lookup"><span data-stu-id="a7b62-251">In hello following code example, each returned entity is added tooa list.</span></span> <span data-ttu-id="a7b62-252">En gång Hej loopen avslutas, hello listan skickas tooa vy för visning:</span><span class="sxs-lookup"><span data-stu-id="a7b62-252">Once hello loop ends, hello list is passed tooa view for display:</span></span> 

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

1. <span data-ttu-id="a7b62-253">I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **tabeller**, och hello snabbmenyn, Välj **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-253">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Tables**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="a7b62-254">På hello **Lägg till vy** dialogrutan Ange **GetPartition** hello vynamn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-254">On hello **Add View** dialog, enter **GetPartition** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="a7b62-255">Öppna `GetPartition.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="a7b62-255">Open `GetPartition.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

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

1. <span data-ttu-id="a7b62-256">I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="a7b62-256">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="a7b62-257">Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="a7b62-257">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Get partition", "GetPartition", "Tables")</li>
    ```

1. <span data-ttu-id="a7b62-258">Kör hello programmet och välj **hämta partitionen** toosee resulterar liknande toohello följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="a7b62-258">Run hello application, and select **Get Partition** toosee results similar toohello following screen shot:</span></span>
  
    ![Hämta Partition](./media/vs-storage-aspnet-getting-started-tables/get-partition-results.png)

## <a name="delete-an-entity"></a><span data-ttu-id="a7b62-260">Ta bort en entitet</span><span class="sxs-lookup"><span data-stu-id="a7b62-260">Delete an entity</span></span>

<span data-ttu-id="a7b62-261">Detta avsnitt visar hur toodelete en entitet från en tabell.</span><span class="sxs-lookup"><span data-stu-id="a7b62-261">This section illustrates how toodelete an entity from a table.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="a7b62-262">Det här avsnittet förutsätter att du har slutfört hello stegen i [ställa in hello utvecklingsmiljö](#set-up-the-development-environment), och använder data från [lägga till en batch med entiteter tooa tabellen](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="a7b62-262">This section assumes you have completed hello steps in [Set up hello development environment](#set-up-the-development-environment), and uses data from [Add a batch of entities tooa table](#add-a-batch-of-entities-to-a-table).</span></span> 

1. <span data-ttu-id="a7b62-263">Öppna hello `TablesController.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="a7b62-263">Open hello `TablesController.cs` file.</span></span>

1. <span data-ttu-id="a7b62-264">Lägg till en metod som kallas **DeleteEntity** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-264">Add a method called **DeleteEntity** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult DeleteEntity()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="a7b62-265">Inom hello **DeleteEntity** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="a7b62-265">Within hello **DeleteEntity** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="a7b62-266">Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)</span><span class="sxs-lookup"><span data-stu-id="a7b62-266">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="a7b62-267">Hämta en **CloudTableClient** -objektet representerar en tabelltjänstens klient.</span><span class="sxs-lookup"><span data-stu-id="a7b62-267">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="a7b62-268">Hämta en **CloudTable** objekt som representerar en toohello tabell från vilken du vill ta bort hello entitet.</span><span class="sxs-lookup"><span data-stu-id="a7b62-268">Get a **CloudTable** object that represents a reference toohello table from which you are deleting hello entity.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="a7b62-269">Skapa åtgärden ta bort objekt som tar ett entitetsobjekt som härletts från **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-269">Create a delete operation object that takes an entity object derived from **TableEntity**.</span></span> <span data-ttu-id="a7b62-270">I detta fall kan vi använda hello **CustomerEntity** klass och data som visas i avsnittet hello [lägga till en batch med entiteter tooa tabellen](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="a7b62-270">In this case, we use hello **CustomerEntity** class and data presented in hello section [Add a batch of entities tooa table](#add-a-batch-of-entities-to-a-table).</span></span> <span data-ttu-id="a7b62-271">Hej entitetens **ETag** tooa giltigt värde måste anges.</span><span class="sxs-lookup"><span data-stu-id="a7b62-271">hello entity's **ETag** must be set tooa valid value.</span></span>  

    ```csharp
    TableOperation deleteOperation = 
        TableOperation.Delete(new CustomerEntity("Smith", "Ben") { ETag = "*" } );
    ```

1. <span data-ttu-id="a7b62-272">Köra hello borttagningsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="a7b62-272">Execute hello delete operation.</span></span>   

    ```csharp
    TableResult result = table.Execute(deleteOperation);
    ```

1. <span data-ttu-id="a7b62-273">Skicka hello resultatet toohello vy för visning.</span><span class="sxs-lookup"><span data-stu-id="a7b62-273">Pass hello result toohello view for display.</span></span>

    ```csharp
    return View(result);
    ```

1. <span data-ttu-id="a7b62-274">I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **tabeller**, och hello snabbmenyn, Välj **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-274">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Tables**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="a7b62-275">På hello **Lägg till vy** dialogrutan Ange **DeleteEntity** hello vynamn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a7b62-275">On hello **Add View** dialog, enter **DeleteEntity** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="a7b62-276">Öppna `DeleteEntity.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="a7b62-276">Open `DeleteEntity.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

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

1. <span data-ttu-id="a7b62-277">I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="a7b62-277">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="a7b62-278">Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="a7b62-278">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Delete entity", "DeleteEntity", "Tables")</li>
    ```

1. <span data-ttu-id="a7b62-279">Kör hello programmet och välj **ta bort entiteten** toosee resulterar liknande toohello följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="a7b62-279">Run hello application, and select **Delete entity** toosee results similar toohello following screen shot:</span></span>
  
    ![Hämta enda](./media/vs-storage-aspnet-getting-started-tables/delete-entity-results.png)

## <a name="next-steps"></a><span data-ttu-id="a7b62-281">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a7b62-281">Next steps</span></span>
<span data-ttu-id="a7b62-282">Visa mer funktionen guider toolearn om ytterligare alternativ för att lagra data i Azure.</span><span class="sxs-lookup"><span data-stu-id="a7b62-282">View more feature guides toolearn about additional options for storing data in Azure.</span></span>

  * [<span data-ttu-id="a7b62-283">Kom igång med Azure blob storage och Visual Studio anslutna tjänster (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="a7b62-283">Get started with Azure blob storage and Visual Studio Connected Services (ASP.NET)</span></span>](../storage/vs-storage-aspnet-getting-started-blobs.md)
  * [<span data-ttu-id="a7b62-284">Kom igång med Azure queue storage- och Visual Studio anslutna tjänster (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="a7b62-284">Get started with Azure queue storage and Visual Studio Connected Services (ASP.NET)</span></span>](../storage/vs-storage-aspnet-getting-started-queues.md)
