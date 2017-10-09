---
title: aaaHow tooadd operations tooan API i Azure API Management | Microsoft Docs
description: "Lär dig hur tooadd operations tooan API i Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 1158a023-1913-4e9c-93de-9164b672f9b3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: d57fa59a2b0ceb392cde23150a0cbb326e52d27d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-operations-tooan-api-in-azure-api-management"></a><span data-ttu-id="7e93c-103">Hur tooadd operations tooan API i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="7e93c-103">How tooadd operations tooan API in Azure API Management</span></span>
<span data-ttu-id="7e93c-104">Innan en API i API Management kan användas, måste du lägga till åtgärder.</span><span class="sxs-lookup"><span data-stu-id="7e93c-104">Before an API in API Management can be used, operations must be added.</span></span> <span data-ttu-id="7e93c-105">Detta hjälper visar hur tooadd och konfigurera olika typer av åtgärder tooan API i API-hantering.</span><span class="sxs-lookup"><span data-stu-id="7e93c-105">This guide shows how tooadd and configure different types of operations tooan API in API Management.</span></span>

## <span data-ttu-id="7e93c-106"><a name="add-operation"></a>Lägga till en åtgärd</span><span class="sxs-lookup"><span data-stu-id="7e93c-106"><a name="add-operation"> </a>Add an operation</span></span>
<span data-ttu-id="7e93c-107">Åtgärder har lagts till och konfigurerats tooan API i hello publisher portal.</span><span class="sxs-lookup"><span data-stu-id="7e93c-107">Operations are added and configured tooan API in hello publisher portal.</span></span> <span data-ttu-id="7e93c-108">tooaccess hello publisher klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7e93c-108">tooaccess hello publisher portal, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span>

![Utgivarportalen][api-management-management-console]

> <span data-ttu-id="7e93c-110">Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.</span><span class="sxs-lookup"><span data-stu-id="7e93c-110">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="7e93c-111">Välj hello önskade API i hello publisher portal och välj sedan hello **Operations** fliken.</span><span class="sxs-lookup"><span data-stu-id="7e93c-111">Select hello desired API in hello publisher portal and then select hello **Operations** tab.</span></span> 

![Åtgärder][api-management-operations]

<span data-ttu-id="7e93c-113">Klicka på **lägga till åtgärden** tooadd en ny operation.</span><span class="sxs-lookup"><span data-stu-id="7e93c-113">Click **Add Operation** tooadd a new operation.</span></span> <span data-ttu-id="7e93c-114">Hej **nya åtgärden** visas och hello **signatur** fliken väljs som standard.</span><span class="sxs-lookup"><span data-stu-id="7e93c-114">hello **New operation** will be displayed and hello **Signature** tab will be selected by default.</span></span>

![Lägg till åtgärd][api-management-add-operation]

<span data-ttu-id="7e93c-116">Ange hello **HTTP-verbet** genom att välja från hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="7e93c-116">Specify hello **HTTP verb** by choosing from hello drop-down list.</span></span>

![HTTP-metoden][api-management-http-method]

<a name="url-template"></a>

<span data-ttu-id="7e93c-118">Definiera hello URL: en mall genom att skriva in ett URL-fragment som består av en eller flera URL-sökvägssegment och noll eller flera sträng Frågeparametrar.</span><span class="sxs-lookup"><span data-stu-id="7e93c-118">Define hello URL template by typing in a URL fragment consisting of one or more URL path segments and zero or more query string parameters.</span></span> <span data-ttu-id="7e93c-119">hello URL: en mall, tillagda toohello bas-URL för hello API, identifierar en enskild HTTP-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="7e93c-119">hello URL template, appended toohello base URL of hello API, identifies a single HTTP operation.</span></span> <span data-ttu-id="7e93c-120">Det kan innehålla en eller flera namngivna variabeln delar som identifieras av klammerparenteser.</span><span class="sxs-lookup"><span data-stu-id="7e93c-120">It may contain one or more named variable parts that are identified by curly braces.</span></span> <span data-ttu-id="7e93c-121">Dessa rörliga delar kallas mallparametrar och tilldelas dynamiskt värden som extraheras från hello URL: en när hello begäran behandlas av hello API Management-plattformen.</span><span class="sxs-lookup"><span data-stu-id="7e93c-121">These variable parts are called template parameters and are dynamically assigned values extracted from hello request's URL when hello request is being processed by hello API Management platform.</span></span>

> <span data-ttu-id="7e93c-122">hello URL: en mall kan innehålla mönster med jokertecken.</span><span class="sxs-lookup"><span data-stu-id="7e93c-122">hello URL template can include wildcard patterns.</span></span> <span data-ttu-id="7e93c-123">Ange till exempel `/*` vidarebefordra alla begäranden om att HTTP-metoden toohello tillbaka avslutas tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7e93c-123">For example, specifying `/*` will forward all requests for that HTTP method toohello back end service.</span></span>

