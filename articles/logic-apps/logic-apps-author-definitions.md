---
title: "aaaDefine arbetsflöden med JSON - Azure Logic Apps | Microsoft Docs"
description: "Hur toowrite arbetsflödesdefinitioner i JSON för logic apps"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 03/29/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 0d69d334ecee9c3e7f8684cfde68ef0e85280358
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-workflow-definitions-for-logic-apps-using-json"></a><span data-ttu-id="07043-103">Skapa arbetsflödesdefinitioner för logic apps med JSON</span><span class="sxs-lookup"><span data-stu-id="07043-103">Create workflow definitions for logic apps using JSON</span></span>

<span data-ttu-id="07043-104">Du kan skapa arbetsflödesdefinitioner för [Azure Logikappar](logic-apps-what-are-logic-apps.md) med enkla, deklarativa JSON-språk.</span><span class="sxs-lookup"><span data-stu-id="07043-104">You can create workflow definitions for [Azure Logic Apps](logic-apps-what-are-logic-apps.md) with simple, declarative JSON language.</span></span> <span data-ttu-id="07043-105">Om du inte redan gjort först granska [hur toocreate logikappen första med logik App Designer](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="07043-105">If you haven't already, first review [how toocreate your first logic app with Logic App Designer](logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="07043-106">Se även hello [fullständig referens för hello språk i arbetsflödesdefinitionen](http://aka.ms/logicappsdocs).</span><span class="sxs-lookup"><span data-stu-id="07043-106">Also, see hello [full reference for hello Workflow Definition Language](http://aka.ms/logicappsdocs).</span></span>

## <a name="repeat-steps-over-a-list"></a><span data-ttu-id="07043-107">Upprepa steg över en lista</span><span class="sxs-lookup"><span data-stu-id="07043-107">Repeat steps over a list</span></span>

<span data-ttu-id="07043-108">tooiterate via en matris som har upp too10 000 objekt och utföra en åtgärd för varje objekt kan använda hello [foreach typen](logic-apps-loops-and-scopes.md).</span><span class="sxs-lookup"><span data-stu-id="07043-108">tooiterate through an array that has up too10,000 items and perform an action for each item, use hello [foreach type](logic-apps-loops-and-scopes.md).</span></span>

## <a name="handle-failures-if-something-goes-wrong"></a><span data-ttu-id="07043-109">Hantera fel om något går fel</span><span class="sxs-lookup"><span data-stu-id="07043-109">Handle failures if something goes wrong</span></span>

<span data-ttu-id="07043-110">Vanligtvis vill tooinclude en *reparation steg* – vissa logik som kör *endast om* misslyckas för en eller flera av dina anrop.</span><span class="sxs-lookup"><span data-stu-id="07043-110">Usually, you want tooinclude a *remediation step* — some logic that executes *if and only if* one or more of your calls fail.</span></span> <span data-ttu-id="07043-111">Det här exemplet hämtar data från olika platser, men om hello anropet misslyckas, vi tooPOST meddelandet någonstans så att vi kan spåra att fel senare:</span><span class="sxs-lookup"><span data-stu-id="07043-111">This example gets data from various places, but if hello call fails, we want tooPOST a message somewhere so we can track down that failure later:</span></span>  

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "triggers": {
    "Request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "readData": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      }
    },
    "postToErrorMessageQueue": {
      "type": "ApiConnection",
      "inputs": "...",
      "runAfter": {
        "readData": [
          "Failed"
        ]
      }
    }
  },
  "outputs": {}
}
```

<span data-ttu-id="07043-112">toospecify som `postToErrorMessageQueue` körs bara `readData` har `Failed`, använda hello `runAfter` egenskap, till exempel toospecify en lista över möjliga värden så att `runAfter` kan vara `["Succeeded", "Failed"]`.</span><span class="sxs-lookup"><span data-stu-id="07043-112">toospecify that `postToErrorMessageQueue` only runs after `readData` has `Failed`, use hello `runAfter` property, for example, toospecify a list of possible values, so that `runAfter` could be `["Succeeded", "Failed"]`.</span></span>

<span data-ttu-id="07043-113">Slutligen, eftersom det här exemplet hanterar nu hello fel, vi inte längre markera hello kör som- `Failed`.</span><span class="sxs-lookup"><span data-stu-id="07043-113">Finally, because this example now handles hello error, we no longer mark hello run as `Failed`.</span></span> <span data-ttu-id="07043-114">Eftersom vi har lagt till hello steg för att hantera detta fel i det här exemplet hello kör har `Succeeded` även om ett steg `Failed`.</span><span class="sxs-lookup"><span data-stu-id="07043-114">Because we added hello step for handling this failure in this example, hello run has `Succeeded` although one step `Failed`.</span></span>

## <a name="execute-two-or-more-steps-in-parallel"></a><span data-ttu-id="07043-115">Köra två eller flera steg parallellt</span><span class="sxs-lookup"><span data-stu-id="07043-115">Execute two or more steps in parallel</span></span>

<span data-ttu-id="07043-116">toorun flera åtgärder parallellt, hello `runAfter` egenskapen måste ha samma vid körning.</span><span class="sxs-lookup"><span data-stu-id="07043-116">toorun multiple actions in parallel, hello `runAfter` property must be equivalent at runtime.</span></span> 

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "triggers": {
    "Request": {
      "kind": "http",
      "type": "Request"
    }
  },
  "actions": {
    "readData": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      }
    },
    "branch1": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    },
    "branch2": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

<span data-ttu-id="07043-117">I det här exemplet både `branch1` och `branch2` ställs toorun efter `readData`.</span><span class="sxs-lookup"><span data-stu-id="07043-117">In this example, both `branch1` and `branch2` are set toorun after `readData`.</span></span> <span data-ttu-id="07043-118">Därför körs båda grenar parallellt.</span><span class="sxs-lookup"><span data-stu-id="07043-118">As a result, both branches run in parallel.</span></span> <span data-ttu-id="07043-119">hello tidsstämpel för båda filialer är identiska.</span><span class="sxs-lookup"><span data-stu-id="07043-119">hello timestamp for both branches is identical.</span></span>

![Parallell](media/logic-apps-author-definitions/parallel.png)

## <a name="join-two-parallel-branches"></a><span data-ttu-id="07043-121">Ansluta två parallella grenar</span><span class="sxs-lookup"><span data-stu-id="07043-121">Join two parallel branches</span></span>

<span data-ttu-id="07043-122">Du kan delta i två åtgärder som är inställda toorun parallellt genom att lägga till objekt toohello `runAfter` egenskap som hello föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="07043-122">You can join two actions that are set toorun in parallel by adding items toohello `runAfter` property as in hello previous example.</span></span>

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-04-01-preview/workflowdefinition.json#",
  "actions": {
    "readData": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {}
    },
    "branch1": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    },
    "branch2": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "readData": [
          "Succeeded"
        ]
      }
    },
    "join": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://myurl"
      },
      "runAfter": {
        "branch1": [
          "Succeeded"
        ],
        "branch2": [
          "Succeeded"
        ]
      }
    }
  },
  "parameters": {},
  "triggers": {
    "Request": {
      "type": "Request",
      "kind": "Http",
      "inputs": {
        "schema": {}
      }
    }
  },
  "contentVersion": "1.0.0.0",
  "outputs": {}
}
```

