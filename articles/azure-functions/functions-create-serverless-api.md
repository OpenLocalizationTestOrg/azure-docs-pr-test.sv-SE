---
title: "aaaCreate ett serverlösa API med hjälp av Azure Functions | Microsoft Docs"
description: "Hur toocreate ett serverlösa API med hjälp av Azure-funktioner"
services: functions
author: mattchenderson
manager: erikre
ms.service: functions
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: tutorial
ms.date: 05/04/2017
ms.author: mahender
ms.custom: mvc
ms.openlocfilehash: 877e3b229d5477fc5fec594ccd284fb55d7f3c07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-serverless-api-using-azure-functions"></a><span data-ttu-id="3f9cf-103">Skapa en serverlösa API med hjälp av Azure-funktioner</span><span class="sxs-lookup"><span data-stu-id="3f9cf-103">Create a serverless API using Azure Functions</span></span>

<span data-ttu-id="3f9cf-104">I den här kursen får du lära dig hur Azure Functions kan du toobuild mycket skalbar API: er.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-104">In this tutorial you will learn how Azure Functions allows you toobuild highly-scalable APIs.</span></span> <span data-ttu-id="3f9cf-105">Azure Functions innehåller en uppsättning inbyggda HTTP-utlösare och bindningar som gör det enkelt tooauthor en slutpunkt på flera olika språk, inklusive Node.JS, C# och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-105">Azure Functions comes with a collection of built-in HTTP triggers and bindings which make it easy tooauthor an endpoint in a variety of languages, including Node.JS, C#, and more.</span></span> <span data-ttu-id="3f9cf-106">I den här självstudiekursen kommer du anpassa en HTTP-utlösaren toohandle specifika åtgärder i utformningen av din API.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-106">In this tutorial, you will customize an HTTP trigger toohandle specific actions in your API design.</span></span> <span data-ttu-id="3f9cf-107">Dessutom förbereder du för växande din API genom att integrera med Azure Functions proxyservrar och ställa in fingerad API: er.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-107">You will also prepare for growing your API by integrating it with Azure Functions Proxies and setting up mock APIs.</span></span> <span data-ttu-id="3f9cf-108">Allt detta sker ovanpå hello funktioner serverlösa beräkning miljö, så att du inte har tooworry om att skala resurser – du kan bara fokusera på din API-logik.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-108">All of this is accomplished on top of hello Functions serverless compute environment, so you don't have tooworry about scaling resources - you can just focus on your API logic.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f9cf-109">Krav</span><span class="sxs-lookup"><span data-stu-id="3f9cf-109">Prerequisites</span></span> 

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

<span data-ttu-id="3f9cf-110">hello resulterande funktionen används för hello resten av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-110">hello resulting function will be used for hello rest of this tutorial.</span></span>

### <a name="sign-in-tooazure"></a><span data-ttu-id="3f9cf-111">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="3f9cf-111">Sign in tooAzure</span></span>

