---
title: "aaaCreate en Programgateway som värd för flera platser | Microsoft Docs"
description: "Den här sidan finns instruktioner toocreate, konfigurera en gateway för Azure-program som värd för flera webbprogram på hello samma gateway."
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
ms.openlocfilehash: bad9a76be0a73a7026a770630fa7156f6e5940c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-for-hosting-multiple-web-applications"></a><span data-ttu-id="1e43a-103">Skapa en Programgateway som värd för flera webbprogram</span><span class="sxs-lookup"><span data-stu-id="1e43a-103">Create an application gateway for hosting multiple web applications</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="1e43a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1e43a-104">Azure portal</span></span>](application-gateway-create-multisite-portal.md)
> * [<span data-ttu-id="1e43a-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="1e43a-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-multisite-azureresourcemanager-powershell.md)

<span data-ttu-id="1e43a-106">Värd för flera plats kan du toodeploy mer än ett webbprogram på hello samma Programgateway.</span><span class="sxs-lookup"><span data-stu-id="1e43a-106">Multiple site hosting allows you toodeploy more than one web application on hello same application gateway.</span></span> <span data-ttu-id="1e43a-107">Det är beroende av förekomsten av värdhuvudet i hello inkommande HTTP-begäran, toodetermine vilka lyssnare skulle ta emot trafik.</span><span class="sxs-lookup"><span data-stu-id="1e43a-107">It relies on presence of host header in hello incoming HTTP request, toodetermine which listener would receive traffic.</span></span> <span data-ttu-id="1e43a-108">hello lyssnare dirigerar sedan trafik tooappropriate serverdelspool som konfigurerats i hello regler definition av hello gateway.</span><span class="sxs-lookup"><span data-stu-id="1e43a-108">hello listener then directs traffic tooappropriate backend pool as configured in hello rules definition of hello gateway.</span></span> <span data-ttu-id="1e43a-109">I SSL aktiverat webbprogram beroende Programgateway hello Server Servernamnsindikation (SNI)-tillägget toochoose hello rätt lyssnare för hello webbtrafik.</span><span class="sxs-lookup"><span data-stu-id="1e43a-109">In SSL enabled web applications, application gateway relies on hello Server Name Indication (SNI) extension toochoose hello correct listener for hello web traffic.</span></span> <span data-ttu-id="1e43a-110">Ett vanligt användningsområde för värd för flera plats är tooload begäranden för annan webbplats domäner toodifferent backend-serverpooler.</span><span class="sxs-lookup"><span data-stu-id="1e43a-110">A common use for multiple site hosting is tooload balance requests for different web domains toodifferent back-end server pools.</span></span> <span data-ttu-id="1e43a-111">På liknande sätt hello flera underdomäner i hello samma rotdomänen kan också finnas på samma Programgateway.</span><span class="sxs-lookup"><span data-stu-id="1e43a-111">Similarly multiple subdomains of hello same root domain could also be hosted on hello same application gateway.</span></span>

## <a name="scenario"></a><span data-ttu-id="1e43a-112">Scenario</span><span class="sxs-lookup"><span data-stu-id="1e43a-112">Scenario</span></span>

<span data-ttu-id="1e43a-113">I följande exempel hello, Programgateway betjäna trafik för contoso.com och fabrikam.com med två backend-server-adresspooler: contoso-serverpoolen och fabrikam-serverpoolen.</span><span class="sxs-lookup"><span data-stu-id="1e43a-113">In hello following example, application gateway is serving traffic for contoso.com and fabrikam.com with two back-end server pools: contoso server pool and fabrikam server pool.</span></span> <span data-ttu-id="1e43a-114">Liknande installationsprogrammet kan vara används toohost underdomäner som app.contoso.com och blog.contoso.com.</span><span class="sxs-lookup"><span data-stu-id="1e43a-114">Similar setup could be used toohost subdomains like app.contoso.com and blog.contoso.com.</span></span>

![imageURLroute](./media/application-gateway-create-multisite-azureresourcemanager-powershell/multisite.png)