![URL: en mall][api-management-url-template]

<a name="rewrite-url-template"></a>

<span data-ttu-id="7e93c-125">Om du vill ange hello **skriva om URL: en mall**.</span><span class="sxs-lookup"><span data-stu-id="7e93c-125">If desired, specify hello **Rewrite URL template**.</span></span> <span data-ttu-id="7e93c-126">Detta gör att du toouse hello URL: en standardmall för bearbetning av inkommande begäranden på hello frontend, när du anropar hello serverdel via en konverterade URL enligt toohello omskrivning av mallen.</span><span class="sxs-lookup"><span data-stu-id="7e93c-126">This allows you toouse hello standard URL template for processing incoming requests on hello front-end, while calling hello back-end via a converted URL according toohello rewrite template.</span></span> <span data-ttu-id="7e93c-127">Mallparametrar från hello URL mallen ska användas i hello omarbetning mallen.</span><span class="sxs-lookup"><span data-stu-id="7e93c-127">Template parameters from hello URL template should be used in hello rewrite template.</span></span> <span data-ttu-id="7e93c-128">hello visas följande exempel hur content-type kodad som sökvägssegmentet i hello webbtjänst från hello föregående exempel kan anges som en frågeparameter i hello API som publicerats via hello API-plattform med hjälp av mallar för hello-URL.</span><span class="sxs-lookup"><span data-stu-id="7e93c-128">hello following example shows how content type encoded as path segment in hello web service from hello previous example can be provided as a query parameter in hello API published via hello API Management platform using hello URL templates.</span></span>

![URL: en mall för omarbetning][api-management-url-template-rewrite]

<span data-ttu-id="7e93c-130">Anropare toohello åtgärden använder hello formatet `/customers?customerid=ALFKI` och detta kommer att mappas för`/Customers('ALFKI')` när hello backend-tjänst har anropats.</span><span class="sxs-lookup"><span data-stu-id="7e93c-130">Callers toohello operation will use hello format `/customers?customerid=ALFKI` and this will be mapped too`/Customers('ALFKI')` when hello back-end service is invoked.</span></span>

<span data-ttu-id="7e93c-131">**Visa** namn och **beskrivning** ger en beskrivning av åtgärden hello och är används tooprovide dokumentationen toohello utvecklare som använder detta API i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="7e93c-131">**Display** name and **Description** provide a description of hello operation and are used tooprovide documentation toohello developers using this API in hello developer portal.</span></span>

![Beskrivning][api-management-description]

<span data-ttu-id="7e93c-133">hello åtgärden beskrivning kan anges som oformaterad text eller HTML i hello **beskrivning** textruta.</span><span class="sxs-lookup"><span data-stu-id="7e93c-133">hello operation description can be specified as plain text or HTML in hello **Description** text box.</span></span>

## <span data-ttu-id="7e93c-134"><a name="operation-caching"></a>Åtgärden cachelagring</span><span class="sxs-lookup"><span data-stu-id="7e93c-134"><a name="operation-caching"> </a>Operation caching</span></span>
<span data-ttu-id="7e93c-135">Svaret cachelagring minskar svarstider uppfattas av hello API konsumenter, driftmiljön bandbreddsanvändning och minskar hello belastningen på hello HTTP web service implementera hello API.</span><span class="sxs-lookup"><span data-stu-id="7e93c-135">Response caching reduces latency perceived by hello API consumers, lowers bandwidth consumption and decreases hello load on hello HTTP web service implementing hello API.</span></span> 

<span data-ttu-id="7e93c-136">tooeasily och snabbt aktivera cachelagring för hello åtgärden väljer hello **cachelagring** och kontrollerar hello **aktivera** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="7e93c-136">tooeasily and quickly enable caching for hello operation, select hello **Caching** tab and check hello **Enable** checkbox.</span></span>

![Cachelagring][api-management-caching-tab]

<span data-ttu-id="7e93c-138">**Varaktighet** anger hello tidsperiod under vilken hello åtgärden svar finns kvar i hello cache.</span><span class="sxs-lookup"><span data-stu-id="7e93c-138">**Duration** specifies hello time period during which hello operation response remains in hello cache.</span></span> <span data-ttu-id="7e93c-139">hello standardvärdet är 3 600 sekunder eller 1 timme.</span><span class="sxs-lookup"><span data-stu-id="7e93c-139">hello default value is 3600 seconds or 1 hour.</span></span>

