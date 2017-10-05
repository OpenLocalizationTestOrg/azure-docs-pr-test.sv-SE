---
title: "Hur du använder SendGrid i Azure Functions | Microsoft Docs"
description: "Visar hur du använder SendGrid i Azure Functions"
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
ms.openlocfilehash: 05c9f4e4a4351219da68af8b702c25f21d7d4d02
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-sendgrid-in-azure-functions"></a><span data-ttu-id="abff8-103">Hur du använder SendGrid i Azure Functions</span><span class="sxs-lookup"><span data-stu-id="abff8-103">How to use SendGrid in Azure Functions</span></span>

## <a name="sendgrid-overview"></a><span data-ttu-id="abff8-104">Översikt över SendGrid</span><span class="sxs-lookup"><span data-stu-id="abff8-104">SendGrid Overview</span></span>

<span data-ttu-id="abff8-105">Azure Functions stöder SendGrid utdata bindningar om du vill aktivera dina funktioner att skicka e-postmeddelanden med några få rader med kod och ett SendGrid-konto.</span><span class="sxs-lookup"><span data-stu-id="abff8-105">Azure Functions supports SendGrid output bindings to enable your functions to send email messages with a few lines of code and a SendGrid account.</span></span>

<span data-ttu-id="abff8-106">Om du vill använda SendGrid-API i en Azure-funktion, måste du ha en [SendGrid konto](http://SendGrid.com).</span><span class="sxs-lookup"><span data-stu-id="abff8-106">To use the SendGrid API in an Azure Function, you must have a [SendGrid account](http://SendGrid.com).</span></span> <span data-ttu-id="abff8-107">Du måste dessutom ha en SendGrid API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="abff8-107">Additionally, you must have a SendGrid API Key.</span></span> <span data-ttu-id="abff8-108">Logga in på ditt SendGrid-konto och klicka på **inställningar** sedan **API-nyckel** att generera en API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="abff8-108">Log in to your SendGrid account and click **Settings** then **API Key** to generate an API key.</span></span> <span data-ttu-id="abff8-109">Behåll den här nyckeln som är tillgängliga när du använder den i ett kommande steg.</span><span class="sxs-lookup"><span data-stu-id="abff8-109">Keep this key available as you use it in an upcoming step.</span></span>

<span data-ttu-id="abff8-110">Du är nu redo att skapa en app i Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="abff8-110">You are now ready to create an Azure Function app.</span></span>

## <a name="create-an-azure-function-app"></a><span data-ttu-id="abff8-111">Skapa en Azure Functions-app</span><span class="sxs-lookup"><span data-stu-id="abff8-111">Create an Azure Function app</span></span> 

<span data-ttu-id="abff8-112">Azure funktionen appar är behållare för en eller flera Azure functions.</span><span class="sxs-lookup"><span data-stu-id="abff8-112">Azure Function Apps are containers for one or more Azure functions.</span></span> <span data-ttu-id="abff8-113">Azure functions är just - funktionen.</span><span class="sxs-lookup"><span data-stu-id="abff8-113">Azure functions are just that - a function.</span></span> <span data-ttu-id="abff8-114">Varje Azure-funktion är kopplad till en utlösare som är en händelse som gör att funktionen ska köras.</span><span class="sxs-lookup"><span data-stu-id="abff8-114">Each Azure function is tied to one trigger, which is an event that causes the function to run.</span></span>
<span data-ttu-id="abff8-115">Varje funktion kan innehålla valfritt antal indata eller utdata bindningar.</span><span class="sxs-lookup"><span data-stu-id="abff8-115">Each function can contain any number of input or output bindings.</span></span> <span data-ttu-id="abff8-116">Bindningar är tjänster som du kan använda i en funktion.</span><span class="sxs-lookup"><span data-stu-id="abff8-116">Bindings are services that you can use in a function.</span></span> <span data-ttu-id="abff8-117">SendGrid är en bindning för utdata som du kan använda för att skicka e-post.</span><span class="sxs-lookup"><span data-stu-id="abff8-117">SendGrid is an output binding you can use to send email.</span></span> 

1. <span data-ttu-id="abff8-118">Logga in på Azure-portalen och [skapa ett Azure-Funktionsapp](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) eller öppna en befintlig funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="abff8-118">Log in to the Azure portal and [create an Azure Function App](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) or open an existing Function app.</span></span> 
2. <span data-ttu-id="abff8-119">Skapa en Azure-funktion.</span><span class="sxs-lookup"><span data-stu-id="abff8-119">Create an Azure function.</span></span> <span data-ttu-id="abff8-120">Om du vill göra det enkelt att välja en manuell utlösare och C#.</span><span class="sxs-lookup"><span data-stu-id="abff8-120">To keep it simple, choose a manual trigger and C#.</span></span> 

 ![Skapa en Azure-funktion](./media/functions-how-to-use-sendgrid/functions-new-function-manual-trigger-page.png)

## <a name="configure-sendgrid-for-use-in-an-azure-function-app"></a><span data-ttu-id="abff8-122">Konfigurera SendGrid för användning i en app i Azure-funktion</span><span class="sxs-lookup"><span data-stu-id="abff8-122">Configure SendGrid for use in an Azure Function app</span></span>

<span data-ttu-id="abff8-123">Du måste lagra din SendGrid API-nyckel som en appinställningen för att använda den i en funktion.</span><span class="sxs-lookup"><span data-stu-id="abff8-123">You must store your SendGrid API Key as an app setting to use it in a function.</span></span> <span data-ttu-id="abff8-124">Fältet ApiKey är inte din faktiska SendGrid API-nyckel, men en app som du du definiera som representerar din faktiska API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="abff8-124">The ApiKey field is not your actual SendGrid API key, but an app setting you define that represents your actual API key.</span></span> <span data-ttu-id="abff8-125">Lagra din nyckel för det här sättet är för säkerhet, eftersom det är separat från någon kod eller filer som kan kontrolleras i källkodskontroll.</span><span class="sxs-lookup"><span data-stu-id="abff8-125">Storing your key this way is for security, since it is separate from any code or files that might be checked into source code control.</span></span>

- <span data-ttu-id="abff8-126">Skapa en **AppSettings** nyckeln i appen funktionen **programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="abff8-126">Create an **AppSettings** key in your function app's **Application Settings**.</span></span>

 ![Skapa en Azure-funktion](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-api-key-app-settings.png)

## <a name="configure-sendgrid-output-bindings"></a><span data-ttu-id="abff8-128">Konfigurera SendGrid utdata bindningar</span><span class="sxs-lookup"><span data-stu-id="abff8-128">Configure SendGrid output bindings</span></span>

<span data-ttu-id="abff8-129">SendGrid är tillgänglig som en Azure funktionen utdatabindning.</span><span class="sxs-lookup"><span data-stu-id="abff8-129">SendGrid is available as an Azure function output binding.</span></span> <span data-ttu-id="abff8-130">Om du vill skapa en SendGrid utdatabindning:</span><span class="sxs-lookup"><span data-stu-id="abff8-130">To create a SendGrid output binding:</span></span>

1. <span data-ttu-id="abff8-131">Gå till den **integrera** fliken för funktionen i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="abff8-131">Go to the **Integrate** tab of the function in the Azure portal.</span></span>
2. <span data-ttu-id="abff8-132">Klicka på **nya utdata** att skapa en SendGrid utdatabindning.</span><span class="sxs-lookup"><span data-stu-id="abff8-132">Click **New Output** to create a SendGrid output binding.</span></span>
3. <span data-ttu-id="abff8-133">Fyll i den **API-nyckel** och **meddelandet parameternamnet** egenskaper.</span><span class="sxs-lookup"><span data-stu-id="abff8-133">Fill in the **API Key** and **Message parameter name** properties.</span></span> <span data-ttu-id="abff8-134">Om du vill kan du ange de andra egenskaperna nu eller code dem i stället.</span><span class="sxs-lookup"><span data-stu-id="abff8-134">If you want, you can enter the other properties now, or code them instead.</span></span> <span data-ttu-id="abff8-135">De här inställningarna kan användas som standard.</span><span class="sxs-lookup"><span data-stu-id="abff8-135">These settings can be used as defaults.</span></span>

 ![Konfigurera SendGrid utdata bindningar](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-output-bindings.png)

<span data-ttu-id="abff8-137">Om du lägger till en bindning till en funktion skapas en fil med namnet **function.json** i mappen för din funktion.</span><span class="sxs-lookup"><span data-stu-id="abff8-137">Adding a binding to a function creates a file called **function.json** in your function's folder.</span></span> <span data-ttu-id="abff8-138">Den här filen innehåller samma information som visas i funktionen Azure **integrera** fliken, men i Json-format.</span><span class="sxs-lookup"><span data-stu-id="abff8-138">This file contains all the same information that you see in the Azure function's **Integrate** tab, but in Json format.</span></span> <span data-ttu-id="abff8-139">Ange den **ApiKey**, **meddelandet**, och **från** fält skapa följande poster i den **function.json** fil:</span><span class="sxs-lookup"><span data-stu-id="abff8-139">Setting the **ApiKey**, **message**, and **from** fields create the following entries in the **function.json** file:</span></span> 

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

<span data-ttu-id="abff8-140">Om du vill kan du ändra den här filen själv direkt.</span><span class="sxs-lookup"><span data-stu-id="abff8-140">If you prefer, you may modify this file yourself directly.</span></span>

<span data-ttu-id="abff8-141">Nu när du har skapat och konfigurerat Funktionsapp och funktionen, kan du skriva kod för att skicka ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="abff8-141">Now that you have created and configured the Function App and function, you can write the code to send an email.</span></span>

## <a name="write-code-that-creates-and-sends-email"></a><span data-ttu-id="abff8-142">Skriva kod som skapar och skickar e-post</span><span class="sxs-lookup"><span data-stu-id="abff8-142">Write code that creates and sends email</span></span>

<span data-ttu-id="abff8-143">SendGrid API innehåller alla kommandon som du behöver skapa och skicka ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="abff8-143">The SendGrid API contains all the commands you need to create and send an email.</span></span>  

- <span data-ttu-id="abff8-144">Ersätt Koden i funktionen med följande kod:</span><span class="sxs-lookup"><span data-stu-id="abff8-144">Replace the code in the function with the following code:</span></span>

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
    // change to email of recipient
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

<span data-ttu-id="abff8-145">Notera den första raden innehåller den ```#r``` direktiv som refererar till SendGrid-sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="abff8-145">Notice the first line contains the ```#r``` directive that references the SendGrid assembly.</span></span> <span data-ttu-id="abff8-146">Sedan kan du använda en ```using``` instruktion för att enkelt komma åt objekt i namnutrymmet.</span><span class="sxs-lookup"><span data-stu-id="abff8-146">After that, you can use a ```using``` statement to more easily access the objects in that namespace.</span></span> <span data-ttu-id="abff8-147">I koden skapar instanser av ```Mail```, ```Personalization```, och ```Content``` objekt från SendGrid-API som ska utgöra den e-postadressen.</span><span class="sxs-lookup"><span data-stu-id="abff8-147">In the code, create instances of ```Mail```, ```Personalization```, and ```Content``` objects from the SendGrid API that compose the email.</span></span> <span data-ttu-id="abff8-148">När du returnerar meddelandet ger SendGrid den.</span><span class="sxs-lookup"><span data-stu-id="abff8-148">When you return the message, SendGrid delivers it.</span></span> 

<span data-ttu-id="abff8-149">Funktionens signaturen innehåller också en extra out-parameter av typen ```Mail``` med namnet ```message```.</span><span class="sxs-lookup"><span data-stu-id="abff8-149">The function's signature also contains an extra out parameter of type ```Mail``` named ```message```.</span></span> <span data-ttu-id="abff8-150">Både indata och utdata bindningar express sig själva som funktionsparametrar i kod.</span><span class="sxs-lookup"><span data-stu-id="abff8-150">Both input and output bindings express themselves as function parameters in code.</span></span> 

2. <span data-ttu-id="abff8-151">Testa din kod genom att klicka på **Test** och ange ett meddelande till den **Begärandetext** fältet, klicka på **kör** knappen.</span><span class="sxs-lookup"><span data-stu-id="abff8-151">Test your code by clicking **Test** and entering a message into the **Request body** field, then clicking the **Run** button.</span></span>

 ![Testa din kod](./media/functions-how-to-use-sendgrid/functions-develop-test-sendgrid.png)

3. <span data-ttu-id="abff8-153">Kontrollera e-post för att kontrollera att SendGrid har skickat e-postmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="abff8-153">Check email to verify that SendGrid sent the email.</span></span> <span data-ttu-id="abff8-154">Det ska gå till adressen i koden från steg 1 och innehålla meddelandet från den **Begärandetext**.</span><span class="sxs-lookup"><span data-stu-id="abff8-154">It should go to the address in the code from step 1, and contain the message from the **Request body**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="abff8-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="abff8-155">Next steps</span></span>
<span data-ttu-id="abff8-156">Den här artikeln har du lärt dig att använda SendGrid-tjänsten för att skapa och skicka e-post.</span><span class="sxs-lookup"><span data-stu-id="abff8-156">This article has demonstrated how to use the SendGrid service to create and send email.</span></span> <span data-ttu-id="abff8-157">Mer information om hur du använder Azure Functions i dina appar finns i följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="abff8-157">To learn more about using Azure Functions in your apps, see the following topics:</span></span> 

- <span data-ttu-id="abff8-158">[Metodtips för Azure Functions](functions-best-practices.md) listar några rekommendationer att använda när du skapar Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="abff8-158">[Best practices for Azure Functions](functions-best-practices.md) Lists some best practices to use when creating Azure Functions.</span></span>

- <span data-ttu-id="abff8-159">[Azure Functions för utvecklare](functions-reference.md) programmerare om att koda funktioner och definiera utlösare och bindningar.</span><span class="sxs-lookup"><span data-stu-id="abff8-159">[Azure Functions developer reference](functions-reference.md) Programmer reference for coding functions and defining triggers and bindings.</span></span>

- <span data-ttu-id="abff8-160">[Testa Azure Functions](functions-test-a-function.md) beskriver olika verktyg och tekniker för att testa funktioner.</span><span class="sxs-lookup"><span data-stu-id="abff8-160">[Testing Azure Functions](functions-test-a-function.md) Describes various tools and techniques for testing your functions.</span></span>