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
# <a name="create-workflow-definitions-for-logic-apps-using-json"></a>Skapa arbetsflödesdefinitioner för logic apps med JSON

Du kan skapa arbetsflödesdefinitioner för [Azure Logikappar](logic-apps-what-are-logic-apps.md) med enkla, deklarativa JSON-språk. Om du inte redan gjort först granska [hur toocreate logikappen första med logik App Designer](logic-apps-create-a-logic-app.md). Se även hello [fullständig referens för hello språk i arbetsflödesdefinitionen](http://aka.ms/logicappsdocs).

## <a name="repeat-steps-over-a-list"></a>Upprepa steg över en lista

tooiterate via en matris som har upp too10 000 objekt och utföra en åtgärd för varje objekt kan använda hello [foreach typen](logic-apps-loops-and-scopes.md).

## <a name="handle-failures-if-something-goes-wrong"></a>Hantera fel om något går fel

Vanligtvis vill tooinclude en *reparation steg* – vissa logik som kör *endast om* misslyckas för en eller flera av dina anrop. Det här exemplet hämtar data från olika platser, men om hello anropet misslyckas, vi tooPOST meddelandet någonstans så att vi kan spåra att fel senare:  

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

toospecify som `postToErrorMessageQueue` körs bara `readData` har `Failed`, använda hello `runAfter` egenskap, till exempel toospecify en lista över möjliga värden så att `runAfter` kan vara `["Succeeded", "Failed"]`.

Slutligen, eftersom det här exemplet hanterar nu hello fel, vi inte längre markera hello kör som- `Failed`. Eftersom vi har lagt till hello steg för att hantera detta fel i det här exemplet hello kör har `Succeeded` även om ett steg `Failed`.

## <a name="execute-two-or-more-steps-in-parallel"></a>Köra två eller flera steg parallellt

toorun flera åtgärder parallellt, hello `runAfter` egenskapen måste ha samma vid körning. 

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

I det här exemplet både `branch1` och `branch2` ställs toorun efter `readData`. Därför körs båda grenar parallellt. hello tidsstämpel för båda filialer är identiska.

![Parallell](media/logic-apps-author-definitions/parallel.png)

## <a name="join-two-parallel-branches"></a>Ansluta två parallella grenar

Du kan delta i två åtgärder som är inställda toorun parallellt genom att lägga till objekt toohello `runAfter` egenskap som hello föregående exempel.

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

## <a name="map-list-items-tooa-different-configuration"></a>Mappa lista objekt tooa annan konfiguration

Sedan anta att vi vill tooget olika innehåll baserat på hello-värdet för en egenskap. Vi kan skapa en karta över värden toodestinations som en parameter:  

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

I det här fallet hämta vi först en lista över artiklar. Hello andra steget använder en karta toolook in hello URL: en för att hämta hello innehåll baserat på hello kategori som har definierats som en parameter.

Några tillfällen toonote här: 

*   Hej [ `intersection()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) funktionen kontrollerar om hello kategori matchar en av hello kända angivna kategorier.

*   När vi har fått hello kategori, hämtar vi hello objekt från hello karta med hakparenteser:`parameters[...]`

## <a name="process-strings"></a>Process-strängar

Du kan använda olika funktioner toomanipulate strängar. Anta exempelvis att vi har en sträng som vi vill toopass tooa system, men vi inte är säker på om korrekt hantering för teckenkodning. Ett alternativ är toobase64 koda den här strängen. Dock tooavoid undantagstecken i URL-adresser, vi tooreplace några tecken. 

Vi vill även en understräng hello ordning namn eftersom hello första fem tecknen inte används.

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

Fungerar från inuti toooutside:

1. Hämta hello [ `length()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) för hello orderer namn, så vi komma hello Totalt antal tecken.

2. Subtrahera 5 eftersom vi vill att en kortare sträng.

3. Faktiskt, ta hello [ `substring()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring). Vi börjar vid index `5` och gå hello resten av hello-sträng.

4. Konvertera den här delsträngen tooa [ `base64()` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) sträng.

5. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)alla hello `+` tecken med `-` tecken.

6. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)alla hello `/` tecken med `_` tecken.

## <a name="work-with-date-times"></a>Arbeta med datum

Datum kan vara användbart, särskilt när du försöker toopull data från en datakälla som inte stöder naturligt *utlösare*. Du kan också använda datum för att söka efter hur länge olika steg tar.

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

I det här exemplet vi extrahera hello `startTime` hello föregående steg. Sedan vi hämta hello aktuell tid och ta bort en sekund:

[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) 

Du kan använda andra tidsenheter, som `minutes` eller `hours`. Slutligen kan vi Jämför dessa två värden. Om hello första värdet är mindre än andra hello-värdet och mer än en sekund har gått sedan hello beställning gjordes först.

tooformat datum vi kan använda string-formaterare. Till exempel tooget hello RFC1123, vi använder [ `utcnow('r')` ](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow). toolearn formatera datum, se [språk i arbetsflödesdefinitionen](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow).

## <a name="deployment-parameters-for-different-environments"></a>Distributionsparametrarna för olika miljöer

Distribution livscykler har vanligtvis en utvecklingsmiljö, en mellanlagringsmiljön och en produktionsmiljö. Du kan till exempel använda hello samma definition i dessa miljöer, men använder olika databaser. På samma sätt kan du kanske vill toouse hello samma definition över olika regioner för hög tillgänglighet men vill varje logik app-instansen tootalk toothat regions-databasen.
Det här scenariot skiljer sig från tar parametrar på *runtime* där i stället du bör använda hello `trigger()` fungera som hello föregående exempel.

Du kan börja med en grundläggande definition som det här exemplet:

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

I hello faktiska `PUT` begäran för hello logic apps kan du ange hello parametern `uri`. Eftersom det finns inte längre ett standardvärde, kräver den här parametern hello logik app nyttolast:

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

Varje miljö kan du ange ett annat värde för hello `connection` parameter. 

Alla hello alternativ som du har för att skapa och hantera logikappar finns hello [REST API-dokumentation](https://msdn.microsoft.com/library/azure/mt643787.aspx). 
