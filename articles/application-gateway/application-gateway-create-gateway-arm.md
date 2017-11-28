---
title: aaaCreate och hantera Azure Application Gateway - PowerShell | Microsoft Docs
description: "Den här sidan innehåller instruktioner toocreate, konfigurera, starta och ta bort en gateway för Azure-program med Azure Resource Manager"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: ab98d5f9aa0dc309f8353b7f72591359e1121849
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a><span data-ttu-id="6d808-103">Skapa, starta eller ta bort en programgateway med Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6d808-103">Create, start, or delete an application gateway by using Azure Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="6d808-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6d808-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="6d808-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6d808-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="6d808-106">PowerShell och den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6d808-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="6d808-107">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="6d808-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="6d808-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6d808-108">Azure CLI</span></span>](application-gateway-create-gateway-cli.md)

<span data-ttu-id="6d808-109">Azure Application Gateway är en Layer 7-belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="6d808-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="6d808-110">Det ger redundans och prestanda inkommande HTTP-begäranden mellan olika servrar, oavsett om de är på hello molnet eller lokalt.</span><span class="sxs-lookup"><span data-stu-id="6d808-110">It provides failover and performance-routing HTTP requests between different servers, whether they are on hello cloud or on-premises.</span></span> <span data-ttu-id="6d808-111">Application Gateway innehåller många ADC-funktioner (Application Delivery Controller), inklusive HTTP-belastningsutjämning, cookie-baserad sessionstilldelning, SSL-avlastning (Secure Sockets Layer), anpassade hälsoavsökningar, stöd för flera platser och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="6d808-111">Application Gateway provides many application delivery controller (ADC) features including HTTP load balancing, cookie-based session affinity, Secure Sockets Layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="6d808-112">toofind en fullständig lista över funktioner som stöds finns [Programgateway översikt](application-gateway-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6d808-112">toofind a complete list of supported features, visit [Application Gateway overview](application-gateway-introduction.md).</span></span>

