---
title: aaaAzure funktioner SendGrid bindningar | Microsoft Docs
description: Azure Functions SendGrid bindningar referens
services: functions
documentationcenter: na
author: rachelappel
manager: erikre
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/16/2017
ms.author: rachelap
ms.openlocfilehash: 10a3837875eb6ae18e6c789bcf64cc401cf5f26a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-sendgrid-bindings"></a>Azure Functions SendGrid bindningar

Den här artikeln förklarar hur tooconfigure och arbeta med SendGrid-bindningar i Azure Functions. Med SendGrid, kan du använda Azure Functions toosend anpassade e-post via programmering.

Den här artikeln är referensinformation för Azure Functions-utvecklare. Om du är ny tooAzure funktioner kan du börja med hello följande resurser:

[Skapa din första Azure-funktion](functions-create-first-azure-function.md). 
[C#](functions-reference-csharp.md), [F #](functions-reference-fsharp.md), eller [nod](functions-reference-node.md) developer referenser.

## <a name="functionjson-for-sendgrid-bindings"></a>Function.JSON för SendGrid bindningar

Azure Functions innehåller en output-bindning för SendGrid. Hej SendGrid utdata bindning kan du toocreate och skicka e-post via programmering. 

Hej SendGrid bindningen stöder hello följande egenskaper:

- `name`: Krävs – hello variabelnamn som används i Funktionskoden för hello begäran eller begärandetexten. Det här värdet är ```$return``` när det är endast ett returvärde. 
- `type`: Krävs – måste anges för ”sendGrid”.
- `direction`: Krävs – måste anges för ”out”.
- `apiKey`: Krävs – måste vara toohello namn för din API-nyckel som lagras i appen hello-funktionen app-inställningar.
- `to`: hello mottagarens e-postadress.
- `from`: hello avsändarens e-postadress.
- `subject`: hello ämnet för e-hello.
- `text`: hello e-postinnehåll.

Exempel på **function.json**:

```json 
{
    "bindings": [
        {
            "name": "$return",
            "type": "sendGrid",
            "direction": "out",
            "apiKey" : "MySendGridKey",
            "to": "{ToEmail}",
            "from": "{FromEmail}",
            "subject": "SendGrid output bindings"
        }
    ]
}
```

> [!NOTE]
> Azure Functions lagrar dina anslutningsinformationen och API-nycklar som app-inställningar så att de inte kontrolleras i din källkontroll. Den här åtgärden skyddar känslig information.
>
>

## <a name="c-example-of-hello-sendgrid-output-binding"></a>C#-exempel på hello SendGrid utdatabindning

```csharp
#r "SendGrid"
using System;
using SendGrid.Helpers.Mail;

public static Mail Run(TraceWriter log, string input, out Mail message)
{
     message = new Mail
    {        
        Subject = "Azure news"          
    };

    var personalization = new Personalization();
    personalization.AddTo(new Email("recipient@contoso.com"));   

    Content content = new Content
    {
        Type = "text/plain",
        Value = input
    };
    message.AddContent(content);
    message.AddPersonalization(personalization);
}
```

## <a name="node-example-of-hello-sendgrid-output-binding"></a>Noden exempel på hello SendGrid utdata bindning

```javascript
module.exports = function (context, input) {    
    var message = {
         "personalizations": [ { "to": [ { "email": "sample@sample.com" } ] } ],
        from: "sender@contoso.com",        
        subject: "Azure news",
        content: [{
            type: 'text/plain',
            value: input
        }]
    };

    context.done(null, message);
};

```

## <a name="next-steps"></a>Nästa steg
Information om andra bindningar och utlösare för Azure Functions finns i 
- [Azure Functions-utlösare och bindningar för utvecklare](functions-triggers-bindings.md)

- [Metodtips för Azure Functions](functions-best-practices.md) innehåller vissa bästa praxis toouse när du skapar Azure Functions.

- [Azure Functions för utvecklare](functions-reference.md) programmerare om att koda funktioner och definiera utlösare och bindningar.
