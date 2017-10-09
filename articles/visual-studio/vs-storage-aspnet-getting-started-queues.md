---
title: "aaaGet igång med Azure queue storage- och Visual Studio anslutna tjänster (ASP.NET) | Microsoft Docs"
description: "Hur tooget igång med Azure queue storage i ASP.NET-projekt i Visual Studio när du har anslutit tooa lagringskonto med hjälp av Visual Studio anslutna Services"
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
ms.openlocfilehash: 415a437c4ce60b1e2e328f8e937c73b0d5c50e78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services-aspnet"></a><span data-ttu-id="d2d64-103">Kom igång med Azure queue storage- och Visual Studio anslutna tjänster (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="d2d64-103">Get started with Azure queue storage and Visual Studio Connected Services (ASP.NET)</span></span>
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="d2d64-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="d2d64-104">Overview</span></span>

<span data-ttu-id="d2d64-105">Azure queue storage erbjuder molntjänster meddelandehantering mellan programkomponenter.</span><span class="sxs-lookup"><span data-stu-id="d2d64-105">Azure queue storage provides cloud messaging between application components.</span></span> <span data-ttu-id="d2d64-106">När program utformas för skalning är programkomponenterna ofta fristående, så att de kan skalas oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="d2d64-106">In designing applications for scale, application components are often decoupled, so that they can scale independently.</span></span> <span data-ttu-id="d2d64-107">Queue storage ger asynkrona meddelanden för kommunikation mellan programkomponenter, oavsett om de körs i hello molnet på hello skrivbordet, på en lokal server eller på en mobil enhet.</span><span class="sxs-lookup"><span data-stu-id="d2d64-107">Queue storage delivers asynchronous messaging for communication between application components, whether they are running in hello cloud, on hello desktop, on an on-premises server, or on a mobile device.</span></span> <span data-ttu-id="d2d64-108">Queue Storage har också stöd för hantering av asynkrona åtgärder och utveckling av processarbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="d2d64-108">Queue storage also supports managing asynchronous tasks and building process work flows.</span></span>

<span data-ttu-id="d2d64-109">Den här kursen visar hur toowrite ASP.NET kod för några vanliga scenarier med hjälp av Azure queue storage entiteter.</span><span class="sxs-lookup"><span data-stu-id="d2d64-109">This tutorial shows how toowrite ASP.NET code for some common scenarios using Azure queue storage entities.</span></span> <span data-ttu-id="d2d64-110">Dessa scenarier som inkluderar vanliga aktiviteter som att skapa en Azure-kö och lägga till, ändra, läsa och tar bort Kömeddelanden.</span><span class="sxs-lookup"><span data-stu-id="d2d64-110">These scenarios include common tasks such as creating an Azure queue, and adding, modifying, reading, and removing queue messages.</span></span>

##<a name="prerequisites"></a><span data-ttu-id="d2d64-111">Krav</span><span class="sxs-lookup"><span data-stu-id="d2d64-111">Prerequisites</span></span>

