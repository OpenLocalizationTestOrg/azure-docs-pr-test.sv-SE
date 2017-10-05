---
title: "WebSocket-stöd i Azure Programgateway | Microsoft Docs"
description: "Den här sidan innehåller en översikt av programmet Gateway WebSocket-stöd."
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
ms.openlocfilehash: 75b06ddd02da231b7813c609c848c75e42116da5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="overview-of-websocket-support-in-application-gateway"></a><span data-ttu-id="bd4d7-103">Översikt över WebSocket-stöd i Programgateway</span><span class="sxs-lookup"><span data-stu-id="bd4d7-103">Overview of WebSocket support in Application Gateway</span></span>

<span data-ttu-id="bd4d7-104">Application Gateway har inbyggt stöd för WebSocket över alla gateway-storlekar.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-104">Application Gateway provides native support for WebSocket across all gateway sizes.</span></span> <span data-ttu-id="bd4d7-105">Det finns ingen kan konfigureras inställning du vill aktivera eller inaktivera WebSocket-stöd.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-105">There is no user-configurable setting to selectively enable or disable WebSocket support.</span></span> 

<span data-ttu-id="bd4d7-106">WebSocket-protokollet är standardiserade i [RFC6455](https://tools.ietf.org/html/rfc6455) aktiverar en fullständig duplex kommunikation mellan en server och en klient över en tidskrävande TCP-anslutning.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-106">WebSocket protocol standardized in [RFC6455](https://tools.ietf.org/html/rfc6455) enables a full duplex communication between a server and a client over a long running TCP connection.</span></span> <span data-ttu-id="bd4d7-107">Den här funktionen tillåter för en mer interaktiva kommunikation mellan webbservern och klienten, som kan vara dubbelriktad utan att behöva för avsökning som krävs i HTTP-baserade implementeringar.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-107">This feature allows for a more interactive communication between the web server and the client, which can be bidirectional without the need for polling as required in HTTP-based implementations.</span></span> <span data-ttu-id="bd4d7-108">WebSocket har lite extra till skillnad från HTTP och kan återanvända samma TCP-anslutning för flera begäranden/svar ledde till ett mer effektivt utnyttjande av resurser.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-108">WebSocket has low overhead unlike HTTP and can reuse the same TCP connection for multiple request/responses resulting in a more efficient utilization of resources.</span></span> <span data-ttu-id="bd4d7-109">WebSocket-protokoll är utformade att fungera via traditionella HTTP-portarna 80 och 443.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-109">WebSocket protocols are designed to work over traditional HTTP ports of 80 and 443.</span></span>

<span data-ttu-id="bd4d7-110">Du kan fortsätta att använda en standard HTTP-lyssnaren på port 80 eller 443 ta emot WebSocket-trafik.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-110">You can continue using a standard HTTP listener on port 80 or 443 to receive WebSocket traffic.</span></span> <span data-ttu-id="bd4d7-111">WebSocket-trafik omdirigeras sedan till WebSocket aktiverad serverdelen med lämpliga serverdelspoolen som anges i programmet gateway regler.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-111">WebSocket traffic is then directed to the WebSocket enabled backend server using the appropriate backend pool as specified in application gateway rules.</span></span> <span data-ttu-id="bd4d7-112">Backend-servern måste svara på gateway-avsökningar programmet som beskrivs i den [hälsa avsökningen översikt](application-gateway-probe-overview.md) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-112">The backend server must respond to the application gateway probes, which are described in the [health probe overview](application-gateway-probe-overview.md) section.</span></span> <span data-ttu-id="bd4d7-113">Programmet gateway hälsoavsökningar är endast HTTP/HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-113">Application gateway health probes are HTTP/HTTPS only.</span></span> <span data-ttu-id="bd4d7-114">Varje backend-servern måste svara på http-avsökningar för Programgateway att dirigera WebSocket-trafik till servern.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-114">Each backend server must respond to HTTP probes for application gateway to route WebSocket traffic to the server.</span></span>

## <a name="listener-configuration-element"></a><span data-ttu-id="bd4d7-115">Listener-konfigurationselementet</span><span class="sxs-lookup"><span data-stu-id="bd4d7-115">Listener configuration element</span></span>

<span data-ttu-id="bd4d7-116">En befintlig HTTP-lyssnare kan användas för att stödja WebSocket-trafik.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-116">An existing HTTP listener can be used to support WebSocket traffic.</span></span> <span data-ttu-id="bd4d7-117">Följande är ett fragment av ett httpListeners element från en exempelfil för mallen.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-117">The following is a snippet of an httpListeners element from a sample template file.</span></span> <span data-ttu-id="bd4d7-118">Behöver du både HTTP och HTTPS-lyssnare för att stödja WebSocket och säker WebSocket-trafik.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-118">You would need both HTTP and HTTPS listeners to support WebSocket and secure WebSocket traffic.</span></span> <span data-ttu-id="bd4d7-119">På liknande sätt kan du använda den [portal](application-gateway-create-gateway-portal.md) eller [PowerShell](application-gateway-create-gateway-arm.md) att skapa en Programgateway med lyssnare på port 80/443 för att stödja WebSocket-trafik.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-119">Similarly you can use the [portal](application-gateway-create-gateway-portal.md) or [PowerShell](application-gateway-create-gateway-arm.md) to create an application gateway with listeners on port 80/443 to support WebSocket traffic.</span></span>

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

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a><span data-ttu-id="bd4d7-120">BackendAddressPool, BackendHttpSetting och routning regelkonfigurationen</span><span class="sxs-lookup"><span data-stu-id="bd4d7-120">BackendAddressPool, BackendHttpSetting, and Routing rule configuration</span></span>

<span data-ttu-id="bd4d7-121">En BackendAddressPool används för att definiera en serverdelspool med WebSocket aktiverad servrar.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-121">A BackendAddressPool is used to define a backend pool with WebSocket enabled servers.</span></span> <span data-ttu-id="bd4d7-122">BackendHttpSetting definieras med en backend-port 80 och 443.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-122">The backendHttpSetting is defined with a backend port 80 and 443.</span></span> <span data-ttu-id="bd4d7-123">Egenskaper för cookie-baserad tillhörighet och requestTimeouts är inte relevanta för WebSocket-trafik.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-123">The properties for cookie-based affinity and requestTimeouts are not relevant to WebSocket traffic.</span></span> <span data-ttu-id="bd4d7-124">Ändring ingen som krävs i regeln för routning, ”Basic” används för att koppla motsvarande serverdelsadresspool lämplig lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-124">There is no change required in the routing rule, 'Basic' is used to tie the appropriate listener to the corresponding backend address pool.</span></span> 

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

## <a name="websocket-enabled-backend"></a><span data-ttu-id="bd4d7-125">WebSocket aktiverat backend</span><span class="sxs-lookup"><span data-stu-id="bd4d7-125">WebSocket enabled backend</span></span>

<span data-ttu-id="bd4d7-126">Din serverdel måste ha en HTTP/HTTPS-webbserver som körs på den konfigurerade port (vanligtvis 80/443) för WebSocket ska fungera.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-126">Your backend must have a HTTP/HTTPS web server running on the configured port (usually 80/443) for WebSocket to work.</span></span> <span data-ttu-id="bd4d7-127">Det här kravet är eftersom WebSocket-protokollet krävs inledande handskakning ska HTTP med uppgraderingen WebSocket-protokollet som en huvudfält.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-127">This requirement is because WebSocket protocol requires the initial handshake to be HTTP with upgrade to WebSocket protocol as a header field.</span></span> <span data-ttu-id="bd4d7-128">Följande är ett exempel på ett sidhuvud:</span><span class="sxs-lookup"><span data-stu-id="bd4d7-128">The following is an example of a header:</span></span>

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

<span data-ttu-id="bd4d7-129">En annan orsak kan vara att programmet gateway backend-hälsoavsökningen stöder endast HTTP och HTTPS-protokoll.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-129">Another reason for this is that application gateway backend health probe supports HTTP and HTTPS protocols only.</span></span> <span data-ttu-id="bd4d7-130">Om backend-servern inte svarar på HTTP eller HTTPS avsökningar, tas den utanför serverdelspool.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-130">If the backend server does not respond to HTTP or HTTPS probes, it is taken out of backend pool.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bd4d7-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bd4d7-131">Next steps</span></span>

<span data-ttu-id="bd4d7-132">Efter att lära dig mer om WebSocket-support, gå till [skapa en Programgateway](application-gateway-create-gateway.md) komma igång med en WebSocket-aktiverade program.</span><span class="sxs-lookup"><span data-stu-id="bd4d7-132">After learning about WebSocket support, go to [create an application gateway](application-gateway-create-gateway.md) to get started with a WebSocket enabled web application.</span></span>

