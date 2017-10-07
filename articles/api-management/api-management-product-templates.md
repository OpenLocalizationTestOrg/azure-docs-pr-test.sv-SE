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
# <a name="product-templates-in-azure-api-management"></a>Produktmallar i Azure API Management
Azure API Management ger du hello möjlighet toocustomize hello innehållet i developer portalens sidor med hjälp av en uppsättning mallar som konfigurerar deras innehåll. Med hjälp av [DotLiquid](http://dotliquidmarkup.org/) syntax och hello redigeringsprogram, t.ex [DotLiquid för Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), och en angiven uppsättning lokaliserade [String resurser](api-management-template-resources.md#strings), [ Glyf resurser](api-management-template-resources.md#glyphs), och [sidan kontroller](api-management-page-controls.md), du har stor flexibilitet tooconfigure hello innehåll hello sidor som du vill använda dessa mallar.  
  
 hello mallar i det här avsnittet kan du toocustomize hello innehåll hello produkten sidor i hello developer-portalen.  
  
-   [Produktlista](#ProductList)  
  
-   [Produkten](#Product)  
  
> [!NOTE]
>  Standard exempelmallarna ingår i hello följande dokumentation, men är ämne toochange på grund av toocontinuous förbättringar. Du kan visa hello live standardmallarna i hello developer-portalen genom att gå toohello önskad enskilda mallar. Mer information om hur du arbetar med mallar finns [hur toocustomize hello API Management developer-portalen med hjälp av mallar](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  
  
##  <a name="ProductList"></a>Produktlista  
 Hej **produktlista** mall kan du toocustomize hello brödtext hello produktsidan listan i hello developer-portalen.  
  
 ![Lista över produkter](./media/api-management-product-templates/APIM_ProductsListTemplatePage.png "APIM_ProductsListTemplatePage")  
  
### <a name="default-template"></a>Standardmall  
  
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
  
### <a name="controls"></a>Kontroller  
 Hej `Product list` mall kan använda följande hello [sidan kontroller](api-management-page-controls.md).  
  
-   [växlingsfil-kontroll](api-management-page-controls.md#paging-control)  
  
-   [Sök-kontroll](api-management-page-controls.md#search-control)  
  
### <a name="data-model"></a>Datamodell  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|Sidindelning|[Växling](api-management-template-data-model-reference.md#Paging) entitet.|hello sidindelning information för hello produkter samling.|  
|Filtrering|[Filtrering](api-management-template-data-model-reference.md#Filtering) entitet.|hello filtrering information för hello-produkter listas sidan.|  
|Produkter|Samling av [produkten](api-management-template-data-model-reference.md#Product) entiteter.|hello produkter synliga toohello aktuella användaren.|  
  
### <a name="sample-template-data"></a>Mallen exempeldata  
  
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
  
##  <a name="Product"></a>Produkten  
 Hej **produkten** mall kan du toocustomize hello brödtext hello produktsidan i hello developer-portalen.  
  
 ![Developer portal produktsidan](./media/api-management-product-templates/APIM_ProductPage.png "APIM_ProductPage")  
  
### <a name="default-template"></a>Standardmall  
  
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
  
### <a name="controls"></a>Kontroller  
 Hej `Product list` mall kan använda följande hello [sidan kontroller](api-management-page-controls.md).  
  
-   [prenumerera på knappen](api-management-page-controls.md#subscribe-button)  
  
### <a name="data-model"></a>Datamodell  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|Produkt|[Produkten](api-management-template-data-model-reference.md#Product)|hello angiven produkt.|  
|IsDeveloperSubscribed|Booleskt värde|Om hello aktuell användare är prenumererade toothis produkten.|  
|SubscriptionState|Antal|hello tillståndet för hello prenumeration. Möjliga tillstånd är:<br /><br /> -   `0 - suspended`– hello prenumeration är blockerat och hello prenumeranten kan inte anropa alla API: er för hello produkten.<br />-   `1 - active`– hello prenumeration är aktiv.<br />-   `2 - expired`– hello prenumerationen uppnåtts dess förfallodatum och har inaktiverats.<br />-   `3 - submitted`– hello prenumerationsbegäran har gjorts av hello utvecklare, men har ännu inte godkänts eller avvisats.<br />-   `4 - rejected`– hello prenumerationsbegäran har nekats av en administratör.<br />-   `5 - cancelled`– hello prenumerationen har avbrutits av hello utvecklare eller administratör.|  
|Begränsningar|matris|Den här egenskapen är inaktuell och bör inte användas.|  
|DelegatedSubscriptionEnabled|Booleskt värde|Om [delegering](https://azure.microsoft.com/documentation/articles/api-management-howto-setup-delegation/) har aktiverats för den här prenumerationen.|  
|DelegatedSubscriptionUrl|Sträng|Om delegering är aktiverat delegerade hello prenumeration URL.|  
|IsAgreed|Booleskt värde|Om hello produkten har villkoren, om hello aktuell användare har gått med toohello villkor.|  
|Prenumerationer|Samling av [prenumeration sammanfattning](api-management-template-data-model-reference.md#SubscriptionSummary) entiteter.|hello prenumerationer toohello produkt.|  
|API: er|Samling av [API](api-management-template-data-model-reference.md#API) entiteter.|hello API: er i den här produkten.|  
|CannotAddBecauseSubscriptionNumberLimitReached|Booleskt värde|Om hello aktuell användare är berättigad toosubscribe toothis produkt med beaktande toohello gränsen.|  
|CannotAddBecauseMultipleSubscriptionsNotAllowed|Booleskt värde|Om hello aktuell användare är berättigad toosubscribe toothis produkt med beaktande toomultiple prenumerationer tillåten eller inte.|  
  
### <a name="sample-template-data"></a>Mallen exempeldata  
  
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

## <a name="next-steps"></a>Nästa steg
Mer information om hur du arbetar med mallar finns [hur toocustomize hello API Management developer-portalen med hjälp av mallar](api-management-developer-portal-templates.md).
