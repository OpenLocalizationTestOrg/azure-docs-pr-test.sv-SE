---
title: aaaTesting Azure Functions | Microsoft Docs
description: "Testa dina Azure funktioner med hjälp av Postman cURL och Node.js."
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "Azure funktioner, funktioner, händelsebearbetning, webhooks, dynamiska beräkning, serverlösa arkitektur och testning"
ms.assetid: c00f3082-30d2-46b3-96ea-34faf2f15f77
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/02/2017
ms.author: wesmc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a084f8dbc8089356c3c19d789dc9098f2bb63052
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="strategies-for-testing-your-code-in-azure-functions"></a><span data-ttu-id="e36b4-104">Strategier för att testa din kod i Azure Functions</span><span class="sxs-lookup"><span data-stu-id="e36b4-104">Strategies for testing your code in Azure Functions</span></span>

<span data-ttu-id="e36b4-105">Det här avsnittet visar hello olika sätt tootest funktioner, inklusive användning av hello följande allmänna närmar sig:</span><span class="sxs-lookup"><span data-stu-id="e36b4-105">This topic demonstrates hello various ways tootest functions, including using hello following general approaches:</span></span>

+ <span data-ttu-id="e36b4-106">HTTP-baserade verktyg som cURL, Postman och även en webbläsare för Webbaserad utlösare</span><span class="sxs-lookup"><span data-stu-id="e36b4-106">HTTP-based tools, such as cURL, Postman, and even a web browser for web-based triggers</span></span>
+ <span data-ttu-id="e36b4-107">Azure Lagringsutforskaren tootest Azure Storage-baserade utlösare</span><span class="sxs-lookup"><span data-stu-id="e36b4-107">Azure Storage Explorer, tootest Azure Storage-based triggers</span></span>
+ <span data-ttu-id="e36b4-108">Testa fliken i hello Azure Functions-portalen</span><span class="sxs-lookup"><span data-stu-id="e36b4-108">Test tab in hello Azure Functions portal</span></span>
+ <span data-ttu-id="e36b4-109">Funktion som utlöses av timer</span><span class="sxs-lookup"><span data-stu-id="e36b4-109">Timer-triggered function</span></span>
+ <span data-ttu-id="e36b4-110">Testa programmet eller framework</span><span class="sxs-lookup"><span data-stu-id="e36b4-110">Testing application or framework</span></span>

<span data-ttu-id="e36b4-111">Alla dessa metoder funktionen en HTTP-utlösare som accepterar indata via antingen en query sträng parameter eller hello brödtext i begäran.</span><span class="sxs-lookup"><span data-stu-id="e36b4-111">All these testing methods use an HTTP trigger function that accepts input through either a query string parameter or hello request body.</span></span> <span data-ttu-id="e36b4-112">Du skapar den här funktionen i hello första avsnittet.</span><span class="sxs-lookup"><span data-stu-id="e36b4-112">You create this function in hello first section.</span></span>

