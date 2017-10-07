---
title: aaaAdd cachelagring tooimprove prestanda i Azure API Management | Microsoft Docs
description: "Lär dig hur tooimprove hello svarstid, bandbreddsanvändning och webbtjänsten ladda för hantering av API-anrop."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 740f6a27-8323-474d-ade2-828ae0c75e7a
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 056ab7cf788218327e30bd5c028b76e3b1977fb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-caching-tooimprove-performance-in-azure-api-management"></a><span data-ttu-id="a7353-103">Lägg till cachelagring tooimprove prestanda i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="a7353-103">Add caching tooimprove performance in Azure API Management</span></span>
<span data-ttu-id="a7353-104">Du kan konfigurera åtgärder i API Management för cachelagring av svar.</span><span class="sxs-lookup"><span data-stu-id="a7353-104">Operations in API Management can be configured for response caching.</span></span> <span data-ttu-id="a7353-105">Cachelagring av svar kan avsevärt minska API:ets svarstider, bandbreddsanvändning och webbtjänstbelastning när data inte ändras så ofta.</span><span class="sxs-lookup"><span data-stu-id="a7353-105">Response caching can significantly reduce API latency, bandwidth consumption, and web service load for data that does not change frequently.</span></span>

<span data-ttu-id="a7353-106">Den här guiden visar hur tooadd svar cachelagring för din API och konfigurera principer för hello exempel Echo-API: et.</span><span class="sxs-lookup"><span data-stu-id="a7353-106">This guide shows you how tooadd response caching for your API and configure policies for hello sample Echo API operations.</span></span> <span data-ttu-id="a7353-107">Sedan kan du anropa hello åtgärden från hello developer portal tooverify cachelagring i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a7353-107">You can then call hello operation from hello developer portal tooverify caching in action.</span></span>

