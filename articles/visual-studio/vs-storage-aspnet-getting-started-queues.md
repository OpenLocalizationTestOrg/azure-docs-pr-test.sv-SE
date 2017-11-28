---
title: "Kom igång med Azure queue storage- och Visual Studio anslutna tjänster (ASP.NET) | Microsoft Docs"
description: "Hur du kommer igång med Azure queue storage i ASP.NET-projekt i Visual Studio efter anslutning till ett lagringskonto med hjälp av Visual Studio anslutna Services"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 94ca3413-5497-433f-abbe-836f83a9de72
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/23/2016
ms.author: kraigb
ms.openlocfilehash: 4687e5dfce72583728068c176d86d100313badf6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services-aspnet"></a><span data-ttu-id="86d20-103">Kom igång med Azure queue storage- och Visual Studio anslutna tjänster (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="86d20-103">Get started with Azure queue storage and Visual Studio Connected Services (ASP.NET)</span></span>
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="86d20-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="86d20-104">Overview</span></span>

<span data-ttu-id="86d20-105">Azure queue storage erbjuder molntjänster meddelandehantering mellan programkomponenter.</span><span class="sxs-lookup"><span data-stu-id="86d20-105">Azure queue storage provides cloud messaging between application components.</span></span> <span data-ttu-id="86d20-106">När program utformas för skalning är programkomponenterna ofta fristående, så att de kan skalas oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="86d20-106">In designing applications for scale, application components are often decoupled, so that they can scale independently.</span></span> <span data-ttu-id="86d20-107">Queue Storage är en asynkron meddelandelösning för kommunikation mellan programkomponenter, oavsett om de körs i molnet, på skrivbordet, på en lokal server eller på en mobil enhet.</span><span class="sxs-lookup"><span data-stu-id="86d20-107">Queue storage delivers asynchronous messaging for communication between application components, whether they are running in the cloud, on the desktop, on an on-premises server, or on a mobile device.</span></span> <span data-ttu-id="86d20-108">Queue Storage har också stöd för hantering av asynkrona åtgärder och utveckling av processarbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="86d20-108">Queue storage also supports managing asynchronous tasks and building process work flows.</span></span>

<span data-ttu-id="86d20-109">Den här kursen visar hur du skriver ASP.NET-kod för några vanliga scenarier med hjälp av Azure queue storage entiteter.</span><span class="sxs-lookup"><span data-stu-id="86d20-109">This tutorial shows how to write ASP.NET code for some common scenarios using Azure queue storage entities.</span></span> <span data-ttu-id="86d20-110">Dessa scenarier som inkluderar vanliga aktiviteter som att skapa en Azure-kö och lägga till, ändra, läsa och tar bort Kömeddelanden.</span><span class="sxs-lookup"><span data-stu-id="86d20-110">These scenarios include common tasks such as creating an Azure queue, and adding, modifying, reading, and removing queue messages.</span></span>

##<a name="prerequisites"></a><span data-ttu-id="86d20-111">Krav</span><span class="sxs-lookup"><span data-stu-id="86d20-111">Prerequisites</span></span>

