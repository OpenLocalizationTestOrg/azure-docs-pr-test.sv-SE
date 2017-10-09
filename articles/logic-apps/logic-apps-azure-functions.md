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
# <a name="add-and-run-custom-code-for-logic-apps-through-azure-functions"></a>Lägg till och köra anpassad kod för logic apps via Azure Functions

toorun anpassade kodavsnitt för C# eller node.js i logikappar, kan du skapa anpassade funktioner via Azure Functions. 
[Azure Functions](../azure-functions/functions-overview.md) erbjuder server-dator i Microsoft Azure och är användbara för att utföra dessa uppgifter:

* Avancerad formatering eller beräknade fält i logikappar
* Utföra beräkningar i ett arbetsflöde.
* Utöka hello logik funktionalitet med funktioner som stöds i C# eller node.js

## <a name="create-custom-functions-for-your-logic-apps"></a>Skapa anpassade funktioner för dina logic apps

Vi rekommenderar att du skapar en funktion i hello Azure Functions-portalen från hello **allmän Webhook - nod** eller **allmän Webhook - C#** mallar. hello resultatet skapas en auto-fylls i en mall som accepterar `application/json` från en logikapp. Funktioner som du skapar från dessa mallar identifieras automatiskt och visas i hello logik App Designer under **Azure Functions i min region.**

I hello Azure-portalen på hello **integrera** fönstret för din funktion mallen ska visas som **läge** ställa in också**Webhook** och **Webhook typen** har angetts för**allmänna JSON**. 

Webhook-funktioner acceptera en begäran och skicka den till hello metoden via en `data` variabel. Du kan komma åt hello egenskaper för din nyttolast med hjälp av punktnotation som `data.function-name`. Till exempel en enkel JavaScript-funktion som konverterar ett DateTime-värde till en datumsträng ser ut så hello följande exempel:

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-logic-apps"></a>Anropa Azure Functions från logikappar

toolist hello behållare i din prenumeration och välj hello-funktionen som du vill toocall i logik App Designer klickar du på hello **åtgärder** -menyn och välj bland **Azure Functions i min Region**.

När du har valt hello-funktionen är och toospecify objektets nyttolasten indata. Det här objektet är hello-meddelande som hello logik appen skickar toohello funktion och måste vara ett JSON-objekt. Om du vill toopass i hello exempelvis **ändrades senast** datum från en Salesforce-utlösare hello funktionen nyttolasten kan se ut som i det här exemplet:

![Senast ändrad datum][1]

## <a name="trigger-logic-apps-from-a-function"></a>Utlösaren logikappar från en funktion

Du kan utlösa en logikapp inuti en funktion. Se [Logic apps som callable slutpunkter](logic-apps-http-endpoint.md). Skapa en logikapp som har en manuell utlösare och sedan generera från en HTTP POST toohello manuell utlösaren URL med hello-nyttolast som du vill skicka toohello logikapp i din funktion.

### <a name="create-a-function-from-logic-app-designer"></a>Skapa en funktion från logik App Designer

Du kan också skapa en webhook node.js-funktion från hello designer. Välj först **Azure Functions i min Region** och välj sedan en behållare för din funktion. Om du ännu inte har en behållare, måste en toocreate från hello [Azure Functions-portalen](https://functions.azure.com/signin). Välj sedan **Skapa nytt**.  

toogenerate en mall baserat på hello data som du vill toocompute, ange hello context-objektet som du planerar toopass i en funktion. Det här objektet måste vara ett JSON-objekt. Till exempel om du skickar i hello innehåll från en FTP-åtgärd hello kontexten nyttolast ser ut som i det här exemplet:

![Nyttolasten i kontexten][2]

> [!NOTE]
> Eftersom det inte att omvandla det här objektet som en sträng, hello innehåll läggs direkt toohello JSON-nyttolast. Dock felmeddelande ett om hello-objektet inte är en JSON-token (det vill säga en sträng eller en JSON-objekt /-matris). toocast hello-objektet som en sträng, lägga till citattecken som hello första bilden i den här artikeln.
> 

hello designer skapar sedan en mall för funktionen att du kan skapa infogade. Variabler skapas före baserat på hello kontext som du planerar toopass till hello-funktionen.

<!--Image references-->
[1]: ./media/logic-apps-azure-functions/callfunction.png
[2]: ./media/logic-apps-azure-functions/createfunction.png
