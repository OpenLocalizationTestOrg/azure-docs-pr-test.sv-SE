---
title: aaaWork med proxyservrar i Azure Functions | Microsoft Docs
description: "Översikt över hur toouse Azure Functions proxyservrar"
services: functions
documentationcenter: 
author: mattchenderson
manager: erikre
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/11/2017
ms.author: mahender
ms.openlocfilehash: 4d94c89e8f8f2d2c771b01bae142bf9a4f3b7f2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="work-with-azure-functions-proxies-preview"></a><span data-ttu-id="f77ad-103">Arbeta med Azure Functions-proxyservrar (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="f77ad-103">Work with Azure Functions Proxies (preview)</span></span>

> [!NOTE] 
> <span data-ttu-id="f77ad-104">Azure Functions proxyservrar är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="f77ad-104">Azure Functions Proxies is currently in preview.</span></span> <span data-ttu-id="f77ad-105">Det är gratis när i förhandsgranskningen men standardfunktioner fakturering tillämpar tooproxy körningar.</span><span class="sxs-lookup"><span data-stu-id="f77ad-105">It is free while in preview, but standard Functions billing applies tooproxy executions.</span></span> <span data-ttu-id="f77ad-106">Mer information finns i [priser för Azure Functions](https://azure.microsoft.com/pricing/details/functions/).</span><span class="sxs-lookup"><span data-stu-id="f77ad-106">For more information, see [Azure Functions pricing](https://azure.microsoft.com/pricing/details/functions/).</span></span>

<span data-ttu-id="f77ad-107">Den här artikeln förklarar hur tooconfigure och arbeta med Azure Functions proxyservrar.</span><span class="sxs-lookup"><span data-stu-id="f77ad-107">This article explains how tooconfigure and work with Azure Functions Proxies.</span></span> <span data-ttu-id="f77ad-108">Med den här funktionen kan du ange slutpunkter på appen funktion som implementeras av en annan resurs.</span><span class="sxs-lookup"><span data-stu-id="f77ad-108">With this feature, you can specify endpoints on your function app that are implemented by another resource.</span></span> <span data-ttu-id="f77ad-109">Du kan använda dessa proxyservrar toobreak stora API i flera funktionen appar (som en mikrotjänster arkitektur) och ändå kunna erbjuda en enda API-yta för klienter.</span><span class="sxs-lookup"><span data-stu-id="f77ad-109">You can use these proxies toobreak a large API into multiple function apps (as in a microservice architecture), while still presenting a single API surface for clients.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]


## <span data-ttu-id="f77ad-110"><a name="enable"></a>Aktivera Azure Functions proxyservrar</span><span class="sxs-lookup"><span data-stu-id="f77ad-110"><a name="enable"></a>Enable Azure Functions Proxies</span></span>

<span data-ttu-id="f77ad-111">Proxy aktiveras inte som standard.</span><span class="sxs-lookup"><span data-stu-id="f77ad-111">Proxies are not enabled by default.</span></span> <span data-ttu-id="f77ad-112">Du kan skapa proxyservrar när hello-funktionen är inaktiverad, men de kommer inte att köra.</span><span class="sxs-lookup"><span data-stu-id="f77ad-112">You can create proxies while hello feature is disabled, but they will not execute.</span></span> <span data-ttu-id="f77ad-113">tooenable proxyservrar hello följande:</span><span class="sxs-lookup"><span data-stu-id="f77ad-113">tooenable proxies, do hello following:</span></span>

1. <span data-ttu-id="f77ad-114">Öppna hello [Azure-portalen], och sedan gå tooyour funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="f77ad-114">Open hello [Azure portal], and then go tooyour function app.</span></span>
2. <span data-ttu-id="f77ad-115">Välj **fungerar appinställningar**.</span><span class="sxs-lookup"><span data-stu-id="f77ad-115">Select **Function app settings**.</span></span>
3. <span data-ttu-id="f77ad-116">Växeln **aktivera Azure Functions proxyservrar (förhandsgranskning)** för**på**.</span><span class="sxs-lookup"><span data-stu-id="f77ad-116">Switch **Enable Azure Functions Proxies (preview)** too**On**.</span></span>

<span data-ttu-id="f77ad-117">Du kan också returnera här tooupdate hello proxy runtime när nya funktioner blir tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="f77ad-117">You can also return here tooupdate hello proxy runtime as new features become available.</span></span>


## <span data-ttu-id="f77ad-118"><a name="create"></a>Skapa en proxy</span><span class="sxs-lookup"><span data-stu-id="f77ad-118"><a name="create"></a>Create a proxy</span></span>

<span data-ttu-id="f77ad-119">Det här avsnittet visar hur toocreate en proxy i hello Functions-portalen.</span><span class="sxs-lookup"><span data-stu-id="f77ad-119">This section shows you how toocreate a proxy in hello Functions portal.</span></span>

1. <span data-ttu-id="f77ad-120">Öppna hello [Azure-portalen], och sedan gå tooyour funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="f77ad-120">Open hello [Azure portal], and then go tooyour function app.</span></span>
2. <span data-ttu-id="f77ad-121">I hello vänster och välj **ny proxy**.</span><span class="sxs-lookup"><span data-stu-id="f77ad-121">In hello left pane, select **New proxy**.</span></span>
3. <span data-ttu-id="f77ad-122">Ange ett namn för proxyservern.</span><span class="sxs-lookup"><span data-stu-id="f77ad-122">Provide a name for your proxy.</span></span>
4. <span data-ttu-id="f77ad-123">Konfigurera hello-slutpunkt som är exponerad på den här funktionen appen genom att ange hello **flödesmallen** och **HTTP-metoderna**.</span><span class="sxs-lookup"><span data-stu-id="f77ad-123">Configure hello endpoint that's exposed on this function app by specifying hello **route template** and **HTTP methods**.</span></span> <span data-ttu-id="f77ad-124">Dessa parametrar fungerar bl.a toohello regler för [http-utlösare].</span><span class="sxs-lookup"><span data-stu-id="f77ad-124">These parameters behave according toohello rules for [HTTP triggers].</span></span>
5. <span data-ttu-id="f77ad-125">Ange hello **backend-URL** tooanother slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="f77ad-125">Set hello **backend URL** tooanother endpoint.</span></span> <span data-ttu-id="f77ad-126">Den här slutpunkten kan vara en funktion i en annan funktionsapp eller kan det bero på andra API: er.</span><span class="sxs-lookup"><span data-stu-id="f77ad-126">This endpoint could be a function in another function app, or it could be any other API.</span></span> <span data-ttu-id="f77ad-127">hello värdet behöver inte toobe statiskt och det kan referera till [programinställningar] och [parametrar från hello ursprungliga klientbegäran].</span><span class="sxs-lookup"><span data-stu-id="f77ad-127">hello value does not need toobe static, and it can reference [application settings] and [parameters from hello original client request].</span></span>
6. <span data-ttu-id="f77ad-128">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f77ad-128">Click **Create**.</span></span>

<span data-ttu-id="f77ad-129">Proxyservern finns nu som en ny slutpunkt i appen funktion.</span><span class="sxs-lookup"><span data-stu-id="f77ad-129">Your proxy now exists as a new endpoint on your function app.</span></span> <span data-ttu-id="f77ad-130">Ur klienten är det motsvarande tooan HttpTrigger i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="f77ad-130">From a client perspective, it is equivalent tooan HttpTrigger in Azure Functions.</span></span> <span data-ttu-id="f77ad-131">Du kan testa den nya proxyserverkonfigurationen genom att kopiera hello Proxy-URL och testa med din favorit HTTP-klient.</span><span class="sxs-lookup"><span data-stu-id="f77ad-131">You can try out your new proxy by copying hello Proxy URL and testing it with your favorite HTTP client.</span></span>

## <span data-ttu-id="f77ad-132"><a name="modify-requests-responses"></a>Ändra begäranden och -svar</span><span class="sxs-lookup"><span data-stu-id="f77ad-132"><a name="modify-requests-responses"></a>Modify requests and responses</span></span>

<span data-ttu-id="f77ad-133">Med Azure Functions-proxyservrar, kan du ändra begäranden tooand svar från hello serverdel.</span><span class="sxs-lookup"><span data-stu-id="f77ad-133">With Azure Functions Proxies, you can modify requests tooand responses from hello back end.</span></span> <span data-ttu-id="f77ad-134">Dessa omvandlingar kan använda variabler som definieras i [använda variabler].</span><span class="sxs-lookup"><span data-stu-id="f77ad-134">These transformations can use variables as defined in [Use variables].</span></span>

### <span data-ttu-id="f77ad-135"><a name="modify-backend-request"></a>Ändra hello backend-begäran</span><span class="sxs-lookup"><span data-stu-id="f77ad-135"><a name="modify-backend-request"></a>Modify hello back-end request</span></span>

<span data-ttu-id="f77ad-136">Som standard initieras hello backend-begäran som en kopia av hello ursprungliga begäran.</span><span class="sxs-lookup"><span data-stu-id="f77ad-136">By default, hello back-end request is initialized as a copy of hello original request.</span></span> <span data-ttu-id="f77ad-137">Dessutom toosetting hello backend-URL, du kan göra ändringar toohello HTTP-metod, rubriker och fråga string-parametrar.</span><span class="sxs-lookup"><span data-stu-id="f77ad-137">In addition toosetting hello back-end URL, you can make changes toohello HTTP method, headers, and query string parameters.</span></span> <span data-ttu-id="f77ad-138">hello ändrade värden kan referera till [programinställningar] och [parametrar från hello ursprungliga klientbegäran].</span><span class="sxs-lookup"><span data-stu-id="f77ad-138">hello modified values can reference [application settings] and [parameters from hello original client request].</span></span>

<span data-ttu-id="f77ad-139">Det finns för närvarande inga portaler för att ändra backend-begäranden.</span><span class="sxs-lookup"><span data-stu-id="f77ad-139">Currently, there is no portal experience for modifying back-end requests.</span></span> <span data-ttu-id="f77ad-140">toolearn hur tooapply funktionen från proxies.json, se [definiera ett requestOverrides objekt].</span><span class="sxs-lookup"><span data-stu-id="f77ad-140">toolearn how tooapply this capability from proxies.json, see [Define a requestOverrides object].</span></span>

### <span data-ttu-id="f77ad-141"><a name="modify-response"></a>Ändra hello svar</span><span class="sxs-lookup"><span data-stu-id="f77ad-141"><a name="modify-response"></a>Modify hello response</span></span>

<span data-ttu-id="f77ad-142">Som standard initieras hello Klientsvar som en kopia av hello backend-svar.</span><span class="sxs-lookup"><span data-stu-id="f77ad-142">By default, hello client response is initialized as a copy of hello back-end response.</span></span> <span data-ttu-id="f77ad-143">Du kan göra ändringar toohello Svarets statuskod, orsaksfrasen, rubriker och brödtext.</span><span class="sxs-lookup"><span data-stu-id="f77ad-143">You can make changes toohello response's status code, reason phrase, headers, and body.</span></span> <span data-ttu-id="f77ad-144">hello ändrade värden kan referera till [programinställningar], [parametrar från hello ursprungliga klientbegäran], och [parametrar från hello backend-svar].</span><span class="sxs-lookup"><span data-stu-id="f77ad-144">hello modified values can reference [application settings], [parameters from hello original client request], and [parameters from hello back-end response].</span></span>

<span data-ttu-id="f77ad-145">Det finns för närvarande inga portaler för att ändra svar.</span><span class="sxs-lookup"><span data-stu-id="f77ad-145">Currently, there is no portal experience for modifying responses.</span></span> <span data-ttu-id="f77ad-146">toolearn hur tooapply funktionen från proxies.json, se [definiera ett responseOverrides objekt].</span><span class="sxs-lookup"><span data-stu-id="f77ad-146">toolearn how tooapply this capability from proxies.json, see [Define a responseOverrides object].</span></span>

## <span data-ttu-id="f77ad-147"><a name="using-variables"></a>Använda variabler</span><span class="sxs-lookup"><span data-stu-id="f77ad-147"><a name="using-variables"></a>Use variables</span></span>

<span data-ttu-id="f77ad-148">hello-konfiguration för en proxy behöver inte toobe statisk.</span><span class="sxs-lookup"><span data-stu-id="f77ad-148">hello configuration for a proxy does not need toobe static.</span></span> <span data-ttu-id="f77ad-149">Du kan villkor den toouse variabler från hello ursprungliga begäran, hello backend-svar eller programinställningar.</span><span class="sxs-lookup"><span data-stu-id="f77ad-149">You can condition it toouse variables from hello original request, hello back-end response, or application settings.</span></span>

### <span data-ttu-id="f77ad-150"><a name="request-parameters"></a>Parametrarna som referens</span><span class="sxs-lookup"><span data-stu-id="f77ad-150"><a name="request-parameters"></a>Reference request parameters</span></span>

<span data-ttu-id="f77ad-151">Du kan använda parametrarna som anger toohello backend-URL: en egenskap eller som en del av ändra begäranden och -svar.</span><span class="sxs-lookup"><span data-stu-id="f77ad-151">You can use request parameters as inputs toohello back-end URL property or as part of modifying requests and responses.</span></span> <span data-ttu-id="f77ad-152">Vissa parametrar kan bindas från hello flödesmallen som anges i grundläggande hello-proxykonfigurationen och andra kan komma från egenskaperna för hello inkommande begäran.</span><span class="sxs-lookup"><span data-stu-id="f77ad-152">Some parameters can be bound from hello route template that's specified in hello base proxy configuration, and others can come from properties of hello incoming request.</span></span>

#### <a name="route-template-parameters"></a><span data-ttu-id="f77ad-153">Vidarebefordra mallparametrar</span><span class="sxs-lookup"><span data-stu-id="f77ad-153">Route template parameters</span></span>
<span data-ttu-id="f77ad-154">Parametrar som används i hello flödesmallen är tillgängliga toobe som refereras av namn.</span><span class="sxs-lookup"><span data-stu-id="f77ad-154">Parameters that are used in hello route template are available toobe referenced by name.</span></span> <span data-ttu-id="f77ad-155">hello parameternamn omges av klammerparenteser ({}).</span><span class="sxs-lookup"><span data-stu-id="f77ad-155">hello parameter names are enclosed in braces ({}).</span></span>

<span data-ttu-id="f77ad-156">Om exempelvis en proxy som har en flödesmallen `/pets/{petId}`, hello backend-URL kan innehålla hello värdet för `{petId}`, som i `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}`.</span><span class="sxs-lookup"><span data-stu-id="f77ad-156">For example, if a proxy has a route template, such as `/pets/{petId}`, hello back-end URL can include hello value of `{petId}`, as in `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}`.</span></span> <span data-ttu-id="f77ad-157">Om hello flödesmallen avslutas med ett jokertecken som `/api/{*restOfPath}`, hello värdet `{restOfPath}` är en strängrepresentation av hello återstående sökvägssegment från hello inkommande begäran.</span><span class="sxs-lookup"><span data-stu-id="f77ad-157">If hello route template terminates in a wildcard, such as `/api/{*restOfPath}`, hello value `{restOfPath}` is a string representation of hello remaining path segments from hello incoming request.</span></span>

#### <a name="additional-request-parameters"></a><span data-ttu-id="f77ad-158">Parametrar för ytterligare begäran</span><span class="sxs-lookup"><span data-stu-id="f77ad-158">Additional request parameters</span></span>
<span data-ttu-id="f77ad-159">Dessutom toohello vidarebefordra mallparametrar, hello följande värden kan användas i konfigurationsvärden:</span><span class="sxs-lookup"><span data-stu-id="f77ad-159">In addition toohello route template parameters, hello following values can be used in config values:</span></span>

* <span data-ttu-id="f77ad-160">**{request.method} **: hello HTTP-metod som används på hello ursprungliga begäran.</span><span class="sxs-lookup"><span data-stu-id="f77ad-160">**{request.method}**: hello HTTP method that's used on hello original request.</span></span>
* <span data-ttu-id="f77ad-161">**{request.headers. \<HeaderName\>}**: ett huvud som kan läsas från hello ursprungliga begäran.</span><span class="sxs-lookup"><span data-stu-id="f77ad-161">**{request.headers.\<HeaderName\>}**: A header that can be read from hello original request.</span></span> <span data-ttu-id="f77ad-162">Ersätt * \<HeaderName\> * med hello namnet på hello-huvud som du vill tooread.</span><span class="sxs-lookup"><span data-stu-id="f77ad-162">Replace *\<HeaderName\>* with hello name of hello header that you want tooread.</span></span> <span data-ttu-id="f77ad-163">Om hello-huvud inte ingår i hello begäran, blir hello värdet hello tom sträng.</span><span class="sxs-lookup"><span data-stu-id="f77ad-163">If hello header is not included on hello request, hello value will be hello empty string.</span></span>
* <span data-ttu-id="f77ad-164">**{request.querystring. \<ParameterName\>}**: en frågesträngsparameter som kan läsas från hello ursprungliga begäran.</span><span class="sxs-lookup"><span data-stu-id="f77ad-164">**{request.querystring.\<ParameterName\>}**: A query string parameter that can be read from hello original request.</span></span> <span data-ttu-id="f77ad-165">Ersätt * \<ParameterName\> * med hello namnet på hello-parameter som du vill tooread.</span><span class="sxs-lookup"><span data-stu-id="f77ad-165">Replace *\<ParameterName\>* with hello name of hello parameter that you want tooread.</span></span> <span data-ttu-id="f77ad-166">Om hello-parametern inte finns på hello begäran, blir hello värdet hello tom sträng.</span><span class="sxs-lookup"><span data-stu-id="f77ad-166">If hello parameter is not included on hello request, hello value will be hello empty string.</span></span>

### <span data-ttu-id="f77ad-167"><a name="response-parameters"></a>Referens för backend-svarsparametrar</span><span class="sxs-lookup"><span data-stu-id="f77ad-167"><a name="response-parameters"></a>Reference back-end response parameters</span></span>

<span data-ttu-id="f77ad-168">Svarsparametrar kan användas som en del av ändra hello svar toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="f77ad-168">Response parameters can be used as part of modifying hello response toohello client.</span></span> <span data-ttu-id="f77ad-169">hello följande värden kan användas i konfigurationsvärden:</span><span class="sxs-lookup"><span data-stu-id="f77ad-169">hello following values can be used in config values:</span></span>

* <span data-ttu-id="f77ad-170">**{backend.response.statusCode} **: hello HTTP-statuskod som returneras hello backend-svaret.</span><span class="sxs-lookup"><span data-stu-id="f77ad-170">**{backend.response.statusCode}**: hello HTTP status code that's returned on hello back-end response.</span></span>
* <span data-ttu-id="f77ad-171">**{backend.response.statusReason} **: hello HTTP orsaksfrasen som returneras hello backend-svaret.</span><span class="sxs-lookup"><span data-stu-id="f77ad-171">**{backend.response.statusReason}**: hello HTTP reason phrase that's returned on hello back-end response.</span></span>
* <span data-ttu-id="f77ad-172">**{backend.response.headers. \<HeaderName\>}**: ett huvud som kan läsas från hello backend-svar.</span><span class="sxs-lookup"><span data-stu-id="f77ad-172">**{backend.response.headers.\<HeaderName\>}**: A header that can be read from hello back-end response.</span></span> <span data-ttu-id="f77ad-173">Ersätt * \<HeaderName\> * hello namnet på hello-huvud ska tooread.</span><span class="sxs-lookup"><span data-stu-id="f77ad-173">Replace *\<HeaderName\>* with hello name of hello header you want tooread.</span></span> <span data-ttu-id="f77ad-174">Om hello-huvud inte ingår i hello begäran, blir hello värdet hello tom sträng.</span><span class="sxs-lookup"><span data-stu-id="f77ad-174">If hello header is not included on hello request, hello value will be hello empty string.</span></span>

### <span data-ttu-id="f77ad-175"><a name="use-appsettings"></a>Referens för programinställningar</span><span class="sxs-lookup"><span data-stu-id="f77ad-175"><a name="use-appsettings"></a>Reference application settings</span></span>

<span data-ttu-id="f77ad-176">Du kan också referera [programinställningar som definierats för hello funktionsapp](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#develop) genom att omge hello inställningsnamn med procenttecken (%).</span><span class="sxs-lookup"><span data-stu-id="f77ad-176">You can also reference [application settings defined for hello function app](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#develop) by surrounding hello setting name with percent signs (%).</span></span>

<span data-ttu-id="f77ad-177">Till exempel backend-URL: en *https://%ORDER_PROCESSING_HOST%/api/orders* skulle ha ”% ORDER_PROCESSING_HOST %” ersätts med hello hello ORDER_PROCESSING_HOST inställningens värde.</span><span class="sxs-lookup"><span data-stu-id="f77ad-177">For example, a back-end URL of *https://%ORDER_PROCESSING_HOST%/api/orders* would have "%ORDER_PROCESSING_HOST%" replaced with hello value of hello ORDER_PROCESSING_HOST setting.</span></span>

> [!TIP] 
> <span data-ttu-id="f77ad-178">Använd programinställningar för backend-värdar när du har flera distributioner eller testmiljöer.</span><span class="sxs-lookup"><span data-stu-id="f77ad-178">Use application settings for back-end hosts when you have multiple deployments or test environments.</span></span> <span data-ttu-id="f77ad-179">På så sätt kan du se till att du alltid pratar toohello tillbaka slut för den miljön.</span><span class="sxs-lookup"><span data-stu-id="f77ad-179">That way, you can make sure that you are always talking toohello right back end for that environment.</span></span>

## <a name="advanced-configuration"></a><span data-ttu-id="f77ad-180">Avancerad konfiguration</span><span class="sxs-lookup"><span data-stu-id="f77ad-180">Advanced configuration</span></span>

<span data-ttu-id="f77ad-181">hello-proxyservrar som du konfigurerar lagras i en proxies.json-fil som finns i hello roten för en funktion programkatalogen.</span><span class="sxs-lookup"><span data-stu-id="f77ad-181">hello proxies that you configure are stored in a proxies.json file, which is located in hello root of a function app directory.</span></span> <span data-ttu-id="f77ad-182">Du kan manuellt redigera den här filen och distribuera det som en del av din app när du använder någon av hello [distributionsmetoder](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) som stöder funktioner.</span><span class="sxs-lookup"><span data-stu-id="f77ad-182">You can manually edit this file and deploy it as part of your app when you use any of hello [deployment methods](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) that Functions supports.</span></span> <span data-ttu-id="f77ad-183">hello-funktionen måste vara [aktiverat](#enable) för hello filen toobe bearbetas.</span><span class="sxs-lookup"><span data-stu-id="f77ad-183">hello feature must be [enabled](#enable) for hello file toobe processed.</span></span> 

> [!TIP] 
> <span data-ttu-id="f77ad-184">Om du inte har angett något hello distributionsmetoder, kan du också fungera med hello proxies.json filen i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="f77ad-184">If you have not set up one of hello deployment methods, you can also work with hello proxies.json file in hello portal.</span></span> <span data-ttu-id="f77ad-185">Gå tooyour funktionsapp, Välj **plattformsfunktioner**, och välj sedan **App Service Editor**.</span><span class="sxs-lookup"><span data-stu-id="f77ad-185">Go tooyour function app, select **Platform features**, and then select **App Service Editor**.</span></span> <span data-ttu-id="f77ad-186">På så sätt, kan du visa hello hela filen strukturen för din funktionsapp och göra ändringar.</span><span class="sxs-lookup"><span data-stu-id="f77ad-186">By doing so, you can view hello entire file structure of your function app and then make changes.</span></span>

<span data-ttu-id="f77ad-187">Proxies.JSON definieras av ett proxyservrar-objekt som består av namngivna proxyservrar och deras definitioner.</span><span class="sxs-lookup"><span data-stu-id="f77ad-187">Proxies.json is defined by a proxies object, which is composed of named proxies and their definitions.</span></span> <span data-ttu-id="f77ad-188">Du kan också om redigeringsprogram stöder detta, kan du referera en [JSON-schema](http://json.schemastore.org/proxies) för kod slutförande.</span><span class="sxs-lookup"><span data-stu-id="f77ad-188">Optionally, if your editor supports it, you can reference a [JSON schema](http://json.schemastore.org/proxies) for code completion.</span></span> <span data-ttu-id="f77ad-189">En exempelfil kan se ut hello följande:</span><span class="sxs-lookup"><span data-stu-id="f77ad-189">An example file might look like hello following:</span></span>

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>"
        }
    }
}
```

<span data-ttu-id="f77ad-190">Varje proxy har ett eget namn som *proxy1* i föregående exempel hello.</span><span class="sxs-lookup"><span data-stu-id="f77ad-190">Each proxy has a friendly name, such as *proxy1* in hello preceding example.</span></span> <span data-ttu-id="f77ad-191">hello motsvarande definition proxyobjekt definieras av hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="f77ad-191">hello corresponding proxy definition object is defined by hello following properties:</span></span>

* <span data-ttu-id="f77ad-192">**matchCondition**: krävs – ett objekt som definierar hello-begäranden som utlöser hello körningen av denna proxy.</span><span class="sxs-lookup"><span data-stu-id="f77ad-192">**matchCondition**: Required--an object defining hello requests that trigger hello execution of this proxy.</span></span> <span data-ttu-id="f77ad-193">Den innehåller två egenskaper som delas med [http-utlösare]:</span><span class="sxs-lookup"><span data-stu-id="f77ad-193">It contains two properties that are shared with [HTTP triggers]:</span></span>
    * <span data-ttu-id="f77ad-194">_metoder_: en matris med hello HTTP-metoder som hello proxy svarar.</span><span class="sxs-lookup"><span data-stu-id="f77ad-194">_methods_: An array of hello HTTP methods that hello proxy responds to.</span></span> <span data-ttu-id="f77ad-195">Om den inte är angiven svarar hello proxy tooall HTTP-metoderna för hello flödet.</span><span class="sxs-lookup"><span data-stu-id="f77ad-195">If it is not specified, hello proxy responds tooall HTTP methods on hello route.</span></span>
    * <span data-ttu-id="f77ad-196">_väg_: krävs--definierar hello flödesmallen styra som begära webbadresserna din proxyserver svarar på.</span><span class="sxs-lookup"><span data-stu-id="f77ad-196">_route_: Required--defines hello route template, controlling which request URLs your proxy responds to.</span></span> <span data-ttu-id="f77ad-197">Till skillnad från i HTTP-utlösare har inget standardvärde.</span><span class="sxs-lookup"><span data-stu-id="f77ad-197">Unlike in HTTP triggers, there is no default value.</span></span>
* <span data-ttu-id="f77ad-198">**backendUri**: hello Webbadress för hello backend-resurs toowhich hello begäran måste vara via proxy.</span><span class="sxs-lookup"><span data-stu-id="f77ad-198">**backendUri**: hello URL of hello back-end resource toowhich hello request should be proxied.</span></span> <span data-ttu-id="f77ad-199">Det här värdet kan referera till programinställningar och parametrar från hello ursprungliga klientbegäran.</span><span class="sxs-lookup"><span data-stu-id="f77ad-199">This value can reference application settings and parameters from hello original client request.</span></span> <span data-ttu-id="f77ad-200">Om den här egenskapen inte är inkluderad svarar Azure Functions med en HTTP-200 OK.</span><span class="sxs-lookup"><span data-stu-id="f77ad-200">If this property is not included, Azure Functions responds with an HTTP 200 OK.</span></span>
* <span data-ttu-id="f77ad-201">**requestOverrides**: ett objekt som definierar transformationer toohello backend-begäran.</span><span class="sxs-lookup"><span data-stu-id="f77ad-201">**requestOverrides**: An object that defines transformations toohello back-end request.</span></span> <span data-ttu-id="f77ad-202">Se [definiera ett requestOverrides objekt].</span><span class="sxs-lookup"><span data-stu-id="f77ad-202">See [Define a requestOverrides object].</span></span>
* <span data-ttu-id="f77ad-203">**responseOverrides**: ett objekt som definierar transformationer toohello klientsvar.</span><span class="sxs-lookup"><span data-stu-id="f77ad-203">**responseOverrides**: An object that defines transformations toohello client response.</span></span> <span data-ttu-id="f77ad-204">Se [definiera ett responseOverrides objekt].</span><span class="sxs-lookup"><span data-stu-id="f77ad-204">See [Define a responseOverrides object].</span></span>

> [!NOTE] 
> <span data-ttu-id="f77ad-205">hello flödet egenskapen Azure Functions proxyservrar inte att behandla hello routePrefix-egenskapen för Värdkonfiguration för hello funktioner.</span><span class="sxs-lookup"><span data-stu-id="f77ad-205">hello route property Azure Functions Proxies does not honor hello routePrefix property of hello Functions host configuration.</span></span> <span data-ttu-id="f77ad-206">Om du vill tooinclude ett prefix, till exempel /api, måste det ingå i hello flödet egenskap.</span><span class="sxs-lookup"><span data-stu-id="f77ad-206">If you want tooinclude a prefix such as /api, it must be included in hello route property.</span></span>

### <span data-ttu-id="f77ad-207"><a name="requestOverrides"></a>Definiera ett requestOverrides-objekt</span><span class="sxs-lookup"><span data-stu-id="f77ad-207"><a name="requestOverrides"></a>Define a requestOverrides object</span></span>

<span data-ttu-id="f77ad-208">Hej requestOverrides objektet definierar ändringar som gjorts toohello begäran när hello backend-resursen anropas.</span><span class="sxs-lookup"><span data-stu-id="f77ad-208">hello requestOverrides object defines changes made toohello request when hello back-end resource is called.</span></span> <span data-ttu-id="f77ad-209">hello objekt definieras av hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="f77ad-209">hello object is defined by hello following properties:</span></span>

* <span data-ttu-id="f77ad-210">**backend.Request.Method**: hello HTTP-metod som har använt toocall hello-serverdel.</span><span class="sxs-lookup"><span data-stu-id="f77ad-210">**backend.request.method**: hello HTTP method that's used toocall hello back end.</span></span>
* <span data-ttu-id="f77ad-211">**backend.Request.QueryString. \<ParameterName\>**: en frågesträngsparameter som kan anges för anrop hello toohello serverdel.</span><span class="sxs-lookup"><span data-stu-id="f77ad-211">**backend.request.querystring.\<ParameterName\>**: A query string parameter that can be set for hello call toohello back end.</span></span> <span data-ttu-id="f77ad-212">Ersätt * \<ParameterName\> * med hello namnet på hello-parameter som du vill tooset.</span><span class="sxs-lookup"><span data-stu-id="f77ad-212">Replace *\<ParameterName\>* with hello name of hello parameter that you want tooset.</span></span> <span data-ttu-id="f77ad-213">Om hello tom sträng ingår inte hello parametern på hello backend-begäran.</span><span class="sxs-lookup"><span data-stu-id="f77ad-213">If hello empty string is provided, hello parameter is not included on hello back-end request.</span></span>
* <span data-ttu-id="f77ad-214">**backend.Request.headers. \<HeaderName\>**: ett huvud som kan anges för anrop hello toohello serverdel.</span><span class="sxs-lookup"><span data-stu-id="f77ad-214">**backend.request.headers.\<HeaderName\>**: A header that can be set for hello call toohello back end.</span></span> <span data-ttu-id="f77ad-215">Ersätt * \<HeaderName\> * med hello namnet på hello-huvud som du vill tooset.</span><span class="sxs-lookup"><span data-stu-id="f77ad-215">Replace *\<HeaderName\>* with hello name of hello header that you want tooset.</span></span> <span data-ttu-id="f77ad-216">Om du tillhandahåller hello tom sträng ingår inte hello sidhuvud på hello backend-begäran.</span><span class="sxs-lookup"><span data-stu-id="f77ad-216">If you provide hello empty string, hello header is not included on hello back-end request.</span></span>

<span data-ttu-id="f77ad-217">Värden kan referera till programinställningar och parametrar från hello ursprungliga klientbegäran.</span><span class="sxs-lookup"><span data-stu-id="f77ad-217">Values can reference application settings and parameters from hello original client request.</span></span>

<span data-ttu-id="f77ad-218">En exempelkonfiguration kan se ut hello följande:</span><span class="sxs-lookup"><span data-stu-id="f77ad-218">An example configuration might look like hello following:</span></span>

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>",
            "requestOverrides": {
                "backend.request.headers.Accept": "application/xml",
                "backend.request.headers.x-functions-key": "%ANOTHERAPP_API_KEY%"
            }
        }
    }
}
```

### <span data-ttu-id="f77ad-219"><a name="responseOverrides"></a>Definiera ett responseOverrides-objekt</span><span class="sxs-lookup"><span data-stu-id="f77ad-219"><a name="responseOverrides"></a>Define a responseOverrides object</span></span>

<span data-ttu-id="f77ad-220">Hej requestOverrides objektet definierar ändringar som görs toohello svar som har gått tillbaka toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="f77ad-220">hello requestOverrides object defines changes that are made toohello response that's passed back toohello client.</span></span> <span data-ttu-id="f77ad-221">hello objekt definieras av hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="f77ad-221">hello object is defined by hello following properties:</span></span>

* <span data-ttu-id="f77ad-222">**response.statusCode**: hello HTTP-status kod toobe returnerade toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="f77ad-222">**response.statusCode**: hello HTTP status code toobe returned toohello client.</span></span>
* <span data-ttu-id="f77ad-223">**response.statusReason**: hello HTTP orsak frasen toobe returnerade toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="f77ad-223">**response.statusReason**: hello HTTP reason phrase toobe returned toohello client.</span></span>
* <span data-ttu-id="f77ad-224">**Response.body**: hello strängrepresentation av hello brödtext toobe returnerade toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="f77ad-224">**response.body**: hello string representation of hello body toobe returned toohello client.</span></span>
* <span data-ttu-id="f77ad-225">**Response.headers. \<HeaderName\>**: ett huvud som kan anges för hello svar toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="f77ad-225">**response.headers.\<HeaderName\>**: A header that can be set for hello response toohello client.</span></span> <span data-ttu-id="f77ad-226">Ersätt * \<HeaderName\> * med hello namnet på hello-huvud som du vill tooset.</span><span class="sxs-lookup"><span data-stu-id="f77ad-226">Replace *\<HeaderName\>* with hello name of hello header that you want tooset.</span></span> <span data-ttu-id="f77ad-227">Om du tillhandahåller hello tom sträng ingår inte hello sidhuvud hello svaret.</span><span class="sxs-lookup"><span data-stu-id="f77ad-227">If you provide hello empty string, hello header is not included on hello response.</span></span>

<span data-ttu-id="f77ad-228">Värden kan referera till programinställningar, parametrar från hello ursprungliga klientbegäran och parametrar från hello backend-svar.</span><span class="sxs-lookup"><span data-stu-id="f77ad-228">Values can reference application settings, parameters from hello original client request, and parameters from hello back-end response.</span></span>

<span data-ttu-id="f77ad-229">En exempelkonfiguration kan se ut hello följande:</span><span class="sxs-lookup"><span data-stu-id="f77ad-229">An example configuration might look like hello following:</span></span>

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "responseOverrides": {
                "response.body": "Hello, {test}",
                "response.headers.Content-Type": "text/plain"
            }
        }
    }
}
```
> [!NOTE] 
> <span data-ttu-id="f77ad-230">I det här exemplet hello brödtext anges direkt, så ingen `backendUri` egenskapen krävs.</span><span class="sxs-lookup"><span data-stu-id="f77ad-230">In this example, hello body is being set directly, so no `backendUri` property is needed.</span></span> <span data-ttu-id="f77ad-231">hello exempel visas hur du kan använda Azure Functions proxyservrar för mocking API: er.</span><span class="sxs-lookup"><span data-stu-id="f77ad-231">hello example shows how you might use Azure Functions Proxies for mocking APIs.</span></span>

[Azure Portal]: https://portal.azure.com
[HTTP-utlösare]: https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#http-trigger
[Modify hello back-end request]: #modify-backend-request
[Modify hello response]: #modify-response
[Definiera ett requestOverrides-objekt]: #requestOverrides
[Definiera ett responseOverrides-objekt]: #responseOverrides
[programinställningar]: #use-appsettings
[Använda variabler]: #using-variables
[parametrar från hello ursprungliga klientbegäran]: #request-parameters
[parametrar från hello backend-svar]: #response-parameters
