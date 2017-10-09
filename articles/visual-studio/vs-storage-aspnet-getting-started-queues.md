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
# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services-aspnet"></a>Kom igång med Azure queue storage- och Visual Studio anslutna tjänster (ASP.NET)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Översikt

Azure queue storage erbjuder molntjänster meddelandehantering mellan programkomponenter. När program utformas för skalning är programkomponenterna ofta fristående, så att de kan skalas oberoende av varandra. Queue storage ger asynkrona meddelanden för kommunikation mellan programkomponenter, oavsett om de körs i hello molnet på hello skrivbordet, på en lokal server eller på en mobil enhet. Queue Storage har också stöd för hantering av asynkrona åtgärder och utveckling av processarbetsflöden.

Den här kursen visar hur toowrite ASP.NET kod för några vanliga scenarier med hjälp av Azure queue storage entiteter. Dessa scenarier som inkluderar vanliga aktiviteter som att skapa en Azure-kö och lägga till, ändra, läsa och tar bort Kömeddelanden.

##<a name="prerequisites"></a>Krav

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure Storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>Skapa en MVC-enhet 

1. I hello **Solution Explorer**, högerklicka på **domänkontrollanter**, hello snabbmenyn, Välj **Lägg till -> Controller**.

    ![Lägg till en domänkontrollant tooan ASP.NET MVC-app](./media/vs-storage-aspnet-getting-started-queues/add-controller-menu.png)

1. På hello **Lägg till Kodskelett** markerar **MVC 5 styrenhet – tom**, och välj **Lägg till**.

    ![Ange typ av MVC-domänkontrollant](./media/vs-storage-aspnet-getting-started-queues/add-controller.png)

1. På hello **Lägg till styrenhet** dialogrutan, namnet hello controller *QueuesController*, och välj **Lägg till**.

    ![Namnet hello MVC-enhet](./media/vs-storage-aspnet-getting-started-queues/add-controller-name.png)

1. Lägg till följande hello *med* direktiven toohello `QueuesController.cs` fil:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    ```
## <a name="create-a-queue"></a>Skapa en kö

hello följande steg visar hur toocreate en kö:

> [!NOTE]
> 
> Det här avsnittet förutsätter att du har slutfört steg hello [ställa in hello utvecklingsmiljö](#set-up-the-development-environment). 

1. Öppna hello `QueuesController.cs` fil. 

1. Lägg till en metod som kallas **CreateQueue** som returnerar en **ActionResult**.

    ```csharp
    public ActionResult CreateQueue()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. Inom hello **CreateQueue** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Hämta en **CloudQueueClient** -objektet representerar en queue-tjänstklienten.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```
1. Hämta en **CloudQueue** objekt som representerar en referensnamn toohello önskad kö. Hej **CloudQueueClient.GetQueueReference** inte gör en begäran mot queue storage. hello referens returneras hello kön finns eller inte. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Anropa hello **CloudQueue.CreateIfNotExists** metoden toocreate hello kö om den inte finns ännu. Hej **CloudQueue.CreateIfNotExists** metoden returnerar **SANT** om hello kön finns inte och har skapats. Annars **FALSKT** returneras.    

    ```csharp
    ViewBag.Success = queue.CreateIfNotExists();
    ```

1. Uppdatera hello **ViewBag** med hello namnet på hello kö.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```

1. I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **köer**, och hello snabbmenyn, Välj **Lägg till -> Visa**.

1. På hello **Lägg till vy** dialogrutan Ange **CreateQueue** hello vynamn och välj **Lägg till**.

1. Öppna `CreateQueue.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:

    ```csharp
    @{
        ViewBag.Title = "Create Queue";
    }
    
    <h2>Create Queue results</h2>

    Creation of @ViewBag.QueueName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.

1. Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create queue", "CreateQueue", "Queues")</li>
    ```

1. Kör hello programmet och välj **Skapa kö** toosee resulterar liknande toohello följande skärmbild:
  
    ![Skapa kö](./media/vs-storage-aspnet-getting-started-queues/create-queue-results.png)

    Som tidigare nämnts hello **CloudQueue.CreateIfNotExists** metoden returnerar **SANT** endast när hello kön finns inte och har skapats. Om du kör hello app när hello kön finns, hello-metoden returnerar därför **FALSKT**. toorun hello app flera gånger, måste du ta bort hello kön innan du kör hello appen igen. Ta bort hello kön kan göras via hello **CloudQueue.Delete** metod. Du kan också ta bort hello kön med hello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040) eller hello [Microsoft Azure Lagringsutforskaren](../vs-azure-tools-storage-manage-with-storage-explorer.md).  

## <a name="add-a-message-tooa-queue"></a>Lägg till en meddelandekö tooa

När du har [skapade en kö](#create-a-queue), kan du lägga till meddelanden toothat kön. Det här avsnittet vägleder dig genom att lägga till en meddelandekö tooa *test-kön*. 

> [!NOTE]
> 
> Det här avsnittet förutsätter att du har slutfört steg hello [ställa in hello utvecklingsmiljö](#set-up-the-development-environment). 

1. Öppna hello `QueuesController.cs` fil.

1. Lägg till en metod som kallas **AddMessage** som returnerar en **ActionResult**.

    ```csharp
    public ActionResult AddMessage()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. Inom hello **AddMessage** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Hämta en **CloudQueueClient** -objektet representerar en queue-tjänstklienten.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Hämta en **CloudQueueContainer** objekt som representerar en referens toohello kö. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Skapa hello **CloudQueueMessage** objekt som representerar hello-meddelande som du vill tooadd toohello kön. En **CloudQueueMessage** objekt kan skapas från en sträng (i UTF-8-format) eller en byte-matris.

    ```csharp
    CloudQueueMessage message = new CloudQueueMessage("Hello, Azure Queue Storage");
    ```

1. Anropa hello **CloudQueue.AddMessage** metoden tooadd hello postmeddelandet toohello kön.

    ```csharp
    queue.AddMessage(message);
    ```

1. Skapa och ange några **ViewBag** egenskaper för visning i hello vy.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```

1. I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **köer**, och hello snabbmenyn, Välj **Lägg till -> Visa**.

1. På hello **Lägg till vy** dialogrutan Ange **AddMessage** hello vynamn och välj **Lägg till**.

1. Öppna `AddMessage.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:

    ```csharp
    @{
        ViewBag.Title = "Add Message";
    }
    
    <h2>Add Message results</h2>
    
    hello message '@ViewBag.Message' was added toohello queue '@ViewBag.QueueName'.
    ```

1. I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.

1. Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Add message", "AddMessage", "Queues")</li>
    ```

1. Kör hello programmet och välj **Lägg till meddelandet** toosee resulterar liknande toohello följande skärmbild:
  
    ![Lägga till meddelande](./media/vs-storage-aspnet-getting-started-queues/add-message-results.png)

Hej två avsnitt - [läsa ett meddelande från en kö utan att ta bort den](#read-a-message-from-a-queue-without-removing-it) och [Läs- och ta bort ett meddelande från en kö](#read-and-remove-a-message-from-a-queue) -illustrerar hur tooread meddelanden från en kö.  

## <a name="read-a-message-from-a-queue-without-removing-it"></a>Läsa ett meddelande från en kö utan att ta bort den

Detta avsnitt visar hur toopeek på ett meddelande i kön (skrivskyddade hello första meddelande utan att ta bort det).  

> [!NOTE]
> 
> Det här avsnittet förutsätter att du har slutfört steg hello [ställa in hello utvecklingsmiljö](#set-up-the-development-environment). 

1. Öppna hello `QueuesController.cs` fil.

1. Lägg till en metod som kallas **PeekMessage** som returnerar en **ActionResult**.

    ```csharp
    public ActionResult PeekMessage()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. Inom hello **PeekMessage** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Hämta en **CloudQueueClient** -objektet representerar en queue-tjänstklienten.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Hämta en **CloudQueueContainer** objekt som representerar en referens toohello kö. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Anropa hello **CloudQueue.PeekMessage** metoden tooread hello första meddelandet i hello kön utan att ta bort den från hello kö. 

    ```csharp
    CloudQueueMessage message = queue.PeekMessage();
    ```