* [<span data-ttu-id="d2d64-112">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d2d64-112">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="d2d64-113">Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="d2d64-113">Azure storage account</span></span>](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a><span data-ttu-id="d2d64-114">Skapa en MVC-enhet</span><span class="sxs-lookup"><span data-stu-id="d2d64-114">Create an MVC controller</span></span> 

1. <span data-ttu-id="d2d64-115">I hello **Solution Explorer**, högerklicka på **domänkontrollanter**, hello snabbmenyn, Välj **Lägg till -> Controller**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-115">In hello **Solution Explorer**, right-click **Controllers**, and, from hello context menu, select **Add->Controller**.</span></span>

    ![Lägg till en domänkontrollant tooan ASP.NET MVC-app](./media/vs-storage-aspnet-getting-started-queues/add-controller-menu.png)

1. <span data-ttu-id="d2d64-117">På hello **Lägg till Kodskelett** markerar **MVC 5 styrenhet – tom**, och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-117">On hello **Add Scaffold** dialog, select **MVC 5 Controller - Empty**, and select **Add**.</span></span>

    ![Ange typ av MVC-domänkontrollant](./media/vs-storage-aspnet-getting-started-queues/add-controller.png)

1. <span data-ttu-id="d2d64-119">På hello **Lägg till styrenhet** dialogrutan, namnet hello controller *QueuesController*, och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-119">On hello **Add Controller** dialog, name hello controller *QueuesController*, and select **Add**.</span></span>

    ![Namnet hello MVC-enhet](./media/vs-storage-aspnet-getting-started-queues/add-controller-name.png)

1. <span data-ttu-id="d2d64-121">Lägg till följande hello *med* direktiven toohello `QueuesController.cs` fil:</span><span class="sxs-lookup"><span data-stu-id="d2d64-121">Add hello following *using* directives toohello `QueuesController.cs` file:</span></span>

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    ```
## <a name="create-a-queue"></a><span data-ttu-id="d2d64-122">Skapa en kö</span><span class="sxs-lookup"><span data-stu-id="d2d64-122">Create a queue</span></span>

<span data-ttu-id="d2d64-123">hello följande steg visar hur toocreate en kö:</span><span class="sxs-lookup"><span data-stu-id="d2d64-123">hello following steps illustrate how toocreate a queue:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="d2d64-124">Det här avsnittet förutsätter att du har slutfört steg hello [ställa in hello utvecklingsmiljö](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="d2d64-124">This section assumes you have completed hello steps [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="d2d64-125">Öppna hello `QueuesController.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="d2d64-125">Open hello `QueuesController.cs` file.</span></span> 

1. <span data-ttu-id="d2d64-126">Lägg till en metod som kallas **CreateQueue** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-126">Add a method called **CreateQueue** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult CreateQueue()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="d2d64-127">Inom hello **CreateQueue** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="d2d64-127">Within hello **CreateQueue** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="d2d64-128">Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)</span><span class="sxs-lookup"><span data-stu-id="d2d64-128">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="d2d64-129">Hämta en **CloudQueueClient** -objektet representerar en queue-tjänstklienten.</span><span class="sxs-lookup"><span data-stu-id="d2d64-129">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```
1. <span data-ttu-id="d2d64-130">Hämta en **CloudQueue** objekt som representerar en referensnamn toohello önskad kö.</span><span class="sxs-lookup"><span data-stu-id="d2d64-130">Get a **CloudQueue** object that represents a reference toohello desired queue name.</span></span> <span data-ttu-id="d2d64-131">Hej **CloudQueueClient.GetQueueReference** inte gör en begäran mot queue storage.</span><span class="sxs-lookup"><span data-stu-id="d2d64-131">hello **CloudQueueClient.GetQueueReference** method does not make a request against queue storage.</span></span> <span data-ttu-id="d2d64-132">hello referens returneras hello kön finns eller inte.</span><span class="sxs-lookup"><span data-stu-id="d2d64-132">hello reference is returned whether or not hello queue exists.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="d2d64-133">Anropa hello **CloudQueue.CreateIfNotExists** metoden toocreate hello kö om den inte finns ännu.</span><span class="sxs-lookup"><span data-stu-id="d2d64-133">Call hello **CloudQueue.CreateIfNotExists** method toocreate hello queue if it does not yet exist.</span></span> <span data-ttu-id="d2d64-134">Hej **CloudQueue.CreateIfNotExists** metoden returnerar **SANT** om hello kön finns inte och har skapats.</span><span class="sxs-lookup"><span data-stu-id="d2d64-134">hello **CloudQueue.CreateIfNotExists** method returns **true** if hello queue does not exist, and is successfully created.</span></span> <span data-ttu-id="d2d64-135">Annars **FALSKT** returneras.</span><span class="sxs-lookup"><span data-stu-id="d2d64-135">Otherwise, **false** is returned.</span></span>    

    ```csharp
    ViewBag.Success = queue.CreateIfNotExists();
    ```

1. <span data-ttu-id="d2d64-136">Uppdatera hello **ViewBag** med hello namnet på hello kö.</span><span class="sxs-lookup"><span data-stu-id="d2d64-136">Update hello **ViewBag** with hello name of hello queue.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```

1. <span data-ttu-id="d2d64-137">I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **köer**, och hello snabbmenyn, Välj **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-137">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Queues**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="d2d64-138">På hello **Lägg till vy** dialogrutan Ange **CreateQueue** hello vynamn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-138">On hello **Add View** dialog, enter **CreateQueue** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="d2d64-139">Öppna `CreateQueue.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="d2d64-139">Open `CreateQueue.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Create Queue";
    }
    
    <h2>Create Queue results</h2>

    Creation of @ViewBag.QueueName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. <span data-ttu-id="d2d64-140">I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="d2d64-140">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="d2d64-141">Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="d2d64-141">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Create queue", "CreateQueue", "Queues")</li>
    ```

1. <span data-ttu-id="d2d64-142">Kör hello programmet och välj **Skapa kö** toosee resulterar liknande toohello följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="d2d64-142">Run hello application, and select **Create queue** toosee results similar toohello following screen shot:</span></span>
  
    ![Skapa kö](./media/vs-storage-aspnet-getting-started-queues/create-queue-results.png)

    <span data-ttu-id="d2d64-144">Som tidigare nämnts hello **CloudQueue.CreateIfNotExists** metoden returnerar **SANT** endast när hello kön finns inte och har skapats.</span><span class="sxs-lookup"><span data-stu-id="d2d64-144">As mentioned previously, hello **CloudQueue.CreateIfNotExists** method returns **true** only when hello queue doesn't exist and is created.</span></span> <span data-ttu-id="d2d64-145">Om du kör hello app när hello kön finns, hello-metoden returnerar därför **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-145">Therefore, if you run hello app when hello queue exists, hello method returns **false**.</span></span> <span data-ttu-id="d2d64-146">toorun hello app flera gånger, måste du ta bort hello kön innan du kör hello appen igen.</span><span class="sxs-lookup"><span data-stu-id="d2d64-146">toorun hello app multiple times, you must delete hello queue before running hello app again.</span></span> <span data-ttu-id="d2d64-147">Ta bort hello kön kan göras via hello **CloudQueue.Delete** metod.</span><span class="sxs-lookup"><span data-stu-id="d2d64-147">Deleting hello queue can be done via hello **CloudQueue.Delete** method.</span></span> <span data-ttu-id="d2d64-148">Du kan också ta bort hello kön med hello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040) eller hello [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="d2d64-148">You can also delete hello queue using hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) or hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>  

## <a name="add-a-message-tooa-queue"></a><span data-ttu-id="d2d64-149">Lägg till en meddelandekö tooa</span><span class="sxs-lookup"><span data-stu-id="d2d64-149">Add a message tooa queue</span></span>

<span data-ttu-id="d2d64-150">När du har [skapade en kö](#create-a-queue), kan du lägga till meddelanden toothat kön.</span><span class="sxs-lookup"><span data-stu-id="d2d64-150">Once you've [created a queue](#create-a-queue), you can add messages toothat queue.</span></span> <span data-ttu-id="d2d64-151">Det här avsnittet vägleder dig genom att lägga till en meddelandekö tooa *test-kön*.</span><span class="sxs-lookup"><span data-stu-id="d2d64-151">This section walks you through adding a message tooa queue *test-queue*.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="d2d64-152">Det här avsnittet förutsätter att du har slutfört steg hello [ställa in hello utvecklingsmiljö](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="d2d64-152">This section assumes you have completed hello steps [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="d2d64-153">Öppna hello `QueuesController.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="d2d64-153">Open hello `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="d2d64-154">Lägg till en metod som kallas **AddMessage** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-154">Add a method called **AddMessage** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult AddMessage()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="d2d64-155">Inom hello **AddMessage** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="d2d64-155">Within hello **AddMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="d2d64-156">Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)</span><span class="sxs-lookup"><span data-stu-id="d2d64-156">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="d2d64-157">Hämta en **CloudQueueClient** -objektet representerar en queue-tjänstklienten.</span><span class="sxs-lookup"><span data-stu-id="d2d64-157">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="d2d64-158">Hämta en **CloudQueueContainer** objekt som representerar en referens toohello kö.</span><span class="sxs-lookup"><span data-stu-id="d2d64-158">Get a **CloudQueueContainer** object that represents a reference toohello queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="d2d64-159">Skapa hello **CloudQueueMessage** objekt som representerar hello-meddelande som du vill tooadd toohello kön.</span><span class="sxs-lookup"><span data-stu-id="d2d64-159">Create hello **CloudQueueMessage** object representing hello message you want tooadd toohello queue.</span></span> <span data-ttu-id="d2d64-160">En **CloudQueueMessage** objekt kan skapas från en sträng (i UTF-8-format) eller en byte-matris.</span><span class="sxs-lookup"><span data-stu-id="d2d64-160">A **CloudQueueMessage** object can be created from either a string (in UTF-8 format) or a byte array.</span></span>

    ```csharp
    CloudQueueMessage message = new CloudQueueMessage("Hello, Azure Queue Storage");
    ```

1. <span data-ttu-id="d2d64-161">Anropa hello **CloudQueue.AddMessage** metoden tooadd hello postmeddelandet toohello kön.</span><span class="sxs-lookup"><span data-stu-id="d2d64-161">Call hello **CloudQueue.AddMessage** method tooadd hello messaged toohello queue.</span></span>

    ```csharp
    queue.AddMessage(message);
    ```

1. <span data-ttu-id="d2d64-162">Skapa och ange några **ViewBag** egenskaper för visning i hello vy.</span><span class="sxs-lookup"><span data-stu-id="d2d64-162">Create and set a couple of **ViewBag** properties for display in hello view.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```

1. <span data-ttu-id="d2d64-163">I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **köer**, och hello snabbmenyn, Välj **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-163">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Queues**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="d2d64-164">På hello **Lägg till vy** dialogrutan Ange **AddMessage** hello vynamn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-164">On hello **Add View** dialog, enter **AddMessage** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="d2d64-165">Öppna `AddMessage.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="d2d64-165">Open `AddMessage.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Add Message";
    }
    
    <h2>Add Message results</h2>
    
    hello message '@ViewBag.Message' was added toohello queue '@ViewBag.QueueName'.
    ```

1. <span data-ttu-id="d2d64-166">I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="d2d64-166">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="d2d64-167">Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="d2d64-167">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Add message", "AddMessage", "Queues")</li>
    ```

1. <span data-ttu-id="d2d64-168">Kör hello programmet och välj **Lägg till meddelandet** toosee resulterar liknande toohello följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="d2d64-168">Run hello application, and select **Add message** toosee results similar toohello following screen shot:</span></span>
  
    ![Lägga till meddelande](./media/vs-storage-aspnet-getting-started-queues/add-message-results.png)

<span data-ttu-id="d2d64-170">Hej två avsnitt - [läsa ett meddelande från en kö utan att ta bort den](#read-a-message-from-a-queue-without-removing-it) och [Läs- och ta bort ett meddelande från en kö](#read-and-remove-a-message-from-a-queue) -illustrerar hur tooread meddelanden från en kö.</span><span class="sxs-lookup"><span data-stu-id="d2d64-170">hello two sections - [Read a message from a queue without removing it](#read-a-message-from-a-queue-without-removing-it) and [Read and remove a message from a queue](#read-and-remove-a-message-from-a-queue) - illustrate how tooread messages from a queue.</span></span>  

## <a name="read-a-message-from-a-queue-without-removing-it"></a><span data-ttu-id="d2d64-171">Läsa ett meddelande från en kö utan att ta bort den</span><span class="sxs-lookup"><span data-stu-id="d2d64-171">Read a message from a queue without removing it</span></span>

<span data-ttu-id="d2d64-172">Detta avsnitt visar hur toopeek på ett meddelande i kön (skrivskyddade hello första meddelande utan att ta bort det).</span><span class="sxs-lookup"><span data-stu-id="d2d64-172">This section illustrates how toopeek at a queued message (read hello first message without removing it).</span></span>  

> [!NOTE]
> 
> <span data-ttu-id="d2d64-173">Det här avsnittet förutsätter att du har slutfört steg hello [ställa in hello utvecklingsmiljö](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="d2d64-173">This section assumes you have completed hello steps [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="d2d64-174">Öppna hello `QueuesController.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="d2d64-174">Open hello `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="d2d64-175">Lägg till en metod som kallas **PeekMessage** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-175">Add a method called **PeekMessage** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult PeekMessage()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="d2d64-176">Inom hello **PeekMessage** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="d2d64-176">Within hello **PeekMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="d2d64-177">Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)</span><span class="sxs-lookup"><span data-stu-id="d2d64-177">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="d2d64-178">Hämta en **CloudQueueClient** -objektet representerar en queue-tjänstklienten.</span><span class="sxs-lookup"><span data-stu-id="d2d64-178">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="d2d64-179">Hämta en **CloudQueueContainer** objekt som representerar en referens toohello kö.</span><span class="sxs-lookup"><span data-stu-id="d2d64-179">Get a **CloudQueueContainer** object that represents a reference toohello queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="d2d64-180">Anropa hello **CloudQueue.PeekMessage** metoden tooread hello första meddelandet i hello kön utan att ta bort den från hello kö.</span><span class="sxs-lookup"><span data-stu-id="d2d64-180">Call hello **CloudQueue.PeekMessage** method tooread hello first message in hello queue without removing it from hello queue.</span></span> 

    ```csharp
    CloudQueueMessage message = queue.PeekMessage();
    ```

1. <span data-ttu-id="d2d64-181">Uppdatera hello **ViewBag** med två värden: hello könamnet och hello-meddelande som har lästs.</span><span class="sxs-lookup"><span data-stu-id="d2d64-181">Update hello **ViewBag** with two values: hello queue name and hello message that was read.</span></span> <span data-ttu-id="d2d64-182">Hej **CloudQueueMessage** objektet innehåller två egenskaper för att hämta hello Objektvärde: **CloudQueueMessage.AsBytes** och **CloudQueueMessage.AsString**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-182">hello **CloudQueueMessage** object exposes two properties for getting hello object's value: **CloudQueueMessage.AsBytes** and **CloudQueueMessage.AsString**.</span></span> <span data-ttu-id="d2d64-183">**AsString** (används i det här exemplet) returnerar en sträng, medan **AsBytes** returnerar en bytematris.</span><span class="sxs-lookup"><span data-stu-id="d2d64-183">**AsString** (used in this example) returns a string, while **AsBytes** returns a byte array.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name; 
    ViewBag.Message = (message != null ? message.AsString : "");
    ```

1. <span data-ttu-id="d2d64-184">I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **köer**, och hello snabbmenyn, Välj **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-184">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Queues**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="d2d64-185">På hello **Lägg till vy** dialogrutan Ange **PeekMessage** hello vynamn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-185">On hello **Add View** dialog, enter **PeekMessage** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="d2d64-186">Öppna `PeekMessage.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="d2d64-186">Open `PeekMessage.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

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

1. <span data-ttu-id="d2d64-187">I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="d2d64-187">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="d2d64-188">Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="d2d64-188">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Peek message", "PeekMessage", "Queues")</li>
    ```

1. <span data-ttu-id="d2d64-189">Kör hello programmet och välj **titt meddelandet** toosee resulterar liknande toohello följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="d2d64-189">Run hello application, and select **Peek message** toosee results similar toohello following screen shot:</span></span>
  
    ![Granska meddelande](./media/vs-storage-aspnet-getting-started-queues/peek-message-results.png)

## <a name="read-and-remove-a-message-from-a-queue"></a><span data-ttu-id="d2d64-191">Läsa och ta bort ett meddelande från en kö</span><span class="sxs-lookup"><span data-stu-id="d2d64-191">Read and remove a message from a queue</span></span>

<span data-ttu-id="d2d64-192">I det här avsnittet lär du dig hur tooread och ta bort ett meddelande från en kö.</span><span class="sxs-lookup"><span data-stu-id="d2d64-192">In this section, you learn how tooread and remove a message from a queue.</span></span>   

> [!NOTE]
> 
> <span data-ttu-id="d2d64-193">Det här avsnittet förutsätter att du har slutfört steg hello [ställa in hello utvecklingsmiljö](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="d2d64-193">This section assumes you have completed hello steps [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="d2d64-194">Öppna hello `QueuesController.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="d2d64-194">Open hello `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="d2d64-195">Lägg till en metod som kallas **ReadMessage** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-195">Add a method called **ReadMessage** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult ReadMessage()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="d2d64-196">Inom hello **ReadMessage** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="d2d64-196">Within hello **ReadMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="d2d64-197">Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)</span><span class="sxs-lookup"><span data-stu-id="d2d64-197">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="d2d64-198">Hämta en **CloudQueueClient** -objektet representerar en queue-tjänstklienten.</span><span class="sxs-lookup"><span data-stu-id="d2d64-198">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="d2d64-199">Hämta en **CloudQueueContainer** objekt som representerar en referens toohello kö.</span><span class="sxs-lookup"><span data-stu-id="d2d64-199">Get a **CloudQueueContainer** object that represents a reference toohello queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="d2d64-200">Anropa hello **CloudQueue.GetMessage** metoden tooread hello första meddelandet i kön hello.</span><span class="sxs-lookup"><span data-stu-id="d2d64-200">Call hello **CloudQueue.GetMessage** method tooread hello first message in hello queue.</span></span> <span data-ttu-id="d2d64-201">Hej **CloudQueue.GetMessage** metod kan hello meddelandet osynlig för 30 sekunder (som standard) tooany annan kod läsa meddelanden så att ingen annan kod kan ändra eller ta bort hello-meddelande under din bearbetning av den.</span><span class="sxs-lookup"><span data-stu-id="d2d64-201">hello **CloudQueue.GetMessage** method makes hello message invisible for 30 seconds (by default) tooany other code reading messages so that no other code can modify or delete hello message while your processing it.</span></span> <span data-ttu-id="d2d64-202">toochange hello mängden tid hello-meddelande är osynliga, ändra hello **visibilityTimeout** parameter som skickas toohello **CloudQueue.GetMessage** metod.</span><span class="sxs-lookup"><span data-stu-id="d2d64-202">toochange hello amount of time hello message is invisible, modify hello **visibilityTimeout** parameter being passed toohello **CloudQueue.GetMessage** method.</span></span>

    ```csharp
    // This message will be invisible tooother code for 30 seconds.
    CloudQueueMessage message = queue.GetMessage();     
    ```

1. <span data-ttu-id="d2d64-203">Anropa hello **CloudQueueMessage.Delete** metoden toodelete hello-meddelande från hello kö.</span><span class="sxs-lookup"><span data-stu-id="d2d64-203">Call hello **CloudQueueMessage.Delete** method toodelete hello message from hello queue.</span></span>

    ```csharp
    queue.DeleteMessage(message);
    ```

1. <span data-ttu-id="d2d64-204">Uppdatera hello **ViewBag** med hello meddelandet tas bort och hello namnet på hello kö.</span><span class="sxs-lookup"><span data-stu-id="d2d64-204">Update hello **ViewBag** with hello message deleted, and hello name of hello queue.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```
 
1. <span data-ttu-id="d2d64-205">I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **köer**, och hello snabbmenyn, Välj **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-205">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Queues**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="d2d64-206">På hello **Lägg till vy** dialogrutan Ange **ReadMessage** hello vynamn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-206">On hello **Add View** dialog, enter **ReadMessage** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="d2d64-207">Öppna `ReadMessage.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="d2d64-207">Open `ReadMessage.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

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

1. <span data-ttu-id="d2d64-208">I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="d2d64-208">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="d2d64-209">Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="d2d64-209">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Read/Delete message", "ReadMessage", "Queues")</li>
    ```

1. <span data-ttu-id="d2d64-210">Kör hello programmet och välj **läsa/ta bort meddelandet** toosee resulterar liknande toohello följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="d2d64-210">Run hello application, and select **Read/Delete message** toosee results similar toohello following screen shot:</span></span>
  
    ![Läsa och ta bort meddelandet](./media/vs-storage-aspnet-getting-started-queues/read-message-results.png)

## <a name="get-hello-queue-length"></a><span data-ttu-id="d2d64-212">Hämta hello Kölängd</span><span class="sxs-lookup"><span data-stu-id="d2d64-212">Get hello queue length</span></span>

<span data-ttu-id="d2d64-213">Detta avsnitt visar hur tooget hello Kölängd (antal meddelanden).</span><span class="sxs-lookup"><span data-stu-id="d2d64-213">This section illustrates how tooget hello queue length (number of messages).</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="d2d64-214">Det här avsnittet förutsätter att du har slutfört steg hello [ställa in hello utvecklingsmiljö](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="d2d64-214">This section assumes you have completed hello steps [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="d2d64-215">Öppna hello `QueuesController.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="d2d64-215">Open hello `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="d2d64-216">Lägg till en metod som kallas **GetQueueLength** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-216">Add a method called **GetQueueLength** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult GetQueueLength()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="d2d64-217">Inom hello **ReadMessage** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="d2d64-217">Within hello **ReadMessage** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="d2d64-218">Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)</span><span class="sxs-lookup"><span data-stu-id="d2d64-218">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="d2d64-219">Hämta en **CloudQueueClient** -objektet representerar en queue-tjänstklienten.</span><span class="sxs-lookup"><span data-stu-id="d2d64-219">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="d2d64-220">Hämta en **CloudQueueContainer** objekt som representerar en referens toohello kö.</span><span class="sxs-lookup"><span data-stu-id="d2d64-220">Get a **CloudQueueContainer** object that represents a reference toohello queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="d2d64-221">Anropa hello **CloudQueue.FetchAttributes** metoden tooretrieve hello köns attribut (inklusive dess längd).</span><span class="sxs-lookup"><span data-stu-id="d2d64-221">Call hello **CloudQueue.FetchAttributes** method tooretrieve hello queue's attributes (including its length).</span></span> 

    ```csharp
    queue.FetchAttributes();
    ```

6. <span data-ttu-id="d2d64-222">Åtkomst hello **CloudQueue.ApproximateMessageCount** egenskapen tooget hello köns längd.</span><span class="sxs-lookup"><span data-stu-id="d2d64-222">Access hello **CloudQueue.ApproximateMessageCount** property tooget hello queue's length.</span></span>
 
    ```csharp
    int? nMessages = queue.ApproximateMessageCount;
    ```

1. <span data-ttu-id="d2d64-223">Uppdatera hello **ViewBag** med hello namnet hello kön och dess längd.</span><span class="sxs-lookup"><span data-stu-id="d2d64-223">Update hello **ViewBag** with hello name of hello queue, and its length.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Length = nMessages;
    ```
 
1. <span data-ttu-id="d2d64-224">I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **köer**, och hello snabbmenyn, Välj **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-224">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Queues**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="d2d64-225">På hello **Lägg till vy** dialogrutan Ange **GetQueueLength** hello vynamn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-225">On hello **Add View** dialog, enter **GetQueueLength** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="d2d64-226">Öppna `GetQueueLengthMessage.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="d2d64-226">Open `GetQueueLengthMessage.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "GetQueueLength";
    }
    
    <h2>Get Queue Length results</h2>
    
    hello queue '@ViewBag.QueueName' has a length of (number of messages): @ViewBag.Length
    ```

1. <span data-ttu-id="d2d64-227">I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="d2d64-227">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="d2d64-228">Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="d2d64-228">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Get queue length", "GetQueueLength", "Queues")</li>
    ```

1. <span data-ttu-id="d2d64-229">Kör hello programmet och välj **hämta kölängden** toosee resulterar liknande toohello följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="d2d64-229">Run hello application, and select **Get queue length** toosee results similar toohello following screen shot:</span></span>
  
    ![Hämta kölängden](./media/vs-storage-aspnet-getting-started-queues/get-queue-length-results.png)


## <a name="delete-a-queue"></a><span data-ttu-id="d2d64-231">Ta bort en kö</span><span class="sxs-lookup"><span data-stu-id="d2d64-231">Delete a queue</span></span>
<span data-ttu-id="d2d64-232">Detta avsnitt visar hur toodelete en kö.</span><span class="sxs-lookup"><span data-stu-id="d2d64-232">This section illustrates how toodelete a queue.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="d2d64-233">Det här avsnittet förutsätter att du har slutfört steg hello [ställa in hello utvecklingsmiljö](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="d2d64-233">This section assumes you have completed hello steps [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="d2d64-234">Öppna hello `QueuesController.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="d2d64-234">Open hello `QueuesController.cs` file.</span></span>

1. <span data-ttu-id="d2d64-235">Lägg till en metod som kallas **DeleteQueue** som returnerar en **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-235">Add a method called **DeleteQueue** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult DeleteQueue()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="d2d64-236">Inom hello **DeleteQueue** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="d2d64-236">Within hello **DeleteQueue** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="d2d64-237">Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)</span><span class="sxs-lookup"><span data-stu-id="d2d64-237">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="d2d64-238">Hämta en **CloudQueueClient** -objektet representerar en queue-tjänstklienten.</span><span class="sxs-lookup"><span data-stu-id="d2d64-238">Get a **CloudQueueClient** object represents a queue service client.</span></span>
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. <span data-ttu-id="d2d64-239">Hämta en **CloudQueueContainer** objekt som representerar en referens toohello kö.</span><span class="sxs-lookup"><span data-stu-id="d2d64-239">Get a **CloudQueueContainer** object that represents a reference toohello queue.</span></span> 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. <span data-ttu-id="d2d64-240">Anropa hello **CloudQueue.Delete** metoden toodelete hello kön representeras av hello **CloudQueue** objekt.</span><span class="sxs-lookup"><span data-stu-id="d2d64-240">Call hello **CloudQueue.Delete** method toodelete hello queue represented by hello **CloudQueue** object.</span></span>

    ```csharp
    queue.Delete();
    ```

1. <span data-ttu-id="d2d64-241">Uppdatera hello **ViewBag** med hello namnet hello kön och dess längd.</span><span class="sxs-lookup"><span data-stu-id="d2d64-241">Update hello **ViewBag** with hello name of hello queue, and its length.</span></span>

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```
 
1. <span data-ttu-id="d2d64-242">I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **köer**, och hello snabbmenyn, Välj **Lägg till -> Visa**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-242">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Queues**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="d2d64-243">På hello **Lägg till vy** dialogrutan Ange **DeleteQueue** hello vynamn och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d2d64-243">On hello **Add View** dialog, enter **DeleteQueue** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="d2d64-244">Öppna `DeleteQueue.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="d2d64-244">Open `DeleteQueue.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "DeleteQueue";
    }
    
    <h2>Delete Queue results</h2>
    
    @ViewBag.QueueName deleted.
    ```

1. <span data-ttu-id="d2d64-245">I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="d2d64-245">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="d2d64-246">Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="d2d64-246">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Delete queue", "DeleteQueue", "Queues")</li>
    ```

1. <span data-ttu-id="d2d64-247">Kör hello programmet och välj **hämta kölängden** toosee resulterar liknande toohello följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="d2d64-247">Run hello application, and select **Get queue length** toosee results similar toohello following screen shot:</span></span>
  
    ![Ta bort kön](./media/vs-storage-aspnet-getting-started-queues/delete-queue-results.png)

## <a name="next-steps"></a><span data-ttu-id="d2d64-249">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d2d64-249">Next steps</span></span>
<span data-ttu-id="d2d64-250">Visa mer funktionen guider toolearn om ytterligare alternativ för att lagra data i Azure.</span><span class="sxs-lookup"><span data-stu-id="d2d64-250">View more feature guides toolearn about additional options for storing data in Azure.</span></span>

  * [<span data-ttu-id="d2d64-251">Kom igång med Azure blob storage och Visual Studio anslutna tjänster (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="d2d64-251">Get started with Azure blob storage and Visual Studio Connected Services (ASP.NET)</span></span>](../storage/vs-storage-aspnet-getting-started-blobs.md)
  * [<span data-ttu-id="d2d64-252">Kom igång med Azure-tabellagring och Visual Studio anslutna tjänster (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="d2d64-252">Get started with Azure table storage and Visual Studio Connected Services (ASP.NET)</span></span>](vs-storage-aspnet-getting-started-tables.md)
