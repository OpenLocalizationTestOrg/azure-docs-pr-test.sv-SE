---
title: "Anpassad kod för Logikappar i Azure med Azure Functions | Microsoft Docs"
description: "Skapa och köra anpassad kod för Logikappar i Azure med Azure Functions"
services: logic-apps,functions
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 9fab1050-cfbc-4a8b-b1b3-5531bee92856
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 18442c87b049200fac5ed41cc7034ba7a848b8d3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-and-run-custom-code-for-logic-apps-through-azure-functions"></a><span data-ttu-id="33bb9-103">Lägg till och köra anpassad kod för logic apps via Azure Functions</span><span class="sxs-lookup"><span data-stu-id="33bb9-103">Add and run custom code for logic apps through Azure Functions</span></span>

<span data-ttu-id="33bb9-104">Om du vill köra anpassade kodavsnitt för C# eller node.js i logikappar, kan du skapa anpassade funktioner via Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="33bb9-104">To run custom snippets of C# or node.js in logic apps, you can create custom functions through Azure Functions.</span></span> 
<span data-ttu-id="33bb9-105">[Azure Functions](../azure-functions/functions-overview.md) erbjuder server-dator i Microsoft Azure och är användbara för att utföra dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="33bb9-105">[Azure Functions](../azure-functions/functions-overview.md) offers server-free computing in Microsoft Azure and are useful for performing these tasks:</span></span>

* <span data-ttu-id="33bb9-106">Avancerad formatering eller beräknade fält i logikappar</span><span class="sxs-lookup"><span data-stu-id="33bb9-106">Advanced formatting or compute of fields in logic apps</span></span>
* <span data-ttu-id="33bb9-107">Utföra beräkningar i ett arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="33bb9-107">Perform calculations in a workflow.</span></span>
* <span data-ttu-id="33bb9-108">Utöka funktionerna för logik-app med funktioner som stöds i C# eller node.js</span><span class="sxs-lookup"><span data-stu-id="33bb9-108">Extend the logic app functionality with functions that are supported in C# or node.js</span></span>

## <a name="create-custom-functions-for-your-logic-apps"></a><span data-ttu-id="33bb9-109">Skapa anpassade funktioner för dina logic apps</span><span class="sxs-lookup"><span data-stu-id="33bb9-109">Create custom functions for your logic apps</span></span>

<span data-ttu-id="33bb9-110">Vi rekommenderar att du skapar en funktion i Azure Functions-portalen från den **allmän Webhook - nod** eller **allmän Webhook - C#** mallar.</span><span class="sxs-lookup"><span data-stu-id="33bb9-110">We recommend that you create a function in the Azure Functions portal, from the **Generic Webhook - Node** or **Generic Webhook - C#** templates.</span></span> <span data-ttu-id="33bb9-111">Resultatet skapas en auto-fylls i en mall som accepterar `application/json` från en logikapp.</span><span class="sxs-lookup"><span data-stu-id="33bb9-111">The result creates an auto-populated a template that accepts `application/json` from a logic app.</span></span> <span data-ttu-id="33bb9-112">Funktioner som du skapar från dessa mallar identifieras automatiskt och visas i designern logik appen under **Azure Functions i min region.**</span><span class="sxs-lookup"><span data-stu-id="33bb9-112">Functions that you create from these templates are automatically detected and appear in the Logic App Designer under **Azure Functions in my region.**</span></span>

<span data-ttu-id="33bb9-113">I Azure-portalen på den **integrera** fönstret för din funktion mallen ska visas som **läge** inställd på **Webhook** och **Webhook typen** är inställd på **allmänna JSON**.</span><span class="sxs-lookup"><span data-stu-id="33bb9-113">In the Azure portal, on the **Integrate** pane for your function, your template should show that **Mode** set to **Webhook** and **Webhook type** is set to **Generic JSON**.</span></span> 

<span data-ttu-id="33bb9-114">Webhook-funktioner acceptera en begäran och skicka det till metoden via en `data` variabel.</span><span class="sxs-lookup"><span data-stu-id="33bb9-114">Webhook functions accept a request and pass it into the method via a `data` variable.</span></span> <span data-ttu-id="33bb9-115">Du kan komma åt egenskaperna för din nyttolast med hjälp av punktnotation som `data.function-name`.</span><span class="sxs-lookup"><span data-stu-id="33bb9-115">You can access the properties of your payload by using dot notation like `data.function-name`.</span></span> <span data-ttu-id="33bb9-116">Till exempel en enkel JavaScript-funktion som konverterar ett DateTime-värde till en datumsträng ser ut som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="33bb9-116">For example, a simple JavaScript function that converts a DateTime value into a date string looks like the following example:</span></span>

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-logic-apps"></a><span data-ttu-id="33bb9-117">Anropa Azure Functions från logikappar</span><span class="sxs-lookup"><span data-stu-id="33bb9-117">Call Azure Functions from logic apps</span></span>

<span data-ttu-id="33bb9-118">För att lista behållare i din prenumeration och välj den funktion som du vill anropa i logik App Designer klickar du på den **åtgärder** -menyn och välj bland **Azure Functions i min Region**.</span><span class="sxs-lookup"><span data-stu-id="33bb9-118">To list the containers in your subscription and select the function that you want to call, in Logic App Designer, click the **Actions** menu, and select from **Azure Functions in my Region**.</span></span>

