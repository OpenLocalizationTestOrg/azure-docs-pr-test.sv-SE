---
title: "aaaCreate en anpassad avsökning - Azure Application Gateway - PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate en anpassad avsökning för Programgateway med PowerShell i Resource Manager"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 68feb660-7fa4-4f69-a7e4-bdd7bdc474db
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: gwallace
ms.openlocfilehash: 44c9ffa75401d6d0db023e66fa82c701fb0cf8bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-probe-for-azure-application-gateway-by-using-powershell-for-azure-resource-manager"></a><span data-ttu-id="a374c-103">Skapa en anpassad avsökningsåtgärd för Programgateway i Azure med hjälp av PowerShell för Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a374c-103">Create a custom probe for Azure Application Gateway by using PowerShell for Azure Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="a374c-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a374c-104">Azure portal</span></span>](application-gateway-create-probe-portal.md)
> * [<span data-ttu-id="a374c-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a374c-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-probe-ps.md)
> * [<span data-ttu-id="a374c-106">PowerShell och den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a374c-106">Azure Classic PowerShell</span></span>](application-gateway-create-probe-classic-ps.md)

<span data-ttu-id="a374c-107">Lägg till en anpassad avsökningsåtgärd tooan befintliga Programgateway med PowerShell i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a374c-107">In this article, you add a custom probe tooan existing application gateway with PowerShell.</span></span> <span data-ttu-id="a374c-108">Anpassade avsökningar är användbara för program som har ett visst hälsotillstånd Kontrollera sidan eller för program som inte tillhandahåller ett lyckat svar på hello standardwebbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="a374c-108">Custom probes are useful for applications that have a specific health check page or for applications that do not provide a successful response on hello default web application.</span></span>

> [!NOTE]
> <span data-ttu-id="a374c-109">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="a374c-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="a374c-110">Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello [klassiska distributionsmodellen](application-gateway-create-probe-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="a374c-110">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](application-gateway-create-probe-classic-ps.md).</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway-with-a-custom-probe"></a><span data-ttu-id="a374c-111">Skapa en Programgateway med en anpassad avsökningsåtgärd</span><span class="sxs-lookup"><span data-stu-id="a374c-111">Create an application gateway with a custom probe</span></span>

### <a name="sign-in-and-create-resource-group"></a><span data-ttu-id="a374c-112">Logga in och skapa resursgrupp</span><span class="sxs-lookup"><span data-stu-id="a374c-112">Sign in and create resource group</span></span>

1. <span data-ttu-id="a374c-113">Använd `Login-AzureRmAccount` tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="a374c-113">Use `Login-AzureRmAccount` tooauthenticate.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

1. <span data-ttu-id="a374c-114">Hämta hello prenumerationer för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="a374c-114">Get hello subscriptions for hello account.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

1. <span data-ttu-id="a374c-115">Välj vilka av dina Azure-prenumerationer toouse.</span><span class="sxs-lookup"><span data-stu-id="a374c-115">Choose which of your Azure subscriptions toouse.</span></span>

  ```powershell
  Select-AzureRmSubscription -Subscriptionid '{subscriptionGuid}'
  ```

1. <span data-ttu-id="a374c-116">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a374c-116">Create a resource group.</span></span> <span data-ttu-id="a374c-117">Du kan hoppa över det här steget om du har en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a374c-117">You can skip this step if you have an existing resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name appgw-rg -Location 'West US'
  ```

<span data-ttu-id="a374c-118">Azure Resource Manager kräver att alla resursgrupper anger en plats.</span><span class="sxs-lookup"><span data-stu-id="a374c-118">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="a374c-119">Den här platsen används som hello standardplatsen för resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="a374c-119">This location is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="a374c-120">Se till att alla kommandon toocreate ett program gateway används hello samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a374c-120">Make sure that all commands toocreate an application gateway use hello same resource group.</span></span>

<span data-ttu-id="a374c-121">I föregående exempel hello, vi har skapat en resursgrupp med namnet **appgw RG** plats **västra USA**.</span><span class="sxs-lookup"><span data-stu-id="a374c-121">In hello preceding example, we created a resource group called **appgw-RG** in location **West US**.</span></span>

### <a name="create-a-virtual-network-and-a-subnet"></a><span data-ttu-id="a374c-122">Skapa ett virtuellt nätverk och ett undernät</span><span class="sxs-lookup"><span data-stu-id="a374c-122">Create a virtual network and a subnet</span></span>

<span data-ttu-id="a374c-123">hello följande exempel skapas ett virtuellt nätverk och ett undernät för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="a374c-123">hello following example creates a virtual network and a subnet for hello application gateway.</span></span> <span data-ttu-id="a374c-124">Programgateway kräver sin egen undernät för användning.</span><span class="sxs-lookup"><span data-stu-id="a374c-124">Application gateway requires its own subnet for use.</span></span> <span data-ttu-id="a374c-125">Hello-undernät som skapats för hello Programgateway bör vara mindre än hello adressutrymme hello VNET tooallow för andra undernät toobe skapas och används därför.</span><span class="sxs-lookup"><span data-stu-id="a374c-125">For this reason, hello subnet created for hello application gateway should be smaller than hello address space of hello VNET tooallow for other subnets toobe created and used.</span></span>

```powershell
# Assign hello address range 10.0.0.0/24 tooa subnet variable toobe used toocreate a virtual network.
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

# Create a virtual network named appgwvnet in resource group appgw-rg for hello West US region using hello prefix 10.0.0.0/16 with subnet 10.0.0.0/24.
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet

# Assign a subnet variable for hello next steps, which create an application gateway.
$subnet = $vnet.Subnets[0]
```

### <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a><span data-ttu-id="a374c-126">Skapa en offentlig IP-adress för hello frontend-konfiguration</span><span class="sxs-lookup"><span data-stu-id="a374c-126">Create a public IP address for hello front-end configuration</span></span>

<span data-ttu-id="a374c-127">Skapa en offentlig IP-resurs **publicIP01** i resursgruppen **appgw rg** för hello västra USA region.</span><span class="sxs-lookup"><span data-stu-id="a374c-127">Create a public IP resource **publicIP01** in resource group **appgw-rg** for hello West US region.</span></span> <span data-ttu-id="a374c-128">Det här exemplet används en offentlig IP-adress för hello frontend IP-adress hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="a374c-128">This example uses a public IP address for hello front-end IP address of hello application gateway.</span></span>  <span data-ttu-id="a374c-129">Programgateway kräver hello offentliga IP-adress toohave ett dynamiskt skapade DNS-namn därför hello `-DomainNameLabel` kan inte anges under hello skapandet av hello offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="a374c-129">Application gateway requires hello public IP address toohave a dynamically created DNS name therefore hello `-DomainNameLabel` cannot be specified during hello creation of hello public IP address.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name publicIP01 -Location 'West US' -AllocationMethod Dynamic
```

### <a name="create-an-application-gateway"></a><span data-ttu-id="a374c-130">Skapa en programgateway</span><span class="sxs-lookup"><span data-stu-id="a374c-130">Create an application gateway</span></span>

<span data-ttu-id="a374c-131">Du ställa in alla konfigurationsobjekt innan du skapar hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="a374c-131">You set up all configuration items before creating hello application gateway.</span></span> <span data-ttu-id="a374c-132">hello skapar följande exempel hello konfigurationsobjekt som behövs för en gateway programresursen.</span><span class="sxs-lookup"><span data-stu-id="a374c-132">hello following example creates hello configuration items that are needed for an application gateway resource.</span></span>

| <span data-ttu-id="a374c-133">**Komponent**</span><span class="sxs-lookup"><span data-stu-id="a374c-133">**Component**</span></span> | <span data-ttu-id="a374c-134">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="a374c-134">**Description**</span></span> |
|---|---|
| <span data-ttu-id="a374c-135">**Gateway-IP-konfiguration**</span><span class="sxs-lookup"><span data-stu-id="a374c-135">**Gateway IP configuration**</span></span> | <span data-ttu-id="a374c-136">En IP-konfiguration för en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="a374c-136">An IP configuration for an application gateway.</span></span>|
| <span data-ttu-id="a374c-137">**Serverdelspool**</span><span class="sxs-lookup"><span data-stu-id="a374c-137">**Backend pool**</span></span> | <span data-ttu-id="a374c-138">En pool med IP-adresser, FQDN eller nätverkskort som är toohello programservrar som värd för hello webbprogrammet</span><span class="sxs-lookup"><span data-stu-id="a374c-138">A pool of IP addresses, FQDN's, or NICs that are toohello application servers that host hello web application</span></span>|
| <span data-ttu-id="a374c-139">**Hälsoavsökningen**</span><span class="sxs-lookup"><span data-stu-id="a374c-139">**Health probe**</span></span> | <span data-ttu-id="a374c-140">En anpassad avsökning används toomonitor hello hälsotillstånd hello backend-poolmedlemmar</span><span class="sxs-lookup"><span data-stu-id="a374c-140">A custom probe used toomonitor hello health of hello backend pool members</span></span>|
| <span data-ttu-id="a374c-141">**HTTP-inställningar**</span><span class="sxs-lookup"><span data-stu-id="a374c-141">**HTTP settings**</span></span> | <span data-ttu-id="a374c-142">En samling inställningar inklusive, port, protokoll, cookie-baserad tillhörighet, avsökning och timeout.</span><span class="sxs-lookup"><span data-stu-id="a374c-142">A collection of settings including, port, protocol, cookie-based affinity, probe, and timeout.</span></span>  <span data-ttu-id="a374c-143">De här inställningarna avgör hur trafiken är routade toohello backend-poolmedlemmar</span><span class="sxs-lookup"><span data-stu-id="a374c-143">These settings determine how traffic is routed toohello backend pool members</span></span>|
| <span data-ttu-id="a374c-144">**Klientdelsport**</span><span class="sxs-lookup"><span data-stu-id="a374c-144">**Frontend port**</span></span> | <span data-ttu-id="a374c-145">hello-port som hello Programgateway lyssnar efter trafik på</span><span class="sxs-lookup"><span data-stu-id="a374c-145">hello port that hello application gateway listens for traffic on</span></span>|
| <span data-ttu-id="a374c-146">**Lyssnare**</span><span class="sxs-lookup"><span data-stu-id="a374c-146">**Listener**</span></span> | <span data-ttu-id="a374c-147">En kombination av protokoll, klientdelens IP-konfiguration och klientdelsport.</span><span class="sxs-lookup"><span data-stu-id="a374c-147">A combination of a protocol, frontend IP configuration, and frontend port.</span></span> <span data-ttu-id="a374c-148">Det här är vad lyssnar efter inkommande begäranden.</span><span class="sxs-lookup"><span data-stu-id="a374c-148">This is what listens for incoming requests.</span></span>
|<span data-ttu-id="a374c-149">**Regeln**</span><span class="sxs-lookup"><span data-stu-id="a374c-149">**Rule**</span></span>| <span data-ttu-id="a374c-150">Vägar hello trafik toohello lämplig backend baserat på HTTP-inställningar.</span><span class="sxs-lookup"><span data-stu-id="a374c-150">Routes hello traffic toohello appropriate backend based on HTTP settings.</span></span>|

```powershell
# Creates a application gateway Frontend IP configuration named gatewayIP01
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

#Creates a back-end IP address pool named pool01 with IP addresses 134.170.185.46, 134.170.188.221, 134.170.185.50.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

# Creates a probe that will check health at http://contoso.com/path/path.htm
$probe = New-AzureRmApplicationGatewayProbeConfig -Name probe01 -Protocol Http -HostName 'contoso.com' -Path '/path/path.htm' -Interval 30 -Timeout 120 -UnhealthyThreshold 8

# Creates hello backend http settings toobe used. This component references hello $probe created in hello previous command.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 80

# Creates a frontend port for hello application gateway toolisten on port 80 that will be used by hello listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01 -Port 80

# Creates a frontend IP configuration. This associates hello $publicip variable defined previously with hello front-end IP that will be used by hello listener.
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

# Creates hello listener. hello listener is a combination of protocol and hello frontend IP configuration $fipconfig and frontend port $fp created in previous steps.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

# Creates hello rule that routes traffic toohello backend pools.  In this example we create a basic rule that uses hello previous defined http settings and backend address pool.  It also associates hello listener toohello rule
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Sets hello SKU of hello application gateway, in this example we create a small standard application gateway with 2 instances.
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

# hello final step creates hello application gateway with all hello previously defined components.
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location 'West US' -BackendAddressPools $pool -Probes $probe -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

## <a name="add-a-probe-tooan-existing-application-gateway"></a><span data-ttu-id="a374c-151">Lägg till en avsökning tooan befintliga Programgateway</span><span class="sxs-lookup"><span data-stu-id="a374c-151">Add a probe tooan existing application gateway</span></span>

<span data-ttu-id="a374c-152">hello följande kodavsnitt lägger till en avsökning tooan befintliga Programgateway.</span><span class="sxs-lookup"><span data-stu-id="a374c-152">hello following code snippet adds a probe tooan existing application gateway.</span></span>

```powershell
# Load hello application gateway resource into a PowerShell variable by using Get-AzureRmApplicationGateway.
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

# Create hello probe object that will check health at http://contoso.com/path/path.htm
$getgw = Add-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name probe01 -Protocol Http -HostName 'contoso.com' -Path '/path/custompath.htm' -Interval 30 -Timeout 120 -UnhealthyThreshold 8

# Set hello backend HTTP settings toouse hello new probe
$getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 120

# Save hello application gateway with hello configuration changes
Set-AzureRmApplicationGateway -ApplicationGateway $getgw
```

## <a name="remove-a-probe-from-an-existing-application-gateway"></a><span data-ttu-id="a374c-153">Ta bort en avsökning från en befintlig Programgateway</span><span class="sxs-lookup"><span data-stu-id="a374c-153">Remove a probe from an existing application gateway</span></span>

<span data-ttu-id="a374c-154">hello följande kodavsnitt tar bort en avsökning från en befintlig gateway för programmet.</span><span class="sxs-lookup"><span data-stu-id="a374c-154">hello following code snippet removes a probe from an existing application gateway.</span></span>

```powershell
# Load hello application gateway resource into a PowerShell variable by using Get-AzureRmApplicationGateway.
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

# Remove hello probe from hello application gateway configuration object
$getgw = Remove-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name $getgw.Probes.name

# Set hello backend HTTP settings tooremove hello reference toohello probe. hello backend http settings now use hello default probe
$getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol http -CookieBasedAffinity Disabled

# Save hello application gateway with hello configuration changes
Set-AzureRmApplicationGateway -ApplicationGateway $getgw
```

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="a374c-155">Hämta DNS-namn för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="a374c-155">Get application gateway DNS name</span></span>

<span data-ttu-id="a374c-156">När du har skapat hello gateway är hello nästa steg tooconfigure hello klientdelen för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="a374c-156">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="a374c-157">När du använder en offentlig IP-adress krävs ett dynamiskt tilldelat DNS-namn som inte är användarvänligt.</span><span class="sxs-lookup"><span data-stu-id="a374c-157">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="a374c-158">tooensure slutanvändare kan träffa hello Programgateway en CNAME-post kan användas för toopoint toohello offentlig slutpunkt för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="a374c-158">tooensure end users can hit hello application gateway a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="a374c-159">[Konfigurera ett eget domännamn i Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a374c-159">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="a374c-160">toodo detta, hämta information om hello Programgateway och dess associerade IP DNS-namn med hjälp av hello PublicIPAddress element bifogade toohello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="a374c-160">toodo this, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="a374c-161">hello programmet gateway DNS-namn ska använda toocreate en CNAME-post som pekar hello två web program toothis DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="a374c-161">hello application gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="a374c-162">hello rekommenderas A-poster inte eftersom hello VIP ändras vid omstart för Programgateway.</span><span class="sxs-lookup"><span data-stu-id="a374c-162">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : appgw-RG
Location                 : westus
Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="next-steps"></a><span data-ttu-id="a374c-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a374c-163">Next steps</span></span>

<span data-ttu-id="a374c-164">Läs tooconfigure SSL genom att avlasta genom att besöka: [Konfigurera SSL-avlastning](application-gateway-ssl-arm.md)</span><span class="sxs-lookup"><span data-stu-id="a374c-164">Learn tooconfigure SSL offloading by visiting: [Configure SSL Offload](application-gateway-ssl-arm.md)</span></span>

