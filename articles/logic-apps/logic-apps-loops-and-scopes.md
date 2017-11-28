---
title: "aaaCreate slinga scope och debatch data i arbetsflöden - Azure Logic Apps | Microsoft Docs"
description: "Skapa slingor tooiterate via data, gruppen åtgärder till scope, eller debatch data toostart fler arbetsflöden i Azure Logic Apps."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 75b52eeb-23a7-47dd-a42f-1351c6dfebdc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: e612ec2e83541f028916a07bf12c44e7b1f57ad1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="logic-apps-loops-scopes-and-debatching"></a><span data-ttu-id="41d30-103">Logic Apps-slingor, -omfattningar och Debatching</span><span class="sxs-lookup"><span data-stu-id="41d30-103">Logic Apps Loops, Scopes, and Debatching</span></span>
  
<span data-ttu-id="41d30-104">Logic Apps innehåller ett antal sätt toowork med matriser, samlingar, batchar och loopar i ett arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="41d30-104">Logic Apps provides a number of ways toowork with arrays, collections, batches, and loops within a workflow.</span></span>
  
## <a name="foreach-loop-and-arrays"></a><span data-ttu-id="41d30-105">ForEach-loop och matriser</span><span class="sxs-lookup"><span data-stu-id="41d30-105">ForEach loop and arrays</span></span>
  
<span data-ttu-id="41d30-106">Logic Apps kan du tooloop över en mängd av data och utföra en åtgärd för varje objekt.</span><span class="sxs-lookup"><span data-stu-id="41d30-106">Logic Apps allows you tooloop over a set of data and perform an action for each item.</span></span>  <span data-ttu-id="41d30-107">Det är möjligt via hello `foreach` åtgärd.</span><span class="sxs-lookup"><span data-stu-id="41d30-107">This is possible via hello `foreach` action.</span></span>  <span data-ttu-id="41d30-108">Du kan ange tooadd i hello designer, ett för varje loop.</span><span class="sxs-lookup"><span data-stu-id="41d30-108">In hello designer, you can specify tooadd a for each loop.</span></span>  <span data-ttu-id="41d30-109">När du har valt hello matris som du vill tooiterate över, kan du börja lägga till åtgärder.</span><span class="sxs-lookup"><span data-stu-id="41d30-109">After selecting hello array you wish tooiterate over, you can begin adding actions.</span></span>  <span data-ttu-id="41d30-110">Är för närvarande begränsad tooonly en åtgärd per foreach loop, men den här begränsningen ska dras tillbaka i hello kommer veckor.</span><span class="sxs-lookup"><span data-stu-id="41d30-110">Currently you are limited tooonly one action per foreach loop, but this restriction will be lifted in hello coming weeks.</span></span>  <span data-ttu-id="41d30-111">En gång i hello loop kan du börja toospecify vad ska ske vid varje värde i hello-matrisen.</span><span class="sxs-lookup"><span data-stu-id="41d30-111">Once within hello loop you can begin toospecify what should occur at each value of hello array.</span></span>

<span data-ttu-id="41d30-112">Om du använder vyn kod, kan du ange en för varje loop som nedan.</span><span class="sxs-lookup"><span data-stu-id="41d30-112">If using code-view, you can specify a for each loop like below.</span></span>  <span data-ttu-id="41d30-113">Detta är ett exempel på en för varje loop som skickar ett e-postmeddelande för varje e-postadress som innehåller ”microsoft.com”:</span><span class="sxs-lookup"><span data-stu-id="41d30-113">This is an example of a for each loop that sends an email for each email address that contains 'microsoft.com':</span></span>

``` json
{
    "email_filter": {
        "type": "query",
        "inputs": {
            "from": "@triggerBody()['emails']",
            "where": "@contains(item()['email'], 'microsoft.com')"
        }
    },
    "forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "actions": {
            "send_email": {
                "type": "ApiConnection",
                "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                },
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                },
                }
            }
        },
        "runAfter":{
            "email_filter": [ "Succeeded" ]
        }
    }
}
```
  
  <span data-ttu-id="41d30-114">En `foreach` åtgärd kan iterera över matriser in too5 000 rader.</span><span class="sxs-lookup"><span data-stu-id="41d30-114">A `foreach` action can iterate over arrays up too5,000 rows.</span></span>  <span data-ttu-id="41d30-115">Varje iteration körs parallellt som standard.</span><span class="sxs-lookup"><span data-stu-id="41d30-115">Each iteration will execute in parallel by default.</span></span>  

