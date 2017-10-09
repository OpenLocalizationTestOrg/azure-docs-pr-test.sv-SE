---
title: "aaaCustom koden för Logikappar i Azure med Azure Functions | Microsoft Docs"
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
ms.openlocfilehash: 18b3821ed7e434feb568b9b96e9a5a2189dba3bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-and-run-custom-code-for-logic-apps-through-azure-functions"></a><span data-ttu-id="a5022-103">Lägg till och köra anpassad kod för logic apps via Azure Functions</span><span class="sxs-lookup"><span data-stu-id="a5022-103">Add and run custom code for logic apps through Azure Functions</span></span>

<span data-ttu-id="a5022-104">toorun anpassade kodavsnitt för C# eller node.js i logikappar, kan du skapa anpassade funktioner via Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="a5022-104">toorun custom snippets of C# or node.js in logic apps, you can create custom functions through Azure Functions.</span></span> 
<span data-ttu-id="a5022-105">[Azure Functions](../azure-functions/functions-overview.md) erbjuder server-dator i Microsoft Azure och är användbara för att utföra dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="a5022-105">[Azure Functions](../azure-functions/functions-overview.md) offers server-free computing in Microsoft Azure and are useful for performing these tasks:</span></span>

* <span data-ttu-id="a5022-106">Avancerad formatering eller beräknade fält i logikappar</span><span class="sxs-lookup"><span data-stu-id="a5022-106">Advanced formatting or compute of fields in logic apps</span></span>
* <span data-ttu-id="a5022-107">Utföra beräkningar i ett arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="a5022-107">Perform calculations in a workflow.</span></span>
* <span data-ttu-id="a5022-108">Utöka hello logik funktionalitet med funktioner som stöds i C# eller node.js</span><span class="sxs-lookup"><span data-stu-id="a5022-108">Extend hello logic app functionality with functions that are supported in C# or node.js</span></span>

## <a name="create-custom-functions-for-your-logic-apps"></a><span data-ttu-id="a5022-109">Skapa anpassade funktioner för dina logic apps</span><span class="sxs-lookup"><span data-stu-id="a5022-109">Create custom functions for your logic apps</span></span>

<span data-ttu-id="a5022-110">Vi rekommenderar att du skapar en funktion i hello Azure Functions-portalen från hello **allmän Webhook - nod** eller **allmän Webhook - C#** mallar.</span><span class="sxs-lookup"><span data-stu-id="a5022-110">We recommend that you create a function in hello Azure Functions portal, from hello **Generic Webhook - Node** or **Generic Webhook - C#** templates.</span></span> <span data-ttu-id="a5022-111">hello resultatet skapas en auto-fylls i en mall som accepterar `application/json` från en logikapp.</span><span class="sxs-lookup"><span data-stu-id="a5022-111">hello result creates an auto-populated a template that accepts `application/json` from a logic app.</span></span> <span data-ttu-id="a5022-112">Funktioner som du skapar från dessa mallar identifieras automatiskt och visas i hello logik App Designer under **Azure Functions i min region.**</span><span class="sxs-lookup"><span data-stu-id="a5022-112">Functions that you create from these templates are automatically detected and appear in hello Logic App Designer under **Azure Functions in my region.**</span></span>

<span data-ttu-id="a5022-113">I hello Azure-portalen på hello **integrera** fönstret för din funktion mallen ska visas som **läge** ställa in också**Webhook** och **Webhook typen** har angetts för**allmänna JSON**.</span><span class="sxs-lookup"><span data-stu-id="a5022-113">In hello Azure portal, on hello **Integrate** pane for your function, your template should show that **Mode** set too**Webhook** and **Webhook type** is set too**Generic JSON**.</span></span> 

<span data-ttu-id="a5022-114">Webhook-funktioner acceptera en begäran och skicka den till hello metoden via en `data` variabel.</span><span class="sxs-lookup"><span data-stu-id="a5022-114">Webhook functions accept a request and pass it into hello method via a `data` variable.</span></span> <span data-ttu-id="a5022-115">Du kan komma åt hello egenskaper för din nyttolast med hjälp av punktnotation som `data.function-name`.</span><span class="sxs-lookup"><span data-stu-id="a5022-115">You can access hello properties of your payload by using dot notation like `data.function-name`.</span></span> <span data-ttu-id="a5022-116">Till exempel en enkel JavaScript-funktion som konverterar ett DateTime-värde till en datumsträng ser ut så hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="a5022-116">For example, a simple JavaScript function that converts a DateTime value into a date string looks like hello following example:</span></span>

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-logic-apps"></a><span data-ttu-id="a5022-117">Anropa Azure Functions från logikappar</span><span class="sxs-lookup"><span data-stu-id="a5022-117">Call Azure Functions from logic apps</span></span>

<span data-ttu-id="a5022-118">toolist hello behållare i din prenumeration och välj hello-funktionen som du vill toocall i logik App Designer klickar du på hello **åtgärder** -menyn och välj bland **Azure Functions i min Region**.</span><span class="sxs-lookup"><span data-stu-id="a5022-118">toolist hello containers in your subscription and select hello function that you want toocall, in Logic App Designer, click hello **Actions** menu, and select from **Azure Functions in my Region**.</span></span>

