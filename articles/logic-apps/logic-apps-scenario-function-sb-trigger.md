---
title: "Scenario - utlösaren logikappar med Azure Functions och Azure Service Bus | Microsoft Docs"
description: "Skapa en funktion för att utlösa en logikapp med hjälp av Azure Functions och Azure Service Bus"
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
ms.openlocfilehash: 088f10bc32dd492f82f0a10a7e5829e76f588758
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="scenario-trigger-a-logic-app-with-azure-functions-and-azure-service-bus"></a><span data-ttu-id="38dab-103">Scenario: Utlösa en logikapp med Azure Functions och Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="38dab-103">Scenario: Trigger a logic app with Azure Functions and Azure Service Bus</span></span>

<span data-ttu-id="38dab-104">Du kan använda Azure Functions för att skapa en utlösare för en logikapp när du behöver distribuera en tidskrävande lyssnare eller aktivitet.</span><span class="sxs-lookup"><span data-stu-id="38dab-104">You can use Azure Functions to create a trigger for a logic app when you need to deploy a long-running listener or task.</span></span> <span data-ttu-id="38dab-105">Du kan till exempel skapa en funktion som avlyssnar en kö och omedelbart eller en logikapp som en push-utlösare.</span><span class="sxs-lookup"><span data-stu-id="38dab-105">For example, you can create a function that listens in on a queue and then immediately fire a logic app as a push trigger.</span></span>

## <a name="build-the-logic-app"></a><span data-ttu-id="38dab-106">Skapa logikappen</span><span class="sxs-lookup"><span data-stu-id="38dab-106">Build the logic app</span></span>
<span data-ttu-id="38dab-107">I det här exemplet har en funktion som körs för varje logikappen som måste aktiveras.</span><span class="sxs-lookup"><span data-stu-id="38dab-107">In this example, you have a function running for each logic app that needs to be triggered.</span></span> <span data-ttu-id="38dab-108">Först skapa en logikapp som har en utlösare för HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="38dab-108">First, create a logic app that has an HTTP request trigger.</span></span> <span data-ttu-id="38dab-109">Funktionen anropar denna slutpunkt när ett kömeddelande tas emot.</span><span class="sxs-lookup"><span data-stu-id="38dab-109">The function calls that endpoint whenever a queue message is received.</span></span>  

1. <span data-ttu-id="38dab-110">Skapa en logikapp.</span><span class="sxs-lookup"><span data-stu-id="38dab-110">Create a logic app.</span></span>
2. <span data-ttu-id="38dab-111">Välj den **manuellt - när en HTTP-begäran tas emot** utlösare.</span><span class="sxs-lookup"><span data-stu-id="38dab-111">Select the **Manual - When an HTTP Request is Received** trigger.</span></span>
   <span data-ttu-id="38dab-112">Alternativt kan du ange en JSON-schema för att använda kömeddelandet med hjälp av ett verktyg som [jsonschema.net](http://jsonschema.net).</span><span class="sxs-lookup"><span data-stu-id="38dab-112">Optionally, you can specify a JSON schema to use with the queue message by using a tool like [jsonschema.net](http://jsonschema.net).</span></span> <span data-ttu-id="38dab-113">Klistra in schemat i utlösaren.</span><span class="sxs-lookup"><span data-stu-id="38dab-113">Paste the schema in the trigger.</span></span> <span data-ttu-id="38dab-114">Scheman med hjälp av designern förstå formen av egenskaperna data och flödar enklare genom arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="38dab-114">Schemas help the designer understand the shape of the data and flow properties more easily through the workflow.</span></span>
2. <span data-ttu-id="38dab-115">Lägg till eventuella ytterligare steg som ska inträffa när ett meddelande i kön har tagits emot.</span><span class="sxs-lookup"><span data-stu-id="38dab-115">Add any additional steps that you want to occur after a queue message is received.</span></span> <span data-ttu-id="38dab-116">Till exempel skicka ett e-postmeddelande via Office 365.</span><span class="sxs-lookup"><span data-stu-id="38dab-116">For example, send an email via Office 365.</span></span>  
3. <span data-ttu-id="38dab-117">Spara logikapp för att generera återanrop-URL: en för utlösaren till den här logikapp.</span><span class="sxs-lookup"><span data-stu-id="38dab-117">Save the logic app to generate the callback URL for the trigger to this logic app.</span></span> <span data-ttu-id="38dab-118">URL-Adressen visas på kortet utlösare.</span><span class="sxs-lookup"><span data-stu-id="38dab-118">The URL appears on the trigger card.</span></span>

![Återanropet URL-Adressen visas på kortet utlösare][1]

## <a name="build-the-function"></a><span data-ttu-id="38dab-120">Skapa funktionen</span><span class="sxs-lookup"><span data-stu-id="38dab-120">Build the function</span></span>
<span data-ttu-id="38dab-121">Därefter måste skapa du en funktion som fungerar som utlösare och lyssnar till kön.</span><span class="sxs-lookup"><span data-stu-id="38dab-121">Next, you must create a function that acts as the trigger and listens to the queue.</span></span>

1. <span data-ttu-id="38dab-122">I den [Azure Functions-portalen](https://functions.azure.com/signin)väljer **nya funktionen**, och välj sedan den **ServiceBusQueueTrigger - C#** mall.</span><span class="sxs-lookup"><span data-stu-id="38dab-122">In the [Azure Functions portal](https://functions.azure.com/signin), select **New Function**, and then select the **ServiceBusQueueTrigger - C#** template.</span></span>
   
    ![Azure Functions-portalen][2]
2. <span data-ttu-id="38dab-124">Konfigurera anslutningen till Service Bus-kö som använder Azure Service Bus SDK `OnMessageReceive()` lyssnare.</span><span class="sxs-lookup"><span data-stu-id="38dab-124">Configure the connection to the Service Bus queue, which uses the Azure Service Bus SDK `OnMessageReceive()` listener.</span></span>
3. <span data-ttu-id="38dab-125">Skriva en grundläggande funktion för att anropa logik app slutpunkten (som skapats tidigare) med hjälp av kömeddelandet som en utlösare.</span><span class="sxs-lookup"><span data-stu-id="38dab-125">Write a basic function to call the logic app endpoint (created earlier) by using the queue message as a trigger.</span></span> <span data-ttu-id="38dab-126">Här är ett fullständigt exempel på en funktion.</span><span class="sxs-lookup"><span data-stu-id="38dab-126">Here's a full example of a function.</span></span> <span data-ttu-id="38dab-127">I exemplet används en `application/json` innehållstyp för meddelandet, men du kan ändra den här typen efter behov.</span><span class="sxs-lookup"><span data-stu-id="38dab-127">The example uses an `application/json` message content type, but you can change this type as necessary.</span></span>
   
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

<span data-ttu-id="38dab-128">Testa genom att lägga till ett kömeddelande via ett verktyg som [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer).</span><span class="sxs-lookup"><span data-stu-id="38dab-128">To test, add a queue message via a tool like [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer).</span></span> <span data-ttu-id="38dab-129">Se logikappen eller omedelbart efter funktionen tar emot meddelandet.</span><span class="sxs-lookup"><span data-stu-id="38dab-129">See the logic app fire immediately after the function receives the message.</span></span>

<!-- Image References -->
[1]: ./media/logic-apps-scenario-function-sb-trigger/manualtrigger.png
[2]: ./media/logic-apps-scenario-function-sb-trigger/newqueuetriggerfunction.png