* [<span data-ttu-id="86d20-112">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86d20-112">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="86d20-113">Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="86d20-113">Azure storage account</span></span>](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a><span data-ttu-id="86d20-114">Skapa en MVC-enhet</span><span class="sxs-lookup"><span data-stu-id="86d20-114">Create an MVC controller</span></span> 

1. <span data-ttu-id="86d20-115">I den **Solution Explorer**, högerklicka på **domänkontrollanter**, och på snabbmenyn Välj **Lägg till -> styrenhet**.</span><span class="sxs-lookup"><span data-stu-id="86d20-115">In the **Solution Explorer**, right-click **Controllers**, and, from the context menu, select **Add->Controller**.</span></span>

    ![Lägg till en domänkontrollant i en ASP.NET MVC-app](./media/vs-storage-aspnet-getting-started-queues/add-controller-menu.png)

1. <span data-ttu-id="86d20-117">På den **Lägg till Kodskelett** markerar **MVC 5 styrenhet – tom**, och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="86d20-117">On the **Add Scaffold** dialog, select **MVC 5 Controller - Empty**, and select **Add**.</span></span>

    ![Ange typ av MVC-domänkontrollant](./media/vs-storage-aspnet-getting-started-queues/add-controller.png)

1. <span data-ttu-id="86d20-119">På den **Lägg till styrenhet** dialogrutan namn styrenheten *QueuesController*, och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="86d20-119">On the **Add Controller** dialog, name the controller *QueuesController*, and select **Add**.</span></span>

    ![Namnet på MVC-enhet](./media/vs-storage-aspnet-getting-started-queues/add-controller-name.png)

1. <span data-ttu-id="86d20-121">Lägg till följande *med* direktiven till den `QueuesController.cs` filen:</span><span class="sxs-lookup"><span data-stu-id="86d20-121">Add the following *using* directives to the `QueuesController.cs` file:</span></span>

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    ```
## <a name="create-a-queue"></a><span data-ttu-id="86d20-122">Skapa en kö</span><span class="sxs-lookup"><span data-stu-id="86d20-122">Create a queue</span></span>

<span data-ttu-id="86d20-123">Följande steg visar hur du skapar en kö:</span><span class="sxs-lookup"><span data-stu-id="86d20-123">The following steps illustrate how to create a queue:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="86d20-124">Det här avsnittet förutsätter att du har slutfört stegen [Konfigurera utvecklingsmiljön](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="86d20-124">This section assumes you have completed the steps [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="86d20-125">Öppna filen `QueuesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="86d20-125">Open the `QueuesController.cs` file.</span></span> 

1. <span data-ttu-id="86d20-126">Lägg till en metod som kallas **CreateQueue** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="86d20-126">Add a method called **CreateQueue** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult CreateQueue()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="86d20-127">I den **CreateQueue** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="86d20-127">Within the **CreateQueue** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="86d20-128">Använda följande kod för att hämta anslutningssträngen för lagring och information om lagringskonto från Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* till namnet på Azure storage-konto du försöker komma åt.)</span><span class="sxs-lookup"><span data-stu-id="86d20-128">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="86d20-129">Hämta en **CloudQueueClient** -objektet representerar en queue-tjänstklienten.</span><span class="sxs-lookup"><span data-stu-id="86d20-129">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```
1. <span data-ttu-id="86d20-130">Hämta en **CloudQueue** objekt som representerar en referens till önskad kö-namn.</span><span class="sxs-lookup"><span data-stu-id="86d20-130">Get a **CloudQueue** object that represents a reference to the desired queue name.</span></span> <span data-ttu-id="86d20-131">Den **CloudQueueClient.GetQueueReference** inte gör en begäran mot queue storage.</span><span class="sxs-lookup"><span data-stu-id="86d20-131">The **CloudQueueClient.GetQueueReference** method does not make a request against queue storage.</span></span> <span data-ttu-id="86d20-132">Referensen returneras om kön finns eller inte.</span><span class="sxs-lookup"><span data-stu-id="86d20-132">The reference is returned whether or not the queue exists.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="86d20-133">Anropa den **CloudQueue.CreateIfNotExists** metod för att skapa kön om det inte finns ännu.</span><span class="sxs-lookup"><span data-stu-id="86d20-133">Call the **CloudQueue.CreateIfNotExists** method to create the queue if it does not yet exist.</span></span> <span data-ttu-id="86d20-134">Den **CloudQueue.CreateIfNotExists** metoden returnerar **SANT** om kön finns inte och har skapats.</span><span class="sxs-lookup"><span data-stu-id="86d20-134">The **CloudQueue.CreateIfNotExists** method returns **true** if the queue does not exist, and is successfully created.</span></span> <span data-ttu-id="86d20-135">Annars **FALSKT** returneras.</span><span class="sxs-lookup"><span data-stu-id="86d20-135">Otherwise, **false** is returned.</span></span>    

    ```csharp
    ViewBag.Success = queue.CreateIfNotExists();
    ```

1. <span data-ttu-id="86d20-136">Uppdatering av **ViewBag** med namnet på kön.</span><span class="sxs-lookup"><span data-stu-id="86d20-136">Update the **ViewBag** with the name of the queue.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```

1. <span data-ttu-id="86d20-137">I den **Solution Explorer**, expandera den **vyer** mappen, högerklicka på **köer**, och på snabbmenyn väljer **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="86d20-137">In the **Solution Explorer**, expand the **Views** folder, right-click **Queues**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="86d20-138">På den **Lägg till vy** dialogrutan Ange **CreateQueue** för namn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="86d20-138">On the **Add View** dialog, enter **CreateQueue** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="86d20-139">Öppna `CreateQueue.cshtml`, och ändra den så att det ser ut som följande kodavsnitt:</span><span class="sxs-lookup"><span data-stu-id="86d20-139">Open `CreateQueue.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Create Queue";
    }
    
    <h2>Create Queue results</h2>

    Creation of @ViewBag.QueueName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. <span data-ttu-id="86d20-140">I den **Solution Explorer**, expandera den **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="86d20-140">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="86d20-141">Efter senast **Html.ActionLink**, Lägg till följande **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="86d20-141">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Create queue", "CreateQueue", "Queues")</li>
    ```