<span data-ttu-id="3f9cf-112">Öppna hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-112">Open hello Azure portal.</span></span> <span data-ttu-id="3f9cf-113">toodo, logga in för[https://portal.azure.com](https://portal.azure.com) med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-113">toodo this, sign in too[https://portal.azure.com](https://portal.azure.com) with your Azure account.</span></span>

## <a name="customize-your-http-function"></a><span data-ttu-id="3f9cf-114">Anpassa din HTTP-funktion</span><span class="sxs-lookup"><span data-stu-id="3f9cf-114">Customize your HTTP function</span></span>

<span data-ttu-id="3f9cf-115">HTTP-utlösta-funktionen är som standard konfigurerade tooaccept HTTP-metoden.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-115">By default, your HTTP-triggered function is configured tooaccept any HTTP method.</span></span> <span data-ttu-id="3f9cf-116">Det finns också en standard-URL i formatet hello `http://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>`.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-116">There is also a default URL of hello form `http://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>`.</span></span> <span data-ttu-id="3f9cf-117">Om du har följt hello quickstart sedan `<funcname>` förmodligen ser ut ungefär så ”HttpTriggerJS1”.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-117">If you followed hello quickstart, then `<funcname>` probably looks something like "HttpTriggerJS1".</span></span> <span data-ttu-id="3f9cf-118">I det här avsnittet ska du ändra hello funktionen toorespond endast tooGET förfrågningar mot `/api/hello` dirigera i stället.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-118">In this section, you will modify hello function toorespond only tooGET requests against `/api/hello` route instead.</span></span> 

<span data-ttu-id="3f9cf-119">Navigera tooyour funktion i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-119">Navigate tooyour function in hello Azure portal.</span></span> <span data-ttu-id="3f9cf-120">Välj **integrera** i hello vänstra navigeringsrutan.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-120">Select **Integrate** in hello left navigation.</span></span>

![Anpassa en HTTP-funktion](./media/functions-create-serverless-api/customizing-http.png)

<span data-ttu-id="3f9cf-122">Använda HTTP-inställningarna som anges i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-122">Use the HTTP trigger settings as specified in hello table.</span></span>

| <span data-ttu-id="3f9cf-123">Fält</span><span class="sxs-lookup"><span data-stu-id="3f9cf-123">Field</span></span> | <span data-ttu-id="3f9cf-124">Exempelvärde</span><span class="sxs-lookup"><span data-stu-id="3f9cf-124">Sample value</span></span> | <span data-ttu-id="3f9cf-125">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3f9cf-125">Description</span></span> |
|---|---|---|
| <span data-ttu-id="3f9cf-126">Tillåtna HTTP-metoder</span><span class="sxs-lookup"><span data-stu-id="3f9cf-126">Allowed HTTP methods</span></span> | <span data-ttu-id="3f9cf-127">Valda metoder</span><span class="sxs-lookup"><span data-stu-id="3f9cf-127">Selected methods</span></span> | <span data-ttu-id="3f9cf-128">Bestämmer vilka HTTP-metoder kan vara används tooinvoke den här funktionen</span><span class="sxs-lookup"><span data-stu-id="3f9cf-128">Determines what HTTP methods may be used tooinvoke this function</span></span> |
| <span data-ttu-id="3f9cf-129">Den valda http-metoder</span><span class="sxs-lookup"><span data-stu-id="3f9cf-129">Selected HTTP methods</span></span> | <span data-ttu-id="3f9cf-130">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="3f9cf-130">GET</span></span> | <span data-ttu-id="3f9cf-131">Tillåter endast valda http-metoderna toobe används tooinvoke för den här funktionen</span><span class="sxs-lookup"><span data-stu-id="3f9cf-131">Allows only selected HTTP methods toobe used tooinvoke this function</span></span> |
| <span data-ttu-id="3f9cf-132">Flödesmallen</span><span class="sxs-lookup"><span data-stu-id="3f9cf-132">Route template</span></span> | <span data-ttu-id="3f9cf-133">Sverige</span><span class="sxs-lookup"><span data-stu-id="3f9cf-133">/hello</span></span> | <span data-ttu-id="3f9cf-134">Anger vilka vägen används tooinvoke den här funktionen</span><span class="sxs-lookup"><span data-stu-id="3f9cf-134">Determines what route is used tooinvoke this function</span></span> |

<span data-ttu-id="3f9cf-135">Observera att du inte inkluderade hello `/api` basera sökväg prefix i hello flödesmallen som hanteras av en global inställning.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-135">Note that you did not include hello `/api` base path prefix in hello route template, as this is handled by a global setting.</span></span>

<span data-ttu-id="3f9cf-136">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-136">Click **Save**.</span></span>

<span data-ttu-id="3f9cf-137">Du kan lära dig mer om hur du anpassar http-funktioner i [Azure Functions HTTP och webhook bindningar](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#customizing-the-http-endpoint).</span><span class="sxs-lookup"><span data-stu-id="3f9cf-137">You can learn more about customizing HTTP functions in [Azure Functions HTTP and webhook bindings](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#customizing-the-http-endpoint).</span></span>

### <a name="test-your-api"></a><span data-ttu-id="3f9cf-138">Testa ditt API</span><span class="sxs-lookup"><span data-stu-id="3f9cf-138">Test your API</span></span>

<span data-ttu-id="3f9cf-139">Sedan testa funktionen-toosee den arbetar med hello nya API-ytan.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-139">Next, test your function toosee it working with hello new API surface.</span></span>

<span data-ttu-id="3f9cf-140">Gå tillbaka toohello development sidan genom att klicka på hello funktionens namn i hello vänstra navigeringsrutan.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-140">Navigate back toohello development page by clicking on hello function's name in hello left navigation.</span></span>

<span data-ttu-id="3f9cf-141">Klicka på **få funktionen URL** och kopiera hello-URL.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-141">Click **Get function URL** and copy hello URL.</span></span> <span data-ttu-id="3f9cf-142">Du bör se till att den använder hello `/api/hello` vidarebefordra nu.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-142">You should see that it uses hello `/api/hello` route now.</span></span>

<span data-ttu-id="3f9cf-143">Kopiera hello URL till en ny webbläsarflik eller önskade REST-klient.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-143">Copy hello URL into a new browser tab or your preferred REST client.</span></span> <span data-ttu-id="3f9cf-144">Webbläsare använder GET som standard.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-144">Browsers will use GET by default.</span></span>

<span data-ttu-id="3f9cf-145">Kör hello-funktionen och bekräfta att den fungerar.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-145">Run hello function and confirm that it is working.</span></span> <span data-ttu-id="3f9cf-146">Du kan behöva tooprovide hello ”name”-parametern som en fråga sträng toosatisfy hello quickstart kod.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-146">You may need tooprovide hello "name" parameter as a query string toosatisfy hello quickstart code.</span></span>

<span data-ttu-id="3f9cf-147">Du kan också försöka hello slutpunkt med ett annat HTTP-metoden tooconfirm inte utförs hello-funktionen anropas.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-147">You can also try calling hello endpoint with another HTTP method tooconfirm that hello function is not executed.</span></span> <span data-ttu-id="3f9cf-148">För att göra detta behöver du toouse en REST-klient, till exempel cURL, Postman eller Fiddler.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-148">For this, you will need toouse a REST client, such as cURL, Postman, or Fiddler.</span></span>

## <a name="proxies-overview"></a><span data-ttu-id="3f9cf-149">Översikt över proxyservrar</span><span class="sxs-lookup"><span data-stu-id="3f9cf-149">Proxies overview</span></span>

<span data-ttu-id="3f9cf-150">I nästa avsnitt om hello, kommer du ansluta din API via en proxyserver.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-150">In hello next section, you will surface your API through a proxy.</span></span> <span data-ttu-id="3f9cf-151">Azure Functions proxyservrar är en förhandsversion av funktionen som gör att du tooforward begäranden tooother resurser.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-151">Azure Functions Proxies is a preview feature that allows you tooforward requests tooother resources.</span></span> <span data-ttu-id="3f9cf-152">Du definierar en HTTP-slutpunkt precis som med HTTP-utlösare, men i stället för att skriva kod tooexecute när denna slutpunkt anropas, du anger en URL tooa remote implementering.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-152">You define an HTTP endpoint just like with HTTP trigger, but instead of writing code tooexecute when that endpoint is called, you provide a URL tooa remote implementation.</span></span> <span data-ttu-id="3f9cf-153">Detta ger dig toocompose flera API datakällor i en enda API-yta som är enkelt för klienter tooconsume.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-153">This allows you toocompose multiple API sources into a single API surface which is easy for clients tooconsume.</span></span> <span data-ttu-id="3f9cf-154">Detta är särskilt användbart om du inte vill toobuild din API som mikrotjänster.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-154">This is particularly useful if you wish toobuild your API as microservices.</span></span>

<span data-ttu-id="3f9cf-155">En proxy kan peka tooany HTTP-resurs, som:</span><span class="sxs-lookup"><span data-stu-id="3f9cf-155">A proxy can point tooany HTTP resource, such as:</span></span>
- <span data-ttu-id="3f9cf-156">Azure Functions</span><span class="sxs-lookup"><span data-stu-id="3f9cf-156">Azure Functions</span></span> 
- <span data-ttu-id="3f9cf-157">API apps i [Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is)</span><span class="sxs-lookup"><span data-stu-id="3f9cf-157">API apps in [Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is)</span></span>
- <span data-ttu-id="3f9cf-158">Docker-behållare i [Apptjänst på Linux](https://docs.microsoft.com/azure/app-service/app-service-linux-readme)</span><span class="sxs-lookup"><span data-stu-id="3f9cf-158">Docker containers in [App Service on Linux](https://docs.microsoft.com/azure/app-service/app-service-linux-readme)</span></span>
- <span data-ttu-id="3f9cf-159">Värdbaserade API</span><span class="sxs-lookup"><span data-stu-id="3f9cf-159">Any other hosted API</span></span>

<span data-ttu-id="3f9cf-160">toolearn mer information om proxyservrar, se [arbeta med Azure Functions-proxyservrar (förhandsgranskning)].</span><span class="sxs-lookup"><span data-stu-id="3f9cf-160">toolearn more about proxies, see [Working with Azure Functions Proxies (preview)].</span></span>

## <a name="create-your-first-proxy"></a><span data-ttu-id="3f9cf-161">Skapa din första proxy</span><span class="sxs-lookup"><span data-stu-id="3f9cf-161">Create your first proxy</span></span>

<span data-ttu-id="3f9cf-162">I det här avsnittet skapar du en ny proxy som fungerar som en klientdel tooyour övergripande API.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-162">In this section, you will create a new proxy which serves as a frontend tooyour overall API.</span></span> 

### <a name="setting-up-hello-frontend-environment"></a><span data-ttu-id="3f9cf-163">Konfigurera hello frontend-miljö</span><span class="sxs-lookup"><span data-stu-id="3f9cf-163">Setting up hello frontend environment</span></span>

<span data-ttu-id="3f9cf-164">Upprepa hello steg för[skapa en funktionsapp](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function#create-a-function-app) toocreate en ny funktionsapp i som du skapar din proxyserver.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-164">Repeat hello steps too[Create a function app](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function#create-a-function-app) toocreate a new function app in which you will create your proxy.</span></span> <span data-ttu-id="3f9cf-165">Den här nya appen fungerar som klientdel hello för vårt API och hello funktionsapp du redigerade tidigare fungerar som en serverdel.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-165">This new app will serve as hello frontend for our API, and hello function app you were previously editing will serve as a backend.</span></span>

<span data-ttu-id="3f9cf-166">Navigera tooyour ny klientdel funktionsapp hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-166">Navigate tooyour new frontend function app in hello portal.</span></span>

<span data-ttu-id="3f9cf-167">Välj **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-167">Select **Settings**.</span></span> <span data-ttu-id="3f9cf-168">Sedan växla **aktivera Azure Functions proxyservrar (förhandsgranskning)** för ”On”.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-168">Then toggle **Enable Azure Functions Proxies (preview)** too"On".</span></span>

<span data-ttu-id="3f9cf-169">Välj **plattform inställningar** och välj **programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-169">Select **Platform Settings** and choose **Application Settings**.</span></span>

<span data-ttu-id="3f9cf-170">Rulla nedåt för**appinställningar** och skapa en ny inställning med nyckeln ”HELLO_HOST”.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-170">Scroll down too**App settings** and create a new setting with key "HELLO_HOST".</span></span> <span data-ttu-id="3f9cf-171">Ange värdet toohello värden för din app för backend-funktion, till exempel `<YourApp>.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-171">Set its value toohello host of your backend function app, such as `<YourApp>.azurewebsites.net`.</span></span> <span data-ttu-id="3f9cf-172">Detta är en del av hello-URL som du kopierade tidigare när du testar din HTTP-funktion.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-172">This is part of hello URL that you copied earlier when testing your HTTP function.</span></span> <span data-ttu-id="3f9cf-173">Du måste referera till den här inställningen i konfigurationen för hello senare.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-173">You'll reference this setting in hello configuration later.</span></span>

> [!NOTE] 
> <span data-ttu-id="3f9cf-174">App-inställningar rekommenderas för hello värden configuration tooprevent ett hårdkodat miljö beroende för hello-proxy.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-174">App settings are recommended for hello host configuration tooprevent a hard-coded environment dependency for hello proxy.</span></span> <span data-ttu-id="3f9cf-175">Med hjälp av app-inställningar innebär att du kan flytta hello proxykonfiguration mellan miljöer och hälsning miljöspecifikt appinställningar tillämpas.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-175">Using app settings means that you can move hello proxy configuration between environments, and hello environment-specific app settings will be applied.</span></span>

<span data-ttu-id="3f9cf-176">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-176">Click **Save**.</span></span>

### <a name="creating-a-proxy-on-hello-frontend"></a><span data-ttu-id="3f9cf-177">Att skapa en proxy på hello klientdel</span><span class="sxs-lookup"><span data-stu-id="3f9cf-177">Creating a proxy on hello frontend</span></span>

<span data-ttu-id="3f9cf-178">Gå tillbaka tooyour klientdel funktionsapp hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-178">Navigate back tooyour frontend function app in hello portal.</span></span>

<span data-ttu-id="3f9cf-179">I hello vänstra navigeringsfönstret klickar du på hello plustecknet '+' Nästa för ”proxyservrar (förhandsgranskning)”.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-179">In hello left-hand navigation, click hello plus sign '+' next too"Proxies (preview)".</span></span>

![Att skapa en proxy](./media/functions-create-serverless-api/creating-proxy.png)

<span data-ttu-id="3f9cf-181">Använd proxy-inställningar som anges i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-181">Use proxy settings as specified in hello table.</span></span>

| <span data-ttu-id="3f9cf-182">Fält</span><span class="sxs-lookup"><span data-stu-id="3f9cf-182">Field</span></span> | <span data-ttu-id="3f9cf-183">Exempelvärde</span><span class="sxs-lookup"><span data-stu-id="3f9cf-183">Sample value</span></span> | <span data-ttu-id="3f9cf-184">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3f9cf-184">Description</span></span> |
|---|---|---|
| <span data-ttu-id="3f9cf-185">Namn</span><span class="sxs-lookup"><span data-stu-id="3f9cf-185">Name</span></span> | <span data-ttu-id="3f9cf-186">HelloProxy</span><span class="sxs-lookup"><span data-stu-id="3f9cf-186">HelloProxy</span></span> | <span data-ttu-id="3f9cf-187">Ett eget namn som används endast för hantering</span><span class="sxs-lookup"><span data-stu-id="3f9cf-187">A friendly name used only for management</span></span> |
| <span data-ttu-id="3f9cf-188">Flödesmallen</span><span class="sxs-lookup"><span data-stu-id="3f9cf-188">Route template</span></span> | <span data-ttu-id="3f9cf-189">/ api/hello</span><span class="sxs-lookup"><span data-stu-id="3f9cf-189">/api/hello</span></span> | <span data-ttu-id="3f9cf-190">Anger vilka vägen används tooinvoke denna proxy</span><span class="sxs-lookup"><span data-stu-id="3f9cf-190">Determines what route is used tooinvoke this proxy</span></span> |
| <span data-ttu-id="3f9cf-191">Backend-URL</span><span class="sxs-lookup"><span data-stu-id="3f9cf-191">Backend URL</span></span> | <span data-ttu-id="3f9cf-192">https://%HELLO_HOST%/API/hello</span><span class="sxs-lookup"><span data-stu-id="3f9cf-192">https://%HELLO_HOST%/api/hello</span></span> | <span data-ttu-id="3f9cf-193">Anger hello endpoint toowhich hello begäran bör vara via proxy</span><span class="sxs-lookup"><span data-stu-id="3f9cf-193">Specifies hello endpoint toowhich hello request should be proxied</span></span> |

<span data-ttu-id="3f9cf-194">Observera att proxyservrar inte innehåller hello `/api` bassökväg prefix och måste tas med i hello flödesmallen.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-194">Note that Proxies does not provide hello `/api` base path prefix, and this must be included in hello route template.</span></span>

<span data-ttu-id="3f9cf-195">Hej `%HELLO_HOST%` syntax refererar hello appinställning du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-195">hello `%HELLO_HOST%` syntax will reference hello app setting you created earlier.</span></span> <span data-ttu-id="3f9cf-196">hello matcha URL: en pekar tooyour ursprungliga funktion.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-196">hello resolved URL will point tooyour original function.</span></span>

<span data-ttu-id="3f9cf-197">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-197">Click **Create**.</span></span>

<span data-ttu-id="3f9cf-198">Du kan testa den nya proxyserverkonfigurationen genom att kopiera hello Proxy-URL och testa det i hello webbläsare eller med din favorit HTTP-klienten.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-198">You can try out your new proxy by copying hello Proxy URL and testing it in hello browser or with your favorite HTTP client.</span></span>

## <a name="create-a-mock-api"></a><span data-ttu-id="3f9cf-199">Skapa en fingerad API</span><span class="sxs-lookup"><span data-stu-id="3f9cf-199">Create a mock API</span></span>

<span data-ttu-id="3f9cf-200">Sedan använder du en proxy-toocreate en fingerad API för din lösning.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-200">Next, you will use a proxy toocreate a mock API for your solution.</span></span> <span data-ttu-id="3f9cf-201">Detta gör att klienten development tooprogress utan att behöva hello backend fullt ut.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-201">This allows client development tooprogress, without needing hello backend fully implemented.</span></span> <span data-ttu-id="3f9cf-202">Senare i utveckling, kan du skapa en ny funktionsapp som stöder denna logik och omdirigera proxy-tooit.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-202">Later in development, you could create a new function app which supports this logic and redirect your proxy tooit.</span></span>

<span data-ttu-id="3f9cf-203">toocreate detta mock API, skapas en ny proxy med hjälp hello [App Service Editor](https://github.com/projectkudu/kudu/wiki/App-Service-Editor).</span><span class="sxs-lookup"><span data-stu-id="3f9cf-203">toocreate this mock API, we will create a new proxy, this time using hello [App Service Editor](https://github.com/projectkudu/kudu/wiki/App-Service-Editor).</span></span> <span data-ttu-id="3f9cf-204">tooget igång, navigera tooyour funktionsapp hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-204">tooget started, navigate tooyour function app in hello portal.</span></span> <span data-ttu-id="3f9cf-205">Välj **plattformsfunktioner** och hitta **App Service Editor**.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-205">Select **Platform features** and find **App Service Editor**.</span></span> <span data-ttu-id="3f9cf-206">Klicka på det här öppnas hello App Service Editor i en ny flik.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-206">Clicking this will open hello App Service Editor in a new tab.</span></span>

<span data-ttu-id="3f9cf-207">Välj `proxies.json` i hello vänstra navigeringsrutan.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-207">Select `proxies.json` in hello left navigation.</span></span> <span data-ttu-id="3f9cf-208">Detta är hello-fil som lagrar hello konfiguration för alla dina proxyservrar.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-208">This is hello file which stores hello configuration for all of your proxies.</span></span> <span data-ttu-id="3f9cf-209">Om du använder en hello [fungerar distributionsmetoder](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment), detta är hello-fil som du ska behålla i källkontroll.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-209">If you use one of hello [Functions deployment methods](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment), this is hello file you will maintain in source control.</span></span> <span data-ttu-id="3f9cf-210">toolearn mer om den här filen finns [proxyservrar avancerad konfiguration](https://docs.microsoft.com/azure/azure-functions/functions-proxies#advanced-configuration).</span><span class="sxs-lookup"><span data-stu-id="3f9cf-210">toolearn more about this file, see [Proxies advanced configuration](https://docs.microsoft.com/azure/azure-functions/functions-proxies#advanced-configuration).</span></span>

<span data-ttu-id="3f9cf-211">Om du har följt hittills, bör din proxies.json se ut hello följande:</span><span class="sxs-lookup"><span data-stu-id="3f9cf-211">If you've followed along so far, your proxies.json should look like hello following:</span></span>

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "HelloProxy": {
            "matchCondition": {
                "route": "/api/hello"
            },
            "backendUri": "https://%HELLO_HOST%/api/hello"
        }
    }
}
```

<span data-ttu-id="3f9cf-212">Härnäst ska du lägga till din fingerad API.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-212">Next you'll add your mock API.</span></span> <span data-ttu-id="3f9cf-213">Ersätt filen proxies.json med hello följande:</span><span class="sxs-lookup"><span data-stu-id="3f9cf-213">Replace your proxies.json file with hello following:</span></span>

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "HelloProxy": {
            "matchCondition": {
                "route": "/api/hello"
            },
            "backendUri": "https://%HELLO_HOST%/api/hello"
        },
        "GetUserByName" : {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/users/{username}"
            },
            "responseOverrides": {
                "response.statusCode": "200",
                "response.headers.Content-Type" : "application/json",
                "response.body": {
                    "name": "{username}",
                    "description": "Awesome developer and master of serverless APIs",
                    "skills": [
                        "Serverless",
                        "APIs",
                        "Azure",
                        "Cloud"
                    ]
                }
            }
        }
    }
}
```

<span data-ttu-id="3f9cf-214">Detta lägger till en ny proxy, ”GetUserByName” utan hello backendUri egenskap.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-214">This adds a new proxy, "GetUserByName", without hello backendUri property.</span></span> <span data-ttu-id="3f9cf-215">Funktionen ändrar hello Standardsvar från proxyservrar med en åsidosättning för svar i stället för att anropa en annan resurs.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-215">Instead of calling another resource, it modifies hello default response from Proxies using a response override.</span></span> <span data-ttu-id="3f9cf-216">Förfrågan och svar åsidosättningar kan också användas tillsammans med en backend-URL.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-216">Request and response overrides can also be used in conjunction with a backend URL.</span></span> <span data-ttu-id="3f9cf-217">Detta är särskilt användbart när proxyanslutning tooa äldre system, där du kanske behöver toomodify huvuden frågeparametrar, etc. toolearn mer om begäran och svar åsidosättningar, se [ändra begäranden och -svar i proxyservrar](https://docs.microsoft.com/azure/azure-functions/functions-proxies#a-namemodify-requests-responsesamodifying-requests-and-responses).</span><span class="sxs-lookup"><span data-stu-id="3f9cf-217">This is particularly useful when proxying tooa legacy system, where you might need toomodify headers, query parameters, etc. toolearn more about request and response overrides, see [Modifying requests and responses in Proxies](https://docs.microsoft.com/azure/azure-functions/functions-proxies#a-namemodify-requests-responsesamodifying-requests-and-responses).</span></span>

<span data-ttu-id="3f9cf-218">Testa din fingerad API genom att anropa hello `/api/users/{username}` slutpunkten med hjälp av en webbläsare eller ditt favoritprogram REST-klient.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-218">Test your mock API by calling hello `/api/users/{username}` endpoint using a browser or your favorite REST client.</span></span> <span data-ttu-id="3f9cf-219">Vara säker på att tooreplace _{username}_ med ett strängvärde som representerar ett användarnamn.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-219">Be sure tooreplace _{username}_ with a string value representing a username.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3f9cf-220">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3f9cf-220">Next steps</span></span>

<span data-ttu-id="3f9cf-221">I kursen får du lärt dig hur toobuild och anpassa en API på Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-221">In this tutorial, you learned how toobuild and customize an API on Azure Functions.</span></span> <span data-ttu-id="3f9cf-222">Du också lära dig hur toobring flera API: er, inklusive mocks, samtidigt som en enhetlig API-ytan.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-222">You also learned how toobring multiple APIs, including mocks, together as a unified API surface.</span></span> <span data-ttu-id="3f9cf-223">Du kan använda dessa tekniker toobuild ut API: er för alla komplexitet, alla när du kör på hello serverlösa compute modellen som tillhandahålls av Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="3f9cf-223">You can use these techniques toobuild out APIs of any complexity, all while running on hello serverless compute model provided by Azure Functions.</span></span>

<span data-ttu-id="3f9cf-224">hello kan följande referenser vara till hjälp när du utvecklar dina API ytterligare:</span><span class="sxs-lookup"><span data-stu-id="3f9cf-224">hello following references may be helpful as you develop your API further:</span></span>

- [<span data-ttu-id="3f9cf-225">Azure Functions HTTP och webhook bindningar</span><span class="sxs-lookup"><span data-stu-id="3f9cf-225">Azure Functions HTTP and webhook bindings</span></span>](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook)
- <span data-ttu-id="3f9cf-226">[arbeta med Azure Functions-proxyservrar (förhandsgranskning)]</span><span class="sxs-lookup"><span data-stu-id="3f9cf-226">[Working with Azure Functions Proxies (preview)]</span></span>
- [<span data-ttu-id="3f9cf-227">Dokumentera Azure Functions API (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="3f9cf-227">Documenting an Azure Functions API (preview)</span></span>](https://docs.microsoft.com/azure/azure-functions/functions-api-definition-getting-started)


[Create your first function]: https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function
[arbeta med Azure Functions-proxyservrar (förhandsgranskning)]: https://docs.microsoft.com/azure/azure-functions/functions-proxies