<span data-ttu-id="33bb9-119">När du har valt funktionen uppmanas du att ange ett inkommande nyttolast-objekt.</span><span class="sxs-lookup"><span data-stu-id="33bb9-119">After you select the function, you are asked to specify an input payload object.</span></span> <span data-ttu-id="33bb9-120">Det här objektet är meddelandet att logikappen skickas till funktionen och måste vara ett JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="33bb9-120">This object is the message that the logic app sends to the function and must be a JSON object.</span></span> <span data-ttu-id="33bb9-121">Om du vill skicka i till exempel den **ändrades senast** datum från en Salesforce-utlösare funktionen nyttolasten kan se ut som i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="33bb9-121">For example, if you want to pass in the **Last Modified** date from a Salesforce trigger, the function payload might look like this example:</span></span>

![Senast ändrad datum][1]

## <a name="trigger-logic-apps-from-a-function"></a><span data-ttu-id="33bb9-123">Utlösaren logikappar från en funktion</span><span class="sxs-lookup"><span data-stu-id="33bb9-123">Trigger logic apps from a function</span></span>

<span data-ttu-id="33bb9-124">Du kan utlösa en logikapp inuti en funktion.</span><span class="sxs-lookup"><span data-stu-id="33bb9-124">You can trigger a logic app from inside a function.</span></span> <span data-ttu-id="33bb9-125">Se [Logic apps som callable slutpunkter](logic-apps-http-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="33bb9-125">See [Logic apps as callable endpoints](logic-apps-http-endpoint.md).</span></span> <span data-ttu-id="33bb9-126">Skapa en logikapp som har en manuell utlösare och sedan generera från en HTTP POST till manuell utlösaren URL med nyttolast som ska skickas till logikappen i din funktion.</span><span class="sxs-lookup"><span data-stu-id="33bb9-126">Create a logic app that has a manual trigger, then from inside your function, generate an HTTP POST to the manual trigger URL with the payload that you want sent to the logic app.</span></span>

### <a name="create-a-function-from-logic-app-designer"></a><span data-ttu-id="33bb9-127">Skapa en funktion från logik App Designer</span><span class="sxs-lookup"><span data-stu-id="33bb9-127">Create a function from Logic App Designer</span></span>

<span data-ttu-id="33bb9-128">Du kan också skapa en webhook node.js-funktion från designer.</span><span class="sxs-lookup"><span data-stu-id="33bb9-128">You can also create a node.js webhook function from the designer.</span></span> <span data-ttu-id="33bb9-129">Välj först **Azure Functions i min Region** och välj sedan en behållare för din funktion.</span><span class="sxs-lookup"><span data-stu-id="33bb9-129">First, select **Azure Functions in my Region,** and then choose a container for your function.</span></span> <span data-ttu-id="33bb9-130">Om du ännu inte har en behållare, måste du skapa en från den [Azure Functions-portalen](https://functions.azure.com/signin).</span><span class="sxs-lookup"><span data-stu-id="33bb9-130">If you don't yet have a container, you need to create one from the [Azure Functions portal](https://functions.azure.com/signin).</span></span> <span data-ttu-id="33bb9-131">Välj sedan **Skapa nytt**.</span><span class="sxs-lookup"><span data-stu-id="33bb9-131">Then select **Create New**.</span></span>  

<span data-ttu-id="33bb9-132">Ange context-objektet som du vill skicka till en funktion för att generera en mall baserat på de data som du vill beräkna.</span><span class="sxs-lookup"><span data-stu-id="33bb9-132">To generate a template based on the data that you want to compute, specify the context object that you plan to pass into a function.</span></span> <span data-ttu-id="33bb9-133">Det här objektet måste vara ett JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="33bb9-133">This object must be a JSON object.</span></span> <span data-ttu-id="33bb9-134">Till exempel om du skickar i filen innehåll från en FTP-åtgärd, kontext nyttolasten ser ut som det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="33bb9-134">For example, if you pass in the file content from an FTP action, the context payload looks like this example:</span></span>

![Nyttolasten i kontexten][2]

> [!NOTE]
> <span data-ttu-id="33bb9-136">Eftersom det inte att omvandla det här objektet som en sträng, läggs innehållet direkt till JSON-nyttolast.</span><span class="sxs-lookup"><span data-stu-id="33bb9-136">Because this object wasn't cast as a string, the content is added directly to the JSON payload.</span></span> <span data-ttu-id="33bb9-137">Dock felmeddelande ett om objektet inte är en JSON-token (det vill säga en sträng eller en JSON-objekt /-matris).</span><span class="sxs-lookup"><span data-stu-id="33bb9-137">However, an error occurs if the object is not a JSON token (that is, a string or a JSON object/array).</span></span> <span data-ttu-id="33bb9-138">Lägg till citattecken för att omvandla objektet som en sträng som visas i den första illustrationen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="33bb9-138">To cast the object as a string, add quotes as shown in the first illustration in this article.</span></span>
> 

<span data-ttu-id="33bb9-139">Designern skapar sedan en mall för funktionen att du kan skapa infogad.</span><span class="sxs-lookup"><span data-stu-id="33bb9-139">The designer then generates a function template that you can create inline.</span></span> <span data-ttu-id="33bb9-140">Variabler skapas före baserat på kontext som du vill skicka till funktionen.</span><span class="sxs-lookup"><span data-stu-id="33bb9-140">Variables are pre-created based on the context that you plan to pass into the function.</span></span>

<!--Image references-->
[1]: ./media/logic-apps-azure-functions/callfunction.png
[2]: ./media/logic-apps-azure-functions/createfunction.png
