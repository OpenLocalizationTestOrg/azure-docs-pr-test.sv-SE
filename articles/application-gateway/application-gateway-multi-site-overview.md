---
title: "aaaHosting flera platser på Azure Programgateway | Microsoft Docs"
description: "Den här sidan innehåller en översikt över hello Programgateway stöd för flera platser."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: 
ms.assetid: 49993fd2-87e5-4a66-b386-8d22056a616d
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2017
ms.author: amsriva
ms.openlocfilehash: 4ab6faa97f1891d7525affdaa36463681bf99e9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-gateway-multiple-site-hosting"></a><span data-ttu-id="5e212-103">Flera webbplatser i Application Gateway</span><span class="sxs-lookup"><span data-stu-id="5e212-103">Application Gateway multiple site hosting</span></span>

<span data-ttu-id="5e212-104">Värd för flera plats kan du tooconfigure mer än ett webbprogram på hello samma program gateway-instans.</span><span class="sxs-lookup"><span data-stu-id="5e212-104">Multiple site hosting enables you tooconfigure more than one web application on hello same application gateway instance.</span></span> <span data-ttu-id="5e212-105">Den här funktionen kan du tooconfigure en effektivare topologin för din distribution genom att lägga ihop too20 webbplatser tooone Programgateway.</span><span class="sxs-lookup"><span data-stu-id="5e212-105">This feature allows you tooconfigure a more efficient topology for your deployments by adding up too20 websites tooone application gateway.</span></span> <span data-ttu-id="5e212-106">Varje webbplats kan dirigeras tooits äger serverdelspool.</span><span class="sxs-lookup"><span data-stu-id="5e212-106">Each website can be directed tooits own backend pool.</span></span> <span data-ttu-id="5e212-107">I följande exempel hello, fungerar Programgateway trafik för contoso.com och fabrikam.com från två backend-serverpooler kallas ContosoServerPool och FabrikamServerPool.</span><span class="sxs-lookup"><span data-stu-id="5e212-107">In hello following example, application gateway is serving traffic for contoso.com and fabrikam.com from two back-end server pools called ContosoServerPool and FabrikamServerPool.</span></span>

![imageURLroute](./media/application-gateway-multi-site-overview/multisite.png)

> [!IMPORTANT]
> <span data-ttu-id="5e212-109">Regler bearbetas i hello ordning de anges i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="5e212-109">Rules are processed in hello order they are listed in hello portal.</span></span> <span data-ttu-id="5e212-110">Det är första tidigare tooconfiguring för rekommenderas tooconfigure flera platser lyssnare en grundläggande lyssnare.</span><span class="sxs-lookup"><span data-stu-id="5e212-110">It is highly recommended tooconfigure multi-site listeners first prior tooconfiguring a basic listener.</span></span>  <span data-ttu-id="5e212-111">Detta säkerställer att trafik hämtar routade toohello tillbaka avslutas.</span><span class="sxs-lookup"><span data-stu-id="5e212-111">This will ensure that traffic gets routed toohello right back end.</span></span> <span data-ttu-id="5e212-112">Om en grundläggande lyssnare visas först och matchar en inkommande begäran kommer den att bearbetas av den lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="5e212-112">If a basic listener is listed first and matches an incoming request, it gets processed by that listener.</span></span>

<span data-ttu-id="5e212-113">Begäranden för http://contoso.com är routade tooContosoServerPool och http://fabrikam.com är routade tooFabrikamServerPool.</span><span class="sxs-lookup"><span data-stu-id="5e212-113">Requests for http://contoso.com are routed tooContosoServerPool, and http://fabrikam.com are routed tooFabrikamServerPool.</span></span>