## <a name="before-you-begin"></a><span data-ttu-id="1e43a-116">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="1e43a-116">Before you begin</span></span>

1. <span data-ttu-id="1e43a-117">Installera hello senaste versionen av hello Azure PowerShell-cmdlets med hello installationsprogram för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="1e43a-117">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="1e43a-118">Du kan hämta och installera hello senaste versionen från hello **Windows PowerShell** avsnitt i hello [Nedladdningssida](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="1e43a-118">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="1e43a-119">hello servrar lagts toohello backend-adresspool toouse hello Programgateway måste finnas eller har skapat sina slutpunkter i hello virtuellt nätverk i ett separat undernät eller med en offentlig IP-adress/VIP tilldelad.</span><span class="sxs-lookup"><span data-stu-id="1e43a-119">hello servers added toohello back-end pool toouse hello application gateway must exist or have their endpoints created either in hello virtual network in a separate subnet or with a public IP/VIP assigned.</span></span>

## <a name="requirements"></a><span data-ttu-id="1e43a-120">Krav</span><span class="sxs-lookup"><span data-stu-id="1e43a-120">Requirements</span></span>

* <span data-ttu-id="1e43a-121">**Backend-serverpoolen:** hello lista över IP-adresser för hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="1e43a-121">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="1e43a-122">hello IP-adresser som anges antingen ska tillhöra toohello undernät för virtuellt nätverk eller ska vara en offentlig IP-adress/VIP.</span><span class="sxs-lookup"><span data-stu-id="1e43a-122">hello IP addresses listed should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span> <span data-ttu-id="1e43a-123">FQDN kan också användas.</span><span class="sxs-lookup"><span data-stu-id="1e43a-123">FQDN can also be used.</span></span>
* <span data-ttu-id="1e43a-124">**Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet.</span><span class="sxs-lookup"><span data-stu-id="1e43a-124">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="1e43a-125">De här inställningarna är bundet tooa poolen och tillämpade tooall servrar inom hello poolen.</span><span class="sxs-lookup"><span data-stu-id="1e43a-125">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="1e43a-126">**Frontend-port:** den här porten är hello offentliga som öppnas på hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="1e43a-126">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="1e43a-127">Trafik träffar den här porten och sedan hämtar omdirigeras tooone hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="1e43a-127">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="1e43a-128">**Lyssnare:** hello-lyssnare har en frontend-port, ett protokoll (Http eller Https dessa värden är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning).</span><span class="sxs-lookup"><span data-stu-id="1e43a-128">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span> <span data-ttu-id="1e43a-129">För flera platser aktiverade programmet gateway läggs värdnamn och SNI indikatorer till.</span><span class="sxs-lookup"><span data-stu-id="1e43a-129">For multi-site enabled application gateways, host name and SNI indicators are also added.</span></span>
* <span data-ttu-id="1e43a-130">**Regel:** hello regeln Binder hello lyssnare, hello backend-serverpoolen, och definierar vilken backend-server pool hello trafik ska vara riktad toowhen den når en viss lyssnare.</span><span class="sxs-lookup"><span data-stu-id="1e43a-130">**Rule:** hello rule binds hello listener, hello back-end server pool, and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span> <span data-ttu-id="1e43a-131">Regler bearbetas i hello ordning och trafik dirigeras via hello första regeln som överensstämmer oavsett särskilda egenskaper.</span><span class="sxs-lookup"><span data-stu-id="1e43a-131">Rules are processed in hello order they are listed, and traffic will be directed via hello first rule that matches regardless of specificity.</span></span> <span data-ttu-id="1e43a-132">Till exempel om du har en regel med en grundläggande lyssnare och en regel med en flera platser lyssnare båda på samma port hello regel med hello måste hello flera platser lyssnare anges innan hello regeln med grundläggande hello-lyssnare för hello flera platser regeln toofunction som förväntades.</span><span class="sxs-lookup"><span data-stu-id="1e43a-132">For example, if you have a rule using a basic listener and a rule using a multi-site listener both on hello same port, hello rule with hello multi-site listener must be listed before hello rule with hello basic listener in order for hello multi-site rule toofunction as expected.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="1e43a-133">Skapa en programgateway</span><span class="sxs-lookup"><span data-stu-id="1e43a-133">Create an application gateway</span></span>

<span data-ttu-id="1e43a-134">hello följande är hello steg behövs toocreate en Programgateway:</span><span class="sxs-lookup"><span data-stu-id="1e43a-134">hello following are hello steps needed toocreate an application gateway:</span></span>

1. <span data-ttu-id="1e43a-135">Skapa en resursgrupp för Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1e43a-135">Create a resource group for Resource Manager.</span></span>
2. <span data-ttu-id="1e43a-136">Skapa ett virtuellt nätverk, undernät och offentliga IP för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="1e43a-136">Create a virtual network, subnets, and public IP for hello application gateway.</span></span>
3. <span data-ttu-id="1e43a-137">Skapa ett konfigurationsobjekt för programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="1e43a-137">Create an application gateway configuration object.</span></span>
4. <span data-ttu-id="1e43a-138">Skapa en resurs för en programgateway.</span><span class="sxs-lookup"><span data-stu-id="1e43a-138">Create an application gateway resource.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="1e43a-139">Skapa en resursgrupp för Resource Manager</span><span class="sxs-lookup"><span data-stu-id="1e43a-139">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="1e43a-140">Kontrollera att du använder hello senaste versionen av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1e43a-140">Make sure that you are using hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="1e43a-141">Mer information finns på [med hjälp av Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="1e43a-141">More information is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="1e43a-142">Steg 1</span><span class="sxs-lookup"><span data-stu-id="1e43a-142">Step 1</span></span>

<span data-ttu-id="1e43a-143">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="1e43a-143">Log in tooAzure</span></span>

```powershell
Login-AzureRmAccount
```
<span data-ttu-id="1e43a-144">Du kan ange tooauthenticate med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="1e43a-144">You are prompted tooauthenticate with your credentials.</span></span>

### <a name="step-2"></a><span data-ttu-id="1e43a-145">Steg 2</span><span class="sxs-lookup"><span data-stu-id="1e43a-145">Step 2</span></span>

<span data-ttu-id="1e43a-146">Kontrollera hello prenumerationer för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="1e43a-146">Check hello subscriptions for hello account.</span></span>

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a><span data-ttu-id="1e43a-147">Steg 3</span><span class="sxs-lookup"><span data-stu-id="1e43a-147">Step 3</span></span>

<span data-ttu-id="1e43a-148">Välj vilka av dina Azure-prenumerationer toouse.</span><span class="sxs-lookup"><span data-stu-id="1e43a-148">Choose which of your Azure subscriptions toouse.</span></span>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a><span data-ttu-id="1e43a-149">Steg 4</span><span class="sxs-lookup"><span data-stu-id="1e43a-149">Step 4</span></span>

<span data-ttu-id="1e43a-150">Skapa en resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp).</span><span class="sxs-lookup"><span data-stu-id="1e43a-150">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-RG -location "West US"
```

<span data-ttu-id="1e43a-151">Alternativt kan du också skapa taggar för en resursgrupp för Programgateway:</span><span class="sxs-lookup"><span data-stu-id="1e43a-151">Alternatively you can also create tags for a resource group for application gateway:</span></span>

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway multiple site"}
```

