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
# <a name="use-azure-functions-toocreate-a-function-that-connects-tooother-azure-services"></a>Använd Azure Functions toocreate en funktion som ansluter tooother Azure services

Det här avsnittet visar hur toocreate en funktion i Azure-funktioner som lyssnar toomessages på ett Azure Storage-kö och kopior hello meddelanden toorows i en tabell i Azure Storage. En som utlöste timerfunktion är används tooload meddelanden i kö för hello. En annan funktion läser från hello kön och skriver toohello tabell. Både hello kön och hello tabell skapas du Azure Functions hello bindning definitionerna. 

mer intressant toomake saker, en funktion är skriven i JavaScript och hello andra är skriven i C#-skript. Detta visar hur en funktionsapp kan ha funktioner på olika språk. 

Det här scenariot visas i en [video på Channel 9](https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player).

## <a name="create-a-function-that-writes-toohello-queue"></a>Skapa en funktion som skriver toohello kön

Innan du kan ansluta tooa storage-kö måste toocreate en funktion som läser in hello meddelandekön. Den här JavaScript-funktionen använder en timer som utlösare som skriver en meddelandekö toohello var 10: e sekund. Om du inte redan har ett Azure-konto, kolla hello [försök Azure Functions](https://functions.azure.com/try) uppstår eller [skapa din kostnadsfria Azure-konto](https://azure.microsoft.com/free/).

1. Gå toohello Azure-portalen och leta upp din funktionsapp.

2. Klicka på **Ny funktion** > **TimerTrigger-JavaScript**. 

3. Namnge hello funktionen **FunctionsBindingsDemo1**, anger du cron uttrycksvärdet `0/10 * * * * *` för **schema**, och klicka sedan på **skapa**.
   
    ![Skapa en timerutlöst funktion](./media/functions-create-an-azure-connected-function/new-trigger-timer-function.png)

    Du har nu skapat en timerutlöst funktion som körs var 10:e sekund.

5. På hello **utveckla** klickar du på **loggar** och visa hello aktivitet i hello-loggen. Du ser att en loggpost skrivs var 10: e sekund.
   
    ![Visa hello loggen tooverify hello funktionen fungerar](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-view-log.png)

## <a name="add-a-message-queue-output-binding"></a>Lägga till en meddelandekö utan utdatabindning

1. På hello **integrera** , Välj **nya utdata** > **Azure Queue Storage** > **Välj**.

    ![Lägga till en utlösande timer-funktion](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab.png)

2. Ange `myQueueItem` för **meddelandet parameternamnet** och `functions-bindings` för **könamnet**, Välj en befintlig **konto lagringsanslutning** eller klicka på **nya** toocreate en anslutning-kontot och klicka sedan på **spara**.  

    ![Skapa hello bindning toohello lagring utdatakön](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab2.png)

1. Tillbaka i hello **utveckla** fliken, Lägg till följande kod toohello funktionen hello:
   
    ```javascript
   
    function myQueueItem() 
    {
        return {
            msg: "some message goes here",
            time: "time goes here"
        }
    }
   
    ```
2. Leta upp hello *om* instruktionen runt rad 9 för hello-funktionen och infoga hello följande kod efter denna instruktion.
   
    ```javascript
   
    var toBeQed = myQueueItem();
    toBeQed.time = timeStamp;
    context.bindings.myQueueItem = toBeQed;
   
    ```  
   
    Den här koden skapar en **myQueueItem** och ställer in dess **tid** egenskapen toohello aktuell tidsstämpel. Lägger sedan till hello ny kö objektet toohello kontexts **myQueueItem** bindning.

3. Klicka på **Spara och kör**.

## <a name="view-storage-updates-by-using-storage-explorer"></a>Visa lagringsuppdateringar med Storage Explorer
Du kan kontrollera att funktionen fungerar genom att visa meddelanden i hello kön som du skapade.  Du kan ansluta tooyour storage-kö med Cloud Explorer i Visual Studio. Dock gör hello portal det enkelt tooconnect tooyour storage-konto med hjälp av Microsoft Azure Lagringsutforskaren.

1. I hello **integrera** klickar du på kön utdatabindning > **dokumentationen**, visa hello anslutningssträngen för ditt lagringskonto och kopiera hello värde. Du kan använda det här värdet tooconnect tooyour lagringskontot.

    ![Hämta Azure Storage Explorer](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab3.png)


2. Om du inte redan har gjort det laddar du ned och installerar [Microsoft Azure Storage Explorer](http://storageexplorer.com). 
 
3. I Lagringsutforskaren, klickar du på hello ansluta tooAzure ikon för serverlagring, klistra in hello anslutningssträngen i hello fältet och slutför guiden hello.

    ![Lägga till en anslutning i Storage Explorer](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-storage-explorer.png)

4. Under **lokala och anslutna**, expandera **Lagringskonton** > ditt lagringskonto > **köer** > **funktioner-bindningar**och kontrollera att meddelanden skrivs toohello kön.

    ![Vy över meddelanden i kö för hello](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer.png)

    Om hello kön finns inte eller är tom, är det förmodligen ett problem med din funktionsbindning eller kod.

## <a name="create-a-function-that-reads-from-hello-queue"></a>Skapa en funktion som läser från hello kö

Nu när du har meddelanden som läggs till toohello kön, du kan skapa en annan funktion som läser från hello kön och skrivningar hello meddelanden permanent tooan Azure Storage-tabellen.

1. Klicka på **Ny funktion** > **QueueTrigger-CSharp**. 
 
2. Namnge hello funktionen `FunctionsBindingsDemo2`, ange **funktioner bindningar** i hello **könamnet** fält, Välj ett befintligt lagringskonto eller skapa ett och klicka på **skapa**.

    ![Lägg till en timer-funktion för utdatakö](./media/functions-create-an-azure-connected-function/function-demo2-new-function.png) 

3. (Valfritt) Du kan kontrollera att nya hello-funktionen fungerar genom att visa hello ny kö i Lagringsutforskaren som innan. Du kan också använda Cloud Explorer i Visual Studio.  

4. (Valfritt) Uppdatera hello **funktioner bindningar** kö och Lägg märke till att objekt har tagits bort från hello kö. hello borttagning beror hello-funktionen är bundna toohello **funktioner bindningar** kö som en inkommande utlösare och hello funktion läser hello kön. 
 
## <a name="add-a-table-output-binding"></a>Lägga till en utdatabindningstabell

1. I FunctionsBindingsDemo2 klickar du på **Integrera** > **Nya utdata** > **Azure Table Storage** > **Välj**.

    ![Lägg till en bindning tooan Azure Storage-tabell](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab.png) 

2. Skriv `functionbindings` som **Tabellnamn** och `myTable` som **Tabellparameternamn**, välj en **Lagringskontoanslutning** eller skapa en ny, och klicka på **Spara**.

    ![Konfigurera hello tabellbindning för lagring](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab2.png)
   
3. I hello **utveckla** fliken ersätter hello befintliga Funktionskoden med hello följande:
   
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
    Hej **TableItem** klassen representerar en rad i tabellen för lagring av hello och du lägger till hello objektet toohello `myTable` samling **TableItem** objekt. Du måste ange hello **PartitionKey** och **RowKey** egenskaper toobe kan tooinsert i hello-tabellen.

4. Klicka på **Spara**.  Slutligen kan du kontrollera hello funktionen fungerar genom att visa hello tabellen i Lagringsutforskaren eller Visual Studio Cloud Explorer.

5. (Valfritt) Expandera i ditt lagringskonto i Lagringsutforskaren **tabeller** > **functionsbindings** och verifiera att rader läggs toohello tabell. Du kan göra hello samma i Cloud Explorer i Visual Studio.

    ![Vy av rader i tabellen hello](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer2.png)

    Om hello tabellen finns inte eller är tom, är det förmodligen ett problem med din funktionsbindning eller kod. 
 
[!INCLUDE [More binding information](../../includes/functions-bindings-next-steps.md)]

## <a name="next-steps"></a>Nästa steg
Mer information om Azure Functions finns hello följande avsnitt:

* [Azure Functions, info för utvecklare](functions-reference.md)  
  Info för programmerare om att koda funktioner och definiera utlösare och bindningar.
* [Testa Azure Functions](functions-test-a-function.md)  
  Beskriver olika verktyg och tekniker för att testa funktioner.
* [Hur tooscale Azure Functions](functions-scale.md)  
  Beskriver tillgängliga med Azure Functions, inklusive hello förbrukning värd planerings- och hur toochoose hello rätt plan serviceplaner. 

[!INCLUDE [Getting help note](../../includes/functions-get-help.md)]

