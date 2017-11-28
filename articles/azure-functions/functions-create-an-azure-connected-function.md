---
title: "Skapa en funktion som ansluter till Azure-tjänster | Microsoft-dokument"
description: "Använd Azure Functions för att skapa ett serverlöst program som ansluter till andra Azure-tjänster."
services: functions
documentationcenter: dev-center-name
author: yochay
manager: manager-alias
editor: 
tags: 
keywords: "azure-funktioner, funktioner, händelsebearbetning, webhooks, dynamisk beräkning, serverlös arkitektur"
ms.assetid: ab86065d-6050-46c9-a336-1bfc1fa4b5a1
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/01/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 65964a322f0adab4f648fb350bedb77b46bf9054
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-functions-to-create-a-function-that-connects-to-other-azure-services"></a><span data-ttu-id="a9d09-104">Använd Azure Functions för att skapa en funktion som ansluter till andra Azure-tjänster</span><span class="sxs-lookup"><span data-stu-id="a9d09-104">Use Azure Functions to create a function that connects to other Azure services</span></span>

<span data-ttu-id="a9d09-105">I det här avsnittet får du lära dig att skapa en funktion i Azure Functions som lyssnar efter meddelanden i en Azure Storage-kö och kopierar meddelandena till en Azure Storage-tabell.</span><span class="sxs-lookup"><span data-stu-id="a9d09-105">This topic shows you how to create a function in Azure Functions that listens to messages on an Azure Storage queue and copies the messages to rows in an Azure Storage table.</span></span> <span data-ttu-id="a9d09-106">En timerutlöst funktion används för att läsa in meddelanden i kön.</span><span class="sxs-lookup"><span data-stu-id="a9d09-106">A timer triggered function is used to load messages into the queue.</span></span> <span data-ttu-id="a9d09-107">En andra funktion läser från kön och skriver meddelanden till tabellen.</span><span class="sxs-lookup"><span data-stu-id="a9d09-107">A second function reads from the queue and writes messages to the table.</span></span> <span data-ttu-id="a9d09-108">Både kön och tabellen skapas av Azure Functions utifrån bindningsdefinitionerna.</span><span class="sxs-lookup"><span data-stu-id="a9d09-108">Both the queue and the table are created for you by Azure Functions based on the binding definitions.</span></span> 

<span data-ttu-id="a9d09-109">För att göra det ännu mer intressant är en funktion skriven i JavaScript och en i C#-skript.</span><span class="sxs-lookup"><span data-stu-id="a9d09-109">To make things more interesting, one function is written in JavaScript and the other is written in C# script.</span></span> <span data-ttu-id="a9d09-110">Detta visar hur en funktionsapp kan ha funktioner på olika språk.</span><span class="sxs-lookup"><span data-stu-id="a9d09-110">This demonstrates how a function app can have functions in various languages.</span></span> 

<span data-ttu-id="a9d09-111">Det här scenariot visas i en [video på Channel 9](https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player).</span><span class="sxs-lookup"><span data-stu-id="a9d09-111">You can see this scenario demonstrated in a [video on Channel 9](https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player).</span></span>

## <a name="create-a-function-that-writes-to-the-queue"></a><span data-ttu-id="a9d09-112">Skapa en funktion som skriver till kön</span><span class="sxs-lookup"><span data-stu-id="a9d09-112">Create a function that writes to the queue</span></span>

