---
title: "Skapa och hantera Azure Application Gateway – PowerShell | Microsoft Docs"
description: "Den här sidan innehåller anvisningar för hur du skapar, konfigurerar, startar och tar bort en programgateway i Azure med hjälp av Azure Resource Manager"
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
ms.openlocfilehash: 5f1713365406764998de505ff62309bab9fa2567
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a><span data-ttu-id="c13f6-103">Skapa, starta eller ta bort en programgateway med Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c13f6-103">Create, start, or delete an application gateway by using Azure Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c13f6-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c13f6-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="c13f6-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c13f6-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="c13f6-106">PowerShell och den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c13f6-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="c13f6-107">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="c13f6-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="c13f6-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c13f6-108">Azure CLI</span></span>](application-gateway-create-gateway-cli.md)

<span data-ttu-id="c13f6-109">Azure Application Gateway är en Layer 7-belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="c13f6-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="c13f6-110">Den tillhandahåller redundans och prestandabaserad routning av HTTP-begäranden mellan olika servrar, oavsett om de finns i molnet eller lokalt.</span><span class="sxs-lookup"><span data-stu-id="c13f6-110">It provides failover and performance-routing HTTP requests between different servers, whether they are on the cloud or on-premises.</span></span> <span data-ttu-id="c13f6-111">Application Gateway innehåller många ADC-funktioner (Application Delivery Controller), inklusive HTTP-belastningsutjämning, cookie-baserad sessionstilldelning, SSL-avlastning (Secure Sockets Layer), anpassade hälsoavsökningar, stöd för flera platser och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="c13f6-111">Application Gateway provides many application delivery controller (ADC) features including HTTP load balancing, cookie-based session affinity, Secure Sockets Layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="c13f6-112">En fullständig lista över funktioner som stöds finns i [Översikt över Application Gateway](application-gateway-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c13f6-112">To find a complete list of supported features, visit [Application Gateway overview](application-gateway-introduction.md).</span></span>