1. Uppdatera hello **ViewBag** med två värden: hello könamnet och hello-meddelande som har lästs. Hej **CloudQueueMessage** objektet innehåller två egenskaper för att hämta hello Objektvärde: **CloudQueueMessage.AsBytes** och **CloudQueueMessage.AsString**. **AsString** (används i det här exemplet) returnerar en sträng, medan **AsBytes** returnerar en bytematris.

    ```csharp
    ViewBag.QueueName = queue.Name; 
    ViewBag.Message = (message != null ? message.AsString : "");
    ```

1. I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **köer**, och hello snabbmenyn, Välj **Lägg till -> Visa**.

1. På hello **Lägg till vy** dialogrutan Ange **PeekMessage** hello vynamn och välj **Lägg till**.

1. Öppna `PeekMessage.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:

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

1. I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.

1. Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Peek message", "PeekMessage", "Queues")</li>
    ```

1. Kör hello programmet och välj **titt meddelandet** toosee resulterar liknande toohello följande skärmbild:
  
    ![Granska meddelande](./media/vs-storage-aspnet-getting-started-queues/peek-message-results.png)

## <a name="read-and-remove-a-message-from-a-queue"></a>Läsa och ta bort ett meddelande från en kö

I det här avsnittet lär du dig hur tooread och ta bort ett meddelande från en kö.   

