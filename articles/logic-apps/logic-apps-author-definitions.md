---
title: "Definiera arbetsflöden med JSON - Azure Logic Apps | Microsoft Docs"
description: "Hur du skriver arbetsflödesdefinitioner i JSON för logic apps"
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
ms.openlocfilehash: 7f9e5a10066df8a464c285273e77a85c0d562ebb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-workflow-definitions-for-logic-apps-using-json"></a><span data-ttu-id="0e950-103">Skapa arbetsflödesdefinitioner för logic apps med JSON</span><span class="sxs-lookup"><span data-stu-id="0e950-103">Create workflow definitions for logic apps using JSON</span></span>

<span data-ttu-id="0e950-104">Du kan skapa arbetsflödesdefinitioner för [Azure Logikappar](logic-apps-what-are-logic-apps.md) med enkla, deklarativa JSON-språk.</span><span class="sxs-lookup"><span data-stu-id="0e950-104">You can create workflow definitions for [Azure Logic Apps](logic-apps-what-are-logic-apps.md) with simple, declarative JSON language.</span></span> <span data-ttu-id="0e950-105">Om du inte redan gjort först granska [hur du skapar din första logikapp med logik App Designer](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="0e950-105">If you haven't already, first review [how to create your first logic app with Logic App Designer](logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="0e950-106">Se även den [fullständig referens för arbetsflödet Definition Language](http://aka.ms/logicappsdocs).</span><span class="sxs-lookup"><span data-stu-id="0e950-106">Also, see the [full reference for the Workflow Definition Language](http://aka.ms/logicappsdocs).</span></span>

## <a name="repeat-steps-over-a-list"></a><span data-ttu-id="0e950-107">Upprepa steg över en lista</span><span class="sxs-lookup"><span data-stu-id="0e950-107">Repeat steps over a list</span></span>

<span data-ttu-id="0e950-108">Gå igenom en matris som har upp till 10 000 objekt och utföra en åtgärd för varje objekt genom att använda den [foreach typen](logic-apps-loops-and-scopes.md).</span><span class="sxs-lookup"><span data-stu-id="0e950-108">To iterate through an array that has up to 10,000 items and perform an action for each item, use the [foreach type](logic-apps-loops-and-scopes.md).</span></span>

## <a name="handle-failures-if-something-goes-wrong"></a><span data-ttu-id="0e950-109">Hantera fel om något går fel</span><span class="sxs-lookup"><span data-stu-id="0e950-109">Handle failures if something goes wrong</span></span>

<span data-ttu-id="0e950-110">Vanligtvis du vill inkludera en *reparation steg* – vissa logik som kör *endast om* misslyckas för en eller flera av dina anrop.</span><span class="sxs-lookup"><span data-stu-id="0e950-110">Usually, you want to include a *remediation step* — some logic that executes *if and only if* one or more of your calls fail.</span></span> <span data-ttu-id="0e950-111">Det här exemplet hämtar data från olika platser, men om anropet misslyckas, vi vill skicka ett meddelande någonstans så att vi kan spåra att fel senare:</span><span class="sxs-lookup"><span data-stu-id="0e950-111">This example gets data from various places, but if the call fails, we want to POST a message somewhere so we can track down that failure later:</span></span>  

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

<span data-ttu-id="0e950-112">Du vill ange att `postToErrorMessageQueue` körs bara `readData` har `Failed`, använda den `runAfter` egenskap, till exempel vill ange en lista över möjliga värden så att `runAfter` kan vara `["Succeeded", "Failed"]`.</span><span class="sxs-lookup"><span data-stu-id="0e950-112">To specify that `postToErrorMessageQueue` only runs after `readData` has `Failed`, use the `runAfter` property, for example, to specify a list of possible values, so that `runAfter` could be `["Succeeded", "Failed"]`.</span></span>

<span data-ttu-id="0e950-113">Slutligen, eftersom det här exemplet hanterar nu felet, vi inte längre markera kör som- `Failed`.</span><span class="sxs-lookup"><span data-stu-id="0e950-113">Finally, because this example now handles the error, we no longer mark the run as `Failed`.</span></span> <span data-ttu-id="0e950-114">Eftersom vi har lagt till steg för att hantera detta fel i det här exemplet kör har `Succeeded` även om ett steg `Failed`.</span><span class="sxs-lookup"><span data-stu-id="0e950-114">Because we added the step for handling this failure in this example, the run has `Succeeded` although one step `Failed`.</span></span>

## <a name="execute-two-or-more-steps-in-parallel"></a><span data-ttu-id="0e950-115">Köra två eller flera steg parallellt</span><span class="sxs-lookup"><span data-stu-id="0e950-115">Execute two or more steps in parallel</span></span>

<span data-ttu-id="0e950-116">Att köra flera åtgärder parallellt, den `runAfter` egenskapen måste ha samma vid körning.</span><span class="sxs-lookup"><span data-stu-id="0e950-116">To run multiple actions in parallel, the `runAfter` property must be equivalent at runtime.</span></span> 

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

<span data-ttu-id="0e950-117">I det här exemplet både `branch1` och `branch2` är inställd på att köras `readData`.</span><span class="sxs-lookup"><span data-stu-id="0e950-117">In this example, both `branch1` and `branch2` are set to run after `readData`.</span></span> <span data-ttu-id="0e950-118">Därför körs båda grenar parallellt.</span><span class="sxs-lookup"><span data-stu-id="0e950-118">As a result, both branches run in parallel.</span></span> <span data-ttu-id="0e950-119">Tidsstämpel för båda filialer är identiska.</span><span class="sxs-lookup"><span data-stu-id="0e950-119">The timestamp for both branches is identical.</span></span>

![Parallell](media/logic-apps-author-definitions/parallel.png)

## <a name="join-two-parallel-branches"></a><span data-ttu-id="0e950-121">Ansluta två parallella grenar</span><span class="sxs-lookup"><span data-stu-id="0e950-121">Join two parallel branches</span></span>

<span data-ttu-id="0e950-122">Du kan delta i två åtgärder som körs parallellt genom att lägga till objekt i den `runAfter` egenskapen som i föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="0e950-122">You can join two actions that are set to run in parallel by adding items to the `runAfter` property as in the previous example.</span></span>

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

## <a name="map-list-items-to-a-different-configuration"></a><span data-ttu-id="0e950-124">Mappa objekt till en annan konfiguration</span><span class="sxs-lookup"><span data-stu-id="0e950-124">Map list items to a different configuration</span></span>

<span data-ttu-id="0e950-125">Sedan anta att vi vill hämta olika innehåll baserat på värdet för en egenskap.</span><span class="sxs-lookup"><span data-stu-id="0e950-125">Next, let's say that we want to get different content based on the value of a property.</span></span> <span data-ttu-id="0e950-126">Vi kan skapa en karta över värden till mål som en parameter:</span><span class="sxs-lookup"><span data-stu-id="0e950-126">We can create a map of values to destinations as a parameter:</span></span>  

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

<span data-ttu-id="0e950-127">I det här fallet hämta vi först en lista över artiklar.</span><span class="sxs-lookup"><span data-stu-id="0e950-127">In this case, we first get a list of articles.</span></span> <span data-ttu-id="0e950-128">Det andra steget används baserat på den kategori som har definierats som en parameter, en karta för att slå upp URL: en för att hämta innehållet.</span><span class="sxs-lookup"><span data-stu-id="0e950-128">Based on the category that was defined as a parameter, the second step uses a map to look up the URL for getting the content.</span></span>

<span data-ttu-id="0e950-129">Vissa tidpunkter Observera:</span><span class="sxs-lookup"><span data-stu-id="0e950-129">Some times to note here:</span></span> 

*   <span data-ttu-id="0e950-130">Den [ `intersection()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) funktionen kontrollerar om kategorin matchar ett av de angivna kategorierna kända.</span><span class="sxs-lookup"><span data-stu-id="0e950-130">The [`intersection()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) function checks whether the category matches one of the known defined categories.</span></span>

*   <span data-ttu-id="0e950-131">När vi har fått kategorin hämtar vi objekt från mappningen med hakparenteser:`parameters[...]`</span><span class="sxs-lookup"><span data-stu-id="0e950-131">After we get the category, we can pull the item from the map using square brackets: `parameters[...]`</span></span>

## <a name="process-strings"></a><span data-ttu-id="0e950-132">Process-strängar</span><span class="sxs-lookup"><span data-stu-id="0e950-132">Process strings</span></span>

<span data-ttu-id="0e950-133">Du kan använda olika funktioner för att ändra strängar.</span><span class="sxs-lookup"><span data-stu-id="0e950-133">You can use various functions to manipulate strings.</span></span> <span data-ttu-id="0e950-134">Anta exempelvis att vi har en sträng som vi ska skickas till ett system, men vi inte är säker på om korrekt hantering för teckenkodning.</span><span class="sxs-lookup"><span data-stu-id="0e950-134">For example, suppose we have a string that we want to pass to a system, but we aren't confident about proper handling for character encoding.</span></span> <span data-ttu-id="0e950-135">Ett alternativ är att base64 koda den här strängen.</span><span class="sxs-lookup"><span data-stu-id="0e950-135">One option is to base64 encode this string.</span></span> <span data-ttu-id="0e950-136">Men om du vill undvika undantagstecken i URL-adresser ska ersätta några tecken.</span><span class="sxs-lookup"><span data-stu-id="0e950-136">However, to avoid escaping in a URL, we are going to replace a few characters.</span></span> 

<span data-ttu-id="0e950-137">Vi vill även en understräng i den ordning namn eftersom de första fem tecknen inte används.</span><span class="sxs-lookup"><span data-stu-id="0e950-137">We also want a substring of the order's name because the first five characters are not used.</span></span>

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

<span data-ttu-id="0e950-138">Arbeta från inuti till utanför:</span><span class="sxs-lookup"><span data-stu-id="0e950-138">Working from inside to outside:</span></span>

1. <span data-ttu-id="0e950-139">Hämta den [ `length()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) för den orderer namn, så vi få tillbaka det totala antalet tecken.</span><span class="sxs-lookup"><span data-stu-id="0e950-139">Get the [`length()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) for the orderer's name, so we get back the total number of characters.</span></span>

2. <span data-ttu-id="0e950-140">Subtrahera 5 eftersom vi vill att en kortare sträng.</span><span class="sxs-lookup"><span data-stu-id="0e950-140">Subtract 5 because we want a shorter string.</span></span>

3. <span data-ttu-id="0e950-141">Faktiskt, ta den [ `substring()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring).</span><span class="sxs-lookup"><span data-stu-id="0e950-141">Actually, take the [`substring()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring).</span></span> <span data-ttu-id="0e950-142">Vi börjar vid index `5` och gå resten av strängen.</span><span class="sxs-lookup"><span data-stu-id="0e950-142">We start at index `5` and go the remainder of the string.</span></span>

4. <span data-ttu-id="0e950-143">Konvertera den här delsträng som en [ `base64()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) sträng.</span><span class="sxs-lookup"><span data-stu-id="0e950-143">Convert this substring to a [`base64()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) string.</span></span>

5. <span data-ttu-id="0e950-144">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)alla de `+` tecken med `-` tecken.</span><span class="sxs-lookup"><span data-stu-id="0e950-144">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace) all the `+` characters with `-` characters.</span></span>