<span data-ttu-id="5e212-114">På liknande sätt hello två underdomäner i hello samma överordnade domänen kan finnas på samma gateway-programdistribution.</span><span class="sxs-lookup"><span data-stu-id="5e212-114">Similarly two subdomains of hello same parent domain can be hosted on hello same application gateway deployment.</span></span> <span data-ttu-id="5e212-115">Exempel på användning av underdomäner kan vara http://blog.contoso.com och http://app.contoso.com på samma distribution av en programgateway.</span><span class="sxs-lookup"><span data-stu-id="5e212-115">Examples of using subdomains could include http://blog.contoso.com and http://app.contoso.com hosted on a single application gateway deployment.</span></span>

## <a name="host-headers-and-server-name-indication-sni"></a><span data-ttu-id="5e212-116">Värdhuvuden och servernamnsindikator (SNI)</span><span class="sxs-lookup"><span data-stu-id="5e212-116">Host headers and Server Name Indication (SNI)</span></span>

<span data-ttu-id="5e212-117">Det finns tre vanliga mekanismer för att aktivera flera för webbplatsen på hello samma infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="5e212-117">There are three common mechanisms for enabling multiple site hosting on hello same infrastructure.</span></span>

1. <span data-ttu-id="5e212-118">Flera webbprogram på varsin unik IP-adress.</span><span class="sxs-lookup"><span data-stu-id="5e212-118">Host multiple web applications each on a unique IP address.</span></span>
2. <span data-ttu-id="5e212-119">Använd värdnamnet toohost flera webbprogram på hello samma IP-adress.</span><span class="sxs-lookup"><span data-stu-id="5e212-119">Use host name toohost multiple web applications on hello same IP address.</span></span>
3. <span data-ttu-id="5e212-120">Använd olika portar toohost flera webbprogram på hello samma IP-adress.</span><span class="sxs-lookup"><span data-stu-id="5e212-120">Use different ports toohost multiple web applications on hello same IP address.</span></span>

<span data-ttu-id="5e212-121">För tillfället får en Application Gateway en enda IP-adress som den lyssnar på trafik från.</span><span class="sxs-lookup"><span data-stu-id="5e212-121">Currently an application gateway gets a single public IP address on which it listens for traffic.</span></span> <span data-ttu-id="5e212-122">Därför stöds inte alternativet där varje program har sin egen IP-adress för tillfället.</span><span class="sxs-lookup"><span data-stu-id="5e212-122">Therefore supporting multiple applications, each with its own IP address, is currently not supported.</span></span> <span data-ttu-id="5e212-123">Application Gateway har stöd för värd för flera program varje lyssnar på olika portar, men det här scenariot kräver hello program tooaccept trafik på portarna inte är standard och inte är en önskad konfiguration.</span><span class="sxs-lookup"><span data-stu-id="5e212-123">Application Gateway supports hosting multiple applications each listening on different ports but this scenario would require hello applications tooaccept traffic on non-standard ports and is often not a desired configuration.</span></span> <span data-ttu-id="5e212-124">Programgateway är beroende av HTTP 1.1 värden huvuden toohost mer än en webbplats på hello samma offentliga IP-adress och port.</span><span class="sxs-lookup"><span data-stu-id="5e212-124">Application Gateway relies on HTTP 1.1 host headers toohost more than one website on hello same public IP address and port.</span></span> <span data-ttu-id="5e212-125">hello webbplatser som finns på Programgateway kan också stöd för SSL-avlastning med Server Servernamnsindikation (SNI) TLS-tillägg.</span><span class="sxs-lookup"><span data-stu-id="5e212-125">hello sites hosted on application gateway can also support SSL offload with Server Name Indication (SNI) TLS extension.</span></span> <span data-ttu-id="5e212-126">Det här scenariot innebär att hello klientens webbläsare och backend webbservergrupp måste ha stöd för HTTP/1.1 och TLS-tillägg som definieras i RFC 6066.</span><span class="sxs-lookup"><span data-stu-id="5e212-126">This scenario means that hello client browser and backend web farm must support HTTP/1.1 and TLS extension as defined in RFC 6066.</span></span>

## <a name="listener-configuration-element"></a><span data-ttu-id="5e212-127">Listener-konfigurationselementet</span><span class="sxs-lookup"><span data-stu-id="5e212-127">Listener configuration element</span></span>

