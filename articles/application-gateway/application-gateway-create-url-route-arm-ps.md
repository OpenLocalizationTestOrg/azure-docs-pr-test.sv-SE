---
title: "Skapa en Programgateway med hjälp av URL: en routningsregler | Microsoft Docs"
description: "Den här sidan innehåller instruktioner för att skapa, konfigurera en gateway för Azure-program som använder routningsregler för URL"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: d141cfbb-320a-4fc9-9125-10001c6fa4cf
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: gwallace
ms.openlocfilehash: ba756d3262b9780c5701e69faad860ba32bba08b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-application-gateway-using-path-based-routing"></a><span data-ttu-id="b25ed-103">Skapa en Programgateway använder sökväg-baserade Routning</span><span class="sxs-lookup"><span data-stu-id="b25ed-103">Create an application gateway using Path-based routing</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b25ed-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b25ed-104">Azure portal</span></span>](application-gateway-create-url-route-portal.md)
> * [<span data-ttu-id="b25ed-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b25ed-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-url-route-arm-ps.md)
> * [<span data-ttu-id="b25ed-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b25ed-106">Azure CLI 2.0</span></span>](application-gateway-create-url-route-cli.md)

<span data-ttu-id="b25ed-107">URL-sökväg-baserad routning kan du associera rutter baserat på en URL-sökväg för en Http-begäran.</span><span class="sxs-lookup"><span data-stu-id="b25ed-107">URL Path-based routing enables you to associate routes based on the URL path of an Http request.</span></span> <span data-ttu-id="b25ed-108">Den kontrollerar om det finns en väg till en backend-adresspool som konfigurerats för den URL som visas i Programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="b25ed-108">It checks if there is a route to a back-end pool configured for the URL presented in the Application Gateway.</span></span> <span data-ttu-id="b25ed-109">Sedan skickar den nätverkstrafiken till definierade backend-poolen.</span><span class="sxs-lookup"><span data-stu-id="b25ed-109">It then sends the network traffic to the defined back-end pool.</span></span> <span data-ttu-id="b25ed-110">Ett vanligt användningsområde för URL-baserade routning är att belastningsutjämna förfrågningar för olika typer av innehåll till olika backend-serverpooler.</span><span class="sxs-lookup"><span data-stu-id="b25ed-110">A common use for URL-based routing is to load balance requests for different content types to different back-end server pools.</span></span>

<span data-ttu-id="b25ed-111">URL-baserade routning introducerar en ny regeltyp av i Programgateway.</span><span class="sxs-lookup"><span data-stu-id="b25ed-111">URL-based routing introduces a new rule type to application gateway.</span></span> <span data-ttu-id="b25ed-112">Programgateway har två regeltyper: basic och PathBasedRouting.</span><span class="sxs-lookup"><span data-stu-id="b25ed-112">Application gateway has two rule types: basic and PathBasedRouting.</span></span> <span data-ttu-id="b25ed-113">Grundläggande regeltyp resursallokering tjänst för backend-pooler när PathBasedRouting förutom resursallokering (round robin), också beaktas sökvägar för den begärda Webbadressen när du väljer serverdelspoolen.</span><span class="sxs-lookup"><span data-stu-id="b25ed-113">Basic rule type provides round-robin service for the back-end pools while PathBasedRouting in addition to round robin distribution, also takes path pattern of the request URL into account while choosing the backend pool.</span></span>

## <a name="scenario"></a><span data-ttu-id="b25ed-114">Scenario</span><span class="sxs-lookup"><span data-stu-id="b25ed-114">Scenario</span></span>

<span data-ttu-id="b25ed-115">I följande exempel Programgateway betjäna trafik för contoso.com med två backend-server-adresspooler: video-serverpoolen och image-serverpoolen.</span><span class="sxs-lookup"><span data-stu-id="b25ed-115">In the following example, Application Gateway is serving traffic for contoso.com with two back-end server pools: video server pool and image server pool.</span></span>

<span data-ttu-id="b25ed-116">Begäranden för http://contoso.com/image * dirigeras till bilden serverpoolen (pool1) och http://contoso.com/video * dirigeras till video serverpoolen (pool2).</span><span class="sxs-lookup"><span data-stu-id="b25ed-116">Requests for http://contoso.com/image* are routed to image server pool (pool1), and http://contoso.com/video* are routed to video server pool (pool2).</span></span> <span data-ttu-id="b25ed-117">Om inget av mönster som sökväg matchar väljs en standard serverpool (pool1).</span><span class="sxs-lookup"><span data-stu-id="b25ed-117">if none of the path patterns match, a default server pool (pool1) is selected.</span></span>

![URL-väg](./media/application-gateway-create-url-route-arm-ps/figure1.png)

## <a name="before-you-begin"></a><span data-ttu-id="b25ed-119">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="b25ed-119">Before you begin</span></span>

1. <span data-ttu-id="b25ed-120">Installera den senaste versionen av Azure PowerShell-cmdlets med hjälp av installationsprogrammet för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="b25ed-120">Install the latest version of the Azure PowerShell cmdlets by using the Web Platform Installer.</span></span> <span data-ttu-id="b25ed-121">Du kan hämta och installera den senaste versionen från avsnittet om **Windows PowerShell** på [hämtningssidan](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="b25ed-121">You can download and install the latest version from the **Windows PowerShell** section of the [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="b25ed-122">Du skapar ett virtuellt nätverk och undernät för Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="b25ed-122">You create a virtual network and subnet for Application Gateway.</span></span> <span data-ttu-id="b25ed-123">Kontrollera att inga virtuella datorer eller molndistributioner använder undernätet.</span><span class="sxs-lookup"><span data-stu-id="b25ed-123">Make sure that no virtual machines or cloud deployments are using the subnet.</span></span> <span data-ttu-id="b25ed-124">Programgatewayen måste vara fristående i ett virtuellt nätverks undernät.</span><span class="sxs-lookup"><span data-stu-id="b25ed-124">The application gateway must be by itself in a virtual network subnet.</span></span>
3. <span data-ttu-id="b25ed-125">Servrar som läggs till backend-poolen som ska användas programgatewayen måste finnas eller har skapat sina slutpunkter i det virtuella nätverket eller med en offentlig IP-adress/VIP tilldelad.</span><span class="sxs-lookup"><span data-stu-id="b25ed-125">The servers added to the back-end pool to use the application gateway must exist or have their endpoints created either in the virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-to-create-an-application-gateway"></a><span data-ttu-id="b25ed-126">Vad krävs för att skapa en programgateway?</span><span class="sxs-lookup"><span data-stu-id="b25ed-126">What is required to create an application gateway?</span></span>

* <span data-ttu-id="b25ed-127">**Backend-serverpool:** Listan med IP-adresser för backend-servrarna.</span><span class="sxs-lookup"><span data-stu-id="b25ed-127">**Back-end server pool:** The list of IP addresses of the back-end servers.</span></span> <span data-ttu-id="b25ed-128">IP-adresserna som anges bör antingen tillhöra det virtuella undernätet eller vara en offentlig IP-/VIP-adress.</span><span class="sxs-lookup"><span data-stu-id="b25ed-128">The IP addresses listed should either belong to the virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="b25ed-129">**Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet.</span><span class="sxs-lookup"><span data-stu-id="b25ed-129">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="b25ed-130">Dessa inställningar är knutna till en pool och tillämpas på alla servrar i poolen.</span><span class="sxs-lookup"><span data-stu-id="b25ed-130">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="b25ed-131">**Frontend-port:** Den här porten är den offentliga porten som är öppen på programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="b25ed-131">**Front-end port:** This port is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="b25ed-132">Trafiken kommer till den här porten och omdirigeras till en av backend-servrarna.</span><span class="sxs-lookup"><span data-stu-id="b25ed-132">Traffic hits this port, and then gets redirected to one of the back-end servers.</span></span>
* <span data-ttu-id="b25ed-133">**Lyssnare:** Lyssnaren har en frontend-port, ett protokoll (Http eller Https; dessa värden är skiftlägeskänsliga) och SSL-certifikatnamnet (om du konfigurerar SSL-avlastning).</span><span class="sxs-lookup"><span data-stu-id="b25ed-133">**Listener:** The listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="b25ed-134">**Regel:** Regeln binder lyssnaren och backend-serverpoolen och definierar vilken backend-serverpool som trafiken ska dirigeras till när den når en viss lyssnare.</span><span class="sxs-lookup"><span data-stu-id="b25ed-134">**Rule:** The rule binds the listener, the back-end server pool and defines which back-end server pool the traffic should be directed to when it hits a particular listener.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="b25ed-135">Skapa en programgateway</span><span class="sxs-lookup"><span data-stu-id="b25ed-135">Create an application gateway</span></span>

<span data-ttu-id="b25ed-136">Skillnaden mellan att använda den klassiska Azure-portalen och Azure Resource Manager är i vilken ordning du skapar programgatewayen och de objekt som ska konfigureras.</span><span class="sxs-lookup"><span data-stu-id="b25ed-136">The difference between using Azure Classic and Azure Resource Manager is the order in which you create the application gateway and the items that need to be configured.</span></span>

<span data-ttu-id="b25ed-137">Med Resource Manager konfigureras alla objekt som bildar en programgateway separat och sätts sedan ihop för att skapa en programgatewayresurs.</span><span class="sxs-lookup"><span data-stu-id="b25ed-137">With Resource Manager, all items that make an application gateway are configured individually and then put together to create the application gateway resource.</span></span>

<span data-ttu-id="b25ed-138">Här följer de steg som krävs för att skapa en programgateway:</span><span class="sxs-lookup"><span data-stu-id="b25ed-138">Here are the steps that are needed to create an application gateway:</span></span>

1. <span data-ttu-id="b25ed-139">Skapa en resursgrupp för Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b25ed-139">Create a resource group for Resource Manager.</span></span>
2. <span data-ttu-id="b25ed-140">Skapa ett virtuellt nätverk, ett undernät och en offentlig IP för programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="b25ed-140">Create a virtual network, subnet, and public IP for the application gateway.</span></span>
3. <span data-ttu-id="b25ed-141">Skapa ett konfigurationsobjekt för programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="b25ed-141">Create an application gateway configuration object.</span></span>
4. <span data-ttu-id="b25ed-142">Skapa en resurs för en programgateway.</span><span class="sxs-lookup"><span data-stu-id="b25ed-142">Create an application gateway resource.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="b25ed-143">Skapa en resursgrupp för Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b25ed-143">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="b25ed-144">Kontrollera att du använder den senaste versionen av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b25ed-144">Make sure that you are using the latest version of Azure PowerShell.</span></span> <span data-ttu-id="b25ed-145">Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="b25ed-145">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="b25ed-146">Steg 1</span><span class="sxs-lookup"><span data-stu-id="b25ed-146">Step 1</span></span>

<span data-ttu-id="b25ed-147">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="b25ed-147">Log in to Azure</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="b25ed-148">Du ombeds att autentisera dig med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="b25ed-148">You are prompted to authenticate with your credentials.</span></span><BR>

### <a name="step-2"></a><span data-ttu-id="b25ed-149">Steg 2</span><span class="sxs-lookup"><span data-stu-id="b25ed-149">Step 2</span></span>

<span data-ttu-id="b25ed-150">Kontrollera prenumerationerna för kontot.</span><span class="sxs-lookup"><span data-stu-id="b25ed-150">Check the subscriptions for the account.</span></span>

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a><span data-ttu-id="b25ed-151">Steg 3</span><span class="sxs-lookup"><span data-stu-id="b25ed-151">Step 3</span></span>

<span data-ttu-id="b25ed-152">Välj vilka av dina Azure-prenumerationer som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="b25ed-152">Choose which of your Azure subscriptions to use.</span></span> <BR>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a><span data-ttu-id="b25ed-153">Steg 4</span><span class="sxs-lookup"><span data-stu-id="b25ed-153">Step 4</span></span>

<span data-ttu-id="b25ed-154">Skapa en resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp).</span><span class="sxs-lookup"><span data-stu-id="b25ed-154">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US"
```

<span data-ttu-id="b25ed-155">Alternativt kan du också skapa taggar för en resursgrupp för Programgateway:</span><span class="sxs-lookup"><span data-stu-id="b25ed-155">Alternatively you can also create tags for a resource group for application gateway:</span></span>

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway URL routing"} 
```

<span data-ttu-id="b25ed-156">Azure Resource Manager kräver att alla resursgrupper anger en plats.</span><span class="sxs-lookup"><span data-stu-id="b25ed-156">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="b25ed-157">Den här resursgruppen används som standard för resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="b25ed-157">This resource group is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="b25ed-158">Se till att alla kommandon för att skapa en Programgateway använder samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="b25ed-158">Make sure that all commands to create an application gateway use the same resource group.</span></span>

<span data-ttu-id="b25ed-159">I exemplet ovan skapade vi resursgruppen ”appgw-RG” och platsen ”West US”.</span><span class="sxs-lookup"><span data-stu-id="b25ed-159">In the example above, we created a resource group called "appgw-RG" and location "West US".</span></span>

> [!NOTE]
> <span data-ttu-id="b25ed-160">Om du behöver konfigurera en anpassad avsökning för din programgateway läser du [Skapa en programgateway med anpassade avsökningar med hjälp av PowerShell](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="b25ed-160">If you need to configure a custom probe for your application gateway, see [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md).</span></span> <span data-ttu-id="b25ed-161">Mer information finns i [Anpassade avsökningar och hälsoövervakning](application-gateway-probe-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b25ed-161">Check out [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>
> 
> 

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a><span data-ttu-id="b25ed-162">Skapa ett virtuellt nätverk och ett undernät för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="b25ed-162">Create a virtual network and a subnet for the application gateway</span></span>

<span data-ttu-id="b25ed-163">Följande exempel illustrerar hur du skapar ett virtuellt nätverk med hjälp av Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b25ed-163">The following example shows how to create a virtual network by using Resource Manager.</span></span> <span data-ttu-id="b25ed-164">Det här exemplet skapar ett virtuellt nätverk för Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="b25ed-164">This example creates a VNET for the Application Gateway.</span></span> <span data-ttu-id="b25ed-165">Programgateway kräver sin egen undernät därför undernät som skapats för Programgateway är mindre än VNET-adressutrymmet.</span><span class="sxs-lookup"><span data-stu-id="b25ed-165">Application Gateway requires its own subnet, for this reason the subnet created for the Application Gateway is smaller than the VNET address space.</span></span> <span data-ttu-id="b25ed-166">Detta ger andra resurser, inklusive men inte begränsat till webbservrar som ska konfigureras i samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="b25ed-166">This allows for other resources, including but not limited to web servers to be configured in the same VNET.</span></span>

### <a name="step-1"></a><span data-ttu-id="b25ed-167">Steg 1</span><span class="sxs-lookup"><span data-stu-id="b25ed-167">Step 1</span></span>

<span data-ttu-id="b25ed-168">Tilldela adressintervallet 10.0.0.0/24 till undernätsvariabeln som ska användas för att skapa ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="b25ed-168">Assign the address range 10.0.0.0/24 to the subnet variable to be used to create a virtual network.</span></span>  <span data-ttu-id="b25ed-169">Detta skapar undernät konfigurationsobjektet för Programgateway som används i nästa exempel.</span><span class="sxs-lookup"><span data-stu-id="b25ed-169">This creates the subnet configuration object for the Application Gateway, which is used in the next example.</span></span>

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a><span data-ttu-id="b25ed-170">Steg 2</span><span class="sxs-lookup"><span data-stu-id="b25ed-170">Step 2</span></span>

<span data-ttu-id="b25ed-171">Skapa ett virtuellt nätverk med namnet **appgwvnet** i resursgruppen **appgw-rg** för regionen USA, västra med prefixet 10.0.0.0/16 och undernätet 10.0.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="b25ed-171">Create a virtual network named **appgwvnet** in resource group **appgw-rg** for the West US region using the prefix 10.0.0.0/16 with subnet 10.0.0.0/24.</span></span> <span data-ttu-id="b25ed-172">Nu är du klar med konfigurationen av virtuellt nätverk med ett enda undernät för Programgateway finns.</span><span class="sxs-lookup"><span data-stu-id="b25ed-172">This completes the configuration of the VNET with a single subnet for the Application Gateway to reside.</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a><span data-ttu-id="b25ed-173">Steg 3</span><span class="sxs-lookup"><span data-stu-id="b25ed-173">Step 3</span></span>

<span data-ttu-id="b25ed-174">Tilldela variabeln undernät i nästa steg, detta skickas till den `New-AzureRMApplicationGateway` cmdlet i ett kommande steg.</span><span class="sxs-lookup"><span data-stu-id="b25ed-174">Assign the subnet variable for the next steps, this is passed to the `New-AzureRMApplicationGateway` cmdlet in a future step.</span></span>

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a><span data-ttu-id="b25ed-175">Skapa en offentlig IP-adress för frontend-konfigurationen</span><span class="sxs-lookup"><span data-stu-id="b25ed-175">Create a public IP address for the front-end configuration</span></span>

<span data-ttu-id="b25ed-176">Skapa en offentlig IP-resurs, **publicIP01**, i resursgruppen **appgw-rg** för regionen USA, västra.</span><span class="sxs-lookup"><span data-stu-id="b25ed-176">Create a public IP resource **publicIP01** in resource group **appgw-rg** for the West US region.</span></span> <span data-ttu-id="b25ed-177">Application Gateway kan använda en offentlig IP-adress, en intern IP-adress eller båda för att ta emot förfrågningar för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="b25ed-177">Application Gateway can use a public IP address, internal IP address or both to receive requests for load balancing.</span></span>  <span data-ttu-id="b25ed-178">I det här exemplet används endast en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="b25ed-178">This example only uses a public IP address.</span></span> <span data-ttu-id="b25ed-179">I följande exempel konfigureras inget DNS-namn när den offentliga IP-adressen skapas.</span><span class="sxs-lookup"><span data-stu-id="b25ed-179">In the following example, no DNS name is configured for creating the Public IP address.</span></span>  <span data-ttu-id="b25ed-180">Application Gateway stöder inte anpassade DNS-namn med offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="b25ed-180">Application Gateway does not support custom DNS names on public IP addresses.</span></span>  <span data-ttu-id="b25ed-181">Om ett anpassat namn krävs för den offentliga slutpunkten bör en CNAME-post skapas som pekar på det automatiskt genererade DNS-namnet för den offentliga IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="b25ed-181">If a custom name is required for the public endpoint, a CNAME record should be created to point to the automatically generated DNS name for the public IP address.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

<span data-ttu-id="b25ed-182">En IP-adress tilldelas till programgatewayen när tjänsten startas.</span><span class="sxs-lookup"><span data-stu-id="b25ed-182">An IP address is assigned to the application gateway when the service starts.</span></span>

## <a name="create-application-gateway-configuration"></a><span data-ttu-id="b25ed-183">Skapa program gateway-konfiguration</span><span class="sxs-lookup"><span data-stu-id="b25ed-183">Create application gateway configuration</span></span>

<span data-ttu-id="b25ed-184">Alla konfigurationsobjekt måste konfigureras innan programgatewayen skapas.</span><span class="sxs-lookup"><span data-stu-id="b25ed-184">All configuration items must be set up before creating the application gateway.</span></span> <span data-ttu-id="b25ed-185">Följande steg skapar konfigurationsobjekten som behövs för en programgatewayresurs.</span><span class="sxs-lookup"><span data-stu-id="b25ed-185">The following steps create the configuration items that are needed for an application gateway resource.</span></span>

### <a name="step-1"></a><span data-ttu-id="b25ed-186">Steg 1</span><span class="sxs-lookup"><span data-stu-id="b25ed-186">Step 1</span></span>

<span data-ttu-id="b25ed-187">Skapa en IP-konfiguration för programgatewayen med namnet **gatewayIP01**.</span><span class="sxs-lookup"><span data-stu-id="b25ed-187">Create an application gateway IP configuration named **gatewayIP01**.</span></span> <span data-ttu-id="b25ed-188">När Application Gateway startar hämtar den en IP-adress från det konfigurerade undernätet och dirigerar nätverkstrafik till IP-adresserna i backend-IP-poolen.</span><span class="sxs-lookup"><span data-stu-id="b25ed-188">When Application Gateway starts, it picks up an IP address from the subnet configured and route network traffic to the IP addresses in the back-end IP pool.</span></span> <span data-ttu-id="b25ed-189">Tänk på att varje instans använder en IP-adress.</span><span class="sxs-lookup"><span data-stu-id="b25ed-189">Keep in mind that each instance takes one IP address.</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a><span data-ttu-id="b25ed-190">Steg 2</span><span class="sxs-lookup"><span data-stu-id="b25ed-190">Step 2</span></span>

<span data-ttu-id="b25ed-191">Konfigurera backend-IP-adresspool med namnet **pool01** och **pool2** med IP-adresser för **pool1** och **pool2**.</span><span class="sxs-lookup"><span data-stu-id="b25ed-191">Configure the back-end IP address pool named **pool01** and **pool2** with IP addresses for **pool1** and **pool2**.</span></span> <span data-ttu-id="b25ed-192">Dessa IP-adresser är IP-adresserna för de resurser som är värdar för webbprogrammet som ska skyddas av programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="b25ed-192">These IP addresses are the IP addresses of the resources that are hosting the web application to be protected by the application gateway.</span></span> <span data-ttu-id="b25ed-193">Hälsotillståndet för dessa medlemmar i serverdelspoolen verifieras genom grundläggande eller anpassade avsökningar.</span><span class="sxs-lookup"><span data-stu-id="b25ed-193">These backend pool members are all validated to be healthy by probes whether they are basic probes or custom probes.</span></span>  <span data-ttu-id="b25ed-194">Sedan dirigeras trafiken till dem när begäranden kommer till programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="b25ed-194">Traffic is then routed to them when requests come into the application gateway.</span></span> <span data-ttu-id="b25ed-195">Serverdelspooler kan användas av flera regler i programgatewayen, vilket innebär att en serverdelspool kan användas för flera webbprogram som finns på samma värd.</span><span class="sxs-lookup"><span data-stu-id="b25ed-195">Backend pools can be used by multiple rules within the application gateway, which means one backend pool could be used for multiple web applications that reside on the same host.</span></span>

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 134.170.186.47, 134.170.189.222, 134.170.186.51
```

<span data-ttu-id="b25ed-196">I det här exemplet finns det två serverdelspooler för dirigering av nätverkstrafik baserat på URL-sökvägen.</span><span class="sxs-lookup"><span data-stu-id="b25ed-196">In this example, there are two back-end pools to route network traffic based on the URL path.</span></span> <span data-ttu-id="b25ed-197">En pool tar emot trafik från URL-sökvägen ”/video” och en annan pool tar emot trafik från sökvägen ”/image”.</span><span class="sxs-lookup"><span data-stu-id="b25ed-197">One pool receives traffic from URL path "/video" and other pool receive traffic from path "/image".</span></span> <span data-ttu-id="b25ed-198">Ersätt IP-adresserna med IP-adresslutpunkterna för dina egna program.</span><span class="sxs-lookup"><span data-stu-id="b25ed-198">Replace the preceding IP addresses to add your own application IP address endpoints.</span></span> 

### <a name="step-3"></a><span data-ttu-id="b25ed-199">Steg 3</span><span class="sxs-lookup"><span data-stu-id="b25ed-199">Step 3</span></span>

<span data-ttu-id="b25ed-200">Konfigurera gateway programinställning **poolsetting01** och **poolsetting02** för nätverkstrafik Utjämning av nätverksbelastning i backend-poolen.</span><span class="sxs-lookup"><span data-stu-id="b25ed-200">Configure application gateway setting **poolsetting01** and **poolsetting02** for the load-balanced network traffic in the back-end pool.</span></span> <span data-ttu-id="b25ed-201">I det här exemplet kan du konfigurera inställningarna för olika backend-pool för backend-pooler.</span><span class="sxs-lookup"><span data-stu-id="b25ed-201">In this example, you configure different back-end pool settings for the back-end pools.</span></span> <span data-ttu-id="b25ed-202">Varje serverdelspool kan ha sin egen serverdelspoolinställning.</span><span class="sxs-lookup"><span data-stu-id="b25ed-202">Each back-end pool can have its own back-end pool setting.</span></span>  <span data-ttu-id="b25ed-203">Serverdelens HTTP-inställningar används av regler för att dirigera trafik till rätt medlemmar i serverdelspoolen.</span><span class="sxs-lookup"><span data-stu-id="b25ed-203">Backend HTTP settings are used by rules to route traffic to the correct backend pool members.</span></span> <span data-ttu-id="b25ed-204">Anger protokoll och port som används när du skickar trafik till backend-poolmedlemmar.</span><span class="sxs-lookup"><span data-stu-id="b25ed-204">This determines the protocol and port that is used when sending traffic to the backend pool members.</span></span> <span data-ttu-id="b25ed-205">Cookie-baserade sessioner bestäms också av serverdelens HTTP-inställningar.</span><span class="sxs-lookup"><span data-stu-id="b25ed-205">Cookie-based sessions are also determined by the backend HTTP settings.</span></span>  <span data-ttu-id="b25ed-206">Om cookie-baserad sessionstillhörighet är aktiverat skickas trafiken till samma serverdel som tidigare begäranden för varje paket.</span><span class="sxs-lookup"><span data-stu-id="b25ed-206">If enabled, cookie-based session affinity sends traffic to the same backend as previous requests for each packet.</span></span>

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a><span data-ttu-id="b25ed-207">Steg 4</span><span class="sxs-lookup"><span data-stu-id="b25ed-207">Step 4</span></span>

<span data-ttu-id="b25ed-208">Konfigurera klientdelens IP med den offentliga IP-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="b25ed-208">Configure the front-end IP with public IP endpoint.</span></span> <span data-ttu-id="b25ed-209">Konfigurationsobjektet för klientdelens IP används av en lyssnare för att relatera den utåtgående IP-adressen med lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="b25ed-209">The front-end IP configuration object is used by a listener to relate the outward facing IP address with the listener.</span></span>

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a><span data-ttu-id="b25ed-210">Steg 5</span><span class="sxs-lookup"><span data-stu-id="b25ed-210">Step 5</span></span>

<span data-ttu-id="b25ed-211">Konfigurera klientdelsporten för en programgateway.</span><span class="sxs-lookup"><span data-stu-id="b25ed-211">Configure the front-end port for an application gateway.</span></span> <span data-ttu-id="b25ed-212">Konfigurationsobjektet för klientdelsporten används av en lyssnare för att definiera vilken port som Application Gateway lyssnar efter trafik på.</span><span class="sxs-lookup"><span data-stu-id="b25ed-212">The front-end port configuration object is used by a listener to define what port the Application Gateway listens for traffic on the listener.</span></span>

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 80
```

### <a name="step-6"></a><span data-ttu-id="b25ed-213">Steg 6</span><span class="sxs-lookup"><span data-stu-id="b25ed-213">Step 6</span></span>

<span data-ttu-id="b25ed-214">Konfigurera lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="b25ed-214">Configure the listener.</span></span> <span data-ttu-id="b25ed-215">Det här steget konfigurerar lyssnaren för den offentliga IP-adressen och porten som används för att ta emot inkommande nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="b25ed-215">This step configures the listener for the public IP address and port used to receive incoming network traffic.</span></span> <span data-ttu-id="b25ed-216">I följande exempel tar tidigare konfigurerade frontend IP-konfigurationen och frontend portkonfiguration ett protokoll (http eller https) och konfigurerar lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="b25ed-216">The following example takes the previously configured front-end IP configuration,  front-end port configuration, and a protocol (http or https) and configures the listener.</span></span> <span data-ttu-id="b25ed-217">I det här exemplet lyssnar lyssnaren efter HTTP-trafik på port 80 på den offentliga IP-adressen som skapades tidigare.</span><span class="sxs-lookup"><span data-stu-id="b25ed-217">In this example, the listener listens to HTTP traffic on port 80 on the public IP address that was created earlier.</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Http -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01
```

### <a name="step-7"></a><span data-ttu-id="b25ed-218">Steg 7</span><span class="sxs-lookup"><span data-stu-id="b25ed-218">Step 7</span></span>

<span data-ttu-id="b25ed-219">Konfigurera URL: en regel sökvägar för backend-pooler.</span><span class="sxs-lookup"><span data-stu-id="b25ed-219">Configure URL rule paths for the back-end pools.</span></span> <span data-ttu-id="b25ed-220">Det här steget konfigurerar du den relativa sökvägen som används av Programgateway och definierar mappningen mellan en URL-sökväg och backend-poolen som har tilldelats för den inkommande trafiken.</span><span class="sxs-lookup"><span data-stu-id="b25ed-220">This step configures the relative path used by application gateway and defines the mapping between the URL path and the back-end pool that is assigned to handle the incoming traffic.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b25ed-221">Varje sökväg måste börja med / och endast en ”\*” är tillåtna, finns i slutet.</span><span class="sxs-lookup"><span data-stu-id="b25ed-221">Each path must start with / and the only place a "\*" is allowed, is at the end.</span></span> <span data-ttu-id="b25ed-222">Giltiga exempel är /xyz, /xyz* eller/xyz / *.</span><span class="sxs-lookup"><span data-stu-id="b25ed-222">Valid examples are /xyz, /xyz* or /xyz/*.</span></span> <span data-ttu-id="b25ed-223">Strängen som skickas till sökvägen matcher innehåller inte någon text efter först ””? eller ”#”, och dessa tecken är inte tillåtna.</span><span class="sxs-lookup"><span data-stu-id="b25ed-223">The string fed to the path matcher does not include any text after the first "?" or "#", and those characters are not allowed.</span></span> 

<span data-ttu-id="b25ed-224">I följande exempel skapas två regler: en för ”/ bild /” sökväg routning trafik till backend-”pool1” och en annan för ”/ video /” sökväg dirigera trafiken till backend-”pool2”.</span><span class="sxs-lookup"><span data-stu-id="b25ed-224">The following example creates two rules: one for "/image/" path routing traffic to back-end "pool1" and another one for "/video/" path routing traffic to back-end "pool2."</span></span> <span data-ttu-id="b25ed-225">De här reglerna Kontrollera att trafik för varje uppsättning URL-adresser dirigeras till serverdelen.</span><span class="sxs-lookup"><span data-stu-id="b25ed-225">These rules ensure that traffic for each set of urls is routed to the backend.</span></span> <span data-ttu-id="b25ed-226">Till exempel http://contoso.com/image/figure1.jpg går till pool1 och http://contoso.com/video/example.mp4 går till pool2.</span><span class="sxs-lookup"><span data-stu-id="b25ed-226">For example, http://contoso.com/image/figure1.jpg goes to pool1 and http://contoso.com/video/example.mp4 goes to pool2.</span></span>

```powershell
$imagePathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule1" -Paths "/image/*" -BackendAddressPool $pool1 -BackendHttpSettings $poolSetting01

$videoPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule2" -Paths "/video/*" -BackendAddressPool $pool2 -BackendHttpSettings $poolSetting02
```

<span data-ttu-id="b25ed-227">Om sökvägen inte matchar någon av de fördefinierade sökvägsreglerna, konfigurerar regelkonfigurationen sökväg kartan även en standard backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="b25ed-227">If the path doesn't match any of the pre-defined path rules, the rule path map configuration also configures a default back-end address pool.</span></span> <span data-ttu-id="b25ed-228">Till exempel går http://contoso.com/shoppingcart/test.html till pool1 som har definierats som standard för omatchade trafik.</span><span class="sxs-lookup"><span data-stu-id="b25ed-228">For example, http://contoso.com/shoppingcart/test.html goes to pool1 as it is defined as the default pool for unmatched traffic.</span></span>

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $videoPathRule, $imagePathRule -DefaultBackendAddressPool $pool1 -DefaultBackendHttpSettings $poolSetting02
```

### <a name="step-8"></a><span data-ttu-id="b25ed-229">Steg 8</span><span class="sxs-lookup"><span data-stu-id="b25ed-229">Step 8</span></span>

<span data-ttu-id="b25ed-230">Skapa en regel inställning.</span><span class="sxs-lookup"><span data-stu-id="b25ed-230">Create a rule setting.</span></span> <span data-ttu-id="b25ed-231">Det här steget konfigurerar Programgateway för att använda URL-sökväg-baserade routning.</span><span class="sxs-lookup"><span data-stu-id="b25ed-231">This step configures the application gateway to use URL path-based routing.</span></span> <span data-ttu-id="b25ed-232">Den `$urlPathMap` variabel som anges i tidigare steg nu användas för att skapa regeln baserat på sökvägen.</span><span class="sxs-lookup"><span data-stu-id="b25ed-232">The `$urlPathMap` variable defined in the earlier step is now used to create the path-based rule.</span></span> <span data-ttu-id="b25ed-233">Vi associera regeln med en lyssnare och URL: en sökvägsmappning skapade tidigare i det här steget.</span><span class="sxs-lookup"><span data-stu-id="b25ed-233">In this step, we associate the rule with a listener and the url path mapping created earlier.</span></span>

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-9"></a><span data-ttu-id="b25ed-234">Steg 9</span><span class="sxs-lookup"><span data-stu-id="b25ed-234">Step 9</span></span>

<span data-ttu-id="b25ed-235">Konfigurera antalet instanser av och storleken på programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="b25ed-235">Configure the number of instances and size for the application gateway.</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Small" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a><span data-ttu-id="b25ed-236">Skapa Programgateway</span><span class="sxs-lookup"><span data-stu-id="b25ed-236">Create Application Gateway</span></span>

<span data-ttu-id="b25ed-237">Skapa en Programgateway med alla konfigurationsobjekt från föregående steg.</span><span class="sxs-lookup"><span data-stu-id="b25ed-237">Create an application gateway with all configuration objects from the preceding steps.</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku
```

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="b25ed-238">Hämta DNS-namn för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="b25ed-238">Get application gateway DNS name</span></span>

<span data-ttu-id="b25ed-239">När du har skapat gatewayen, är nästa steg att konfigurera klientprogrammet för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="b25ed-239">Once the gateway is created, the next step is to configure the front end for communication.</span></span> <span data-ttu-id="b25ed-240">När du använder en offentlig IP-adress krävs ett dynamiskt tilldelat DNS-namn som inte är användarvänligt.</span><span class="sxs-lookup"><span data-stu-id="b25ed-240">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="b25ed-241">För att säkerställa att slutanvändare kan nå programgatewayen kan en CNAME-post användas för att peka på den offentliga slutpunkten för programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="b25ed-241">To ensure end users can hit the application gateway, a CNAME record can be used to point to the public endpoint of the application gateway.</span></span> <span data-ttu-id="b25ed-242">[Konfigurera ett eget domännamn i Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b25ed-242">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="b25ed-243">Hämta information om programgatewayen och dess tillhörande IP DNS-namn med PublicIPAddress-element som bifogas programgatewayen om du vill konfigurera klientdelens IP-CNAME-post.</span><span class="sxs-lookup"><span data-stu-id="b25ed-243">To configure the frontend IP CNAME record, retrieve details of the application gateway and its associated IP/DNS name using the PublicIPAddress element attached to the application gateway.</span></span> <span data-ttu-id="b25ed-244">Den Programgateway DNS-namn ska användas för att skapa en CNAME-post.</span><span class="sxs-lookup"><span data-stu-id="b25ed-244">The application gateway's DNS name should be used to create a CNAME record.</span></span> <span data-ttu-id="b25ed-245">Användning av A-poster rekommenderas inte eftersom VIP kan ändras vid omstart av programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="b25ed-245">The use of A-records is not recommended since the VIP may change on restart of application gateway.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="b25ed-246">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b25ed-246">Next steps</span></span>

<span data-ttu-id="b25ed-247">Om du vill veta Secure Sockets Layer (SSL)-avlastning, se [konfigurera en Programgateway för SSL-avlastning](application-gateway-ssl-arm.md).</span><span class="sxs-lookup"><span data-stu-id="b25ed-247">If you want to learn Secure Sockets Layer (SSL) offload, see [Configure an application gateway for SSL offload](application-gateway-ssl-arm.md).</span></span>

