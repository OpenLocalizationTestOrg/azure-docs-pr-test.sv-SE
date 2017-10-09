---
title: aaaHow toouse SendGrid i Azure Functions | Microsoft Docs
description: Visar hur toouse SendGrid i Azure Functions
services: functions
documentationcenter: na
author: rachelappel
manager: erikre
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/31/2017
ms.author: rachelap
ms.openlocfilehash: a0ffdae04e5924c773d2d26427626fc1f570f7ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-sendgrid-in-azure-functions"></a><span data-ttu-id="11a16-103">Hur toouse SendGrid i Azure Functions</span><span class="sxs-lookup"><span data-stu-id="11a16-103">How toouse SendGrid in Azure Functions</span></span>

## <a name="sendgrid-overview"></a><span data-ttu-id="11a16-104">Översikt över SendGrid</span><span class="sxs-lookup"><span data-stu-id="11a16-104">SendGrid Overview</span></span>

<span data-ttu-id="11a16-105">Azure Functions stöder SendGrid utdata bindningar tooenable funktioner toosend e-postmeddelanden med några få rader med kod och ett SendGrid-konto.</span><span class="sxs-lookup"><span data-stu-id="11a16-105">Azure Functions supports SendGrid output bindings tooenable your functions toosend email messages with a few lines of code and a SendGrid account.</span></span>

