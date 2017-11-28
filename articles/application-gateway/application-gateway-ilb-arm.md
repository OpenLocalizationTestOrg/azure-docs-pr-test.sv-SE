---
title: "aaaUsing Azure Programgateway med interna belastningsutjämnare - PowerShell | Microsoft Docs"
description: "Den här sidan innehåller instruktioner toocreate, konfigurera, starta och ta bort en Azure Programgateway med intern belastningsutjämnare (ILB) för Azure Resource Manager"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 75cfd5a2-e378-4365-99ee-a2b2abda2e0d
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: dd0d7e954b1fa219ae6ebe42cb4b479dbcf08653
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb-by-using-azure-resource-manager"></a><span data-ttu-id="0ec66-103">Skapa en programgateway med en intern belastningsutjämnare (ILB) med hjälp av Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0ec66-103">Create an application gateway with an internal load balancer (ILB) by using Azure Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="0ec66-104">PowerShell och den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0ec66-104">Azure Classic PowerShell</span></span>](application-gateway-ilb.md)
> * [<span data-ttu-id="0ec66-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0ec66-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ilb-arm.md)

<span data-ttu-id="0ec66-106">Azure Application Gateway kan konfigureras med en Internet-riktade VIP eller med en intern slutpunkt som inte är synliga toohello Internet, även kallat en intern belastningsutjämnare (ILB) slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="0ec66-106">Azure Application Gateway can be configured with an Internet-facing VIP or with an internal endpoint that is not exposed toohello Internet, also known as an internal load balancer (ILB) endpoint.</span></span> <span data-ttu-id="0ec66-107">Konfigurera hello gateway med en ILB är användbart för interna line-of-business-program som inte är synliga toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="0ec66-107">Configuring hello gateway with an ILB is useful for internal line-of-business applications that are not exposed toohello Internet.</span></span> <span data-ttu-id="0ec66-108">Det är också användbart för tjänster och nivåer inom en flernivåapp som sitter i en säkerhetsgräns som inte är synliga toohello Internet men fortfarande kräver resursallokering belastningsutjämna distribution, varaktighet för sessionen eller Secure Sockets Layer (SSL)-avslutning.</span><span class="sxs-lookup"><span data-stu-id="0ec66-108">It's also useful for services and tiers within a multi-tier application that sit in a security boundary that is not exposed toohello Internet but still require round-robin load distribution, session stickiness, or Secure Sockets Layer (SSL) termination.</span></span>

<span data-ttu-id="0ec66-109">Den här artikeln vägleder dig genom hello steg tooconfigure en Programgateway med en ILB.</span><span class="sxs-lookup"><span data-stu-id="0ec66-109">This article walks you through hello steps tooconfigure an application gateway with an ILB.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="0ec66-110">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="0ec66-110">Before you begin</span></span>

1. <span data-ttu-id="0ec66-111">Installera hello senaste versionen av hello Azure PowerShell-cmdlets med hello installationsprogram för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="0ec66-111">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="0ec66-112">Du kan hämta och installera hello senaste versionen från hello **Windows PowerShell** avsnitt i hello [Nedladdningssida](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="0ec66-112">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="0ec66-113">Du ska skapa ett virtuellt nätverk och ett undernät för Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="0ec66-113">You create a virtual network and a subnet for Application Gateway.</span></span> <span data-ttu-id="0ec66-114">Se till att inga virtuella datorer eller molndistributioner använder hello undernät.</span><span class="sxs-lookup"><span data-stu-id="0ec66-114">Make sure that no virtual machines or cloud deployments are using hello subnet.</span></span> <span data-ttu-id="0ec66-115">Application Gateway måste vara fristående i ett virtuellt nätverks undernät.</span><span class="sxs-lookup"><span data-stu-id="0ec66-115">Application Gateway must be by itself in a virtual network subnet.</span></span>
3. <span data-ttu-id="0ec66-116">hello-servrar som du konfigurerar toouse hello Programgateway måste finnas eller tilldelats deras slutpunkter har skapats i hello virtuellt nätverk eller med en offentlig IP-adress/VIP.</span><span class="sxs-lookup"><span data-stu-id="0ec66-116">hello servers that you configure toouse hello application gateway must exist or have their endpoints created either in hello virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-toocreate-an-application-gateway"></a><span data-ttu-id="0ec66-117">Vad är obligatoriska toocreate en Programgateway?</span><span class="sxs-lookup"><span data-stu-id="0ec66-117">What is required toocreate an application gateway?</span></span>

* <span data-ttu-id="0ec66-118">**Backend-serverpoolen:** hello lista över IP-adresser för hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="0ec66-118">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="0ec66-119">hello IP-adresser i listan ska antingen tillhöra toohello virtuellt nätverk, men i ett annat undernät för hello Programgateway eller ska vara en offentlig IP-adress/VIP.</span><span class="sxs-lookup"><span data-stu-id="0ec66-119">hello IP addresses listed should either belong toohello virtual network but in a different subnet for hello application gateway or should be a public IP/VIP.</span></span>
* <span data-ttu-id="0ec66-120">**Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet.</span><span class="sxs-lookup"><span data-stu-id="0ec66-120">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="0ec66-121">De här inställningarna är bundet tooa poolen och tillämpade tooall servrar inom hello poolen.</span><span class="sxs-lookup"><span data-stu-id="0ec66-121">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="0ec66-122">**Frontend-port:** den här porten är hello offentliga som öppnas på hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="0ec66-122">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="0ec66-123">Trafik träffar den här porten och sedan hämtar omdirigeras tooone hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="0ec66-123">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="0ec66-124">**Lyssnare:** hello-lyssnare har en frontend-port, ett protokoll (Http eller Https, dessa är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning).</span><span class="sxs-lookup"><span data-stu-id="0ec66-124">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="0ec66-125">**Regel:** hello regeln Binder hello-lyssnare och hello backend-serverpoolen och definierar vilken backend-server pool hello trafik ska vara riktad toowhen den når en viss lyssnare.</span><span class="sxs-lookup"><span data-stu-id="0ec66-125">**Rule:** hello rule binds hello listener and hello back-end server pool and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span> <span data-ttu-id="0ec66-126">För närvarande endast hello *grundläggande* regeln stöds.</span><span class="sxs-lookup"><span data-stu-id="0ec66-126">Currently, only hello *basic* rule is supported.</span></span> <span data-ttu-id="0ec66-127">Hej *grundläggande* regeln är resursallokering belastningsdistribution.</span><span class="sxs-lookup"><span data-stu-id="0ec66-127">hello *basic* rule is round-robin load distribution.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="0ec66-128">Skapa en programgateway</span><span class="sxs-lookup"><span data-stu-id="0ec66-128">Create an application gateway</span></span>

<span data-ttu-id="0ec66-129">hello skillnaden mellan att använda den klassiska Azure och Azure Resource Manager är hello order som du kan skapa hello Programgateway och hello-objekt som behöver toobe konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="0ec66-129">hello difference between using Azure Classic and Azure Resource Manager is hello order in which you create hello application gateway and hello items that need toobe configured.</span></span>
<span data-ttu-id="0ec66-130">Med Resource Manager alla poster i en Programgateway konfigureras individuellt och sedan sätta ihop toocreate hello programresursen gateway.</span><span class="sxs-lookup"><span data-stu-id="0ec66-130">With Resource Manager, all items that make an application gateway is configured individually and then put together toocreate hello application gateway resource.</span></span>

<span data-ttu-id="0ec66-131">Här följer hello steg som är nödvändiga toocreate en Programgateway:</span><span class="sxs-lookup"><span data-stu-id="0ec66-131">Here are hello steps that are needed toocreate an application gateway:</span></span>

1. <span data-ttu-id="0ec66-132">Skapa en resursgrupp för Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0ec66-132">Create a resource group for Resource Manager</span></span>
2. <span data-ttu-id="0ec66-133">Skapa ett virtuellt nätverk och ett undernät för hello Programgateway</span><span class="sxs-lookup"><span data-stu-id="0ec66-133">Create a virtual network and a subnet for hello application gateway</span></span>
3. <span data-ttu-id="0ec66-134">Skapa ett konfigurationsobjekt för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="0ec66-134">Create an application gateway configuration object</span></span>
4. <span data-ttu-id="0ec66-135">Skapa en resurs för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="0ec66-135">Create an application gateway resource</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="0ec66-136">Skapa en resursgrupp för Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0ec66-136">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="0ec66-137">Kontrollera att du växla PowerShell läge toouse hello Azure Resource Manager-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="0ec66-137">Make sure that you switch PowerShell mode toouse hello Azure Resource Manager cmdlets.</span></span> <span data-ttu-id="0ec66-138">Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="0ec66-138">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="0ec66-139">Steg 1</span><span class="sxs-lookup"><span data-stu-id="0ec66-139">Step 1</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a><span data-ttu-id="0ec66-140">Steg 2</span><span class="sxs-lookup"><span data-stu-id="0ec66-140">Step 2</span></span>

<span data-ttu-id="0ec66-141">Kontrollera hello prenumerationer för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="0ec66-141">Check hello subscriptions for hello account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="0ec66-142">Du kan ange tooauthenticate med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0ec66-142">You are prompted tooauthenticate with your credentials.</span></span>

### <a name="step-3"></a><span data-ttu-id="0ec66-143">Steg 3</span><span class="sxs-lookup"><span data-stu-id="0ec66-143">Step 3</span></span>

<span data-ttu-id="0ec66-144">Välj vilka av dina Azure-prenumerationer toouse.</span><span class="sxs-lookup"><span data-stu-id="0ec66-144">Choose which of your Azure subscriptions toouse.</span></span>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a><span data-ttu-id="0ec66-145">Steg 4</span><span class="sxs-lookup"><span data-stu-id="0ec66-145">Step 4</span></span>

<span data-ttu-id="0ec66-146">Skapa en ny resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp).</span><span class="sxs-lookup"><span data-stu-id="0ec66-146">Create a new resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-rg -location "West US"
```

<span data-ttu-id="0ec66-147">Azure Resource Manager kräver att alla resursgrupper anger en plats.</span><span class="sxs-lookup"><span data-stu-id="0ec66-147">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="0ec66-148">Detta används som hello standardplatsen för resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="0ec66-148">This is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="0ec66-149">Se till att alla kommandon toocreate använder en Programgateway hello samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="0ec66-149">Make sure that all commands toocreate an application gateway uses hello same resource group.</span></span>

<span data-ttu-id="0ec66-150">I föregående exempel hello, skapat vi en resursgrupp med namnet ”appgw rg” och plats ”USA, västra”.</span><span class="sxs-lookup"><span data-stu-id="0ec66-150">In hello preceding example, we created a resource group called "appgw-rg" and location "West US".</span></span>

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a><span data-ttu-id="0ec66-151">Skapa ett virtuellt nätverk och ett undernät för hello Programgateway</span><span class="sxs-lookup"><span data-stu-id="0ec66-151">Create a virtual network and a subnet for hello application gateway</span></span>

<span data-ttu-id="0ec66-152">följande exempel visar hur hello toocreate ett virtuellt nätverk med hjälp av hanteraren för filserverresurser:</span><span class="sxs-lookup"><span data-stu-id="0ec66-152">hello following example shows how toocreate a virtual network by using Resource Manager:</span></span>

### <a name="step-1"></a><span data-ttu-id="0ec66-153">Steg 1</span><span class="sxs-lookup"><span data-stu-id="0ec66-153">Step 1</span></span>

```powershell
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

<span data-ttu-id="0ec66-154">Det här steget tilldelas hello adressintervallet 10.0.0.0/24 tooa undernät variabeln toobe används toocreate ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="0ec66-154">This step assigns hello address range 10.0.0.0/24 tooa subnet variable toobe used toocreate a virtual network.</span></span>

### <a name="step-2"></a><span data-ttu-id="0ec66-155">Steg 2</span><span class="sxs-lookup"><span data-stu-id="0ec66-155">Step 2</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig
```

<span data-ttu-id="0ec66-156">Det här steget skapar ett virtuellt nätverk med namnet ”appgwvnet” i resursen grupp ”appgw-rg” för hello västra USA region med undernätet 10.0.0.0/24 hello prefixet 10.0.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="0ec66-156">This step creates a virtual network named "appgwvnet" in resource group "appgw-rg" for hello West US region using hello prefix 10.0.0.0/16 with subnet 10.0.0.0/24.</span></span>

### <a name="step-3"></a><span data-ttu-id="0ec66-157">Steg 3</span><span class="sxs-lookup"><span data-stu-id="0ec66-157">Step 3</span></span>

```powershell
$subnet = $vnet.subnets[0]
```

<span data-ttu-id="0ec66-158">Det här steget tilldelas hello undernät objektet toovariable $subnet hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="0ec66-158">This step assigns hello subnet object toovariable $subnet for hello next steps.</span></span>

## <a name="create-an-application-gateway-configuration-object"></a><span data-ttu-id="0ec66-159">Skapa ett konfigurationsobjekt för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="0ec66-159">Create an application gateway configuration object</span></span>

### <a name="step-1"></a><span data-ttu-id="0ec66-160">Steg 1</span><span class="sxs-lookup"><span data-stu-id="0ec66-160">Step 1</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

<span data-ttu-id="0ec66-161">Det här steget skapar ett program gateway IP-konfiguration med namnet ”gatewayIP01”.</span><span class="sxs-lookup"><span data-stu-id="0ec66-161">This step creates an application gateway IP configuration named "gatewayIP01".</span></span> <span data-ttu-id="0ec66-162">När Programgateway startar hämtar en IP-adress från hello-undernät som konfigurerats och dirigera trafik toohello IP-adresser på nätverket i hello backend-IP-adresspool.</span><span class="sxs-lookup"><span data-stu-id="0ec66-162">When Application Gateway starts, it picks up an IP address from hello subnet configured and route network traffic toohello IP addresses in hello back-end IP pool.</span></span> <span data-ttu-id="0ec66-163">Tänk på att varje instans använder en IP-adress.</span><span class="sxs-lookup"><span data-stu-id="0ec66-163">Keep in mind that each instance takes one IP address.</span></span>

### <a name="step-2"></a><span data-ttu-id="0ec66-164">Steg 2</span><span class="sxs-lookup"><span data-stu-id="0ec66-164">Step 2</span></span>

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.1.1.8,10.1.1.9,10.1.1.10
```

<span data-ttu-id="0ec66-165">Det här steget konfigurerar hello backend-IP-adresspool med namnet ”pool01” med IP-adresser ”10.1.1.8, 10.1.1.9, 10.1.1.10”.</span><span class="sxs-lookup"><span data-stu-id="0ec66-165">This step configures hello back-end IP address pool named "pool01" with IP addresses "10.1.1.8, 10.1.1.9, 10.1.1.10".</span></span> <span data-ttu-id="0ec66-166">De är hello IP-adresser som tar emot hello nätverkstrafik som kommer från hello frontend IP-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="0ec66-166">Those are hello IP addresses that receive hello network traffic that comes from hello front-end IP endpoint.</span></span> <span data-ttu-id="0ec66-167">Ersätt hello föregående IP-adresser tooadd egna programslutpunkter IP-adress.</span><span class="sxs-lookup"><span data-stu-id="0ec66-167">You replace hello preceding IP addresses tooadd your own application IP address endpoints.</span></span>

### <a name="step-3"></a><span data-ttu-id="0ec66-168">Steg 3</span><span class="sxs-lookup"><span data-stu-id="0ec66-168">Step 3</span></span>

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

<span data-ttu-id="0ec66-169">Det här steget konfigurerar programmet gateway inställningen ”poolsetting01” för hello belastningsutjämnade trafik i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="0ec66-169">This step configures application gateway setting "poolsetting01" for hello load balanced network traffic in hello back-end pool.</span></span>

### <a name="step-4"></a><span data-ttu-id="0ec66-170">Steg 4</span><span class="sxs-lookup"><span data-stu-id="0ec66-170">Step 4</span></span>

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80
```

<span data-ttu-id="0ec66-171">Det här steget konfigurerar hello frontend IP-port med namnet ”frontendport01” för hello ILB.</span><span class="sxs-lookup"><span data-stu-id="0ec66-171">This step configures hello front-end IP port named "frontendport01" for hello ILB.</span></span>

### <a name="step-5"></a><span data-ttu-id="0ec66-172">Steg 5</span><span class="sxs-lookup"><span data-stu-id="0ec66-172">Step 5</span></span>

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet
```

<span data-ttu-id="0ec66-173">Det här steget skapar hello frontend IP-konfiguration som kallas ”fipconfig01” och associerar den med en privat IP-adress från hello aktuella undernät för virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="0ec66-173">This step creates hello front-end IP configuration called "fipconfig01" and associates it with a private IP from hello current virtual network subnet.</span></span>

### <a name="step-6"></a><span data-ttu-id="0ec66-174">Steg 6</span><span class="sxs-lookup"><span data-stu-id="0ec66-174">Step 6</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp
```

<span data-ttu-id="0ec66-175">Det här steget skapar hello lyssnare som kallas ”listener01” och associerar hello frontend port toohello frontend IP-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="0ec66-175">This step creates hello listener called "listener01" and associates hello front-end port toohello front-end IP configuration.</span></span>

### <a name="step-7"></a><span data-ttu-id="0ec66-176">Steg 7</span><span class="sxs-lookup"><span data-stu-id="0ec66-176">Step 7</span></span>

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

<span data-ttu-id="0ec66-177">Det här steget skapar hello routning belastningsutjämningsregeln kallas ”rule01” som konfigurerar hello belastningsutjämnaren GUID.</span><span class="sxs-lookup"><span data-stu-id="0ec66-177">This step creates hello load balancer routing rule called "rule01" that configures hello load balancer behavior.</span></span>

### <a name="step-8"></a><span data-ttu-id="0ec66-178">Steg 8</span><span class="sxs-lookup"><span data-stu-id="0ec66-178">Step 8</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

<span data-ttu-id="0ec66-179">Det här steget konfigurerar hello instansstorleken för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="0ec66-179">This step configures hello instance size of hello application gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="0ec66-180">Hej standardvärdet för *InstanceCount* är 2, med ett maximalt värde 10.</span><span class="sxs-lookup"><span data-stu-id="0ec66-180">hello default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="0ec66-181">Hej standardvärdet för *GatewaySize* är Medium.</span><span class="sxs-lookup"><span data-stu-id="0ec66-181">hello default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="0ec66-182">Du kan välja mellan Standard_Small, Standard_Medium och Standard_Large.</span><span class="sxs-lookup"><span data-stu-id="0ec66-182">You can choose between Standard_Small, Standard_Medium, and Standard_Large.</span></span>

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a><span data-ttu-id="0ec66-183">Skapa en programgateway med hjälp av New-AzureApplicationGateway</span><span class="sxs-lookup"><span data-stu-id="0ec66-183">Create an application gateway by using New-AzureApplicationGateway</span></span>

<span data-ttu-id="0ec66-184">Skapar en Programgateway med alla konfigurationsobjekt från hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="0ec66-184">Creates an application gateway with all configuration items from hello preceding steps.</span></span> <span data-ttu-id="0ec66-185">I det här exemplet kallas hello Programgateway ”appgwtest”.</span><span class="sxs-lookup"><span data-stu-id="0ec66-185">In this example, hello application gateway is called "appgwtest".</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

<span data-ttu-id="0ec66-186">Det här steget skapar en Programgateway med alla konfigurationsobjekt från hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="0ec66-186">This step creates an application gateway with all configuration items from hello preceding steps.</span></span> <span data-ttu-id="0ec66-187">I exemplet hello kallas hello Programgateway ”appgwtest”.</span><span class="sxs-lookup"><span data-stu-id="0ec66-187">In hello example, hello application gateway is called "appgwtest".</span></span>

## <a name="delete-an-application-gateway"></a><span data-ttu-id="0ec66-188">Ta bort en programgateway</span><span class="sxs-lookup"><span data-stu-id="0ec66-188">Delete an application gateway</span></span>

<span data-ttu-id="0ec66-189">toodelete en Programgateway måste toodo hello följa stegen i ordning:</span><span class="sxs-lookup"><span data-stu-id="0ec66-189">toodelete an application gateway, you need toodo hello following steps in order:</span></span>

1. <span data-ttu-id="0ec66-190">Använd hello `Stop-AzureRmApplicationGateway` cmdlet toostop hello gateway.</span><span class="sxs-lookup"><span data-stu-id="0ec66-190">Use hello `Stop-AzureRmApplicationGateway` cmdlet toostop hello gateway.</span></span>
2. <span data-ttu-id="0ec66-191">Använd hello `Remove-AzureRmApplicationGateway` cmdlet tooremove hello gateway.</span><span class="sxs-lookup"><span data-stu-id="0ec66-191">Use hello `Remove-AzureRmApplicationGateway` cmdlet tooremove hello gateway.</span></span>
3. <span data-ttu-id="0ec66-192">Kontrollera att hello-gateway har tagits bort med hjälp av hello `Get-AzureApplicationGateway` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="0ec66-192">Verify that hello gateway has been removed by using hello `Get-AzureApplicationGateway` cmdlet.</span></span>

### <a name="step-1"></a><span data-ttu-id="0ec66-193">Steg 1</span><span class="sxs-lookup"><span data-stu-id="0ec66-193">Step 1</span></span>

<span data-ttu-id="0ec66-194">Hämta hello programobjektet gateway och associera den tooa variabeln ”$getgw”.</span><span class="sxs-lookup"><span data-stu-id="0ec66-194">Get hello application gateway object and associate it tooa variable "$getgw".</span></span>

```powershell
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

### <a name="step-2"></a><span data-ttu-id="0ec66-195">Steg 2</span><span class="sxs-lookup"><span data-stu-id="0ec66-195">Step 2</span></span>

<span data-ttu-id="0ec66-196">Använd `Stop-AzureRmApplicationGateway` toostop hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="0ec66-196">Use `Stop-AzureRmApplicationGateway` toostop hello application gateway.</span></span> <span data-ttu-id="0ec66-197">Det här exemplet visar hello `Stop-AzureRmApplicationGateway` cmdlet på hello första rad, följt av hello utdata.</span><span class="sxs-lookup"><span data-stu-id="0ec66-197">This sample shows hello `Stop-AzureRmApplicationGateway` cmdlet on hello first line, followed by hello output.</span></span>

```powershell
Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  
```

```
VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8
```

<span data-ttu-id="0ec66-198">När hello Programgateway är i ett stoppat tillstånd, använder hello `Remove-AzureRmApplicationGateway` cmdlet tooremove hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0ec66-198">Once hello application gateway is in a stopped state, use hello `Remove-AzureRmApplicationGateway` cmdlet tooremove hello service.</span></span>

```powershell
Remove-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force
```

```
VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301
```

> [!NOTE]
> <span data-ttu-id="0ec66-199">Hej **-tvinga** växel kan vara används toosuppress hello ta bort bekräftelsemeddelande.</span><span class="sxs-lookup"><span data-stu-id="0ec66-199">hello **-force** switch can be used toosuppress hello remove confirmation message.</span></span>

<span data-ttu-id="0ec66-200">tooverify som hello tjänsten har tagits bort kan du använda hello `Get-AzureRmApplicationGateway` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="0ec66-200">tooverify that hello service has been removed, you can use hello `Get-AzureRmApplicationGateway` cmdlet.</span></span> <span data-ttu-id="0ec66-201">Det här steget är inte obligatoriskt.</span><span class="sxs-lookup"><span data-stu-id="0ec66-201">This step is not required.</span></span>

```powershell
Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: hello gateway does not exist.
```

## <a name="next-steps"></a><span data-ttu-id="0ec66-202">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0ec66-202">Next steps</span></span>

<span data-ttu-id="0ec66-203">Om du vill tooconfigure SSL-avlastning, se [konfigurera en Programgateway för SSL-avlastning](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="0ec66-203">If you want tooconfigure SSL offload, see [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="0ec66-204">Om du vill tooconfigure ett program gateway toouse med en ILB finns [skapa en Programgateway med en intern belastningsutjämnare (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="0ec66-204">If you want tooconfigure an application gateway toouse with an ILB, see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="0ec66-205">Om du vill ha mer information om belastningsutjämningsalternativ i allmänhet läser du:</span><span class="sxs-lookup"><span data-stu-id="0ec66-205">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="0ec66-206">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="0ec66-206">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="0ec66-207">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="0ec66-207">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

