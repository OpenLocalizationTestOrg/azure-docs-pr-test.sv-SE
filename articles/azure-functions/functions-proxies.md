---
title: Arbeta med proxyservrar i Azure Functions | Microsoft Docs
description: "Översikt över hur du använder Azure Functions proxyservrar"
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
ms.openlocfilehash: 102e54627a8fee721d3ed85e86a8009e706bb5b1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="work-with-azure-functions-proxies-preview"></a><span data-ttu-id="95eaa-103">Arbeta med Azure Functions-proxyservrar (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="95eaa-103">Work with Azure Functions Proxies (preview)</span></span>

> [!NOTE] 
> <span data-ttu-id="95eaa-104">Azure Functions proxyservrar är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="95eaa-104">Azure Functions Proxies is currently in preview.</span></span> <span data-ttu-id="95eaa-105">Det är gratis medan i förhandsgranskningen men standardfunktioner fakturering gäller proxy körningar.</span><span class="sxs-lookup"><span data-stu-id="95eaa-105">It is free while in preview, but standard Functions billing applies to proxy executions.</span></span> <span data-ttu-id="95eaa-106">Mer information finns i [priser för Azure Functions](https://azure.microsoft.com/pricing/details/functions/).</span><span class="sxs-lookup"><span data-stu-id="95eaa-106">For more information, see [Azure Functions pricing](https://azure.microsoft.com/pricing/details/functions/).</span></span>

<span data-ttu-id="95eaa-107">Den här artikeln beskriver hur du konfigurerar och arbetar med Azure Functions proxyservrar.</span><span class="sxs-lookup"><span data-stu-id="95eaa-107">This article explains how to configure and work with Azure Functions Proxies.</span></span> <span data-ttu-id="95eaa-108">Med den här funktionen kan du ange slutpunkter på appen funktion som implementeras av en annan resurs.</span><span class="sxs-lookup"><span data-stu-id="95eaa-108">With this feature, you can specify endpoints on your function app that are implemented by another resource.</span></span> <span data-ttu-id="95eaa-109">Du kan använda dessa proxyservrar för att dela en stor API i flera funktionen appar (som en mikrotjänster arkitektur) och ändå kunna erbjuda en enda API-yta för klienter.</span><span class="sxs-lookup"><span data-stu-id="95eaa-109">You can use these proxies to break a large API into multiple function apps (as in a microservice architecture), while still presenting a single API surface for clients.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]


## <span data-ttu-id="95eaa-110"><a name="enable"></a>Aktivera Azure Functions proxyservrar</span><span class="sxs-lookup"><span data-stu-id="95eaa-110"><a name="enable"></a>Enable Azure Functions Proxies</span></span>

<span data-ttu-id="95eaa-111">Proxy aktiveras inte som standard.</span><span class="sxs-lookup"><span data-stu-id="95eaa-111">Proxies are not enabled by default.</span></span> <span data-ttu-id="95eaa-112">Du kan skapa proxyservrar när funktionen är inaktiverad, men de kommer inte att köra.</span><span class="sxs-lookup"><span data-stu-id="95eaa-112">You can create proxies while the feature is disabled, but they will not execute.</span></span> <span data-ttu-id="95eaa-113">Om du vill aktivera proxyservrar, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="95eaa-113">To enable proxies, do the following:</span></span>

1. <span data-ttu-id="95eaa-114">Öppna den [Azure-portalen], och gå sedan till din funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="95eaa-114">Open the [Azure portal], and then go to your function app.</span></span>
2. <span data-ttu-id="95eaa-115">Välj **fungerar appinställningar**.</span><span class="sxs-lookup"><span data-stu-id="95eaa-115">Select **Function app settings**.</span></span>
3. <span data-ttu-id="95eaa-116">Växeln **aktivera Azure Functions proxyservrar (förhandsgranskning)** till **på**.</span><span class="sxs-lookup"><span data-stu-id="95eaa-116">Switch **Enable Azure Functions Proxies (preview)** to **On**.</span></span>

<span data-ttu-id="95eaa-117">Du kan också returnera här om du vill uppdatera proxy runtime när nya funktioner blir tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="95eaa-117">You can also return here to update the proxy runtime as new features become available.</span></span>


## <span data-ttu-id="95eaa-118"><a name="create"></a>Skapa en proxy</span><span class="sxs-lookup"><span data-stu-id="95eaa-118"><a name="create"></a>Create a proxy</span></span>

<span data-ttu-id="95eaa-119">Det här avsnittet visar hur du skapar en proxy i Functions-portalen.</span><span class="sxs-lookup"><span data-stu-id="95eaa-119">This section shows you how to create a proxy in the Functions portal.</span></span>

1. <span data-ttu-id="95eaa-120">Öppna den [Azure-portalen], och gå sedan till din funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="95eaa-120">Open the [Azure portal], and then go to your function app.</span></span>
2. <span data-ttu-id="95eaa-121">I den vänstra rutan, Välj **ny proxy**.</span><span class="sxs-lookup"><span data-stu-id="95eaa-121">In the left pane, select **New proxy**.</span></span>
3. <span data-ttu-id="95eaa-122">Ange ett namn för proxyservern.</span><span class="sxs-lookup"><span data-stu-id="95eaa-122">Provide a name for your proxy.</span></span>
4. <span data-ttu-id="95eaa-123">Konfigurera den slutpunkt som är exponerad på den här funktionen appen genom att ange den **flödesmallen** och **HTTP-metoderna**.</span><span class="sxs-lookup"><span data-stu-id="95eaa-123">Configure the endpoint that's exposed on this function app by specifying the **route template** and **HTTP methods**.</span></span> <span data-ttu-id="95eaa-124">Dessa parametrar fungerar enligt reglerna för [http-utlösare].</span><span class="sxs-lookup"><span data-stu-id="95eaa-124">These parameters behave according to the rules for [HTTP triggers].</span></span>
5. <span data-ttu-id="95eaa-125">Ange den **backend-URL** till en annan slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="95eaa-125">Set the **backend URL** to another endpoint.</span></span> <span data-ttu-id="95eaa-126">Den här slutpunkten kan vara en funktion i en annan funktionsapp eller kan det bero på andra API: er.</span><span class="sxs-lookup"><span data-stu-id="95eaa-126">This endpoint could be a function in another function app, or it could be any other API.</span></span> <span data-ttu-id="95eaa-127">Värdet behöver inte vara statiska och kan referera till [programinställningar] och [parametrar från den ursprungliga klientbegäran].</span><span class="sxs-lookup"><span data-stu-id="95eaa-127">The value does not need to be static, and it can reference [application settings] and [parameters from the original client request].</span></span>
6. <span data-ttu-id="95eaa-128">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="95eaa-128">Click **Create**.</span></span>

<span data-ttu-id="95eaa-129">Proxyservern finns nu som en ny slutpunkt i appen funktion.</span><span class="sxs-lookup"><span data-stu-id="95eaa-129">Your proxy now exists as a new endpoint on your function app.</span></span> <span data-ttu-id="95eaa-130">Ur klienten motsvarar den en HttpTrigger i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="95eaa-130">From a client perspective, it is equivalent to an HttpTrigger in Azure Functions.</span></span> <span data-ttu-id="95eaa-131">Du kan testa den nya proxyserverkonfigurationen genom att kopiera URL: en för Proxy och testa med din favorit HTTP-klient.</span><span class="sxs-lookup"><span data-stu-id="95eaa-131">You can try out your new proxy by copying the Proxy URL and testing it with your favorite HTTP client.</span></span>

## <span data-ttu-id="95eaa-132"><a name="modify-requests-responses"></a>Ändra begäranden och -svar</span><span class="sxs-lookup"><span data-stu-id="95eaa-132"><a name="modify-requests-responses"></a>Modify requests and responses</span></span>

<span data-ttu-id="95eaa-133">Med Azure Functions-proxyservrar, kan du ändra till och -svar från serverdelen.</span><span class="sxs-lookup"><span data-stu-id="95eaa-133">With Azure Functions Proxies, you can modify requests to and responses from the back end.</span></span> <span data-ttu-id="95eaa-134">Dessa omvandlingar kan använda variabler som definieras i [använda variabler].</span><span class="sxs-lookup"><span data-stu-id="95eaa-134">These transformations can use variables as defined in [Use variables].</span></span>

### <span data-ttu-id="95eaa-135"><a name="modify-backend-request"></a>Ändra backend-begäran</span><span class="sxs-lookup"><span data-stu-id="95eaa-135"><a name="modify-backend-request"></a>Modify the back-end request</span></span>

<span data-ttu-id="95eaa-136">Som standard initieras backend-begäran som en kopia av den ursprungliga begäranden.</span><span class="sxs-lookup"><span data-stu-id="95eaa-136">By default, the back-end request is initialized as a copy of the original request.</span></span> <span data-ttu-id="95eaa-137">Utöver att ställa in backend-URL: en kan du ändra till HTTP-metoden, rubriker och fråga string-parametrar.</span><span class="sxs-lookup"><span data-stu-id="95eaa-137">In addition to setting the back-end URL, you can make changes to the HTTP method, headers, and query string parameters.</span></span> <span data-ttu-id="95eaa-138">Ändrade värden kan referera [programinställningar] och [parametrar från den ursprungliga klientbegäran].</span><span class="sxs-lookup"><span data-stu-id="95eaa-138">The modified values can reference [application settings] and [parameters from the original client request].</span></span>

<span data-ttu-id="95eaa-139">Det finns för närvarande inga portaler för att ändra backend-begäranden.</span><span class="sxs-lookup"><span data-stu-id="95eaa-139">Currently, there is no portal experience for modifying back-end requests.</span></span> <span data-ttu-id="95eaa-140">Information om hur du använder den här funktionen från proxies.json finns [definiera ett requestOverrides objekt].</span><span class="sxs-lookup"><span data-stu-id="95eaa-140">To learn how to apply this capability from proxies.json, see [Define a requestOverrides object].</span></span>

### <span data-ttu-id="95eaa-141"><a name="modify-response"></a>Ändra svaret</span><span class="sxs-lookup"><span data-stu-id="95eaa-141"><a name="modify-response"></a>Modify the response</span></span>

<span data-ttu-id="95eaa-142">Som standard initieras klienten svaret som en kopia av backend-svaret.</span><span class="sxs-lookup"><span data-stu-id="95eaa-142">By default, the client response is initialized as a copy of the back-end response.</span></span> <span data-ttu-id="95eaa-143">Du kan ändra svaret statuskod, orsaksfrasen, rubriker och brödtext.</span><span class="sxs-lookup"><span data-stu-id="95eaa-143">You can make changes to the response's status code, reason phrase, headers, and body.</span></span> <span data-ttu-id="95eaa-144">Ändrade värden kan referera [programinställningar], [parametrar från den ursprungliga klientbegäran], och [parametrar från backend-svaret].</span><span class="sxs-lookup"><span data-stu-id="95eaa-144">The modified values can reference [application settings], [parameters from the original client request], and [parameters from the back-end response].</span></span>

<span data-ttu-id="95eaa-145">Det finns för närvarande inga portaler för att ändra svar.</span><span class="sxs-lookup"><span data-stu-id="95eaa-145">Currently, there is no portal experience for modifying responses.</span></span> <span data-ttu-id="95eaa-146">Information om hur du använder den här funktionen från proxies.json finns [definiera ett responseOverrides objekt].</span><span class="sxs-lookup"><span data-stu-id="95eaa-146">To learn how to apply this capability from proxies.json, see [Define a responseOverrides object].</span></span>

## <span data-ttu-id="95eaa-147"><a name="using-variables"></a>Använda variabler</span><span class="sxs-lookup"><span data-stu-id="95eaa-147"><a name="using-variables"></a>Use variables</span></span>

<span data-ttu-id="95eaa-148">Konfigurationen för en proxy behöver inte vara statisk.</span><span class="sxs-lookup"><span data-stu-id="95eaa-148">The configuration for a proxy does not need to be static.</span></span> <span data-ttu-id="95eaa-149">Du kan villkor och använda variabler från den ursprungliga begäranden, backend-svar eller programinställningar.</span><span class="sxs-lookup"><span data-stu-id="95eaa-149">You can condition it to use variables from the original request, the back-end response, or application settings.</span></span>

### <span data-ttu-id="95eaa-150"><a name="request-parameters"></a>Parametrarna som referens</span><span class="sxs-lookup"><span data-stu-id="95eaa-150"><a name="request-parameters"></a>Reference request parameters</span></span>

<span data-ttu-id="95eaa-151">Du kan använda parametrarna som indata till backend-URL: en egenskap eller som en del av att ändra begäranden och -svar.</span><span class="sxs-lookup"><span data-stu-id="95eaa-151">You can use request parameters as inputs to the back-end URL property or as part of modifying requests and responses.</span></span> <span data-ttu-id="95eaa-152">Vissa parametrar kan bindas från flödesmallen som anges i grundläggande proxykonfigurationen och andra kan komma från egenskaperna för den inkommande begäranden.</span><span class="sxs-lookup"><span data-stu-id="95eaa-152">Some parameters can be bound from the route template that's specified in the base proxy configuration, and others can come from properties of the incoming request.</span></span>

#### <a name="route-template-parameters"></a><span data-ttu-id="95eaa-153">Vidarebefordra mallparametrar</span><span class="sxs-lookup"><span data-stu-id="95eaa-153">Route template parameters</span></span>
<span data-ttu-id="95eaa-154">Parametrar som används i flödesmallen kan refereras till av namn.</span><span class="sxs-lookup"><span data-stu-id="95eaa-154">Parameters that are used in the route template are available to be referenced by name.</span></span> <span data-ttu-id="95eaa-155">Parameternamn omges av klammerparenteser ({}).</span><span class="sxs-lookup"><span data-stu-id="95eaa-155">The parameter names are enclosed in braces ({}).</span></span>

<span data-ttu-id="95eaa-156">Om exempelvis en proxy som har en flödesmallen `/pets/{petId}`, URL: en för backend-kan innehålla värdet för `{petId}`, som i `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}`.</span><span class="sxs-lookup"><span data-stu-id="95eaa-156">For example, if a proxy has a route template, such as `/pets/{petId}`, the back-end URL can include the value of `{petId}`, as in `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}`.</span></span> <span data-ttu-id="95eaa-157">Om flödesmallen avslutas med ett jokertecken, till exempel `/api/{*restOfPath}`, värdet `{restOfPath}` är en strängrepresentation av återstående sökvägssegment från den inkommande begäranden.</span><span class="sxs-lookup"><span data-stu-id="95eaa-157">If the route template terminates in a wildcard, such as `/api/{*restOfPath}`, the value `{restOfPath}` is a string representation of the remaining path segments from the incoming request.</span></span>

#### <a name="additional-request-parameters"></a><span data-ttu-id="95eaa-158">Parametrar för ytterligare begäran</span><span class="sxs-lookup"><span data-stu-id="95eaa-158">Additional request parameters</span></span>
<span data-ttu-id="95eaa-159">Följande värden kan användas i konfigurationsvärden förutom mallparametrar väg:</span><span class="sxs-lookup"><span data-stu-id="95eaa-159">In addition to the route template parameters, the following values can be used in config values:</span></span>

* <span data-ttu-id="95eaa-160">**{request.method}** : HTTP-metod som används på den ursprungliga begäranden.</span><span class="sxs-lookup"><span data-stu-id="95eaa-160">**{request.method}**: The HTTP method that's used on the original request.</span></span>
* <span data-ttu-id="95eaa-161">**{request.headers. \<HeaderName\>}**: ett huvud som kan läsas från den ursprungliga begäranden.</span><span class="sxs-lookup"><span data-stu-id="95eaa-161">**{request.headers.\<HeaderName\>}**: A header that can be read from the original request.</span></span> <span data-ttu-id="95eaa-162">Ersätt  *\<HeaderName\>*  med namnet på det huvud som du vill läsa.</span><span class="sxs-lookup"><span data-stu-id="95eaa-162">Replace *\<HeaderName\>* with the name of the header that you want to read.</span></span> <span data-ttu-id="95eaa-163">Om rubriken inte finns på begäran, ska värdet vara en tom sträng.</span><span class="sxs-lookup"><span data-stu-id="95eaa-163">If the header is not included on the request, the value will be the empty string.</span></span>
* <span data-ttu-id="95eaa-164">**{request.querystring. \<ParameterName\>}**: en frågesträngsparameter som kan läsas från den ursprungliga begäranden.</span><span class="sxs-lookup"><span data-stu-id="95eaa-164">**{request.querystring.\<ParameterName\>}**: A query string parameter that can be read from the original request.</span></span> <span data-ttu-id="95eaa-165">Ersätt  *\<ParameterName\>*  med namnet på den parameter som du vill läsa.</span><span class="sxs-lookup"><span data-stu-id="95eaa-165">Replace *\<ParameterName\>* with the name of the parameter that you want to read.</span></span> <span data-ttu-id="95eaa-166">Om parametern inte finns på begäran, ska värdet vara en tom sträng.</span><span class="sxs-lookup"><span data-stu-id="95eaa-166">If the parameter is not included on the request, the value will be the empty string.</span></span>

### <span data-ttu-id="95eaa-167"><a name="response-parameters"></a>Referens för backend-svarsparametrar</span><span class="sxs-lookup"><span data-stu-id="95eaa-167"><a name="response-parameters"></a>Reference back-end response parameters</span></span>

<span data-ttu-id="95eaa-168">Svarsparametrar kan användas som en del av ändra svar till klienten.</span><span class="sxs-lookup"><span data-stu-id="95eaa-168">Response parameters can be used as part of modifying the response to the client.</span></span> <span data-ttu-id="95eaa-169">Följande värden kan användas i konfigurationsvärden:</span><span class="sxs-lookup"><span data-stu-id="95eaa-169">The following values can be used in config values:</span></span>

* <span data-ttu-id="95eaa-170">**{backend.response.statusCode}** : HTTP-statuskod som returneras av backend-svaret.</span><span class="sxs-lookup"><span data-stu-id="95eaa-170">**{backend.response.statusCode}**: The HTTP status code that's returned on the back-end response.</span></span>
* <span data-ttu-id="95eaa-171">**{backend.response.statusReason}** : HTTP-orsaksfrasen som returneras av backend-svaret.</span><span class="sxs-lookup"><span data-stu-id="95eaa-171">**{backend.response.statusReason}**: The HTTP reason phrase that's returned on the back-end response.</span></span>
* <span data-ttu-id="95eaa-172">**{backend.response.headers. \<HeaderName\>}**: ett huvud som kan läsas från backend-svaret.</span><span class="sxs-lookup"><span data-stu-id="95eaa-172">**{backend.response.headers.\<HeaderName\>}**: A header that can be read from the back-end response.</span></span> <span data-ttu-id="95eaa-173">Ersätt  *\<HeaderName\>*  med namnet på det huvud som du vill läsa.</span><span class="sxs-lookup"><span data-stu-id="95eaa-173">Replace *\<HeaderName\>* with the name of the header you want to read.</span></span> <span data-ttu-id="95eaa-174">Om rubriken inte finns på begäran, ska värdet vara en tom sträng.</span><span class="sxs-lookup"><span data-stu-id="95eaa-174">If the header is not included on the request, the value will be the empty string.</span></span>

### <span data-ttu-id="95eaa-175"><a name="use-appsettings"></a>Referens för programinställningar</span><span class="sxs-lookup"><span data-stu-id="95eaa-175"><a name="use-appsettings"></a>Reference application settings</span></span>

<span data-ttu-id="95eaa-176">Du kan också referera [programinställningar som definierats för funktion appen](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#develop) genom att omge inställningsnamn med procenttecken (%).</span><span class="sxs-lookup"><span data-stu-id="95eaa-176">You can also reference [application settings defined for the function app](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#develop) by surrounding the setting name with percent signs (%).</span></span>

<span data-ttu-id="95eaa-177">Till exempel backend-URL: en *https://%ORDER_PROCESSING_HOST%/api/orders* skulle ha ”% ORDER_PROCESSING_HOST %” ersätts med värdet för inställningen ORDER_PROCESSING_HOST.</span><span class="sxs-lookup"><span data-stu-id="95eaa-177">For example, a back-end URL of *https://%ORDER_PROCESSING_HOST%/api/orders* would have "%ORDER_PROCESSING_HOST%" replaced with the value of the ORDER_PROCESSING_HOST setting.</span></span>

> [!TIP] 
> <span data-ttu-id="95eaa-178">Använd programinställningar för backend-värdar när du har flera distributioner eller testmiljöer.</span><span class="sxs-lookup"><span data-stu-id="95eaa-178">Use application settings for back-end hosts when you have multiple deployments or test environments.</span></span> <span data-ttu-id="95eaa-179">På så sätt kan kan du se till att du alltid talar till direkt till serverdelen för den miljön.</span><span class="sxs-lookup"><span data-stu-id="95eaa-179">That way, you can make sure that you are always talking to the right back end for that environment.</span></span>

## <a name="advanced-configuration"></a><span data-ttu-id="95eaa-180">Avancerad konfiguration</span><span class="sxs-lookup"><span data-stu-id="95eaa-180">Advanced configuration</span></span>

<span data-ttu-id="95eaa-181">Proxyservrar som du konfigurerar lagras i en proxies.json-fil som finns i roten av en funktion programkatalogen.</span><span class="sxs-lookup"><span data-stu-id="95eaa-181">The proxies that you configure are stored in a proxies.json file, which is located in the root of a function app directory.</span></span> <span data-ttu-id="95eaa-182">Du kan manuellt redigera den här filen och distribuera det som en del av din app när du använder någon av de [distributionsmetoder](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) som stöder funktioner.</span><span class="sxs-lookup"><span data-stu-id="95eaa-182">You can manually edit this file and deploy it as part of your app when you use any of the [deployment methods](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) that Functions supports.</span></span> <span data-ttu-id="95eaa-183">Funktionen måste vara [aktiverat](#enable) för filen som ska bearbetas.</span><span class="sxs-lookup"><span data-stu-id="95eaa-183">The feature must be [enabled](#enable) for the file to be processed.</span></span> 

> [!TIP] 
> <span data-ttu-id="95eaa-184">Om du inte har angett något av metoder för distribution, kan du också fungera med filen proxies.json i portalen.</span><span class="sxs-lookup"><span data-stu-id="95eaa-184">If you have not set up one of the deployment methods, you can also work with the proxies.json file in the portal.</span></span> <span data-ttu-id="95eaa-185">Gå till funktionen appen, Välj **plattformsfunktioner**, och välj sedan **App Service Editor**.</span><span class="sxs-lookup"><span data-stu-id="95eaa-185">Go to your function app, select **Platform features**, and then select **App Service Editor**.</span></span> <span data-ttu-id="95eaa-186">På så sätt, kan du visa hela filen strukturen för din funktionsapp och göra ändringar.</span><span class="sxs-lookup"><span data-stu-id="95eaa-186">By doing so, you can view the entire file structure of your function app and then make changes.</span></span>

<span data-ttu-id="95eaa-187">Proxies.JSON definieras av ett proxyservrar-objekt som består av namngivna proxyservrar och deras definitioner.</span><span class="sxs-lookup"><span data-stu-id="95eaa-187">Proxies.json is defined by a proxies object, which is composed of named proxies and their definitions.</span></span> <span data-ttu-id="95eaa-188">Du kan också om redigeringsprogram stöder detta, kan du referera en [JSON-schema](http://json.schemastore.org/proxies) för kod slutförande.</span><span class="sxs-lookup"><span data-stu-id="95eaa-188">Optionally, if your editor supports it, you can reference a [JSON schema](http://json.schemastore.org/proxies) for code completion.</span></span> <span data-ttu-id="95eaa-189">En exempelfil kan se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="95eaa-189">An example file might look like the following:</span></span>

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

<span data-ttu-id="95eaa-190">Varje proxy har ett eget namn som *proxy1* i föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="95eaa-190">Each proxy has a friendly name, such as *proxy1* in the preceding example.</span></span> <span data-ttu-id="95eaa-191">Objektet för definition av motsvarande proxy definieras av följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="95eaa-191">The corresponding proxy definition object is defined by the following properties:</span></span>

* <span data-ttu-id="95eaa-192">**matchCondition**: krävs – ett objekt som definierar de begäranden som utlösa körning av denna proxy.</span><span class="sxs-lookup"><span data-stu-id="95eaa-192">**matchCondition**: Required--an object defining the requests that trigger the execution of this proxy.</span></span> <span data-ttu-id="95eaa-193">Den innehåller två egenskaper som delas med [http-utlösare]:</span><span class="sxs-lookup"><span data-stu-id="95eaa-193">It contains two properties that are shared with [HTTP triggers]:</span></span>
    * <span data-ttu-id="95eaa-194">_metoder_: en matris med proxyservern svarar på HTTP-metoderna.</span><span class="sxs-lookup"><span data-stu-id="95eaa-194">_methods_: An array of the HTTP methods that the proxy responds to.</span></span> <span data-ttu-id="95eaa-195">Om den inte är angiven svarar proxyn alla HTTP-metoder på vägen.</span><span class="sxs-lookup"><span data-stu-id="95eaa-195">If it is not specified, the proxy responds to all HTTP methods on the route.</span></span>
    * <span data-ttu-id="95eaa-196">_väg_: krävs--definierar flödesmallen styra som begära webbadresserna din proxyserver svarar på.</span><span class="sxs-lookup"><span data-stu-id="95eaa-196">_route_: Required--defines the route template, controlling which request URLs your proxy responds to.</span></span> <span data-ttu-id="95eaa-197">Till skillnad från i HTTP-utlösare har inget standardvärde.</span><span class="sxs-lookup"><span data-stu-id="95eaa-197">Unlike in HTTP triggers, there is no default value.</span></span>
* <span data-ttu-id="95eaa-198">**backendUri**: URL: en för backend-resursen som begäran ska vara via proxy.</span><span class="sxs-lookup"><span data-stu-id="95eaa-198">**backendUri**: The URL of the back-end resource to which the request should be proxied.</span></span> <span data-ttu-id="95eaa-199">Det här värdet kan referera till programinställningar och parametrar från den ursprungliga klientbegäran.</span><span class="sxs-lookup"><span data-stu-id="95eaa-199">This value can reference application settings and parameters from the original client request.</span></span> <span data-ttu-id="95eaa-200">Om den här egenskapen inte är inkluderad svarar Azure Functions med en HTTP-200 OK.</span><span class="sxs-lookup"><span data-stu-id="95eaa-200">If this property is not included, Azure Functions responds with an HTTP 200 OK.</span></span>
* <span data-ttu-id="95eaa-201">**requestOverrides**: ett objekt som definierar omformningar på backend-begäran.</span><span class="sxs-lookup"><span data-stu-id="95eaa-201">**requestOverrides**: An object that defines transformations to the back-end request.</span></span> <span data-ttu-id="95eaa-202">Se [definiera ett requestOverrides objekt].</span><span class="sxs-lookup"><span data-stu-id="95eaa-202">See [Define a requestOverrides object].</span></span>
* <span data-ttu-id="95eaa-203">**responseOverrides**: ett objekt som definierar omvandlingar för klient-svaret.</span><span class="sxs-lookup"><span data-stu-id="95eaa-203">**responseOverrides**: An object that defines transformations to the client response.</span></span> <span data-ttu-id="95eaa-204">Se [definiera ett responseOverrides objekt].</span><span class="sxs-lookup"><span data-stu-id="95eaa-204">See [Define a responseOverrides object].</span></span>

> [!NOTE] 
> <span data-ttu-id="95eaa-205">Egenskapen väg Azure Functions proxyservrar inte att behandla egenskapen routePrefix för Värdkonfiguration funktioner.</span><span class="sxs-lookup"><span data-stu-id="95eaa-205">The route property Azure Functions Proxies does not honor the routePrefix property of the Functions host configuration.</span></span> <span data-ttu-id="95eaa-206">Om du vill inkludera ett prefix, till exempel /api måste tas med i egenskapen vägen.</span><span class="sxs-lookup"><span data-stu-id="95eaa-206">If you want to include a prefix such as /api, it must be included in the route property.</span></span>

### <span data-ttu-id="95eaa-207"><a name="requestOverrides"></a>Definiera ett requestOverrides-objekt</span><span class="sxs-lookup"><span data-stu-id="95eaa-207"><a name="requestOverrides"></a>Define a requestOverrides object</span></span>

<span data-ttu-id="95eaa-208">Objektet requestOverrides definierar ändringar som gjorts på begäran när resursen backend-anropas.</span><span class="sxs-lookup"><span data-stu-id="95eaa-208">The requestOverrides object defines changes made to the request when the back-end resource is called.</span></span> <span data-ttu-id="95eaa-209">Objektet definieras av följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="95eaa-209">The object is defined by the following properties:</span></span>

* <span data-ttu-id="95eaa-210">**backend.Request.Method**: HTTP-metod som används för att anropa serverdelen.</span><span class="sxs-lookup"><span data-stu-id="95eaa-210">**backend.request.method**: The HTTP method that's used to call the back end.</span></span>
* <span data-ttu-id="95eaa-211">**backend.Request.QueryString. \<ParameterName\>**: en frågesträngsparameter som kan anges för anropet till serverdelen.</span><span class="sxs-lookup"><span data-stu-id="95eaa-211">**backend.request.querystring.\<ParameterName\>**: A query string parameter that can be set for the call to the back end.</span></span> <span data-ttu-id="95eaa-212">Ersätt  *\<ParameterName\>*  med namnet på den parameter som du vill ange.</span><span class="sxs-lookup"><span data-stu-id="95eaa-212">Replace *\<ParameterName\>* with the name of the parameter that you want to set.</span></span> <span data-ttu-id="95eaa-213">Om en tom sträng ingår inte parametern på backend-begäran.</span><span class="sxs-lookup"><span data-stu-id="95eaa-213">If the empty string is provided, the parameter is not included on the back-end request.</span></span>
* <span data-ttu-id="95eaa-214">**backend.Request.headers. \<HeaderName\>**: ett huvud som kan anges för anropet till serverdelen.</span><span class="sxs-lookup"><span data-stu-id="95eaa-214">**backend.request.headers.\<HeaderName\>**: A header that can be set for the call to the back end.</span></span> <span data-ttu-id="95eaa-215">Ersätt  *\<HeaderName\>*  med namnet på det huvud som du vill ange.</span><span class="sxs-lookup"><span data-stu-id="95eaa-215">Replace *\<HeaderName\>* with the name of the header that you want to set.</span></span> <span data-ttu-id="95eaa-216">Om du anger en tom sträng ingår inte rubriken på backend-begäran.</span><span class="sxs-lookup"><span data-stu-id="95eaa-216">If you provide the empty string, the header is not included on the back-end request.</span></span>

<span data-ttu-id="95eaa-217">Värden kan referera till programinställningar och parametrar från den ursprungliga klientbegäran.</span><span class="sxs-lookup"><span data-stu-id="95eaa-217">Values can reference application settings and parameters from the original client request.</span></span>

<span data-ttu-id="95eaa-218">En exempelkonfiguration kan se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="95eaa-218">An example configuration might look like the following:</span></span>

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

### <span data-ttu-id="95eaa-219"><a name="responseOverrides"></a>Definiera ett responseOverrides-objekt</span><span class="sxs-lookup"><span data-stu-id="95eaa-219"><a name="responseOverrides"></a>Define a responseOverrides object</span></span>

<span data-ttu-id="95eaa-220">Objektet requestOverrides definierar ändringar som görs i svaret som skickas tillbaka till klienten.</span><span class="sxs-lookup"><span data-stu-id="95eaa-220">The requestOverrides object defines changes that are made to the response that's passed back to the client.</span></span> <span data-ttu-id="95eaa-221">Objektet definieras av följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="95eaa-221">The object is defined by the following properties:</span></span>

* <span data-ttu-id="95eaa-222">**response.statusCode**: HTTP-statuskod som ska returneras till klienten.</span><span class="sxs-lookup"><span data-stu-id="95eaa-222">**response.statusCode**: The HTTP status code to be returned to the client.</span></span>
* <span data-ttu-id="95eaa-223">**response.statusReason**: HTTP orsaksfrasen ska returneras till klienten.</span><span class="sxs-lookup"><span data-stu-id="95eaa-223">**response.statusReason**: The HTTP reason phrase to be returned to the client.</span></span>
* <span data-ttu-id="95eaa-224">**Response.body**: strängrepresentation av text som ska returneras till klienten.</span><span class="sxs-lookup"><span data-stu-id="95eaa-224">**response.body**: The string representation of the body to be returned to the client.</span></span>
* <span data-ttu-id="95eaa-225">**Response.headers. \<HeaderName\>**: ett huvud som kan anges för svar till klienten.</span><span class="sxs-lookup"><span data-stu-id="95eaa-225">**response.headers.\<HeaderName\>**: A header that can be set for the response to the client.</span></span> <span data-ttu-id="95eaa-226">Ersätt  *\<HeaderName\>*  med namnet på det huvud som du vill ange.</span><span class="sxs-lookup"><span data-stu-id="95eaa-226">Replace *\<HeaderName\>* with the name of the header that you want to set.</span></span> <span data-ttu-id="95eaa-227">Om du anger en tom sträng ingår inte huvudet i svaret.</span><span class="sxs-lookup"><span data-stu-id="95eaa-227">If you provide the empty string, the header is not included on the response.</span></span>

<span data-ttu-id="95eaa-228">Värden kan referera till programinställningar, parametrar från den ursprungliga klientbegäran och parametrar från backend-svaret.</span><span class="sxs-lookup"><span data-stu-id="95eaa-228">Values can reference application settings, parameters from the original client request, and parameters from the back-end response.</span></span>

<span data-ttu-id="95eaa-229">En exempelkonfiguration kan se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="95eaa-229">An example configuration might look like the following:</span></span>

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
> <span data-ttu-id="95eaa-230">I det här exemplet brödtexten anges direkt, så ingen `backendUri` egenskapen krävs.</span><span class="sxs-lookup"><span data-stu-id="95eaa-230">In this example, the body is being set directly, so no `backendUri` property is needed.</span></span> <span data-ttu-id="95eaa-231">Exemplet visar hur du kan använda Azure Functions proxyservrar för mocking API: er.</span><span class="sxs-lookup"><span data-stu-id="95eaa-231">The example shows how you might use Azure Functions Proxies for mocking APIs.</span></span>

<span data-ttu-id="95eaa-232">[Azure-portalen]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="95eaa-232">[Azure portal]: https://portal.azure.com</span></span>
<span data-ttu-id="95eaa-233">[http-utlösare]: https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#http-trigger</span><span class="sxs-lookup"><span data-stu-id="95eaa-233">[HTTP triggers]: https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#http-trigger</span></span>
[Modify the back-end request]: #modify-backend-request
[Modify the response]: #modify-response
<span data-ttu-id="95eaa-234">[definiera ett requestOverrides objekt]: #requestOverrides</span><span class="sxs-lookup"><span data-stu-id="95eaa-234">[Define a requestOverrides object]: #requestOverrides</span></span>
<span data-ttu-id="95eaa-235">[definiera ett responseOverrides objekt]: #responseOverrides</span><span class="sxs-lookup"><span data-stu-id="95eaa-235">[Define a responseOverrides object]: #responseOverrides</span></span>
<span data-ttu-id="95eaa-236">[programinställningar]: #use-appsettings</span><span class="sxs-lookup"><span data-stu-id="95eaa-236">[application settings]: #use-appsettings</span></span>
<span data-ttu-id="95eaa-237">[använda variabler]: #using-variables</span><span class="sxs-lookup"><span data-stu-id="95eaa-237">[Use variables]: #using-variables</span></span>
<span data-ttu-id="95eaa-238">[parametrar från den ursprungliga klientbegäran]: #request-parameters</span><span class="sxs-lookup"><span data-stu-id="95eaa-238">[parameters from the original client request]: #request-parameters</span></span>
<span data-ttu-id="95eaa-239">[parametrar från backend-svaret]: #response-parameters</span><span class="sxs-lookup"><span data-stu-id="95eaa-239">[parameters from the back-end response]: #response-parameters</span></span>
