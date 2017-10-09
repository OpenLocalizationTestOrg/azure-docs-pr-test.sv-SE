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
# <a name="application-gateway-multiple-site-hosting"></a>Flera webbplatser i Application Gateway

Värd för flera plats kan du tooconfigure mer än ett webbprogram på hello samma program gateway-instans. Den här funktionen kan du tooconfigure en effektivare topologin för din distribution genom att lägga ihop too20 webbplatser tooone Programgateway. Varje webbplats kan dirigeras tooits äger serverdelspool. I följande exempel hello, fungerar Programgateway trafik för contoso.com och fabrikam.com från två backend-serverpooler kallas ContosoServerPool och FabrikamServerPool.

![imageURLroute](./media/application-gateway-multi-site-overview/multisite.png)

> [!IMPORTANT]
> Regler bearbetas i hello ordning de anges i hello-portalen. Det är första tidigare tooconfiguring för rekommenderas tooconfigure flera platser lyssnare en grundläggande lyssnare.  Detta säkerställer att trafik hämtar routade toohello tillbaka avslutas. Om en grundläggande lyssnare visas först och matchar en inkommande begäran kommer den att bearbetas av den lyssnaren.

Begäranden för http://contoso.com är routade tooContosoServerPool och http://fabrikam.com är routade tooFabrikamServerPool.

På liknande sätt hello två underdomäner i hello samma överordnade domänen kan finnas på samma gateway-programdistribution. Exempel på användning av underdomäner kan vara http://blog.contoso.com och http://app.contoso.com på samma distribution av en programgateway.

## <a name="host-headers-and-server-name-indication-sni"></a>Värdhuvuden och servernamnsindikator (SNI)

Det finns tre vanliga mekanismer för att aktivera flera för webbplatsen på hello samma infrastruktur.

1. Flera webbprogram på varsin unik IP-adress.
2. Använd värdnamnet toohost flera webbprogram på hello samma IP-adress.
3. Använd olika portar toohost flera webbprogram på hello samma IP-adress.

För tillfället får en Application Gateway en enda IP-adress som den lyssnar på trafik från. Därför stöds inte alternativet där varje program har sin egen IP-adress för tillfället. Application Gateway har stöd för värd för flera program varje lyssnar på olika portar, men det här scenariot kräver hello program tooaccept trafik på portarna inte är standard och inte är en önskad konfiguration. Programgateway är beroende av HTTP 1.1 värden huvuden toohost mer än en webbplats på hello samma offentliga IP-adress och port. hello webbplatser som finns på Programgateway kan också stöd för SSL-avlastning med Server Servernamnsindikation (SNI) TLS-tillägg. Det här scenariot innebär att hello klientens webbläsare och backend webbservergrupp måste ha stöd för HTTP/1.1 och TLS-tillägg som definieras i RFC 6066.

## <a name="listener-configuration-element"></a>Listener-konfigurationselementet

Befintliga HTTPListener konfigurationselement är förbättrad toosupport värden namn och en server name indikation element, som används av gateway tooroute trafik tooappropriate backend programpoolen. hello är följande kodexempel hello fragment av HttpListeners element från mallen.

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

Du besöker [Resource Manager-mallen med hjälp av flera värd för webbplatsen](https://github.com/Azure/azure-quickstart-templates/blob/master/201-application-gateway-multihosting) end tooend mallbaserade distribution.

## <a name="routing-rule"></a>Routingregeln

Det finns ingen ändring som krävs i hello routningsregel. Hej routningsregel ”Basic” bör fortsätta toobe valt tootie hello lämplig plats lyssnare toohello motsvarande serverdelen för adresspoolen.

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

## <a name="next-steps"></a>Nästa steg

När du lära dig mer om flera värd för platsen, gå för[skapa en Programgateway med flera värd för webbplatsen](application-gateway-create-multisite-azureresourcemanager-powershell.md) toocreate en Programgateway med möjlighet toosupport mer än ett webbprogram.