![Parallell](media/logic-apps-author-definitions/join.png)

## <a name="map-list-items-tooa-different-configuration"></a><span data-ttu-id="07043-124">Mappa lista objekt tooa annan konfiguration</span><span class="sxs-lookup"><span data-stu-id="07043-124">Map list items tooa different configuration</span></span>

<span data-ttu-id="07043-125">Sedan anta att vi vill tooget olika innehåll baserat på hello-värdet för en egenskap.</span><span class="sxs-lookup"><span data-stu-id="07043-125">Next, let's say that we want tooget different content based on hello value of a property.</span></span> <span data-ttu-id="07043-126">Vi kan skapa en karta över värden toodestinations som en parameter:</span><span class="sxs-lookup"><span data-stu-id="07043-126">We can create a map of values toodestinations as a parameter:</span></span>  

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "specialCategories": {
      "defaultValue": [
        "science",
        "google",
        "microsoft",
        "robots",
        "NSA"
      ],
      "type": "Array"
    },
    "destinationMap": {
      "defaultValue": {
        "science": "http://www.nasa.gov",
        "microsoft": "https://www.microsoft.com/en-us/default.aspx",
        "google": "https://www.google.com",
        "robots": "https://en.wikipedia.org/wiki/Robot",
        "NSA": "https://www.nsa.gov/"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "Request": {
      "type": "Request",
      "kind": "http"
    }
  },
  "actions": {
    "getArticles": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.wired.com/wired/index"
      }
    },
    "forEachArticle": {
      "type": "foreach",
      "foreach": "@body('getArticles').responseData.feed.entries",
      "actions": {
        "ifGreater": {
          "type": "if",
          "expression": "@greater(length(intersection(item().categories, parameters('specialCategories'))), 0)",
          "actions": {
            "getSpecialPage": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('destinationMap')[first(intersection(item().categories, parameters('specialCategories')))]"
              }
            }
          }
        }
      },
      "runAfter": {
        "getArticles": [
          "Succeeded"
        ]
      }
    }
  }
}
```

<span data-ttu-id="07043-127">I det här fallet hämta vi först en lista över artiklar.</span><span class="sxs-lookup"><span data-stu-id="07043-127">In this case, we first get a list of articles.</span></span> <span data-ttu-id="07043-128">Hello andra steget använder en karta toolook in hello URL: en för att hämta hello innehåll baserat på hello kategori som har definierats som en parameter.</span><span class="sxs-lookup"><span data-stu-id="07043-128">Based on hello category that was defined as a parameter, hello second step uses a map toolook up hello URL for getting hello content.</span></span>

<span data-ttu-id="07043-129">Några tillfällen toonote här:</span><span class="sxs-lookup"><span data-stu-id="07043-129">Some times toonote here:</span></span> 

*   <span data-ttu-id="07043-130">Hej [ `intersection()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) funktionen kontrollerar om hello kategori matchar en av hello kända angivna kategorier.</span><span class="sxs-lookup"><span data-stu-id="07043-130">hello [`intersection()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) function checks whether hello category matches one of hello known defined categories.</span></span>

*   <span data-ttu-id="07043-131">När vi har fått hello kategori, hämtar vi hello objekt från hello karta med hakparenteser:`parameters[...]`</span><span class="sxs-lookup"><span data-stu-id="07043-131">After we get hello category, we can pull hello item from hello map using square brackets: `parameters[...]`</span></span>

## <a name="process-strings"></a><span data-ttu-id="07043-132">Process-strängar</span><span class="sxs-lookup"><span data-stu-id="07043-132">Process strings</span></span>

<span data-ttu-id="07043-133">Du kan använda olika funktioner toomanipulate strängar.</span><span class="sxs-lookup"><span data-stu-id="07043-133">You can use various functions toomanipulate strings.</span></span> <span data-ttu-id="07043-134">Anta exempelvis att vi har en sträng som vi vill toopass tooa system, men vi inte är säker på om korrekt hantering för teckenkodning.</span><span class="sxs-lookup"><span data-stu-id="07043-134">For example, suppose we have a string that we want toopass tooa system, but we aren't confident about proper handling for character encoding.</span></span> <span data-ttu-id="07043-135">Ett alternativ är toobase64 koda den här strängen.</span><span class="sxs-lookup"><span data-stu-id="07043-135">One option is toobase64 encode this string.</span></span> <span data-ttu-id="07043-136">Dock tooavoid undantagstecken i URL-adresser, vi tooreplace några tecken.</span><span class="sxs-lookup"><span data-stu-id="07043-136">However, tooavoid escaping in a URL, we are going tooreplace a few characters.</span></span> 

<span data-ttu-id="07043-137">Vi vill även en understräng hello ordning namn eftersom hello första fem tecknen inte används.</span><span class="sxs-lookup"><span data-stu-id="07043-137">We also want a substring of hello order's name because hello first five characters are not used.</span></span>

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder1",
        "orderer": "NAME=Contoso"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').orderer,5,sub(length(parameters('order').orderer), 5) )),'+','-') ,'/' ,'_' )}"
      }
    }
  },
  "outputs": {}
}
```

<span data-ttu-id="07043-138">Fungerar från inuti toooutside:</span><span class="sxs-lookup"><span data-stu-id="07043-138">Working from inside toooutside:</span></span>

1. <span data-ttu-id="07043-139">Hämta hello [ `length()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) för hello orderer namn, så vi komma hello Totalt antal tecken.</span><span class="sxs-lookup"><span data-stu-id="07043-139">Get hello [`length()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) for hello orderer's name, so we get back hello total number of characters.</span></span>

2. <span data-ttu-id="07043-140">Subtrahera 5 eftersom vi vill att en kortare sträng.</span><span class="sxs-lookup"><span data-stu-id="07043-140">Subtract 5 because we want a shorter string.</span></span>

3. <span data-ttu-id="07043-141">Faktiskt, ta hello [ `substring()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring).</span><span class="sxs-lookup"><span data-stu-id="07043-141">Actually, take hello [`substring()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring).</span></span> <span data-ttu-id="07043-142">Vi börjar vid index `5` och gå hello resten av hello-sträng.</span><span class="sxs-lookup"><span data-stu-id="07043-142">We start at index `5` and go hello remainder of hello string.</span></span>

4. <span data-ttu-id="07043-143">Konvertera den här delsträngen tooa [ `base64()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) sträng.</span><span class="sxs-lookup"><span data-stu-id="07043-143">Convert this substring tooa [`base64()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) string.</span></span>

5. <span data-ttu-id="07043-144">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)alla hello `+` tecken med `-` tecken.</span><span class="sxs-lookup"><span data-stu-id="07043-144">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace) all hello `+` characters with `-` characters.</span></span>