<span data-ttu-id="5e212-128">Befintliga HTTPListener konfigurationselement är förbättrad toosupport värden namn och en server name indikation element, som används av gateway tooroute trafik tooappropriate backend programpoolen.</span><span class="sxs-lookup"><span data-stu-id="5e212-128">Existing HTTPListener configuration element is enhanced toosupport host name and server name indication elements, which is used by application gateway tooroute traffic tooappropriate backend pool.</span></span> <span data-ttu-id="5e212-129">hello är följande kodexempel hello fragment av HttpListeners element från mallen.</span><span class="sxs-lookup"><span data-stu-id="5e212-129">hello following code example is hello snippet of HttpListeners element from template file.</span></span>

```json
"httpListeners": [
    {
        "name": "appGatewayHttpsListener1",
        "properties": {
            "FrontendIPConfiguration": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/DefaultFrontendPublicIP"
            },
            "FrontendPort": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort443'"
            },
            "Protocol": "Https",
            "SslCertificate": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/sslCertificates/appGatewaySslCert1'"
            },
            "HostName": "contoso.com",
            "RequireServerNameIndication": "true"
        }
    },
    {
        "name": "appGatewayHttpListener2",
        "properties": {
            "FrontendIPConfiguration": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/appGatewayFrontendIP'"
            },
            "FrontendPort": {
                "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort80'"
            },
            "Protocol": "Http",
            "HostName": "fabrikam.com",
            "RequireServerNameIndication": "false"
        }
    }
],
```

<span data-ttu-id="5e212-130">Du besöker [Resource Manager-mallen med hjälp av flera värd för webbplatsen](https://github.com/Azure/azure-quickstart-templates/blob/master/201-application-gateway-multihosting) end tooend mallbaserade distribution.</span><span class="sxs-lookup"><span data-stu-id="5e212-130">You can visit [Resource Manager template using multiple site hosting](https://github.com/Azure/azure-quickstart-templates/blob/master/201-application-gateway-multihosting) for an end tooend template-based deployment.</span></span>

## <a name="routing-rule"></a><span data-ttu-id="5e212-131">Routingregeln</span><span class="sxs-lookup"><span data-stu-id="5e212-131">Routing rule</span></span>

<span data-ttu-id="5e212-132">Det finns ingen ändring som krävs i hello routningsregel.</span><span class="sxs-lookup"><span data-stu-id="5e212-132">There is no change required in hello routing rule.</span></span> <span data-ttu-id="5e212-133">Hej routningsregel ”Basic” bör fortsätta toobe valt tootie hello lämplig plats lyssnare toohello motsvarande serverdelen för adresspoolen.</span><span class="sxs-lookup"><span data-stu-id="5e212-133">hello routing rule 'Basic' should continue toobe chosen tootie hello appropriate site listener toohello corresponding backend address pool.</span></span>

```json
"requestRoutingRules": [
{
    "name": "<ruleName1>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpsListener1')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }
    }

},
{
    "name": "<ruleName2>",
    "properties": {
        "RuleType": "Basic",
        "httpListener": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpListener2')]"
        },
        "backendAddressPool": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/FabrikamServerPool')]"
        },
        "backendHttpSettings": {
            "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
        }
    }

}
]
```

## <a name="next-steps"></a><span data-ttu-id="5e212-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5e212-134">Next steps</span></span>

<span data-ttu-id="5e212-135">När du lära dig mer om flera värd för platsen, gå för[skapa en Programgateway med flera värd för webbplatsen](application-gateway-create-multisite-azureresourcemanager-powershell.md) toocreate en Programgateway med möjlighet toosupport mer än ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="5e212-135">After learning about multiple site hosting, go too[create an application gateway using multiple site hosting](application-gateway-create-multisite-azureresourcemanager-powershell.md) toocreate an application gateway with ability toosupport more than one web application.</span></span>

