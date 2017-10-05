---
title: Konfigurera SSL-avlastning - Azure Application Gateway - Azure CLI 2.0 | Microsoft Docs
description: "Den här sidan innehåller instruktioner för att skapa en Programgateway med SSL-avlastning av Azure CLI 2.0"
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
ms.openlocfilehash: e8c1ba09daef09ef5002e33345905772961c1d93
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-cli-20"></a><span data-ttu-id="31ad2-103">Konfigurera en Programgateway för SSL-avlastning med hjälp av Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="31ad2-103">Configure an application gateway for SSL offload by using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="31ad2-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="31ad2-104">Azure portal</span></span>](application-gateway-ssl-portal.md)
> * [<span data-ttu-id="31ad2-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="31ad2-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ssl-arm.md)
> * [<span data-ttu-id="31ad2-106">PowerShell och den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="31ad2-106">Azure Classic PowerShell</span></span>](application-gateway-ssl.md)
> * [<span data-ttu-id="31ad2-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="31ad2-107">Azure CLI 2.0</span></span>](application-gateway-ssl-cli.md)

<span data-ttu-id="31ad2-108">Azure Application Gateway kan konfigureras att avsluta SSL-sessionen (Secure Sockets Layer) på gatewayen så att du undviker kostsamma SSL-dekrypteringsaktiviteter i webbservergruppen.</span><span class="sxs-lookup"><span data-stu-id="31ad2-108">Azure Application Gateway can be configured to terminate the Secure Sockets Layer (SSL) session at the gateway to avoid costly SSL decryption tasks to happen at the web farm.</span></span> <span data-ttu-id="31ad2-109">SSL-avlastning förenklar också certifikathantering på frontend-servern.</span><span class="sxs-lookup"><span data-stu-id="31ad2-109">SSL offload also simplifies certificate management at the front-end server.</span></span>

## <a name="prerequisite-install-the-azure-cli-20"></a><span data-ttu-id="31ad2-110">Förutsättning: Installera Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="31ad2-110">Prerequisite: Install the Azure CLI 2.0</span></span>

<span data-ttu-id="31ad2-111">Om du vill utföra stegen i den här artikeln, måste du [installera Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="31ad2-111">To perform the steps in this article, you need to [install the Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="required-components"></a><span data-ttu-id="31ad2-112">Nödvändiga komponenter</span><span class="sxs-lookup"><span data-stu-id="31ad2-112">Required components</span></span>

* <span data-ttu-id="31ad2-113">**Backend-serverpool:** Listan med IP-adresser för backend-servrarna.</span><span class="sxs-lookup"><span data-stu-id="31ad2-113">**Back-end server pool:** The list of IP addresses of the back-end servers.</span></span> <span data-ttu-id="31ad2-114">IP-adresserna som anges bör antingen tillhöra det virtuella undernätet eller vara en offentlig IP-/VIP-adress.</span><span class="sxs-lookup"><span data-stu-id="31ad2-114">The IP addresses listed should either belong to the virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="31ad2-115">**Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet.</span><span class="sxs-lookup"><span data-stu-id="31ad2-115">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="31ad2-116">Dessa inställningar är knutna till en pool och tillämpas på alla servrar i poolen.</span><span class="sxs-lookup"><span data-stu-id="31ad2-116">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="31ad2-117">**Frontend-port:** Den här porten är den offentliga porten som är öppen på programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="31ad2-117">**Front-end port:** This port is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="31ad2-118">Trafiken kommer till den här porten och omdirigeras till en av backend-servrarna.</span><span class="sxs-lookup"><span data-stu-id="31ad2-118">Traffic hits this port, and then gets redirected to one of the back-end servers.</span></span>
* <span data-ttu-id="31ad2-119">**Lyssnare:** Lyssnaren har en frontend-port, ett protokoll (Http eller Https; dessa inställningar är skiftlägeskänsliga) och SSL-certifikatnamnet (om du konfigurerar SSL-avlastning).</span><span class="sxs-lookup"><span data-stu-id="31ad2-119">**Listener:** The listener has a front-end port, a protocol (Http or Https, these settings are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="31ad2-120">**Regel:** Regeln binder lyssnaren och backend-serverpoolen och definierar vilken backend-serverpool som trafiken ska dirigeras till när den når en viss lyssnare.</span><span class="sxs-lookup"><span data-stu-id="31ad2-120">**Rule:** The rule binds the listener and the back-end server pool and defines which back-end server pool the traffic should be directed to when it hits a particular listener.</span></span> <span data-ttu-id="31ad2-121">För närvarande stöds endast regeln *basic*.</span><span class="sxs-lookup"><span data-stu-id="31ad2-121">Currently, only the *basic* rule is supported.</span></span> <span data-ttu-id="31ad2-122">Regeln *basic* använder belastningsutjämning med resursallokering.</span><span class="sxs-lookup"><span data-stu-id="31ad2-122">The *basic* rule is round-robin load distribution.</span></span>

<span data-ttu-id="31ad2-123">**Information om ytterligare konfiguration**</span><span class="sxs-lookup"><span data-stu-id="31ad2-123">**Additional configuration notes**</span></span>

<span data-ttu-id="31ad2-124">För konfiguration av SSL-certifikat bör protokollet i **HttpListener** ändras till *Https* (skiftlägeskänsligt).</span><span class="sxs-lookup"><span data-stu-id="31ad2-124">For SSL certificates configuration, the protocol in **HttpListener** should change to *Https* (case sensitive).</span></span> <span data-ttu-id="31ad2-125">Elementet **SslCertificate** läggs till i **HttpListener** med variabelvärdet som har konfigurerats för SSL-certifikatet.</span><span class="sxs-lookup"><span data-stu-id="31ad2-125">The **SslCertificate** element is added to **HttpListener** with the variable value configured for the SSL certificate.</span></span> <span data-ttu-id="31ad2-126">Frontend-porten måste uppdateras till 443.</span><span class="sxs-lookup"><span data-stu-id="31ad2-126">The front-end port should be updated to 443.</span></span>

<span data-ttu-id="31ad2-127">**Aktivera cookiebaserad tillhörighet**: En programgateway kan konfigureras att säkerställa att en begäran från en klientsession alltid dirigeras till samma virtuella dator i webbservergruppen.</span><span class="sxs-lookup"><span data-stu-id="31ad2-127">**To enable cookie-based affinity**: An application gateway can be configured to ensure that a request from a client session is always directed to the same VM in the web farm.</span></span> <span data-ttu-id="31ad2-128">Det här scenariot görs med en sessions-cookie som ser till att gatewayen dirigerar trafiken på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="31ad2-128">This scenario is done by injection of a session cookie that allows the gateway to direct traffic appropriately.</span></span> <span data-ttu-id="31ad2-129">Du kan aktivera cookiebaserad tillhörighet genom att ange **CookieBasedAffinity** till *Enabled* i elementet **BackendHttpSettings**.</span><span class="sxs-lookup"><span data-stu-id="31ad2-129">To enable cookie-based affinity, set **CookieBasedAffinity** to *Enabled* in the **BackendHttpSettings** element.</span></span>

## <a name="configure-ssl-offload-on-an-existing-application-gateway"></a><span data-ttu-id="31ad2-130">Konfigurera SSL-avlastning på en befintlig Programgateway</span><span class="sxs-lookup"><span data-stu-id="31ad2-130">Configure SSL offload on an existing application gateway</span></span>

```azurecli-interactive
#!/bin/bash

# Create a new front end port to be used for SSL
az network application-gateway frontend-port create \
  --name sslport \
  --port 443 \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Upload the .pfx certificate for SSL offload
az network application-gateway ssl-cert create \
  --name "newcert" \
  --cert-file /home/azureuser/self-signed/AdatumAppGatewayCert.pfx \
  --cert-password P@ssw0rd \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Create a new listener referencing the port and certificate created earlier
az network application-gateway http-listener create \
  --frontend-ip "appGatewayFrontendIP" \
  --frontend-port sslport  \
  --name sslListener \
  --ssl-cert newcert \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Create a new back-end pool to be used
az network application-gateway address-pool create \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG" \
  --name "appGatewayBackendPool2" \
  --servers 10.0.0.7 10.0.0.8

# Create a new back-end HTTP settings using the new probe
az network application-gateway http-settings create \
  --name "settings2" \
  --port 80 \
  --cookie-based-affinity Enabled \
  --protocol "Http" \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Create a new rule linking the listener to the back-end pool
az network application-gateway rule create \
  --name "rule2" \
  --rule-type Basic \
  --http-settings settings2 \
  --http-listener ssllistener \
  --address-pool temp1 \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

```

## <a name="create-an-application-gateway-with-ssl-offload"></a><span data-ttu-id="31ad2-131">Skapa en Programgateway med SSL-avlastning</span><span class="sxs-lookup"><span data-stu-id="31ad2-131">Create an application gateway with SSL Offload</span></span>

<span data-ttu-id="31ad2-132">I följande exempel skapar en Programgateway med SSL-avlastning.</span><span class="sxs-lookup"><span data-stu-id="31ad2-132">The following sample creates an application gateway with SSL offload.</span></span>  <span data-ttu-id="31ad2-133">Certifikat och lösenord för certifikatet måste uppdateras till en giltig privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="31ad2-133">The certificate and certificate password must be updated to a valid private key.</span></span>

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

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="31ad2-134">Hämta DNS-namn för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="31ad2-134">Get application gateway DNS name</span></span>

<span data-ttu-id="31ad2-135">När du har skapat gatewayen, är nästa steg att konfigurera klientprogrammet för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="31ad2-135">Once the gateway is created, the next step is to configure the front end for communication.</span></span> <span data-ttu-id="31ad2-136">När du använder en offentlig IP-adress krävs ett dynamiskt tilldelat DNS-namn som inte är användarvänligt.</span><span class="sxs-lookup"><span data-stu-id="31ad2-136">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="31ad2-137">För att säkerställa att slutanvändare kan nå programgatewayen kan en CNAME-post användas för att peka på den offentliga slutpunkten för programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="31ad2-137">To ensure end users can hit the application gateway, a CNAME record can be used to point to the public endpoint of the application gateway.</span></span> <span data-ttu-id="31ad2-138">[Konfigurera ett eget domännamn i Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="31ad2-138">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="31ad2-139">Hämta information om programgatewayen och dess tillhörande IP DNS-namn med PublicIPAddress-element som bifogas programgatewayen om du vill konfigurera ett alias.</span><span class="sxs-lookup"><span data-stu-id="31ad2-139">To configure an alias, retrieve details of the application gateway and its associated IP/DNS name using the PublicIPAddress element attached to the application gateway.</span></span> <span data-ttu-id="31ad2-140">programgatewayens DNS-namn ska användas för att skapa en CNAME-post som leder de två webbapparna till detta DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="31ad2-140">The application gateway's DNS name should be used to create a CNAME record, which points the two web applications to this DNS name.</span></span> <span data-ttu-id="31ad2-141">Användning av A-poster rekommenderas inte eftersom VIP kan ändras vid omstart av programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="31ad2-141">The use of A-records is not recommended since the VIP may change on restart of application gateway.</span></span>


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

## <a name="next-steps"></a><span data-ttu-id="31ad2-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="31ad2-142">Next steps</span></span>

<span data-ttu-id="31ad2-143">Om du vill konfigurera en programgateway för användning med en intern belastningsutjämnare läser du [Skapa en programgateway med en intern belastningsutjämnare (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="31ad2-143">If you want to configure an application gateway to use with an internal load balancer (ILB), see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="31ad2-144">Om du vill ha mer information om belastningsutjämningsalternativ i allmänhet läser du:</span><span class="sxs-lookup"><span data-stu-id="31ad2-144">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="31ad2-145">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="31ad2-145">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="31ad2-146">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="31ad2-146">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)
