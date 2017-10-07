---
title: "aaaScenario - utlösare logikappar med Azure Functions och Azure Service Bus | Microsoft Docs"
description: "Skapa en funktion tootrigger en logikapp med hjälp av Azure Functions och Azure Service Bus"
services: logic-apps,functions
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 19cbd921-7071-4221-ab86-b44d0fc0ecef
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/23/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: a7b78ebcfe492eee2e08ceeae6b9c5f8ed4717bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scenario-trigger-a-logic-app-with-azure-functions-and-azure-service-bus"></a>Scenario: Utlösa en logikapp med Azure Functions och Azure Service Bus

Du kan använda Azure Functions toocreate en utlösare för en logikapp när du behöver toodeploy en tidskrävande lyssnare eller aktivitet. Du kan till exempel skapa en funktion som avlyssnar en kö och omedelbart eller en logikapp som en push-utlösare.

## <a name="build-hello-logic-app"></a>Skapa hello logikapp
I det här exemplet har en funktion som körs för varje logikappen som behöver toobe utlöses. Först skapa en logikapp som har en utlösare för HTTP-begäran. hello anropar denna slutpunkt när ett kömeddelande tas emot.  

1. Skapa en logikapp.
2. Välj hello **manuellt - när en HTTP-begäran tas emot** utlösare.
   Alternativt kan du ange toouse en JSON-schema med hälsningsmeddelande för kön med hjälp av ett verktyg som [jsonschema.net](http://jsonschema.net). Klistra in hello schemat i hello utlösaren. Scheman hjälpa hello designer förstå hello form av hello data och flödar egenskaper enklare via hello arbetsflöde.
2. Lägg till eventuella ytterligare steg som du vill toooccur när ett meddelande i kön har tagits emot. Till exempel skicka ett e-postmeddelande via Office 365.  
3. Spara hello logik toogenerate hello återanrop Webbadress för hello utlösaren toothis logikapp. hello URL visas på hello utlösaren kort.

![hello återanrop URL visas på hello utlösaren-kort][1]

## <a name="build-hello-function"></a>Skapa hello-funktion
Därefter måste skapa du en funktion som fungerar som hello utlösare och lyssnar toohello kön.

1. I hello [Azure Functions-portalen](https://functions.azure.com/signin)väljer **nya funktionen**, och välj sedan hello **ServiceBusQueueTrigger - C#** mall.
   
    ![Azure Functions-portalen][2]
2. Konfigurera hello anslutning toohello Service Bus-kö, som använder hello Azure Service Bus SDK `OnMessageReceive()` lyssnare.
3. Skriv en grundläggande funktion toocall hello logik app slutpunkt (som skapats tidigare) med hälsningsmeddelande för kön som en utlösare. Här är ett fullständigt exempel på en funktion. hello exempel används en `application/json` innehållstyp för meddelandet, men du kan ändra den här typen efter behov.
   
   ```
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;
   
   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";
   
   public static void Run(string myQueueItem, TraceWriter log)
   {
   
       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       using (var client = new HttpClient())
       {
           var response = client.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
       }
   }
   ```

tootest, lägga till ett kömeddelande via ett verktyg som [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer). Se hello logikapp eller omedelbart efter hello funktionen tar emot hello-meddelande.

<!-- Image References -->
[1]: ./media/logic-apps-scenario-function-sb-trigger/manualtrigger.png
[2]: ./media/logic-apps-scenario-function-sb-trigger/newqueuetriggerfunction.png