> [!NOTE]
> <span data-ttu-id="a7353-108">Mer information om hur du cachelagrar objekt med nycklar med hjälp av principuttryck finns i [Anpassad cachelagring i Azure API Management](api-management-sample-cache-by-key.md).</span><span class="sxs-lookup"><span data-stu-id="a7353-108">For information on caching items by key using policy expressions, see [Custom caching in Azure API Management](api-management-sample-cache-by-key.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="a7353-109">Krav</span><span class="sxs-lookup"><span data-stu-id="a7353-109">Prerequisites</span></span>
<span data-ttu-id="a7353-110">Innan följande hello stegen i den här guiden, måste du ha en API Management service-instans med API och en produkt som har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="a7353-110">Before following hello steps in this guide, you must have an API Management service instance with an API and a product configured.</span></span> <span data-ttu-id="a7353-111">Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.</span><span class="sxs-lookup"><span data-stu-id="a7353-111">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>

## <span data-ttu-id="a7353-112"><a name="configure-caching"> </a>Konfigurera en åtgärd för cachelagring</span><span class="sxs-lookup"><span data-stu-id="a7353-112"><a name="configure-caching"> </a>Configure an operation for caching</span></span>
<span data-ttu-id="a7353-113">I det här steget ska du granska hello cacheinställningar av hello **hämta resurs (Zeroed)** åtgärden hello exemplet Echo-API.</span><span class="sxs-lookup"><span data-stu-id="a7353-113">In this step, you will review hello caching settings of hello **GET Resource (cached)** operation of hello sample Echo API.</span></span>

> [!NOTE]
> <span data-ttu-id="a7353-114">Varje instans för API Management-tjänsten kommer förkonfigurerade med en Echo-API som kan använda tooexperiment med och lär dig mer om API-hantering.</span><span class="sxs-lookup"><span data-stu-id="a7353-114">Each API Management service instance comes preconfigured with an Echo API that can be used tooexperiment with and learn about API Management.</span></span> <span data-ttu-id="a7353-115">Mer information finns i [Komma igång med Azure API Management][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="a7353-115">For more information, see [Get started with Azure API Management][Get started with Azure API Management].</span></span>
> 
> 

<span data-ttu-id="a7353-116">tooget har startats klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a7353-116">tooget started, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span> <span data-ttu-id="a7353-117">Då kommer du toohello API Management publisher portal.</span><span class="sxs-lookup"><span data-stu-id="a7353-117">This takes you toohello API Management publisher portal.</span></span>

![Utgivarportalen][api-management-management-console]

<span data-ttu-id="a7353-119">Klicka på **API: er** från hello **API Management** menyn på hello vänster och klicka sedan på **Echo API**.</span><span class="sxs-lookup"><span data-stu-id="a7353-119">Click **APIs** from hello **API Management** menu on hello left, and then click **Echo API**.</span></span>

![Echo API][api-management-echo-api]

<span data-ttu-id="a7353-121">Klicka på hello **Operations** fliken och klicka sedan på hello **hämta resurs (Zeroed)** åtgärden från hello **Operations** lista.</span><span class="sxs-lookup"><span data-stu-id="a7353-121">Click hello **Operations** tab, and then click hello **GET Resource (cached)** operation from hello **Operations** list.</span></span>

![Echo API-åtgärder][api-management-echo-api-operations]

<span data-ttu-id="a7353-123">Klicka på hello **cachelagring** fliken tooview hello cache-inställningar för den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a7353-123">Click hello **Caching** tab tooview hello caching settings for this operation.</span></span>

![Fliken Cachelagring][api-management-caching-tab]

<span data-ttu-id="a7353-125">tooenable cachelagring för en åtgärd, Välj hello **aktivera** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="a7353-125">tooenable caching for an operation, select hello **Enable** check box.</span></span> <span data-ttu-id="a7353-126">I det här exemplet är cachelagring aktiverat.</span><span class="sxs-lookup"><span data-stu-id="a7353-126">In this example, caching is enabled.</span></span>

<span data-ttu-id="a7353-127">Anpassade varje åtgärden svar, baserat på hello värdena i hello **variera inom strängen frågeparametrar** och **variera av huvuden** fält.</span><span class="sxs-lookup"><span data-stu-id="a7353-127">Each operation response is keyed, based on hello values in hello **Vary by query string parameters** and **Vary by headers** fields.</span></span> <span data-ttu-id="a7353-128">Om du vill toocache som flera svar baserat på frågan string-parametrar eller huvuden kan konfigurera du dem i dessa två fält.</span><span class="sxs-lookup"><span data-stu-id="a7353-128">If you want toocache multiple responses based on query string parameters or headers, you can configure them in these two fields.</span></span>

<span data-ttu-id="a7353-129">**Varaktighet** anger hello giltighetstid tidsintervall hello cachelagrat svar.</span><span class="sxs-lookup"><span data-stu-id="a7353-129">**Duration** specifies hello expiration interval of hello cached responses.</span></span> <span data-ttu-id="a7353-130">I det här exemplet hello är **3600** sekunder, vilket är likvärdiga tooone timme.</span><span class="sxs-lookup"><span data-stu-id="a7353-130">In this example, hello interval is **3600** seconds, which is equivalent tooone hour.</span></span>

<span data-ttu-id="a7353-131">Med hello cachelagring konfigurationen i det här exemplet hello första begäran toohello **hämta resurs (Zeroed)** åtgärd returnerar ett svar från hello backend-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a7353-131">Using hello caching configuration in this example, hello first request toohello **GET Resource (cached)** operation returns a response from hello backend service.</span></span> <span data-ttu-id="a7353-132">Svaret ska cachelagras Nyckelbaserade av hello angetts sidhuvuden och fråga strängparametrar.</span><span class="sxs-lookup"><span data-stu-id="a7353-132">This response will be cached, keyed by hello specified headers and query string parameters.</span></span> <span data-ttu-id="a7353-133">Efterföljande anrop toohello åtgärden med matchande parametrar, har hello cachelagrade svar som returneras till hello cache varaktighet intervall har gått ut.</span><span class="sxs-lookup"><span data-stu-id="a7353-133">Subsequent calls toohello operation, with matching parameters, will have hello cached response returned, until hello cache duration interval has expired.</span></span>

## <span data-ttu-id="a7353-134"><a name="caching-policies"></a>Granska hello cachelagring principer</span><span class="sxs-lookup"><span data-stu-id="a7353-134"><a name="caching-policies"> </a>Review hello caching policies</span></span>
<span data-ttu-id="a7353-135">I det här steget kan du granska hello cacheinställningar för hello **hämta resurs (Zeroed)** åtgärden hello exemplet Echo-API.</span><span class="sxs-lookup"><span data-stu-id="a7353-135">In this step, you review hello caching settings for hello **GET Resource (cached)** operation of hello sample Echo API.</span></span>

<span data-ttu-id="a7353-136">När inställningar för cachelagring har konfigurerats för en åtgärd på hello **cachelagring** fliken Cachelagring principer har lagts till för hello igen.</span><span class="sxs-lookup"><span data-stu-id="a7353-136">When caching settings are configured for an operation on hello **Caching** tab, caching policies are added for hello operation.</span></span> <span data-ttu-id="a7353-137">Dessa principer kan visas och ändras i hello redigeraren.</span><span class="sxs-lookup"><span data-stu-id="a7353-137">These policies can be viewed and edited in hello policy editor.</span></span>

<span data-ttu-id="a7353-138">Klicka på **principer** från hello **API Management** menyn på hello vänster och välj sedan **Echo API / hämta resurs (Zeroed)** från hello **åtgärden**listrutan.</span><span class="sxs-lookup"><span data-stu-id="a7353-138">Click **Policies** from hello **API Management** menu on hello left, and then select **Echo API / GET Resource (cached)** from hello **Operation** drop-down list.</span></span>

![Åtgärden Principomfattning][api-management-operation-dropdown]

<span data-ttu-id="a7353-140">Redigeraren för hello visas hello principer för den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a7353-140">This displays hello policies for this operation in hello policy editor.</span></span>

![API Management-principredigeraren][api-management-policy-editor]

<span data-ttu-id="a7353-142">hello principdefinitionen för den här åtgärden innehåller hello principer som definierar hello cachelagring konfiguration som kontrollerades med hello **cachelagring** fliken i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="a7353-142">hello policy definition for this operation includes hello policies that define hello caching configuration that were reviewed using hello **Caching** tab in hello previous step.</span></span>

```xml
<policies>
    <inbound>
        <base />
        <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
            <vary-by-header>Accept</vary-by-header>
            <vary-by-header>Accept-Charset</vary-by-header>
        </cache-lookup>
        <rewrite-uri template="/resource" />
    </inbound>
    <outbound>
        <base />
        <cache-store caching-mode="cache-on" duration="3600" />
    </outbound>
</policies>
```

> [!NOTE]
> <span data-ttu-id="a7353-143">Ändringar som gjorts toohello cachelagring principer i Redigeraren för hello används på hello **cachelagring** fliken för en åtgärd och vice versa.</span><span class="sxs-lookup"><span data-stu-id="a7353-143">Changes made toohello caching policies in hello policy editor will be reflected on hello **Caching** tab of an operation, and vice-versa.</span></span>
> 
> 

## <span data-ttu-id="a7353-144"><a name="test-operation"></a>Anropa en åtgärd och testa hello cachelagring</span><span class="sxs-lookup"><span data-stu-id="a7353-144"><a name="test-operation"> </a>Call an operation and test hello caching</span></span>
<span data-ttu-id="a7353-145">toosee hello cachelagring i praktiken vi kallar hello åtgärden från hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="a7353-145">toosee hello caching in action, we can call hello operation from hello developer portal.</span></span> <span data-ttu-id="a7353-146">Klicka på **utvecklarportalen** i hello övre högra menyn.</span><span class="sxs-lookup"><span data-stu-id="a7353-146">Click **Developer portal** in hello top right menu.</span></span>

![Utvecklarportalen][api-management-developer-portal-menu]

<span data-ttu-id="a7353-148">Klicka på **API: er** i hello översta menyn och välj sedan **Echo API**.</span><span class="sxs-lookup"><span data-stu-id="a7353-148">Click **APIs** in hello top menu, and then select **Echo API**.</span></span>

![Echo API][api-management-apis-echo-api]

> <span data-ttu-id="a7353-150">Om du har bara en API som konfigurerats eller synliga tooyour konto, klicka på API: er kommer du direkt toohello åtgärder för att API.</span><span class="sxs-lookup"><span data-stu-id="a7353-150">If you have only one API configured or visible tooyour account, then clicking APIs takes you directly toohello operations for that API.</span></span>
> 
> 

<span data-ttu-id="a7353-151">Välj hello **hämta resurs (Zeroed)** igen och klicka sedan på **öppna konsolen**.</span><span class="sxs-lookup"><span data-stu-id="a7353-151">Select hello **GET Resource (cached)** operation, and then click **Open Console**.</span></span>

![Öppna konsolen][api-management-open-console]

<span data-ttu-id="a7353-153">hello-konsolen kan du tooinvoke operations direkt från hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="a7353-153">hello console allows you tooinvoke operations directly from hello developer portal.</span></span>

![Konsolen][api-management-console]

<span data-ttu-id="a7353-155">Behåll hello standardvärden för **param1** och **param2**.</span><span class="sxs-lookup"><span data-stu-id="a7353-155">Keep hello default values for **param1** and **param2**.</span></span>

<span data-ttu-id="a7353-156">Välj hello önskade nyckeln från hello **prenumeration nyckeln** listrutan.</span><span class="sxs-lookup"><span data-stu-id="a7353-156">Select hello desired key from hello **subscription-key** drop-down list.</span></span> <span data-ttu-id="a7353-157">Om ditt konto bara har en prenumeration är den redan vald.</span><span class="sxs-lookup"><span data-stu-id="a7353-157">If your account has only one subscription, it will already be selected.</span></span>

<span data-ttu-id="a7353-158">Ange **sampleheader:value1** i hello **begärandehuvuden** textruta.</span><span class="sxs-lookup"><span data-stu-id="a7353-158">Enter **sampleheader:value1** in hello **Request headers** text box.</span></span>

<span data-ttu-id="a7353-159">Klicka på **HTTP Get** och anteckna hello-svarshuvuden.</span><span class="sxs-lookup"><span data-stu-id="a7353-159">Click **HTTP Get** and make a note of hello response headers.</span></span>

<span data-ttu-id="a7353-160">Ange **sampleheader:value2** i hello **Begäransrubriker** textrutan och klicka sedan på **HTTP Get**.</span><span class="sxs-lookup"><span data-stu-id="a7353-160">Enter **sampleheader:value2** in hello **Request headers** text box, and then click **HTTP Get**.</span></span>

<span data-ttu-id="a7353-161">Observera att hello-värdet för **sampleheader** fortfarande **value1** hello svar.</span><span class="sxs-lookup"><span data-stu-id="a7353-161">Note that hello value of **sampleheader** is still **value1** in hello response.</span></span> <span data-ttu-id="a7353-162">Prova några olika värden och Observera att hello cachelagrade svar från hello första anropet returneras.</span><span class="sxs-lookup"><span data-stu-id="a7353-162">Try some different values and note that hello cached response from hello first call is returned.</span></span>

<span data-ttu-id="a7353-163">Ange **25** till hello **param2** fältet och klickar sedan på **HTTP Get**.</span><span class="sxs-lookup"><span data-stu-id="a7353-163">Enter **25** into hello **param2** field, and then click **HTTP Get**.</span></span>

<span data-ttu-id="a7353-164">Observera att hello-värdet för **sampleheader** hello svar finns i **value2**.</span><span class="sxs-lookup"><span data-stu-id="a7353-164">Note that hello value of **sampleheader** in hello response is now **value2**.</span></span> <span data-ttu-id="a7353-165">Eftersom hello Åtgärdsresultat baseras frågesträng, returnerades hello föregående cachelagrade svar inte.</span><span class="sxs-lookup"><span data-stu-id="a7353-165">Because hello operation results are keyed by query string, hello previous cached response was not returned.</span></span>

## <span data-ttu-id="a7353-166"><a name="next-steps"> </a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a7353-166"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="a7353-167">Mer information om cachelagring principer finns [cachelagring principer] [ Caching policies] i hello [principreferens för API Management][API Management policy reference].</span><span class="sxs-lookup"><span data-stu-id="a7353-167">For more information about caching policies, see [Caching policies][Caching policies] in hello [API Management policy reference][API Management policy reference].</span></span>
* <span data-ttu-id="a7353-168">Mer information om hur du cachelagrar objekt med nycklar med hjälp av principuttryck finns i [Anpassad cachelagring i Azure API Management](api-management-sample-cache-by-key.md).</span><span class="sxs-lookup"><span data-stu-id="a7353-168">For information on caching items by key using policy expressions, see [Custom caching in Azure API Management](api-management-sample-cache-by-key.md).</span></span>

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How tooadd and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs tooa product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md

[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[Configure an operation for caching]: #configure-caching
[Review hello caching policies]: #caching-policies
[Call an operation and test hello caching]: #test-operation
[Next steps]: #next-steps