<span data-ttu-id="1e43a-152">Azure Resource Manager kräver att alla resursgrupper anger en plats.</span><span class="sxs-lookup"><span data-stu-id="1e43a-152">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="1e43a-153">Den här platsen används som hello standardplatsen för resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="1e43a-153">This location is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="1e43a-154">Se till att alla kommandon toocreate ett program gateway används hello samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="1e43a-154">Make sure that all commands toocreate an application gateway use hello same resource group.</span></span>

<span data-ttu-id="1e43a-155">Vi har skapat en resursgrupp med namnet i hello-exemplet ovan, **appgw RG** med en plats för **västra USA**.</span><span class="sxs-lookup"><span data-stu-id="1e43a-155">In hello example above, we created a resource group called **appgw-RG** with a location of **West US**.</span></span>

> [!NOTE]
> <span data-ttu-id="1e43a-156">Om du behöver tooconfigure en anpassad avsökningsåtgärd för din Programgateway finns [skapa en Programgateway med anpassade avsökningar med hjälp av PowerShell](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="1e43a-156">If you need tooconfigure a custom probe for your application gateway, see [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-ps.md).</span></span> <span data-ttu-id="1e43a-157">Besök [anpassade avsökningar, hälsoövervakning och](application-gateway-probe-overview.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="1e43a-157">Visit [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>

## <a name="create-a-virtual-network-and-subnets"></a><span data-ttu-id="1e43a-158">Skapa ett virtuellt nätverk och undernät</span><span class="sxs-lookup"><span data-stu-id="1e43a-158">Create a virtual network and subnets</span></span>

<span data-ttu-id="1e43a-159">följande exempel visar hur hello toocreate ett virtuellt nätverk med hjälp av hanteraren för filserverresurser.</span><span class="sxs-lookup"><span data-stu-id="1e43a-159">hello following example shows how toocreate a virtual network by using Resource Manager.</span></span> <span data-ttu-id="1e43a-160">Två undernät skapas i det här steget.</span><span class="sxs-lookup"><span data-stu-id="1e43a-160">Two subnets are created in this step.</span></span> <span data-ttu-id="1e43a-161">hello första undernätet är för hello Programgateway sig själv.</span><span class="sxs-lookup"><span data-stu-id="1e43a-161">hello first subnet is for hello application gateway itself.</span></span> <span data-ttu-id="1e43a-162">Programgateway kräver sin egen undernät toohold instanser.</span><span class="sxs-lookup"><span data-stu-id="1e43a-162">Application gateway requires its own subnet toohold its instances.</span></span> <span data-ttu-id="1e43a-163">Andra programgatewayer kan distribueras i det undernätet.</span><span class="sxs-lookup"><span data-stu-id="1e43a-163">Only other application gateways can be deployed in that subnet.</span></span> <span data-ttu-id="1e43a-164">hello andra undernät är används toohold hello programmet backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="1e43a-164">hello second subnet is used toohold hello application backend servers.</span></span>

### <a name="step-1"></a><span data-ttu-id="1e43a-165">Steg 1</span><span class="sxs-lookup"><span data-stu-id="1e43a-165">Step 1</span></span>

<span data-ttu-id="1e43a-166">Tilldela hello adressintervallet 10.0.0.0/24 toohello undernät variabeln toobe används toohold hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="1e43a-166">Assign hello address range 10.0.0.0/24 toohello subnet variable toobe used toohold hello application gateway.</span></span>

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -AddressPrefix 10.0.0.0/24
```
### <a name="step-2"></a><span data-ttu-id="1e43a-167">Steg 2</span><span class="sxs-lookup"><span data-stu-id="1e43a-167">Step 2</span></span>

<span data-ttu-id="1e43a-168">Tilldela hello adressintervallet 10.0.1.0/24 toohello Undernät2 variabeln toobe används för hello serverdelspooler.</span><span class="sxs-lookup"><span data-stu-id="1e43a-168">Assign hello address range 10.0.1.0/24 toohello subnet2 variable toobe used for hello backend pools.</span></span>

```powershell
$subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -AddressPrefix 10.0.1.0/24
```

### <a name="step-3"></a><span data-ttu-id="1e43a-169">Steg 3</span><span class="sxs-lookup"><span data-stu-id="1e43a-169">Step 3</span></span>

<span data-ttu-id="1e43a-170">Skapa ett virtuellt nätverk med namnet **appgwvnet** i resursgruppen **appgw rg** för hello västra USA region med undernätet 10.0.0.0/24 hello prefixet 10.0.0.0/16 och 10.0.1.0/24.</span><span class="sxs-lookup"><span data-stu-id="1e43a-170">Create a virtual network named **appgwvnet** in resource group **appgw-rg** for hello West US region using hello prefix 10.0.0.0/16 with subnet 10.0.0.0/24, and 10.0.1.0/24.</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet,$subnet2
```

### <a name="step-4"></a><span data-ttu-id="1e43a-171">Steg 4</span><span class="sxs-lookup"><span data-stu-id="1e43a-171">Step 4</span></span>

<span data-ttu-id="1e43a-172">Tilldela en variabel för hello nästa steg, undernät som skapar en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="1e43a-172">Assign a subnet variable for hello next steps, which creates an application gateway.</span></span>

```powershell
$appgatewaysubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -VirtualNetwork $vnet
$backendsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a><span data-ttu-id="1e43a-173">Skapa en offentlig IP-adress för hello frontend-konfiguration</span><span class="sxs-lookup"><span data-stu-id="1e43a-173">Create a public IP address for hello front-end configuration</span></span>

<span data-ttu-id="1e43a-174">Skapa en offentlig IP-resurs **publicIP01** i resursgruppen **appgw rg** för hello västra USA region.</span><span class="sxs-lookup"><span data-stu-id="1e43a-174">Create a public IP resource **publicIP01** in resource group **appgw-rg** for hello West US region.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

<span data-ttu-id="1e43a-175">En IP-adress tilldelas toohello Programgateway när hello-tjänsten startas.</span><span class="sxs-lookup"><span data-stu-id="1e43a-175">An IP address is assigned toohello application gateway when hello service starts.</span></span>

## <a name="create-application-gateway-configuration"></a><span data-ttu-id="1e43a-176">Skapa program gateway-konfiguration</span><span class="sxs-lookup"><span data-stu-id="1e43a-176">Create application gateway configuration</span></span>

<span data-ttu-id="1e43a-177">Du har tooset in alla konfigurationsobjekt innan du skapar hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="1e43a-177">You have tooset up all configuration items before creating hello application gateway.</span></span> <span data-ttu-id="1e43a-178">hello med följande steg skapar hello konfigurationsobjekt som behövs för en gateway programresursen.</span><span class="sxs-lookup"><span data-stu-id="1e43a-178">hello following steps create hello configuration items that are needed for an application gateway resource.</span></span>

### <a name="step-1"></a><span data-ttu-id="1e43a-179">Steg 1</span><span class="sxs-lookup"><span data-stu-id="1e43a-179">Step 1</span></span>

<span data-ttu-id="1e43a-180">Skapa en IP-konfiguration för programgatewayen med namnet **gatewayIP01**.</span><span class="sxs-lookup"><span data-stu-id="1e43a-180">Create an application gateway IP configuration named **gatewayIP01**.</span></span> <span data-ttu-id="1e43a-181">När Programgateway startar hämtar en IP-adress från hello-undernät som konfigurerats och dirigera trafik toohello IP-adresser på nätverket i hello backend-IP-adresspool.</span><span class="sxs-lookup"><span data-stu-id="1e43a-181">When application gateway starts, it picks up an IP address from hello subnet configured and route network traffic toohello IP addresses in hello back-end IP pool.</span></span> <span data-ttu-id="1e43a-182">Tänk på att varje instans använder en IP-adress.</span><span class="sxs-lookup"><span data-stu-id="1e43a-182">Keep in mind that each instance takes one IP address.</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $appgatewaysubnet
```

### <a name="step-2"></a><span data-ttu-id="1e43a-183">Steg 2</span><span class="sxs-lookup"><span data-stu-id="1e43a-183">Step 2</span></span>

<span data-ttu-id="1e43a-184">Konfigurera hello backend-IP-adresspool med namnet **pool01** och **pool2** med IP-adresser **134.170.185.46**, **134.170.188.221**, **134.170.185.50** för **pool1** och **134.170.186.46**, **134.170.189.221**, **134.170.186.50**  för **pool2**.</span><span class="sxs-lookup"><span data-stu-id="1e43a-184">Configure hello back-end IP address pool named **pool01** and **pool2** with IP addresses **134.170.185.46**, **134.170.188.221**, **134.170.185.50** for **pool1** and **134.170.186.46**, **134.170.189.221**, **134.170.186.50** for **pool2**.</span></span>

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.0.1.100, 10.0.1.101, 10.0.1.102
$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 10.0.1.103, 10.0.1.104, 10.0.1.105
```

<span data-ttu-id="1e43a-185">I det här exemplet finns det två backend-pooler tooroute nätverkstrafik som baserat på hello begärda webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="1e43a-185">In this example, there are two back-end pools tooroute network traffic based on hello requested site.</span></span> <span data-ttu-id="1e43a-186">En pool tar emot trafik från platsen ”contoso.com” och andra poolen tar emot trafik från platsen ”fabrikam.com”.</span><span class="sxs-lookup"><span data-stu-id="1e43a-186">One pool receives traffic from site "contoso.com" and other pool receives traffic from site "fabrikam.com".</span></span> <span data-ttu-id="1e43a-187">Du har tooreplace hello föregående IP-adresser tooadd egna programslutpunkter IP-adress.</span><span class="sxs-lookup"><span data-stu-id="1e43a-187">You have tooreplace hello preceding IP addresses tooadd your own application IP address endpoints.</span></span> <span data-ttu-id="1e43a-188">I stället för interna IP-adresser, kan du också använda offentliga IP-adresser, FQDN eller en virtuell dators nätverkskort för backend-instanser.</span><span class="sxs-lookup"><span data-stu-id="1e43a-188">In place of internal IP addresses, you could also use public IP addresses, FQDN, or a VM's NIC for backend instances.</span></span> <span data-ttu-id="1e43a-189">toospecify FQDN i stället för IP-adresser PowerShell används ”-BackendFQDNs” parametern.</span><span class="sxs-lookup"><span data-stu-id="1e43a-189">toospecify FQDNs instead of IPs in PowerShell use "-BackendFQDNs" parameter.</span></span>

### <a name="step-3"></a><span data-ttu-id="1e43a-190">Steg 3</span><span class="sxs-lookup"><span data-stu-id="1e43a-190">Step 3</span></span>

<span data-ttu-id="1e43a-191">Konfigurera gateway programinställning **poolsetting01** och **poolsetting02** för hello belastningsutjämnad nätverkstrafik i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="1e43a-191">Configure application gateway setting **poolsetting01** and **poolsetting02** for hello load-balanced network traffic in hello back-end pool.</span></span> <span data-ttu-id="1e43a-192">I det här exemplet kan du konfigurera inställningarna för olika backend-pool för hello backend-pooler.</span><span class="sxs-lookup"><span data-stu-id="1e43a-192">In this example, you configure different back-end pool settings for hello back-end pools.</span></span> <span data-ttu-id="1e43a-193">Varje serverdelspool kan ha sin egen serverdelspoolinställning.</span><span class="sxs-lookup"><span data-stu-id="1e43a-193">Each back-end pool can have its own back-end pool setting.</span></span>

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120
$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a><span data-ttu-id="1e43a-194">Steg 4</span><span class="sxs-lookup"><span data-stu-id="1e43a-194">Step 4</span></span>

<span data-ttu-id="1e43a-195">Konfigurera IP-frontend-hello med offentliga IP-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="1e43a-195">Configure hello front-end IP with public IP endpoint.</span></span>

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a><span data-ttu-id="1e43a-196">Steg 5</span><span class="sxs-lookup"><span data-stu-id="1e43a-196">Step 5</span></span>

<span data-ttu-id="1e43a-197">Konfigurera hello frontend-port för en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="1e43a-197">Configure hello front-end port for an application gateway.</span></span>

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 443
```

### <a name="step-6"></a><span data-ttu-id="1e43a-198">Steg 6</span><span class="sxs-lookup"><span data-stu-id="1e43a-198">Step 6</span></span>

<span data-ttu-id="1e43a-199">Konfigurera två SSL-certifikat för hello två webbplatser vi toosupport i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="1e43a-199">Configure two SSL certificates for hello two websites we are going toosupport in this example.</span></span> <span data-ttu-id="1e43a-200">Ett certifikat är för contoso.com trafik och hello andra för fabrikam.com trafik.</span><span class="sxs-lookup"><span data-stu-id="1e43a-200">One certificate is for contoso.com traffic and hello other is for fabrikam.com traffic.</span></span> <span data-ttu-id="1e43a-201">Dessa certifikat ska vara en certifikatutfärdare som utfärdade certifikat för webbplatser.</span><span class="sxs-lookup"><span data-stu-id="1e43a-201">These certificates should be a Certificate Authority issued certificates for your websites.</span></span> <span data-ttu-id="1e43a-202">Självsignerade certifikat stöds men rekommenderas inte för produktion trafik.</span><span class="sxs-lookup"><span data-stu-id="1e43a-202">Self-signed certificates are supported but not recommended for production traffic.</span></span>

```powershell
$cert01 = New-AzureRmApplicationGatewaySslCertificate -Name contosocert -CertificateFile <file path> -Password <password>
$cert02 = New-AzureRmApplicationGatewaySslCertificate -Name fabrikamcert -CertificateFile <file path> -Password <password>
```

### <a name="step-7"></a><span data-ttu-id="1e43a-203">Steg 7</span><span class="sxs-lookup"><span data-stu-id="1e43a-203">Step 7</span></span>

<span data-ttu-id="1e43a-204">Konfigurera två lyssnare för hello två webbplatser i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="1e43a-204">Configure two listeners for hello two web sites in this example.</span></span> <span data-ttu-id="1e43a-205">Det här steget konfigurerar hello-lyssnare för offentlig IP-adress, port och värdnamn används tooreceive inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="1e43a-205">This step configures hello listeners for public IP address, port, and host used tooreceive incoming traffic.</span></span> <span data-ttu-id="1e43a-206">HostName parametern krävs för stöd för flera plats och bör vara set toohello lämplig webbplats för vilken hello trafik som tas emot.</span><span class="sxs-lookup"><span data-stu-id="1e43a-206">HostName parameter is required for multiple site support and should be set toohello appropriate website for which hello traffic is received.</span></span> <span data-ttu-id="1e43a-207">RequireServerNameIndication parameter ska anges tootrue för webbplatser som behöver stöd för SSL i ett scenario med flera värden.</span><span class="sxs-lookup"><span data-stu-id="1e43a-207">RequireServerNameIndication parameter should be set tootrue for websites that need support for SSL in a multiple host scenario.</span></span> <span data-ttu-id="1e43a-208">Om SSL-stöd krävs, måste du också toospecify hello SSL-certifikatet som används toosecure trafik för att webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="1e43a-208">If SSL support is required, you also need toospecify hello SSL certificate that is used toosecure traffic for that web application.</span></span> <span data-ttu-id="1e43a-209">hello kombination av FrontendIPConfiguration, FrontendPort och värdnamn måste vara unika tooa lyssnare.</span><span class="sxs-lookup"><span data-stu-id="1e43a-209">hello combination of FrontendIPConfiguration, FrontendPort, and HostName must be unique tooa listener.</span></span> <span data-ttu-id="1e43a-210">Varje lyssnare har stöd för ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="1e43a-210">Each listener can support one certificate.</span></span>

```powershell
$listener01 = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "contoso11.com" -RequireServerNameIndication true  -SslCertificate $cert01
$listener02 = New-AzureRmApplicationGatewayHttpListener -Name "listener02" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "fabrikam11.com" -RequireServerNameIndication true -SslCertificate $cert02
```

### <a name="step-8"></a><span data-ttu-id="1e43a-211">Steg 8</span><span class="sxs-lookup"><span data-stu-id="1e43a-211">Step 8</span></span>

<span data-ttu-id="1e43a-212">Skapa två regel inställningen för hello två webbprogram i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="1e43a-212">Create two rule setting for hello two web applications in this example.</span></span> <span data-ttu-id="1e43a-213">En regel innehåller lyssnare, serverdelspooler och http-inställningar.</span><span class="sxs-lookup"><span data-stu-id="1e43a-213">A rule ties together listeners, backend pools and http settings.</span></span> <span data-ttu-id="1e43a-214">Det här steget konfigurerar hello programmet gateway toouse grundläggande routningsregel, ett för varje webbplats.</span><span class="sxs-lookup"><span data-stu-id="1e43a-214">This step configures hello application gateway toouse Basic routing rule, one for each website.</span></span> <span data-ttu-id="1e43a-215">Trafik tooeach webbplats tas emot av dess konfigurerade lyssnare och sedan dirigeras tooits konfigurerats serverdelspoolen, genom att använda hello egenskaper som anges i hello BackendHttpSettings.</span><span class="sxs-lookup"><span data-stu-id="1e43a-215">Traffic tooeach website is received by its configured listener, and is then directed tooits configured backend pool, using hello properties specified in hello BackendHttpSettings.</span></span>

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule01" -RuleType Basic -HttpListener $listener01 -BackendHttpSettings $poolSetting01 -BackendAddressPool $pool1
$rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule02" -RuleType Basic -HttpListener $listener02 -BackendHttpSettings $poolSetting02 -BackendAddressPool $pool2
```

### <a name="step-9"></a><span data-ttu-id="1e43a-216">Steg 9</span><span class="sxs-lookup"><span data-stu-id="1e43a-216">Step 9</span></span>

<span data-ttu-id="1e43a-217">Konfigurera hello antal förekomster och storleken för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="1e43a-217">Configure hello number of instances and size for hello application gateway.</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Medium" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a><span data-ttu-id="1e43a-218">Skapa Programgateway</span><span class="sxs-lookup"><span data-stu-id="1e43a-218">Create application gateway</span></span>

<span data-ttu-id="1e43a-219">Skapa en Programgateway med alla konfigurationsobjekt från hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="1e43a-219">Create an application gateway with all configuration objects from hello preceding steps.</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 -Sku $sku -SslCertificates $cert01, $cert02
```

> [!IMPORTANT]
> <span data-ttu-id="1e43a-220">Programmet Gateway etablering är en tidskrävande åtgärd och kan ta viss tid toocomplete.</span><span class="sxs-lookup"><span data-stu-id="1e43a-220">Application Gateway provisioning is a long running operation and may take some time toocomplete.</span></span>
> 
> 

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="1e43a-221">Hämta DNS-namn för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="1e43a-221">Get application gateway DNS name</span></span>

<span data-ttu-id="1e43a-222">När du har skapat hello gateway är hello nästa steg tooconfigure hello klientdelen för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="1e43a-222">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="1e43a-223">När du använder en offentlig IP-adress krävs ett dynamiskt tilldelat DNS-namn som inte är användarvänligt.</span><span class="sxs-lookup"><span data-stu-id="1e43a-223">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="1e43a-224">tooensure slutanvändare kan träffa hello Programgateway, en CNAME-post kan vara används toopoint toohello offentlig slutpunkt för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="1e43a-224">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="1e43a-225">[Konfigurera ett eget domännamn i Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1e43a-225">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="1e43a-226">toodo detta, hämta information om hello Programgateway och dess associerade IP DNS-namn med hjälp av hello PublicIPAddress element bifogade toohello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="1e43a-226">toodo this, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="1e43a-227">hello programmet gateway DNS-namn ska använda toocreate en CNAME-post som pekar hello två web program toothis DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="1e43a-227">hello application gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="1e43a-228">hello rekommenderas A-poster inte eftersom hello VIP ändras vid omstart för Programgateway.</span><span class="sxs-lookup"><span data-stu-id="1e43a-228">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="1e43a-229">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1e43a-229">Next steps</span></span>

<span data-ttu-id="1e43a-230">Lär dig hur tooprotect dina webbplatser med [Programgateway - Brandvägg för webbaserade program](application-gateway-webapplicationfirewall-overview.md)</span><span class="sxs-lookup"><span data-stu-id="1e43a-230">Learn how tooprotect your websites with [Application Gateway - Web Application Firewall](application-gateway-webapplicationfirewall-overview.md)</span></span>