> [!NOTE]
> 
> Det här avsnittet förutsätter att du har slutfört steg hello [ställa in hello utvecklingsmiljö](#set-up-the-development-environment). 

1. Öppna hello `QueuesController.cs` fil.

1. Lägg till en metod som kallas **ReadMessage** som returnerar en **ActionResult**.

    ```csharp
    public ActionResult ReadMessage()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. Inom hello **ReadMessage** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Hämta en **CloudQueueClient** -objektet representerar en queue-tjänstklienten.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Hämta en **CloudQueueContainer** objekt som representerar en referens toohello kö. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Anropa hello **CloudQueue.GetMessage** metoden tooread hello första meddelandet i kön hello. Hej **CloudQueue.GetMessage** metod kan hello meddelandet osynlig för 30 sekunder (som standard) tooany annan kod läsa meddelanden så att ingen annan kod kan ändra eller ta bort hello-meddelande under din bearbetning av den. toochange hello mängden tid hello-meddelande är osynliga, ändra hello **visibilityTimeout** parameter som skickas toohello **CloudQueue.GetMessage** metod.

    ```csharp
    // This message will be invisible tooother code for 30 seconds.
    CloudQueueMessage message = queue.GetMessage();     
    ```

1. Anropa hello **CloudQueueMessage.Delete** metoden toodelete hello-meddelande från hello kö.

    ```csharp
    queue.DeleteMessage(message);
    ```

1. Uppdatera hello **ViewBag** med hello meddelandet tas bort och hello namnet på hello kö.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Message = message.AsString;
    ```
 
1. I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **köer**, och hello snabbmenyn, Välj **Lägg till -> Visa**.

1. På hello **Lägg till vy** dialogrutan Ange **ReadMessage** hello vynamn och välj **Lägg till**.

1. Öppna `ReadMessage.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:

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

1. I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.

1. Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Read/Delete message", "ReadMessage", "Queues")</li>
    ```

1. Kör hello programmet och välj **läsa/ta bort meddelandet** toosee resulterar liknande toohello följande skärmbild:
  
    ![Läsa och ta bort meddelandet](./media/vs-storage-aspnet-getting-started-queues/read-message-results.png)

## <a name="get-hello-queue-length"></a>Hämta hello Kölängd

Detta avsnitt visar hur tooget hello Kölängd (antal meddelanden). 

> [!NOTE]
> 
> Det här avsnittet förutsätter att du har slutfört steg hello [ställa in hello utvecklingsmiljö](#set-up-the-development-environment). 

1. Öppna hello `QueuesController.cs` fil.

1. Lägg till en metod som kallas **GetQueueLength** som returnerar en **ActionResult**.

    ```csharp
    public ActionResult GetQueueLength()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. Inom hello **ReadMessage** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Hämta en **CloudQueueClient** -objektet representerar en queue-tjänstklienten.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Hämta en **CloudQueueContainer** objekt som representerar en referens toohello kö. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Anropa hello **CloudQueue.FetchAttributes** metoden tooretrieve hello köns attribut (inklusive dess längd). 

    ```csharp
    queue.FetchAttributes();
    ```

6. Åtkomst hello **CloudQueue.ApproximateMessageCount** egenskapen tooget hello köns längd.
 
    ```csharp
    int? nMessages = queue.ApproximateMessageCount;
    ```

1. Uppdatera hello **ViewBag** med hello namnet hello kön och dess längd.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ViewBag.Length = nMessages;
    ```
 
1. I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **köer**, och hello snabbmenyn, Välj **Lägg till -> Visa**.

1. På hello **Lägg till vy** dialogrutan Ange **GetQueueLength** hello vynamn och välj **Lägg till**.

1. Öppna `GetQueueLengthMessage.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:

    ```csharp
    @{
        ViewBag.Title = "GetQueueLength";
    }
    
    <h2>Get Queue Length results</h2>
    
    hello queue '@ViewBag.QueueName' has a length of (number of messages): @ViewBag.Length
    ```

1. I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.

1. Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Get queue length", "GetQueueLength", "Queues")</li>
    ```

1. Kör hello programmet och välj **hämta kölängden** toosee resulterar liknande toohello följande skärmbild:
  
    ![Hämta kölängden](./media/vs-storage-aspnet-getting-started-queues/get-queue-length-results.png)


## <a name="delete-a-queue"></a>Ta bort en kö
Detta avsnitt visar hur toodelete en kö. 

> [!NOTE]
> 
> Det här avsnittet förutsätter att du har slutfört steg hello [ställa in hello utvecklingsmiljö](#set-up-the-development-environment). 

1. Öppna hello `QueuesController.cs` fil.

1. Lägg till en metod som kallas **DeleteQueue** som returnerar en **ActionResult**.

    ```csharp
    public ActionResult DeleteQueue()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. Inom hello **DeleteQueue** metod, hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använd hello följande kod tooget hello anslutning sträng och lagring information om lagringskonto från hello Azure tjänstkonfiguration: (ändra  *&lt;behållarens kontonamn >* toohello namnet på hello Azure storage kontot som du försöker komma åt IT-avdelning.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Hämta en **CloudQueueClient** -objektet representerar en queue-tjänstklienten.
   
    ```csharp
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    ```

1. Hämta en **CloudQueueContainer** objekt som representerar en referens toohello kö. 
   
    ```csharp
    CloudQueue queue = queueClient.GetQueueReference("test-queue");
    ```

1. Anropa hello **CloudQueue.Delete** metoden toodelete hello kön representeras av hello **CloudQueue** objekt.

    ```csharp
    queue.Delete();
    ```

1. Uppdatera hello **ViewBag** med hello namnet hello kön och dess längd.

    ```csharp
    ViewBag.QueueName = queue.Name;
    ```
 
1. I hello **Solution Explorer**, expandera hello **vyer** mappen, högerklicka på **köer**, och hello snabbmenyn, Välj **Lägg till -> Visa**.

1. På hello **Lägg till vy** dialogrutan Ange **DeleteQueue** hello vynamn och välj **Lägg till**.

1. Öppna `DeleteQueue.cshtml`, och ändra den så att det ser ut som följande kodavsnitt hello:

    ```csharp
    @{
        ViewBag.Title = "DeleteQueue";
    }
    
    <h2>Delete Queue results</h2>
    
    @ViewBag.QueueName deleted.
    ```

1. I hello **Solution Explorer**, expandera hello **vyer -> delade** och öppna `_Layout.cshtml`.

1. Efter hello senaste **Html.ActionLink**, Lägg till följande hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete queue", "DeleteQueue", "Queues")</li>
    ```

1. Kör hello programmet och välj **hämta kölängden** toosee resulterar liknande toohello följande skärmbild:
  
    ![Ta bort kön](./media/vs-storage-aspnet-getting-started-queues/delete-queue-results.png)

## <a name="next-steps"></a>Nästa steg
Visa mer funktionen guider toolearn om ytterligare alternativ för att lagra data i Azure.

  * [Kom igång med Azure blob storage och Visual Studio anslutna tjänster (ASP.NET)](../storage/vs-storage-aspnet-getting-started-blobs.md)
  * [Kom igång med Azure-tabellagring och Visual Studio anslutna tjänster (ASP.NET)](vs-storage-aspnet-getting-started-tables.md)
