---
title: "aaaCreate en funktion i Azure som utlöses av en allmän webhook | Microsoft Docs"
description: "Använd Azure Functions toocreate en serverlösa funktion som anropas av en webhook i Azure."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: fafc10c0-84da-4404-b4fa-eea03c7bf2b1
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/12/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 0a4868da91d216c8d20930ce7ec82eaa059c75ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-triggered-by-a-generic-webhook"></a>Skapa en funktion som utlöses av en allmän webhook

Azure Functions kan du köra din kod i en miljö med serverlösa utan toofirst skapa en virtuell dator eller publicera ett webbprogram. Du kan till exempel konfigurera en funktion toobe som utlöses av en avisering som skapats av Azure-Monitor. Det här avsnittet visar hur tooexecute C#-kod när en resursgrupp är läggs tooyour prenumeration.   

![Allmän webhook aktiveras funktionen i hello Azure-portalen](./media/functions-create-generic-webhook-triggered-function/function-completed.png)

## <a name="prerequisites"></a>Krav 

toocomplete den här kursen:

+ Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Skapa en Azure Functions-app

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Därefter skapar du en funktion i hello ny funktionsapp.

## <a name="create-function"></a>Skapa en allmän webhook utlöses-funktion

1. Expandera funktionen appen och klicka på hello  **+**  knappen för nästa**funktioner**. Om den här funktionen är hello förstnämnda i funktionen appen, Välj **anpassad funktionen**. Detta visar hello fullständig uppsättning funktionen mallar.

    ![Funktioner quickstart sida i hello Azure-portalen](./media/functions-create-generic-webhook-triggered-function/add-first-function.png)

2. Välj hello **allmän WebHook - C#** mall. Skriv ett namn för ditt C#-funktionen och sedan välja **skapa**.

     ![Skapa en allmän webhook utlöses funktion i hello Azure-portalen](./media/functions-create-generic-webhook-triggered-function/functions-create-generic-webhook-trigger.png) 

2. Klicka på den nya funktionen **<> / Get funktions-URL**, kopiera och spara hello värde. Du kan använda det här värdet tooconfigure hello webhooken. 

    ![Granska hello Funktionskoden](./media/functions-create-generic-webhook-triggered-function/functions-copy-function-url.png)
         
Därefter skapar du en webhook-slutpunkt i en aktivitet loggen avisering i Azure-Monitor. 

## <a name="create-an-activity-log-alert"></a>Skapa en aktivitet loggen avisering

1. I hello Azure portal, navigerar toohello **övervakaren** tjänsten, Välj **aviseringar**, och klicka på **Lägg till aktivitet loggen avisering**.   

    ![Övervaka](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert.png)

2. Använda hello inställningar som anges i hello tabell:

    ![Skapa en aktivitet loggen avisering](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert-settings.png)

    | Inställning      |  Föreslaget värde   | Beskrivning                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Aktiviteten loggnamn för avisering** | resurs-grupp-skapa-avisering | Namnet på loggen varning hello. |
    | **Prenumeration** | Din prenumeration | hello-prenumeration som du använder för den här kursen. | 
    |  **Resursgrupp** | myResourceGroup | hello resursgrupp som hello avisering resurser har distribuerats till. Med hjälp av hello samma resursgrupp som appen funktionen gör det enklare tooclean när du har slutfört hello kursen. |
    | **Händelsekategori** | Administrativa | Den här kategorin innehåller ändringar som gjorts tooAzure resurser.  |
    | **Resurstyp** | Resursgrupper | Filtrerar aviseringar tooresource gruppaktiviteter. |
    | **Resursgrupp**<br/>och **resurs** | Alla | Övervaka alla resurser. |
    | **Åtgärdsnamn** | Skapa resursgrupp | Filtrerar aviseringar toocreate åtgärder. |
    | **Nivå** | Information | Omfatta nivå informationsaviseringar. | 
    | **Status** | Lyckades | Filtrerar tooactions för aviseringar som har slutförts. |
    | **Grupp** | Ny | Skapa en ny åtgärdsgrupp, som definierar hello åtgärden tar när en avisering utlöses. |
    | **Åtgärden gruppnamn** | funktionen webhooken | En tooidentify hello åtgärd gruppnamnet.  | 
    | **Kort namn** | funcwebhook | Ett kort namn för hello grupp. |  