<span data-ttu-id="c13f6-113">Den här artikeln beskriver steg för steg hur du skapar, konfigurerar, startar och tar bort en programgateway.</span><span class="sxs-lookup"><span data-stu-id="c13f6-113">This article walks you through the steps to create, configure, start, and delete an application gateway.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c13f6-114">Innan du börjar arbeta med Azure-resurser är det viktigt att du förstår att Azure för närvarande har två distributionsmodeller: Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="c13f6-114">Before you work with Azure resources, it is important to understand that Azure currently has two deployment models: Resource Manager and classic.</span></span> <span data-ttu-id="c13f6-115">Det är viktigt att du förstår [distributionsmodellerna och verktygen](../azure-classic-rm.md) innan du börjar arbeta med Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="c13f6-115">Make sure that you understand [deployment models and tools](../azure-classic-rm.md) before working with any Azure resource.</span></span> <span data-ttu-id="c13f6-116">Du kan granska dokumentationen för olika verktyg genom att klicka på flikarna överst i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="c13f6-116">You can view the documentation for different tools by clicking the tabs at the top of this article.</span></span> <span data-ttu-id="c13f6-117">I det här dokumentet beskrivs hur du kan skapa en programgateway med hjälp av Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c13f6-117">This document covers creating an application gateway by using Azure Resource Manager.</span></span> <span data-ttu-id="c13f6-118">Om du vill använda den klassiska versionen går du till [Skapa en programgateway med den klassiska programdistributionen med hjälp av PowerShell](application-gateway-create-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="c13f6-118">To use the classic version, go to [Create an application gateway classic deployment by using PowerShell](application-gateway-create-gateway.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="c13f6-119">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="c13f6-119">Before you begin</span></span>

1. <span data-ttu-id="c13f6-120">Installera den senaste versionen av Azure PowerShell-cmdlets med hjälp av installationsprogrammet för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="c13f6-120">Install the latest version of the Azure PowerShell cmdlets by using the Web Platform Installer.</span></span> <span data-ttu-id="c13f6-121">Du kan hämta och installera den senaste versionen från avsnittet om **Windows PowerShell** på [hämtningssidan](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="c13f6-121">You can download and install the latest version from the **Windows PowerShell** section of the [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
1. <span data-ttu-id="c13f6-122">Om du har ett befintligt virtuellt nätverk väljer du antingen ett befintligt tomt undernät eller skapar ett undernät i det befintliga virtuella nätverket som enbart är avsett för att användas av programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="c13f6-122">If you have an existing virtual network, either select an existing empty subnet or create a subnet in your existing virtual network solely for use by the application gateway.</span></span> <span data-ttu-id="c13f6-123">Du kan inte distribuera programgatewayen till något annat virtuellt nätverk än vad de resurser som du avser att distribuera bakom programgatewayen tillåter.</span><span class="sxs-lookup"><span data-stu-id="c13f6-123">You cannot deploy the application gateway to a different virtual network than the resources you intend to deploy behind the application gateway.</span></span>
1. <span data-ttu-id="c13f6-124">De servrar som du konfigurerar för användning av programgatewayen måste finnas i det virtuella nätverket eller ha slutpunkter som skapats där eller tilldelats en offentlig IP-/VIP-adress.</span><span class="sxs-lookup"><span data-stu-id="c13f6-124">The servers that you configure to use the application gateway must exist or have their endpoints created either in the virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-to-create-an-application-gateway"></a><span data-ttu-id="c13f6-125">Vad krävs för att skapa en programgateway?</span><span class="sxs-lookup"><span data-stu-id="c13f6-125">What is required to create an application gateway?</span></span>

* <span data-ttu-id="c13f6-126">**Serverdelspool:** Listan med IP-adresser, fullständigt kvalificerade domännamn eller nätverkskort för backend-servrarna.</span><span class="sxs-lookup"><span data-stu-id="c13f6-126">**Backend server pool:** The list of IP addresses, FQDNs, or NICs of the backend servers.</span></span> <span data-ttu-id="c13f6-127">Om IP-adresser används bör de antingen tillhöra det virtuella undernätet eller vara offentliga IP-/VIP-adresser.</span><span class="sxs-lookup"><span data-stu-id="c13f6-127">If IP addresses are used, they should either belong to the virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="c13f6-128">**Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookie-baserad tillhörighet.</span><span class="sxs-lookup"><span data-stu-id="c13f6-128">**Backend server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="c13f6-129">Dessa inställningar är knutna till en pool och tillämpas på alla servrar i poolen.</span><span class="sxs-lookup"><span data-stu-id="c13f6-129">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="c13f6-130">**Frontend-port:** Den här porten är den offentliga porten som är öppen på programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="c13f6-130">**frontend port:** This port is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="c13f6-131">Trafiken kommer till den här porten och omdirigeras till en av backend-servrarna.</span><span class="sxs-lookup"><span data-stu-id="c13f6-131">Traffic hits this port, and then gets redirected to one of the backend servers.</span></span>
* <span data-ttu-id="c13f6-132">**Lyssnare:** Lyssnaren har en frontend-port, ett protokoll (Http eller Https; dessa värden är skiftlägeskänsliga) och SSL-certifikatnamnet (om du konfigurerar SSL-avlastning).</span><span class="sxs-lookup"><span data-stu-id="c13f6-132">**Listener:** The listener has a frontend port, a protocol (Http or Https, these values are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="c13f6-133">**Regel:** Regeln binder lyssnaren och backend-serverpoolen och definierar vilken backend-serverpool som trafiken ska dirigeras till när den når en viss lyssnare.</span><span class="sxs-lookup"><span data-stu-id="c13f6-133">**Rule:** The rule binds the listener, the backend server pool and defines which backend server pool the traffic should be directed to when it hits a particular listener.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="c13f6-134">Skapa en resursgrupp för Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c13f6-134">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="c13f6-135">Kontrollera att du använder den senaste versionen av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c13f6-135">Make sure that you are using the latest version of Azure PowerShell.</span></span> <span data-ttu-id="c13f6-136">Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="c13f6-136">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

1. <span data-ttu-id="c13f6-137">Logga in i Azure och ange dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="c13f6-137">Log in to Azure and enter your credentials.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

2. <span data-ttu-id="c13f6-138">Kontrollera prenumerationerna för kontot.</span><span class="sxs-lookup"><span data-stu-id="c13f6-138">Check the subscriptions for the account.</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

3. <span data-ttu-id="c13f6-139">Välj vilka av dina Azure-prenumerationer som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="c13f6-139">Choose which of your Azure subscriptions to use.</span></span>

  ```powershell
  Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
  ```

4. <span data-ttu-id="c13f6-140">Skapa en resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp).</span><span class="sxs-lookup"><span data-stu-id="c13f6-140">Create a resource group (skip this step if you're using an existing resource group).</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name ContosoRG -Location "West US"
  ```

<span data-ttu-id="c13f6-141">Azure Resource Manager kräver att alla resursgrupper anger en plats.</span><span class="sxs-lookup"><span data-stu-id="c13f6-141">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="c13f6-142">Den här platsen används som standardplats för resurserna i den resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="c13f6-142">This location is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="c13f6-143">Se till att alla kommandon du använder för att skapa en programgateway använder samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="c13f6-143">Make sure that all commands to create an application gateway uses the same resource group.</span></span>

<span data-ttu-id="c13f6-144">I exemplet ovan skapade vi en resursgrupp med namnet **ContosoRG** och platsen **Östra USA**.</span><span class="sxs-lookup"><span data-stu-id="c13f6-144">In the example above, we created a resource group called **ContosoRG** and location **East US**.</span></span>

> [!NOTE]
> <span data-ttu-id="c13f6-145">Om du behöver konfigurera en anpassad avsökning för din programgateway läser du [Skapa en programgateway med anpassade avsökningar med hjälp av PowerShell](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="c13f6-145">If you need to configure a custom probe for your application gateway, visit: [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md).</span></span> <span data-ttu-id="c13f6-146">Mer information finns i [Anpassade avsökningar och hälsoövervakning](application-gateway-probe-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c13f6-146">Check out [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>


## <a name="create-the-application-gateway-configuration-objects"></a><span data-ttu-id="c13f6-147">Skapa konfigurationsobjekt för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="c13f6-147">Create the application gateway configuration objects</span></span>

<span data-ttu-id="c13f6-148">Alla konfigurationsobjekt måste konfigureras innan programgatewayen skapas.</span><span class="sxs-lookup"><span data-stu-id="c13f6-148">All configuration items must be set up before creating the application gateway.</span></span> <span data-ttu-id="c13f6-149">Följande steg skapar konfigurationsobjekten som behövs för en programgatewayresurs.</span><span class="sxs-lookup"><span data-stu-id="c13f6-149">The following steps create the configuration items that are needed for an application gateway resource.</span></span>

```powershell
# Create a subnet and assign the address space of 10.0.0.0/24
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

# Create a virtual network with the address space of 10.0.0.0/16 and add the subnet
$vnet = New-AzureRmVirtualNetwork -Name ContosoVNET -ResourceGroupName ContosoRG -Location "East US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

# Retrieve the newly created subnet
$subnet=$vnet.Subnets[0]

# Create a public IP address that is used to connect to the application gateway. Application Gateway does not support custom DNS names on public IP addresses.  If a custom name is required for the public endpoint, a CNAME record should be created to point to the automatically generated DNS name for the public IP address.
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName ContosoRG -name publicIP01 -location "East US" -AllocationMethod Dynamic

# Create a gateway IP configuration. The gateway picks up an IP addressfrom the configured subnet and routes network traffic to the IP addresses in the backend IP pool. Keep in mind that each instance takes one IP address.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

# Configure a backend pool with the addresses of your web servers. These backend pool members are all validated to be healthy by probes, whether they are basic probes or custom probes.  Traffic is then routed to them when requests come into the application gateway. Backend pools can be used by multiple rules within the application gateway, which means one backend pool could be used for multiple web applications that reside on the same host.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

# Configure backend http settings to determine the protocol and port that is used when sending traffic to the backend servers. Cookie-based sessions are also determined by the backend HTTP settings.  If enabled, cookie-based session affinity sends traffic to the same backend as previous requests for each packet.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

# Configure a frontend port that is used to connect to the application gateway through the public IP address
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

# Configure the frontend IP configuration with the public IP address created earlier.
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

# Configure the listener.  The listener is a combination of the front end IP configuration, protocol, and port and is used to receive incoming network traffic. 
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

# Configure a basic rule that is used to route traffic to the backend servers. The backend pool settings, listener, and backend pool created in the previous steps make up the rule. Based on the criteria defined traffic is routed to the appropriate backend.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure the SKU for the application gateway, this determines the size and whether or not WAF is used.
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

# Create the application gateway
$appgw = New-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG -Location "East US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

<span data-ttu-id="c13f6-150">När du har utfört stegen hämtar du DNS- och VIP-informationen för programgatewayen från den offentliga IP-resursen som är associerad med programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="c13f6-150">When complete, retrieve DNS and VIP details of the application gateway from the public IP resource attached to the application gateway.</span></span>

```powershell
Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName ContosoRG
```

## <a name="delete-the-application-gateway"></a><span data-ttu-id="c13f6-151">Ta bort programgatewayen</span><span class="sxs-lookup"><span data-stu-id="c13f6-151">Delete the application gateway</span></span>

<span data-ttu-id="c13f6-152">I följande exempel tar vi bort programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="c13f6-152">The following example deletes the application gateway.</span></span>

```powershell
# Retrieve the application gateway
$gw = Get-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG

# Stops the application gateway
Stop-AzureRmApplicationGateway -ApplicationGateway $gw

# Once the application gateway is in a stopped state, use the `Remove-AzureRmApplicationGateway` cmdlet to remove the service.
Remove-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG -Force
```

> [!NOTE]
> <span data-ttu-id="c13f6-153">Du kan använda växeln **-force** om du inte vill att några bekräftelsemeddelanden ska visas.</span><span class="sxs-lookup"><span data-stu-id="c13f6-153">The **-force** switch can be used to suppress the remove confirmation message.</span></span>

<span data-ttu-id="c13f6-154">Du kan kontrollera att tjänsten har tagits bort genom att använda cmdleten `Get-AzureRmApplicationGateway`.</span><span class="sxs-lookup"><span data-stu-id="c13f6-154">To verify that the service has been removed, you can use the `Get-AzureRmApplicationGateway` cmdlet.</span></span> <span data-ttu-id="c13f6-155">Det här steget är inte obligatoriskt.</span><span class="sxs-lookup"><span data-stu-id="c13f6-155">This step is not required.</span></span>

```powershell
Get-AzureRmApplicationGateway -Name ContosoAppGateway -ResourceGroupName ContosoRG
```

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="c13f6-156">Hämta DNS-namn för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="c13f6-156">Get application gateway DNS name</span></span>

<span data-ttu-id="c13f6-157">När du har skapat gatewayen, är nästa steg att konfigurera klientprogrammet för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="c13f6-157">Once the gateway is created, the next step is to configure the front end for communication.</span></span> <span data-ttu-id="c13f6-158">När du använder en offentlig IP-adress krävs ett dynamiskt tilldelat DNS-namn som inte är användarvänligt.</span><span class="sxs-lookup"><span data-stu-id="c13f6-158">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="c13f6-159">För att säkerställa att slutanvändare kan nå programgatewayen kan en CNAME-post användas för att peka på den offentliga slutpunkten för programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="c13f6-159">To ensure end users can hit the application gateway, a CNAME record can be used to point to the public endpoint of the application gateway.</span></span> <span data-ttu-id="c13f6-160">Gör detta genom att hämta information om programgatewayen och dess associerade IP/DNS-namn med PublicIPAddress-elementet kopplat till programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="c13f6-160">To do this, retrieve details of the application gateway and its associated IP/DNS name using the PublicIPAddress element attached to the application gateway.</span></span> <span data-ttu-id="c13f6-161">Du kan göra detta med Azure DNS eller andra DNS-providers genom att skapa en CNAME-post som pekar på den [offentliga IP-adressen](../dns/dns-custom-domain.md#public-ip-address).</span><span class="sxs-lookup"><span data-stu-id="c13f6-161">This can be done with Azure DNS or other DNS providers, by creating a CNAME record that points to the [public IP address](../dns/dns-custom-domain.md#public-ip-address).</span></span> <span data-ttu-id="c13f6-162">Användning av A-poster rekommenderas inte eftersom VIP kan ändras vid omstart av programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="c13f6-162">The use of A-records is not recommended since the VIP may change on restart of application gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="c13f6-163">En IP-adress tilldelas till programgatewayen när tjänsten startas.</span><span class="sxs-lookup"><span data-stu-id="c13f6-163">An IP address is assigned to the application gateway when the service starts.</span></span>

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

## <a name="delete-all-resources"></a><span data-ttu-id="c13f6-164">Ta bort alla resurser</span><span class="sxs-lookup"><span data-stu-id="c13f6-164">Delete all resources</span></span>

<span data-ttu-id="c13f6-165">Så här tar du bort alla resurser som skapats i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="c13f6-165">To delete all resources created in this article, complete the following step:</span></span>

```powershell
Remove-AzureRmResourceGroup -Name ContosoRG
```

## <a name="next-steps"></a><span data-ttu-id="c13f6-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c13f6-166">Next steps</span></span>

<span data-ttu-id="c13f6-167">Om du vill konfigurera SSL-avlastning läser du [Konfigurera en programgateway för SSL-avlastning](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="c13f6-167">If you want to configure SSL offload, visit: [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="c13f6-168">Om du vill konfigurera en programgateway för användning med en intern belastningsutjämnare läser du [Skapa en programgateway med en intern belastningsutjämnare (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="c13f6-168">If you want to configure an application gateway to use with an internal load balancer, visit: [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="c13f6-169">Om du vill ha mer information om belastningsutjämningsalternativ i allmänhet läser du:</span><span class="sxs-lookup"><span data-stu-id="c13f6-169">If you want more information about load balancing options in general, visit:</span></span>

* [<span data-ttu-id="c13f6-170">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="c13f6-170">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="c13f6-171">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="c13f6-171">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)
