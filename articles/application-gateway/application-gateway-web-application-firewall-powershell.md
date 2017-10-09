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
ms.date: 05/03/2017
ms.author: gwallace
ms.openlocfilehash: bd2a69901f0ec0d6551d633e2588b74c3ab59a71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a><span data-ttu-id="c4ea1-103">Konfigurera Brandvägg för webbaserade program på en ny eller befintlig Programgateway</span><span class="sxs-lookup"><span data-stu-id="c4ea1-103">Configure web application firewall on a new or existing Application Gateway</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c4ea1-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c4ea1-104">Azure portal</span></span>](application-gateway-web-application-firewall-portal.md)
> * [<span data-ttu-id="c4ea1-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c4ea1-105">PowerShell</span></span>](application-gateway-web-application-firewall-powershell.md)
> * [<span data-ttu-id="c4ea1-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c4ea1-106">Azure CLI</span></span>](application-gateway-web-application-firewall-cli.md)

<span data-ttu-id="c4ea1-107">Lär dig hur toocreate Brandvägg för webbaserade program aktiverade Application Gateway eller Lägg till web application brandväggen tooan befintliga Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-107">Learn how toocreate a web application firewall enabled Application Gateway or add web application firewall tooan existing Application Gateway.</span></span>

<span data-ttu-id="c4ea1-108">hello Brandvägg för webbaserade program (Brandvägg) i Azure Application Gateway skyddar webbprogram från vanliga webbaserade attacker som SQL injection, webbplatser scripting attacker och sessionen hijacks.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-108">hello web application firewall (WAF) in Azure Application Gateway protects web applications from common web-based attacks like SQL injection, cross-site scripting attacks, and session hijacks.</span></span>