3. I **åtgärder**, lägga till en åtgärd med hello inställningar som anges i hello tabell: 

    ![Lägg till en grupp](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert-settings-2.png)

    | Inställning      |  Föreslaget värde   | Beskrivning                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Namn** | CallFunctionWebhook | Ett namn för hello åtgärden. |
    | **Åtgärdstyp** | Webhook | hello svar toohello aviseringen är att en Webhooksadressen anropas. |
    | **Detaljer** | Funktions-URL | Klistra in i hello Webhooksadressen för hello-funktion som du kopierade tidigare. |v

4. Klicka på **OK** toocreate hello aviseringen och åtgärden grupp.  

Hej webhook kallas nu när gruppen skapas i din prenumeration. Därefter uppdaterar du hello kod i din funktion toohandle hello JSON loggdata i hello brödtext hello-begäran.   

## <a name="update-hello-function-code"></a>Uppdatera Funktionskoden hello

1. Gå tillbaka tooyour funktionsapp hello-portalen och expandera din funktion. 

2. Ersätt hello C# skriptkod i hello-funktionen i hello portal med hello följande kod:

    ```csharp
    #r "Newtonsoft.Json"
    
    using System;
    using System.Net;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
    {
        log.Info($"Webhook was triggered!");
    
        // Get hello activityLog object from hello JSON in hello message body.
        string jsonContent = await req.Content.ReadAsStringAsync();
        JToken activityLog = JObject.Parse(jsonContent.ToString())
            .SelectToken("data.context.activityLog");
    
        // Return an error if hello resource in hello activity log isn't a resource group. 
        if (activityLog == null || !string.Equals((string)activityLog["resourceType"], 
            "Microsoft.Resources/subscriptions/resourcegroups"))
        {
            log.Error("An error occured");
            return req.CreateResponse(HttpStatusCode.BadRequest, new
            {
                error = "Unexpected message payload or wrong alert received."
            });
        }
    
        // Write information about hello created resource group toohello streaming log.
        log.Info(string.Format("Resource group '{0}' was {1} on {2}.",
            (string)activityLog["resourceGroupName"],
            ((string)activityLog["subStatus"]).ToLower(), 
            (DateTime)activityLog["submissionTimestamp"]));
    
        return req.CreateResponse(HttpStatusCode.OK);    
    }
    ```

Nu kan du testa hello funktionen genom att skapa en ny resursgrupp i din prenumeration.

## <a name="test-hello-function"></a>Testa hello-funktionen

1. Klickar du på ikonen hello resurs grupp hello till vänster i hello Azure portal, Välj **+ Lägg till**, ange ett **resursgruppnamn**, och välj **skapa** toocreate en tom resursgrupp.
    
    ![Skapa en resursgrupp.](./media/functions-create-generic-webhook-triggered-function/functions-create-resource-group.png)

2. Gå tillbaka tooyour och expandera hello **loggar** fönster. När du har skapat hello resursgruppen hello aktivitet loggen avisering utlösare hello webhook och hello funktionen körs. Du kan se hello namn hello ny resursgrupp skrivs toohello loggar.  

    ![Lägga till en programinställning för testet.](./media/functions-create-generic-webhook-triggered-function/function-view-logs.png)

3. (Valfritt) Gå tillbaka och ta bort hello resursgruppen som du skapade. Observera att den här aktiviteten inte utlöses hello-funktionen. Detta beror på att ta bort operations filtreras bort av hello aviseringen. 

## <a name="clean-up-resources"></a>Rensa resurser

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nästa steg

Du har skapat en funktion som körs när en begäran tas emot från en allmän webhook. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Mer information om webhook-utlösare finns i [Azure Functions HTTP och webhook-bindningar](functions-bindings-http-webhook.md). toolearn mer information om hur du utvecklar funktioner i C#, se [Azure Functions C# skript för utvecklare](functions-reference-csharp.md).