1. <span data-ttu-id="86d20-142">Kör programmet och välj **Skapa kö** att se resultatet liknar följande Skärmdump:</span><span class="sxs-lookup"><span data-stu-id="86d20-142">Run the application, and select **Create queue** to see results similar to the following screen shot:</span></span>
  
    ![Skapa kö](./media/vs-storage-aspnet-getting-started-queues/create-queue-results.png)

    <span data-ttu-id="86d20-144">Som nämnts tidigare i **CloudQueue.CreateIfNotExists** metoden returnerar **SANT** endast när kön finns inte och har skapats.</span><span class="sxs-lookup"><span data-stu-id="86d20-144">As mentioned previously, the **CloudQueue.CreateIfNotExists** method returns **true** only when the queue doesn't exist and is created.</span></span> <span data-ttu-id="86d20-145">Om du kör appen när kön finns, metoden returnerar därför **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="86d20-145">Therefore, if you run the app when the queue exists, the method returns **false**.</span></span> <span data-ttu-id="86d20-146">Om du vill köra appen flera gånger, måste du ta bort kön innan du kör appen igen.</span><span class="sxs-lookup"><span data-stu-id="86d20-146">To run the app multiple times, you must delete the queue before running the app again.</span></span> <span data-ttu-id="86d20-147">Ta bort kön kan göras den **CloudQueue.Delete** metod.</span><span class="sxs-lookup"><span data-stu-id="86d20-147">Deleting the queue can be done via the **CloudQueue.Delete** method.</span></span> <span data-ttu-id="86d20-148">Du kan också ta bort en kö med hjälp av den [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040) eller [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="86d20-148">You can also delete the queue using the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) or the [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>  

## <a name="add-a-message-to-a-queue"></a><span data-ttu-id="86d20-149">Lägga till ett meddelande till en kö</span><span class="sxs-lookup"><span data-stu-id="86d20-149">Add a message to a queue</span></span>

<span data-ttu-id="86d20-150">När du har [skapade en kö](#create-a-queue), du kan lägga till meddelanden till kön.</span><span class="sxs-lookup"><span data-stu-id="86d20-150">Once you've [created a queue](#create-a-queue), you can add messages to that queue.</span></span> <span data-ttu-id="86d20-151">Det här avsnittet vägleder dig genom att lägga till ett meddelande till en kö *test-kön*.</span><span class="sxs-lookup"><span data-stu-id="86d20-151">This section walks you through adding a message to a queue *test-queue*.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="86d20-152">Det här avsnittet förutsätter att du har slutfört stegen [Konfigurera utvecklingsmiljön](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="86d20-152">This section assumes you have completed the steps [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="86d20-153">Öppna filen `QueuesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="86d20-153">Open the `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="86d20-154">Lägg till en metod som kallas **AddMessage** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="86d20-154">Add a method called **AddMessage** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult AddMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="86d20-155">I den **AddMessage** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="86d20-155">Within the **AddMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="86d20-156">Använda följande kod för att hämta anslutningssträngen för lagring och information om lagringskonto från Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* till namnet på Azure storage-konto du försöker komma åt.)</span><span class="sxs-lookup"><span data-stu-id="86d20-156">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="86d20-157">Hämta en **CloudQueueClient** -objektet representerar en queue-tjänstklienten.</span><span class="sxs-lookup"><span data-stu-id="86d20-157">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="86d20-158">Hämta en **CloudQueueContainer** objekt som representerar en referens till kön.</span><span class="sxs-lookup"><span data-stu-id="86d20-158">Get a **CloudQueueContainer** object that represents a reference to the queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="86d20-159">Skapa den **CloudQueueMessage** objekt som representerar det meddelande som du vill lägga till i kön.</span><span class="sxs-lookup"><span data-stu-id="86d20-159">Create the **CloudQueueMessage** object representing the message you want to add to the queue.</span></span> <span data-ttu-id="86d20-160">En **CloudQueueMessage** objekt kan skapas från en sträng (i UTF-8-format) eller en byte-matris.</span><span class="sxs-lookup"><span data-stu-id="86d20-160">A **CloudQueueMessage** object can be created from either a string (in UTF-8 format) or a byte array.</span></span>

    ```csharp
    CloudQueueMessage message = new CloudQueueMessage("Hello, Azure Queue Storage");
    ```

1. <span data-ttu-id="86d20-161">Anropa den **CloudQueue.AddMessage** metod för att lägga till den messaged i kön.</span><span class="sxs-lookup"><span data-stu-id="86d20-161">Call the **CloudQueue.AddMessage** method to add the messaged to the queue.</span></span>

    ```csharp
    queue.AddMessage(message);
    ```

1. <span data-ttu-id="86d20-162">Skapa och ange några **ViewBag** egenskaper som ska visas i vyn.</span><span class="sxs-lookup"><span data-stu-id="86d20-162">Create and set a couple of **ViewBag** properties for display in the view.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```

1. <span data-ttu-id="86d20-163">I den **Solution Explorer**, expandera den **vyer** mappen, högerklicka på **köer**, och på snabbmenyn väljer **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="86d20-163">In the **Solution Explorer**, expand the **Views** folder, right-click **Queues**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="86d20-164">På den **Lägg till vy** dialogrutan Ange **AddMessage** för namn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="86d20-164">On the **Add View** dialog, enter **AddMessage** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="86d20-165">Öppna `AddMessage.cshtml`, och ändra den så att det ser ut som följande kodavsnitt:</span><span class="sxs-lookup"><span data-stu-id="86d20-165">Open `AddMessage.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Add Message";
    }
    
    <h2>Add Message results</h2>
    
    The message '@ViewBag.Message' was added to the queue '@ViewBag.QueueName'.
    ```

1. <span data-ttu-id="86d20-166">I den **Solution Explorer**, expandera den **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="86d20-166">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="86d20-167">Efter senast **Html.ActionLink**, Lägg till följande **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="86d20-167">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Add message", "AddMessage", "Queues")</li>
    ```

