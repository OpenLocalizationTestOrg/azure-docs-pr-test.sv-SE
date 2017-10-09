---
title: "aaaCreate en funktion som ansluter tooAzure tjänster | Microsoft Docs"
description: "Använd Azure Functions toocreate ett program som ansluter tooother Azure services."
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
ms.openlocfilehash: 9d1f7d3b236f8d2c1a404c76aee410f6d458fb7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-functions-toocreate-a-function-that-connects-tooother-azure-services"></a><span data-ttu-id="94cb1-104">Använd Azure Functions toocreate en funktion som ansluter tooother Azure services</span><span class="sxs-lookup"><span data-stu-id="94cb1-104">Use Azure Functions toocreate a function that connects tooother Azure services</span></span>

<span data-ttu-id="94cb1-105">Det här avsnittet visar hur toocreate en funktion i Azure-funktioner som lyssnar toomessages på ett Azure Storage-kö och kopior hello meddelanden toorows i en tabell i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="94cb1-105">This topic shows you how toocreate a function in Azure Functions that listens toomessages on an Azure Storage queue and copies hello messages toorows in an Azure Storage table.</span></span> <span data-ttu-id="94cb1-106">En som utlöste timerfunktion är används tooload meddelanden i kö för hello.</span><span class="sxs-lookup"><span data-stu-id="94cb1-106">A timer triggered function is used tooload messages into hello queue.</span></span> <span data-ttu-id="94cb1-107">En annan funktion läser från hello kön och skriver toohello tabell.</span><span class="sxs-lookup"><span data-stu-id="94cb1-107">A second function reads from hello queue and writes messages toohello table.</span></span> <span data-ttu-id="94cb1-108">Både hello kön och hello tabell skapas du Azure Functions hello bindning definitionerna.</span><span class="sxs-lookup"><span data-stu-id="94cb1-108">Both hello queue and hello table are created for you by Azure Functions based on hello binding definitions.</span></span> 

<span data-ttu-id="94cb1-109">mer intressant toomake saker, en funktion är skriven i JavaScript och hello andra är skriven i C#-skript.</span><span class="sxs-lookup"><span data-stu-id="94cb1-109">toomake things more interesting, one function is written in JavaScript and hello other is written in C# script.</span></span> <span data-ttu-id="94cb1-110">Detta visar hur en funktionsapp kan ha funktioner på olika språk.</span><span class="sxs-lookup"><span data-stu-id="94cb1-110">This demonstrates how a function app can have functions in various languages.</span></span> 

<span data-ttu-id="94cb1-111">Det här scenariot visas i en [video på Channel 9](https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player).</span><span class="sxs-lookup"><span data-stu-id="94cb1-111">You can see this scenario demonstrated in a [video on Channel 9](https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player).</span></span>

## <a name="create-a-function-that-writes-toohello-queue"></a><span data-ttu-id="94cb1-112">Skapa en funktion som skriver toohello kön</span><span class="sxs-lookup"><span data-stu-id="94cb1-112">Create a function that writes toohello queue</span></span>