<span data-ttu-id="a5022-119">När du har valt hello-funktionen är och toospecify objektets nyttolasten indata.</span><span class="sxs-lookup"><span data-stu-id="a5022-119">After you select hello function, you are asked toospecify an input payload object.</span></span> <span data-ttu-id="a5022-120">Det här objektet är hello-meddelande som hello logik appen skickar toohello funktion och måste vara ett JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="a5022-120">This object is hello message that hello logic app sends toohello function and must be a JSON object.</span></span> <span data-ttu-id="a5022-121">Om du vill toopass i hello exempelvis **ändrades senast** datum från en Salesforce-utlösare hello funktionen nyttolasten kan se ut som i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="a5022-121">For example, if you want toopass in hello **Last Modified** date from a Salesforce trigger, hello function payload might look like this example:</span></span>

![Senast ändrad datum][1]

## <a name="trigger-logic-apps-from-a-function"></a><span data-ttu-id="a5022-123">Utlösaren logikappar från en funktion</span><span class="sxs-lookup"><span data-stu-id="a5022-123">Trigger logic apps from a function</span></span>

<span data-ttu-id="a5022-124">Du kan utlösa en logikapp inuti en funktion.</span><span class="sxs-lookup"><span data-stu-id="a5022-124">You can trigger a logic app from inside a function.</span></span> <span data-ttu-id="a5022-125">Se [Logic apps som callable slutpunkter](logic-apps-http-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="a5022-125">See [Logic apps as callable endpoints](logic-apps-http-endpoint.md).</span></span> <span data-ttu-id="a5022-126">Skapa en logikapp som har en manuell utlösare och sedan generera från en HTTP POST toohello manuell utlösaren URL med hello-nyttolast som du vill skicka toohello logikapp i din funktion.</span><span class="sxs-lookup"><span data-stu-id="a5022-126">Create a logic app that has a manual trigger, then from inside your function, generate an HTTP POST toohello manual trigger URL with hello payload that you want sent toohello logic app.</span></span>

### <a name="create-a-function-from-logic-app-designer"></a><span data-ttu-id="a5022-127">Skapa en funktion från logik App Designer</span><span class="sxs-lookup"><span data-stu-id="a5022-127">Create a function from Logic App Designer</span></span>

<span data-ttu-id="a5022-128">Du kan också skapa en webhook node.js-funktion från hello designer.</span><span class="sxs-lookup"><span data-stu-id="a5022-128">You can also create a node.js webhook function from hello designer.</span></span> <span data-ttu-id="a5022-129">Välj först **Azure Functions i min Region** och välj sedan en behållare för din funktion.</span><span class="sxs-lookup"><span data-stu-id="a5022-129">First, select **Azure Functions in my Region,** and then choose a container for your function.</span></span> <span data-ttu-id="a5022-130">Om du ännu inte har en behållare, måste en toocreate från hello [Azure Functions-portalen](https://functions.azure.com/signin).</span><span class="sxs-lookup"><span data-stu-id="a5022-130">If you don't yet have a container, you need toocreate one from hello [Azure Functions portal](https://functions.azure.com/signin).</span></span> <span data-ttu-id="a5022-131">Välj sedan **Skapa nytt**.</span><span class="sxs-lookup"><span data-stu-id="a5022-131">Then select **Create New**.</span></span>  

<span data-ttu-id="a5022-132">toogenerate en mall baserat på hello data som du vill toocompute, ange hello context-objektet som du planerar toopass i en funktion.</span><span class="sxs-lookup"><span data-stu-id="a5022-132">toogenerate a template based on hello data that you want toocompute, specify hello context object that you plan toopass into a function.</span></span> <span data-ttu-id="a5022-133">Det här objektet måste vara ett JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="a5022-133">This object must be a JSON object.</span></span> <span data-ttu-id="a5022-134">Till exempel om du skickar i hello innehåll från en FTP-åtgärd hello kontexten nyttolast ser ut som i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="a5022-134">For example, if you pass in hello file content from an FTP action, hello context payload looks like this example:</span></span>

![Nyttolasten i kontexten][2]

> [!NOTE]
> <span data-ttu-id="a5022-136">Eftersom det inte att omvandla det här objektet som en sträng, hello innehåll läggs direkt toohello JSON-nyttolast.</span><span class="sxs-lookup"><span data-stu-id="a5022-136">Because this object wasn't cast as a string, hello content is added directly toohello JSON payload.</span></span> <span data-ttu-id="a5022-137">Dock felmeddelande ett om hello-objektet inte är en JSON-token (det vill säga en sträng eller en JSON-objekt /-matris).</span><span class="sxs-lookup"><span data-stu-id="a5022-137">However, an error occurs if hello object is not a JSON token (that is, a string or a JSON object/array).</span></span> <span data-ttu-id="a5022-138">toocast hello-objektet som en sträng, lägga till citattecken som hello första bilden i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a5022-138">toocast hello object as a string, add quotes as shown in hello first illustration in this article.</span></span>
> 

<span data-ttu-id="a5022-139">hello designer skapar sedan en mall för funktionen att du kan skapa infogade.</span><span class="sxs-lookup"><span data-stu-id="a5022-139">hello designer then generates a function template that you can create inline.</span></span> <span data-ttu-id="a5022-140">Variabler skapas före baserat på hello kontext som du planerar toopass till hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="a5022-140">Variables are pre-created based on hello context that you plan toopass into hello function.</span></span>

<!--Image references-->
[1]: ./media/logic-apps-azure-functions/callfunction.png
[2]: ./media/logic-apps-azure-functions/createfunction.png