<span data-ttu-id="c4ea1-109">Azure Application Gateway är en Layer 7-belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="c4ea1-110">Det ger redundans, prestanda-routing HTTP-begäranden mellan olika servrar, oavsett om de är på hello molnet eller lokalt.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-110">It provides failover, performance-routing HTTP requests between different servers, whether they are on hello cloud or on-premises.</span></span> <span data-ttu-id="c4ea1-111">Programgateway innehåller många program leverans (ADC) funktioner i nätverksstyrenheten inklusive HTTP läsa in belastningsutjämning, cookie-baserad session tillhörighet, secure sockets layer (SSL)-avlastning, anpassade hälsoavsökningar, stöd för flera platser och många andra.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-111">Application Gateway provides many application delivery controller (ADC) features including HTTP load balancing, cookie-based session affinity, secure sockets layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="c4ea1-112">Besök toofind en fullständig lista över funktioner som stöds: [översikt för Programgateway](application-gateway-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c4ea1-112">toofind a complete list of supported features, visit: [Overview of Application Gateway](application-gateway-introduction.md).</span></span>

<span data-ttu-id="c4ea1-113">hello följande artikeln visar hur för[lägga till befintliga Programgateway web application brandväggen tooan](#add-web-application-firewall-to-an-existing-application-gateway) och [skapa en Gateway för program som använder Brandvägg för webbaserade program](#create-an-application-gateway-with-web-application-firewall).</span><span class="sxs-lookup"><span data-stu-id="c4ea1-113">hello following article shows how too[add web application firewall tooan existing Application Gateway](#add-web-application-firewall-to-an-existing-application-gateway) and [create an Application Gateway that uses web application firewall](#create-an-application-gateway-with-web-application-firewall).</span></span>

![scenario-bild][scenario]

## <a name="waf-configuration-differences"></a><span data-ttu-id="c4ea1-115">Brandvägg configuration skillnader</span><span class="sxs-lookup"><span data-stu-id="c4ea1-115">WAF configuration differences</span></span>

<span data-ttu-id="c4ea1-116">Om du har läst [skapar en Programgateway med PowerShell](application-gateway-create-gateway-arm.md), du förstår hello SKU inställningar tooconfigure när du skapar en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-116">If you have read [Create an Application Gateway with PowerShell](application-gateway-create-gateway-arm.md), you understand hello SKU settings tooconfigure when creating an Application Gateway.</span></span> <span data-ttu-id="c4ea1-117">Brandvägg ger ytterligare inställningar toodefine när du konfigurerar hello SKU: N på en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-117">WAF provides additional settings toodefine when configuring hello SKU on an Application Gateway.</span></span> <span data-ttu-id="c4ea1-118">Det finns inga ytterligare ändringar du gör på hello Programgateway sig själv.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-118">There are no additional changes that you make on hello Application Gateway itself.</span></span>

| <span data-ttu-id="c4ea1-119">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="c4ea1-119">**Setting**</span></span> | <span data-ttu-id="c4ea1-120">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="c4ea1-120">**Details**</span></span>
|---|---|
|<span data-ttu-id="c4ea1-121">**SKU**</span><span class="sxs-lookup"><span data-stu-id="c4ea1-121">**SKU**</span></span> |<span data-ttu-id="c4ea1-122">En normal Programgateway utan Brandvägg stöder **Standard\_små**, **Standard\_medel**, och **Standard\_stor** storlekar.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-122">A normal Application Gateway without WAF supports **Standard\_Small**, **Standard\_Medium**, and **Standard\_Large** sizes.</span></span> <span data-ttu-id="c4ea1-123">Med hello införandet av Brandvägg, det finns två ytterligare SKU: er, **Brandvägg\_medel** och **Brandvägg\_stor**.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-123">With hello introduction of WAF, there are two additional SKUs, **WAF\_Medium** and **WAF\_Large**.</span></span> <span data-ttu-id="c4ea1-124">Brandvägg stöds inte på små Programgatewayer.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-124">WAF is not supported on small Application Gateways.</span></span>|
|<span data-ttu-id="c4ea1-125">**Nivå**</span><span class="sxs-lookup"><span data-stu-id="c4ea1-125">**Tier**</span></span> | <span data-ttu-id="c4ea1-126">hello tillgängliga värden är **Standard** eller **Brandvägg**.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-126">hello available values are **Standard** or **WAF**.</span></span> <span data-ttu-id="c4ea1-127">När du använder Brandvägg för webbaserade program **Brandvägg** måste väljas.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-127">When using web application firewall, **WAF** must be chosen.</span></span>|
|<span data-ttu-id="c4ea1-128">**Läge**</span><span class="sxs-lookup"><span data-stu-id="c4ea1-128">**Mode**</span></span> | <span data-ttu-id="c4ea1-129">Den här inställningen är hello läge för Brandvägg.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-129">This setting is hello mode of WAF.</span></span> <span data-ttu-id="c4ea1-130">tillåtna värden är **identifiering** och **förebyggande**.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-130">allowed values are **Detection** and **Prevention**.</span></span> <span data-ttu-id="c4ea1-131">När Brandvägg är inställd i identifieringsläget lagras alla hot i en loggfil.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-131">When WAF is set up in detection mode, all threats are stored in a log file.</span></span> <span data-ttu-id="c4ea1-132">Händelser loggas fortfarande i förebyggande läge, men hello angripare tar emot ett 403 obehörig svar från hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-132">In prevention mode, events are still logged but hello attacker receives a 403 unauthorized response from hello Application Gateway.</span></span>|

## <a name="add-web-application-firewall-tooan-existing-application-gateway"></a><span data-ttu-id="c4ea1-133">Lägg till web application brandväggen tooan befintliga Programgateway</span><span class="sxs-lookup"><span data-stu-id="c4ea1-133">Add web application firewall tooan existing Application Gateway</span></span>

<span data-ttu-id="c4ea1-134">Kontrollera att du använder hello senaste versionen av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-134">Make sure that you are using hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="c4ea1-135">Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="c4ea1-135">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

1. <span data-ttu-id="c4ea1-136">Logga in tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-136">Log in tooyour Azure Account.</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

2. <span data-ttu-id="c4ea1-137">Välj hello prenumeration toouse för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-137">Select hello subscription toouse for this scenario.</span></span>

    ```powershell
    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"
    ```

3. <span data-ttu-id="c4ea1-138">Hämta hello-gateway som du lägger till Brandvägg för webbaserade program till.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-138">Retrieve hello gateway that you are adding web application firewall to.</span></span>

    ```powershell
    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"
    ```

1. <span data-ttu-id="c4ea1-139">Konfigurera hello web application firewall sku.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-139">Configure hello web application firewall sku.</span></span> <span data-ttu-id="c4ea1-140">hello tillgängliga storlekar är **Brandvägg\_stor** och **Brandvägg\_medel**.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-140">hello available sizes are **WAF\_Large** and **WAF\_Medium**.</span></span> <span data-ttu-id="c4ea1-141">När Brandvägg för webbaserade program används hello nivå måste vara **Brandvägg**, hello kapacitet måste bekräftas när hello sku.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-141">When web application firewall is used hello tier must be **WAF**, hello capacity must be confirmed when setting hello sku.</span></span>

    ```powershell
    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2
    ```

1. <span data-ttu-id="c4ea1-142">Konfigurera inställningar för hello Brandvägg som har definierats i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="c4ea1-142">Configure hello WAF settings as defined in hello following example:</span></span>

   <span data-ttu-id="c4ea1-143">För **FirewallMode**, hello tillgängliga värden är förebyggande och identifiering.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-143">For **FirewallMode**, hello available values are Prevention and Detection.</span></span>

    ```powershell
    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention
    ```

1. <span data-ttu-id="c4ea1-144">Uppdatera hello Programgateway med hello inställningarna i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-144">Update hello Application Gateway with hello settings defined in hello preceding step.</span></span>

    ```powershell
    Set-AzureRmApplicationGateway -ApplicationGateway $gw
    ```

<span data-ttu-id="c4ea1-145">Det här kommandot uppdaterar hello Programgateway med Brandvägg för webbaserade program.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-145">This command updates hello Application Gateway with web application firewall.</span></span> <span data-ttu-id="c4ea1-146">Besök [Programgateway diagnostik](application-gateway-diagnostics.md) toounderstand hur tooview loggar för Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-146">Visit [Application Gateway diagnostics](application-gateway-diagnostics.md) toounderstand how tooview logs for your Application Gateway.</span></span> <span data-ttu-id="c4ea1-147">På grund av toohello säkerhet uppbyggnad Brandvägg, loggar måste toobe ses över regelbundet toounderstand hello säkerhetstillståndet webbprogram.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-147">Due toohello security nature of WAF, logs need toobe reviewed regularly toounderstand hello security posture of your web applications.</span></span>

## <a name="create-an-application-gateway-with-web-application-firewall"></a><span data-ttu-id="c4ea1-148">Skapa en Programgateway med Brandvägg för webbaserade program</span><span class="sxs-lookup"><span data-stu-id="c4ea1-148">Create an Application Gateway with web application firewall</span></span>

<span data-ttu-id="c4ea1-149">hello följande steg leder dig genom hello hela processen från början tooend för att skapa en Programgateway med Brandvägg för webbaserade program.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-149">hello following steps take you through hello entire process from beginning tooend for creating an Application Gateway with web application firewall.</span></span>

<span data-ttu-id="c4ea1-150">Kontrollera att du använder hello senaste versionen av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-150">Make sure that you are using hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="c4ea1-151">Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="c4ea1-151">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

1. <span data-ttu-id="c4ea1-152">Logga in tooAzure genom att köra `Login-AzureRmAccount`, du är ange tooauthenticate med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-152">Log in tooAzure by running `Login-AzureRmAccount`, you are prompted tooauthenticate with your credentials.</span></span>

1. <span data-ttu-id="c4ea1-153">Kontrollera hello prenumerationer för hello konto genom att köra`Get-AzureRmSubscription`</span><span class="sxs-lookup"><span data-stu-id="c4ea1-153">Check hello subscriptions for hello account by running `Get-AzureRmSubscription`</span></span>

1. <span data-ttu-id="c4ea1-154">Välj vilka av dina Azure-prenumerationer toouse.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-154">Choose which of your Azure subscriptions toouse.</span></span>

    ```powershell
    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
    ```

### <a name="create-a-resource-group"></a><span data-ttu-id="c4ea1-155">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="c4ea1-155">Create a resource group</span></span>

<span data-ttu-id="c4ea1-156">Skapa en resursgrupp för hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-156">Create a resource group for hello Application Gateway.</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

<span data-ttu-id="c4ea1-157">Azure Resource Manager kräver att alla resursgrupper anger en plats.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-157">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="c4ea1-158">Den här platsen används som hello standardplatsen för resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-158">This location is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="c4ea1-159">Se till att alla kommandon som använder toocreate en Programgateway hello samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-159">Make sure that all commands toocreate an Application Gateway uses hello same resource group.</span></span>

<span data-ttu-id="c4ea1-160">I föregående exempel hello, vi har skapat en resursgrupp med namnet ”appgw RG” och plats ”USA, västra”.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-160">In hello preceding example, we created a resource group called "appgw-RG" and location "West US."</span></span>

> [!NOTE]
> <span data-ttu-id="c4ea1-161">Om du behöver tooconfigure en anpassad avsökningsåtgärd för Application Gateway finns [skapa en Programgateway med anpassade avsökningar med hjälp av PowerShell](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="c4ea1-161">If you need tooconfigure a custom probe for your Application Gateway, see [Create an Application Gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md).</span></span> <span data-ttu-id="c4ea1-162">Mer information finns i [Anpassade avsökningar och hälsoövervakning](application-gateway-probe-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c4ea1-162">Check out [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>

### <a name="configure-virtual-network"></a><span data-ttu-id="c4ea1-163">Konfigurera ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="c4ea1-163">Configure virtual network</span></span>

<span data-ttu-id="c4ea1-164">Programgateway kräver ett undernät egna.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-164">Application Gateway requires a subnet of its own.</span></span> <span data-ttu-id="c4ea1-165">I det här steget skapar du ett virtuellt nätverk med ett adressutrymme för 10.0.0.0/16 och två undernät, en för hello Application Gateway och en för backend-poolmedlemmar.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-165">In this step, you create a virtual network with an address space of 10.0.0.0/16 and two subnets, one for hello Application Gateway and one for backend pool members.</span></span>

```powershell
# Create a subnet configuration object for hello Application Gateway subnet. A subnet for an application should have a minimum of 28 mask bits. This value leaves 10 available addresses in hello subnet for Application Gateway instances. With a smaller subnet, you may not be able tooadd more instance of your Application Gateway in hello future.
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

# Create a subnet configuration object for hello backend pool members subnet
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

# Create hello virtual network with hello previous created subnets
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="configure-public-ip-address"></a><span data-ttu-id="c4ea1-166">Konfigurera offentliga IP-adress</span><span class="sxs-lookup"><span data-stu-id="c4ea1-166">Configure public IP address</span></span>

<span data-ttu-id="c4ea1-167">I ordning toohandle externa förfrågningar kräver Programgateway en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-167">In order toohandle external requests, Application Gateway requires a public IP address.</span></span> <span data-ttu-id="c4ea1-168">Den här offentliga IP-adressen får inte ha en `DomainNameLabel` definierats toobe som används av hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-168">This public IP address must not have a `DomainNameLabel` defined toobe used by hello Application Gateway.</span></span>

```powershell
# Create a public IP address for use with hello Application Gateway. Defining hello domainnamelabel during creation is not supported for use with Application Gateway
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic
```

### <a name="configure-hello-application-gateway"></a><span data-ttu-id="c4ea1-169">Konfigurera hello Programgateway</span><span class="sxs-lookup"><span data-stu-id="c4ea1-169">Configure hello Application Gateway</span></span>

```powershell
# Create a IP configuration. This configures what subnet hello Application Gateway uses. When Application Gateway starts, it picks up an IP address from hello subnet configured and routes network traffic toohello IP addresses in hello back-end IP pool.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

# Create a backend pool toohold hello addresses or NICs for hello application that Application Gateway is protecting.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

# Upload hello authenication certificate that will be used toocommunicate with hello backend servers
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path too.cer file>

# Conifugre hello backend HTTP settings toobe used toodefine how traffic is routed toohello backend pool. hello authenication certificate used in hello previous step is added toohello backend http settings.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

# Create a frontend port toobe used by hello listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

# Create a frontend IP configuration tooassociate hello public IP address with hello Application Gateway
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

# Configure hello certificate for hello Application Gateway. This certificate is used toodecrypt and re-encrypt hello traffic on hello Application Gateway.
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path too.pfx file> -Password <password for certificate file>

# Create hello HTTP listener for hello Application Gateway. Assign hello front-end ip configuration, port, and ssl certificate toouse.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

#Create a load balancer routing rule that configures hello load balancer behavior. In this example, a basic round robin rule is created.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure hello SKU of hello Application Gateway
$sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

# Define hello SSL policy toouse
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S

#Configure hello waf configuration settings.
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"

# Create hello Application Gateway utilizing all hello previously created configuration objects
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert
```

> [!NOTE]
> <span data-ttu-id="c4ea1-170">Programgatewayer som skapats med konfiguration av hello grundläggande webbprogram brandväggen konfigureras med CR 3.0 för skydd.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-170">Application Gateways created with hello basic web application firewall configuration are configured with CRS 3.0 for protections.</span></span>

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="c4ea1-171">Hämta program Gateway DNS-namn</span><span class="sxs-lookup"><span data-stu-id="c4ea1-171">Get Application Gateway DNS name</span></span>

<span data-ttu-id="c4ea1-172">När du har skapat hello gateway är hello nästa steg tooconfigure hello klientdelen för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-172">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="c4ea1-173">När du använder en offentlig IP-adress, kräver en dynamiskt tilldelad DNS-namn som inte är eget Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-173">When using a public IP, Application Gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="c4ea1-174">tooensure slutanvändare kan träffa hello Programgateway, en CNAME-post kan vara används toopoint toohello offentlig slutpunkt för hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-174">tooensure end users can hit hello Application Gateway, a CNAME record can be used toopoint toohello public endpoint of hello Application Gateway.</span></span> <span data-ttu-id="c4ea1-175">[Konfigurera ett eget domännamn i Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c4ea1-175">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="c4ea1-176">tooconfigure ett alias, hämta information om hello Programgateway och dess associerade IP DNS-namn med hjälp av hello PublicIPAddress element kopplade toohello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-176">tooconfigure an alias, retrieve details of hello Application Gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello Application Gateway.</span></span> <span data-ttu-id="c4ea1-177">hello Application Gateway DNS-namn ska använda toocreate en CNAME-post som pekar hello två web program toothis DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-177">hello Application Gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="c4ea1-178">hello rekommenderas A-poster inte eftersom hello VIP ändras vid omstart av Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="c4ea1-178">hello use of A-records is not recommended since hello VIP may change on restart of Application Gateway.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="c4ea1-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c4ea1-179">Next steps</span></span>

<span data-ttu-id="c4ea1-180">Lär dig hur tooconfigure diagnostikloggning, toolog hello händelser som har identifierats eller förhindras med Brandvägg för webbaserade program genom att besöka [Gateway Programdiagnostik](application-gateway-diagnostics.md)</span><span class="sxs-lookup"><span data-stu-id="c4ea1-180">Learn how tooconfigure diagnostic logging, toolog hello events that are detected or prevented with web application firewall by visiting [Application Gateway Diagnostics](application-gateway-diagnostics.md)</span></span>

[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png