1. <span data-ttu-id="86d20-168">Kör programmet och välj **Lägg till meddelandet** att se resultatet liknar följande Skärmdump:</span><span class="sxs-lookup"><span data-stu-id="86d20-168">Run the application, and select **Add message** to see results similar to the following screen shot:</span></span>
  
    ![Lägga till meddelande](./media/vs-storage-aspnet-getting-started-queues/add-message-results.png)

<span data-ttu-id="86d20-170">Två avsnitt - [läsa ett meddelande från en kö utan att ta bort den](#read-a-message-from-a-queue-without-removing-it) och [Läs- och ta bort ett meddelande från en kö](#read-and-remove-a-message-from-a-queue) -illustrerar hur du kan läsa meddelanden från en kö.</span><span class="sxs-lookup"><span data-stu-id="86d20-170">The two sections - [Read a message from a queue without removing it](#read-a-message-from-a-queue-without-removing-it) and [Read and remove a message from a queue](#read-and-remove-a-message-from-a-queue) - illustrate how to read messages from a queue.</span></span>    

## <a name="read-a-message-from-a-queue-without-removing-it"></a><span data-ttu-id="86d20-171">Läsa ett meddelande från en kö utan att ta bort den</span><span class="sxs-lookup"><span data-stu-id="86d20-171">Read a message from a queue without removing it</span></span>

<span data-ttu-id="86d20-172">Det här avsnittet visar hur du kan kika på meddelandet i kön (läsa det första meddelandet inte ta bort).</span><span class="sxs-lookup"><span data-stu-id="86d20-172">This section illustrates how to peek at a queued message (read the first message without removing it).</span></span>  

> [!NOTE]
> 
> <span data-ttu-id="86d20-173">Det här avsnittet förutsätter att du har slutfört stegen [Konfigurera utvecklingsmiljön](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="86d20-173">This section assumes you have completed the steps [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="86d20-174">Öppna filen `QueuesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="86d20-174">Open the `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="86d20-175">Lägg till en metod som kallas **PeekMessage** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="86d20-175">Add a method called **PeekMessage** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult PeekMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="86d20-176">I den **PeekMessage** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="86d20-176">Within the **PeekMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="86d20-177">Använda följande kod för att hämta anslutningssträngen för lagring och information om lagringskonto från Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* till namnet på Azure storage-konto du försöker komma åt.)</span><span class="sxs-lookup"><span data-stu-id="86d20-177">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="86d20-178">Hämta en **CloudQueueClient** -objektet representerar en queue-tjänstklienten.</span><span class="sxs-lookup"><span data-stu-id="86d20-178">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="86d20-179">Hämta en **CloudQueueContainer** objekt som representerar en referens till kön.</span><span class="sxs-lookup"><span data-stu-id="86d20-179">Get a **CloudQueueContainer** object that represents a reference to the queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="86d20-180">Anropa den **CloudQueue.PeekMessage** metod för att läsa det första meddelandet i kön utan att ta bort den från kön.</span><span class="sxs-lookup"><span data-stu-id="86d20-180">Call the **CloudQueue.PeekMessage** method to read the first message in the queue without removing it from the queue.</span></span> 

    ```csharp
    CloudQueueMessage message = queue.PeekMessage();
    ```

1. <span data-ttu-id="86d20-181">Uppdatering av **ViewBag** med två värden: könamnet och meddelandet som lästes.</span><span class="sxs-lookup"><span data-stu-id="86d20-181">Update the **ViewBag** with two values: the queue name and the message that was read.</span></span> <span data-ttu-id="86d20-182">Den **CloudQueueMessage** objektet innehåller två egenskaper för att hämta objektets värde: **CloudQueueMessage.AsBytes** och **CloudQueueMessage.AsString**.</span><span class="sxs-lookup"><span data-stu-id="86d20-182">The **CloudQueueMessage** object exposes two properties for getting the object's value: **CloudQueueMessage.AsBytes** and **CloudQueueMessage.AsString**.</span></span> <span data-ttu-id="86d20-183">**AsString** (används i det här exemplet) returnerar en sträng, medan **AsBytes** returnerar en bytematris.</span><span class="sxs-lookup"><span data-stu-id="86d20-183">**AsString** (used in this example) returns a string, while **AsBytes** returns a byte array.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name; 
    ViewBag.Message = (message != null ? message.AsString : "");
    ```

1. <span data-ttu-id="86d20-184">I den **Solution Explorer**, expandera den **vyer** mappen, högerklicka på **köer**, och på snabbmenyn väljer **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="86d20-184">In the **Solution Explorer**, expand the **Views** folder, right-click **Queues**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="86d20-185">På den **Lägg till vy** dialogrutan Ange **PeekMessage** för namn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="86d20-185">On the **Add View** dialog, enter **PeekMessage** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="86d20-186">Öppna `PeekMessage.cshtml`, och ändra den så att det ser ut som följande kodavsnitt:</span><span class="sxs-lookup"><span data-stu-id="86d20-186">Open `PeekMessage.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "PeekMessage";
    }
    
    <h2>Peek Message results</h2>
    
    <table border="1">
        <tr><th>Queue</th><th>Peeked Message</th></tr>
        <tr><td>@ViewBag.QueueName</td><td>@ViewBag.Message</td></tr>
    </table>    
    ```

1. <span data-ttu-id="86d20-187">I den **Solution Explorer**, expandera den **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="86d20-187">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="86d20-188">Efter senast **Html.ActionLink**, Lägg till följande **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="86d20-188">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Peek message", "PeekMessage", "Queues")</li>
    ```

1. <span data-ttu-id="86d20-189">Kör programmet och välj **titt meddelandet** att se resultatet liknar följande Skärmdump:</span><span class="sxs-lookup"><span data-stu-id="86d20-189">Run the application, and select **Peek message** to see results similar to the following screen shot:</span></span>
  
    ![Granska meddelande](./media/vs-storage-aspnet-getting-started-queues/peek-message-results.png)

## <a name="read-and-remove-a-message-from-a-queue"></a><span data-ttu-id="86d20-191">Läsa och ta bort ett meddelande från en kö</span><span class="sxs-lookup"><span data-stu-id="86d20-191">Read and remove a message from a queue</span></span>

<span data-ttu-id="86d20-192">I det här avsnittet lär du dig läsa och ta bort ett meddelande från en kö.</span><span class="sxs-lookup"><span data-stu-id="86d20-192">In this section, you learn how to read and remove a message from a queue.</span></span>   

> [!NOTE]
> 
> <span data-ttu-id="86d20-193">Det här avsnittet förutsätter att du har slutfört stegen [Konfigurera utvecklingsmiljön](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="86d20-193">This section assumes you have completed the steps [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="86d20-194">Öppna filen `QueuesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="86d20-194">Open the `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="86d20-195">Lägg till en metod som kallas **ReadMessage** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="86d20-195">Add a method called **ReadMessage** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult ReadMessage()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="86d20-196">I den **ReadMessage** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="86d20-196">Within the **ReadMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="86d20-197">Använda följande kod för att hämta anslutningssträngen för lagring och information om lagringskonto från Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* till namnet på Azure storage-konto du försöker komma åt.)</span><span class="sxs-lookup"><span data-stu-id="86d20-197">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="86d20-198">Hämta en **CloudQueueClient** -objektet representerar en queue-tjänstklienten.</span><span class="sxs-lookup"><span data-stu-id="86d20-198">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="86d20-199">Hämta en **CloudQueueContainer** objekt som representerar en referens till kön.</span><span class="sxs-lookup"><span data-stu-id="86d20-199">Get a **CloudQueueContainer** object that represents a reference to the queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="86d20-200">Anropa den **CloudQueue.GetMessage** metod för att läsa det första meddelandet i kön.</span><span class="sxs-lookup"><span data-stu-id="86d20-200">Call the **CloudQueue.GetMessage** method to read the first message in the queue.</span></span> <span data-ttu-id="86d20-201">Den **CloudQueue.GetMessage** metoden gör meddelandet osynligt i 30 sekunder (som standard) till andra meddelanden som läser så att ingen annan kod kan ändra eller ta bort meddelandet när din bearbeta denna kod.</span><span class="sxs-lookup"><span data-stu-id="86d20-201">The **CloudQueue.GetMessage** method makes the message invisible for 30 seconds (by default) to any other code reading messages so that no other code can modify or delete the message while your processing it.</span></span> <span data-ttu-id="86d20-202">Om du vill ändra hur lång tid som meddelandet är osynliga ändra den **visibilityTimeout** parameter som skickas till den **CloudQueue.GetMessage** metod.</span><span class="sxs-lookup"><span data-stu-id="86d20-202">To change the amount of time the message is invisible, modify the **visibilityTimeout** parameter being passed to the **CloudQueue.GetMessage** method.</span></span>

    ```csharp
    // This message will be invisible to other code for 30 seconds.
    CloudQueueMessage message = queue.GetMessage();     
    ```

1. <span data-ttu-id="86d20-203">Anropa den **CloudQueueMessage.Delete** metod för att ta bort meddelandet från kön.</span><span class="sxs-lookup"><span data-stu-id="86d20-203">Call the **CloudQueueMessage.Delete** method to delete the message from the queue.</span></span>

    ```csharp
    queue.DeleteMessage(message);
    ```

1. <span data-ttu-id="86d20-204">Uppdatering av **ViewBag** med meddelandet tas bort och namnet på kön.</span><span class="sxs-lookup"><span data-stu-id="86d20-204">Update the **ViewBag** with the message deleted, and the name of the queue.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```
 
1. <span data-ttu-id="86d20-205">I den **Solution Explorer**, expandera den **vyer** mappen, högerklicka på **köer**, och på snabbmenyn väljer **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="86d20-205">In the **Solution Explorer**, expand the **Views** folder, right-click **Queues**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="86d20-206">På den **Lägg till vy** dialogrutan Ange **ReadMessage** för namn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="86d20-206">On the **Add View** dialog, enter **ReadMessage** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="86d20-207">Öppna `ReadMessage.cshtml`, och ändra den så att det ser ut som följande kodavsnitt:</span><span class="sxs-lookup"><span data-stu-id="86d20-207">Open `ReadMessage.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "ReadMessage";
    }
    
    <h2>Read Message results</h2>
    
    <table border="1">
        <tr><th>Queue</th><th>Read (and Deleted) Message</th></tr>
        <tr><td>@ViewBag.QueueName</td><td>@ViewBag.Message</td></tr>
    </table>
    ```

1. <span data-ttu-id="86d20-208">I den **Solution Explorer**, expandera den **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="86d20-208">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="86d20-209">Efter senast **Html.ActionLink**, Lägg till följande **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="86d20-209">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Read/Delete message", "ReadMessage", "Queues")</li>
    ```

1. <span data-ttu-id="86d20-210">Kör programmet och välj **läsa/ta bort meddelandet** att se resultatet liknar följande Skärmdump:</span><span class="sxs-lookup"><span data-stu-id="86d20-210">Run the application, and select **Read/Delete message** to see results similar to the following screen shot:</span></span>
  
    ![Läsa och ta bort meddelandet](./media/vs-storage-aspnet-getting-started-queues/read-message-results.png)

## <a name="get-the-queue-length"></a><span data-ttu-id="86d20-212">Hämta kölängden</span><span class="sxs-lookup"><span data-stu-id="86d20-212">Get the queue length</span></span>

<span data-ttu-id="86d20-213">Det här avsnittet visar hur du kan hämta kölängden (antal meddelanden).</span><span class="sxs-lookup"><span data-stu-id="86d20-213">This section illustrates how to get the queue length (number of messages).</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="86d20-214">Det här avsnittet förutsätter att du har slutfört stegen [Konfigurera utvecklingsmiljön](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="86d20-214">This section assumes you have completed the steps [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="86d20-215">Öppna filen `QueuesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="86d20-215">Open the `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="86d20-216">Lägg till en metod som kallas **GetQueueLength** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="86d20-216">Add a method called **GetQueueLength** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult GetQueueLength()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="86d20-217">I den **ReadMessage** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="86d20-217">Within the **ReadMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="86d20-218">Använda följande kod för att hämta anslutningssträngen för lagring och information om lagringskonto från Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* till namnet på Azure storage-konto du försöker komma åt.)</span><span class="sxs-lookup"><span data-stu-id="86d20-218">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="86d20-219">Hämta en **CloudQueueClient** -objektet representerar en queue-tjänstklienten.</span><span class="sxs-lookup"><span data-stu-id="86d20-219">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="86d20-220">Hämta en **CloudQueueContainer** objekt som representerar en referens till kön.</span><span class="sxs-lookup"><span data-stu-id="86d20-220">Get a **CloudQueueContainer** object that represents a reference to the queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="86d20-221">Anropa den **CloudQueue.FetchAttributes** metod för att hämta köattributen (inklusive dess längd).</span><span class="sxs-lookup"><span data-stu-id="86d20-221">Call the **CloudQueue.FetchAttributes** method to retrieve the queue's attributes (including its length).</span></span> 

    ```csharp
    queue.FetchAttributes();
    ```

6. <span data-ttu-id="86d20-222">Åtkomst till den **CloudQueue.ApproximateMessageCount** egenskapen för att hämta köns längd.</span><span class="sxs-lookup"><span data-stu-id="86d20-222">Access the **CloudQueue.ApproximateMessageCount** property to get the queue's length.</span></span>
 
    ```csharp
    int? nMessages = queue.ApproximateMessageCount;
    ```

1. <span data-ttu-id="86d20-223">Uppdatering av **ViewBag** med namnet på kön och dess längd.</span><span class="sxs-lookup"><span data-stu-id="86d20-223">Update the **ViewBag** with the name of the queue, and its length.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Length = nMessages;
    ```
 
1. <span data-ttu-id="86d20-224">I den **Solution Explorer**, expandera den **vyer** mappen, högerklicka på **köer**, och på snabbmenyn väljer **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="86d20-224">In the **Solution Explorer**, expand the **Views** folder, right-click **Queues**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="86d20-225">På den **Lägg till vy** dialogrutan Ange **GetQueueLength** för namn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="86d20-225">On the **Add View** dialog, enter **GetQueueLength** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="86d20-226">Öppna `GetQueueLengthMessage.cshtml`, och ändra den så att det ser ut som följande kodavsnitt:</span><span class="sxs-lookup"><span data-stu-id="86d20-226">Open `GetQueueLengthMessage.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "GetQueueLength";
    }
    
    <h2>Get Queue Length results</h2>
    
    The queue '@ViewBag.QueueName' has a length of (number of messages): @ViewBag.Length
    ```

1. <span data-ttu-id="86d20-227">I den **Solution Explorer**, expandera den **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="86d20-227">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="86d20-228">Efter senast **Html.ActionLink**, Lägg till följande **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="86d20-228">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Get queue length", "GetQueueLength", "Queues")</li>
    ```

1. <span data-ttu-id="86d20-229">Kör programmet och välj **hämta kölängden** att se resultatet liknar följande Skärmdump:</span><span class="sxs-lookup"><span data-stu-id="86d20-229">Run the application, and select **Get queue length** to see results similar to the following screen shot:</span></span>
  
    ![Hämta kölängden](./media/vs-storage-aspnet-getting-started-queues/get-queue-length-results.png)


## <a name="delete-a-queue"></a><span data-ttu-id="86d20-231">Ta bort en kö</span><span class="sxs-lookup"><span data-stu-id="86d20-231">Delete a queue</span></span>
<span data-ttu-id="86d20-232">Det här avsnittet visas hur du tar bort en kö.</span><span class="sxs-lookup"><span data-stu-id="86d20-232">This section illustrates how to delete a queue.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="86d20-233">Det här avsnittet förutsätter att du har slutfört stegen [Konfigurera utvecklingsmiljön](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="86d20-233">This section assumes you have completed the steps [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="86d20-234">Öppna filen `QueuesController.cs`.</span><span class="sxs-lookup"><span data-stu-id="86d20-234">Open the `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="86d20-235">Lägg till en metod som kallas **DeleteQueue** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="86d20-235">Add a method called **DeleteQueue** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult DeleteQueue()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="86d20-236">I den **DeleteQueue** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="86d20-236">Within the **DeleteQueue** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="86d20-237">Använda följande kod för att hämta anslutningssträngen för lagring och information om lagringskonto från Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* till namnet på Azure storage-konto du försöker komma åt.)</span><span class="sxs-lookup"><span data-stu-id="86d20-237">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="86d20-238">Hämta en **CloudQueueClient** -objektet representerar en queue-tjänstklienten.</span><span class="sxs-lookup"><span data-stu-id="86d20-238">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="86d20-239">Hämta en **CloudQueueContainer** objekt som representerar en referens till kön.</span><span class="sxs-lookup"><span data-stu-id="86d20-239">Get a **CloudQueueContainer** object that represents a reference to the queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="86d20-240">Anropa den **CloudQueue.Delete** metod för att ta bort den kö som representeras av den **CloudQueue** objekt.</span><span class="sxs-lookup"><span data-stu-id="86d20-240">Call the **CloudQueue.Delete** method to delete the queue represented by the **CloudQueue** object.</span></span>

    ```csharp
    queue.Delete();
    ```

1. <span data-ttu-id="86d20-241">Uppdatering av **ViewBag** med namnet på kön och dess längd.</span><span class="sxs-lookup"><span data-stu-id="86d20-241">Update the **ViewBag** with the name of the queue, and its length.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```
 
1. <span data-ttu-id="86d20-242">I den **Solution Explorer**, expandera den **vyer** mappen, högerklicka på **köer**, och på snabbmenyn väljer **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="86d20-242">In the **Solution Explorer**, expand the **Views** folder, right-click **Queues**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="86d20-243">På den **Lägg till vy** dialogrutan Ange **DeleteQueue** för namn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="86d20-243">On the **Add View** dialog, enter **DeleteQueue** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="86d20-244">Öppna `DeleteQueue.cshtml`, och ändra den så att det ser ut som följande kodavsnitt:</span><span class="sxs-lookup"><span data-stu-id="86d20-244">Open `DeleteQueue.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "DeleteQueue";
    }
    
    <h2>Delete Queue results</h2>
    
    @ViewBag.QueueName deleted.
    ```

1. <span data-ttu-id="86d20-245">I den **Solution Explorer**, expandera den **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="86d20-245">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="86d20-246">Efter senast **Html.ActionLink**, Lägg till följande **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="86d20-246">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Delete queue", "DeleteQueue", "Queues")</li>
    ```

1. <span data-ttu-id="86d20-247">Kör programmet och välj **hämta kölängden** att se resultatet liknar följande Skärmdump:</span><span class="sxs-lookup"><span data-stu-id="86d20-247">Run the application, and select **Get queue length** to see results similar to the following screen shot:</span></span>
  
    ![Ta bort kön](./media/vs-storage-aspnet-getting-started-queues/delete-queue-results.png)

## <a name="next-steps"></a><span data-ttu-id="86d20-249">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="86d20-249">Next steps</span></span>
<span data-ttu-id="86d20-250">Visa fler funktionsguider och lär dig mer om andra alternativ för att lagra data i Azure.</span><span class="sxs-lookup"><span data-stu-id="86d20-250">View more feature guides to learn about additional options for storing data in Azure.</span></span>

  * [<span data-ttu-id="86d20-251">Kom igång med Azure blob storage och Visual Studio anslutna tjänster (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="86d20-251">Get started with Azure blob storage and Visual Studio Connected Services (ASP.NET)</span></span>](../storage/vs-storage-aspnet-getting-started-blobs.md)
  * [<span data-ttu-id="86d20-252">Kom igång med Azure-tabellagring och Visual Studio anslutna tjänster (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="86d20-252">Get started with Azure table storage and Visual Studio Connected Services (ASP.NET)</span></span>](vs-storage-aspnet-getting-started-tables.md)
