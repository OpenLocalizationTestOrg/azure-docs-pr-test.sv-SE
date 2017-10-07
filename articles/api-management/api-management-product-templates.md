---
title: aaaProduct mallar i Azure API Management | Microsoft Docs
description: "Lär dig hur toocustomize hello innehållet hello produkt sidor i hello Azure API Management developer-portalen."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 49f9254c-4c5f-4ed4-9c8d-798f44e805ee
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 60600299287aad87f9b621782ab5ceb866601d03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="product-templates-in-azure-api-management"></a><span data-ttu-id="b063f-103">Produktmallar i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="b063f-103">Product templates in Azure API Management</span></span>
<span data-ttu-id="b063f-104">Azure API Management ger du hello möjlighet toocustomize hello innehållet i developer portalens sidor med hjälp av en uppsättning mallar som konfigurerar deras innehåll.</span><span class="sxs-lookup"><span data-stu-id="b063f-104">Azure API Management provides you hello ability toocustomize hello content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="b063f-105">Med hjälp av [DotLiquid](http://dotliquidmarkup.org/) syntax och hello redigeringsprogram, t.ex [DotLiquid för Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), och en angiven uppsättning lokaliserade [String resurser](api-management-template-resources.md#strings), [ Glyf resurser](api-management-template-resources.md#glyphs), och [sidan kontroller](api-management-page-controls.md), du har stor flexibilitet tooconfigure hello innehåll hello sidor som du vill använda dessa mallar.</span><span class="sxs-lookup"><span data-stu-id="b063f-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and hello editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility tooconfigure hello content of hello pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="b063f-106">hello mallar i det här avsnittet kan du toocustomize hello innehåll hello produkten sidor i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="b063f-106">hello templates in this section allow you toocustomize hello content of hello product pages in hello developer portal.</span></span>  
  
-   [<span data-ttu-id="b063f-107">Produktlista</span><span class="sxs-lookup"><span data-stu-id="b063f-107">Product list</span></span>](#ProductList)  
  
-   [<span data-ttu-id="b063f-108">Produkten</span><span class="sxs-lookup"><span data-stu-id="b063f-108">Product</span></span>](#Product)  
  
> [!NOTE]
>  <span data-ttu-id="b063f-109">Standard exempelmallarna ingår i hello följande dokumentation, men är ämne toochange på grund av toocontinuous förbättringar.</span><span class="sxs-lookup"><span data-stu-id="b063f-109">Sample default templates are included in hello following documentation, but are subject toochange due toocontinuous improvements.</span></span> <span data-ttu-id="b063f-110">Du kan visa hello live standardmallarna i hello developer-portalen genom att gå toohello önskad enskilda mallar.</span><span class="sxs-lookup"><span data-stu-id="b063f-110">You can view hello live default templates in hello developer portal by navigating toohello desired individual templates.</span></span> <span data-ttu-id="b063f-111">Mer information om hur du arbetar med mallar finns [hur toocustomize hello API Management developer-portalen med hjälp av mallar](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="b063f-111">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="b063f-112"><a name="ProductList"></a>Produktlista</span><span class="sxs-lookup"><span data-stu-id="b063f-112"><a name="ProductList"></a> Product list</span></span>  
 <span data-ttu-id="b063f-113">Hej **produktlista** mall kan du toocustomize hello brödtext hello produktsidan listan i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="b063f-113">hello **Product list** template allows you toocustomize hello body of hello product list page in hello developer portal.</span></span>  
  
 <span data-ttu-id="b063f-114">![Lista över produkter](./media/api-management-product-templates/APIM_ProductsListTemplatePage.png "APIM_ProductsListTemplatePage")</span><span class="sxs-lookup"><span data-stu-id="b063f-114">![Products list](./media/api-management-product-templates/APIM_ProductsListTemplatePage.png "APIM_ProductsListTemplatePage")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="b063f-115">Standardmall</span><span class="sxs-lookup"><span data-stu-id="b063f-115">Default template</span></span>  
  
```xml  
<search-control></search-control>  
<div class="row">  
    <div class="col-md-9">  
        <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>  
    </div>  
</div>  
<div class="row">  
    <div class="col-md-12">  
    {% if products.size > 0 %}  
    <ul class="list-unstyled">  
    {% for product in products %}  
        <li>  
            <h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>  
            {{product.description}}  
        </li>     
    {% endfor %}  
    </ul>  
    <paging-control></paging-control>  
    {% else %}  
    {% localized "CommonResources|NoItemsToDisplay" %}  
    {% endif %}  
    </div>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="b063f-116">Kontroller</span><span class="sxs-lookup"><span data-stu-id="b063f-116">Controls</span></span>  
 <span data-ttu-id="b063f-117">Hej `Product list` mall kan använda följande hello [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="b063f-117">hello `Product list` template may use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="b063f-118">växlingsfil-kontroll</span><span class="sxs-lookup"><span data-stu-id="b063f-118">paging-control</span></span>](api-management-page-controls.md#paging-control)  
  
-   [<span data-ttu-id="b063f-119">Sök-kontroll</span><span class="sxs-lookup"><span data-stu-id="b063f-119">search-control</span></span>](api-management-page-controls.md#search-control)  
  
### <a name="data-model"></a><span data-ttu-id="b063f-120">Datamodell</span><span class="sxs-lookup"><span data-stu-id="b063f-120">Data model</span></span>  
  
|<span data-ttu-id="b063f-121">Egenskap</span><span class="sxs-lookup"><span data-stu-id="b063f-121">Property</span></span>|<span data-ttu-id="b063f-122">Typ</span><span class="sxs-lookup"><span data-stu-id="b063f-122">Type</span></span>|<span data-ttu-id="b063f-123">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b063f-123">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="b063f-124">Sidindelning</span><span class="sxs-lookup"><span data-stu-id="b063f-124">Paging</span></span>|<span data-ttu-id="b063f-125">[Växling](api-management-template-data-model-reference.md#Paging) entitet.</span><span class="sxs-lookup"><span data-stu-id="b063f-125">[Paging](api-management-template-data-model-reference.md#Paging) entity.</span></span>|<span data-ttu-id="b063f-126">hello sidindelning information för hello produkter samling.</span><span class="sxs-lookup"><span data-stu-id="b063f-126">hello paging information for hello products collection.</span></span>|  
|<span data-ttu-id="b063f-127">Filtrering</span><span class="sxs-lookup"><span data-stu-id="b063f-127">Filtering</span></span>|<span data-ttu-id="b063f-128">[Filtrering](api-management-template-data-model-reference.md#Filtering) entitet.</span><span class="sxs-lookup"><span data-stu-id="b063f-128">[Filtering](api-management-template-data-model-reference.md#Filtering) entity.</span></span>|<span data-ttu-id="b063f-129">hello filtrering information för hello-produkter listas sidan.</span><span class="sxs-lookup"><span data-stu-id="b063f-129">hello filtering information for hello products list page.</span></span>|  
|<span data-ttu-id="b063f-130">Produkter</span><span class="sxs-lookup"><span data-stu-id="b063f-130">Products</span></span>|<span data-ttu-id="b063f-131">Samling av [produkten](api-management-template-data-model-reference.md#Product) entiteter.</span><span class="sxs-lookup"><span data-stu-id="b063f-131">Collection of [Product](api-management-template-data-model-reference.md#Product) entities.</span></span>|<span data-ttu-id="b063f-132">hello produkter synliga toohello aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="b063f-132">hello products visible toohello current user.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="b063f-133">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="b063f-133">Sample template data</span></span>  
  
```json  
{  
    "Paging": {  
        "Page": 1,  
        "PageSize": 10,  
        "TotalItemCount": 2,  
        "ShowAll": false,  
        "PageCount": 1  
    },  
    "Filtering": {  
        "Pattern": null,  
        "Placeholder": "Search products"  
    },  
    "Products": [  
        {  
            "Id": "56f9445ffaf7560049060001",  
            "Title": "Starter",  
            "Description": "Subscribers will be able toorun 5 calls/minute up tooa maximum of 100 calls/week.",  
            "Terms": "",  
            "ProductState": 1,  
            "AllowMultipleSubscriptions": false,  
            "MultipleSubscriptionsCount": 1  
        },  
        {  
            "Id": "56f9445ffaf7560049060002",  
            "Title": "Unlimited",  
            "Description": "Subscribers have completely unlimited access toohello API. Administrator approval is required.",  
            "Terms": null,  
            "ProductState": 1,  
            "AllowMultipleSubscriptions": false,  
            "MultipleSubscriptionsCount": 1  
        }  
    ]  
}  
```  
  
##  <span data-ttu-id="b063f-134"><a name="Product"></a>Produkten</span><span class="sxs-lookup"><span data-stu-id="b063f-134"><a name="Product"></a> Product</span></span>  
 <span data-ttu-id="b063f-135">Hej **produkten** mall kan du toocustomize hello brödtext hello produktsidan i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="b063f-135">hello **Product** template allows you toocustomize hello body of hello product page in hello developer portal.</span></span>  
  
 <span data-ttu-id="b063f-136">![Developer portal produktsidan](./media/api-management-product-templates/APIM_ProductPage.png "APIM_ProductPage")</span><span class="sxs-lookup"><span data-stu-id="b063f-136">![Developer portal product page](./media/api-management-product-templates/APIM_ProductPage.png "APIM_ProductPage")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="b063f-137">Standardmall</span><span class="sxs-lookup"><span data-stu-id="b063f-137">Default template</span></span>  
  
```xml  
<h2>{{Product.Title}}</h2>  
<p>{{Product.Description}}</p>  
  
{% assign replaceString0 = '{0}' %}  
  
{% if Limits and Limits.size > 0 %}  
<h3>{% localized "ProductDetailsStrings|WebProductsUsageLimitsHeader"%}</h3>  
<ul>  
  {% for limit in Limits %}  
  <li>{{limit.DisplayName}}</li>  
  {% endfor %}  
</ul>  
{% endif %}  
  
{% if apis.size > 0 %}  
<p>  
  <b>  
    {% if apis.size == 1 %}  
    {% capture apisCountText %}{% localized "ProductDetailsStrings|TextblockSingleApisCount" %}{% endcapture %}  
    {% else %}  
    {% capture apisCountText %}{% localized "ProductDetailsStrings|TextblockMultipleApisCount" %}{% endcapture %}  
    {% endif %}  
  
    {% capture apisCount %}{{apis.size}}{% endcapture %}  
    {{ apisCountText | replace : replaceString0, apisCount }}  
  </b>  
</p>  
  
<ul>  
  {% for api in Apis %}  
  <li>  
    <a href="/docs/services/{{api.Id}}">{{api.Name}}</a>  
  </li>  
  {% endfor %}  
</ul>  
{% endif %}  
  
{% if subscriptions.size > 0 %}  
<p>  
  <b>  
    {% if subscriptions.size == 1 %}  
    {% capture subscriptionsCountText %}{% localized "ProductDetailsStrings|TextblockSingleSubscriptionsCount" %}{% endcapture %}  
    {% else %}  
    {% capture subscriptionsCountText %}{% localized "ProductDetailsStrings|TextblockMultipleSubscriptionsCount" %}{% endcapture %}  
    {% endif %}  
  
    {% capture subscriptionsCount %}{{subscriptions.size}}{% endcapture %}  
    {{ subscriptionsCountText | replace : replaceString0, subscriptionsCount }}  
  </b>  
</p>  
  
<ul>  
  {% for subscription in subscriptions %}  
  <li>  
    <a href="/developer#{{subscription.Id}}">{{subscription.DisplayName}}</a>  
  </li>  
  {% endfor %}  
</ul>  
{% endif %}  
{% if CannotAddBecauseSubscriptionNumberLimitReached %}  
<b>{% localized "ProductDetailsStrings|TextblockSubscriptionLimitReached" %}</b>  
{% elsif CannotAddBecauseMultipleSubscriptionsNotAllowed == false %}  
<subscribe-button></subscribe-button>  
{% endif %}  
```  
  
### <a name="controls"></a><span data-ttu-id="b063f-138">Kontroller</span><span class="sxs-lookup"><span data-stu-id="b063f-138">Controls</span></span>  
 <span data-ttu-id="b063f-139">Hej `Product list` mall kan använda följande hello [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="b063f-139">hello `Product list` template may use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="b063f-140">prenumerera på knappen</span><span class="sxs-lookup"><span data-stu-id="b063f-140">subscribe-button</span></span>](api-management-page-controls.md#subscribe-button)  
  
### <a name="data-model"></a><span data-ttu-id="b063f-141">Datamodell</span><span class="sxs-lookup"><span data-stu-id="b063f-141">Data model</span></span>  
  
|<span data-ttu-id="b063f-142">Egenskap</span><span class="sxs-lookup"><span data-stu-id="b063f-142">Property</span></span>|<span data-ttu-id="b063f-143">Typ</span><span class="sxs-lookup"><span data-stu-id="b063f-143">Type</span></span>|<span data-ttu-id="b063f-144">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b063f-144">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="b063f-145">Produkt</span><span class="sxs-lookup"><span data-stu-id="b063f-145">Product</span></span>|[<span data-ttu-id="b063f-146">Produkten</span><span class="sxs-lookup"><span data-stu-id="b063f-146">Product</span></span>](api-management-template-data-model-reference.md#Product)|<span data-ttu-id="b063f-147">hello angiven produkt.</span><span class="sxs-lookup"><span data-stu-id="b063f-147">hello specified product.</span></span>|  
|<span data-ttu-id="b063f-148">IsDeveloperSubscribed</span><span class="sxs-lookup"><span data-stu-id="b063f-148">IsDeveloperSubscribed</span></span>|<span data-ttu-id="b063f-149">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="b063f-149">boolean</span></span>|<span data-ttu-id="b063f-150">Om hello aktuell användare är prenumererade toothis produkten.</span><span class="sxs-lookup"><span data-stu-id="b063f-150">Whether hello current user is subscribed toothis product.</span></span>|  
|<span data-ttu-id="b063f-151">SubscriptionState</span><span class="sxs-lookup"><span data-stu-id="b063f-151">SubscriptionState</span></span>|<span data-ttu-id="b063f-152">Antal</span><span class="sxs-lookup"><span data-stu-id="b063f-152">number</span></span>|<span data-ttu-id="b063f-153">hello tillståndet för hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b063f-153">hello state of hello subscription.</span></span> <span data-ttu-id="b063f-154">Möjliga tillstånd är:</span><span class="sxs-lookup"><span data-stu-id="b063f-154">Possible states are:</span></span><br /><br /> <span data-ttu-id="b063f-155">-   `0 - suspended`– hello prenumeration är blockerat och hello prenumeranten kan inte anropa alla API: er för hello produkten.</span><span class="sxs-lookup"><span data-stu-id="b063f-155">-   `0 - suspended` – hello subscription is blocked, and hello subscriber cannot call any APIs of hello product.</span></span><br /><span data-ttu-id="b063f-156">-   `1 - active`– hello prenumeration är aktiv.</span><span class="sxs-lookup"><span data-stu-id="b063f-156">-   `1 - active` – hello subscription is active.</span></span><br /><span data-ttu-id="b063f-157">-   `2 - expired`– hello prenumerationen uppnåtts dess förfallodatum och har inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="b063f-157">-   `2 - expired` – hello subscription reached its expiration date and was deactivated.</span></span><br /><span data-ttu-id="b063f-158">-   `3 - submitted`– hello prenumerationsbegäran har gjorts av hello utvecklare, men har ännu inte godkänts eller avvisats.</span><span class="sxs-lookup"><span data-stu-id="b063f-158">-   `3 - submitted` – hello subscription request has been made by hello developer, but has not yet been approved or rejected.</span></span><br /><span data-ttu-id="b063f-159">-   `4 - rejected`– hello prenumerationsbegäran har nekats av en administratör.</span><span class="sxs-lookup"><span data-stu-id="b063f-159">-   `4 - rejected` – hello subscription request has been denied by an administrator.</span></span><br /><span data-ttu-id="b063f-160">-   `5 - cancelled`– hello prenumerationen har avbrutits av hello utvecklare eller administratör.</span><span class="sxs-lookup"><span data-stu-id="b063f-160">-   `5 - cancelled` – hello subscription has been cancelled by hello developer or administrator.</span></span>|  
|<span data-ttu-id="b063f-161">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="b063f-161">Limits</span></span>|<span data-ttu-id="b063f-162">matris</span><span class="sxs-lookup"><span data-stu-id="b063f-162">array</span></span>|<span data-ttu-id="b063f-163">Den här egenskapen är inaktuell och bör inte användas.</span><span class="sxs-lookup"><span data-stu-id="b063f-163">This property is deprecated and should not be used.</span></span>|  
|<span data-ttu-id="b063f-164">DelegatedSubscriptionEnabled</span><span class="sxs-lookup"><span data-stu-id="b063f-164">DelegatedSubscriptionEnabled</span></span>|<span data-ttu-id="b063f-165">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="b063f-165">boolean</span></span>|<span data-ttu-id="b063f-166">Om [delegering](https://azure.microsoft.com/documentation/articles/api-management-howto-setup-delegation/) har aktiverats för den här prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="b063f-166">Whether [delegation](https://azure.microsoft.com/documentation/articles/api-management-howto-setup-delegation/) is enabled for this subscription.</span></span>|  
|<span data-ttu-id="b063f-167">DelegatedSubscriptionUrl</span><span class="sxs-lookup"><span data-stu-id="b063f-167">DelegatedSubscriptionUrl</span></span>|<span data-ttu-id="b063f-168">Sträng</span><span class="sxs-lookup"><span data-stu-id="b063f-168">string</span></span>|<span data-ttu-id="b063f-169">Om delegering är aktiverat delegerade hello prenumeration URL.</span><span class="sxs-lookup"><span data-stu-id="b063f-169">If delegation is enabled, hello delegated subscription URL.</span></span>|  
|<span data-ttu-id="b063f-170">IsAgreed</span><span class="sxs-lookup"><span data-stu-id="b063f-170">IsAgreed</span></span>|<span data-ttu-id="b063f-171">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="b063f-171">boolean</span></span>|<span data-ttu-id="b063f-172">Om hello produkten har villkoren, om hello aktuell användare har gått med toohello villkor.</span><span class="sxs-lookup"><span data-stu-id="b063f-172">If hello product has terms, whether hello current user has agreed toohello terms.</span></span>|  
|<span data-ttu-id="b063f-173">Prenumerationer</span><span class="sxs-lookup"><span data-stu-id="b063f-173">Subscriptions</span></span>|<span data-ttu-id="b063f-174">Samling av [prenumeration sammanfattning](api-management-template-data-model-reference.md#SubscriptionSummary) entiteter.</span><span class="sxs-lookup"><span data-stu-id="b063f-174">Collection of [Subscription summary](api-management-template-data-model-reference.md#SubscriptionSummary) entities.</span></span>|<span data-ttu-id="b063f-175">hello prenumerationer toohello produkt.</span><span class="sxs-lookup"><span data-stu-id="b063f-175">hello subscriptions toohello product.</span></span>|  
|<span data-ttu-id="b063f-176">API: er</span><span class="sxs-lookup"><span data-stu-id="b063f-176">Apis</span></span>|<span data-ttu-id="b063f-177">Samling av [API](api-management-template-data-model-reference.md#API) entiteter.</span><span class="sxs-lookup"><span data-stu-id="b063f-177">Collection of [API](api-management-template-data-model-reference.md#API) entities.</span></span>|<span data-ttu-id="b063f-178">hello API: er i den här produkten.</span><span class="sxs-lookup"><span data-stu-id="b063f-178">hello APIs in this product.</span></span>|  
|<span data-ttu-id="b063f-179">CannotAddBecauseSubscriptionNumberLimitReached</span><span class="sxs-lookup"><span data-stu-id="b063f-179">CannotAddBecauseSubscriptionNumberLimitReached</span></span>|<span data-ttu-id="b063f-180">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="b063f-180">boolean</span></span>|<span data-ttu-id="b063f-181">Om hello aktuell användare är berättigad toosubscribe toothis produkt med beaktande toohello gränsen.</span><span class="sxs-lookup"><span data-stu-id="b063f-181">Whether hello current user is eligible toosubscribe toothis product with regard toohello subscription limit.</span></span>|  
|<span data-ttu-id="b063f-182">CannotAddBecauseMultipleSubscriptionsNotAllowed</span><span class="sxs-lookup"><span data-stu-id="b063f-182">CannotAddBecauseMultipleSubscriptionsNotAllowed</span></span>|<span data-ttu-id="b063f-183">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="b063f-183">boolean</span></span>|<span data-ttu-id="b063f-184">Om hello aktuell användare är berättigad toosubscribe toothis produkt med beaktande toomultiple prenumerationer tillåten eller inte.</span><span class="sxs-lookup"><span data-stu-id="b063f-184">Whether hello current user is eligible toosubscribe toothis product with regard toomultiple subscriptions being allowed or not.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="b063f-185">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="b063f-185">Sample template data</span></span>  
  
```json  
{  
    "Product": {  
        "Id": "56f9445ffaf7560049060001",  
        "Title": "Starter",  
        "Description": "Subscribers will be able toorun 5 calls/minute up tooa maximum of 100 calls/week.",  
        "Terms": "",  
        "ProductState": 1,  
        "AllowMultipleSubscriptions": false,  
        "MultipleSubscriptionsCount": 1  
    },  
    "IsDeveloperSubscribed": true,  
    "SubscriptionState": 1,  
    "Limits": [],  
    "DelegatedSubscriptionEnabled": false,  
    "DelegatedSubscriptionUrl": null,  
    "IsAgreed": false,  
    "Subscriptions": [  
        {  
            "Id": "56f9445ffaf7560049070001",  
            "DisplayName": "Starter  (default)"  
        }  
    ],  
    "Apis": [  
        {  
            "id": "56f9445ffaf7560049040001",  
            "name": "Echo API",  
            "description": null,  
            "serviceUrl": "http://echoapi.cloudapp.net/api",  
            "path": "echo",  
            "protocols": [  
                2  
            ],  
            "authenticationSettings": null,  
            "subscriptionKeyParameterNames": null  
        }  
    ],  
    "CannotAddBecauseSubscriptionNumberLimitReached": false,  
    "CannotAddBecauseMultipleSubscriptionsNotAllowed": true  
}  
```

## <a name="next-steps"></a><span data-ttu-id="b063f-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b063f-186">Next steps</span></span>
<span data-ttu-id="b063f-187">Mer information om hur du arbetar med mallar finns [hur toocustomize hello API Management developer-portalen med hjälp av mallar](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b063f-187">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>
