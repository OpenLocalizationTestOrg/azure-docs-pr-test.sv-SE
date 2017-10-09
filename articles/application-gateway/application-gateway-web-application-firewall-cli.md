---
title: "aaaConfigure Brandvägg för webbaserade program - Azure Programgateway | Microsoft Docs"
description: "Den här artikeln innehåller råd om hur toostart med hjälp av web application-brandväggen på en befintlig eller ny Programgateway."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 670b9732-874b-43e6-843b-d2585c160982
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: gwallace
ms.openlocfilehash: d5354984760ceab12ed49efa9e18836e9f1d3c96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway-with-azure-cli"></a><span data-ttu-id="574b5-103">Konfigurera Brandvägg för webbaserade program på en ny eller befintlig Programgateway med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="574b5-103">Configure web application firewall on a new or existing Application Gateway with Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="574b5-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="574b5-104">Azure portal</span></span>](application-gateway-web-application-firewall-portal.md)
> * [<span data-ttu-id="574b5-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="574b5-105">PowerShell</span></span>](application-gateway-web-application-firewall-powershell.md)
> * [<span data-ttu-id="574b5-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="574b5-106">Azure CLI</span></span>](application-gateway-web-application-firewall-cli.md)

<span data-ttu-id="574b5-107">Lär dig hur toocreate Brandvägg för webbaserade program aktiverat Programgateway eller Lägg till web application brandväggen tooan befintliga Programgateway.</span><span class="sxs-lookup"><span data-stu-id="574b5-107">Learn how toocreate a web application firewall enabled application gateway or add web application firewall tooan existing application gateway.</span></span>

<span data-ttu-id="574b5-108">hello Brandvägg för webbaserade program (Brandvägg) i Azure Application Gateway skyddar webbprogram från vanliga webbaserade attacker som SQL injection, webbplatser scripting attacker och sessionen hijacks.</span><span class="sxs-lookup"><span data-stu-id="574b5-108">hello web application firewall (WAF) in Azure Application Gateway protects web applications from common web-based attacks like SQL injection, cross-site scripting attacks, and session hijacks.</span></span>

<span data-ttu-id="574b5-109">Azure Application Gateway är en Layer 7-belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="574b5-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="574b5-110">Det ger redundans, prestanda-routing HTTP-begäranden mellan olika servrar, oavsett om de är på hello molnet eller lokalt.</span><span class="sxs-lookup"><span data-stu-id="574b5-110">It provides failover, performance-routing HTTP requests between different servers, whether they are on hello cloud or on-premises.</span></span> <span data-ttu-id="574b5-111">Programgateway innehåller många program leverans (ADC) funktioner i nätverksstyrenheten inklusive HTTP läsa in belastningsutjämning, cookie-baserad session tillhörighet, secure sockets layer (SSL)-avlastning, anpassade hälsoavsökningar, stöd för flera platser och många andra.</span><span class="sxs-lookup"><span data-stu-id="574b5-111">Application gateway provides many application delivery controller (ADC) features including HTTP load balancing, cookie-based session affinity, secure sockets layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="574b5-112">Besök toofind en fullständig lista över funktioner som stöds: [översikt för Programgateway](application-gateway-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="574b5-112">toofind a complete list of supported features, visit: [Overview of Application Gateway](application-gateway-introduction.md).</span></span>

<span data-ttu-id="574b5-113">hello följande artikeln visar hur för[lägga till web application brandväggen tooan befintliga Programgateway](#add-web-application-firewall-to-an-existing-application-gateway) och [skapa en Programgateway som använder Brandvägg för webbaserade program](#create-an-application-gateway-with-web-application-firewall).</span><span class="sxs-lookup"><span data-stu-id="574b5-113">hello following article shows how too[add web application firewall tooan existing application gateway](#add-web-application-firewall-to-an-existing-application-gateway) and [create an application gateway that uses web application firewall](#create-an-application-gateway-with-web-application-firewall).</span></span>

![scenario-bild][scenario]

## <a name="prerequisite-install-hello-azure-cli-20"></a><span data-ttu-id="574b5-115">Förutsättning: Installera hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="574b5-115">Prerequisite: Install hello Azure CLI 2.0</span></span>

<span data-ttu-id="574b5-116">tooperform hello stegen i den här artikeln, måste du för[installerar hello Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="574b5-116">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="waf-configuration-differences"></a><span data-ttu-id="574b5-117">Brandvägg configuration skillnader</span><span class="sxs-lookup"><span data-stu-id="574b5-117">WAF configuration differences</span></span>

<span data-ttu-id="574b5-118">Om du har läst [skapa en Programgateway med Azure CLI](application-gateway-create-gateway-cli.md), du förstår hello SKU inställningar tooconfigure när du skapar en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="574b5-118">If you have read [Create an Application Gateway with Azure CLI](application-gateway-create-gateway-cli.md), you understand hello SKU settings tooconfigure when creating an application gateway.</span></span> <span data-ttu-id="574b5-119">Brandvägg ger ytterligare inställningar toodefine när du konfigurerar hello SKU: N på en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="574b5-119">WAF provides additional settings toodefine when configuring hello SKU on an application gateway.</span></span> <span data-ttu-id="574b5-120">Det finns inga ytterligare ändringar du gör på hello Programgateway sig själv.</span><span class="sxs-lookup"><span data-stu-id="574b5-120">There are no additional changes that you make on hello application gateway itself.</span></span>

| <span data-ttu-id="574b5-121">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="574b5-121">**Setting**</span></span> | <span data-ttu-id="574b5-122">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="574b5-122">**Details**</span></span>
|---|---|
|<span data-ttu-id="574b5-123">**SKU**</span><span class="sxs-lookup"><span data-stu-id="574b5-123">**SKU**</span></span> |<span data-ttu-id="574b5-124">En normal Programgateway utan Brandvägg stöder **Standard\_små**, **Standard\_medel**, och **Standard\_stor** storlekar.</span><span class="sxs-lookup"><span data-stu-id="574b5-124">A normal application gateway without WAF supports **Standard\_Small**, **Standard\_Medium**, and **Standard\_Large** sizes.</span></span> <span data-ttu-id="574b5-125">Med hello införandet av Brandvägg, det finns två ytterligare SKU: er, **Brandvägg\_medel** och **Brandvägg\_stor**.</span><span class="sxs-lookup"><span data-stu-id="574b5-125">With hello introduction of WAF, there are two additional SKUs, **WAF\_Medium** and **WAF\_Large**.</span></span> <span data-ttu-id="574b5-126">Brandvägg stöds inte på små programgatewayer.</span><span class="sxs-lookup"><span data-stu-id="574b5-126">WAF is not supported on small application gateways.</span></span>|
|<span data-ttu-id="574b5-127">**Läge**</span><span class="sxs-lookup"><span data-stu-id="574b5-127">**Mode**</span></span> | <span data-ttu-id="574b5-128">Den här inställningen är hello läge för Brandvägg.</span><span class="sxs-lookup"><span data-stu-id="574b5-128">This setting is hello mode of WAF.</span></span> <span data-ttu-id="574b5-129">tillåtna värden är **identifiering** och **förebyggande**.</span><span class="sxs-lookup"><span data-stu-id="574b5-129">allowed values are **Detection** and **Prevention**.</span></span> <span data-ttu-id="574b5-130">När Brandvägg är inställd i identifieringsläget lagras alla hot i en loggfil.</span><span class="sxs-lookup"><span data-stu-id="574b5-130">When WAF is set up in detection mode, all threats are stored in a log file.</span></span> <span data-ttu-id="574b5-131">Händelser loggas fortfarande i förebyggande läge, men hello angripare tar emot ett 403 obehörig svar från hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="574b5-131">In prevention mode, events are still logged but hello attacker receives a 403 unauthorized response from hello application gateway.</span></span>|

## <a name="add-web-application-firewall-tooan-existing-application-gateway"></a><span data-ttu-id="574b5-132">Lägg till web application brandväggen tooan befintliga Programgateway</span><span class="sxs-lookup"><span data-stu-id="574b5-132">Add web application firewall tooan existing application gateway</span></span>

<span data-ttu-id="574b5-133">hello Följ kommandot ändringar en befintlig standard gateway tooa Brandvägg aktiverade programmet Programgateway.</span><span class="sxs-lookup"><span data-stu-id="574b5-133">hello follow command changes an existing standard application gateway tooa WAF enabled application gateway.</span></span>

```azurecli-interactive
#!/bin/bash

az network application-gateway waf-config set \
  --enabled true \
  --firewall-mode Prevention \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"
```

<span data-ttu-id="574b5-134">Det här kommandot uppdaterar hello Programgateway med Brandvägg för webbaserade program.</span><span class="sxs-lookup"><span data-stu-id="574b5-134">This command updates hello application gateway with web application firewall.</span></span> <span data-ttu-id="574b5-135">Besök [Gateway Programdiagnostik](application-gateway-diagnostics.md) toounderstand hur tooview loggar för din Programgateway.</span><span class="sxs-lookup"><span data-stu-id="574b5-135">Visit [Application Gateway Diagnostics](application-gateway-diagnostics.md) toounderstand how tooview logs for your application gateway.</span></span> <span data-ttu-id="574b5-136">På grund av toohello säkerhet uppbyggnad Brandvägg, loggar måste toobe ses över regelbundet toounderstand hello säkerhetstillståndet webbprogram.</span><span class="sxs-lookup"><span data-stu-id="574b5-136">Due toohello security nature of WAF, logs need toobe reviewed regularly toounderstand hello security posture of your web applications.</span></span>

## <a name="create-an-application-gateway-with-web-application-firewall"></a><span data-ttu-id="574b5-137">Skapa en Programgateway med Brandvägg för webbaserade program</span><span class="sxs-lookup"><span data-stu-id="574b5-137">Create an Application Gateway with web application firewall</span></span>

<span data-ttu-id="574b5-138">hello följande kommando skapar en Programgateway med Brandvägg för webbaserade program.</span><span class="sxs-lookup"><span data-stu-id="574b5-138">hello following command creates an Application Gateway with web application firewall.</span></span>

```azurecli-interactive
#!/bin/bash

az network application-gateway create \
  --name "AdatumAppGateway2" \
  --location "eastus" \
  --resource-group "AdatumAppGatewayRG" \
  --vnet-name "AdatumAppGatewayVNET2" \
  --vnet-address-prefix "10.0.0.0/16" \
  --subnet "Appgatewaysubnet2" \
  --subnet-address-prefix "10.0.0.0/28" \
 --servers "10.0.0.5 10.0.0.4" \
  --capacity 2 
  --sku "WAF_Medium" \
  --http-settings-cookie-based-affinity "Enabled" \
  --http-settings-protocol "Http" \
  --frontend-port "80" \
  --routing-rule-type "Basic" \
  --http-settings-port "80" \
  --public-ip-address "pip2" \
  --public-ip-address-allocation "dynamic" \
  --tags "cli[2] owner[administrator]"
```

> [!NOTE]
> <span data-ttu-id="574b5-139">Programgatewayer som skapats med konfiguration av hello grundläggande webbprogram brandväggen konfigureras med CR 3.0 för skydd.</span><span class="sxs-lookup"><span data-stu-id="574b5-139">Application gateways created with hello basic web application firewall configuration are configured with CRS 3.0 for protections.</span></span>

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="574b5-140">Hämta DNS-namn för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="574b5-140">Get application gateway DNS name</span></span>

<span data-ttu-id="574b5-141">När du har skapat hello gateway är hello nästa steg tooconfigure hello klientdelen för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="574b5-141">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="574b5-142">När du använder en offentlig IP-adress krävs ett dynamiskt tilldelat DNS-namn som inte är användarvänligt.</span><span class="sxs-lookup"><span data-stu-id="574b5-142">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="574b5-143">tooensure slutanvändare kan träffa hello Programgateway, en CNAME-post kan vara används toopoint toohello offentlig slutpunkt för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="574b5-143">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="574b5-144">[Konfigurera ett eget domännamn i Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="574b5-144">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="574b5-145">tooconfigure en CNAME-post att hämta information om hello Programgateway och dess associerade IP DNS-namn med hjälp av hello PublicIPAddress element bifogade toohello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="574b5-145">tooconfigure a CNAME record, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="574b5-146">hello programmet gateway DNS-namn ska använda toocreate en CNAME-post som pekar hello två web program toothis DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="574b5-146">hello application gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="574b5-147">hello rekommenderas A-poster inte eftersom hello VIP ändras vid omstart för Programgateway.</span><span class="sxs-lookup"><span data-stu-id="574b5-147">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>

```azurecli-interactive
#!/bin/bash

az network public-ip show \
  --name pip2 \
  --resource-group "AdatumAppGatewayRG"
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

## <a name="next-steps"></a><span data-ttu-id="574b5-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="574b5-148">Next steps</span></span>

<span data-ttu-id="574b5-149">Lär dig hur toocustomize Brandvägg regler genom att besöka: [anpassa web application brandväggsregler via hello Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md).</span><span class="sxs-lookup"><span data-stu-id="574b5-149">Learn how toocustomize WAF rules by visiting: [Customize web application firewall rules through hello Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md).</span></span>

[scenario]: ./media/application-gateway-web-application-firewall-cli/scenario.png