### <a name="sequential-foreach-loops"></a><span data-ttu-id="41d30-116">Sekventiella ForEach slingor</span><span class="sxs-lookup"><span data-stu-id="41d30-116">Sequential ForEach loops</span></span>

<span data-ttu-id="41d30-117">tooenable en foreach loop tooexecute hello sekventiellt, `Sequential` åtgärden alternativet ska läggas till.</span><span class="sxs-lookup"><span data-stu-id="41d30-117">tooenable a foreach loop tooexecute sequentially, hello `Sequential` operation option should be added.</span></span>

``` json
"forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "operationOptions": "Sequential",
        "..."
}
```
  
## <a name="until-loop"></a><span data-ttu-id="41d30-118">Tills loop</span><span class="sxs-lookup"><span data-stu-id="41d30-118">Until loop</span></span>
  
  <span data-ttu-id="41d30-119">Du kan utföra en åtgärd eller en serie åtgärder förrän ett villkor uppfylls.</span><span class="sxs-lookup"><span data-stu-id="41d30-119">You can perform an action or series of actions until a condition is met.</span></span>  <span data-ttu-id="41d30-120">hello anropar vanligaste scenariot för detta en slutpunkt förrän du får hello-svar som du letar efter.</span><span class="sxs-lookup"><span data-stu-id="41d30-120">hello most common scenario for this is calling an endpoint until you get hello response you are looking for.</span></span>  <span data-ttu-id="41d30-121">Du kan ange tooadd i hello designer en tills loop.</span><span class="sxs-lookup"><span data-stu-id="41d30-121">In hello designer, you can specify tooadd an until loop.</span></span>  <span data-ttu-id="41d30-122">När du lägger till åtgärder i hello loop kan du ange villkoret för avslutning av hello, samt hello loop gränser.</span><span class="sxs-lookup"><span data-stu-id="41d30-122">After adding actions inside hello loop, you can set hello exit condition, as well as hello loop limits.</span></span>  <span data-ttu-id="41d30-123">Det finns en minuts fördröjning mellan loop cykler.</span><span class="sxs-lookup"><span data-stu-id="41d30-123">There is a 1 minute delay between loop cycles.</span></span>
  
  <span data-ttu-id="41d30-124">Om du använder vyn kod, kan du ange en tills loop som nedan.</span><span class="sxs-lookup"><span data-stu-id="41d30-124">If using code-view, you can specify an until loop like below.</span></span>  <span data-ttu-id="41d30-125">Detta är ett exempel på en HTTP-slutpunkt anropas förrän hello svarstexten har hello värde 'Slutförd'.</span><span class="sxs-lookup"><span data-stu-id="41d30-125">This is an example of calling an HTTP endpoint until hello response body has hello value 'Completed'.</span></span>  <span data-ttu-id="41d30-126">Slutförs när antingen</span><span class="sxs-lookup"><span data-stu-id="41d30-126">It will complete when either</span></span> 
  
  * <span data-ttu-id="41d30-127">HTTP-svar som har statusen ”Slutförd”</span><span class="sxs-lookup"><span data-stu-id="41d30-127">HTTP Response has status of 'Completed'</span></span>
  * <span data-ttu-id="41d30-128">Det har försökt för 1 timme</span><span class="sxs-lookup"><span data-stu-id="41d30-128">It has tried for 1 hour</span></span>
  * <span data-ttu-id="41d30-129">Det har körts i en slinga 100 gånger</span><span class="sxs-lookup"><span data-stu-id="41d30-129">It has looped 100 times</span></span>
  
  ``` json
  {
      "until_successful":{
        "type": "until",
        "expression": "@equals(actions('http')['status'], 'Completed')",
        "limit": {
            "count": 100,
            "timeout": "PT1H"
        },
        "actions": {
            "create_resource": {
                "type": "http",
                "inputs": {
                    "url": "http://provisionRseource.com",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                }
            }
        }
      }
  }
  ```
  