<span data-ttu-id="7e93c-140">Cache-nycklar är används toodifferentiate mellan svar så att hello svaret motsvarande tooeach olika cachenyckel får sitt eget separat cachelagrade värde.</span><span class="sxs-lookup"><span data-stu-id="7e93c-140">Cache keys are used toodifferentiate between responses so that hello response corresponding tooeach different cache key will get its own separate cached value.</span></span> <span data-ttu-id="7e93c-141">Du kan också ange särskilda frågeparametrar för sträng och/eller HTTP-huvuden toobe använts vid beräkning av cache nyckelvärdena i hello **variera inom strängen frågeparametrar** och **variera av huvuden** textrutor respektive.</span><span class="sxs-lookup"><span data-stu-id="7e93c-141">Optionally, enter specific query string parameters and/or HTTP headers toobe used in computing cache key values in hello **Vary by query string parameters** and **Vary by headers** text boxes respectively.</span></span> <span data-ttu-id="7e93c-142">När ingen är angivna, fullständig URL: en och hello följande värden i HTTP-huvudet som används i cache-nyckelgenerering: **acceptera** och **Accept-Charset**.</span><span class="sxs-lookup"><span data-stu-id="7e93c-142">When none are specified, full request URL and hello following HTTP header values are used in cache key generation: **Accept** and **Accept-Charset**.</span></span>

