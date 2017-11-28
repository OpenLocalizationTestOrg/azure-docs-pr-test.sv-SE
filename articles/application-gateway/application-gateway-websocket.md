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
# <a name="overview-of-websocket-support-in-application-gateway"></a><span data-ttu-id="57b58-103">Översikt över WebSocket-stöd i Programgateway</span><span class="sxs-lookup"><span data-stu-id="57b58-103">Overview of WebSocket support in Application Gateway</span></span>

<span data-ttu-id="57b58-104">Application Gateway har inbyggt stöd för WebSocket över alla gateway-storlekar.</span><span class="sxs-lookup"><span data-stu-id="57b58-104">Application Gateway provides native support for WebSocket across all gateway sizes.</span></span> <span data-ttu-id="57b58-105">Det finns ingen inställning som kan konfigureras av användaren tooselectively aktivera eller inaktivera WebSocket-stöd.</span><span class="sxs-lookup"><span data-stu-id="57b58-105">There is no user-configurable setting tooselectively enable or disable WebSocket support.</span></span> 

<span data-ttu-id="57b58-106">WebSocket-protokollet är standardiserade i [RFC6455](https://tools.ietf.org/html/rfc6455) aktiverar en fullständig duplex kommunikation mellan en server och en klient över en tidskrävande TCP-anslutning.</span><span class="sxs-lookup"><span data-stu-id="57b58-106">WebSocket protocol standardized in [RFC6455](https://tools.ietf.org/html/rfc6455) enables a full duplex communication between a server and a client over a long running TCP connection.</span></span> <span data-ttu-id="57b58-107">Den här funktionen tillåter en mer interaktiva kommunikation mellan hello webbservern och hello-klienten, vilket kan vara dubbelriktad utan hello krävs för avsökning som krävs i HTTP-baserade implementeringar.</span><span class="sxs-lookup"><span data-stu-id="57b58-107">This feature allows for a more interactive communication between hello web server and hello client, which can be bidirectional without hello need for polling as required in HTTP-based implementations.</span></span> <span data-ttu-id="57b58-108">WebSocket har lite extra till skillnad från HTTP och kan återanvändning hello samma TCP-anslutning för flera/svar ledde till ett mer effektivt utnyttjande av resurser.</span><span class="sxs-lookup"><span data-stu-id="57b58-108">WebSocket has low overhead unlike HTTP and can reuse hello same TCP connection for multiple request/responses resulting in a more efficient utilization of resources.</span></span> <span data-ttu-id="57b58-109">WebSocket-protokoll är utformad toowork via traditionella HTTP-portarna 80 och 443.</span><span class="sxs-lookup"><span data-stu-id="57b58-109">WebSocket protocols are designed toowork over traditional HTTP ports of 80 and 443.</span></span>

<span data-ttu-id="57b58-110">Du kan fortsätta att använda en standard HTTP-lyssnaren på port 80 eller 443 tooreceive WebSocket-trafik.</span><span class="sxs-lookup"><span data-stu-id="57b58-110">You can continue using a standard HTTP listener on port 80 or 443 tooreceive WebSocket traffic.</span></span> <span data-ttu-id="57b58-111">WebSocket-trafik är riktat toohello WebSocket aktiverat serverdelen med hello lämpliga serverdelspool som anges i programmet gateway regler.</span><span class="sxs-lookup"><span data-stu-id="57b58-111">WebSocket traffic is then directed toohello WebSocket enabled backend server using hello appropriate backend pool as specified in application gateway rules.</span></span> <span data-ttu-id="57b58-112">hello backend-servern måste svara toohello programmet gateway avsökningar, som beskrivs i hello [hälsa avsökningen översikt](application-gateway-probe-overview.md) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="57b58-112">hello backend server must respond toohello application gateway probes, which are described in hello [health probe overview](application-gateway-probe-overview.md) section.</span></span> <span data-ttu-id="57b58-113">Programmet gateway hälsoavsökningar är endast HTTP/HTTPS.</span><span class="sxs-lookup"><span data-stu-id="57b58-113">Application gateway health probes are HTTP/HTTPS only.</span></span> <span data-ttu-id="57b58-114">Varje backend-servern måste svara tooHTTP avsökningar för gateway tooroute WebSocket trafik toohello programserver.</span><span class="sxs-lookup"><span data-stu-id="57b58-114">Each backend server must respond tooHTTP probes for application gateway tooroute WebSocket traffic toohello server.</span></span>

## <a name="listener-configuration-element"></a><span data-ttu-id="57b58-115">Listener-konfigurationselementet</span><span class="sxs-lookup"><span data-stu-id="57b58-115">Listener configuration element</span></span>

<span data-ttu-id="57b58-116">En befintlig HTTP-lyssnare kan vara används toosupport WebSocket-trafik.</span><span class="sxs-lookup"><span data-stu-id="57b58-116">An existing HTTP listener can be used toosupport WebSocket traffic.</span></span> <span data-ttu-id="57b58-117">hello följande är ett fragment av ett httpListeners element från en exempelfil för mallen.</span><span class="sxs-lookup"><span data-stu-id="57b58-117">hello following is a snippet of an httpListeners element from a sample template file.</span></span> <span data-ttu-id="57b58-118">Du måste både HTTP och HTTPS-lyssnare toosupport WebSocket och säker WebSocket-trafik.</span><span class="sxs-lookup"><span data-stu-id="57b58-118">You would need both HTTP and HTTPS listeners toosupport WebSocket and secure WebSocket traffic.</span></span> <span data-ttu-id="57b58-119">På liknande sätt kan du använda hello [portal](application-gateway-create-gateway-portal.md) eller [PowerShell](application-gateway-create-gateway-arm.md) toocreate en Programgateway med lyssnare på port 80/443 toosupport WebSocket-trafik.</span><span class="sxs-lookup"><span data-stu-id="57b58-119">Similarly you can use hello [portal](application-gateway-create-gateway-portal.md) or [PowerShell](application-gateway-create-gateway-arm.md) toocreate an application gateway with listeners on port 80/443 toosupport WebSocket traffic.</span></span>

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

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a><span data-ttu-id="57b58-120">BackendAddressPool, BackendHttpSetting och routning regelkonfigurationen</span><span class="sxs-lookup"><span data-stu-id="57b58-120">BackendAddressPool, BackendHttpSetting, and Routing rule configuration</span></span>

<span data-ttu-id="57b58-121">En BackendAddressPool är används toodefine en serverdelspool med WebSocket aktiverad servrar.</span><span class="sxs-lookup"><span data-stu-id="57b58-121">A BackendAddressPool is used toodefine a backend pool with WebSocket enabled servers.</span></span> <span data-ttu-id="57b58-122">Hej backendHttpSetting definieras med en backend-port 80 och 443.</span><span class="sxs-lookup"><span data-stu-id="57b58-122">hello backendHttpSetting is defined with a backend port 80 and 443.</span></span> <span data-ttu-id="57b58-123">hello egenskaper för cookie-baserad tillhörighet och requestTimeouts är inte relevant tooWebSocket trafik.</span><span class="sxs-lookup"><span data-stu-id="57b58-123">hello properties for cookie-based affinity and requestTimeouts are not relevant tooWebSocket traffic.</span></span> <span data-ttu-id="57b58-124">Ändring ingen som krävs i hello routningsregel är ”Basic” används tootie hello lämpliga lyssnare toohello motsvarande serverdelen för adresspoolen.</span><span class="sxs-lookup"><span data-stu-id="57b58-124">There is no change required in hello routing rule, 'Basic' is used tootie hello appropriate listener toohello corresponding backend address pool.</span></span> 

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

## <a name="websocket-enabled-backend"></a><span data-ttu-id="57b58-125">WebSocket aktiverat backend</span><span class="sxs-lookup"><span data-stu-id="57b58-125">WebSocket enabled backend</span></span>

<span data-ttu-id="57b58-126">Din serverdel måste ha en HTTP/HTTPS-webbserver som körs på hello konfigurerats port (vanligtvis 80/443) för WebSocket toowork.</span><span class="sxs-lookup"><span data-stu-id="57b58-126">Your backend must have a HTTP/HTTPS web server running on hello configured port (usually 80/443) for WebSocket toowork.</span></span> <span data-ttu-id="57b58-127">Det här kravet är eftersom WebSocket-protokollet kräver hello inledande handskakning toobe HTTP med uppgraderingen tooWebSocket-protokollet som en huvudfält.</span><span class="sxs-lookup"><span data-stu-id="57b58-127">This requirement is because WebSocket protocol requires hello initial handshake toobe HTTP with upgrade tooWebSocket protocol as a header field.</span></span> <span data-ttu-id="57b58-128">hello följande är ett exempel på ett sidhuvud:</span><span class="sxs-lookup"><span data-stu-id="57b58-128">hello following is an example of a header:</span></span>

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

<span data-ttu-id="57b58-129">En annan orsak kan vara att programmet gateway backend-hälsoavsökningen stöder endast HTTP och HTTPS-protokoll.</span><span class="sxs-lookup"><span data-stu-id="57b58-129">Another reason for this is that application gateway backend health probe supports HTTP and HTTPS protocols only.</span></span> <span data-ttu-id="57b58-130">Om hello backend-servern inte svarar tooHTTP eller HTTPS-avsökningar tas den utanför serverdelspool.</span><span class="sxs-lookup"><span data-stu-id="57b58-130">If hello backend server does not respond tooHTTP or HTTPS probes, it is taken out of backend pool.</span></span>

## <a name="next-steps"></a><span data-ttu-id="57b58-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="57b58-131">Next steps</span></span>

<span data-ttu-id="57b58-132">Efter att lära dig mer om WebSocket-support, gå för[skapa en Programgateway](application-gateway-create-gateway.md) tooget igång med en WebSocket aktiverat webbprogram.</span><span class="sxs-lookup"><span data-stu-id="57b58-132">After learning about WebSocket support, go too[create an application gateway](application-gateway-create-gateway.md) tooget started with a WebSocket enabled web application.</span></span>

