---
title: "aaaURL-baserat innehåll routning: översikt | Microsoft Docs"
description: "Den här sidan innehåller en översikt över hello URL till Gateway-baserat innehåll routning, UrlPathMap konfiguration och PathBasedRouting regeln."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: 
ms.assetid: 4409159b-e22d-4c9a-a103-f5d32465d163
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2017
ms.author: gwallace
ms.openlocfilehash: 5094b42625baffeb395beace68db0d269e46080c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="url-path-based-routing-overview"></a><span data-ttu-id="a3509-103">Översikt över URL-sökvägsbaserad routning</span><span class="sxs-lookup"><span data-stu-id="a3509-103">URL Path Based Routing overview</span></span>

<span data-ttu-id="a3509-104">URL-sökväg baserad routning kan du tooroute trafik tooback slutpunkt serverpooler baserat på URL-sökvägar för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="a3509-104">URL Path Based Routing allows you tooroute traffic tooback-end server pools based on URL Paths of hello request.</span></span> 

<span data-ttu-id="a3509-105">Ett av scenarierna för hello är tooroute begäranden för olika typer av innehåll toodifferent serverdelspooler server.</span><span class="sxs-lookup"><span data-stu-id="a3509-105">One of hello scenarios is tooroute requests for different content types toodifferent backend server pools.</span></span>

<span data-ttu-id="a3509-106">I följande exempel hello, Programgateway fungerar trafik för contoso.com från poolen för tre backend-server till exempel: VideoServerPool, ImageServerPool och DefaultServerPool.</span><span class="sxs-lookup"><span data-stu-id="a3509-106">In hello following example, Application Gateway is serving traffic for contoso.com from three back-end server pools for example: VideoServerPool, ImageServerPool, and DefaultServerPool.</span></span>

![imageURLroute](./media/application-gateway-url-route-overview/figure1.png)

<span data-ttu-id="a3509-108">Begäranden för http://contoso.com/video * är routade tooVideoServerPool och http://contoso.com/images * är routade tooImageServerPool.</span><span class="sxs-lookup"><span data-stu-id="a3509-108">Requests for http://contoso.com/video* are routed tooVideoServerPool, and http://contoso.com/images* are routed tooImageServerPool.</span></span> <span data-ttu-id="a3509-109">DefaultServerPool är markerad om inga mönster för hello sökvägen stämmer.</span><span class="sxs-lookup"><span data-stu-id="a3509-109">DefaultServerPool is selected if none of hello path patterns match.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a3509-110">Regler bearbetas i hello ordning de anges i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="a3509-110">Rules are processed in hello order they are listed in hello portal.</span></span> <span data-ttu-id="a3509-111">Det är första tidigare tooconfiguring för rekommenderas tooconfigure flera platser lyssnare en grundläggande lyssnare.</span><span class="sxs-lookup"><span data-stu-id="a3509-111">It is highly recommended tooconfigure multi-site listeners first prior tooconfiguring a basic listener.</span></span>  <span data-ttu-id="a3509-112">Detta säkerställer att trafik hämtar routade toohello tillbaka avslutas.</span><span class="sxs-lookup"><span data-stu-id="a3509-112">This ensures that traffic gets routed toohello right back end.</span></span> <span data-ttu-id="a3509-113">Om en grundläggande lyssnare visas först och matchar en inkommande begäran kommer den att bearbetas av den lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="a3509-113">If a basic listener is listed first and matches an incoming request, it gets processed by that listener.</span></span>

## <a name="urlpathmap-configuration-element"></a><span data-ttu-id="a3509-114">UrlPathMap-konfigurationselementet</span><span class="sxs-lookup"><span data-stu-id="a3509-114">UrlPathMap configuration element</span></span>

<span data-ttu-id="a3509-115">hello urlPathMap elementet har använt toospecify sökväg mönster tooback slutpunkt server pool mappningar.</span><span class="sxs-lookup"><span data-stu-id="a3509-115">hello urlPathMap element is used toospecify Path patterns tooback-end server pool mappings.</span></span> <span data-ttu-id="a3509-116">hello är följande kodexempel hello fragment av urlPathMap element från mallen.</span><span class="sxs-lookup"><span data-stu-id="a3509-116">hello following code example is hello snippet of urlPathMap element from template file.</span></span>