6. <span data-ttu-id="07043-145">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)alla hello `/` tecken med `_` tecken.</span><span class="sxs-lookup"><span data-stu-id="07043-145">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace) all hello `/` characters with `_` characters.</span></span>

## <a name="work-with-date-times"></a><span data-ttu-id="07043-146">Arbeta med datum</span><span class="sxs-lookup"><span data-stu-id="07043-146">Work with Date Times</span></span>

<span data-ttu-id="07043-147">Datum kan vara användbart, särskilt när du försöker toopull data från en datakälla som inte stöder naturligt *utlösare*.</span><span class="sxs-lookup"><span data-stu-id="07043-147">Date Times can be useful, particularly when you are trying toopull data from a data source that doesn't naturally support *triggers*.</span></span> <span data-ttu-id="07043-148">Du kan också använda datum för att söka efter hur länge olika steg tar.</span><span class="sxs-lookup"><span data-stu-id="07043-148">You can also use Date Times for finding how long various steps are taking.</span></span>

```
{
  "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "order": {
      "defaultValue": {
        "quantity": 10,
        "id": "myorder1"
      },
      "type": "Object"
    }
  },
  "triggers": {
    "Request": {
      "type": "request",
      "kind": "http"
    }
  },
  "actions": {
    "order": {
      "type": "Http",
      "inputs": {
        "method": "GET",
        "uri": "http://www.example.com/?id=@{parameters('order').id}"
      }
    },
    "ifTimingWarning": {
      "type": "If",
      "expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))",
      "actions": {
        "timingWarning": {
          "type": "Http",
          "inputs": {
            "method": "GET",
            "uri": "http://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
          }
        }
      },
      "runAfter": {
        "order": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

<span data-ttu-id="07043-149">I det här exemplet vi extrahera hello `startTime` hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="07043-149">In this example, we extract hello `startTime` from hello previous step.</span></span> <span data-ttu-id="07043-150">Sedan vi hämta hello aktuell tid och ta bort en sekund:</span><span class="sxs-lookup"><span data-stu-id="07043-150">Then we get hello current time, and subtract one second:</span></span>

[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) 

<span data-ttu-id="07043-151">Du kan använda andra tidsenheter, som `minutes` eller `hours`.</span><span class="sxs-lookup"><span data-stu-id="07043-151">You can use other units of time, like `minutes` or `hours`.</span></span> <span data-ttu-id="07043-152">Slutligen kan vi Jämför dessa två värden.</span><span class="sxs-lookup"><span data-stu-id="07043-152">Finally, we can compare these two values.</span></span> <span data-ttu-id="07043-153">Om hello första värdet är mindre än andra hello-värdet och mer än en sekund har gått sedan hello beställning gjordes först.</span><span class="sxs-lookup"><span data-stu-id="07043-153">If hello first value is less than hello second value, then more than one second has passed since hello order was first placed.</span></span>

<span data-ttu-id="07043-154">tooformat datum vi kan använda string-formaterare.</span><span class="sxs-lookup"><span data-stu-id="07043-154">tooformat dates, we can use string formatters.</span></span> <span data-ttu-id="07043-155">Till exempel tooget hello RFC1123, vi använder [ `utcnow('r')` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span><span class="sxs-lookup"><span data-stu-id="07043-155">For example, tooget hello RFC1123, we use [`utcnow('r')`](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span></span> <span data-ttu-id="07043-156">toolearn formatera datum, se [språk i arbetsflödesdefinitionen](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span><span class="sxs-lookup"><span data-stu-id="07043-156">toolearn about date formatting, see [Workflow Definition Language](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span></span>

## <a name="deployment-parameters-for-different-environments"></a><span data-ttu-id="07043-157">Distributionsparametrarna för olika miljöer</span><span class="sxs-lookup"><span data-stu-id="07043-157">Deployment parameters for different environments</span></span>

<span data-ttu-id="07043-158">Distribution livscykler har vanligtvis en utvecklingsmiljö, en mellanlagringsmiljön och en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="07043-158">Commonly, deployment lifecycles have a development environment, a staging environment, and a production environment.</span></span> <span data-ttu-id="07043-159">Du kan till exempel använda hello samma definition i dessa miljöer, men använder olika databaser.</span><span class="sxs-lookup"><span data-stu-id="07043-159">For example, you might use hello same definition in all these environments but use different databases.</span></span> <span data-ttu-id="07043-160">På samma sätt kan du kanske vill toouse hello samma definition över olika regioner för hög tillgänglighet men vill varje logik app-instansen tootalk toothat regions-databasen.</span><span class="sxs-lookup"><span data-stu-id="07043-160">Likewise, you might want toouse hello same definition across different regions for high availability but want each logic app instance tootalk toothat region's database.</span></span>
<span data-ttu-id="07043-161">Det här scenariot skiljer sig från tar parametrar på *runtime* där i stället du bör använda hello `trigger()` fungera som hello föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="07043-161">This scenario differs from taking parameters at *runtime* where instead, you should use hello `trigger()` function as in hello previous example.</span></span>

<span data-ttu-id="07043-162">Du kan börja med en grundläggande definition som det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="07043-162">You can start with a basic definition like this example:</span></span>

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "uri": {
            "type": "string"
        }
    },
    "triggers": {
        "request": {
          "type": "request",
          "kind": "http"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('uri')"
            }
        }
    },
    "outputs": {}
}
```