> <span data-ttu-id="7e93c-143">Mer information om cachelagring och cachelagring principer finns [hur toocache åtgärden resulterar i Azure API Management][How toocache operation results in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="7e93c-143">For more information on caching and caching policies, see [How toocache operation results in Azure API Management][How toocache operation results in Azure API Management].</span></span>
> 
> 

## <span data-ttu-id="7e93c-144"><a name="request-parameters"></a>Parametrar för begäran</span><span class="sxs-lookup"><span data-stu-id="7e93c-144"><a name="request-parameters"> </a>Request parameters</span></span>
<span data-ttu-id="7e93c-145">Åtgärdsparametrar hanteras på fliken för hello-parametrar. Parametrar som anges i hello **URL mallen** på hello **signatur** läggs till automatiskt och kan ändras endast genom att redigera mallen för hello-URL.</span><span class="sxs-lookup"><span data-stu-id="7e93c-145">Operation parameters are managed on hello Parameters tab. Parameters specified in hello **URL Template** on hello **Signature** tab are added automatically and can be changed only by editing hello URL template.</span></span> <span data-ttu-id="7e93c-146">Ytterligare parametrar kan anges manuellt.</span><span class="sxs-lookup"><span data-stu-id="7e93c-146">Additional parameters can be entered manually.</span></span>

<span data-ttu-id="7e93c-147">tooadd en ny frågeparameter klickar du på **lägga till frågeparameter** och ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="7e93c-147">tooadd a new query parameter, click **Add Query Parameter** and enter hello following information:</span></span>

* <span data-ttu-id="7e93c-148">**Namnet** -parameternamnet.</span><span class="sxs-lookup"><span data-stu-id="7e93c-148">**Name** - parameter name.</span></span>
* <span data-ttu-id="7e93c-149">**Beskrivning** -en kort beskrivning av hello-parametern (valfritt).</span><span class="sxs-lookup"><span data-stu-id="7e93c-149">**Description** - a brief description of hello parameter (optional).</span></span>
* <span data-ttu-id="7e93c-150">**Typen** -parametertypen som valts i hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="7e93c-150">**Type** - parameter type, selected in hello drop down.</span></span>
* <span data-ttu-id="7e93c-151">**Värden** -värden som kan tilldelas toothis-parametern.</span><span class="sxs-lookup"><span data-stu-id="7e93c-151">**Values** - values that can be assigned toothis parameter.</span></span> <span data-ttu-id="7e93c-152">Ett av hello värden kan anges som standard (valfritt).</span><span class="sxs-lookup"><span data-stu-id="7e93c-152">One of hello values can be marked as default (optional).</span></span>
* <span data-ttu-id="7e93c-153">**Krävs** -göra hello parametern obligatorisk genom att kontrollera hello kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="7e93c-153">**Required** - make hello parameter mandatory by checking hello checkbox.</span></span> 

![Parametrar för begäran][api-management-request-parameters]

## <span data-ttu-id="7e93c-155"><a name="request-body"></a>Begärandetext</span><span class="sxs-lookup"><span data-stu-id="7e93c-155"><a name="request-body"> </a>Request body</span></span>
<span data-ttu-id="7e93c-156">Om hello åtgärden tillåter (t.ex. PUT, POST) och kräver en text som du kan ge ett exempel på det till alla hello stöds representation format (t.ex. json, XML).</span><span class="sxs-lookup"><span data-stu-id="7e93c-156">If hello operation allows (e.g. PUT, POST) and requires a body you may provide an example of it in all of hello supported representation formats (e.g. json, XML).</span></span> 

> <span data-ttu-id="7e93c-157">Hej begärandetexten används endast för dokumentation och har inte verifierats.</span><span class="sxs-lookup"><span data-stu-id="7e93c-157">hello request body is used for documentation purposes only and is not validated.</span></span>
> 
> 

<span data-ttu-id="7e93c-158">tooenter en begärantext växla toohello **brödtext** fliken.</span><span class="sxs-lookup"><span data-stu-id="7e93c-158">tooenter a request body, switch toohello **Body** tab.</span></span>

<span data-ttu-id="7e93c-159">Klicka på **lägga till Representation**börjar skriva önskad content-type-namn (t.ex. program/json), markerar du den i hello listrutan och klistra in hello önskad begäran brödtext exempel hello valda format i hello textruta.</span><span class="sxs-lookup"><span data-stu-id="7e93c-159">Click **Add Representation**, start typing desired content type name (e.g. application/json), select it in hello drop-down, and paste hello desired request body example in hello selected format into hello text box.</span></span> 

![Begärandetexten][api-management-request-body]

<span data-ttu-id="7e93c-161">I ytterligare toorepresentations kan du också ange en valfri beskrivning i hello **beskrivning** textruta.</span><span class="sxs-lookup"><span data-stu-id="7e93c-161">In additional toorepresentations, you can also specify an optional text description in hello **Description** text box.</span></span>

## <span data-ttu-id="7e93c-162"><a name="responses"></a>Svar</span><span class="sxs-lookup"><span data-stu-id="7e93c-162"><a name="responses"> </a>Responses</span></span>
<span data-ttu-id="7e93c-163">Det är en bra idé tooprovide exempel på svar för alla statuskoder som kan generera hello igen.</span><span class="sxs-lookup"><span data-stu-id="7e93c-163">It is a good practice tooprovide examples of responses for all status codes that hello operation may produce.</span></span> <span data-ttu-id="7e93c-164">Varje statuskod kan ha fler än ett svar brödtext exempel, en för varje hello stöds innehållstyper.</span><span class="sxs-lookup"><span data-stu-id="7e93c-164">Each status code may have more than one response body example, one for each of hello supported content types.</span></span> 

<span data-ttu-id="7e93c-165">tooadd ett svar, klickar du på **Lägg till** och börja skriva hello önskad statuskod.</span><span class="sxs-lookup"><span data-stu-id="7e93c-165">tooadd a response, click **Add** and start typing hello desired status code.</span></span> <span data-ttu-id="7e93c-166">I det här exemplet hello status koden är **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="7e93c-166">In this example hello status code is **200 OK**.</span></span> <span data-ttu-id="7e93c-167">När hello kod visas i hello listrutan, markerar du det och hello svarskoden är skapas och tillagda tooyour igen.</span><span class="sxs-lookup"><span data-stu-id="7e93c-167">Once hello code is displayed in hello drop-down, select it, and hello response code is created and added tooyour operation.</span></span>

![Svarskod][api-management-response-code]

<span data-ttu-id="7e93c-169">Klicka på **lägga till Representation**, börja skriva hello önskade content-type-namn (t.ex. program/json) och välj sedan den i hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="7e93c-169">Click **Add Representation**, start typing hello desired content type name (e.g. application/json) and then select it in hello drop down.</span></span>

![Brödtext innehållstyp][api-management-response-body-content-type]

<span data-ttu-id="7e93c-171">Klistra in hello svar brödtext exempel hello valda format i hello textruta.</span><span class="sxs-lookup"><span data-stu-id="7e93c-171">Paste hello response body example in hello selected format into hello text box.</span></span> 

![Svarstexten][api-management-response-body]

<span data-ttu-id="7e93c-173">Om du vill lägga till en valfri beskrivning i hello **beskrivning** textruta.</span><span class="sxs-lookup"><span data-stu-id="7e93c-173">If desired, add an optional description into hello **Description** text box.</span></span>

<span data-ttu-id="7e93c-174">När hello åtgärd har konfigurerats klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="7e93c-174">Once hello operation is configured, click **Save**.</span></span>

## <span data-ttu-id="7e93c-175"><a name="next-steps"> </a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7e93c-175"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="7e93c-176">När hello operations läggs tooan API, hello nästa steg tooassociate hello API med en produkt och publicera den så att utvecklare kan anropa åtgärderna.</span><span class="sxs-lookup"><span data-stu-id="7e93c-176">Once hello operations are added tooan API, hello next step is tooassociate hello API with a product and publish it so that developers can call its operations.</span></span>

* <span data-ttu-id="7e93c-177">[Hur toocreate och publicera en produkt][How toocreate and publish a product]</span><span class="sxs-lookup"><span data-stu-id="7e93c-177">[How toocreate and publish a product][How toocreate and publish a product]</span></span>

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[How toocache operation results in Azure API Management]: api-management-howto-cache.md
