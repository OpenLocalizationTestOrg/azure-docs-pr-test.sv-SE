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
# <a name="logic-apps-loops-scopes-and-debatching"></a>Logic Apps-slingor, -omfattningar och Debatching
  
Logic Apps innehåller ett antal sätt toowork med matriser, samlingar, batchar och loopar i ett arbetsflöde.
  
## <a name="foreach-loop-and-arrays"></a>ForEach-loop och matriser
  
Logic Apps kan du tooloop över en mängd av data och utföra en åtgärd för varje objekt.  Det är möjligt via hello `foreach` åtgärd.  Du kan ange tooadd i hello designer, ett för varje loop.  När du har valt hello matris som du vill tooiterate över, kan du börja lägga till åtgärder.  Är för närvarande begränsad tooonly en åtgärd per foreach loop, men den här begränsningen ska dras tillbaka i hello kommer veckor.  En gång i hello loop kan du börja toospecify vad ska ske vid varje värde i hello-matrisen.

Om du använder vyn kod, kan du ange en för varje loop som nedan.  Detta är ett exempel på en för varje loop som skickar ett e-postmeddelande för varje e-postadress som innehåller ”microsoft.com”:

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
  
  En `foreach` åtgärd kan iterera över matriser in too5 000 rader.  Varje iteration körs parallellt som standard.  

### <a name="sequential-foreach-loops"></a>Sekventiella ForEach slingor

tooenable en foreach loop tooexecute hello sekventiellt, `Sequential` åtgärden alternativet ska läggas till.

``` json
"forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "operationOptions": "Sequential",
        "..."
}
```
  
## <a name="until-loop"></a>Tills loop
  
  Du kan utföra en åtgärd eller en serie åtgärder förrän ett villkor uppfylls.  hello anropar vanligaste scenariot för detta en slutpunkt förrän du får hello-svar som du letar efter.  Du kan ange tooadd i hello designer en tills loop.  När du lägger till åtgärder i hello loop kan du ange villkoret för avslutning av hello, samt hello loop gränser.  Det finns en minuts fördröjning mellan loop cykler.
  
  Om du använder vyn kod, kan du ange en tills loop som nedan.  Detta är ett exempel på en HTTP-slutpunkt anropas förrän hello svarstexten har hello värde 'Slutförd'.  Slutförs när antingen 
  
  * HTTP-svar som har statusen ”Slutförd”
  * Det har försökt för 1 timme
  * Det har körts i en slinga 100 gånger
  
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
  
## <a name="spliton-and-debatching"></a>SplitOn och debatching

En utlösare kan ibland visas en matris med objekt som du vill använda toodebatch och starta ett arbetsflöde per artikel.  Detta kan åstadkommas via hello `spliton` kommando.  Som standard om utlösaren-swagger anger en nyttolast som är en matris, en `spliton` kommer att läggas till och starta en körning per artikel.  SplitOn kan bara läggas tooa utlösare.  Detta kan manuellt konfigurerade eller åsidosätts i definition kodvy.  SplitOn kan för närvarande debatch matriser in too5 000 objekt.  Du kan inte ha en `spliton` och även implementera hello synkron svar mönster.  Alla arbetsflöden som har en `response` åtgärd dessutom för`spliton` körs asynkront och skicka en omedelbar `202 Accepted` svar.  

SplitOn kan anges i vyn kod som hello följande exempel.  Detta tar emot en matris med objekt och debatches på varje rad.

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

## <a name="scopes"></a>Scope

Det är möjligt toogroup en serie åtgärder tillsammans med en omfattning.  Detta är särskilt användbart för att implementera undantagshantering.  I hello designer kan du lägga till ett nytt scope och börja lägga till alla åtgärder i den.  Du kan definiera scope i vyn hello följande kod:


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