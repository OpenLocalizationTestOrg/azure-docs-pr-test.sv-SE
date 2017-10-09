---
title: "aaaCreate en funktion i Azure som utlösts av Kömeddelanden | Microsoft Docs"
description: "Använd Azure Functions toocreate en serverlösa funktion som anropas av en meddelanden som skickats tooan Azure Storage-kö."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 0b609bc0-c264-4092-8e3e-0784dcc23b5d
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/17/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 44db90fa80bf77e31bf53dddabd7136de5800b11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-messages-tooan-azure-storage-queue-using-functions"></a>Lägga till meddelanden tooan Azure Storage-kö med hjälp av funktioner

I Azure Functions ange bindningar för inkommande och utgående en deklarativ metod tooconnect tooexternal tjänstdata från din funktion. I det här avsnittet lär du dig hur tooupdate en befintlig funktion genom att lägga till utdata bindning som meddelanden skickas tooAzure Queue storage.  

![Visa meddelandet i hello loggar.](./media/functions-integrate-storage-queue-output-binding/functions-integrate-storage-binding-in-portal.png)

## <a name="prerequisites"></a>Krav 

[!INCLUDE [Previous topics](../../includes/functions-quickstart-previous-topics.md)]

* Installera hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/).

## <a name="add-binding"></a>Lägga till en utdatabindning
 
1. Expandera både din funktionsapp och din funktion.

2. Välj **integrera** och **+ nya utdata**, Välj **Azure Queue storage** och välj **Välj**.
    
    ![Lägga till en kö utdata bindning tooa lagringsfunktionen i hello Azure-portalen.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding.png)

3. Använda hello inställningar som anges i hello tabell: 

    ![Lägga till en kö utdata bindning tooa lagringsfunktionen i hello Azure-portalen.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding-2.png)

    | Inställning      |  Föreslaget värde   | Beskrivning                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Könamn**   | myqueue-items    | hello namnet på hello kö tooconnect tooin ditt lagringskonto. |
    | **Lagringskontoanslutning** | AzureWebJobStorage | Du kan använda hello konto lagringsanslutning redan används av din funktionsapp eller skapa en ny.  |
    | **Meddelandeparameternamn** | outputQueueItem | hello namnet på hello utdataparameter för bindningen. | 

4. Klicka på **spara** tooadd hello bindning.
 
Nu när du har en bindning för utdata som definierats måste tooupdate hello kod toouse hello bindning tooadd tooa kön.  

## <a name="update-hello-function-code"></a>Uppdatera Funktionskoden hello

1. Välj funktionen toodisplay hello Funktionskoden i hello-redigeraren. 

2. Uppdatera din funktionsdefinitionen enligt följande tooadd hello en C# funktion **outputQueueItem** bindningsparameter för lagring. Hoppa över det här steget om du använder en JavaScript-funktion.

    ```cs   
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, 
        ICollector<string> outputQueueItem, TraceWriter log)
    {
        ....
    }
    ```

3. Lägg till följande kod toohello funktionen precis innan hello-metoden returnerar hello. Använd hello lämpliga fragment för hello språket för din funktion.

    ```javascript
    context.bindings.outputQueueItem = "Name passed toohello function: " + 
                (req.query.name || req.body.name);
    ```

    ```cs
    outputQueueItem.Add("Name passed toohello function: " + name);     
    ```

4. Välj **spara** toosave ändringar.

hello-värdet som skickas toohello HTTP-utlösaren ingår i en meddelandekö tillagda toohello.
 
## <a name="test-hello-function"></a>Testa hello-funktionen 

1. När hello kodändringar sparas, Välj **kör**. 

    ![Lägga till en kö utdata bindning tooa lagringsfunktionen i hello Azure-portalen.](./media/functions-integrate-storage-queue-output-binding/functions-test-run-function.png)

2. Kontrollera hello loggar toomake till att hello funktionen lyckades. En ny kö med namnet **outqueue** skapas i ditt lagringskonto av hello Functions-runtime när hello utdatabindning används först.

Därefter kan du ansluta tooyour storage-konto tooverify hello ny kö och hello-meddelande som du har lagt till tooit. 

## <a name="connect-toohello-queue"></a>Ansluta toohello kön

Hoppa över hello först tre steg om du redan har installerat Lagringsutforskaren och anslutna den tooyour storage-konto.    

1. Välj i din funktion **integrera** och hello nya **Azure Queue storage** utdatabindning och expandera sedan **dokumentationen**. Kopiera både **kontonamnet** och **kontonyckeln**. Du använder dessa autentiseringsuppgifter tooconnect toohello storage-konto.
 
    ![Hämta hello Storage-konto med autentiseringsuppgifter för anslutning.](./media/functions-integrate-storage-queue-output-binding/function-get-storage-account-credentials.png)

2. Kör hello [Microsoft Azure Lagringsutforskaren](http://storageexplorer.com/) verktyget, Välj hello ansluta ikonen hello vänster, Välj **använder ett lagringskontonamn och nyckel**, och välj **nästa**.

    ![Köra hello Lagringsutforskaren för kontot.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-1.png)
    
3. Klistra in hello **kontonamn** och **kontonyckel** från steg 1 till sina motsvarande fält och välj sedan **nästa**, och **Anslut**. 
  
    ![Klistra in hello lagring autentiseringsuppgifter och ansluta.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-2.png)

4. Expandera hello ansluten lagringskontot, expandera **köer** och verifiera att en kö med namnet **MinKö objekt** finns. Du bör också se ett meddelande redan i hello kö.  
 
    ![Skapa en lagringskö.](./media/functions-integrate-storage-queue-output-binding/function-queue-storage-output-view-queue.png)
 

## <a name="clean-up-resources"></a>Rensa resurser

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nästa steg

Du har lagt till en befintlig funktion för utdata bindning tooan. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Mer information om bindningen tooQueue lagring finns [Azure Functions Storage kön bindningar](functions-bindings-storage-queue.md). 



