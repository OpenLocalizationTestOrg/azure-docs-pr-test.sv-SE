---
title: "aaaCreate en Programgateway använder URL routning regler | Microsoft Docs"
description: "Den här sidan finns instruktioner toocreate, konfigurera en gateway för Azure-program som använder routningsregler för URL"
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
ms.openlocfilehash: 54fcccc39e48a933576968ce3d8160518c0d0b7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-using-path-based-routing"></a><span data-ttu-id="49db2-103">Skapa en Programgateway använder sökväg-baserade Routning</span><span class="sxs-lookup"><span data-stu-id="49db2-103">Create an application gateway using Path-based routing</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="49db2-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="49db2-104">Azure portal</span></span>](application-gateway-create-url-route-portal.md)
> * [<span data-ttu-id="49db2-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="49db2-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-url-route-arm-ps.md)
> * [<span data-ttu-id="49db2-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="49db2-106">Azure CLI 2.0</span></span>](application-gateway-create-url-route-cli.md)

<span data-ttu-id="49db2-107">URL-sökväg-baserade routning kan du tooassociate rutter baserat på hello URL-sökvägen för en Http-begäran.</span><span class="sxs-lookup"><span data-stu-id="49db2-107">URL Path-based routing enables you tooassociate routes based on hello URL path of an Http request.</span></span> <span data-ttu-id="49db2-108">Den kontrollerar om det finns en väg tooa backend-adresspool för hello-URL som visas i hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="49db2-108">It checks if there is a route tooa back-end pool configured for hello URL presented in hello Application Gateway.</span></span> <span data-ttu-id="49db2-109">Sedan skickar hello nätverket trafik toohello definierats backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="49db2-109">It then sends hello network traffic toohello defined back-end pool.</span></span> <span data-ttu-id="49db2-110">Ett vanligt användningsområde för URL-baserade routning är tooload begäranden för olika typer av innehåll toodifferent backend-serverpooler.</span><span class="sxs-lookup"><span data-stu-id="49db2-110">A common use for URL-based routing is tooload balance requests for different content types toodifferent back-end server pools.</span></span>

<span data-ttu-id="49db2-111">URL-baserade routning introducerar en ny regel typen tooapplication gateway.</span><span class="sxs-lookup"><span data-stu-id="49db2-111">URL-based routing introduces a new rule type tooapplication gateway.</span></span> <span data-ttu-id="49db2-112">Programgateway har två regeltyper: basic och PathBasedRouting.</span><span class="sxs-lookup"><span data-stu-id="49db2-112">Application gateway has two rule types: basic and PathBasedRouting.</span></span> <span data-ttu-id="49db2-113">Grundläggande regeltyp tillhandahåller resursallokering tjänst för hello backend-pooler när PathBasedRouting dessutom tooround robin distribution kan också beaktas sökvägar för hello URL-begäran vid val hello serverdelspool.</span><span class="sxs-lookup"><span data-stu-id="49db2-113">Basic rule type provides round-robin service for hello back-end pools while PathBasedRouting in addition tooround robin distribution, also takes path pattern of hello request URL into account while choosing hello backend pool.</span></span>

## <a name="scenario"></a><span data-ttu-id="49db2-114">Scenario</span><span class="sxs-lookup"><span data-stu-id="49db2-114">Scenario</span></span>

<span data-ttu-id="49db2-115">I följande exempel hello, Programgateway betjäna trafik för contoso.com med två backend-server-adresspooler: video-serverpoolen och image-serverpoolen.</span><span class="sxs-lookup"><span data-stu-id="49db2-115">In hello following example, Application Gateway is serving traffic for contoso.com with two back-end server pools: video server pool and image server pool.</span></span>

<span data-ttu-id="49db2-116">Begäran om http://contoso.com/image * dirigeras tooimage serverpoolen (pool1) och http://contoso.com/video * dirigeras toovideo serverpoolen (pool2).</span><span class="sxs-lookup"><span data-stu-id="49db2-116">Requests for http://contoso.com/image* are routed tooimage server pool (pool1), and http://contoso.com/video* are routed toovideo server pool (pool2).</span></span> <span data-ttu-id="49db2-117">Om inget av hello sökväg mönster matchar väljs en standard serverpool (pool1).</span><span class="sxs-lookup"><span data-stu-id="49db2-117">if none of hello path patterns match, a default server pool (pool1) is selected.</span></span>

![URL-väg](./media/application-gateway-create-url-route-arm-ps/figure1.png)

## <a name="before-you-begin"></a><span data-ttu-id="49db2-119">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="49db2-119">Before you begin</span></span>

1. <span data-ttu-id="49db2-120">Installera hello senaste versionen av hello Azure PowerShell-cmdlets med hello installationsprogram för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="49db2-120">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="49db2-121">Du kan hämta och installera hello senaste versionen från hello **Windows PowerShell** avsnitt i hello [Nedladdningssida](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="49db2-121">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="49db2-122">Du skapar ett virtuellt nätverk och undernät för Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="49db2-122">You create a virtual network and subnet for Application Gateway.</span></span> <span data-ttu-id="49db2-123">Se till att inga virtuella datorer eller molndistributioner använder hello undernät.</span><span class="sxs-lookup"><span data-stu-id="49db2-123">Make sure that no virtual machines or cloud deployments are using hello subnet.</span></span> <span data-ttu-id="49db2-124">hello programmet gateway måste finnas ensamt i ett undernät för virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="49db2-124">hello application gateway must be by itself in a virtual network subnet.</span></span>
3. <span data-ttu-id="49db2-125">hello servrar lagts toohello backend-adresspool toouse hello Programgateway måste finnas eller har skapat sina slutpunkter i hello virtuellt nätverk eller med en offentlig IP-adress/VIP tilldelad.</span><span class="sxs-lookup"><span data-stu-id="49db2-125">hello servers added toohello back-end pool toouse hello application gateway must exist or have their endpoints created either in hello virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-toocreate-an-application-gateway"></a><span data-ttu-id="49db2-126">Vad är obligatoriska toocreate en Programgateway?</span><span class="sxs-lookup"><span data-stu-id="49db2-126">What is required toocreate an application gateway?</span></span>

* <span data-ttu-id="49db2-127">**Backend-serverpoolen:** hello lista över IP-adresser för hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="49db2-127">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="49db2-128">hello IP-adresser som anges antingen ska tillhöra toohello undernät för virtuellt nätverk eller ska vara en offentlig IP-adress/VIP.</span><span class="sxs-lookup"><span data-stu-id="49db2-128">hello IP addresses listed should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="49db2-129">**Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet.</span><span class="sxs-lookup"><span data-stu-id="49db2-129">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="49db2-130">De här inställningarna är bundet tooa poolen och tillämpade tooall servrar inom hello poolen.</span><span class="sxs-lookup"><span data-stu-id="49db2-130">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="49db2-131">**Frontend-port:** den här porten är hello offentliga som öppnas på hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="49db2-131">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="49db2-132">Trafik träffar den här porten och sedan hämtar omdirigeras tooone hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="49db2-132">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="49db2-133">**Lyssnare:** hello-lyssnare har en frontend-port, ett protokoll (Http eller Https dessa värden är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning).</span><span class="sxs-lookup"><span data-stu-id="49db2-133">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="49db2-134">**Regel:** hello regeln Binder hello lyssnare, hello backend-serverpoolen och definierar vilken backend-server pool hello trafik ska vara riktad toowhen den når en viss lyssnare.</span><span class="sxs-lookup"><span data-stu-id="49db2-134">**Rule:** hello rule binds hello listener, hello back-end server pool and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="49db2-135">Skapa en programgateway</span><span class="sxs-lookup"><span data-stu-id="49db2-135">Create an application gateway</span></span>

<span data-ttu-id="49db2-136">hello skillnaden mellan att använda den klassiska Azure och Azure Resource Manager är hello order som du kan skapa hello Programgateway och hello-objekt som behöver toobe konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="49db2-136">hello difference between using Azure Classic and Azure Resource Manager is hello order in which you create hello application gateway and hello items that need toobe configured.</span></span>

<span data-ttu-id="49db2-137">Med Resource Manager alla poster i en Programgateway konfigureras separat och sedan sätta ihop toocreate hello programresursen gateway.</span><span class="sxs-lookup"><span data-stu-id="49db2-137">With Resource Manager, all items that make an application gateway are configured individually and then put together toocreate hello application gateway resource.</span></span>

<span data-ttu-id="49db2-138">Här följer hello steg som är nödvändiga toocreate en Programgateway:</span><span class="sxs-lookup"><span data-stu-id="49db2-138">Here are hello steps that are needed toocreate an application gateway:</span></span>

1. <span data-ttu-id="49db2-139">Skapa en resursgrupp för Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="49db2-139">Create a resource group for Resource Manager.</span></span>
2. <span data-ttu-id="49db2-140">Skapa ett virtuellt nätverk och undernät offentlig IP för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="49db2-140">Create a virtual network, subnet, and public IP for hello application gateway.</span></span>
3. <span data-ttu-id="49db2-141">Skapa ett konfigurationsobjekt för programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="49db2-141">Create an application gateway configuration object.</span></span>
4. <span data-ttu-id="49db2-142">Skapa en resurs för en programgateway.</span><span class="sxs-lookup"><span data-stu-id="49db2-142">Create an application gateway resource.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="49db2-143">Skapa en resursgrupp för Resource Manager</span><span class="sxs-lookup"><span data-stu-id="49db2-143">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="49db2-144">Kontrollera att du använder hello senaste versionen av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="49db2-144">Make sure that you are using hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="49db2-145">Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="49db2-145">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="49db2-146">Steg 1</span><span class="sxs-lookup"><span data-stu-id="49db2-146">Step 1</span></span>

<span data-ttu-id="49db2-147">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="49db2-147">Log in tooAzure</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="49db2-148">Du kan ange tooauthenticate med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="49db2-148">You are prompted tooauthenticate with your credentials.</span></span><BR>

### <a name="step-2"></a><span data-ttu-id="49db2-149">Steg 2</span><span class="sxs-lookup"><span data-stu-id="49db2-149">Step 2</span></span>

<span data-ttu-id="49db2-150">Kontrollera hello prenumerationer för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="49db2-150">Check hello subscriptions for hello account.</span></span>

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a><span data-ttu-id="49db2-151">Steg 3</span><span class="sxs-lookup"><span data-stu-id="49db2-151">Step 3</span></span>

<span data-ttu-id="49db2-152">Välj vilka av dina Azure-prenumerationer toouse.</span><span class="sxs-lookup"><span data-stu-id="49db2-152">Choose which of your Azure subscriptions toouse.</span></span> <BR>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a><span data-ttu-id="49db2-153">Steg 4</span><span class="sxs-lookup"><span data-stu-id="49db2-153">Step 4</span></span>

<span data-ttu-id="49db2-154">Skapa en resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp).</span><span class="sxs-lookup"><span data-stu-id="49db2-154">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US"
```

<span data-ttu-id="49db2-155">Alternativt kan du också skapa taggar för en resursgrupp för Programgateway:</span><span class="sxs-lookup"><span data-stu-id="49db2-155">Alternatively you can also create tags for a resource group for application gateway:</span></span>

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway URL routing"} 
```

<span data-ttu-id="49db2-156">Azure Resource Manager kräver att alla resursgrupper anger en plats.</span><span class="sxs-lookup"><span data-stu-id="49db2-156">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="49db2-157">Den här resursgruppen används som hello standardplatsen för resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="49db2-157">This resource group is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="49db2-158">Se till att alla kommandon toocreate ett program gateway används hello samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="49db2-158">Make sure that all commands toocreate an application gateway use hello same resource group.</span></span>

<span data-ttu-id="49db2-159">I hello-exemplet ovan kan skapat vi en resursgrupp med namnet ”appgw RG” och plats ”West US”.</span><span class="sxs-lookup"><span data-stu-id="49db2-159">In hello example above, we created a resource group called "appgw-RG" and location "West US".</span></span>

> [!NOTE]
> <span data-ttu-id="49db2-160">Om du behöver tooconfigure en anpassad avsökningsåtgärd för din Programgateway finns [skapa en Programgateway med anpassade avsökningar med hjälp av PowerShell](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="49db2-160">If you need tooconfigure a custom probe for your application gateway, see [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md).</span></span> <span data-ttu-id="49db2-161">Mer information finns i [Anpassade avsökningar och hälsoövervakning](application-gateway-probe-overview.md).</span><span class="sxs-lookup"><span data-stu-id="49db2-161">Check out [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>
> 
> 

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a><span data-ttu-id="49db2-162">Skapa ett virtuellt nätverk och ett undernät för hello Programgateway</span><span class="sxs-lookup"><span data-stu-id="49db2-162">Create a virtual network and a subnet for hello application gateway</span></span>

<span data-ttu-id="49db2-163">följande exempel visar hur hello toocreate ett virtuellt nätverk med hjälp av hanteraren för filserverresurser.</span><span class="sxs-lookup"><span data-stu-id="49db2-163">hello following example shows how toocreate a virtual network by using Resource Manager.</span></span> <span data-ttu-id="49db2-164">Det här exemplet skapar ett virtuellt nätverk för hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="49db2-164">This example creates a VNET for hello Application Gateway.</span></span> <span data-ttu-id="49db2-165">Programgateway kräver sin egen undernät därför hello undernät som skapats för hello Programgateway är mindre än hello VNET-adressutrymmet.</span><span class="sxs-lookup"><span data-stu-id="49db2-165">Application Gateway requires its own subnet, for this reason hello subnet created for hello Application Gateway is smaller than hello VNET address space.</span></span> <span data-ttu-id="49db2-166">Detta ger andra resurser, inklusive men inte begränsat tooweb servrar toobe konfigurerad i hello samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="49db2-166">This allows for other resources, including but not limited tooweb servers toobe configured in hello same VNET.</span></span>

### <a name="step-1"></a><span data-ttu-id="49db2-167">Steg 1</span><span class="sxs-lookup"><span data-stu-id="49db2-167">Step 1</span></span>

<span data-ttu-id="49db2-168">Tilldela hello adressintervallet 10.0.0.0/24 toohello undernät variabeln toobe används toocreate ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="49db2-168">Assign hello address range 10.0.0.0/24 toohello subnet variable toobe used toocreate a virtual network.</span></span>  <span data-ttu-id="49db2-169">Detta skapar hello configuration undernätsobjekt för hello Programgateway som används i hello nästa exempel.</span><span class="sxs-lookup"><span data-stu-id="49db2-169">This creates hello subnet configuration object for hello Application Gateway, which is used in hello next example.</span></span>

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a><span data-ttu-id="49db2-170">Steg 2</span><span class="sxs-lookup"><span data-stu-id="49db2-170">Step 2</span></span>

<span data-ttu-id="49db2-171">Skapa ett virtuellt nätverk med namnet **appgwvnet** i resursgruppen **appgw rg** för hello västra USA region med undernätet 10.0.0.0/24 hello prefixet 10.0.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="49db2-171">Create a virtual network named **appgwvnet** in resource group **appgw-rg** for hello West US region using hello prefix 10.0.0.0/16 with subnet 10.0.0.0/24.</span></span> <span data-ttu-id="49db2-172">Detta avslutar hello konfigurationen av hello virtuellt nätverk med ett enda undernät för hello Programgateway tooreside.</span><span class="sxs-lookup"><span data-stu-id="49db2-172">This completes hello configuration of hello VNET with a single subnet for hello Application Gateway tooreside.</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a><span data-ttu-id="49db2-173">Steg 3</span><span class="sxs-lookup"><span data-stu-id="49db2-173">Step 3</span></span>

<span data-ttu-id="49db2-174">Tilldela hello undernät variabel för hello nästa steg, detta skickas toohello `New-AzureRMApplicationGateway` cmdlet i ett kommande steg.</span><span class="sxs-lookup"><span data-stu-id="49db2-174">Assign hello subnet variable for hello next steps, this is passed toohello `New-AzureRMApplicationGateway` cmdlet in a future step.</span></span>

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a><span data-ttu-id="49db2-175">Skapa en offentlig IP-adress för hello frontend-konfiguration</span><span class="sxs-lookup"><span data-stu-id="49db2-175">Create a public IP address for hello front-end configuration</span></span>

<span data-ttu-id="49db2-176">Skapa en offentlig IP-resurs **publicIP01** i resursgruppen **appgw rg** för hello västra USA region.</span><span class="sxs-lookup"><span data-stu-id="49db2-176">Create a public IP resource **publicIP01** in resource group **appgw-rg** for hello West US region.</span></span> <span data-ttu-id="49db2-177">Programgateway kan använda en offentlig IP-adress, interna IP-adress eller båda tooreceive begäranden för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="49db2-177">Application Gateway can use a public IP address, internal IP address or both tooreceive requests for load balancing.</span></span>  <span data-ttu-id="49db2-178">I det här exemplet används endast en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="49db2-178">This example only uses a public IP address.</span></span> <span data-ttu-id="49db2-179">I följande exempel hello, konfigureras inget DNS-namn för att skapa hello offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="49db2-179">In hello following example, no DNS name is configured for creating hello Public IP address.</span></span>  <span data-ttu-id="49db2-180">Application Gateway stöder inte anpassade DNS-namn med offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="49db2-180">Application Gateway does not support custom DNS names on public IP addresses.</span></span>  <span data-ttu-id="49db2-181">Om ett anpassat namn krävs för hello offentlig slutpunkt, skapas en CNAME-post toopoint toohello genereras automatiskt DNS-namn för hello offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="49db2-181">If a custom name is required for hello public endpoint, a CNAME record should be created toopoint toohello automatically generated DNS name for hello public IP address.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

<span data-ttu-id="49db2-182">En IP-adress tilldelas toohello Programgateway när hello-tjänsten startas.</span><span class="sxs-lookup"><span data-stu-id="49db2-182">An IP address is assigned toohello application gateway when hello service starts.</span></span>

## <a name="create-application-gateway-configuration"></a><span data-ttu-id="49db2-183">Skapa program gateway-konfiguration</span><span class="sxs-lookup"><span data-stu-id="49db2-183">Create application gateway configuration</span></span>

<span data-ttu-id="49db2-184">Alla konfigurationsobjekt måste ställas in innan du skapar hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="49db2-184">All configuration items must be set up before creating hello application gateway.</span></span> <span data-ttu-id="49db2-185">hello med följande steg skapar hello konfigurationsobjekt som behövs för en gateway programresursen.</span><span class="sxs-lookup"><span data-stu-id="49db2-185">hello following steps create hello configuration items that are needed for an application gateway resource.</span></span>

### <a name="step-1"></a><span data-ttu-id="49db2-186">Steg 1</span><span class="sxs-lookup"><span data-stu-id="49db2-186">Step 1</span></span>

<span data-ttu-id="49db2-187">Skapa en IP-konfiguration för programgatewayen med namnet **gatewayIP01**.</span><span class="sxs-lookup"><span data-stu-id="49db2-187">Create an application gateway IP configuration named **gatewayIP01**.</span></span> <span data-ttu-id="49db2-188">När Programgateway startar hämtar en IP-adress från hello-undernät som konfigurerats och dirigera trafik toohello IP-adresser på nätverket i hello backend-IP-adresspool.</span><span class="sxs-lookup"><span data-stu-id="49db2-188">When Application Gateway starts, it picks up an IP address from hello subnet configured and route network traffic toohello IP addresses in hello back-end IP pool.</span></span> <span data-ttu-id="49db2-189">Tänk på att varje instans använder en IP-adress.</span><span class="sxs-lookup"><span data-stu-id="49db2-189">Keep in mind that each instance takes one IP address.</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a><span data-ttu-id="49db2-190">Steg 2</span><span class="sxs-lookup"><span data-stu-id="49db2-190">Step 2</span></span>

<span data-ttu-id="49db2-191">Konfigurera hello backend-IP-adresspool med namnet **pool01** och **pool2** med IP-adresser för **pool1** och **pool2**.</span><span class="sxs-lookup"><span data-stu-id="49db2-191">Configure hello back-end IP address pool named **pool01** and **pool2** with IP addresses for **pool1** and **pool2**.</span></span> <span data-ttu-id="49db2-192">Dessa IP-adresser är IP-adresser för hello hello-resurser som är värd för hello web application toobe skyddas av hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="49db2-192">These IP addresses are hello IP addresses of hello resources that are hosting hello web application toobe protected by hello application gateway.</span></span> <span data-ttu-id="49db2-193">Dessa backend poolmedlemmar alla verifierade toobe som felfri av avsökningar oavsett om de är grundläggande avsökningar eller anpassade avsökningar.</span><span class="sxs-lookup"><span data-stu-id="49db2-193">These backend pool members are all validated toobe healthy by probes whether they are basic probes or custom probes.</span></span>  <span data-ttu-id="49db2-194">Trafik dirigeras sedan toothem när begäranden som kommer till hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="49db2-194">Traffic is then routed toothem when requests come into hello application gateway.</span></span> <span data-ttu-id="49db2-195">Serverdelspooler kan användas av flera regler inom hello Programgateway, vilket innebär en serverdelspool kan användas i flera webbprogram som finns på hello samma värd.</span><span class="sxs-lookup"><span data-stu-id="49db2-195">Backend pools can be used by multiple rules within hello application gateway, which means one backend pool could be used for multiple web applications that reside on hello same host.</span></span>

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 134.170.186.47, 134.170.189.222, 134.170.186.51
```

<span data-ttu-id="49db2-196">I det här exemplet finns det två backend-pooler tooroute nätverkstrafik som baserat på hello URL-sökväg.</span><span class="sxs-lookup"><span data-stu-id="49db2-196">In this example, there are two back-end pools tooroute network traffic based on hello URL path.</span></span> <span data-ttu-id="49db2-197">En pool tar emot trafik från URL-sökvägen ”/video” och en annan pool tar emot trafik från sökvägen ”/image”.</span><span class="sxs-lookup"><span data-stu-id="49db2-197">One pool receives traffic from URL path "/video" and other pool receive traffic from path "/image".</span></span> <span data-ttu-id="49db2-198">Ersätt hello föregående IP-adresser tooadd egna programslutpunkter IP-adress.</span><span class="sxs-lookup"><span data-stu-id="49db2-198">Replace hello preceding IP addresses tooadd your own application IP address endpoints.</span></span> 

### <a name="step-3"></a><span data-ttu-id="49db2-199">Steg 3</span><span class="sxs-lookup"><span data-stu-id="49db2-199">Step 3</span></span>

<span data-ttu-id="49db2-200">Konfigurera gateway programinställning **poolsetting01** och **poolsetting02** för hello belastningsutjämnad nätverkstrafik i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="49db2-200">Configure application gateway setting **poolsetting01** and **poolsetting02** for hello load-balanced network traffic in hello back-end pool.</span></span> <span data-ttu-id="49db2-201">I det här exemplet kan du konfigurera inställningarna för olika backend-pool för hello backend-pooler.</span><span class="sxs-lookup"><span data-stu-id="49db2-201">In this example, you configure different back-end pool settings for hello back-end pools.</span></span> <span data-ttu-id="49db2-202">Varje serverdelspool kan ha sin egen serverdelspoolinställning.</span><span class="sxs-lookup"><span data-stu-id="49db2-202">Each back-end pool can have its own back-end pool setting.</span></span>  <span data-ttu-id="49db2-203">Serverdelens HTTP-inställningar som används av regler tooroute trafik toohello rätt backend poolmedlemmar.</span><span class="sxs-lookup"><span data-stu-id="49db2-203">Backend HTTP settings are used by rules tooroute traffic toohello correct backend pool members.</span></span> <span data-ttu-id="49db2-204">Detta avgör hello protokoll och port som används när en skickar trafik toohello backend poolmedlemmar.</span><span class="sxs-lookup"><span data-stu-id="49db2-204">This determines hello protocol and port that is used when sending traffic toohello backend pool members.</span></span> <span data-ttu-id="49db2-205">Cookie-baserad sessioner också bestäms av hello serverdelens HTTP-inställningar.</span><span class="sxs-lookup"><span data-stu-id="49db2-205">Cookie-based sessions are also determined by hello backend HTTP settings.</span></span>  <span data-ttu-id="49db2-206">Om aktiverad, cookie-baserad session tillhörighet skickar trafik toohello samma backend som tidigare begäranden för varje paket.</span><span class="sxs-lookup"><span data-stu-id="49db2-206">If enabled, cookie-based session affinity sends traffic toohello same backend as previous requests for each packet.</span></span>

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a><span data-ttu-id="49db2-207">Steg 4</span><span class="sxs-lookup"><span data-stu-id="49db2-207">Step 4</span></span>

<span data-ttu-id="49db2-208">Konfigurera IP-frontend-hello med offentliga IP-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="49db2-208">Configure hello front-end IP with public IP endpoint.</span></span> <span data-ttu-id="49db2-209">hello frontend IP configuration-objekt används av en lyssnare toorelate hello passiv mot IP-adress med hello-lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="49db2-209">hello front-end IP configuration object is used by a listener toorelate hello outward facing IP address with hello listener.</span></span>

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a><span data-ttu-id="49db2-210">Steg 5</span><span class="sxs-lookup"><span data-stu-id="49db2-210">Step 5</span></span>

<span data-ttu-id="49db2-211">Konfigurera hello frontend-port för en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="49db2-211">Configure hello front-end port for an application gateway.</span></span> <span data-ttu-id="49db2-212">konfigurationsobjekt för hello frontend-port används av en lyssnare toodefine vad port hello Programgateway lyssnar efter trafik på hello lyssnare.</span><span class="sxs-lookup"><span data-stu-id="49db2-212">hello front-end port configuration object is used by a listener toodefine what port hello Application Gateway listens for traffic on hello listener.</span></span>

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 80
```

### <a name="step-6"></a><span data-ttu-id="49db2-213">Steg 6</span><span class="sxs-lookup"><span data-stu-id="49db2-213">Step 6</span></span>

<span data-ttu-id="49db2-214">Konfigurera hello-lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="49db2-214">Configure hello listener.</span></span> <span data-ttu-id="49db2-215">Det här steget konfigurerar hello-lyssnare för hello offentlig IP-adress och port används tooreceive inkommande nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="49db2-215">This step configures hello listener for hello public IP address and port used tooreceive incoming network traffic.</span></span> <span data-ttu-id="49db2-216">hello följande exempel tar hello som tidigare har konfigurerat frontend IP-konfiguration, frontend portkonfiguration och ett protokoll (http eller https) och konfigurerar hello-lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="49db2-216">hello following example takes hello previously configured front-end IP configuration,  front-end port configuration, and a protocol (http or https) and configures hello listener.</span></span> <span data-ttu-id="49db2-217">I det här exemplet lyssnar hello lyssnare tooHTTP trafik på port 80 på hello offentlig IP-adress som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="49db2-217">In this example, hello listener listens tooHTTP traffic on port 80 on hello public IP address that was created earlier.</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Http -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01
```

### <a name="step-7"></a><span data-ttu-id="49db2-218">Steg 7</span><span class="sxs-lookup"><span data-stu-id="49db2-218">Step 7</span></span>

<span data-ttu-id="49db2-219">Konfigurera URL: en regel sökvägar för hello backend-pooler.</span><span class="sxs-lookup"><span data-stu-id="49db2-219">Configure URL rule paths for hello back-end pools.</span></span> <span data-ttu-id="49db2-220">Det här steget konfigurerar hello relativ sökväg används av Programgateway och definierar hello mappning mellan hello URL-sökväg och hello backend-adresspool som är tilldelad toohandle hello inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="49db2-220">This step configures hello relative path used by application gateway and defines hello mapping between hello URL path and hello back-end pool that is assigned toohandle hello incoming traffic.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="49db2-221">Varje sökväg måste börja med / och hello endast placera en ”\*” tillåts, är hello slutet.</span><span class="sxs-lookup"><span data-stu-id="49db2-221">Each path must start with / and hello only place a "\*" is allowed, is at hello end.</span></span> <span data-ttu-id="49db2-222">Giltiga exempel är /xyz, /xyz* eller/xyz / *.</span><span class="sxs-lookup"><span data-stu-id="49db2-222">Valid examples are /xyz, /xyz* or /xyz/*.</span></span> <span data-ttu-id="49db2-223">hello strängen matas toohello sökväg matcher inte innehåller någon text efter hello först ””? eller ”#”, och dessa tecken är inte tillåtna.</span><span class="sxs-lookup"><span data-stu-id="49db2-223">hello string fed toohello path matcher does not include any text after hello first "?" or "#", and those characters are not allowed.</span></span> 

<span data-ttu-id="49db2-224">hello följande exempel skapar två regler: en för ”/ bild /” sökväg routning trafik tooback slutpunkt ”pool1” och en annan för ”/ video /” sökväg routning trafik tooback slutpunkt ”pool2”.</span><span class="sxs-lookup"><span data-stu-id="49db2-224">hello following example creates two rules: one for "/image/" path routing traffic tooback-end "pool1" and another one for "/video/" path routing traffic tooback-end "pool2."</span></span> <span data-ttu-id="49db2-225">Dessa regler kan du kontrollera att trafik för varje uppsättning URL: er är routade toohello backend.</span><span class="sxs-lookup"><span data-stu-id="49db2-225">These rules ensure that traffic for each set of urls is routed toohello backend.</span></span> <span data-ttu-id="49db2-226">Till exempel http://contoso.com/image/figure1.jpg hamnar toopool1 och http://contoso.com/video/example.mp4 går toopool2.</span><span class="sxs-lookup"><span data-stu-id="49db2-226">For example, http://contoso.com/image/figure1.jpg goes toopool1 and http://contoso.com/video/example.mp4 goes toopool2.</span></span>

```powershell
$imagePathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule1" -Paths "/image/*" -BackendAddressPool $pool1 -BackendHttpSettings $poolSetting01

$videoPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule2" -Paths "/video/*" -BackendAddressPool $pool2 -BackendHttpSettings $poolSetting02
```

<span data-ttu-id="49db2-227">Om hello sökväg inte matchar någon av hello fördefinierade sökvägsregler, konfigurerar hello regelkonfigurationen sökväg kartan även en standard backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="49db2-227">If hello path doesn't match any of hello pre-defined path rules, hello rule path map configuration also configures a default back-end address pool.</span></span> <span data-ttu-id="49db2-228">Till exempel blir http://contoso.com/shoppingcart/test.html toopool1 som har definierats som hello Standardpool för omatchade trafik.</span><span class="sxs-lookup"><span data-stu-id="49db2-228">For example, http://contoso.com/shoppingcart/test.html goes toopool1 as it is defined as hello default pool for unmatched traffic.</span></span>

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $videoPathRule, $imagePathRule -DefaultBackendAddressPool $pool1 -DefaultBackendHttpSettings $poolSetting02
```

### <a name="step-8"></a><span data-ttu-id="49db2-229">Steg 8</span><span class="sxs-lookup"><span data-stu-id="49db2-229">Step 8</span></span>

<span data-ttu-id="49db2-230">Skapa en regel inställning.</span><span class="sxs-lookup"><span data-stu-id="49db2-230">Create a rule setting.</span></span> <span data-ttu-id="49db2-231">Det här steget konfigurerar hello programmet gateway toouse URL-sökväg-baserad routning.</span><span class="sxs-lookup"><span data-stu-id="49db2-231">This step configures hello application gateway toouse URL path-based routing.</span></span> <span data-ttu-id="49db2-232">Hej `$urlPathMap` variabel som anges i hello tidigare steget är nu används toocreate hello sökväg-baserade regler.</span><span class="sxs-lookup"><span data-stu-id="49db2-232">hello `$urlPathMap` variable defined in hello earlier step is now used toocreate hello path-based rule.</span></span> <span data-ttu-id="49db2-233">I det här steget associera vi hello regel med en lyssnare och hello url sökvägsmappning skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="49db2-233">In this step, we associate hello rule with a listener and hello url path mapping created earlier.</span></span>

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-9"></a><span data-ttu-id="49db2-234">Steg 9</span><span class="sxs-lookup"><span data-stu-id="49db2-234">Step 9</span></span>

<span data-ttu-id="49db2-235">Konfigurera hello antal förekomster och storleken för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="49db2-235">Configure hello number of instances and size for hello application gateway.</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Small" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a><span data-ttu-id="49db2-236">Skapa Programgateway</span><span class="sxs-lookup"><span data-stu-id="49db2-236">Create Application Gateway</span></span>

<span data-ttu-id="49db2-237">Skapa en Programgateway med alla konfigurationsobjekt från hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="49db2-237">Create an application gateway with all configuration objects from hello preceding steps.</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku
```

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="49db2-238">Hämta DNS-namn för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="49db2-238">Get application gateway DNS name</span></span>

<span data-ttu-id="49db2-239">När du har skapat hello gateway är hello nästa steg tooconfigure hello klientdelen för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="49db2-239">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="49db2-240">När du använder en offentlig IP-adress krävs ett dynamiskt tilldelat DNS-namn som inte är användarvänligt.</span><span class="sxs-lookup"><span data-stu-id="49db2-240">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="49db2-241">tooensure slutanvändare kan träffa hello Programgateway, en CNAME-post kan vara används toopoint toohello offentlig slutpunkt för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="49db2-241">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="49db2-242">[Konfigurera ett eget domännamn i Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="49db2-242">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="49db2-243">tooconfigure hello klientdelens IP-CNAME-post, hämta information om hello Programgateway och dess associerade IP DNS-namn med hjälp av hello PublicIPAddress element bifogade toohello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="49db2-243">tooconfigure hello frontend IP CNAME record, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="49db2-244">hello programmet gateway DNS-namn ska använda toocreate en CNAME-post.</span><span class="sxs-lookup"><span data-stu-id="49db2-244">hello application gateway's DNS name should be used toocreate a CNAME record.</span></span> <span data-ttu-id="49db2-245">hello rekommenderas A-poster inte eftersom hello VIP ändras vid omstart för Programgateway.</span><span class="sxs-lookup"><span data-stu-id="49db2-245">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="49db2-246">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="49db2-246">Next steps</span></span>

<span data-ttu-id="49db2-247">Om du vill toolearn Secure Sockets Layer (SSL)-avlastning, se [konfigurera en Programgateway för SSL-avlastning](application-gateway-ssl-arm.md).</span><span class="sxs-lookup"><span data-stu-id="49db2-247">If you want toolearn Secure Sockets Layer (SSL) offload, see [Configure an application gateway for SSL offload](application-gateway-ssl-arm.md).</span></span>

