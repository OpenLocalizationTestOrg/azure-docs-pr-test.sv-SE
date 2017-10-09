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
# <a name="azure-functions-sendgrid-bindings"></a><span data-ttu-id="daacf-103">Azure Functions SendGrid bindningar</span><span class="sxs-lookup"><span data-stu-id="daacf-103">Azure Functions SendGrid bindings</span></span>

<span data-ttu-id="daacf-104">Den här artikeln förklarar hur tooconfigure och arbeta med SendGrid-bindningar i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="daacf-104">This article explains how tooconfigure and work with SendGrid bindings in Azure Functions.</span></span> <span data-ttu-id="daacf-105">Med SendGrid, kan du använda Azure Functions toosend anpassade e-post via programmering.</span><span class="sxs-lookup"><span data-stu-id="daacf-105">With SendGrid, you can use Azure Functions toosend customized email programmatically.</span></span>

<span data-ttu-id="daacf-106">Den här artikeln är referensinformation för Azure Functions-utvecklare.</span><span class="sxs-lookup"><span data-stu-id="daacf-106">This article is reference information for Azure Functions developers.</span></span> <span data-ttu-id="daacf-107">Om du är ny tooAzure funktioner kan du börja med hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="daacf-107">If you're new tooAzure Functions, start with hello following resources:</span></span>

<span data-ttu-id="daacf-108">[Skapa din första Azure-funktion](functions-create-first-azure-function.md).</span><span class="sxs-lookup"><span data-stu-id="daacf-108">[Create your first Azure Function](functions-create-first-azure-function.md).</span></span> 
<span data-ttu-id="daacf-109">[C#](functions-reference-csharp.md), [F #](functions-reference-fsharp.md), eller [nod](functions-reference-node.md) developer referenser.</span><span class="sxs-lookup"><span data-stu-id="daacf-109">[C#](functions-reference-csharp.md), [F#](functions-reference-fsharp.md), or [Node](functions-reference-node.md) developer references.</span></span>

## <a name="functionjson-for-sendgrid-bindings"></a><span data-ttu-id="daacf-110">Function.JSON för SendGrid bindningar</span><span class="sxs-lookup"><span data-stu-id="daacf-110">function.json for SendGrid bindings</span></span>

<span data-ttu-id="daacf-111">Azure Functions innehåller en output-bindning för SendGrid.</span><span class="sxs-lookup"><span data-stu-id="daacf-111">Azure Functions provides an output binding for SendGrid.</span></span> <span data-ttu-id="daacf-112">Hej SendGrid utdata bindning kan du toocreate och skicka e-post via programmering.</span><span class="sxs-lookup"><span data-stu-id="daacf-112">hello SendGrid output binding enables you toocreate and send email programmatically.</span></span> 

<span data-ttu-id="daacf-113">Hej SendGrid bindningen stöder hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="daacf-113">hello SendGrid binding supports hello following properties:</span></span>

- <span data-ttu-id="daacf-114">`name`: Krävs – hello variabelnamn som används i Funktionskoden för hello begäran eller begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="daacf-114">`name` : Required - hello variable name used in function code for hello request or request body.</span></span> <span data-ttu-id="daacf-115">Det här värdet är ```$return``` när det är endast ett returvärde.</span><span class="sxs-lookup"><span data-stu-id="daacf-115">This value is ```$return``` when there is only one return value.</span></span> 
- <span data-ttu-id="daacf-116">`type`: Krävs – måste anges för ”sendGrid”.</span><span class="sxs-lookup"><span data-stu-id="daacf-116">`type` : Required - must be set too"sendGrid."</span></span>
- <span data-ttu-id="daacf-117">`direction`: Krävs – måste anges för ”out”.</span><span class="sxs-lookup"><span data-stu-id="daacf-117">`direction` : Required - must be set too"out."</span></span>
- <span data-ttu-id="daacf-118">`apiKey`: Krävs – måste vara toohello namn för din API-nyckel som lagras i appen hello-funktionen app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="daacf-118">`apiKey` : Required - must be set toohello name of your API key stored in hello Function App's app settings.</span></span>
- <span data-ttu-id="daacf-119">`to`: hello mottagarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="daacf-119">`to` : hello recipient's email address.</span></span>
- <span data-ttu-id="daacf-120">`from`: hello avsändarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="daacf-120">`from` : hello sender's email address.</span></span>
- <span data-ttu-id="daacf-121">`subject`: hello ämnet för e-hello.</span><span class="sxs-lookup"><span data-stu-id="daacf-121">`subject` : hello subject of hello email.</span></span>
- <span data-ttu-id="daacf-122">`text`: hello e-postinnehåll.</span><span class="sxs-lookup"><span data-stu-id="daacf-122">`text` : hello email content.</span></span>

<span data-ttu-id="daacf-123">Exempel på **function.json**:</span><span class="sxs-lookup"><span data-stu-id="daacf-123">Example of **function.json**:</span></span>

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
> <span data-ttu-id="daacf-124">Azure Functions lagrar dina anslutningsinformationen och API-nycklar som app-inställningar så att de inte kontrolleras i din källkontroll.</span><span class="sxs-lookup"><span data-stu-id="daacf-124">Azure Functions stores your connection information and API keys as app settings so that they are not checked into your source control repository.</span></span> <span data-ttu-id="daacf-125">Den här åtgärden skyddar känslig information.</span><span class="sxs-lookup"><span data-stu-id="daacf-125">This action safeguards your sensitive information.</span></span>
>
>

## <a name="c-example-of-hello-sendgrid-output-binding"></a><span data-ttu-id="daacf-126">C#-exempel på hello SendGrid utdatabindning</span><span class="sxs-lookup"><span data-stu-id="daacf-126">C# example of hello SendGrid output binding</span></span>

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

## <a name="node-example-of-hello-sendgrid-output-binding"></a><span data-ttu-id="daacf-127">Noden exempel på hello SendGrid utdata bindning</span><span class="sxs-lookup"><span data-stu-id="daacf-127">Node example of hello SendGrid output binding</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="daacf-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="daacf-128">Next steps</span></span>
<span data-ttu-id="daacf-129">Information om andra bindningar och utlösare för Azure Functions finns i</span><span class="sxs-lookup"><span data-stu-id="daacf-129">For information about other bindings and triggers for Azure Functions, see</span></span> 
- [<span data-ttu-id="daacf-130">Azure Functions-utlösare och bindningar för utvecklare</span><span class="sxs-lookup"><span data-stu-id="daacf-130">Azure Functions triggers and bindings developer reference</span></span>](functions-triggers-bindings.md)

- <span data-ttu-id="daacf-131">[Metodtips för Azure Functions](functions-best-practices.md) innehåller vissa bästa praxis toouse när du skapar Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="daacf-131">[Best practices for Azure Functions](functions-best-practices.md) Lists some best practices toouse when creating Azure Functions.</span></span>

- <span data-ttu-id="daacf-132">[Azure Functions för utvecklare](functions-reference.md) programmerare om att koda funktioner och definiera utlösare och bindningar.</span><span class="sxs-lookup"><span data-stu-id="daacf-132">[Azure Functions developer reference](functions-reference.md) Programmer reference for coding functions and defining triggers and bindings.</span></span>