<span data-ttu-id="a9d09-113">Innan du kan ansluta till en lagringskö måste du skapa en funktion som läser in meddelandekön.</span><span class="sxs-lookup"><span data-stu-id="a9d09-113">Before you can connect to a storage queue, you need to create a function that loads the message queue.</span></span> <span data-ttu-id="a9d09-114">JavaScript-funktionen använder en timer som utlösare som skriver ett meddelande till kön var 10: e sekund.</span><span class="sxs-lookup"><span data-stu-id="a9d09-114">This JavaScript function uses a timer trigger that writes a message to the queue every 10 seconds.</span></span> <span data-ttu-id="a9d09-115">Om du inte redan har ett Azure-konto kan du läsa [Try Azure Functions](https://functions.azure.com/try) (Prova Azure Functions) eller [skapa ett kostnadsfritt Azure-konto](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="a9d09-115">If you don't already have an Azure account, check out the [Try Azure Functions](https://functions.azure.com/try) experience, or [create your free Azure account](https://azure.microsoft.com/free/).</span></span>

1. <span data-ttu-id="a9d09-116">Gå till Azure Portal och leta upp din funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="a9d09-116">Go to the Azure portal and locate your function app.</span></span>

2. <span data-ttu-id="a9d09-117">Klicka på **Ny funktion** > **TimerTrigger-JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="a9d09-117">Click **New Function** > **TimerTrigger-JavaScript**.</span></span> 

3. <span data-ttu-id="a9d09-118">Ge funktionen namnet **FunctionsBindingsDemo1**, ange ett cron-uttrycksvärde på `0/10 * * * * *` för **Schema** och klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a9d09-118">Name the function **FunctionsBindingsDemo1**, enter a cron expression value of `0/10 * * * * *` for **Schedule**, and then click **Create**.</span></span>
   
    ![Skapa en timerutlöst funktion](./media/functions-create-an-azure-connected-function/new-trigger-timer-function.png)

    <span data-ttu-id="a9d09-120">Du har nu skapat en timerutlöst funktion som körs var 10:e sekund.</span><span class="sxs-lookup"><span data-stu-id="a9d09-120">You have now created a timer triggered function that runs every 10 seconds.</span></span>

5. <span data-ttu-id="a9d09-121">På fliken **Utveckla** klickar du på **Loggar** och visar aktiviteten i loggen.</span><span class="sxs-lookup"><span data-stu-id="a9d09-121">On the **Develop** tab, click **Logs** and view the activity in the log.</span></span> <span data-ttu-id="a9d09-122">Du ser att en loggpost skrivs var 10: e sekund.</span><span class="sxs-lookup"><span data-stu-id="a9d09-122">You see a log entry written every 10 seconds.</span></span>
   
    ![Visa loggen för att kontrollera att funktionen fungerar](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-view-log.png)

## <a name="add-a-message-queue-output-binding"></a><span data-ttu-id="a9d09-124">Lägga till en meddelandekö utan utdatabindning</span><span class="sxs-lookup"><span data-stu-id="a9d09-124">Add a message queue output binding</span></span>

1. <span data-ttu-id="a9d09-125">På fliken **Integrera** väljer du **Nya utdata**  >  **Azure Queue Storage**  >  **Välj** .</span><span class="sxs-lookup"><span data-stu-id="a9d09-125">On the **Integrate** tab, choose **New Output** > **Azure Queue Storage** > **Select**.</span></span>

    ![Lägga till en utlösande timer-funktion](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab.png)

2. <span data-ttu-id="a9d09-127">Ange `myQueueItem` i **Meddelandeparameternamn** och `functions-bindings` i **Könamn**, välj en befintlig **Anslutning till lagringskonto** eller klicka på **ny** för att skapa en anslutning till lagringskonto. Klicka sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a9d09-127">Enter `myQueueItem` for **Message parameter name** and `functions-bindings` for **Queue name**, select an existing **Storage account connection** or click **new** to create a storage account connection, and then click **Save**.</span></span>  

    ![Skapa en utdatabindning till lagringskön](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab2.png)

1. <span data-ttu-id="a9d09-129">På fliken **Utveckla** lägger du till följande kod till funktionen:</span><span class="sxs-lookup"><span data-stu-id="a9d09-129">Back in the **Develop** tab, append the following code to the function:</span></span>
   
    ```javascript
   
    function myQueueItem() 
    {
        return {
            msg: "some message goes here",
            time: "time goes here"
        }
    }
   
    ```
2. <span data-ttu-id="a9d09-130">Leta reda på instruktionen *if* omkring rad 9 i funktionen och infoga följande kod efter instruktionen.</span><span class="sxs-lookup"><span data-stu-id="a9d09-130">Locate the *if* statement around line 9 of the function, and insert the following code after that statement.</span></span>
   
    ```javascript
   
    var toBeQed = myQueueItem();
    toBeQed.time = timeStamp;
    context.bindings.myQueueItem = toBeQed;
   
    ```  
   
    <span data-ttu-id="a9d09-131">Den här koden skapar en **myQueueItem** och ställer in dess **tid**-egenskap till aktuell timeStamp.</span><span class="sxs-lookup"><span data-stu-id="a9d09-131">This code creates a **myQueueItem** and sets its **time** property to the current timeStamp.</span></span> <span data-ttu-id="a9d09-132">Den lägger sedan till det nya köobjektet till kontextens **myQueueItem**-bindning.</span><span class="sxs-lookup"><span data-stu-id="a9d09-132">It then adds the new queue item to the context's **myQueueItem** binding.</span></span>

3. <span data-ttu-id="a9d09-133">Klicka på **Spara och kör**.</span><span class="sxs-lookup"><span data-stu-id="a9d09-133">Click **Save and Run**.</span></span>

## <a name="view-storage-updates-by-using-storage-explorer"></a><span data-ttu-id="a9d09-134">Visa lagringsuppdateringar med Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="a9d09-134">View storage updates by using Storage Explorer</span></span>
<span data-ttu-id="a9d09-135">Du kan kontrollera att funktionen fungerar genom att visa meddelanden i kön du har skapat.</span><span class="sxs-lookup"><span data-stu-id="a9d09-135">You can verify that your function is working by viewing messages in the queue you created.</span></span>  <span data-ttu-id="a9d09-136">Du kan ansluta lagringskön med Cloud Explorer i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a9d09-136">You can connect to your storage queue by using Cloud Explorer in Visual Studio.</span></span> <span data-ttu-id="a9d09-137">Portalen gör det emellertid lätt att ansluta lagringskontot med Microsoft Azure Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="a9d09-137">However, the portal makes it easy to connect to your storage account by using Microsoft Azure Storage Explorer.</span></span>

1. <span data-ttu-id="a9d09-138">På fliken **Integrera** klickar du på din utdatabindningskö > **Dokumentation** och tar sedan fram anslutningssträngen för lagringskontot så att du kan kopiera värdet.</span><span class="sxs-lookup"><span data-stu-id="a9d09-138">In the **Integrate** tab, click your queue output binding > **Documentation**, then unhide the Connection String for your storage account and copy the value.</span></span> <span data-ttu-id="a9d09-139">Du använder värdet för att ansluta till lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="a9d09-139">You use this value to connect to your storage account.</span></span>

    ![Hämta Azure Storage Explorer](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab3.png)


2. <span data-ttu-id="a9d09-141">Om du inte redan har gjort det laddar du ned och installerar [Microsoft Azure Storage Explorer](http://storageexplorer.com).</span><span class="sxs-lookup"><span data-stu-id="a9d09-141">If you haven't already done so, download and install [Microsoft Azure Storage Explorer](http://storageexplorer.com).</span></span> 
 
3. <span data-ttu-id="a9d09-142">I Storage Explorer klickar du på ikonen för att ansluta till Azure Storage, klistrar in anslutningssträngen i fältet och slutför guiden.</span><span class="sxs-lookup"><span data-stu-id="a9d09-142">In Storage Explorer, click the connect to Azure Storage icon, paste the connection string in the field, and complete the wizard.</span></span>

    ![Lägga till en anslutning i Storage Explorer](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-storage-explorer.png)

4. <span data-ttu-id="a9d09-144">Under **Local and attached** (Lokal och ansluten) expanderar du **Lagringskonton** > ditt lagringskonto > **Köer** > **functions-bindings** och kontrollerar att meddelanden skrivs till kön.</span><span class="sxs-lookup"><span data-stu-id="a9d09-144">Under **Local and attached**, expand **Storage Accounts** > your storage account > **Queues** > **functions-bindings** and verify that messages are written to the queue.</span></span>

    ![Visa meddelanden i kön](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer.png)

    <span data-ttu-id="a9d09-146">Om kön inte finns eller är tom finns det troligen ett problem med din funktionsbindning eller kod.</span><span class="sxs-lookup"><span data-stu-id="a9d09-146">If the queue does not exist or is empty, there is most likely a problem with your function binding or code.</span></span>

## <a name="create-a-function-that-reads-from-the-queue"></a><span data-ttu-id="a9d09-147">Skapa en funktion som läser från kön</span><span class="sxs-lookup"><span data-stu-id="a9d09-147">Create a function that reads from the queue</span></span>

<span data-ttu-id="a9d09-148">Du kan skapa en annan funktion som läser från kön och skriver meddelanden permanent till en Azure Storage-tabell nu när du har meddelanden som läggs till i kön.</span><span class="sxs-lookup"><span data-stu-id="a9d09-148">Now that you have messages being added to the queue, you can create another function that reads from the queue and writes the messages permanently to an Azure Storage table.</span></span>

1. <span data-ttu-id="a9d09-149">Klicka på **Ny funktion** > **QueueTrigger-CSharp**.</span><span class="sxs-lookup"><span data-stu-id="a9d09-149">Click **New Function** > **QueueTrigger-CSharp**.</span></span> 
 
2. <span data-ttu-id="a9d09-150">Namnge funktionen `FunctionsBindingsDemo2`, skriv **functions-bindings** i fältet **Könamn**, välj ett befintligt lagringskonto eller skapa ett och klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a9d09-150">Name the function `FunctionsBindingsDemo2`, enter **functions-bindings** in the **Queue name** field, select an existing storage account or create one, and then click **Create**.</span></span>

    ![Lägg till en timer-funktion för utdatakö](./media/functions-create-an-azure-connected-function/function-demo2-new-function.png) 

3. <span data-ttu-id="a9d09-152">(Valfritt) Du kan kontrollera att den nya funktionen fungerar genom att visa den nya kön i Storage Explorer som tidigare.</span><span class="sxs-lookup"><span data-stu-id="a9d09-152">(Optional) You can verify that the new function works by viewing the new queue in Storage Explorer as before.</span></span> <span data-ttu-id="a9d09-153">Du kan också använda Cloud Explorer i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a9d09-153">You can also use Cloud Explorer in Visual Studio.</span></span>  

4. <span data-ttu-id="a9d09-154">(Valfritt) Uppdatera kön **functions-bindings** och lägg märke till om några objekt har tagits bort från kön.</span><span class="sxs-lookup"><span data-stu-id="a9d09-154">(Optional) Refresh the **functions-bindings** queue and notice that items have been removed from the queue.</span></span> <span data-ttu-id="a9d09-155">Borttagningen sker på grund av att funktionen är bunden till kön **functions-bindings** som indatautlösare och funktionen läser kön.</span><span class="sxs-lookup"><span data-stu-id="a9d09-155">The removal occurs because the function is bound to the **functions-bindings** queue as an input trigger and the function reads the queue.</span></span> 
 
## <a name="add-a-table-output-binding"></a><span data-ttu-id="a9d09-156">Lägga till en utdatabindningstabell</span><span class="sxs-lookup"><span data-stu-id="a9d09-156">Add a table output binding</span></span>

1. <span data-ttu-id="a9d09-157">I FunctionsBindingsDemo2 klickar du på **Integrera** > **Nya utdata** > **Azure Table Storage** > **Välj**.</span><span class="sxs-lookup"><span data-stu-id="a9d09-157">In FunctionsBindingsDemo2, click **Integrate** > **New Output** > **Azure Table Storage** > **Select**.</span></span>

    ![Lägga till en bindning till en Azure Storage-tabell](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab.png) 

2. <span data-ttu-id="a9d09-159">Skriv `functionbindings` som **Tabellnamn** och `myTable` som **Tabellparameternamn**, välj en **Lagringskontoanslutning** eller skapa en ny, och klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a9d09-159">Enter `functionbindings` for **Table name** and `myTable` for **Table parameter name**, choose a **Storage account connection** or create a new one, and then click **Save**.</span></span>

    ![Konfigurera Storage-tabellbindning](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab2.png)
   
3. <span data-ttu-id="a9d09-161">På fliken **Utveckla** ersätter du den befintliga funktionskoden med följande:</span><span class="sxs-lookup"><span data-stu-id="a9d09-161">In the **Develop** tab, replace the existing function code with the following:</span></span>
   
    ```cs
    
    using System;
    
    public static void Run(QItem myQueueItem, ICollector<TableItem> myTable, TraceWriter log)
    {    
        TableItem myItem = new TableItem
        {
            PartitionKey = "key",
            RowKey = Guid.NewGuid().ToString(),
            Time = DateTime.Now.ToString("hh.mm.ss.ffffff"),
            Msg = myQueueItem.Msg,
            OriginalTime = myQueueItem.Time    
        };
        
        // Add the item to the table binding collection.
        myTable.Add(myItem);
    
        log.Verbose($"C# Queue trigger function processed: {myItem.RowKey} | {myItem.Msg} | {myItem.Time}");
    }
    
    public class TableItem
    {
        public string PartitionKey {get; set;}
        public string RowKey {get; set;}
        public string Time {get; set;}
        public string Msg {get; set;}
        public string OriginalTime {get; set;}
    }
    
    public class QItem
    {
        public string Msg { get; set;}
        public string Time { get; set;}
    }
    ```
    <span data-ttu-id="a9d09-162">Klassen **TableItem** visar en rad i lagringstabellen, och du lägger till objektet till `myTable` samlingen med **TableItem**-objekt.</span><span class="sxs-lookup"><span data-stu-id="a9d09-162">The **TableItem** class represents a row in the storage table, and you add the item to the `myTable` collection of **TableItem** objects.</span></span> <span data-ttu-id="a9d09-163">Du måste ställa in egenskaperna **PartitionKey** och **RowKey** för att kunna infoga i tabellen.</span><span class="sxs-lookup"><span data-stu-id="a9d09-163">You must set the **PartitionKey** and **RowKey** properties to be able to insert into the table.</span></span>

4. <span data-ttu-id="a9d09-164">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a9d09-164">Click **Save**.</span></span>  <span data-ttu-id="a9d09-165">Slutligen kan du kontrollera att funktionerna fungerar genom att visa tabellen i Storage Explorer eller Visual Studio Cloud Explorer.</span><span class="sxs-lookup"><span data-stu-id="a9d09-165">Finally, you can verify the function works by viewing the table in Storage explorer or Visual Studio Cloud Explorer.</span></span>

5. <span data-ttu-id="a9d09-166">(Valfritt) I ditt lagringskonto i Storage Explorer expanderar du **Tabeller** > **functionsbindings** och kontrollerar att rader har lagts till i tabellen.</span><span class="sxs-lookup"><span data-stu-id="a9d09-166">(Optional) In your storage account in Storage Explorer, expand **Tables** > **functionsbindings** and verify that rows are added to the table.</span></span> <span data-ttu-id="a9d09-167">Du kan göra samma sak i Cloud Explorer i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a9d09-167">You can do the same in Cloud Explorer in Visual Studio.</span></span>

    ![Vy över rader i tabellen](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer2.png)

    <span data-ttu-id="a9d09-169">Om tabellen inte finns eller är tom finns det troligen ett problem med din funktionsbindning eller kod.</span><span class="sxs-lookup"><span data-stu-id="a9d09-169">If the table does not exist or is empty, there is most likely a problem with your function binding or code.</span></span> 
 
[!INCLUDE [More binding information](../../includes/functions-bindings-next-steps.md)]

## <a name="next-steps"></a><span data-ttu-id="a9d09-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a9d09-170">Next steps</span></span>
<span data-ttu-id="a9d09-171">Mer information om Azure Functions finns i följande ämnen:</span><span class="sxs-lookup"><span data-stu-id="a9d09-171">For more information about Azure Functions, see the following topics:</span></span>

* [<span data-ttu-id="a9d09-172">Azure Functions, info för utvecklare</span><span class="sxs-lookup"><span data-stu-id="a9d09-172">Azure Functions developer reference</span></span>](functions-reference.md)  
  <span data-ttu-id="a9d09-173">Info för programmerare om att koda funktioner och definiera utlösare och bindningar.</span><span class="sxs-lookup"><span data-stu-id="a9d09-173">Programmer reference for coding functions and defining triggers and bindings.</span></span>
* [<span data-ttu-id="a9d09-174">Testa Azure Functions</span><span class="sxs-lookup"><span data-stu-id="a9d09-174">Testing Azure Functions</span></span>](functions-test-a-function.md)  
  <span data-ttu-id="a9d09-175">Beskriver olika verktyg och tekniker för att testa funktioner.</span><span class="sxs-lookup"><span data-stu-id="a9d09-175">Describes various tools and techniques for testing your functions.</span></span>
* [<span data-ttu-id="a9d09-176">Så här skalar du Azure Functions</span><span class="sxs-lookup"><span data-stu-id="a9d09-176">How to scale Azure Functions</span></span>](functions-scale.md)  
  <span data-ttu-id="a9d09-177">Beskriver tillgängliga serviceplaner för Azure Functions, inklusive värdplanen för förbrukning, och hur du väljer rätt plan.</span><span class="sxs-lookup"><span data-stu-id="a9d09-177">Discusses service plans available with Azure Functions, including the Consumption hosting plan, and how to choose the right plan.</span></span> 

[!INCLUDE [Getting help note](../../includes/functions-get-help.md)]