```json
"urlPathMaps": [{
    "name": "{urlpathMapName}",
    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/urlPathMaps/{urlpathMapName}",
    "properties": {
        "defaultBackendAddressPool": {
            "id": "/subscriptions/    {subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendAddressPools/{poolName1}"
        },
        "defaultBackendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendHttpSettingsList/{settingname1}"
        },
        "pathRules": [{
            "name": "{pathRuleName}",
            "properties": {
                "paths": [
                    "{pathPattern}"
                ],
                "backendAddressPool": {
                    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendAddressPools/{poolName2}"
                },
                "backendHttpsettings": {
                    "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/backendHttpsettingsList/{settingName2}"
                }
            }
        }]
    }
}]
```

> [!NOTE]
> <span data-ttu-id="a3509-117">PathPattern: Den här inställningen är en lista över sökvägen mönster toomatch.</span><span class="sxs-lookup"><span data-stu-id="a3509-117">PathPattern: This setting is a list of path patterns toomatch.</span></span> <span data-ttu-id="a3509-118">Var och en måste börja med / och hello endast placera en ”*” tillåts på hello slutpunkt följer ett ”/”.</span><span class="sxs-lookup"><span data-stu-id="a3509-118">Each must start with / and hello only place a "*" is allowed is at hello end following a "/."</span></span> <span data-ttu-id="a3509-119">hello strängen matas toohello sökväg matcher inte innehåller någon text efter hello först? eller #, och dessa tecken tillåts inte här.</span><span class="sxs-lookup"><span data-stu-id="a3509-119">hello string fed toohello path matcher does not include any text after hello first? or #, and those chars are not allowed here.</span></span>

<span data-ttu-id="a3509-120">Du kan kolla en [Resource Manager-mall med URL-baserad routning](https://azure.microsoft.com/documentation/templates/201-application-gateway-url-path-based-routing) för mer information.</span><span class="sxs-lookup"><span data-stu-id="a3509-120">You can check out a [Resource Manager template using URL-based routing](https://azure.microsoft.com/documentation/templates/201-application-gateway-url-path-based-routing) for more information.</span></span>

## <a name="pathbasedrouting-rule"></a><span data-ttu-id="a3509-121">PathBasedRouting-regeln</span><span class="sxs-lookup"><span data-stu-id="a3509-121">PathBasedRouting rule</span></span>

<span data-ttu-id="a3509-122">RequestRoutingRule av typen PathBasedRouting är används toobind en lyssnare tooa urlPathMap.</span><span class="sxs-lookup"><span data-stu-id="a3509-122">RequestRoutingRule of type PathBasedRouting is used toobind a listener tooa urlPathMap.</span></span> <span data-ttu-id="a3509-123">Alla begäranden som tas emot för den här lyssnaren dirigeras baserat på principen som anges i urlPathMap.</span><span class="sxs-lookup"><span data-stu-id="a3509-123">All requests that are received for this listener are routed based on policy specified in urlPathMap.</span></span>
<span data-ttu-id="a3509-124">Utdrag från PathBasedRouting-regeln:</span><span class="sxs-lookup"><span data-stu-id="a3509-124">Snippet of PathBasedRouting rule:</span></span>

```json
"requestRoutingRules": [
    {

"name": "{ruleName}",
"id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/requestRoutingRules/{ruleName}",
"properties": {
    "ruleType": "PathBasedRouting",
    "httpListener": {
        "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/httpListeners/<listenerName>"
    },
    "urlPathMap": {
        "id": "/subscriptions/{subscriptionId}/../microsoft.network/applicationGateways/{gatewayName}/ urlPathMaps/{urlpathMapName}"
    },

}
    }
]
```

## <a name="next-steps"></a><span data-ttu-id="a3509-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a3509-125">Next steps</span></span>

<span data-ttu-id="a3509-126">När du lära dig mer om URL-baserade innehåll routning, gå för[skapa en Programgateway med hjälp av URL-baserade routning](application-gateway-create-url-route-portal.md) toocreate en Programgateway med routningsregler för URL: en.</span><span class="sxs-lookup"><span data-stu-id="a3509-126">After learning about URL-based content routing, go too[create an application gateway using URL-based routing](application-gateway-create-url-route-portal.md) toocreate an application gateway with URL routing rules.</span></span>
