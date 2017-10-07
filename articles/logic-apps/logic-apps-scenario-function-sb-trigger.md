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
# <a name="scenario-trigger-a-logic-app-with-azure-functions-and-azure-service-bus"></a><span data-ttu-id="48a87-103">Scenario: Utlösa en logikapp med Azure Functions och Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="48a87-103">Scenario: Trigger a logic app with Azure Functions and Azure Service Bus</span></span>

<span data-ttu-id="48a87-104">Du kan använda Azure Functions toocreate en utlösare för en logikapp när du behöver toodeploy en tidskrävande lyssnare eller aktivitet.</span><span class="sxs-lookup"><span data-stu-id="48a87-104">You can use Azure Functions toocreate a trigger for a logic app when you need toodeploy a long-running listener or task.</span></span> <span data-ttu-id="48a87-105">Du kan till exempel skapa en funktion som avlyssnar en kö och omedelbart eller en logikapp som en push-utlösare.</span><span class="sxs-lookup"><span data-stu-id="48a87-105">For example, you can create a function that listens in on a queue and then immediately fire a logic app as a push trigger.</span></span>

## <a name="build-hello-logic-app"></a><span data-ttu-id="48a87-106">Skapa hello logikapp</span><span class="sxs-lookup"><span data-stu-id="48a87-106">Build hello logic app</span></span>
<span data-ttu-id="48a87-107">I det här exemplet har en funktion som körs för varje logikappen som behöver toobe utlöses.</span><span class="sxs-lookup"><span data-stu-id="48a87-107">In this example, you have a function running for each logic app that needs toobe triggered.</span></span> <span data-ttu-id="48a87-108">Först skapa en logikapp som har en utlösare för HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="48a87-108">First, create a logic app that has an HTTP request trigger.</span></span> <span data-ttu-id="48a87-109">hello anropar denna slutpunkt när ett kömeddelande tas emot.</span><span class="sxs-lookup"><span data-stu-id="48a87-109">hello function calls that endpoint whenever a queue message is received.</span></span>  

1. <span data-ttu-id="48a87-110">Skapa en logikapp.</span><span class="sxs-lookup"><span data-stu-id="48a87-110">Create a logic app.</span></span>
2. <span data-ttu-id="48a87-111">Välj hello **manuellt - när en HTTP-begäran tas emot** utlösare.</span><span class="sxs-lookup"><span data-stu-id="48a87-111">Select hello **Manual - When an HTTP Request is Received** trigger.</span></span>
   <span data-ttu-id="48a87-112">Alternativt kan du ange toouse en JSON-schema med hälsningsmeddelande för kön med hjälp av ett verktyg som [jsonschema.net](http://jsonschema.net).</span><span class="sxs-lookup"><span data-stu-id="48a87-112">Optionally, you can specify a JSON schema toouse with hello queue message by using a tool like [jsonschema.net](http://jsonschema.net).</span></span> <span data-ttu-id="48a87-113">Klistra in hello schemat i hello utlösaren.</span><span class="sxs-lookup"><span data-stu-id="48a87-113">Paste hello schema in hello trigger.</span></span> <span data-ttu-id="48a87-114">Scheman hjälpa hello designer förstå hello form av hello data och flödar egenskaper enklare via hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="48a87-114">Schemas help hello designer understand hello shape of hello data and flow properties more easily through hello workflow.</span></span>
2. <span data-ttu-id="48a87-115">Lägg till eventuella ytterligare steg som du vill toooccur när ett meddelande i kön har tagits emot.</span><span class="sxs-lookup"><span data-stu-id="48a87-115">Add any additional steps that you want toooccur after a queue message is received.</span></span> <span data-ttu-id="48a87-116">Till exempel skicka ett e-postmeddelande via Office 365.</span><span class="sxs-lookup"><span data-stu-id="48a87-116">For example, send an email via Office 365.</span></span>  
3. <span data-ttu-id="48a87-117">Spara hello logik toogenerate hello återanrop Webbadress för hello utlösaren toothis logikapp.</span><span class="sxs-lookup"><span data-stu-id="48a87-117">Save hello logic app toogenerate hello callback URL for hello trigger toothis logic app.</span></span> <span data-ttu-id="48a87-118">hello URL visas på hello utlösaren kort.</span><span class="sxs-lookup"><span data-stu-id="48a87-118">hello URL appears on hello trigger card.</span></span>

![hello återanrop URL visas på hello utlösaren-kort][1]

## <a name="build-hello-function"></a><span data-ttu-id="48a87-120">Skapa hello-funktion</span><span class="sxs-lookup"><span data-stu-id="48a87-120">Build hello function</span></span>
<span data-ttu-id="48a87-121">Därefter måste skapa du en funktion som fungerar som hello utlösare och lyssnar toohello kön.</span><span class="sxs-lookup"><span data-stu-id="48a87-121">Next, you must create a function that acts as hello trigger and listens toohello queue.</span></span>

1. <span data-ttu-id="48a87-122">I hello [Azure Functions-portalen](https://functions.azure.com/signin)väljer **nya funktionen**, och välj sedan hello **ServiceBusQueueTrigger - C#** mall.</span><span class="sxs-lookup"><span data-stu-id="48a87-122">In hello [Azure Functions portal](https://functions.azure.com/signin), select **New Function**, and then select hello **ServiceBusQueueTrigger - C#** template.</span></span>
   
    ![Azure Functions-portalen][2]
2. <span data-ttu-id="48a87-124">Konfigurera hello anslutning toohello Service Bus-kö, som använder hello Azure Service Bus SDK `OnMessageReceive()` lyssnare.</span><span class="sxs-lookup"><span data-stu-id="48a87-124">Configure hello connection toohello Service Bus queue, which uses hello Azure Service Bus SDK `OnMessageReceive()` listener.</span></span>
3. <span data-ttu-id="48a87-125">Skriv en grundläggande funktion toocall hello logik app slutpunkt (som skapats tidigare) med hälsningsmeddelande för kön som en utlösare.</span><span class="sxs-lookup"><span data-stu-id="48a87-125">Write a basic function toocall hello logic app endpoint (created earlier) by using hello queue message as a trigger.</span></span> <span data-ttu-id="48a87-126">Här är ett fullständigt exempel på en funktion.</span><span class="sxs-lookup"><span data-stu-id="48a87-126">Here's a full example of a function.</span></span> <span data-ttu-id="48a87-127">hello exempel används en `application/json` innehållstyp för meddelandet, men du kan ändra den här typen efter behov.</span><span class="sxs-lookup"><span data-stu-id="48a87-127">hello example uses an `application/json` message content type, but you can change this type as necessary.</span></span>
   
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

<span data-ttu-id="48a87-128">tootest, lägga till ett kömeddelande via ett verktyg som [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer).</span><span class="sxs-lookup"><span data-stu-id="48a87-128">tootest, add a queue message via a tool like [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer).</span></span> <span data-ttu-id="48a87-129">Se hello logikapp eller omedelbart efter hello funktionen tar emot hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="48a87-129">See hello logic app fire immediately after hello function receives hello message.</span></span>

<!-- Image References -->
[1]: ./media/logic-apps-scenario-function-sb-trigger/manualtrigger.png
[2]: ./media/logic-apps-scenario-function-sb-trigger/newqueuetriggerfunction.png