<span data-ttu-id="6d808-113">Den här artikeln vägleder dig genom hello steg toocreate, konfigurera, starta och ta bort en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="6d808-113">This article walks you through hello steps toocreate, configure, start, and delete an application gateway.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6d808-114">Innan du börjar arbeta med Azure-resurser, är det viktigt toounderstand att Azure har två distributionsmodeller: Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="6d808-114">Before you work with Azure resources, it is important toounderstand that Azure currently has two deployment models: Resource Manager and classic.</span></span> <span data-ttu-id="6d808-115">Det är viktigt att du förstår [distributionsmodellerna och verktygen](../azure-classic-rm.md) innan du börjar arbeta med Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="6d808-115">Make sure that you understand [deployment models and tools](../azure-classic-rm.md) before working with any Azure resource.</span></span> <span data-ttu-id="6d808-116">Du kan visa hello dokumentationen för olika verktyg genom att klicka på flikarna hello hello överst i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="6d808-116">You can view hello documentation for different tools by clicking hello tabs at hello top of this article.</span></span> <span data-ttu-id="6d808-117">I det här dokumentet beskrivs hur du kan skapa en programgateway med hjälp av Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6d808-117">This document covers creating an application gateway by using Azure Resource Manager.</span></span> <span data-ttu-id="6d808-118">toouse hello klassiska versionen, gå för[skapa en gateway klassiska programdistribution med hjälp av PowerShell](application-gateway-create-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="6d808-118">toouse hello classic version, go too[Create an application gateway classic deployment by using PowerShell](application-gateway-create-gateway.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="6d808-119">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="6d808-119">Before you begin</span></span>

1. <span data-ttu-id="6d808-120">Installera hello senaste versionen av hello Azure PowerShell-cmdlets med hello installationsprogram för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="6d808-120">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="6d808-121">Du kan hämta och installera hello senaste versionen från hello **Windows PowerShell** avsnitt i hello [Nedladdningssida](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="6d808-121">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
1. <span data-ttu-id="6d808-122">Om du har ett befintligt virtuellt nätverk, Välj ett befintligt tomma undernät eller skapa ett undernät i det befintliga virtuella nätverket enbart för användning av hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="6d808-122">If you have an existing virtual network, either select an existing empty subnet or create a subnet in your existing virtual network solely for use by hello application gateway.</span></span> <span data-ttu-id="6d808-123">Du kan inte distribuera hello programmet gateway tooa annat virtuellt nätverk än hello resurser som du avser att toodeploy bakom hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="6d808-123">You cannot deploy hello application gateway tooa different virtual network than hello resources you intend toodeploy behind hello application gateway.</span></span>
1. <span data-ttu-id="6d808-124">hello-servrar som du konfigurerar toouse hello Programgateway måste finnas eller tilldelats deras slutpunkter har skapats i hello virtuellt nätverk eller med en offentlig IP-adress/VIP.</span><span class="sxs-lookup"><span data-stu-id="6d808-124">hello servers that you configure toouse hello application gateway must exist or have their endpoints created either in hello virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-toocreate-an-application-gateway"></a><span data-ttu-id="6d808-125">Vad är obligatoriska toocreate en Programgateway?</span><span class="sxs-lookup"><span data-stu-id="6d808-125">What is required toocreate an application gateway?</span></span>

* <span data-ttu-id="6d808-126">**Backend-serverpoolen:** hello lista över IP-adresser, FQDN eller nätverkskort på hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="6d808-126">**Backend server pool:** hello list of IP addresses, FQDNs, or NICs of hello backend servers.</span></span> <span data-ttu-id="6d808-127">Om du använder IP-adresser ska antingen tillhöra toohello undernät för virtuellt nätverk eller ska vara en offentlig IP-adress/VIP.</span><span class="sxs-lookup"><span data-stu-id="6d808-127">If IP addresses are used, they should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="6d808-128">**Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookie-baserad tillhörighet.</span><span class="sxs-lookup"><span data-stu-id="6d808-128">**Backend server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="6d808-129">De här inställningarna är bundet tooa poolen och tillämpade tooall servrar inom hello poolen.</span><span class="sxs-lookup"><span data-stu-id="6d808-129">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="6d808-130">**klientdelsport:** den här porten är hello offentliga som öppnas på hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="6d808-130">**frontend port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="6d808-131">Trafik träffar den här porten och hämtar omdirigerade tooone på hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="6d808-131">Traffic hits this port, and then gets redirected tooone of hello backend servers.</span></span>
* <span data-ttu-id="6d808-132">**Lyssnare:** hello-lyssnare har en klientdelsport, ett protokoll (Http eller Https dessa värden är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning).</span><span class="sxs-lookup"><span data-stu-id="6d808-132">**Listener:** hello listener has a frontend port, a protocol (Http or Https, these values are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="6d808-133">**Regel:** hello regeln Binder hello lyssnare, hello backend-serverpoolen och definierar vilken backend-servern poolen hello trafik ska vara riktad toowhen den når en viss lyssnare.</span><span class="sxs-lookup"><span data-stu-id="6d808-133">**Rule:** hello rule binds hello listener, hello backend server pool and defines which backend server pool hello traffic should be directed toowhen it hits a particular listener.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="6d808-134">Skapa en resursgrupp för Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6d808-134">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="6d808-135">Kontrollera att du använder hello senaste versionen av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6d808-135">Make sure that you are using hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="6d808-136">Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="6d808-136">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

1. <span data-ttu-id="6d808-137">Logga in tooAzure och ange dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="6d808-137">Log in tooAzure and enter your credentials.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

2. <span data-ttu-id="6d808-138">Kontrollera hello prenumerationer för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="6d808-138">Check hello subscriptions for hello account.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

3. <span data-ttu-id="6d808-139">Välj vilka av dina Azure-prenumerationer toouse.</span><span class="sxs-lookup"><span data-stu-id="6d808-139">Choose which of your Azure subscriptions toouse.</span></span>

  ```powershell
  Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
  ```

4. <span data-ttu-id="6d808-140">Skapa en resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp).</span><span class="sxs-lookup"><span data-stu-id="6d808-140">Create a resource group (skip this step if you're using an existing resource group).</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name ContosoRG -Location "West US"
  ```

<span data-ttu-id="6d808-141">Azure Resource Manager kräver att alla resursgrupper anger en plats.</span><span class="sxs-lookup"><span data-stu-id="6d808-141">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="6d808-142">Den här platsen används som hello standardplatsen för resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="6d808-142">This location is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="6d808-143">Se till att alla kommandon toocreate använder en Programgateway hello samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="6d808-143">Make sure that all commands toocreate an application gateway uses hello same resource group.</span></span>

<span data-ttu-id="6d808-144">Vi har skapat en resursgrupp med namnet i hello-exemplet ovan, **ContosoRG** och plats **östra USA**.</span><span class="sxs-lookup"><span data-stu-id="6d808-144">In hello example above, we created a resource group called **ContosoRG** and location **East US**.</span></span>

> [!NOTE]
> <span data-ttu-id="6d808-145">Om du behöver tooconfigure en anpassad avsökningsåtgärd för din Programgateway går: [skapa en Programgateway med anpassade avsökningar med hjälp av PowerShell](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="6d808-145">If you need tooconfigure a custom probe for your application gateway, visit: [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md).</span></span> <span data-ttu-id="6d808-146">Mer information finns i [Anpassade avsökningar och hälsoövervakning](application-gateway-probe-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6d808-146">Check out [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>


## <a name="create-hello-application-gateway-configuration-objects"></a><span data-ttu-id="6d808-147">Skapa konfigurationsobjekt för hello Programgateway</span><span class="sxs-lookup"><span data-stu-id="6d808-147">Create hello application gateway configuration objects</span></span>

<span data-ttu-id="6d808-148">Alla konfigurationsobjekt måste ställas in innan du skapar hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="6d808-148">All configuration items must be set up before creating hello application gateway.</span></span> <span data-ttu-id="6d808-149">hello med följande steg skapar hello konfigurationsobjekt som behövs för en gateway programresursen.</span><span class="sxs-lookup"><span data-stu-id="6d808-149">hello following steps create hello configuration items that are needed for an application gateway resource.</span></span>

```powershell
# Create a subnet and assign hello address space of 10.0.0.0/24
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

# Create a virtual network with hello address space of 10.0.0.0/16 and add hello subnet
$vnet = New-AzureRmVirtualNetwork -Name ContosoVNET -ResourceGroupName ContosoRG -Location "East US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

# Retrieve hello newly created subnet
$subnet=$vnet.Subnets[0]

# Create a public IP address that is used tooconnect toohello application gateway. Application Gateway does not support custom DNS names on public IP addresses.  If a custom name is required for hello public endpoint, a CNAME record should be created toopoint toohello automatically generated DNS name for hello public IP address.
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName ContosoRG -name publicIP01 -location "East US" -AllocationMethod Dynamic

# Create a gateway IP configuration. hello gateway picks up an IP addressfrom hello configured subnet and routes network traffic toohello IP addresses in hello backend IP pool. Keep in mind that each instance takes one IP address.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

# Configure a backend pool with hello addresses of your web servers. These backend pool members are all validated toobe healthy by probes, whether they are basic probes or custom probes.  Traffic is then routed toothem when requests come into hello application gateway. Backend pools can be used by multiple rules within hello application gateway, which means one backend pool could be used for multiple web applications that reside on hello same host.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

# Configure backend http settings toodetermine hello protocol and port that is used when sending traffic toohello backend servers. Cookie-based sessions are also determined by hello backend HTTP settings.  If enabled, cookie-based session affinity sends traffic toohello same backend as previous requests for each packet.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

# Configure a frontend port that is used tooconnect toohello application gateway through hello public IP address
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

# Configure hello frontend IP configuration with hello public IP address created earlier.
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

# Configure hello listener.  hello listener is a combination of hello front end IP configuration, protocol, and port and is used tooreceive incoming network traffic. 
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

# Configure a basic rule that is used tooroute traffic toohello backend servers. hello backend pool settings, listener, and backend pool created in hello previous steps make up hello rule. Based on hello criteria defined traffic is routed toohello appropriate backend.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure hello SKU for hello application gateway, this determines hello size and whether or not WAF is used.
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

# Create hello application gateway
$appgw = New-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG -Location "East US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

<span data-ttu-id="6d808-150">När du är klar att hämta information om DNS- och VIP för hello Programgateway från hello offentliga IP-resurs bifogade toohello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="6d808-150">When complete, retrieve DNS and VIP details of hello application gateway from hello public IP resource attached toohello application gateway.</span></span>

```powershell
Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName ContosoRG
```

## <a name="delete-hello-application-gateway"></a><span data-ttu-id="6d808-151">Ta bort hello Programgateway</span><span class="sxs-lookup"><span data-stu-id="6d808-151">Delete hello application gateway</span></span>

<span data-ttu-id="6d808-152">hello följande exempel tar bort hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="6d808-152">hello following example deletes hello application gateway.</span></span>

```powershell
# Retrieve hello application gateway
$gw = Get-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG

# Stops hello application gateway
Stop-AzureRmApplicationGateway -ApplicationGateway $gw

# Once hello application gateway is in a stopped state, use hello `Remove-AzureRmApplicationGateway` cmdlet tooremove hello service.
Remove-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG -Force
```

> [!NOTE]
> <span data-ttu-id="6d808-153">Hej **-tvinga** växel kan vara används toosuppress hello ta bort bekräftelsemeddelande.</span><span class="sxs-lookup"><span data-stu-id="6d808-153">hello **-force** switch can be used toosuppress hello remove confirmation message.</span></span>

<span data-ttu-id="6d808-154">tooverify som hello tjänsten har tagits bort kan du använda hello `Get-AzureRmApplicationGateway` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="6d808-154">tooverify that hello service has been removed, you can use hello `Get-AzureRmApplicationGateway` cmdlet.</span></span> <span data-ttu-id="6d808-155">Det här steget är inte obligatoriskt.</span><span class="sxs-lookup"><span data-stu-id="6d808-155">This step is not required.</span></span>

```powershell
Get-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG
```

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="6d808-156">Hämta DNS-namn för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="6d808-156">Get application gateway DNS name</span></span>

<span data-ttu-id="6d808-157">När du har skapat hello gateway är hello nästa steg tooconfigure hello klientdelen för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="6d808-157">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="6d808-158">När du använder en offentlig IP-adress krävs ett dynamiskt tilldelat DNS-namn som inte är användarvänligt.</span><span class="sxs-lookup"><span data-stu-id="6d808-158">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="6d808-159">tooensure slutanvändare kan träffa hello Programgateway, en CNAME-post kan vara används toopoint toohello offentlig slutpunkt för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="6d808-159">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="6d808-160">toodo detta, hämta information om hello Programgateway och dess associerade IP DNS-namn med hjälp av hello PublicIPAddress element bifogade toohello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="6d808-160">toodo this, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="6d808-161">Detta kan göras med Azure DNS eller andra DNS-leverantörer, genom att skapa en CNAME-post som pekar toohello [offentliga IP-adressen](../dns/dns-custom-domain.md#public-ip-address).</span><span class="sxs-lookup"><span data-stu-id="6d808-161">This can be done with Azure DNS or other DNS providers, by creating a CNAME record that points toohello [public IP address](../dns/dns-custom-domain.md#public-ip-address).</span></span> <span data-ttu-id="6d808-162">hello rekommenderas A-poster inte eftersom hello VIP ändras vid omstart för Programgateway.</span><span class="sxs-lookup"><span data-stu-id="6d808-162">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="6d808-163">En IP-adress tilldelas toohello Programgateway när hello-tjänsten startas.</span><span class="sxs-lookup"><span data-stu-id="6d808-163">An IP address is assigned toohello application gateway when hello service starts.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName ContosoRG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : ContosoRG
Location                 : westus
Id                       : /subscriptions/<subscription_id>/resourceGroups/ContosoRG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/ContosoAppGateway/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="delete-all-resources"></a><span data-ttu-id="6d808-164">Ta bort alla resurser</span><span class="sxs-lookup"><span data-stu-id="6d808-164">Delete all resources</span></span>

<span data-ttu-id="6d808-165">toodelete alla resurser skapas i den här artikeln, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6d808-165">toodelete all resources created in this article, complete hello following step:</span></span>

```powershell
Remove-AzureRmResourceGroup -Name ContosoRG
```

## <a name="next-steps"></a><span data-ttu-id="6d808-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6d808-166">Next steps</span></span>

<span data-ttu-id="6d808-167">Om du vill tooconfigure SSL-avlastning, besök: [konfigurera en Programgateway för SSL-avlastning](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="6d808-167">If you want tooconfigure SSL offload, visit: [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="6d808-168">Om du vill tooconfigure ett program gateway toouse med en intern belastningsutjämnare: [skapa en Programgateway med en intern belastningsutjämnare (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="6d808-168">If you want tooconfigure an application gateway toouse with an internal load balancer, visit: [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="6d808-169">Om du vill ha mer information om belastningsutjämningsalternativ i allmänhet läser du:</span><span class="sxs-lookup"><span data-stu-id="6d808-169">If you want more information about load balancing options in general, visit:</span></span>

* [<span data-ttu-id="6d808-170">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="6d808-170">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="6d808-171">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="6d808-171">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)