6. <span data-ttu-id="0e950-145">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)alla de `/` tecken med `_` tecken.</span><span class="sxs-lookup"><span data-stu-id="0e950-145">[`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace) all the `/` characters with `_` characters.</span></span>

## <a name="work-with-date-times"></a><span data-ttu-id="0e950-146">Arbeta med datum</span><span class="sxs-lookup"><span data-stu-id="0e950-146">Work with Date Times</span></span>

<span data-ttu-id="0e950-147">Datum kan vara användbart, särskilt när du försöker hämta data från en datakälla som inte stöder naturligt *utlösare*.</span><span class="sxs-lookup"><span data-stu-id="0e950-147">Date Times can be useful, particularly when you are trying to pull data from a data source that doesn't naturally support *triggers*.</span></span> <span data-ttu-id="0e950-148">Du kan också använda datum för att söka efter hur länge olika steg tar.</span><span class="sxs-lookup"><span data-stu-id="0e950-148">You can also use Date Times for finding how long various steps are taking.</span></span>

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

<span data-ttu-id="0e950-149">I det här exemplet vi extrahera den `startTime` från föregående steg.</span><span class="sxs-lookup"><span data-stu-id="0e950-149">In this example, we extract the `startTime` from the previous step.</span></span> <span data-ttu-id="0e950-150">Sedan vi hämta den aktuella tiden och ta bort en sekund:</span><span class="sxs-lookup"><span data-stu-id="0e950-150">Then we get the current time, and subtract one second:</span></span>

[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) 

<span data-ttu-id="0e950-151">Du kan använda andra tidsenheter, som `minutes` eller `hours`.</span><span class="sxs-lookup"><span data-stu-id="0e950-151">You can use other units of time, like `minutes` or `hours`.</span></span> <span data-ttu-id="0e950-152">Slutligen kan vi Jämför dessa två värden.</span><span class="sxs-lookup"><span data-stu-id="0e950-152">Finally, we can compare these two values.</span></span> <span data-ttu-id="0e950-153">Om det första värdet är mindre än det andra värdet och sedan mer än en sekund har gått sedan ordern gjordes.</span><span class="sxs-lookup"><span data-stu-id="0e950-153">If the first value is less than the second value, then more than one second has passed since the order was first placed.</span></span>

<span data-ttu-id="0e950-154">Vi kan använda sträng formaterare för att formatera datum.</span><span class="sxs-lookup"><span data-stu-id="0e950-154">To format dates, we can use string formatters.</span></span> <span data-ttu-id="0e950-155">Till exempel för att få RFC1123 kan vi använda [ `utcnow('r')` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span><span class="sxs-lookup"><span data-stu-id="0e950-155">For example, to get the RFC1123, we use [`utcnow('r')`](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span></span> <span data-ttu-id="0e950-156">Läs om datumformatering i [språk i arbetsflödesdefinitionen](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span><span class="sxs-lookup"><span data-stu-id="0e950-156">To learn about date formatting, see [Workflow Definition Language](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).</span></span>

## <a name="deployment-parameters-for-different-environments"></a><span data-ttu-id="0e950-157">Distributionsparametrarna för olika miljöer</span><span class="sxs-lookup"><span data-stu-id="0e950-157">Deployment parameters for different environments</span></span>

<span data-ttu-id="0e950-158">Distribution livscykler har vanligtvis en utvecklingsmiljö, en mellanlagringsmiljön och en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="0e950-158">Commonly, deployment lifecycles have a development environment, a staging environment, and a production environment.</span></span> <span data-ttu-id="0e950-159">Du kan till exempel använda samma definition i dessa miljöer men använder olika databaser.</span><span class="sxs-lookup"><span data-stu-id="0e950-159">For example, you might use the same definition in all these environments but use different databases.</span></span> <span data-ttu-id="0e950-160">På samma sätt kan du använda samma definition över olika regioner för hög tillgänglighet, utan varje logik app-instansen tala med den regionen databasen.</span><span class="sxs-lookup"><span data-stu-id="0e950-160">Likewise, you might want to use the same definition across different regions for high availability but want each logic app instance to talk to that region's database.</span></span>
<span data-ttu-id="0e950-161">Det här scenariot skiljer sig från tar parametrar på *runtime* där i stället du bör använda den `trigger()` fungera som i föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="0e950-161">This scenario differs from taking parameters at *runtime* where instead, you should use the `trigger()` function as in the previous example.</span></span>

<span data-ttu-id="0e950-162">Du kan börja med en grundläggande definition som det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="0e950-162">You can start with a basic definition like this example:</span></span>

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

<span data-ttu-id="0e950-163">I den faktiska `PUT` begäran för logic apps kan du ange parametern `uri`.</span><span class="sxs-lookup"><span data-stu-id="0e950-163">In the actual `PUT` request for the logic apps, you can provide the parameter `uri`.</span></span> <span data-ttu-id="0e950-164">Eftersom det finns inte längre ett standardvärde, kräver den här parametern logik app nyttolasten:</span><span class="sxs-lookup"><span data-stu-id="0e950-164">Because a default value no longer exists, the logic app payload requires this parameter:</span></span>

```
{
    "properties": {},
        "definition": {
          // Use the definition from above here
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

<span data-ttu-id="0e950-165">Varje miljö kan du ange ett annat värde för den `connection` parameter.</span><span class="sxs-lookup"><span data-stu-id="0e950-165">In each environment, you can provide a different value for the `connection` parameter.</span></span> 

<span data-ttu-id="0e950-166">Alla de alternativ som du har för att skapa och hantera logic apps finns i [REST API-dokumentation](https://msdn.microsoft.com/library/azure/mt643787.aspx).</span><span class="sxs-lookup"><span data-stu-id="0e950-166">For all the options that you have for creating and managing logic apps, see the [REST API documentation](https://msdn.microsoft.com/library/azure/mt643787.aspx).</span></span> 
