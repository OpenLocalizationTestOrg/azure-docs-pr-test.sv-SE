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
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-cli-20"></a><span data-ttu-id="85f22-103">Konfigurera en Programgateway för SSL-avlastning med hjälp av Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="85f22-103">Configure an application gateway for SSL offload by using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="85f22-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="85f22-104">Azure portal</span></span>](application-gateway-ssl-portal.md)
> * [<span data-ttu-id="85f22-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="85f22-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ssl-arm.md)
> * [<span data-ttu-id="85f22-106">PowerShell och den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="85f22-106">Azure Classic PowerShell</span></span>](application-gateway-ssl.md)
> * [<span data-ttu-id="85f22-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="85f22-107">Azure CLI 2.0</span></span>](application-gateway-ssl-cli.md)

<span data-ttu-id="85f22-108">Azure Application Gateway kan vara konfigurerade tooterminate hello Secure Sockets Layer (SSL)-session på hello gateway tooavoid kostsamma SSL dekryptering uppgifter toohappen på hello webbservergrupp.</span><span class="sxs-lookup"><span data-stu-id="85f22-108">Azure Application Gateway can be configured tooterminate hello Secure Sockets Layer (SSL) session at hello gateway tooavoid costly SSL decryption tasks toohappen at hello web farm.</span></span> <span data-ttu-id="85f22-109">SSL-avlastning förenklar också certifikathantering på hello front-end-server.</span><span class="sxs-lookup"><span data-stu-id="85f22-109">SSL offload also simplifies certificate management at hello front-end server.</span></span>

## <a name="prerequisite-install-hello-azure-cli-20"></a><span data-ttu-id="85f22-110">Förutsättning: Installera hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="85f22-110">Prerequisite: Install hello Azure CLI 2.0</span></span>

<span data-ttu-id="85f22-111">tooperform hello stegen i den här artikeln, måste du för[installerar hello Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="85f22-111">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="required-components"></a><span data-ttu-id="85f22-112">Nödvändiga komponenter</span><span class="sxs-lookup"><span data-stu-id="85f22-112">Required components</span></span>

* <span data-ttu-id="85f22-113">**Backend-serverpoolen:** hello lista över IP-adresser för hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="85f22-113">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="85f22-114">hello IP-adresser som anges antingen ska tillhöra toohello undernät för virtuellt nätverk eller ska vara en offentlig IP-adress/VIP.</span><span class="sxs-lookup"><span data-stu-id="85f22-114">hello IP addresses listed should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="85f22-115">**Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet.</span><span class="sxs-lookup"><span data-stu-id="85f22-115">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="85f22-116">De här inställningarna är bundet tooa poolen och tillämpade tooall servrar inom hello poolen.</span><span class="sxs-lookup"><span data-stu-id="85f22-116">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="85f22-117">**Frontend-port:** den här porten är hello offentliga som öppnas på hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="85f22-117">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="85f22-118">Trafik träffar den här porten och sedan hämtar omdirigeras tooone hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="85f22-118">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="85f22-119">**Lyssnare:** hello-lyssnare har en frontend-port, ett protokoll (Http eller Https inställningarna är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning).</span><span class="sxs-lookup"><span data-stu-id="85f22-119">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these settings are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="85f22-120">**Regel:** hello regeln Binder hello-lyssnare och hello backend-serverpoolen och definierar vilken backend-server pool hello trafik ska vara riktad toowhen den når en viss lyssnare.</span><span class="sxs-lookup"><span data-stu-id="85f22-120">**Rule:** hello rule binds hello listener and hello back-end server pool and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span> <span data-ttu-id="85f22-121">För närvarande endast hello *grundläggande* regeln stöds.</span><span class="sxs-lookup"><span data-stu-id="85f22-121">Currently, only hello *basic* rule is supported.</span></span> <span data-ttu-id="85f22-122">Hej *grundläggande* regeln är resursallokering belastningsdistribution.</span><span class="sxs-lookup"><span data-stu-id="85f22-122">hello *basic* rule is round-robin load distribution.</span></span>

<span data-ttu-id="85f22-123">**Information om ytterligare konfiguration**</span><span class="sxs-lookup"><span data-stu-id="85f22-123">**Additional configuration notes**</span></span>

<span data-ttu-id="85f22-124">Konfiguration för SSL-certifikat, hello-protokollet i **HttpListener** bör ändra för*Https* (skiftlägeskänsligt).</span><span class="sxs-lookup"><span data-stu-id="85f22-124">For SSL certificates configuration, hello protocol in **HttpListener** should change too*Https* (case sensitive).</span></span> <span data-ttu-id="85f22-125">Hej **SslCertificate** läggs elementet för**HttpListener** med hello variabelvärde som konfigurerats för hello SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="85f22-125">hello **SslCertificate** element is added too**HttpListener** with hello variable value configured for hello SSL certificate.</span></span> <span data-ttu-id="85f22-126">hello frontend-porten ska vara uppdaterade too443.</span><span class="sxs-lookup"><span data-stu-id="85f22-126">hello front-end port should be updated too443.</span></span>

<span data-ttu-id="85f22-127">**tooenable cookie-baserad tillhörighet**: en Programgateway kan vara konfigurerade tooensure att en begäran från en klientsession är alltid dirigerad toohello samma virtuella dator i hello webbservergrupp.</span><span class="sxs-lookup"><span data-stu-id="85f22-127">**tooenable cookie-based affinity**: An application gateway can be configured tooensure that a request from a client session is always directed toohello same VM in hello web farm.</span></span> <span data-ttu-id="85f22-128">Det här scenariot sker genom injektion av en sessions-cookie som tillåter trafik för hello gateway toodirect på lämpligt sätt.</span><span class="sxs-lookup"><span data-stu-id="85f22-128">This scenario is done by injection of a session cookie that allows hello gateway toodirect traffic appropriately.</span></span> <span data-ttu-id="85f22-129">Ange tooenable cookie-baserad tillhörighet **CookieBasedAffinity** för*aktiverad* i hello **BackendHttpSettings** element.</span><span class="sxs-lookup"><span data-stu-id="85f22-129">tooenable cookie-based affinity, set **CookieBasedAffinity** too*Enabled* in hello **BackendHttpSettings** element.</span></span>

## <a name="configure-ssl-offload-on-an-existing-application-gateway"></a><span data-ttu-id="85f22-130">Konfigurera SSL-avlastning på en befintlig Programgateway</span><span class="sxs-lookup"><span data-stu-id="85f22-130">Configure SSL offload on an existing application gateway</span></span>

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

## <a name="create-an-application-gateway-with-ssl-offload"></a><span data-ttu-id="85f22-131">Skapa en Programgateway med SSL-avlastning</span><span class="sxs-lookup"><span data-stu-id="85f22-131">Create an application gateway with SSL Offload</span></span>

<span data-ttu-id="85f22-132">hello följande exempel skapar en Programgateway med SSL-avlastning.</span><span class="sxs-lookup"><span data-stu-id="85f22-132">hello following sample creates an application gateway with SSL offload.</span></span>  <span data-ttu-id="85f22-133">hello certifikat och lösenord för certifikatet måste vara uppdaterade tooa giltig privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="85f22-133">hello certificate and certificate password must be updated tooa valid private key.</span></span>

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

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="85f22-134">Hämta DNS-namn för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="85f22-134">Get application gateway DNS name</span></span>

<span data-ttu-id="85f22-135">När du har skapat hello gateway är hello nästa steg tooconfigure hello klientdelen för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="85f22-135">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="85f22-136">När du använder en offentlig IP-adress krävs ett dynamiskt tilldelat DNS-namn som inte är användarvänligt.</span><span class="sxs-lookup"><span data-stu-id="85f22-136">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="85f22-137">tooensure slutanvändare kan träffa hello Programgateway, en CNAME-post kan vara används toopoint toohello offentlig slutpunkt för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="85f22-137">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="85f22-138">[Konfigurera ett eget domännamn i Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="85f22-138">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="85f22-139">tooconfigure ett alias, hämta information om hello Programgateway och dess associerade IP DNS-namn med hjälp av hello PublicIPAddress element bifogade toohello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="85f22-139">tooconfigure an alias, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="85f22-140">hello programmet gateway DNS-namn ska använda toocreate en CNAME-post som pekar hello två web program toothis DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="85f22-140">hello application gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="85f22-141">hello rekommenderas A-poster inte eftersom hello VIP ändras vid omstart för Programgateway.</span><span class="sxs-lookup"><span data-stu-id="85f22-141">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>


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

## <a name="next-steps"></a><span data-ttu-id="85f22-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="85f22-142">Next steps</span></span>

<span data-ttu-id="85f22-143">Om du vill tooconfigure ett program gateway toouse med en intern belastningsutjämnare (ILB), se [skapa en Programgateway med en intern belastningsutjämnare (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="85f22-143">If you want tooconfigure an application gateway toouse with an internal load balancer (ILB), see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="85f22-144">Om du vill ha mer information om belastningsutjämningsalternativ i allmänhet läser du:</span><span class="sxs-lookup"><span data-stu-id="85f22-144">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="85f22-145">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="85f22-145">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="85f22-146">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="85f22-146">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)