## <a name="spliton-and-debatching"></a><span data-ttu-id="41d30-130">SplitOn och debatching</span><span class="sxs-lookup"><span data-stu-id="41d30-130">SplitOn and debatching</span></span>

<span data-ttu-id="41d30-131">En utlösare kan ibland visas en matris med objekt som du vill använda toodebatch och starta ett arbetsflöde per artikel.</span><span class="sxs-lookup"><span data-stu-id="41d30-131">Sometimes a trigger may receive an array of items that you want toodebatch and start a workflow per item.</span></span>  <span data-ttu-id="41d30-132">Detta kan åstadkommas via hello `spliton` kommando.</span><span class="sxs-lookup"><span data-stu-id="41d30-132">This can be accomplished via hello `spliton` command.</span></span>  <span data-ttu-id="41d30-133">Som standard om utlösaren-swagger anger en nyttolast som är en matris, en `spliton` kommer att läggas till och starta en körning per artikel.</span><span class="sxs-lookup"><span data-stu-id="41d30-133">By default, if your trigger swagger specifies a payload that is an array, a `spliton` will be added and start a run per item.</span></span>  <span data-ttu-id="41d30-134">SplitOn kan bara läggas tooa utlösare.</span><span class="sxs-lookup"><span data-stu-id="41d30-134">SplitOn can only be added tooa trigger.</span></span>  <span data-ttu-id="41d30-135">Detta kan manuellt konfigurerade eller åsidosätts i definition kodvy.</span><span class="sxs-lookup"><span data-stu-id="41d30-135">This can be manually configured or overridden in definition code-view.</span></span>  <span data-ttu-id="41d30-136">SplitOn kan för närvarande debatch matriser in too5 000 objekt.</span><span class="sxs-lookup"><span data-stu-id="41d30-136">Currently SplitOn can debatch arrays up too5,000 items.</span></span>  <span data-ttu-id="41d30-137">Du kan inte ha en `spliton` och även implementera hello synkron svar mönster.</span><span class="sxs-lookup"><span data-stu-id="41d30-137">You cannot have a `spliton` and also implement hello synchronous response pattern.</span></span>  <span data-ttu-id="41d30-138">Alla arbetsflöden som har en `response` åtgärd dessutom för`spliton` körs asynkront och skicka en omedelbar `202 Accepted` svar.</span><span class="sxs-lookup"><span data-stu-id="41d30-138">Any workflow called that has a `response` action in addition too`spliton` will run asynchronously and send an immediate `202 Accepted` response.</span></span>  

<span data-ttu-id="41d30-139">SplitOn kan anges i vyn kod som hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="41d30-139">SplitOn can be specified in code-view as hello following example.</span></span>  <span data-ttu-id="41d30-140">Detta tar emot en matris med objekt och debatches på varje rad.</span><span class="sxs-lookup"><span data-stu-id="41d30-140">This receives an array of items and debatches on each row.</span></span>

```
{
    "myDebatchTrigger": {
        "type": "Http",
        "inputs": {
            "url": "http://getNewCustomers",
        },
        "recurrence": {
            "frequencey": "Second",
            "interval": 15
        },
        "spliton": "@triggerBody()['rows']"
    }
}
```

## <a name="scopes"></a><span data-ttu-id="41d30-141">Scope</span><span class="sxs-lookup"><span data-stu-id="41d30-141">Scopes</span></span>

<span data-ttu-id="41d30-142">Det är möjligt toogroup en serie åtgärder tillsammans med en omfattning.</span><span class="sxs-lookup"><span data-stu-id="41d30-142">It is possible toogroup a series of actions together using a scope.</span></span>  <span data-ttu-id="41d30-143">Detta är särskilt användbart för att implementera undantagshantering.</span><span class="sxs-lookup"><span data-stu-id="41d30-143">This is particularly useful for implementing exception handling.</span></span>  <span data-ttu-id="41d30-144">I hello designer kan du lägga till ett nytt scope och börja lägga till alla åtgärder i den.</span><span class="sxs-lookup"><span data-stu-id="41d30-144">In hello designer you can add a new scope, and begin adding any actions inside of it.</span></span>  <span data-ttu-id="41d30-145">Du kan definiera scope i vyn hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="41d30-145">You can define scopes in code-view like hello following:</span></span>


```
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```