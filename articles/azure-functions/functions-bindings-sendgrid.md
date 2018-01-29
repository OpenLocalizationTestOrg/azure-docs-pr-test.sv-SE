---
title: Azure Functions SendGrid bindningar
description: Azure Functions SendGrid bindningar referens.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/29/2017
ms.author: tdykstra
ms.openlocfilehash: aa64bbc764fe96d7c15d62660114222458e5acf4
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/02/2018
---
# <a name="azure-functions-sendgrid-bindings"></a>Azure Functions SendGrid bindningar

Den här artikeln beskrivs hur du skickar e-post med hjälp av [SendGrid](https://sendgrid.com/docs/User_Guide/index.html) bindningar i Azure Functions. Azure Functions stöder en output-bindning för SendGrid.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="example"></a>Exempel

Finns i det språkspecifika:

* [C#](#c-example)
* [C#-skript (.csx)](#c-script-example)
* [JavaScript](#javascript-example)

### <a name="c-example"></a>C#-exempel

Följande exempel visar en [C#-funktionen](functions-dotnet-class-library.md) att utlösa för använder en Service Bus-kö och en SendGrid utdatabindning.

```cs
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] out SendGridMessage message)
{
    message = new SendGridMessage();
    message.AddTo(email.To);
    message.AddContent("text/html", email.Body);
    message.SetFrom(new EmailAddress(email.From));
    message.SetSubject(email.Subject);
}

public class OutgoingEmail
{
    public string To { get; set; }
    public string From { get; set; }
    public string Subject { get; set; }
    public string Body { get; set; }
}
```

Kan du utelämna inställningen attributets `ApiKey` egenskapen om du har din API-nyckel i en appinställning med namnet ”AzureWebJobsSendGridApiKey”.

### <a name="c-script-example"></a>Exempel på C#-skript

I följande exempel visas en SendGrid bindning i en *function.json* fil och en [C#-skriptfunktion](functions-reference-csharp.md) som använder bindningen.

Här är de bindande data den *function.json* fil:

```json 
{
    "bindings": [
        {
            "name": "message",
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

Den [configuration](#configuration) förklaras de här egenskaperna.

Här är skriptkod C#:

```csharp
#r "SendGrid"
using System;
using SendGrid.Helpers.Mail;

public static void Run(TraceWriter log, string input, out Mail message)
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

### <a name="javascript-example"></a>JavaScript-exempel

I följande exempel visas en SendGrid bindning i en *function.json* fil och en [JavaScript-funktionen](functions-reference-node.md) som använder bindningen.

Här är de bindande data den *function.json* fil:

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

Den [configuration](#configuration) förklaras de här egenskaperna.

Här är JavaScript-kod:

```javascript
module.exports = function (context, input) {    
    var message = {
         "personalizations": [ { "to": [ { "email": "sample@sample.com" } ] } ],
        from: { email: "sender@contoso.com" },        
        subject: "Azure news",
        content: [{
            type: 'text/plain',
            value: input
        }]
    };

    context.done(null, message);
};
```

## <a name="attributes"></a>Attribut

I [C#-klassbibliotek](functions-dotnet-class-library.md), använda den [SendGrid](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs) attribut som har definierats i NuGet-paketet [Microsoft.Azure.WebJobs.Extensions.SendGrid](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid).

Information om egenskaper för attribut som du kan konfigurera finns [Configuration](#configuration). Här är en `SendGrid` attributet exempel i en signatur:

```csharp
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] out SendGridMessage message)
{
    ...
}
```

En komplett exempel finns [C#-exempel](#c-example).

## <a name="configuration"></a>Konfiguration

I följande tabell beskrivs konfigurationsegenskaper för bindning som du anger i den *function.json* fil och `SendGrid` attribut.

|Egenskapen Function.JSON | Egenskap |Beskrivning|
|---------|---------|----------------------|
|**typ**|| Krävs – måste vara inställd på `sendGrid`.|
|**riktning**|| Krävs – måste vara inställd på `out`.|
|**Namn**|| Obligatoriskt - variabelnamnet som används i Funktionskoden för begäran eller begärandetexten. Det här värdet är ```$return``` när det är endast ett returvärde. |
|**apiKey**|**ApiKey**| Namnet på en appinställning som innehåller API-nyckel. Om inte aktiverad, standardappen inställningen är ”AzureWebJobsSendGridApiKey”.|
|**att**|**Till**| mottagarens e-postadress. |
|**från**|**Från**| avsändarens e-postadress. |
|**ämne**|**Ämne**| ämnet för e-postmeddelandet. |
|**text**|**Text**| e-postinnehåll. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Lär dig mer om Azure functions-utlösare och bindningar](functions-triggers-bindings.md)