## <a name="create-a-function-for-testing"></a><span data-ttu-id="e36b4-113">Skapa en funktion för testning</span><span class="sxs-lookup"><span data-stu-id="e36b4-113">Create a function for testing</span></span>
<span data-ttu-id="e36b4-114">För merparten av den här kursen använder vi en lite annorlunda version av hello HttpTrigger JavaScript funktionen mall som är tillgängliga när du skapar en funktion.</span><span class="sxs-lookup"><span data-stu-id="e36b4-114">For most of this tutorial, we use a slightly modified version of hello HttpTrigger JavaScript function template that is available when you create a function.</span></span> <span data-ttu-id="e36b4-115">Om du behöver hjälp med att skapa en funktion kan du granska detta [kursen](functions-create-first-azure-function.md).</span><span class="sxs-lookup"><span data-stu-id="e36b4-115">If you need help creating a function, review this [tutorial](functions-create-first-azure-function.md).</span></span> <span data-ttu-id="e36b4-116">Välj hello **HttpTrigger - JavaScript** mallen när du skapar hello funktionsnamnet i hello [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="e36b4-116">Choose hello **HttpTrigger- JavaScript** template when creating hello test function in hello [Azure portal].</span></span>

<span data-ttu-id="e36b4-117">hello standardmallen för funktionen är i grunden en ”hello world”-funktion som ekon tillbaka hello namn från hello begäran brödtext eller fråga strängparameter, `name=<your name>`.</span><span class="sxs-lookup"><span data-stu-id="e36b4-117">hello default function template is basically a "hello world" function that echoes back hello name from hello request body or query string parameter, `name=<your name>`.</span></span>  <span data-ttu-id="e36b4-118">Vi uppdaterar hello kod tooalso kan du tooprovide hello namn och en adress som JSON-innehåll i hello frågans brödtext.</span><span class="sxs-lookup"><span data-stu-id="e36b4-118">We'll update hello code tooalso allow you tooprovide hello name and an address as JSON content in hello request body.</span></span> <span data-ttu-id="e36b4-119">Hello funktionen ekon sedan dessa tillbaka toohello klienten när det är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="e36b4-119">Then hello function echoes these back toohello client when available.</span></span>   

<span data-ttu-id="e36b4-120">Uppdatera hello-funktionen med följande kod, som ska användas för att testa hello:</span><span class="sxs-lookup"><span data-stu-id="e36b4-120">Update hello function with hello following code, which we will use for testing:</span></span>

```javascript
module.exports = function (context, req) {
    context.log("HTTP trigger function processed a request. RequestUri=%s", req.originalUrl);
    context.log("Request Headers = " + JSON.stringify(req.headers));
    var res;

    if (req.query.name || (req.body && req.body.name)) {
        if (typeof req.query.name != "undefined") {
            context.log("Name was provided as a query string param...");
            res = ProcessNewUserInformation(context, req.query.name);
        }
        else {
            context.log("Processing user info from request body...");
            res = ProcessNewUserInformation(context, req.body.name, req.body.address);
        }
    }
    else {
        res = {
            status: 400,
            body: "Please pass a name on hello query string or in hello request body"
        };
    }
    context.done(null, res);
};
function ProcessNewUserInformation(context, name, address) {
    context.log("Processing user information...");
    context.log("name = " + name);
    var echoString = "Hello " + name;
    var res;

    if (typeof address != "undefined") {
        echoString += "\n" + "hello address you provided is " + address;
        context.log("address = " + address);
    }
    res = {
        // status: 200, /* Defaults too200 */
        body: echoString
    };
    return res;
}
```

## <a name="test-a-function-with-tools"></a><span data-ttu-id="e36b4-121">Testa en funktion med verktyg</span><span class="sxs-lookup"><span data-stu-id="e36b4-121">Test a function with tools</span></span>
<span data-ttu-id="e36b4-122">Det finns olika verktyg som du kan använda tootrigger dina funktioner för att testa utanför hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e36b4-122">Outside hello Azure portal, there are various tools that you can use tootrigger your functions for testing.</span></span> <span data-ttu-id="e36b4-123">Dessa inkluderar HTTP testning verktyg (både UI-baserade och kommandot rad), verktyg för Azure Storage-åtkomst och även en enkel webbläsare.</span><span class="sxs-lookup"><span data-stu-id="e36b4-123">These include HTTP testing tools (both UI-based and command line), Azure Storage access tools, and even a simple web browser.</span></span>

### <a name="test-with-a-browser"></a><span data-ttu-id="e36b4-124">Testa med en webbläsare</span><span class="sxs-lookup"><span data-stu-id="e36b4-124">Test with a browser</span></span>
<span data-ttu-id="e36b4-125">hello webbläsaren är ett enkelt sätt tootrigger fungerar via HTTP.</span><span class="sxs-lookup"><span data-stu-id="e36b4-125">hello web browser is a simple way tootrigger functions via HTTP.</span></span> <span data-ttu-id="e36b4-126">Du kan använda en webbläsare för GET-begäranden som inte kräver en brödtext nyttolast och att använda endast fråga strängparametrar.</span><span class="sxs-lookup"><span data-stu-id="e36b4-126">You can use a browser for GET requests that do not require a body payload, and that use only query string parameters.</span></span>

<span data-ttu-id="e36b4-127">tootest hello funktionen vi definierade tidigare, kopiera hello **funktions-Url** från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="e36b4-127">tootest hello function we defined earlier, copy hello **Function Url** from hello portal.</span></span> <span data-ttu-id="e36b4-128">Det har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="e36b4-128">It has hello following form:</span></span>

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

<span data-ttu-id="e36b4-129">Lägg till hello `name` parametern toohello frågesträngen.</span><span class="sxs-lookup"><span data-stu-id="e36b4-129">Append hello `name` parameter toohello query string.</span></span> <span data-ttu-id="e36b4-130">Använda ett faktiska namn för hello `<Enter a name here>` platshållare.</span><span class="sxs-lookup"><span data-stu-id="e36b4-130">Use an actual name for hello `<Enter a name here>` placeholder.</span></span>

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>&name=<Enter a name here>

<span data-ttu-id="e36b4-131">Klistra in hello URL i webbläsaren, och du bör få en liknande toohello följande i svaret.</span><span class="sxs-lookup"><span data-stu-id="e36b4-131">Paste hello URL into your browser, and you should get a response similar toohello following.</span></span>

![Skärmbild av Chrome webbläsarflik med test-svar](./media/functions-test-a-function/browser-test.png)

<span data-ttu-id="e36b4-133">Det här exemplet är hello webbläsaren Chrome som omsluter hello returnerade sträng i XML.</span><span class="sxs-lookup"><span data-stu-id="e36b4-133">This example is hello Chrome browser, which wraps hello returned string in XML.</span></span> <span data-ttu-id="e36b4-134">Andra webbläsare bara hello strängvärde.</span><span class="sxs-lookup"><span data-stu-id="e36b4-134">Other browsers display just hello string value.</span></span>

<span data-ttu-id="e36b4-135">I hello portal **loggar** fönstret utdata liknande toohello följande loggas vid körning hello funktionen:</span><span class="sxs-lookup"><span data-stu-id="e36b4-135">In hello portal **Logs** window, output similar toohello following is logged in executing hello function:</span></span>

    2016-03-23T07:34:59  Welcome, you are now connected toolog-streaming service.
    2016-03-23T07:35:09.195 Function started (Id=61a8c5a9-5e44-4da0-909d-91d293f20445)
    2016-03-23T07:35:10.338 Node.js HTTP trigger function processed a request. RequestUri=https://functionsExample.azurewebsites.net/api/WesmcHttpTriggerNodeJS1?code=XXXXXXXXXX==&name=Glenn from a browser
    2016-03-23T07:35:10.338 Request Headers = {"cache-control":"max-age=0","connection":"Keep-Alive","accept":"text/html","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T07:35:10.338 Name was provided as a query string param.
    2016-03-23T07:35:10.338 Processing User Information...
    2016-03-23T07:35:10.369 Function completed (Success, Id=61a8c5a9-5e44-4da0-909d-91d293f20445)

### <a name="test-with-postman"></a><span data-ttu-id="e36b4-136">Testa med Postman</span><span class="sxs-lookup"><span data-stu-id="e36b4-136">Test with Postman</span></span>
<span data-ttu-id="e36b4-137">hello rekommenderas verktyget tootest det mesta av dina funktioner är Postman som integreras med hello Chrome webbläsare.</span><span class="sxs-lookup"><span data-stu-id="e36b4-137">hello recommended tool tootest most of your functions is Postman, which integrates with hello Chrome browser.</span></span> <span data-ttu-id="e36b4-138">tooinstall Postman, se [hämta Postman](https://www.getpostman.com/).</span><span class="sxs-lookup"><span data-stu-id="e36b4-138">tooinstall Postman, see [Get Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="e36b4-139">Postman ger kontroll över många fler attribut för en HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="e36b4-139">Postman provides control over many more attributes of an HTTP request.</span></span>

> [!TIP]
> <span data-ttu-id="e36b4-140">Verktyget hello HTTP tester som du är mest bekväm med.</span><span class="sxs-lookup"><span data-stu-id="e36b4-140">Use hello HTTP testing tool that you are most comfortable with.</span></span> <span data-ttu-id="e36b4-141">Här följer några alternativ tooPostman:</span><span class="sxs-lookup"><span data-stu-id="e36b4-141">Here are some alternatives tooPostman:</span></span>  
>
> * [<span data-ttu-id="e36b4-142">Fiddler</span><span class="sxs-lookup"><span data-stu-id="e36b4-142">Fiddler</span></span>](http://www.telerik.com/fiddler)  
> * [<span data-ttu-id="e36b4-143">Paw</span><span class="sxs-lookup"><span data-stu-id="e36b4-143">Paw</span></span>](https://luckymarmot.com/paw)  
>
>

<span data-ttu-id="e36b4-144">tootest hello funktion med en begärantext i Postman:</span><span class="sxs-lookup"><span data-stu-id="e36b4-144">tootest hello function with a request body in Postman:</span></span>

1. <span data-ttu-id="e36b4-145">Starta Postman från hello **appar** i hello övre vänstra hörnet av Chrome-webbläsarfönstret.</span><span class="sxs-lookup"><span data-stu-id="e36b4-145">Start Postman from hello **Apps** button in hello upper-left corner of a Chrome browser window.</span></span>
2. <span data-ttu-id="e36b4-146">Kopiera ditt **funktions-Url**, och klistra in den i Postman.</span><span class="sxs-lookup"><span data-stu-id="e36b4-146">Copy your **Function Url**, and paste it into Postman.</span></span> <span data-ttu-id="e36b4-147">Den omfattar hello åtkomst kod frågesträngparametern.</span><span class="sxs-lookup"><span data-stu-id="e36b4-147">It includes hello access code query string parameter.</span></span>
3. <span data-ttu-id="e36b4-148">Ändra hello HTTP-metod för**efter**.</span><span class="sxs-lookup"><span data-stu-id="e36b4-148">Change hello HTTP method too**POST**.</span></span>
4. <span data-ttu-id="e36b4-149">Klicka på **brödtext** > **raw**, och Lägg till en JSON-begäran brödtext liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="e36b4-149">Click **Body** > **raw**, and add a JSON request body similar toohello following:</span></span>

    ```json
    {
        "name" : "Wes testing with Postman",
        "address" : "Seattle, WA 98101"
    }
    ```
5. <span data-ttu-id="e36b4-150">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="e36b4-150">Click **Send**.</span></span>

<span data-ttu-id="e36b4-151">hello visar följande bild tester hello enkla echo funktionen exempel i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="e36b4-151">hello following image shows testing hello simple echo function example in this tutorial.</span></span>

![Skärmbild av Postman användargränssnitt](./media/functions-test-a-function/postman-test.png)

<span data-ttu-id="e36b4-153">I hello portal **loggar** fönstret utdata liknande toohello följande loggas vid körning hello funktionen:</span><span class="sxs-lookup"><span data-stu-id="e36b4-153">In hello portal **Logs** window, output similar toohello following is logged in executing hello function:</span></span>

    2016-03-23T08:04:51  Welcome, you are now connected toolog-streaming service.
    2016-03-23T08:04:57.107 Function started (Id=dc5db8b1-6f1c-4117-b5c4-f6b602d538f7)
    2016-03-23T08:04:57.763 HTTP trigger function processed a request. RequestUri=https://functions841def78.azurewebsites.net/api/WesmcHttpTriggerNodeJS1?code=XXXXXXXXXX==
    2016-03-23T08:04:57.763 Request Headers = {"cache-control":"no-cache","connection":"Keep-Alive","accept":"*/*","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T08:04:57.763 Processing user info from request body...
    2016-03-23T08:04:57.763 Processing User Information...
    2016-03-23T08:04:57.763 name = Wes testing with Postman
    2016-03-23T08:04:57.763 address = Seattle, W.A. 98101
    2016-03-23T08:04:57.795 Function completed (Success, Id=dc5db8b1-6f1c-4117-b5c4-f6b602d538f7)

### <a name="test-with-curl-from-hello-command-line"></a><span data-ttu-id="e36b4-154">Testa med cURL från hello kommandorad</span><span class="sxs-lookup"><span data-stu-id="e36b4-154">Test with cURL from hello command line</span></span>
<span data-ttu-id="e36b4-155">Ofta när du testar programmet är det inte nödvändigt toolook alla ytterligare än hello kommandoraden toohelp debug ditt program.</span><span class="sxs-lookup"><span data-stu-id="e36b4-155">Often when you're testing software, it's not necessary toolook any further than hello command line toohelp debug your application.</span></span> <span data-ttu-id="e36b4-156">Detta är inte annorlunda med testning funktioner.</span><span class="sxs-lookup"><span data-stu-id="e36b4-156">This is no different with testing functions.</span></span> <span data-ttu-id="e36b4-157">Observera att hello cURL är tillgängliga som standard på Linux-baserade system.</span><span class="sxs-lookup"><span data-stu-id="e36b4-157">Note that hello cURL is available by default on Linux-based systems.</span></span> <span data-ttu-id="e36b4-158">I Windows, måste du först hämta och installera hello [cURL verktyget](https://curl.haxx.se/).</span><span class="sxs-lookup"><span data-stu-id="e36b4-158">On Windows, you must first download and install hello [cURL tool](https://curl.haxx.se/).</span></span>

<span data-ttu-id="e36b4-159">tootest hello funktion som vi definierade tidigare, kopiera hello **funktions-URL** från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="e36b4-159">tootest hello function that we defined earlier, copy hello **Function URL** from hello portal.</span></span> <span data-ttu-id="e36b4-160">Det har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="e36b4-160">It has hello following form:</span></span>

    https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

<span data-ttu-id="e36b4-161">Det här är hello URL för att utlösa funktionen.</span><span class="sxs-lookup"><span data-stu-id="e36b4-161">This is hello URL for triggering your function.</span></span> <span data-ttu-id="e36b4-162">Testa detta genom att använda hello cURL-kommando på hello kommandoraden toomake GET (`-G` eller `--get`) begäran mot hello funktionen:</span><span class="sxs-lookup"><span data-stu-id="e36b4-162">Test this by using hello cURL command on hello command line toomake a GET (`-G` or `--get`) request against hello function:</span></span>

    curl -G https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code>

<span data-ttu-id="e36b4-163">Det här exemplet kräver en frågesträngsparameter som kan skickas som Data (`-d`) i hello cURL-kommando:</span><span class="sxs-lookup"><span data-stu-id="e36b4-163">This particular example requires a query string parameter, which can be passed as Data (`-d`) in hello cURL command:</span></span>

    curl -G https://<Your Function App>.azurewebsites.net/api/<Your Function Name>?code=<your access code> -d name=<Enter a name here>

<span data-ttu-id="e36b4-164">Kör hello, och du se hello följande utdata för hello-funktionen på hello kommandoraden:</span><span class="sxs-lookup"><span data-stu-id="e36b4-164">Run hello command, and you see hello following output of hello function on hello command line:</span></span>

![Skärmbild av kommandotolk-utdata](./media/functions-test-a-function/curl-test.png)

<span data-ttu-id="e36b4-166">I hello portal **loggar** fönstret utdata liknande toohello följande loggas vid körning hello funktionen:</span><span class="sxs-lookup"><span data-stu-id="e36b4-166">In hello portal **Logs** window, output similar toohello following is logged in executing hello function:</span></span>

    2016-04-05T21:55:09  Welcome, you are now connected toolog-streaming service.
    2016-04-05T21:55:30.738 Function started (Id=ae6955da-29db-401a-b706-482fcd1b8f7a)
    2016-04-05T21:55:30.738 Node.js HTTP trigger function processed a request. RequestUri=https://functionsExample.azurewebsites.net/api/HttpTriggerNodeJS1?code=XXXXXXX&name=Azure Functions
    2016-04-05T21:55:30.738 Function completed (Success, Id=ae6955da-29db-401a-b706-482fcd1b8f7a)

### <a name="test-a-blob-trigger-by-using-storage-explorer"></a><span data-ttu-id="e36b4-167">Testa en blob-utlösare med Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="e36b4-167">Test a blob trigger by using Storage Explorer</span></span>
<span data-ttu-id="e36b4-168">Du kan testa en utlösare blob-funktion med hjälp av [Azure Lagringsutforskaren](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="e36b4-168">You can test a blob trigger function by using [Azure Storage Explorer](http://storageexplorer.com/).</span></span>

1. <span data-ttu-id="e36b4-169">I hello [Azure-portalen] för funktionen appen, skapa en C#, F # eller JavaScript blob Utlösarfunktion.</span><span class="sxs-lookup"><span data-stu-id="e36b4-169">In hello [Azure portal] for your function app, create a C#, F# or JavaScript blob trigger function.</span></span> <span data-ttu-id="e36b4-170">Ange hello toomonitor toohello sökvägen för blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="e36b4-170">Set hello path toomonitor toohello name of your blob container.</span></span> <span data-ttu-id="e36b4-171">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e36b4-171">For example:</span></span>

        files
2. <span data-ttu-id="e36b4-172">Klicka på hello  **+**  knappen tooselect eller skapa hello-lagringskonto som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="e36b4-172">Click hello **+** button tooselect or create hello storage account you want toouse.</span></span> <span data-ttu-id="e36b4-173">Klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e36b4-173">Then click **Create**.</span></span>
3. <span data-ttu-id="e36b4-174">Skapa en textfil med hello följande text och spara den:</span><span class="sxs-lookup"><span data-stu-id="e36b4-174">Create a text file with hello following text, and save it:</span></span>

        A text file for blob trigger function testing.
4. <span data-ttu-id="e36b4-175">Kör [Azure Lagringsutforskaren](http://storageexplorer.com/), och Anslut toohello blob-behållaren i hello storage-konto som övervakas.</span><span class="sxs-lookup"><span data-stu-id="e36b4-175">Run [Azure Storage Explorer](http://storageexplorer.com/), and connect toohello blob container in hello storage account being monitored.</span></span>
5. <span data-ttu-id="e36b4-176">Klicka på **överför** tooupload hello textfil.</span><span class="sxs-lookup"><span data-stu-id="e36b4-176">Click **Upload** tooupload hello text file.</span></span>

    ![Skärmbild av Lagringsutforskaren](./media/functions-test-a-function/azure-storage-explorer-test.png)

<span data-ttu-id="e36b4-178">hello standard blob utlösaren Funktionskoden rapporterar hello bearbetningen av hello blob i hello loggar:</span><span class="sxs-lookup"><span data-stu-id="e36b4-178">hello default blob trigger function code reports hello processing of hello blob in hello logs:</span></span>

    2016-03-24T11:30:10  Welcome, you are now connected toolog-streaming service.
    2016-03-24T11:30:34.472 Function started (Id=739ebc07-ff9e-4ec4-a444-e479cec2e460)
    2016-03-24T11:30:34.472 C# Blob trigger function processed: A text file for blob trigger function testing.
    2016-03-24T11:30:34.472 Function completed (Success, Id=739ebc07-ff9e-4ec4-a444-e479cec2e460)

## <a name="test-a-function-within-functions"></a><span data-ttu-id="e36b4-179">Testa en funktion i funktioner</span><span class="sxs-lookup"><span data-stu-id="e36b4-179">Test a function within functions</span></span>
<span data-ttu-id="e36b4-180">hello Azure Functions-portalen är utformad toolet du testa HTTP och tidsinställda utlösts funktioner.</span><span class="sxs-lookup"><span data-stu-id="e36b4-180">hello Azure Functions portal is designed toolet you test HTTP and timer triggered functions.</span></span> <span data-ttu-id="e36b4-181">Du kan också skapa funktioner tootrigger andra funktioner som du vill testa.</span><span class="sxs-lookup"><span data-stu-id="e36b4-181">You can also create functions tootrigger other functions that you are testing.</span></span>

### <a name="test-with-hello-functions-portal-run-button"></a><span data-ttu-id="e36b4-182">Testa med portalen kör-knappen i hello funktion</span><span class="sxs-lookup"><span data-stu-id="e36b4-182">Test with hello Functions portal Run button</span></span>
<span data-ttu-id="e36b4-183">Hej och ger en **kör** knapp som du kan använda toodo vissa begränsade testning.</span><span class="sxs-lookup"><span data-stu-id="e36b4-183">hello portal provides a **Run** button that you can use toodo some limited testing.</span></span> <span data-ttu-id="e36b4-184">Du kan ange en begärantext hello-knappen, men du kan inte ange frågeparametrar sträng eller uppdaterar huvuden för begäran.</span><span class="sxs-lookup"><span data-stu-id="e36b4-184">You can provide a request body by using hello button, but you can't provide query string parameters or update request headers.</span></span>

<span data-ttu-id="e36b4-185">Testa hello HTTP utlösaren funktionen som vi skapade tidigare genom att lägga till en JSON-sträng liknande toohello följande i hello **Begärandetext** fältet.</span><span class="sxs-lookup"><span data-stu-id="e36b4-185">Test hello HTTP trigger function we created earlier by adding a JSON string similar toohello following in hello **Request body** field.</span></span> <span data-ttu-id="e36b4-186">Klicka på hello **kör** knappen.</span><span class="sxs-lookup"><span data-stu-id="e36b4-186">Then click hello **Run** button.</span></span>

```json
{
    "name" : "Wes testing Run button",
    "address" : "USA"
}
```

<span data-ttu-id="e36b4-187">I hello portal **loggar** fönstret utdata liknande toohello följande loggas vid körning hello funktionen:</span><span class="sxs-lookup"><span data-stu-id="e36b4-187">In hello portal **Logs** window, output similar toohello following is logged in executing hello function:</span></span>

    2016-03-23T08:03:12  Welcome, you are now connected toolog-streaming service.
    2016-03-23T08:03:17.357 Function started (Id=753a01b0-45a8-4125-a030-3ad543a89409)
    2016-03-23T08:03:18.697 HTTP trigger function processed a request. RequestUri=https://functions841def78.azurewebsites.net/api/wesmchttptriggernodejs1
    2016-03-23T08:03:18.697 Request Headers = {"connection":"Keep-Alive","accept":"*/*","accept-encoding":"gzip","accept-language":"en-US"}
    2016-03-23T08:03:18.697 Processing user info from request body...
    2016-03-23T08:03:18.697 Processing User Information...
    2016-03-23T08:03:18.697 name = Wes testing Run button
    2016-03-23T08:03:18.697 address = USA
    2016-03-23T08:03:18.744 Function completed (Success, Id=753a01b0-45a8-4125-a030-3ad543a89409)


### <a name="test-with-a-timer-trigger"></a><span data-ttu-id="e36b4-188">Testa med en timer som utlösare</span><span class="sxs-lookup"><span data-stu-id="e36b4-188">Test with a timer trigger</span></span>
<span data-ttu-id="e36b4-189">Vissa funktioner kan inte testas tillräckligt med hello verktyg som tidigare nämnts.</span><span class="sxs-lookup"><span data-stu-id="e36b4-189">Some functions can't be adequately tested with hello tools mentioned previously.</span></span> <span data-ttu-id="e36b4-190">Anta till exempel att en kö Utlösarfunktion som körs när ett meddelande har släppts i [Azure Queue storage](../storage/queues/storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="e36b4-190">For example, consider a queue trigger function that runs when a message is dropped into [Azure Queue storage](../storage/queues/storage-dotnet-how-to-use-queues.md).</span></span> <span data-ttu-id="e36b4-191">Du skriva alltid kod toodrop meddelandet i kön och ett exempel på detta i konsolen projekt tillhandahålls nedan.</span><span class="sxs-lookup"><span data-stu-id="e36b4-191">You can always write code toodrop a message into your queue, and an example of this in a console project is provided later in this article.</span></span> <span data-ttu-id="e36b4-192">Det finns en annan metod som du kan använda som testar funktioner direkt.</span><span class="sxs-lookup"><span data-stu-id="e36b4-192">However, there is another approach you can use that tests functions directly.</span></span>  

<span data-ttu-id="e36b4-193">Du kan använda en timer som utlösare konfigurerats med en kö utdatabindning.</span><span class="sxs-lookup"><span data-stu-id="e36b4-193">You can use a timer trigger configured with a queue output binding.</span></span> <span data-ttu-id="e36b4-194">Timer utlösaren koden kan sedan skriva hello test toohello kön.</span><span class="sxs-lookup"><span data-stu-id="e36b4-194">That timer trigger code can then write hello test messages toohello queue.</span></span> <span data-ttu-id="e36b4-195">Det här avsnittet går igenom ett exempel.</span><span class="sxs-lookup"><span data-stu-id="e36b4-195">This section walks through an example.</span></span>

<span data-ttu-id="e36b4-196">Mer information om hur du använder bindningar med Azure Functions finns i avsnittet hello [Azure Functions för utvecklare](functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="e36b4-196">For more in-depth information on using bindings with Azure Functions, see hello [Azure Functions developer reference](functions-reference.md).</span></span>

#### <a name="create-a-queue-trigger-for-testing"></a><span data-ttu-id="e36b4-197">Skapa en kö utlösare för testning</span><span class="sxs-lookup"><span data-stu-id="e36b4-197">Create a queue trigger for testing</span></span>
<span data-ttu-id="e36b4-198">toodemonstrate den här metoden vi först skapa en kö Utlösarfunktion som vi vill tootest för en kö med namnet `queue-newusers`.</span><span class="sxs-lookup"><span data-stu-id="e36b4-198">toodemonstrate this approach, we first create a queue trigger function that we want tootest for a queue named `queue-newusers`.</span></span> <span data-ttu-id="e36b4-199">Den här funktionen bearbetas namn och adress information släppa i Queue storage för en ny användare.</span><span class="sxs-lookup"><span data-stu-id="e36b4-199">This function processes name and address information dropped into Queue storage for a new user.</span></span>

> [!NOTE]
> <span data-ttu-id="e36b4-200">Om du använder en annan kö, kontrollera hello-namn som du använder överensstämmer toohello [namngivning av köer och MetaData](https://msdn.microsoft.com/library/dd179349.aspx) regler.</span><span class="sxs-lookup"><span data-stu-id="e36b4-200">If you use a different queue name, make sure hello name you use conforms toohello [Naming Queues and MetaData](https://msdn.microsoft.com/library/dd179349.aspx) rules.</span></span> <span data-ttu-id="e36b4-201">Annars får du ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="e36b4-201">Otherwise, you get an error.</span></span>
>
>

1. <span data-ttu-id="e36b4-202">I hello [Azure-portalen] appen funktionen klickar du på **nya funktionen** > **QueueTrigger - C#**.</span><span class="sxs-lookup"><span data-stu-id="e36b4-202">In hello [Azure portal] for your function app, click **New Function** > **QueueTrigger - C#**.</span></span>
2. <span data-ttu-id="e36b4-203">Ange hello kön namn toobe övervakas av hello kön funktionen:</span><span class="sxs-lookup"><span data-stu-id="e36b4-203">Enter hello queue name toobe monitored by hello queue function:</span></span>

        queue-newusers
3. <span data-ttu-id="e36b4-204">Klicka på hello  **+**  knappen tooselect eller skapa hello-lagringskonto som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="e36b4-204">Click hello **+** button tooselect or create hello storage account you want toouse.</span></span> <span data-ttu-id="e36b4-205">Klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e36b4-205">Then click **Create**.</span></span>
4. <span data-ttu-id="e36b4-206">Lämna den här portalen webbläsarfönster öppen, så att du kan övervaka hello loggposter för mallen för hello standard kön funktionskoden.</span><span class="sxs-lookup"><span data-stu-id="e36b4-206">Leave this portal browser window open, so you can monitor hello log entries for hello default queue function template code.</span></span>

#### <a name="create-a-timer-trigger-toodrop-a-message-in-hello-queue"></a><span data-ttu-id="e36b4-207">Skapa en timer utlösaren toodrop ett meddelande i kön hello</span><span class="sxs-lookup"><span data-stu-id="e36b4-207">Create a timer trigger toodrop a message in hello queue</span></span>
1. <span data-ttu-id="e36b4-208">Öppna hello [Azure-portalen] i ett nytt webbläsarfönster och navigera tooyour funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="e36b4-208">Open hello [Azure portal] in a new browser window, and navigate tooyour function app.</span></span>
2. <span data-ttu-id="e36b4-209">Klicka på **nya funktionen** > **TimerTrigger - C#**.</span><span class="sxs-lookup"><span data-stu-id="e36b4-209">Click **New Function** > **TimerTrigger - C#**.</span></span> <span data-ttu-id="e36b4-210">Ange en cron uttryck tooset hur ofta hello timer kod testar kö-funktionen.</span><span class="sxs-lookup"><span data-stu-id="e36b4-210">Enter a cron expression tooset how often hello timer code tests your queue function.</span></span> <span data-ttu-id="e36b4-211">Klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e36b4-211">Then click **Create**.</span></span> <span data-ttu-id="e36b4-212">Om du vill hello test toorun med 30 sekunders mellanrum, kan du använda följande hello [CRON-uttryck](https://wikipedia.org/wiki/Cron#CRON_expression):</span><span class="sxs-lookup"><span data-stu-id="e36b4-212">If you want hello test toorun every 30 seconds, you can use hello following [CRON expression](https://wikipedia.org/wiki/Cron#CRON_expression):</span></span>

        */30 * * * * *
3. <span data-ttu-id="e36b4-213">Klicka på hello **integrera** för din nya timer som utlösare.</span><span class="sxs-lookup"><span data-stu-id="e36b4-213">Click hello **Integrate** tab for your new timer trigger.</span></span>
4. <span data-ttu-id="e36b4-214">Under **utdata**, klickar du på **+ nya utdata**.</span><span class="sxs-lookup"><span data-stu-id="e36b4-214">Under **Output**, click **+ New Output**.</span></span> <span data-ttu-id="e36b4-215">Klicka på **kön** och **Välj**.</span><span class="sxs-lookup"><span data-stu-id="e36b4-215">Then click **queue** and **Select**.</span></span>
5. <span data-ttu-id="e36b4-216">Obs hello namn i hello **meddelandet köobjekt**.</span><span class="sxs-lookup"><span data-stu-id="e36b4-216">Note hello name you use for hello **queue message object**.</span></span> <span data-ttu-id="e36b4-217">Du kan använda den i hello timer funktionskoden.</span><span class="sxs-lookup"><span data-stu-id="e36b4-217">You use this in hello timer function code.</span></span>

        myQueue
6. <span data-ttu-id="e36b4-218">Ange hello könamnet där hello-meddelande skickas:</span><span class="sxs-lookup"><span data-stu-id="e36b4-218">Enter hello queue name where hello message is sent:</span></span>

        queue-newusers
7. <span data-ttu-id="e36b4-219">Klicka på hello  **+**  knappen tooselect hello storage-konto som du tidigare använt med hello kö utlösaren.</span><span class="sxs-lookup"><span data-stu-id="e36b4-219">Click hello **+** button tooselect hello storage account you used previously with hello queue trigger.</span></span> <span data-ttu-id="e36b4-220">Klicka sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="e36b4-220">Then click **Save**.</span></span>
8. <span data-ttu-id="e36b4-221">Klicka på hello **utveckla** för din timer som utlösare.</span><span class="sxs-lookup"><span data-stu-id="e36b4-221">Click hello **Develop** tab for your timer trigger.</span></span>
9. <span data-ttu-id="e36b4-222">Du kan använda hello följande kod för hello C# timer-funktionen, så länge som du använde hello samma kö meddelandet objektnamn som visades tidigare.</span><span class="sxs-lookup"><span data-stu-id="e36b4-222">You can use hello following code for hello C# timer function, as long as you used hello same queue message object name shown earlier.</span></span> <span data-ttu-id="e36b4-223">Klicka sedan på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="e36b4-223">Then click **Save**.</span></span>

    ```cs
    using System;

    public static void Run(TimerInfo myTimer, out String myQueue, TraceWriter log)
    {
        String newUser =
        "{\"name\":\"User testing from C# timer function\",\"address\":\"XYZ\"}";

        log.Verbose($"C# Timer trigger function executed at: {DateTime.Now}");   
        log.Verbose($"{newUser}");   

        myQueue = newUser;
    }
    ```

<span data-ttu-id="e36b4-224">Hello C# funktionen timer kör nu med 30 sekunders mellanrum om du använde hello exempel cron-uttryck.</span><span class="sxs-lookup"><span data-stu-id="e36b4-224">At this point, hello C# timer function executes every 30 seconds if you used hello example cron expression.</span></span> <span data-ttu-id="e36b4-225">hello loggar för hello timer funktionen rapportera varje körning:</span><span class="sxs-lookup"><span data-stu-id="e36b4-225">hello logs for hello timer function report each execution:</span></span>

    2016-03-24T10:27:02  Welcome, you are now connected toolog-streaming service.
    2016-03-24T10:27:30.004 Function started (Id=04061790-974f-4043-b851-48bd4ac424d1)
    2016-03-24T10:27:30.004 C# Timer trigger function executed at: 3/24/2016 10:27:30 AM
    2016-03-24T10:27:30.004 {"name":"User testing from C# timer function","address":"XYZ"}
    2016-03-24T10:27:30.004 Function completed (Success, Id=04061790-974f-4043-b851-48bd4ac424d1)

<span data-ttu-id="e36b4-226">Du kan se varje meddelande bearbetas i hello webbläsarfönster för hello kön funktionen:</span><span class="sxs-lookup"><span data-stu-id="e36b4-226">In hello browser window for hello queue function, you can see each message being processed:</span></span>

    2016-03-24T10:27:06  Welcome, you are now connected toolog-streaming service.
    2016-03-24T10:27:30.607 Function started (Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)
    2016-03-24T10:27:30.607 C# Queue trigger function processed: {"name":"User testing from C# timer function","address":"XYZ"}
    2016-03-24T10:27:30.607 Function completed (Success, Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)

## <a name="test-a-function-with-code"></a><span data-ttu-id="e36b4-227">Testa en funktion med kod</span><span class="sxs-lookup"><span data-stu-id="e36b4-227">Test a function with code</span></span>
<span data-ttu-id="e36b4-228">Du kan behöva toocreate ett externt program eller framework tootest dina funktioner.</span><span class="sxs-lookup"><span data-stu-id="e36b4-228">You may need toocreate an external application or framework tootest your functions.</span></span>

### <a name="test-an-http-trigger-function-with-code-nodejs"></a><span data-ttu-id="e36b4-229">Testa en HTTP-Utlösarfunktion med kod: Node.js</span><span class="sxs-lookup"><span data-stu-id="e36b4-229">Test an HTTP trigger function with code: Node.js</span></span>
<span data-ttu-id="e36b4-230">Du kan använda en Node.js-app tooexecute en HTTP-begäran tootest din funktion.</span><span class="sxs-lookup"><span data-stu-id="e36b4-230">You can use a Node.js app tooexecute an HTTP request tootest your function.</span></span>
<span data-ttu-id="e36b4-231">Se till att tooset:</span><span class="sxs-lookup"><span data-stu-id="e36b4-231">Make sure tooset:</span></span>

* <span data-ttu-id="e36b4-232">Hej `host` hello begäran alternativ tooyour funktionen app värden.</span><span class="sxs-lookup"><span data-stu-id="e36b4-232">hello `host` in hello request options tooyour function app host.</span></span>
* <span data-ttu-id="e36b4-233">Din funktionsnamnet i hello `path`.</span><span class="sxs-lookup"><span data-stu-id="e36b4-233">Your function name in hello `path`.</span></span>
* <span data-ttu-id="e36b4-234">Koden åtkomst (`<your code>`) i hello `path`.</span><span class="sxs-lookup"><span data-stu-id="e36b4-234">Your access code (`<your code>`) in hello `path`.</span></span>

<span data-ttu-id="e36b4-235">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e36b4-235">Code example:</span></span>

```javascript
var http = require("http");

var nameQueryString = "name=Wes%20Query%20String%20Test%20From%20Node.js";

var nameBodyJSON = {
    name : "Wes testing with Node.JS code",
    address : "Dallas, T.X. 75201"
};

var bodyString = JSON.stringify(nameBodyJSON);

var options = {
  host: "functions841def78.azurewebsites.net",
  //path: "/api/HttpTriggerNodeJS2?code=sc1wt62opn7k9buhrm8jpds4ikxvvj42m5ojdt0p91lz5jnhfr2c74ipoujyq26wab3wk5gkfbt9&" + nameQueryString,
  path: "/api/HttpTriggerNodeJS2?code=sc1wt62opn7k9buhrm8jpds4ikxvvj42m5ojdt0p91lz5jnhfr2c74ipoujyq26wab3wk5gkfbt9",
  method: "POST",
  headers : {
      "Content-Type":"application/json",
      "Content-Length": Buffer.byteLength(bodyString)
    }    
};

callback = function(response) {
  var str = ""
  response.on("data", function (chunk) {
    str += chunk;
  });

  response.on("end", function () {
    console.log(str);
  });
}

var req = http.request(options, callback);
console.log("*** Sending name and address in body ***");
console.log(bodyString);
req.end(bodyString);
```


<span data-ttu-id="e36b4-236">Resultat:</span><span class="sxs-lookup"><span data-stu-id="e36b4-236">Output:</span></span>

    C:\Users\Wesley\testing\Node.js>node testHttpTriggerExample.js
    *** Sending name and address in body ***
    {"name" : "Wes testing with Node.JS code","address" : "Dallas, T.X. 75201"}
    Hello Wes testing with Node.JS code
    hello address you provided is Dallas, T.X. 75201

<span data-ttu-id="e36b4-237">I hello portal **loggar** fönstret utdata liknande toohello följande loggas vid körning hello funktionen:</span><span class="sxs-lookup"><span data-stu-id="e36b4-237">In hello portal **Logs** window, output similar toohello following is logged in executing hello function:</span></span>

    2016-03-23T08:08:55  Welcome, you are now connected toolog-streaming service.
    2016-03-23T08:08:59.736 Function started (Id=607b891c-08a1-427f-910c-af64ae4f7f9c)
    2016-03-23T08:09:01.153 HTTP trigger function processed a request. RequestUri=http://functionsExample.azurewebsites.net/api/WesmcHttpTriggerNodeJS1/?code=XXXXXXXXXX==
    2016-03-23T08:09:01.153 Request Headers = {"connection":"Keep-Alive","host":"functionsExample.azurewebsites.net"}
    2016-03-23T08:09:01.153 Name not provided as query string param. Checking body...
    2016-03-23T08:09:01.153 Request Body Type = object
    2016-03-23T08:09:01.153 Request Body = [object Object]
    2016-03-23T08:09:01.153 Processing User Information...
    2016-03-23T08:09:01.215 Function completed (Success, Id=607b891c-08a1-427f-910c-af64ae4f7f9c)


### <a name="test-a-queue-trigger-function-with-code-c"></a><span data-ttu-id="e36b4-238">Testa en kö Utlösarfunktion med koden: C#</span><span class="sxs-lookup"><span data-stu-id="e36b4-238">Test a queue trigger function with code: C#</span></span> #
<span data-ttu-id="e36b4-239">Vi nämnt tidigare kan du testa en utlösare för kön med hjälp av koden toodrop meddelandet i kön.</span><span class="sxs-lookup"><span data-stu-id="e36b4-239">We mentioned earlier that you can test a queue trigger by using code toodrop a message in your queue.</span></span> <span data-ttu-id="e36b4-240">Följande exempelkod hello baseras på hello C#-koden som visas i hello [komma igång med Azure Queue storage](../storage/queues/storage-dotnet-how-to-use-queues.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="e36b4-240">hello following example code is based on hello C# code presented in hello [Getting started with Azure Queue storage](../storage/queues/storage-dotnet-how-to-use-queues.md) tutorial.</span></span> <span data-ttu-id="e36b4-241">Koden för andra språk är också tillgänglig från den länken.</span><span class="sxs-lookup"><span data-stu-id="e36b4-241">Code for other languages is also available from that link.</span></span>

<span data-ttu-id="e36b4-242">tootest denna kod i en konsolapp måste du:</span><span class="sxs-lookup"><span data-stu-id="e36b4-242">tootest this code in a console app, you must:</span></span>

* <span data-ttu-id="e36b4-243">[Konfigurera anslutningssträngen för lagring i hello app.config-filen](../storage/queues/storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="e36b4-243">[Configure your storage connection string in hello app.config file](../storage/queues/storage-dotnet-how-to-use-queues.md).</span></span>
* <span data-ttu-id="e36b4-244">Skicka en `name` och `address` som parametrar toohello app.</span><span class="sxs-lookup"><span data-stu-id="e36b4-244">Pass a `name` and `address` as parameters toohello app.</span></span> <span data-ttu-id="e36b4-245">Till exempel `C:\myQueueConsoleApp\test.exe "Wes testing queues" "in a console app"`.</span><span class="sxs-lookup"><span data-stu-id="e36b4-245">For example, `C:\myQueueConsoleApp\test.exe "Wes testing queues" "in a console app"`.</span></span> <span data-ttu-id="e36b4-246">(Den här koden accepterar hello namn och adress för en ny användare som kommandoradsargument under körningen.)</span><span class="sxs-lookup"><span data-stu-id="e36b4-246">(This code accepts hello name and address for a new user as command-line arguments during runtime.)</span></span>

<span data-ttu-id="e36b4-247">C#-exempelkod:</span><span class="sxs-lookup"><span data-stu-id="e36b4-247">Example C# code:</span></span>

```cs
static void Main(string[] args)
{
    string name = null;
    string address = null;
    string queueName = "queue-newusers";
    string JSON = null;

    if (args.Length > 0)
    {
        name = args[0];
    }
    if (args.Length > 1)
    {
        address = args[1];
    }

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConfigurationManager.AppSettings["StorageConnectionString"]);

    // Create hello queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference tooa queue
    CloudQueue queue = queueClient.GetQueueReference(queueName);

    // Create hello queue if it doesn't already exist
    queue.CreateIfNotExists();

    // Create a message and add it toohello queue.
    if (name != null)
    {
        if (address != null)
            JSON = String.Format("{{\"name\":\"{0}\",\"address\":\"{1}\"}}", name, address);
        else
            JSON = String.Format("{{\"name\":\"{0}\"}}", name);
    }

    Console.WriteLine("Adding message too" + queueName + "...");
    Console.WriteLine(JSON);

    CloudQueueMessage message = new CloudQueueMessage(JSON);
    queue.AddMessage(message);
}
```

<span data-ttu-id="e36b4-248">Du kan se varje meddelande bearbetas i hello webbläsarfönster för hello kön funktionen:</span><span class="sxs-lookup"><span data-stu-id="e36b4-248">In hello browser window for hello queue function, you can see each message being processed:</span></span>

    2016-03-24T10:27:06  Welcome, you are now connected toolog-streaming service.
    2016-03-24T10:27:30.607 Function started (Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)
    2016-03-24T10:27:30.607 C# Queue trigger function processed: {"name":"Wes testing queues","address":"in a console app"}
    2016-03-24T10:27:30.607 Function completed (Success, Id=e304450c-ff48-44dc-ba2e-1df7209a9d22)


<!-- URLs. -->

[Azure-portalen]: https://portal.azure.com