<span data-ttu-id="11a16-106">toouse hello SendGrid API i en Azure-funktion, måste du ha en [SendGrid konto](http://SendGrid.com).</span><span class="sxs-lookup"><span data-stu-id="11a16-106">toouse hello SendGrid API in an Azure Function, you must have a [SendGrid account](http://SendGrid.com).</span></span> <span data-ttu-id="11a16-107">Du måste dessutom ha en SendGrid API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="11a16-107">Additionally, you must have a SendGrid API Key.</span></span> <span data-ttu-id="11a16-108">Logga in tooyour SendGrid-kontot och klickar på **inställningar** sedan **API-nyckel** toogenerate en API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="11a16-108">Log in tooyour SendGrid account and click **Settings** then **API Key** toogenerate an API key.</span></span> <span data-ttu-id="11a16-109">Behåll den här nyckeln som är tillgängliga när du använder den i ett kommande steg.</span><span class="sxs-lookup"><span data-stu-id="11a16-109">Keep this key available as you use it in an upcoming step.</span></span>

<span data-ttu-id="11a16-110">Du är nu redo toocreate en app i Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="11a16-110">You are now ready toocreate an Azure Function app.</span></span>

## <a name="create-an-azure-function-app"></a><span data-ttu-id="11a16-111">Skapa en Azure Functions-app</span><span class="sxs-lookup"><span data-stu-id="11a16-111">Create an Azure Function app</span></span> 

<span data-ttu-id="11a16-112">Azure funktionen appar är behållare för en eller flera Azure functions.</span><span class="sxs-lookup"><span data-stu-id="11a16-112">Azure Function Apps are containers for one or more Azure functions.</span></span> <span data-ttu-id="11a16-113">Azure functions är just - funktionen.</span><span class="sxs-lookup"><span data-stu-id="11a16-113">Azure functions are just that - a function.</span></span> <span data-ttu-id="11a16-114">Varje Azure-funktion är bundet tooone trigger, vilket är en händelse som orsakar hello funktionen toorun.</span><span class="sxs-lookup"><span data-stu-id="11a16-114">Each Azure function is tied tooone trigger, which is an event that causes hello function toorun.</span></span>
<span data-ttu-id="11a16-115">Varje funktion kan innehålla valfritt antal indata eller utdata bindningar.</span><span class="sxs-lookup"><span data-stu-id="11a16-115">Each function can contain any number of input or output bindings.</span></span> <span data-ttu-id="11a16-116">Bindningar är tjänster som du kan använda i en funktion.</span><span class="sxs-lookup"><span data-stu-id="11a16-116">Bindings are services that you can use in a function.</span></span> <span data-ttu-id="11a16-117">SendGrid är ett utgående bindning du kan använda toosend e-post.</span><span class="sxs-lookup"><span data-stu-id="11a16-117">SendGrid is an output binding you can use toosend email.</span></span> 

1. <span data-ttu-id="11a16-118">Logga in toohello Azure-portalen och [skapa ett Azure-Funktionsapp](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) eller öppna en befintlig funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="11a16-118">Log in toohello Azure portal and [create an Azure Function App](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) or open an existing Function app.</span></span> 
2. <span data-ttu-id="11a16-119">Skapa en Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="11a16-119">Create an Azure function.</span></span> <span data-ttu-id="11a16-120">tookeep den enkla, välja en manuell utlösare och C#.</span><span class="sxs-lookup"><span data-stu-id="11a16-120">tookeep it simple, choose a manual trigger and C#.</span></span> 

 ![Skapa en Azure-funktion](./media/functions-how-to-use-sendgrid/functions-new-function-manual-trigger-page.png)

## <a name="configure-sendgrid-for-use-in-an-azure-function-app"></a><span data-ttu-id="11a16-122">Konfigurera SendGrid för användning i en app i Azure-funktion</span><span class="sxs-lookup"><span data-stu-id="11a16-122">Configure SendGrid for use in an Azure Function app</span></span>

<span data-ttu-id="11a16-123">Du måste lagra din SendGrid API-nyckel som en app inställningen toouse den i en funktion.</span><span class="sxs-lookup"><span data-stu-id="11a16-123">You must store your SendGrid API Key as an app setting toouse it in a function.</span></span> <span data-ttu-id="11a16-124">Hej ApiKey fältet är inte din faktiska SendGrid API-nyckel, men en app som du du definiera som representerar din faktiska API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="11a16-124">hello ApiKey field is not your actual SendGrid API key, but an app setting you define that represents your actual API key.</span></span> <span data-ttu-id="11a16-125">Lagra din nyckel för det här sättet är för säkerhet, eftersom det är separat från någon kod eller filer som kan kontrolleras i källkodskontroll.</span><span class="sxs-lookup"><span data-stu-id="11a16-125">Storing your key this way is for security, since it is separate from any code or files that might be checked into source code control.</span></span>

- <span data-ttu-id="11a16-126">Skapa en **AppSettings** nyckeln i appen funktionen **programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="11a16-126">Create an **AppSettings** key in your function app's **Application Settings**.</span></span>

 ![Skapa en Azure-funktion](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-api-key-app-settings.png)

## <a name="configure-sendgrid-output-bindings"></a><span data-ttu-id="11a16-128">Konfigurera SendGrid utdata bindningar</span><span class="sxs-lookup"><span data-stu-id="11a16-128">Configure SendGrid output bindings</span></span>

<span data-ttu-id="11a16-129">SendGrid är tillgänglig som en Azure funktionen utdatabindning.</span><span class="sxs-lookup"><span data-stu-id="11a16-129">SendGrid is available as an Azure function output binding.</span></span> <span data-ttu-id="11a16-130">toocreate en SendGrid utdatabindning:</span><span class="sxs-lookup"><span data-stu-id="11a16-130">toocreate a SendGrid output binding:</span></span>

1. <span data-ttu-id="11a16-131">Gå toohello **integrera** fliken hello-funktionen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="11a16-131">Go toohello **Integrate** tab of hello function in hello Azure portal.</span></span>
2. <span data-ttu-id="11a16-132">Klicka på **nya utdata** toocreate en SendGrid utdatabindning.</span><span class="sxs-lookup"><span data-stu-id="11a16-132">Click **New Output** toocreate a SendGrid output binding.</span></span>
3. <span data-ttu-id="11a16-133">Fyll i hello **API-nyckel** och **meddelandet parameternamnet** egenskaper.</span><span class="sxs-lookup"><span data-stu-id="11a16-133">Fill in hello **API Key** and **Message parameter name** properties.</span></span> <span data-ttu-id="11a16-134">Om du vill kan du ange hello andra egenskaper nu eller code dem i stället.</span><span class="sxs-lookup"><span data-stu-id="11a16-134">If you want, you can enter hello other properties now, or code them instead.</span></span> <span data-ttu-id="11a16-135">De här inställningarna kan användas som standard.</span><span class="sxs-lookup"><span data-stu-id="11a16-135">These settings can be used as defaults.</span></span>

 ![Konfigurera SendGrid utdata bindningar](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-output-bindings.png)

<span data-ttu-id="11a16-137">Om du lägger till en bindning tooa funktion skapas en fil med namnet **function.json** i mappen för din funktion.</span><span class="sxs-lookup"><span data-stu-id="11a16-137">Adding a binding tooa function creates a file called **function.json** in your function's folder.</span></span> <span data-ttu-id="11a16-138">Den här filen innehåller alla hello samma information som du ser i hello Azure funktionen **integrera** fliken, men i Json-format.</span><span class="sxs-lookup"><span data-stu-id="11a16-138">This file contains all hello same information that you see in hello Azure function's **Integrate** tab, but in Json format.</span></span> <span data-ttu-id="11a16-139">Inställningen hello **ApiKey**, **meddelandet**, och **från** fält skapa hello följande poster i hello **function.json** fil:</span><span class="sxs-lookup"><span data-stu-id="11a16-139">Setting hello **ApiKey**, **message**, and **from** fields create hello following entries in hello **function.json** file:</span></span> 

```json
{
  "bindings": [    
    {
      "type": "sendGrid",
      "name": "message",
      "apiKey": "SendGridKey",
      "direction": "out",
      "from": "azure@contoso.com"
    }
  ],
  "disabled": false
}
```

<span data-ttu-id="11a16-140">Om du vill kan du ändra den här filen själv direkt.</span><span class="sxs-lookup"><span data-stu-id="11a16-140">If you prefer, you may modify this file yourself directly.</span></span>

<span data-ttu-id="11a16-141">Nu när du har skapat och konfigurerat hello Funktionsapp och funktionen, kan du skriva hello kod toosend ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="11a16-141">Now that you have created and configured hello Function App and function, you can write hello code toosend an email.</span></span>

## <a name="write-code-that-creates-and-sends-email"></a><span data-ttu-id="11a16-142">Skriva kod som skapar och skickar e-post</span><span class="sxs-lookup"><span data-stu-id="11a16-142">Write code that creates and sends email</span></span>

<span data-ttu-id="11a16-143">Hej SendGrid API innehåller alla hello-kommandon du behöver toocreate och skicka ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="11a16-143">hello SendGrid API contains all hello commands you need toocreate and send an email.</span></span>  

- <span data-ttu-id="11a16-144">Ersätt hello koden i hello-funktionen med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="11a16-144">Replace hello code in hello function with hello following code:</span></span>

```cs
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
    // change tooemail of recipient
    personalization.AddTo(new Email("MoreEmailPlease@contoso.com"));   

    Content content = new Content
    {
        Type = "text/plain",
        Value = input
    };
    message.AddContent(content);
    message.AddPersonalization(personalization);
}
```

<span data-ttu-id="11a16-145">Meddelande hello första raden innehåller hello ```#r``` direktiv som refererar till hello SendGrid sammansättning.</span><span class="sxs-lookup"><span data-stu-id="11a16-145">Notice hello first line contains hello ```#r``` directive that references hello SendGrid assembly.</span></span> <span data-ttu-id="11a16-146">Sedan kan du använda en ```using``` instruktionen toomore enkelt komma åt hello objekt i namnutrymmet.</span><span class="sxs-lookup"><span data-stu-id="11a16-146">After that, you can use a ```using``` statement toomore easily access hello objects in that namespace.</span></span> <span data-ttu-id="11a16-147">I hello koden skapar instanser av ```Mail```, ```Personalization```, och ```Content``` objekt från hello SendGrid API som ska utgöra hello e-post.</span><span class="sxs-lookup"><span data-stu-id="11a16-147">In hello code, create instances of ```Mail```, ```Personalization```, and ```Content``` objects from hello SendGrid API that compose hello email.</span></span> <span data-ttu-id="11a16-148">När du går tillbaka hello-meddelande, ger den SendGrid.</span><span class="sxs-lookup"><span data-stu-id="11a16-148">When you return hello message, SendGrid delivers it.</span></span> 

<span data-ttu-id="11a16-149">hello funktionens signaturen innehåller också en extra out-parameter av typen ```Mail``` med namnet ```message```.</span><span class="sxs-lookup"><span data-stu-id="11a16-149">hello function's signature also contains an extra out parameter of type ```Mail``` named ```message```.</span></span> <span data-ttu-id="11a16-150">Både indata och utdata bindningar express sig själva som funktionsparametrar i kod.</span><span class="sxs-lookup"><span data-stu-id="11a16-150">Both input and output bindings express themselves as function parameters in code.</span></span> 

2. <span data-ttu-id="11a16-151">Testa din kod genom att klicka på **Test** och ange ett meddelande i hello **Begärandetext** fältet och sedan på hello **kör** knappen.</span><span class="sxs-lookup"><span data-stu-id="11a16-151">Test your code by clicking **Test** and entering a message into hello **Request body** field, then clicking hello **Run** button.</span></span>

 ![Testa din kod](./media/functions-how-to-use-sendgrid/functions-develop-test-sendgrid.png)

3. <span data-ttu-id="11a16-153">Kontrollera e-tooverify som skickats SendGrid hello e-post.</span><span class="sxs-lookup"><span data-stu-id="11a16-153">Check email tooverify that SendGrid sent hello email.</span></span> <span data-ttu-id="11a16-154">Det ska gå toohello adress i hello kod från steg 1 och innehålla hello-meddelande från hello **Begärandetext**.</span><span class="sxs-lookup"><span data-stu-id="11a16-154">It should go toohello address in hello code from step 1, and contain hello message from hello **Request body**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="11a16-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="11a16-155">Next steps</span></span>
<span data-ttu-id="11a16-156">Den här artikeln har visat hur toouse hello SendGrid service toocreate och skicka e-post.</span><span class="sxs-lookup"><span data-stu-id="11a16-156">This article has demonstrated how toouse hello SendGrid service toocreate and send email.</span></span> <span data-ttu-id="11a16-157">toolearn mer information om hur du använder Azure Functions i dina appar finns hello följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="11a16-157">toolearn more about using Azure Functions in your apps, see hello following topics:</span></span> 

- <span data-ttu-id="11a16-158">[Metodtips för Azure Functions](functions-best-practices.md) innehåller vissa bästa praxis toouse när du skapar Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="11a16-158">[Best practices for Azure Functions](functions-best-practices.md) Lists some best practices toouse when creating Azure Functions.</span></span>

- <span data-ttu-id="11a16-159">[Azure Functions för utvecklare](functions-reference.md) programmerare om att koda funktioner och definiera utlösare och bindningar.</span><span class="sxs-lookup"><span data-stu-id="11a16-159">[Azure Functions developer reference](functions-reference.md) Programmer reference for coding functions and defining triggers and bindings.</span></span>

- <span data-ttu-id="11a16-160">[Testa Azure Functions](functions-test-a-function.md) beskriver olika verktyg och tekniker för att testa funktioner.</span><span class="sxs-lookup"><span data-stu-id="11a16-160">[Testing Azure Functions](functions-test-a-function.md) Describes various tools and techniques for testing your functions.</span></span>