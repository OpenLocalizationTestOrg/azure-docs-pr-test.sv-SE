---
title: aaaConfigure SSL-avlastning - Azure Application Gateway - Azure CLI 2.0 | Microsoft Docs
description: "Den här sidan finns instruktioner toocreate en Programgateway med SSL-avlastning av Azure CLI 2.0"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: gwallace
ms.openlocfilehash: f8d50e0c6ffef17c807938d816410e6d85321c9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-cli-20"></a>Konfigurera en Programgateway för SSL-avlastning med hjälp av Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-ssl-portal.md)
> * [PowerShell och Azure Resource Manager](application-gateway-ssl-arm.md)
> * [PowerShell och den klassiska Azure-portalen](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Azure Application Gateway kan vara konfigurerade tooterminate hello Secure Sockets Layer (SSL)-session på hello gateway tooavoid kostsamma SSL dekryptering uppgifter toohappen på hello webbservergrupp. SSL-avlastning förenklar också certifikathantering på hello front-end-server.

## <a name="prerequisite-install-hello-azure-cli-20"></a>Förutsättning: Installera hello Azure CLI 2.0

tooperform hello stegen i den här artikeln, måste du för[installerar hello Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

## <a name="required-components"></a>Nödvändiga komponenter

* **Backend-serverpoolen:** hello lista över IP-adresser för hello backend-servrar. hello IP-adresser som anges antingen ska tillhöra toohello undernät för virtuellt nätverk eller ska vara en offentlig IP-adress/VIP.
* **Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet. De här inställningarna är bundet tooa poolen och tillämpade tooall servrar inom hello poolen.
* **Frontend-port:** den här porten är hello offentliga som öppnas på hello Programgateway. Trafik träffar den här porten och sedan hämtar omdirigeras tooone hello backend-servrar.
* **Lyssnare:** hello-lyssnare har en frontend-port, ett protokoll (Http eller Https inställningarna är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning).
* **Regel:** hello regeln Binder hello-lyssnare och hello backend-serverpoolen och definierar vilken backend-server pool hello trafik ska vara riktad toowhen den når en viss lyssnare. För närvarande endast hello *grundläggande* regeln stöds. Hej *grundläggande* regeln är resursallokering belastningsdistribution.

**Information om ytterligare konfiguration**

Konfiguration för SSL-certifikat, hello-protokollet i **HttpListener** bör ändra för*Https* (skiftlägeskänsligt). Hej **SslCertificate** läggs elementet för**HttpListener** med hello variabelvärde som konfigurerats för hello SSL-certifikat. hello frontend-porten ska vara uppdaterade too443.

**tooenable cookie-baserad tillhörighet**: en Programgateway kan vara konfigurerade tooensure att en begäran från en klientsession är alltid dirigerad toohello samma virtuella dator i hello webbservergrupp. Det här scenariot sker genom injektion av en sessions-cookie som tillåter trafik för hello gateway toodirect på lämpligt sätt. Ange tooenable cookie-baserad tillhörighet **CookieBasedAffinity** för*aktiverad* i hello **BackendHttpSettings** element.

## <a name="configure-ssl-offload-on-an-existing-application-gateway"></a>Konfigurera SSL-avlastning på en befintlig Programgateway

```azurecli-interactive
#!/bin/bash

# Create a new front end port toobe used for SSL
az network application-gateway frontend-port create \
  --name sslport \
  --port 443 \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Upload hello .pfx certificate for SSL offload
az network application-gateway ssl-cert create \
  --name "newcert" \
  --cert-file /home/azureuser/self-signed/AdatumAppGatewayCert.pfx \
  --cert-password P@ssw0rd \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Create a new listener referencing hello port and certificate created earlier
az network application-gateway http-listener create \
  --frontend-ip "appGatewayFrontendIP" \
  --frontend-port sslport  \
  --name sslListener \
  --ssl-cert newcert \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Create a new back-end pool toobe used
az network application-gateway address-pool create \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG" \
  --name "appGatewayBackendPool2" \
  --servers 10.0.0.7 10.0.0.8

# Create a new back-end HTTP settings using hello new probe
az network application-gateway http-settings create \
  --name "settings2" \
  --port 80 \
  --cookie-based-affinity Enabled \
  --protocol "Http" \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Create a new rule linking hello listener toohello back-end pool
az network application-gateway rule create \
  --name "rule2" \
  --rule-type Basic \
  --http-settings settings2 \
  --http-listener ssllistener \
  --address-pool temp1 \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

```

## <a name="create-an-application-gateway-with-ssl-offload"></a>Skapa en Programgateway med SSL-avlastning

hello följande exempel skapar en Programgateway med SSL-avlastning.  hello certifikat och lösenord för certifikatet måste vara uppdaterade tooa giltig privat nyckel.

```azurecli-interactive
#!/bin/bash

# Creates an application gateway with SSL offload
az network application-gateway create \
  --name "AdatumAppGateway3" \
  --location "eastus" \
  --resource-group "AdatumAppGatewayRG2" \
  --vnet-name "AdatumAppGatewayVNET2" \
  --cert-file /home/azureuser/self-signed/AdatumAppGatewayCert.pfx \
  --cert-password P@ssw0rd \
  --vnet-address-prefix "10.0.0.0/16" \
  --subnet "Appgatewaysubnet" \
  --subnet-address-prefix "10.0.0.0/28" \
  --frontend-port 443 \
  --servers "10.0.0.5 10.0.0.4" \
  --capacity 2 \
  --sku "Standard_Small" \
  --http-settings-cookie-based-affinity "Enabled" \
  --http-settings-protocol "Http" \
  --frontend-port "80" \
  --routing-rule-type "Basic" \
  --http-settings-port "80" \
  --public-ip-address "pip" \
  --public-ip-address-allocation "dynamic"
```

## <a name="get-application-gateway-dns-name"></a>Hämta DNS-namn för programgatewayen

När du har skapat hello gateway är hello nästa steg tooconfigure hello klientdelen för kommunikation. När du använder en offentlig IP-adress krävs ett dynamiskt tilldelat DNS-namn som inte är användarvänligt. tooensure slutanvändare kan träffa hello Programgateway, en CNAME-post kan vara används toopoint toohello offentlig slutpunkt för hello Programgateway. [Konfigurera ett eget domännamn i Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). tooconfigure ett alias, hämta information om hello Programgateway och dess associerade IP DNS-namn med hjälp av hello PublicIPAddress element bifogade toohello Programgateway. hello programmet gateway DNS-namn ska använda toocreate en CNAME-post som pekar hello två web program toothis DNS-namn. hello rekommenderas A-poster inte eftersom hello VIP ändras vid omstart för Programgateway.


```azurecli-interactive
az network public-ip show --name "pip" --resource-group "AdatumAppGatewayRG"
```

```
{
  "dnsSettings": {
    "domainNameLabel": null,
    "fqdn": "8c786058-96d4-4f3e-bb41-660860ceae4c.cloudapp.net",
    "reverseFqdn": null
  },
  "etag": "W/\"3b0ac031-01f0-4860-b572-e3c25e0c57ad\"",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AdatumAppGatewayRG/providers/Microsoft.Network/publicIPAddresses/pip2",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "40.121.167.250",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AdatumAppGatewayRG/providers/Microsoft.Network/applicationGateways/AdatumAppGateway2/frontendIPConfigurations/appGatewayFrontendIP",
    "name": null,
    "privateIpAddress": null,
    "privateIpAllocationMethod": null,
    "provisioningState": null,
    "publicIpAddress": null,
    "resourceGroup": "AdatumAppGatewayRG",
    "subnet": null
  },
  "location": "eastus",
  "name": "pip2",
  "provisioningState": "Succeeded",
  "publicIpAddressVersion": "IPv4",
  "publicIpAllocationMethod": "Dynamic",
  "resourceGroup": "AdatumAppGatewayRG",
  "resourceGuid": "3c30d310-c543-4e9d-9c72-bbacd7fe9b05",
  "tags": {
    "cli[2] owner[administrator]": ""
  },
  "type": "Microsoft.Network/publicIPAddresses"
}
```

## <a name="next-steps"></a>Nästa steg

Om du vill tooconfigure ett program gateway toouse med en intern belastningsutjämnare (ILB), se [skapa en Programgateway med en intern belastningsutjämnare (ILB)](application-gateway-ilb.md).

Om du vill ha mer information om belastningsutjämningsalternativ i allmänhet läser du:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