<span data-ttu-id="07043-163">I hello faktiska `PUT` begäran för hello logic apps kan du ange hello parametern `uri`.</span><span class="sxs-lookup"><span data-stu-id="07043-163">In hello actual `PUT` request for hello logic apps, you can provide hello parameter `uri`.</span></span> <span data-ttu-id="07043-164">Eftersom det finns inte längre ett standardvärde, kräver den här parametern hello logik app nyttolast:</span><span class="sxs-lookup"><span data-stu-id="07043-164">Because a default value no longer exists, hello logic app payload requires this parameter:</span></span>

```
{
    "properties": {},
        "definition": {
          // Use hello definition from above here
        },
        "parameters": {
            "connection": {
                "value": "https://my.connection.that.is.per.enviornment"
            }
        }
    },
    "location": "westus"
}
``` 

<span data-ttu-id="07043-165">Varje miljö kan du ange ett annat värde för hello `connection` parameter.</span><span class="sxs-lookup"><span data-stu-id="07043-165">In each environment, you can provide a different value for hello `connection` parameter.</span></span> 

<span data-ttu-id="07043-166">Alla hello alternativ som du har för att skapa och hantera logikappar finns hello [REST API-dokumentation](https://msdn.microsoft.com/library/azure/mt643787.aspx).</span><span class="sxs-lookup"><span data-stu-id="07043-166">For all hello options that you have for creating and managing logic apps, see hello [REST API documentation](https://msdn.microsoft.com/library/azure/mt643787.aspx).</span></span> 
