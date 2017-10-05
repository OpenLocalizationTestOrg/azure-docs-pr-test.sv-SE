---
title: "Skapa en Programgateway som värd för flera platser | Microsoft Docs"
description: "Den här sidan innehåller instruktioner för att skapa, konfigurera en gateway för Azure-program som värd för flera webbprogram på samma gateway."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: b107d647-c9be-499f-8b55-809c4310c783
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/12/2016
ms.author: amsriva
ms.openlocfilehash: d42efa7d359f5c87c14afbfd138328b37c8ae6c2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-application-gateway-for-hosting-multiple-web-applications"></a><span data-ttu-id="6badc-103">Skapa en Programgateway som värd för flera webbprogram</span><span class="sxs-lookup"><span data-stu-id="6badc-103">Create an application gateway for hosting multiple web applications</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="6badc-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6badc-104">Azure portal</span></span>](application-gateway-create-multisite-portal.md)
> * [<span data-ttu-id="6badc-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6badc-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-multisite-azureresourcemanager-powershell.md)

<span data-ttu-id="6badc-106">Värd för flera plats kan du distribuera flera webbprogram på samma programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="6badc-106">Multiple site hosting allows you to deploy more than one web application on the same application gateway.</span></span> <span data-ttu-id="6badc-107">Det är beroende av förekomsten av värdhuvudet i den inkommande HTTP-begäranden att avgöra vilka lyssnare skulle ta emot trafik.</span><span class="sxs-lookup"><span data-stu-id="6badc-107">It relies on presence of host header in the incoming HTTP request, to determine which listener would receive traffic.</span></span> <span data-ttu-id="6badc-108">Lyssnaren dirigerar sedan trafik till lämplig serverdelspool som konfigurerats i regler definitionen av gatewayen.</span><span class="sxs-lookup"><span data-stu-id="6badc-108">The listener then directs traffic to appropriate backend pool as configured in the rules definition of the gateway.</span></span> <span data-ttu-id="6badc-109">I SSL aktiverat webbprogram Programgateway förlitar sig på servern Servernamnsindikation (SNI)-tillägg för att välja rätt lyssnaren för Internet-trafik.</span><span class="sxs-lookup"><span data-stu-id="6badc-109">In SSL enabled web applications, application gateway relies on the Server Name Indication (SNI) extension to choose the correct listener for the web traffic.</span></span> <span data-ttu-id="6badc-110">Ett vanligt användningsområde för värd för flera plats är att belastningsutjämna förfrågningar för olika webbdomäner till olika backend-serverpooler.</span><span class="sxs-lookup"><span data-stu-id="6badc-110">A common use for multiple site hosting is to load balance requests for different web domains to different back-end server pools.</span></span> <span data-ttu-id="6badc-111">På samma sätt kan flera underdomäner på samma rotdomänen också finnas på samma programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="6badc-111">Similarly multiple subdomains of the same root domain could also be hosted on the same application gateway.</span></span>

## <a name="scenario"></a><span data-ttu-id="6badc-112">Scenario</span><span class="sxs-lookup"><span data-stu-id="6badc-112">Scenario</span></span>

<span data-ttu-id="6badc-113">I följande exempel Programgateway betjäna trafik för contoso.com och fabrikam.com med två backend-server-adresspooler: contoso-serverpoolen och fabrikam-serverpoolen.</span><span class="sxs-lookup"><span data-stu-id="6badc-113">In the following example, application gateway is serving traffic for contoso.com and fabrikam.com with two back-end server pools: contoso server pool and fabrikam server pool.</span></span> <span data-ttu-id="6badc-114">Liknande installationsprogrammet kan användas för att värden underdomäner som app.contoso.com och blog.contoso.com.</span><span class="sxs-lookup"><span data-stu-id="6badc-114">Similar setup could be used to host subdomains like app.contoso.com and blog.contoso.com.</span></span>

![imageURLroute](./media/application-gateway-create-multisite-azureresourcemanager-powershell/multisite.png)

## <a name="before-you-begin"></a><span data-ttu-id="6badc-116">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="6badc-116">Before you begin</span></span>

1. <span data-ttu-id="6badc-117">Installera den senaste versionen av Azure PowerShell-cmdlets med hjälp av installationsprogrammet för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="6badc-117">Install the latest version of the Azure PowerShell cmdlets by using the Web Platform Installer.</span></span> <span data-ttu-id="6badc-118">Du kan hämta och installera den senaste versionen från avsnittet om **Windows PowerShell** på [hämtningssidan](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="6badc-118">You can download and install the latest version from the **Windows PowerShell** section of the [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="6badc-119">Servrar som läggs till backend-poolen som ska användas programgatewayen måste finnas eller har skapat sina slutpunkter i det virtuella nätverket i ett separat undernät eller med en offentlig IP-adress/VIP tilldelad.</span><span class="sxs-lookup"><span data-stu-id="6badc-119">The servers added to the back-end pool to use the application gateway must exist or have their endpoints created either in the virtual network in a separate subnet or with a public IP/VIP assigned.</span></span>

## <a name="requirements"></a><span data-ttu-id="6badc-120">Krav</span><span class="sxs-lookup"><span data-stu-id="6badc-120">Requirements</span></span>

* <span data-ttu-id="6badc-121">**Backend-serverpool:** Listan med IP-adresser för backend-servrarna.</span><span class="sxs-lookup"><span data-stu-id="6badc-121">**Back-end server pool:** The list of IP addresses of the back-end servers.</span></span> <span data-ttu-id="6badc-122">IP-adresserna som anges bör antingen tillhöra det virtuella undernätet eller vara en offentlig IP-/VIP-adress.</span><span class="sxs-lookup"><span data-stu-id="6badc-122">The IP addresses listed should either belong to the virtual network subnet or should be a public IP/VIP.</span></span> <span data-ttu-id="6badc-123">FQDN kan också användas.</span><span class="sxs-lookup"><span data-stu-id="6badc-123">FQDN can also be used.</span></span>
* <span data-ttu-id="6badc-124">**Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet.</span><span class="sxs-lookup"><span data-stu-id="6badc-124">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="6badc-125">Dessa inställningar är knutna till en pool och tillämpas på alla servrar i poolen.</span><span class="sxs-lookup"><span data-stu-id="6badc-125">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="6badc-126">**Frontend-port:** Den här porten är den offentliga porten som är öppen på programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="6badc-126">**Front-end port:** This port is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="6badc-127">Trafiken kommer till den här porten och omdirigeras till en av backend-servrarna.</span><span class="sxs-lookup"><span data-stu-id="6badc-127">Traffic hits this port, and then gets redirected to one of the back-end servers.</span></span>
* <span data-ttu-id="6badc-128">**Lyssnare:** Lyssnaren har en frontend-port, ett protokoll (Http eller Https; dessa värden är skiftlägeskänsliga) och SSL-certifikatnamnet (om du konfigurerar SSL-avlastning).</span><span class="sxs-lookup"><span data-stu-id="6badc-128">**Listener:** The listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span> <span data-ttu-id="6badc-129">För flera platser aktiverade programmet gateway läggs värdnamn och SNI indikatorer till.</span><span class="sxs-lookup"><span data-stu-id="6badc-129">For multi-site enabled application gateways, host name and SNI indicators are also added.</span></span>
* <span data-ttu-id="6badc-130">**Regel:** regeln Binder lyssnaren poolen backend-server och definierar vilka backend-serverpoolen trafiken ska dirigeras till när den når en viss lyssnare.</span><span class="sxs-lookup"><span data-stu-id="6badc-130">**Rule:** The rule binds the listener, the back-end server pool, and defines which back-end server pool the traffic should be directed to when it hits a particular listener.</span></span> <span data-ttu-id="6badc-131">Regler bearbetas i angiven ordning och trafik dirigeras via den första regeln som matchar oavsett särskilda egenskaper.</span><span class="sxs-lookup"><span data-stu-id="6badc-131">Rules are processed in the order they are listed, and traffic will be directed via the first rule that matches regardless of specificity.</span></span> <span data-ttu-id="6badc-132">Till exempel om du har en regel med hjälp av en grundläggande lyssnare och en regel med en flera platser lyssnare båda på samma port måste regeln med flera platser lyssnaren anges innan en regel med grundläggande lyssnare för flera platser regeln för att fungera som förväntat.</span><span class="sxs-lookup"><span data-stu-id="6badc-132">For example, if you have a rule using a basic listener and a rule using a multi-site listener both on the same port, the rule with the multi-site listener must be listed before the rule with the basic listener in order for the multi-site rule to function as expected.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="6badc-133">Skapa en programgateway</span><span class="sxs-lookup"><span data-stu-id="6badc-133">Create an application gateway</span></span>

<span data-ttu-id="6badc-134">Följande är de steg som behövs för att skapa en Programgateway:</span><span class="sxs-lookup"><span data-stu-id="6badc-134">The following are the steps needed to create an application gateway:</span></span>

1. <span data-ttu-id="6badc-135">Skapa en resursgrupp för Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6badc-135">Create a resource group for Resource Manager.</span></span>
2. <span data-ttu-id="6badc-136">Skapa ett virtuellt nätverk, undernät och offentliga IP för programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="6badc-136">Create a virtual network, subnets, and public IP for the application gateway.</span></span>
3. <span data-ttu-id="6badc-137">Skapa ett konfigurationsobjekt för programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="6badc-137">Create an application gateway configuration object.</span></span>
4. <span data-ttu-id="6badc-138">Skapa en resurs för en programgateway.</span><span class="sxs-lookup"><span data-stu-id="6badc-138">Create an application gateway resource.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="6badc-139">Skapa en resursgrupp för Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6badc-139">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="6badc-140">Kontrollera att du använder den senaste versionen av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6badc-140">Make sure that you are using the latest version of Azure PowerShell.</span></span> <span data-ttu-id="6badc-141">Mer information finns på [med hjälp av Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="6badc-141">More information is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="6badc-142">Steg 1</span><span class="sxs-lookup"><span data-stu-id="6badc-142">Step 1</span></span>

<span data-ttu-id="6badc-143">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="6badc-143">Log in to Azure</span></span>

```powershell
Login-AzureRmAccount
```
<span data-ttu-id="6badc-144">Du ombeds att autentisera dig med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="6badc-144">You are prompted to authenticate with your credentials.</span></span>

### <a name="step-2"></a><span data-ttu-id="6badc-145">Steg 2</span><span class="sxs-lookup"><span data-stu-id="6badc-145">Step 2</span></span>

<span data-ttu-id="6badc-146">Kontrollera prenumerationerna för kontot.</span><span class="sxs-lookup"><span data-stu-id="6badc-146">Check the subscriptions for the account.</span></span>

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a><span data-ttu-id="6badc-147">Steg 3</span><span class="sxs-lookup"><span data-stu-id="6badc-147">Step 3</span></span>

<span data-ttu-id="6badc-148">Välj vilka av dina Azure-prenumerationer som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="6badc-148">Choose which of your Azure subscriptions to use.</span></span>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a><span data-ttu-id="6badc-149">Steg 4</span><span class="sxs-lookup"><span data-stu-id="6badc-149">Step 4</span></span>

<span data-ttu-id="6badc-150">Skapa en resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp).</span><span class="sxs-lookup"><span data-stu-id="6badc-150">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-RG -location "West US"
```

<span data-ttu-id="6badc-151">Alternativt kan du också skapa taggar för en resursgrupp för Programgateway:</span><span class="sxs-lookup"><span data-stu-id="6badc-151">Alternatively you can also create tags for a resource group for application gateway:</span></span>

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway multiple site"}
```

<span data-ttu-id="6badc-152">Azure Resource Manager kräver att alla resursgrupper anger en plats.</span><span class="sxs-lookup"><span data-stu-id="6badc-152">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="6badc-153">Den här platsen används som standardplats för resurserna i den resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="6badc-153">This location is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="6badc-154">Se till att alla kommandon för att skapa en Programgateway använder samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="6badc-154">Make sure that all commands to create an application gateway use the same resource group.</span></span>

<span data-ttu-id="6badc-155">I exemplet ovan kan vi skapa en resursgrupp med namnet **appgw RG** med en plats för **västra USA**.</span><span class="sxs-lookup"><span data-stu-id="6badc-155">In the example above, we created a resource group called **appgw-RG** with a location of **West US**.</span></span>

> [!NOTE]
> <span data-ttu-id="6badc-156">Om du behöver konfigurera en anpassad avsökning för din programgateway läser du [Skapa en programgateway med anpassade avsökningar med hjälp av PowerShell](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="6badc-156">If you need to configure a custom probe for your application gateway, see [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md).</span></span> <span data-ttu-id="6badc-157">Besök [anpassade avsökningar, hälsoövervakning och](application-gateway-probe-overview.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="6badc-157">Visit [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>

## <a name="create-a-virtual-network-and-subnets"></a><span data-ttu-id="6badc-158">Skapa ett virtuellt nätverk och undernät</span><span class="sxs-lookup"><span data-stu-id="6badc-158">Create a virtual network and subnets</span></span>

<span data-ttu-id="6badc-159">Följande exempel illustrerar hur du skapar ett virtuellt nätverk med hjälp av Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6badc-159">The following example shows how to create a virtual network by using Resource Manager.</span></span> <span data-ttu-id="6badc-160">Två undernät skapas i det här steget.</span><span class="sxs-lookup"><span data-stu-id="6badc-160">Two subnets are created in this step.</span></span> <span data-ttu-id="6badc-161">Det första undernätet är för Programgateway sig själv.</span><span class="sxs-lookup"><span data-stu-id="6badc-161">The first subnet is for the application gateway itself.</span></span> <span data-ttu-id="6badc-162">Programgateway kräver sin egen undernät för instanser.</span><span class="sxs-lookup"><span data-stu-id="6badc-162">Application gateway requires its own subnet to hold its instances.</span></span> <span data-ttu-id="6badc-163">Andra programgatewayer kan distribueras i det undernätet.</span><span class="sxs-lookup"><span data-stu-id="6badc-163">Only other application gateways can be deployed in that subnet.</span></span> <span data-ttu-id="6badc-164">Andra undernätet används för att hålla program backend-servrarna.</span><span class="sxs-lookup"><span data-stu-id="6badc-164">The second subnet is used to hold the application backend servers.</span></span>

### <a name="step-1"></a><span data-ttu-id="6badc-165">Steg 1</span><span class="sxs-lookup"><span data-stu-id="6badc-165">Step 1</span></span>

<span data-ttu-id="6badc-166">Tilldela variabeln undernät som används för att hålla programgatewayen adressintervallet 10.0.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="6badc-166">Assign the address range 10.0.0.0/24 to the subnet variable to be used to hold the application gateway.</span></span>

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -AddressPrefix 10.0.0.0/24
```
### <a name="step-2"></a><span data-ttu-id="6badc-167">Steg 2</span><span class="sxs-lookup"><span data-stu-id="6badc-167">Step 2</span></span>

<span data-ttu-id="6badc-168">Tilldela variabeln Undernät2 som ska användas för backend-pooler adressintervallet 10.0.1.0/24.</span><span class="sxs-lookup"><span data-stu-id="6badc-168">Assign the address range 10.0.1.0/24 to the subnet2 variable to be used for the backend pools.</span></span>

```powershell
$subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -AddressPrefix 10.0.1.0/24
```

### <a name="step-3"></a><span data-ttu-id="6badc-169">Steg 3</span><span class="sxs-lookup"><span data-stu-id="6badc-169">Step 3</span></span>

<span data-ttu-id="6badc-170">Skapa ett virtuellt nätverk med namnet **appgwvnet** i resursgruppen **appgw rg** för regionen USA, västra med undernätet 10.0.0.0/24 prefixet 10.0.0.0/16 och 10.0.1.0/24.</span><span class="sxs-lookup"><span data-stu-id="6badc-170">Create a virtual network named **appgwvnet** in resource group **appgw-rg** for the West US region using the prefix 10.0.0.0/16 with subnet 10.0.0.0/24, and 10.0.1.0/24.</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet,$subnet2
```

### <a name="step-4"></a><span data-ttu-id="6badc-171">Steg 4</span><span class="sxs-lookup"><span data-stu-id="6badc-171">Step 4</span></span>

<span data-ttu-id="6badc-172">Tilldela en variabel för nästa steg, undernät som skapar en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="6badc-172">Assign a subnet variable for the next steps, which creates an application gateway.</span></span>

```powershell
$appgatewaysubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -VirtualNetwork $vnet
$backendsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a><span data-ttu-id="6badc-173">Skapa en offentlig IP-adress för frontend-konfigurationen</span><span class="sxs-lookup"><span data-stu-id="6badc-173">Create a public IP address for the front-end configuration</span></span>

<span data-ttu-id="6badc-174">Skapa en offentlig IP-resurs, **publicIP01**, i resursgruppen **appgw-rg** för regionen USA, västra.</span><span class="sxs-lookup"><span data-stu-id="6badc-174">Create a public IP resource **publicIP01** in resource group **appgw-rg** for the West US region.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

<span data-ttu-id="6badc-175">En IP-adress tilldelas till programgatewayen när tjänsten startas.</span><span class="sxs-lookup"><span data-stu-id="6badc-175">An IP address is assigned to the application gateway when the service starts.</span></span>

## <a name="create-application-gateway-configuration"></a><span data-ttu-id="6badc-176">Skapa program gateway-konfiguration</span><span class="sxs-lookup"><span data-stu-id="6badc-176">Create application gateway configuration</span></span>

<span data-ttu-id="6badc-177">Du måste konfigurera alla konfigurationsobjekt innan du skapar programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="6badc-177">You have to set up all configuration items before creating the application gateway.</span></span> <span data-ttu-id="6badc-178">Följande steg skapar konfigurationsobjekten som behövs för en programgatewayresurs.</span><span class="sxs-lookup"><span data-stu-id="6badc-178">The following steps create the configuration items that are needed for an application gateway resource.</span></span>

### <a name="step-1"></a><span data-ttu-id="6badc-179">Steg 1</span><span class="sxs-lookup"><span data-stu-id="6badc-179">Step 1</span></span>

<span data-ttu-id="6badc-180">Skapa en IP-konfiguration för programgatewayen med namnet **gatewayIP01**.</span><span class="sxs-lookup"><span data-stu-id="6badc-180">Create an application gateway IP configuration named **gatewayIP01**.</span></span> <span data-ttu-id="6badc-181">När Programgateway startar hämtar en IP-adress från det undernät som konfigurerats och dirigera nätverkstrafik till IP-adresser i backend-IP-adresspool.</span><span class="sxs-lookup"><span data-stu-id="6badc-181">When application gateway starts, it picks up an IP address from the subnet configured and route network traffic to the IP addresses in the back-end IP pool.</span></span> <span data-ttu-id="6badc-182">Tänk på att varje instans använder en IP-adress.</span><span class="sxs-lookup"><span data-stu-id="6badc-182">Keep in mind that each instance takes one IP address.</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $appgatewaysubnet
```

### <a name="step-2"></a><span data-ttu-id="6badc-183">Steg 2</span><span class="sxs-lookup"><span data-stu-id="6badc-183">Step 2</span></span>

<span data-ttu-id="6badc-184">Konfigurera backend-IP-adresspool med namnet **pool01** och **pool2** med IP-adresser **134.170.185.46**, **134.170.188.221**, **134.170.185.50** för **pool1** och **134.170.186.46**, **134.170.189.221**, **134.170.186.50** för **pool2**.</span><span class="sxs-lookup"><span data-stu-id="6badc-184">Configure the back-end IP address pool named **pool01** and **pool2** with IP addresses **134.170.185.46**, **134.170.188.221**, **134.170.185.50** for **pool1** and **134.170.186.46**, **134.170.189.221**, **134.170.186.50** for **pool2**.</span></span>

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.0.1.100, 10.0.1.101, 10.0.1.102
$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 10.0.1.103, 10.0.1.104, 10.0.1.105
```

<span data-ttu-id="6badc-185">I det här exemplet finns det två backend-adresspooler för routning av nätverkstrafik baserat på den begärda webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="6badc-185">In this example, there are two back-end pools to route network traffic based on the requested site.</span></span> <span data-ttu-id="6badc-186">En pool tar emot trafik från platsen ”contoso.com” och andra poolen tar emot trafik från platsen ”fabrikam.com”.</span><span class="sxs-lookup"><span data-stu-id="6badc-186">One pool receives traffic from site "contoso.com" and other pool receives traffic from site "fabrikam.com".</span></span> <span data-ttu-id="6badc-187">Du måste ersätta föregående IP-adresser för att lägga till egna programslutpunkter IP-adress.</span><span class="sxs-lookup"><span data-stu-id="6badc-187">You have to replace the preceding IP addresses to add your own application IP address endpoints.</span></span> <span data-ttu-id="6badc-188">I stället för interna IP-adresser, kan du också använda offentliga IP-adresser, FQDN eller en virtuell dators nätverkskort för backend-instanser.</span><span class="sxs-lookup"><span data-stu-id="6badc-188">In place of internal IP addresses, you could also use public IP addresses, FQDN, or a VM's NIC for backend instances.</span></span> <span data-ttu-id="6badc-189">Ange FQDN: er i stället för IP-adresser i PowerShell Använd ”-BackendFQDNs” parametern.</span><span class="sxs-lookup"><span data-stu-id="6badc-189">To specify FQDNs instead of IPs in PowerShell use "-BackendFQDNs" parameter.</span></span>

### <a name="step-3"></a><span data-ttu-id="6badc-190">Steg 3</span><span class="sxs-lookup"><span data-stu-id="6badc-190">Step 3</span></span>

<span data-ttu-id="6badc-191">Konfigurera gateway programinställning **poolsetting01** och **poolsetting02** för nätverkstrafik Utjämning av nätverksbelastning i backend-poolen.</span><span class="sxs-lookup"><span data-stu-id="6badc-191">Configure application gateway setting **poolsetting01** and **poolsetting02** for the load-balanced network traffic in the back-end pool.</span></span> <span data-ttu-id="6badc-192">I det här exemplet kan du konfigurera inställningarna för olika backend-pool för backend-pooler.</span><span class="sxs-lookup"><span data-stu-id="6badc-192">In this example, you configure different back-end pool settings for the back-end pools.</span></span> <span data-ttu-id="6badc-193">Varje serverdelspool kan ha sin egen serverdelspoolinställning.</span><span class="sxs-lookup"><span data-stu-id="6badc-193">Each back-end pool can have its own back-end pool setting.</span></span>

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120
$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a><span data-ttu-id="6badc-194">Steg 4</span><span class="sxs-lookup"><span data-stu-id="6badc-194">Step 4</span></span>

<span data-ttu-id="6badc-195">Konfigurera klientdelens IP med den offentliga IP-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="6badc-195">Configure the front-end IP with public IP endpoint.</span></span>

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a><span data-ttu-id="6badc-196">Steg 5</span><span class="sxs-lookup"><span data-stu-id="6badc-196">Step 5</span></span>

<span data-ttu-id="6badc-197">Konfigurera klientdelsporten för en programgateway.</span><span class="sxs-lookup"><span data-stu-id="6badc-197">Configure the front-end port for an application gateway.</span></span>

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 443
```

### <a name="step-6"></a><span data-ttu-id="6badc-198">Steg 6</span><span class="sxs-lookup"><span data-stu-id="6badc-198">Step 6</span></span>

<span data-ttu-id="6badc-199">Konfigurera två SSL-certifikat för de två webbplatserna ska stödja i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="6badc-199">Configure two SSL certificates for the two websites we are going to support in this example.</span></span> <span data-ttu-id="6badc-200">Ett certifikat är för contoso.com trafik och den andra för fabrikam.com trafik.</span><span class="sxs-lookup"><span data-stu-id="6badc-200">One certificate is for contoso.com traffic and the other is for fabrikam.com traffic.</span></span> <span data-ttu-id="6badc-201">Dessa certifikat ska vara en certifikatutfärdare som utfärdade certifikat för webbplatser.</span><span class="sxs-lookup"><span data-stu-id="6badc-201">These certificates should be a Certificate Authority issued certificates for your websites.</span></span> <span data-ttu-id="6badc-202">Självsignerade certifikat stöds men rekommenderas inte för produktion trafik.</span><span class="sxs-lookup"><span data-stu-id="6badc-202">Self-signed certificates are supported but not recommended for production traffic.</span></span>

```powershell
$cert01 = New-AzureRmApplicationGatewaySslCertificate -Name contosocert -CertificateFile <file path> -Password <password>
$cert02 = New-AzureRmApplicationGatewaySslCertificate -Name fabrikamcert -CertificateFile <file path> -Password <password>
```

### <a name="step-7"></a><span data-ttu-id="6badc-203">Steg 7</span><span class="sxs-lookup"><span data-stu-id="6badc-203">Step 7</span></span>

<span data-ttu-id="6badc-204">Konfigurera två lyssnare för två webbplatser i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="6badc-204">Configure two listeners for the two web sites in this example.</span></span> <span data-ttu-id="6badc-205">Det här steget konfigurerar lyssnare för offentlig IP-adress, port och värdnamn används för att ta emot inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="6badc-205">This step configures the listeners for public IP address, port, and host used to receive incoming traffic.</span></span> <span data-ttu-id="6badc-206">HostName parametern krävs för stöd för flera plats och ska vara inställd på lämplig webbplats för vilken trafiken som tas emot.</span><span class="sxs-lookup"><span data-stu-id="6badc-206">HostName parameter is required for multiple site support and should be set to the appropriate website for which the traffic is received.</span></span> <span data-ttu-id="6badc-207">RequireServerNameIndication parameter ska vara inställd på true för webbplatser som behöver stöd för SSL i ett scenario med flera värden.</span><span class="sxs-lookup"><span data-stu-id="6badc-207">RequireServerNameIndication parameter should be set to true for websites that need support for SSL in a multiple host scenario.</span></span> <span data-ttu-id="6badc-208">Om SSL-stöd krävs, måste du också ange SSL-certifikatet som används för att skydda trafik för att webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="6badc-208">If SSL support is required, you also need to specify the SSL certificate that is used to secure traffic for that web application.</span></span> <span data-ttu-id="6badc-209">Kombinationen av FrontendIPConfiguration och FrontendPort värdnamn måste vara unikt för en lyssnare.</span><span class="sxs-lookup"><span data-stu-id="6badc-209">The combination of FrontendIPConfiguration, FrontendPort, and HostName must be unique to a listener.</span></span> <span data-ttu-id="6badc-210">Varje lyssnare har stöd för ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="6badc-210">Each listener can support one certificate.</span></span>

```powershell
$listener01 = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "contoso11.com" -RequireServerNameIndication true  -SslCertificate $cert01
$listener02 = New-AzureRmApplicationGatewayHttpListener -Name "listener02" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "fabrikam11.com" -RequireServerNameIndication true -SslCertificate $cert02
```

### <a name="step-8"></a><span data-ttu-id="6badc-211">Steg 8</span><span class="sxs-lookup"><span data-stu-id="6badc-211">Step 8</span></span>

<span data-ttu-id="6badc-212">Skapa två inställning för regel för två webbprogram i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="6badc-212">Create two rule setting for the two web applications in this example.</span></span> <span data-ttu-id="6badc-213">En regel innehåller lyssnare, serverdelspooler och http-inställningar.</span><span class="sxs-lookup"><span data-stu-id="6badc-213">A rule ties together listeners, backend pools and http settings.</span></span> <span data-ttu-id="6badc-214">Det här steget konfigurerar Programgateway om du vill använda grundläggande routningsregel, ett för varje webbplats.</span><span class="sxs-lookup"><span data-stu-id="6badc-214">This step configures the application gateway to use Basic routing rule, one for each website.</span></span> <span data-ttu-id="6badc-215">Trafik till varje webbplats tas emot av dess konfigurerade lyssnare och omdirigeras sedan till dess konfigurerade serverdelspool med hjälp av egenskaper som anges i BackendHttpSettings.</span><span class="sxs-lookup"><span data-stu-id="6badc-215">Traffic to each website is received by its configured listener, and is then directed to its configured backend pool, using the properties specified in the BackendHttpSettings.</span></span>

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule01" -RuleType Basic -HttpListener $listener01 -BackendHttpSettings $poolSetting01 -BackendAddressPool $pool1
$rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule02" -RuleType Basic -HttpListener $listener02 -BackendHttpSettings $poolSetting02 -BackendAddressPool $pool2
```

### <a name="step-9"></a><span data-ttu-id="6badc-216">Steg 9</span><span class="sxs-lookup"><span data-stu-id="6badc-216">Step 9</span></span>

<span data-ttu-id="6badc-217">Konfigurera antalet instanser av och storleken på programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="6badc-217">Configure the number of instances and size for the application gateway.</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Medium" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a><span data-ttu-id="6badc-218">Skapa Programgateway</span><span class="sxs-lookup"><span data-stu-id="6badc-218">Create application gateway</span></span>

<span data-ttu-id="6badc-219">Skapa en Programgateway med alla konfigurationsobjekt från föregående steg.</span><span class="sxs-lookup"><span data-stu-id="6badc-219">Create an application gateway with all configuration objects from the preceding steps.</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 -Sku $sku -SslCertificates $cert01, $cert02
```

> [!IMPORTANT]
> <span data-ttu-id="6badc-220">Programmet Gateway etablering är en tidskrävande åtgärd och kan ta lite tid att slutföra.</span><span class="sxs-lookup"><span data-stu-id="6badc-220">Application Gateway provisioning is a long running operation and may take some time to complete.</span></span>
> 
> 

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="6badc-221">Hämta DNS-namn för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="6badc-221">Get application gateway DNS name</span></span>

<span data-ttu-id="6badc-222">När du har skapat gatewayen, är nästa steg att konfigurera klientprogrammet för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="6badc-222">Once the gateway is created, the next step is to configure the front end for communication.</span></span> <span data-ttu-id="6badc-223">När du använder en offentlig IP-adress krävs ett dynamiskt tilldelat DNS-namn som inte är användarvänligt.</span><span class="sxs-lookup"><span data-stu-id="6badc-223">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="6badc-224">För att säkerställa att slutanvändare kan nå programgatewayen kan en CNAME-post användas för att peka på den offentliga slutpunkten för programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="6badc-224">To ensure end users can hit the application gateway, a CNAME record can be used to point to the public endpoint of the application gateway.</span></span> <span data-ttu-id="6badc-225">[Konfigurera ett eget domännamn i Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6badc-225">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="6badc-226">Gör detta genom att hämta information om programgatewayen och dess associerade IP/DNS-namn med PublicIPAddress-elementet kopplat till programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="6badc-226">To do this, retrieve details of the application gateway and its associated IP/DNS name using the PublicIPAddress element attached to the application gateway.</span></span> <span data-ttu-id="6badc-227">programgatewayens DNS-namn ska användas för att skapa en CNAME-post som leder de två webbapparna till detta DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="6badc-227">The application gateway's DNS name should be used to create a CNAME record, which points the two web applications to this DNS name.</span></span> <span data-ttu-id="6badc-228">Användning av A-poster rekommenderas inte eftersom VIP kan ändras vid omstart av programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="6badc-228">The use of A-records is not recommended since the VIP may change on restart of application gateway.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="6badc-229">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6badc-229">Next steps</span></span>

<span data-ttu-id="6badc-230">Lär dig att skydda dina webbplatser med [Programgateway - Brandvägg för webbaserade program](application-gateway-webapplicationfirewall-overview.md)</span><span class="sxs-lookup"><span data-stu-id="6badc-230">Learn how to protect your websites with [Application Gateway - Web Application Firewall](application-gateway-webapplicationfirewall-overview.md)</span></span>

