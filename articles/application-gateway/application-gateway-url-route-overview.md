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
# <a name="url-path-based-routing-overview"></a>Översikt över URL-sökvägsbaserad routning

URL-sökväg baserad routning kan du tooroute trafik tooback slutpunkt serverpooler baserat på URL-sökvägar för hello-begäran. 

Ett av scenarierna för hello är tooroute begäranden för olika typer av innehåll toodifferent serverdelspooler server.

I följande exempel hello, Programgateway fungerar trafik för contoso.com från poolen för tre backend-server till exempel: VideoServerPool, ImageServerPool och DefaultServerPool.

![imageURLroute](./media/application-gateway-url-route-overview/figure1.png)

Begäranden för http://contoso.com/video * är routade tooVideoServerPool och http://contoso.com/images * är routade tooImageServerPool. DefaultServerPool är markerad om inga mönster för hello sökvägen stämmer.

> [!IMPORTANT]
> Regler bearbetas i hello ordning de anges i hello-portalen. Det är första tidigare tooconfiguring för rekommenderas tooconfigure flera platser lyssnare en grundläggande lyssnare.  Detta säkerställer att trafik hämtar routade toohello tillbaka avslutas. Om en grundläggande lyssnare visas först och matchar en inkommande begäran kommer den att bearbetas av den lyssnaren.

## <a name="urlpathmap-configuration-element"></a>UrlPathMap-konfigurationselementet

hello urlPathMap elementet har använt toospecify sökväg mönster tooback slutpunkt server pool mappningar. hello är följande kodexempel hello fragment av urlPathMap element från mallen.

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
> PathPattern: Den här inställningen är en lista över sökvägen mönster toomatch. Var och en måste börja med / och hello endast placera en ”*” tillåts på hello slutpunkt följer ett ”/”. hello strängen matas toohello sökväg matcher inte innehåller någon text efter hello först? eller #, och dessa tecken tillåts inte här.

Du kan kolla en [Resource Manager-mall med URL-baserad routning](https://azure.microsoft.com/documentation/templates/201-application-gateway-url-path-based-routing) för mer information.

## <a name="pathbasedrouting-rule"></a>PathBasedRouting-regeln

RequestRoutingRule av typen PathBasedRouting är används toobind en lyssnare tooa urlPathMap. Alla begäranden som tas emot för den här lyssnaren dirigeras baserat på principen som anges i urlPathMap.
Utdrag från PathBasedRouting-regeln:

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

## <a name="next-steps"></a>Nästa steg

När du lära dig mer om URL-baserade innehåll routning, gå för[skapa en Programgateway med hjälp av URL-baserade routning](application-gateway-create-url-route-portal.md) toocreate en Programgateway med routningsregler för URL: en.
