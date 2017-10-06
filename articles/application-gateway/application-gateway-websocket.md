---
title: "aaaWebSocket stöd i Azure Programgateway | Microsoft Docs"
description: "Den här sidan innehåller en översikt över hello programmet Gateway WebSocket-stöd."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 8968dac1-e9bc-4fa1-8415-96decacab83f
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/08/2017
ms.author: amsriva
ms.openlocfilehash: 3776117803e8559ad243c2d4c3dd661199c1e48a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-websocket-support-in-application-gateway"></a>Översikt över WebSocket-stöd i Programgateway

Application Gateway har inbyggt stöd för WebSocket över alla gateway-storlekar. Det finns ingen inställning som kan konfigureras av användaren tooselectively aktivera eller inaktivera WebSocket-stöd. 

WebSocket-protokollet är standardiserade i [RFC6455](https://tools.ietf.org/html/rfc6455) aktiverar en fullständig duplex kommunikation mellan en server och en klient över en tidskrävande TCP-anslutning. Den här funktionen tillåter en mer interaktiva kommunikation mellan hello webbservern och hello-klienten, vilket kan vara dubbelriktad utan hello krävs för avsökning som krävs i HTTP-baserade implementeringar. WebSocket har lite extra till skillnad från HTTP och kan återanvändning hello samma TCP-anslutning för flera/svar ledde till ett mer effektivt utnyttjande av resurser. WebSocket-protokoll är utformad toowork via traditionella HTTP-portarna 80 och 443.

Du kan fortsätta att använda en standard HTTP-lyssnaren på port 80 eller 443 tooreceive WebSocket-trafik. WebSocket-trafik är riktat toohello WebSocket aktiverat serverdelen med hello lämpliga serverdelspool som anges i programmet gateway regler. hello backend-servern måste svara toohello programmet gateway avsökningar, som beskrivs i hello [hälsa avsökningen översikt](application-gateway-probe-overview.md) avsnitt. Programmet gateway hälsoavsökningar är endast HTTP/HTTPS. Varje backend-servern måste svara tooHTTP avsökningar för gateway tooroute WebSocket trafik toohello programserver.

## <a name="listener-configuration-element"></a>Listener-konfigurationselementet

En befintlig HTTP-lyssnare kan vara används toosupport WebSocket-trafik. hello följande är ett fragment av ett httpListeners element från en exempelfil för mallen. Du måste både HTTP och HTTPS-lyssnare toosupport WebSocket och säker WebSocket-trafik. På liknande sätt kan du använda hello [portal](application-gateway-create-gateway-portal.md) eller [PowerShell](application-gateway-create-gateway-arm.md) toocreate en Programgateway med lyssnare på port 80/443 toosupport WebSocket-trafik.

```json
"httpListeners": [
        {
            "name": "appGatewayHttpsListener",
            "properties": {
                "FrontendIPConfiguration": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendIPConfigurations/DefaultFrontendPublicIP"
                },
                "FrontendPort": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendPorts/appGatewayFrontendPort443'"
                },
                "Protocol": "Https",
                "SslCertificate": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/sslCertificates/appGatewaySslCert1'"
                },
            }
        },
        {
            "name": "appGatewayHttpListener",
            "properties": {
                "FrontendIPConfiguration": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendIPConfigurations/appGatewayFrontendIP'"
                },
                "FrontendPort": {
                    "Id": "/subscriptions/{subscriptionId/resourceGroups/{resourceGroupName/providers/Microsoft.Network/applicationGateways/{applicationGatewayName/frontendPorts/appGatewayFrontendPort80'"
                },
                "Protocol": "Http",
            }
        }
    ],
```

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>BackendAddressPool, BackendHttpSetting och routning regelkonfigurationen

En BackendAddressPool är används toodefine en serverdelspool med WebSocket aktiverad servrar. Hej backendHttpSetting definieras med en backend-port 80 och 443. hello egenskaper för cookie-baserad tillhörighet och requestTimeouts är inte relevant tooWebSocket trafik. Ändring ingen som krävs i hello routningsregel är ”Basic” används tootie hello lämpliga lyssnare toohello motsvarande serverdelen för adresspoolen. 

```json
"requestRoutingRules": [{
    "name": "<ruleName1>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/httpListeners/appGatewayHttpsListener')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }
    }

}, {
    "name": "<ruleName2>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/httpListeners/appGatewayHttpListener')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }

    }
}]
```

## <a name="websocket-enabled-backend"></a>WebSocket aktiverat backend

Din serverdel måste ha en HTTP/HTTPS-webbserver som körs på hello konfigurerats port (vanligtvis 80/443) för WebSocket toowork. Det här kravet är eftersom WebSocket-protokollet kräver hello inledande handskakning toobe HTTP med uppgraderingen tooWebSocket-protokollet som en huvudfält. hello följande är ett exempel på ett sidhuvud:

```
    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: http://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13
```

En annan orsak kan vara att programmet gateway backend-hälsoavsökningen stöder endast HTTP och HTTPS-protokoll. Om hello backend-servern inte svarar tooHTTP eller HTTPS-avsökningar tas den utanför serverdelspool.

## <a name="next-steps"></a>Nästa steg

Efter att lära dig mer om WebSocket-support, gå för[skapa en Programgateway](application-gateway-create-gateway.md) tooget igång med en WebSocket aktiverat webbprogram.