<span data-ttu-id="94cb1-113">Innan du kan ansluta tooa storage-kö måste toocreate en funktion som läser in hello meddelandekön.</span><span class="sxs-lookup"><span data-stu-id="94cb1-113">Before you can connect tooa storage queue, you need toocreate a function that loads hello message queue.</span></span> <span data-ttu-id="94cb1-114">Den här JavaScript-funktionen använder en timer som utlösare som skriver en meddelandekö toohello var 10: e sekund.</span><span class="sxs-lookup"><span data-stu-id="94cb1-114">This JavaScript function uses a timer trigger that writes a message toohello queue every 10 seconds.</span></span> <span data-ttu-id="94cb1-115">Om du inte redan har ett Azure-konto, kolla hello [försök Azure Functions](https://functions.azure.com/try) uppstår eller [skapa din kostnadsfria Azure-konto](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="94cb1-115">If you don't already have an Azure account, check out hello [Try Azure Functions](https://functions.azure.com/try) experience, or [create your free Azure account](https://azure.microsoft.com/free/).</span></span>

1. <span data-ttu-id="94cb1-116">Gå toohello Azure-portalen och leta upp din funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="94cb1-116">Go toohello Azure portal and locate your function app.</span></span>

2. <span data-ttu-id="94cb1-117">Klicka på **Ny funktion** > **TimerTrigger-JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="94cb1-117">Click **New Function** > **TimerTrigger-JavaScript**.</span></span> 

3. <span data-ttu-id="94cb1-118">Namnge hello funktionen **FunctionsBindingsDemo1**, anger du cron uttrycksvärdet `0/10 * * * * *` för **schema**, och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="94cb1-118">Name hello function **FunctionsBindingsDemo1**, enter a cron expression value of `0/10 * * * * *` for **Schedule**, and then click **Create**.</span></span>
   
    ![Skapa en timerutlöst funktion](./media/functions-create-an-azure-connected-function/new-trigger-timer-function.png)

    <span data-ttu-id="94cb1-120">Du har nu skapat en timerutlöst funktion som körs var 10:e sekund.</span><span class="sxs-lookup"><span data-stu-id="94cb1-120">You have now created a timer triggered function that runs every 10 seconds.</span></span>

5. <span data-ttu-id="94cb1-121">På hello **utveckla** klickar du på **loggar** och visa hello aktivitet i hello-loggen.</span><span class="sxs-lookup"><span data-stu-id="94cb1-121">On hello **Develop** tab, click **Logs** and view hello activity in hello log.</span></span> <span data-ttu-id="94cb1-122">Du ser att en loggpost skrivs var 10: e sekund.</span><span class="sxs-lookup"><span data-stu-id="94cb1-122">You see a log entry written every 10 seconds.</span></span>
   
    ![Visa hello loggen tooverify hello funktionen fungerar](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-view-log.png)

## <a name="add-a-message-queue-output-binding"></a><span data-ttu-id="94cb1-124">Lägga till en meddelandekö utan utdatabindning</span><span class="sxs-lookup"><span data-stu-id="94cb1-124">Add a message queue output binding</span></span>

1. <span data-ttu-id="94cb1-125">På hello **integrera** , Välj **nya utdata** > **Azure Queue Storage** > **Välj**.</span><span class="sxs-lookup"><span data-stu-id="94cb1-125">On hello **Integrate** tab, choose **New Output** > **Azure Queue Storage** > **Select**.</span></span>

    ![Lägga till en utlösande timer-funktion](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab.png)

2. <span data-ttu-id="94cb1-127">Ange `myQueueItem` för **meddelandet parameternamnet** och `functions-bindings` för **könamnet**, Välj en befintlig **konto lagringsanslutning** eller klicka på **nya** toocreate en anslutning-kontot och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="94cb1-127">Enter `myQueueItem` for **Message parameter name** and `functions-bindings` for **Queue name**, select an existing **Storage account connection** or click **new** toocreate a storage account connection, and then click **Save**.</span></span>  

    ![Skapa hello bindning toohello lagring utdatakön](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab2.png)

1. <span data-ttu-id="94cb1-129">Tillbaka i hello **utveckla** fliken, Lägg till följande kod toohello funktionen hello:</span><span class="sxs-lookup"><span data-stu-id="94cb1-129">Back in hello **Develop** tab, append hello following code toohello function:</span></span>
   
    ```javascript
   
    function myQueueItem() 
    {
        return {
            msg: "some message goes here",
            time: "time goes here"
        }
    }
   
    ```
2. <span data-ttu-id="94cb1-130">Leta upp hello *om* instruktionen runt rad 9 för hello-funktionen och infoga hello följande kod efter denna instruktion.</span><span class="sxs-lookup"><span data-stu-id="94cb1-130">Locate hello *if* statement around line 9 of hello function, and insert hello following code after that statement.</span></span>
   
    ```javascript
   
    var toBeQed = myQueueItem();
    toBeQed.time = timeStamp;
    context.bindings.myQueueItem = toBeQed;
   
    ```  
   
    <span data-ttu-id="94cb1-131">Den här koden skapar en **myQueueItem** och ställer in dess **tid** egenskapen toohello aktuell tidsstämpel.</span><span class="sxs-lookup"><span data-stu-id="94cb1-131">This code creates a **myQueueItem** and sets its **time** property toohello current timeStamp.</span></span> <span data-ttu-id="94cb1-132">Lägger sedan till hello ny kö objektet toohello kontexts **myQueueItem** bindning.</span><span class="sxs-lookup"><span data-stu-id="94cb1-132">It then adds hello new queue item toohello context's **myQueueItem** binding.</span></span>

3. <span data-ttu-id="94cb1-133">Klicka på **Spara och kör**.</span><span class="sxs-lookup"><span data-stu-id="94cb1-133">Click **Save and Run**.</span></span>

## <a name="view-storage-updates-by-using-storage-explorer"></a><span data-ttu-id="94cb1-134">Visa lagringsuppdateringar med Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="94cb1-134">View storage updates by using Storage Explorer</span></span>
<span data-ttu-id="94cb1-135">Du kan kontrollera att funktionen fungerar genom att visa meddelanden i hello kön som du skapade.</span><span class="sxs-lookup"><span data-stu-id="94cb1-135">You can verify that your function is working by viewing messages in hello queue you created.</span></span>  <span data-ttu-id="94cb1-136">Du kan ansluta tooyour storage-kö med Cloud Explorer i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="94cb1-136">You can connect tooyour storage queue by using Cloud Explorer in Visual Studio.</span></span> <span data-ttu-id="94cb1-137">Dock gör hello portal det enkelt tooconnect tooyour storage-konto med hjälp av Microsoft Azure Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="94cb1-137">However, hello portal makes it easy tooconnect tooyour storage account by using Microsoft Azure Storage Explorer.</span></span>

1. <span data-ttu-id="94cb1-138">I hello **integrera** klickar du på kön utdatabindning > **dokumentationen**, visa hello anslutningssträngen för ditt lagringskonto och kopiera hello värde.</span><span class="sxs-lookup"><span data-stu-id="94cb1-138">In hello **Integrate** tab, click your queue output binding > **Documentation**, then unhide hello Connection String for your storage account and copy hello value.</span></span> <span data-ttu-id="94cb1-139">Du kan använda det här värdet tooconnect tooyour lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="94cb1-139">You use this value tooconnect tooyour storage account.</span></span>

    ![Hämta Azure Storage Explorer](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab3.png)


2. <span data-ttu-id="94cb1-141">Om du inte redan har gjort det laddar du ned och installerar [Microsoft Azure Storage Explorer](http://storageexplorer.com).</span><span class="sxs-lookup"><span data-stu-id="94cb1-141">If you haven't already done so, download and install [Microsoft Azure Storage Explorer](http://storageexplorer.com).</span></span> 
 
3. <span data-ttu-id="94cb1-142">I Lagringsutforskaren, klickar du på hello ansluta tooAzure ikon för serverlagring, klistra in hello anslutningssträngen i hello fältet och slutför guiden hello.</span><span class="sxs-lookup"><span data-stu-id="94cb1-142">In Storage Explorer, click hello connect tooAzure Storage icon, paste hello connection string in hello field, and complete hello wizard.</span></span>

    ![Lägga till en anslutning i Storage Explorer](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-storage-explorer.png)

4. <span data-ttu-id="94cb1-144">Under **lokala och anslutna**, expandera **Lagringskonton** > ditt lagringskonto > **köer** > **funktioner-bindningar**och kontrollera att meddelanden skrivs toohello kön.</span><span class="sxs-lookup"><span data-stu-id="94cb1-144">Under **Local and attached**, expand **Storage Accounts** > your storage account > **Queues** > **functions-bindings** and verify that messages are written toohello queue.</span></span>

    ![Vy över meddelanden i kö för hello](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer.png)

    <span data-ttu-id="94cb1-146">Om hello kön finns inte eller är tom, är det förmodligen ett problem med din funktionsbindning eller kod.</span><span class="sxs-lookup"><span data-stu-id="94cb1-146">If hello queue does not exist or is empty, there is most likely a problem with your function binding or code.</span></span>

## <a name="create-a-function-that-reads-from-hello-queue"></a><span data-ttu-id="94cb1-147">Skapa en funktion som läser från hello kö</span><span class="sxs-lookup"><span data-stu-id="94cb1-147">Create a function that reads from hello queue</span></span>

<span data-ttu-id="94cb1-148">Nu när du har meddelanden som läggs till toohello kön, du kan skapa en annan funktion som läser från hello kön och skrivningar hello meddelanden permanent tooan Azure Storage-tabellen.</span><span class="sxs-lookup"><span data-stu-id="94cb1-148">Now that you have messages being added toohello queue, you can create another function that reads from hello queue and writes hello messages permanently tooan Azure Storage table.</span></span>

1. <span data-ttu-id="94cb1-149">Klicka på **Ny funktion** > **QueueTrigger-CSharp**.</span><span class="sxs-lookup"><span data-stu-id="94cb1-149">Click **New Function** > **QueueTrigger-CSharp**.</span></span> 
 
2. <span data-ttu-id="94cb1-150">Namnge hello funktionen `FunctionsBindingsDemo2`, ange **funktioner bindningar** i hello **könamnet** fält, Välj ett befintligt lagringskonto eller skapa ett och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="94cb1-150">Name hello function `FunctionsBindingsDemo2`, enter **functions-bindings** in hello **Queue name** field, select an existing storage account or create one, and then click **Create**.</span></span>

    ![Lägg till en timer-funktion för utdatakö](./media/functions-create-an-azure-connected-function/function-demo2-new-function.png) 

3. <span data-ttu-id="94cb1-152">(Valfritt) Du kan kontrollera att nya hello-funktionen fungerar genom att visa hello ny kö i Lagringsutforskaren som innan.</span><span class="sxs-lookup"><span data-stu-id="94cb1-152">(Optional) You can verify that hello new function works by viewing hello new queue in Storage Explorer as before.</span></span> <span data-ttu-id="94cb1-153">Du kan också använda Cloud Explorer i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="94cb1-153">You can also use Cloud Explorer in Visual Studio.</span></span>  

4. <span data-ttu-id="94cb1-154">(Valfritt) Uppdatera hello **funktioner bindningar** kö och Lägg märke till att objekt har tagits bort från hello kö.</span><span class="sxs-lookup"><span data-stu-id="94cb1-154">(Optional) Refresh hello **functions-bindings** queue and notice that items have been removed from hello queue.</span></span> <span data-ttu-id="94cb1-155">hello borttagning beror hello-funktionen är bundna toohello **funktioner bindningar** kö som en inkommande utlösare och hello funktion läser hello kön.</span><span class="sxs-lookup"><span data-stu-id="94cb1-155">hello removal occurs because hello function is bound toohello **functions-bindings** queue as an input trigger and hello function reads hello queue.</span></span> 
 
## <a name="add-a-table-output-binding"></a><span data-ttu-id="94cb1-156">Lägga till en utdatabindningstabell</span><span class="sxs-lookup"><span data-stu-id="94cb1-156">Add a table output binding</span></span>

1. <span data-ttu-id="94cb1-157">I FunctionsBindingsDemo2 klickar du på **Integrera** > **Nya utdata** > **Azure Table Storage** > **Välj**.</span><span class="sxs-lookup"><span data-stu-id="94cb1-157">In FunctionsBindingsDemo2, click **Integrate** > **New Output** > **Azure Table Storage** > **Select**.</span></span>

    ![Lägg till en bindning tooan Azure Storage-tabell](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab.png) 

2. <span data-ttu-id="94cb1-159">Skriv `functionbindings` som **Tabellnamn** och `myTable` som **Tabellparameternamn**, välj en **Lagringskontoanslutning** eller skapa en ny, och klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="94cb1-159">Enter `functionbindings` for **Table name** and `myTable` for **Table parameter name**, choose a **Storage account connection** or create a new one, and then click **Save**.</span></span>

    ![Konfigurera hello tabellbindning för lagring](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab2.png)
   
3. <span data-ttu-id="94cb1-161">I hello **utveckla** fliken ersätter hello befintliga Funktionskoden med hello följande:</span><span class="sxs-lookup"><span data-stu-id="94cb1-161">In hello **Develop** tab, replace hello existing function code with hello following:</span></span>
   
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
        
        // Add hello item toohello table binding collection.
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
    <span data-ttu-id="94cb1-162">Hej **TableItem** klassen representerar en rad i tabellen för lagring av hello och du lägger till hello objektet toohello `myTable` samling **TableItem** objekt.</span><span class="sxs-lookup"><span data-stu-id="94cb1-162">hello **TableItem** class represents a row in hello storage table, and you add hello item toohello `myTable` collection of **TableItem** objects.</span></span> <span data-ttu-id="94cb1-163">Du måste ange hello **PartitionKey** och **RowKey** egenskaper toobe kan tooinsert i hello-tabellen.</span><span class="sxs-lookup"><span data-stu-id="94cb1-163">You must set hello **PartitionKey** and **RowKey** properties toobe able tooinsert into hello table.</span></span>

4. <span data-ttu-id="94cb1-164">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="94cb1-164">Click **Save**.</span></span>  <span data-ttu-id="94cb1-165">Slutligen kan du kontrollera hello funktionen fungerar genom att visa hello tabellen i Lagringsutforskaren eller Visual Studio Cloud Explorer.</span><span class="sxs-lookup"><span data-stu-id="94cb1-165">Finally, you can verify hello function works by viewing hello table in Storage explorer or Visual Studio Cloud Explorer.</span></span>

5. <span data-ttu-id="94cb1-166">(Valfritt) Expandera i ditt lagringskonto i Lagringsutforskaren **tabeller** > **functionsbindings** och verifiera att rader läggs toohello tabell.</span><span class="sxs-lookup"><span data-stu-id="94cb1-166">(Optional) In your storage account in Storage Explorer, expand **Tables** > **functionsbindings** and verify that rows are added toohello table.</span></span> <span data-ttu-id="94cb1-167">Du kan göra hello samma i Cloud Explorer i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="94cb1-167">You can do hello same in Cloud Explorer in Visual Studio.</span></span>

    ![Vy av rader i tabellen hello](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer2.png)

    <span data-ttu-id="94cb1-169">Om hello tabellen finns inte eller är tom, är det förmodligen ett problem med din funktionsbindning eller kod.</span><span class="sxs-lookup"><span data-stu-id="94cb1-169">If hello table does not exist or is empty, there is most likely a problem with your function binding or code.</span></span> 
 
[!INCLUDE [More binding information](../../includes/functions-bindings-next-steps.md)]

## <a name="next-steps"></a><span data-ttu-id="94cb1-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="94cb1-170">Next steps</span></span>
<span data-ttu-id="94cb1-171">Mer information om Azure Functions finns hello följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="94cb1-171">For more information about Azure Functions, see hello following topics:</span></span>

* [<span data-ttu-id="94cb1-172">Azure Functions, info för utvecklare</span><span class="sxs-lookup"><span data-stu-id="94cb1-172">Azure Functions developer reference</span></span>](functions-reference.md)  
  <span data-ttu-id="94cb1-173">Info för programmerare om att koda funktioner och definiera utlösare och bindningar.</span><span class="sxs-lookup"><span data-stu-id="94cb1-173">Programmer reference for coding functions and defining triggers and bindings.</span></span>
* [<span data-ttu-id="94cb1-174">Testa Azure Functions</span><span class="sxs-lookup"><span data-stu-id="94cb1-174">Testing Azure Functions</span></span>](functions-test-a-function.md)  
  <span data-ttu-id="94cb1-175">Beskriver olika verktyg och tekniker för att testa funktioner.</span><span class="sxs-lookup"><span data-stu-id="94cb1-175">Describes various tools and techniques for testing your functions.</span></span>
* [<span data-ttu-id="94cb1-176">Hur tooscale Azure Functions</span><span class="sxs-lookup"><span data-stu-id="94cb1-176">How tooscale Azure Functions</span></span>](functions-scale.md)  
  <span data-ttu-id="94cb1-177">Beskriver tillgängliga med Azure Functions, inklusive hello förbrukning värd planerings- och hur toochoose hello rätt plan serviceplaner.</span><span class="sxs-lookup"><span data-stu-id="94cb1-177">Discusses service plans available with Azure Functions, including hello Consumption hosting plan, and how toochoose hello right plan.</span></span> 

[!INCLUDE [Getting help note](../../includes/functions-get-help.md)]

