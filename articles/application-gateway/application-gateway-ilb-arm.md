---
title: "Azure Application Gateway med intern belastningsutjämnare - PowerShell | Microsoft Docs"
description: "Den här sidan innehåller anvisningar för hur du skapar, konfigurerar, startar och tar bort en Azure-programgateway med en intern belastningsutjämnare (ILB) med hjälp av Azure Resource Manager"
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
ms.openlocfilehash: d218eab7e9f124e4825a8a781b4eeb0dcca58b4a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb-by-using-azure-resource-manager"></a><span data-ttu-id="49225-103">Skapa en programgateway med en intern belastningsutjämnare (ILB) med hjälp av Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="49225-103">Create an application gateway with an internal load balancer (ILB) by using Azure Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="49225-104">PowerShell och den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="49225-104">Azure Classic PowerShell</span></span>](application-gateway-ilb.md)
> * [<span data-ttu-id="49225-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="49225-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ilb-arm.md)

<span data-ttu-id="49225-106">Azure Application Gateway kan konfigureras med en Internetuppkopplad VIP eller med en intern slutpunkt som inte är exponerad för Internet, även kallad en ILB-slutpunkt (intern belastningsutjämnare).</span><span class="sxs-lookup"><span data-stu-id="49225-106">Azure Application Gateway can be configured with an Internet-facing VIP or with an internal endpoint that is not exposed to the Internet, also known as an internal load balancer (ILB) endpoint.</span></span> <span data-ttu-id="49225-107">Det kan vara praktiskt att konfigurera gatewayen med en ILB för interna affärsprogram som inte är exponerade för Internet.</span><span class="sxs-lookup"><span data-stu-id="49225-107">Configuring the gateway with an ILB is useful for internal line-of-business applications that are not exposed to the Internet.</span></span> <span data-ttu-id="49225-108">Det är också användbart för tjänster och nivåer i ett affärsprogram med flera nivåer som finns vid en säkerhetsgräns som inte är exponerad för Internet men som fortfarande kräver distribution med resursallokering (round-robin), sessionsvaraktighet eller SSL-avslut (Secure Sockets Layer).</span><span class="sxs-lookup"><span data-stu-id="49225-108">It's also useful for services and tiers within a multi-tier application that sit in a security boundary that is not exposed to the Internet but still require round-robin load distribution, session stickiness, or Secure Sockets Layer (SSL) termination.</span></span>

<span data-ttu-id="49225-109">Den här artikeln beskriver steg för steg hur du konfigurerar en programgateway med en ILB.</span><span class="sxs-lookup"><span data-stu-id="49225-109">This article walks you through the steps to configure an application gateway with an ILB.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="49225-110">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="49225-110">Before you begin</span></span>

1. <span data-ttu-id="49225-111">Installera den senaste versionen av Azure PowerShell-cmdlets med hjälp av installationsprogrammet för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="49225-111">Install the latest version of the Azure PowerShell cmdlets by using the Web Platform Installer.</span></span> <span data-ttu-id="49225-112">Du kan hämta och installera den senaste versionen från avsnittet om **Windows PowerShell** på [hämtningssidan](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="49225-112">You can download and install the latest version from the **Windows PowerShell** section of the [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="49225-113">Du ska skapa ett virtuellt nätverk och ett undernät för Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="49225-113">You create a virtual network and a subnet for Application Gateway.</span></span> <span data-ttu-id="49225-114">Kontrollera att inga virtuella datorer eller molndistributioner använder undernätet.</span><span class="sxs-lookup"><span data-stu-id="49225-114">Make sure that no virtual machines or cloud deployments are using the subnet.</span></span> <span data-ttu-id="49225-115">Application Gateway måste vara fristående i ett virtuellt nätverks undernät.</span><span class="sxs-lookup"><span data-stu-id="49225-115">Application Gateway must be by itself in a virtual network subnet.</span></span>
3. <span data-ttu-id="49225-116">De servrar som du konfigurerar för användning av programgatewayen måste finnas i det virtuella nätverket eller ha slutpunkter som skapats där eller tilldelats en offentlig IP-/VIP-adress.</span><span class="sxs-lookup"><span data-stu-id="49225-116">The servers that you configure to use the application gateway must exist or have their endpoints created either in the virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-to-create-an-application-gateway"></a><span data-ttu-id="49225-117">Vad krävs för att skapa en programgateway?</span><span class="sxs-lookup"><span data-stu-id="49225-117">What is required to create an application gateway?</span></span>

* <span data-ttu-id="49225-118">**Backend-serverpool:** Listan med IP-adresser för backend-servrarna.</span><span class="sxs-lookup"><span data-stu-id="49225-118">**Back-end server pool:** The list of IP addresses of the back-end servers.</span></span> <span data-ttu-id="49225-119">IP-adresserna som anges måste antingen höra till det virtuella nätverket men i ett annat undernät för programgatewayen eller vara en offentlig IP/VIP.</span><span class="sxs-lookup"><span data-stu-id="49225-119">The IP addresses listed should either belong to the virtual network but in a different subnet for the application gateway or should be a public IP/VIP.</span></span>
* <span data-ttu-id="49225-120">**Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet.</span><span class="sxs-lookup"><span data-stu-id="49225-120">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="49225-121">Dessa inställningar är knutna till en pool och tillämpas på alla servrar i poolen.</span><span class="sxs-lookup"><span data-stu-id="49225-121">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="49225-122">**Frontend-port:** Den här porten är den offentliga porten som är öppen på programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="49225-122">**Front-end port:** This port is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="49225-123">Trafiken kommer till den här porten och omdirigeras till en av backend-servrarna.</span><span class="sxs-lookup"><span data-stu-id="49225-123">Traffic hits this port, and then gets redirected to one of the back-end servers.</span></span>
* <span data-ttu-id="49225-124">**Lyssnare:** Lyssnaren har en frontend-port, ett protokoll (Http eller Https; dessa är skiftlägeskänsliga) och SSL-certifikatnamnet (om du konfigurerar SSL-avlastning).</span><span class="sxs-lookup"><span data-stu-id="49225-124">**Listener:** The listener has a front-end port, a protocol (Http or Https, these are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="49225-125">**Regel:** Regeln binder lyssnaren och backend-serverpoolen och definierar vilken backend-serverpool som trafiken ska dirigeras till när den når en viss lyssnare.</span><span class="sxs-lookup"><span data-stu-id="49225-125">**Rule:** The rule binds the listener and the back-end server pool and defines which back-end server pool the traffic should be directed to when it hits a particular listener.</span></span> <span data-ttu-id="49225-126">För närvarande stöds endast regeln *basic*.</span><span class="sxs-lookup"><span data-stu-id="49225-126">Currently, only the *basic* rule is supported.</span></span> <span data-ttu-id="49225-127">Regeln *basic* använder belastningsutjämning med resursallokering.</span><span class="sxs-lookup"><span data-stu-id="49225-127">The *basic* rule is round-robin load distribution.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="49225-128">Skapa en programgateway</span><span class="sxs-lookup"><span data-stu-id="49225-128">Create an application gateway</span></span>

<span data-ttu-id="49225-129">Skillnaden mellan att använda den klassiska Azure-portalen och Azure Resource Manager är i vilken ordning du skapar programgatewayen och de objekt som ska konfigureras.</span><span class="sxs-lookup"><span data-stu-id="49225-129">The difference between using Azure Classic and Azure Resource Manager is the order in which you create the application gateway and the items that need to be configured.</span></span>
<span data-ttu-id="49225-130">Med Resource Manager konfigureras alla objekt som bildar en programgateway separat och sätts sedan ihop för att skapa en programgatewayresurs.</span><span class="sxs-lookup"><span data-stu-id="49225-130">With Resource Manager, all items that make an application gateway is configured individually and then put together to create the application gateway resource.</span></span>

<span data-ttu-id="49225-131">Här följer de steg som krävs för att skapa en programgateway:</span><span class="sxs-lookup"><span data-stu-id="49225-131">Here are the steps that are needed to create an application gateway:</span></span>

1. <span data-ttu-id="49225-132">Skapa en resursgrupp för Resource Manager</span><span class="sxs-lookup"><span data-stu-id="49225-132">Create a resource group for Resource Manager</span></span>
2. <span data-ttu-id="49225-133">Skapa ett virtuellt nätverk och ett undernät för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="49225-133">Create a virtual network and a subnet for the application gateway</span></span>
3. <span data-ttu-id="49225-134">Skapa ett konfigurationsobjekt för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="49225-134">Create an application gateway configuration object</span></span>
4. <span data-ttu-id="49225-135">Skapa en resurs för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="49225-135">Create an application gateway resource</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="49225-136">Skapa en resursgrupp för Resource Manager</span><span class="sxs-lookup"><span data-stu-id="49225-136">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="49225-137">Glöm inte att byta PowerShell-läge så att du kan använda cmdlets för Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="49225-137">Make sure that you switch PowerShell mode to use the Azure Resource Manager cmdlets.</span></span> <span data-ttu-id="49225-138">Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="49225-138">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="49225-139">Steg 1</span><span class="sxs-lookup"><span data-stu-id="49225-139">Step 1</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a><span data-ttu-id="49225-140">Steg 2</span><span class="sxs-lookup"><span data-stu-id="49225-140">Step 2</span></span>

<span data-ttu-id="49225-141">Kontrollera prenumerationerna för kontot.</span><span class="sxs-lookup"><span data-stu-id="49225-141">Check the subscriptions for the account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="49225-142">Du ombeds att autentisera dig med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="49225-142">You are prompted to authenticate with your credentials.</span></span>

### <a name="step-3"></a><span data-ttu-id="49225-143">Steg 3</span><span class="sxs-lookup"><span data-stu-id="49225-143">Step 3</span></span>

<span data-ttu-id="49225-144">Välj vilka av dina Azure-prenumerationer som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="49225-144">Choose which of your Azure subscriptions to use.</span></span>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a><span data-ttu-id="49225-145">Steg 4</span><span class="sxs-lookup"><span data-stu-id="49225-145">Step 4</span></span>

<span data-ttu-id="49225-146">Skapa en ny resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp).</span><span class="sxs-lookup"><span data-stu-id="49225-146">Create a new resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-rg -location "West US"
```

<span data-ttu-id="49225-147">Azure Resource Manager kräver att alla resursgrupper definierar en plats.</span><span class="sxs-lookup"><span data-stu-id="49225-147">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="49225-148">Den här platsen används som standardplats för resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="49225-148">This is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="49225-149">Se till att alla kommandon du använder för att skapa en programgateway använder samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="49225-149">Make sure that all commands to create an application gateway uses the same resource group.</span></span>

<span data-ttu-id="49225-150">I föregående exempel kan vi skapat en resursgrupp med namnet ”appgw rg” och plats ”West US”.</span><span class="sxs-lookup"><span data-stu-id="49225-150">In the preceding example, we created a resource group called "appgw-rg" and location "West US".</span></span>

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a><span data-ttu-id="49225-151">Skapa ett virtuellt nätverk och ett undernät för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="49225-151">Create a virtual network and a subnet for the application gateway</span></span>

<span data-ttu-id="49225-152">Följande exempel illustrerar hur du skapar ett virtuellt nätverk med hjälp av Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="49225-152">The following example shows how to create a virtual network by using Resource Manager:</span></span>

### <a name="step-1"></a><span data-ttu-id="49225-153">Steg 1</span><span class="sxs-lookup"><span data-stu-id="49225-153">Step 1</span></span>

```powershell
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

<span data-ttu-id="49225-154">Det här steget tilldelas en undernät variabel som används för att skapa ett virtuellt nätverk adressintervallet 10.0.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="49225-154">This step assigns the address range 10.0.0.0/24 to a subnet variable to be used to create a virtual network.</span></span>

### <a name="step-2"></a><span data-ttu-id="49225-155">Steg 2</span><span class="sxs-lookup"><span data-stu-id="49225-155">Step 2</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig
```

<span data-ttu-id="49225-156">Det här steget skapar ett virtuellt nätverk med namnet ”appgwvnet” i resursen grupp ”appgw-rg” för regionen USA, västra med prefixet 10.0.0.0/16 med undernätet 10.0.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="49225-156">This step creates a virtual network named "appgwvnet" in resource group "appgw-rg" for the West US region using the prefix 10.0.0.0/16 with subnet 10.0.0.0/24.</span></span>

### <a name="step-3"></a><span data-ttu-id="49225-157">Steg 3</span><span class="sxs-lookup"><span data-stu-id="49225-157">Step 3</span></span>

```powershell
$subnet = $vnet.subnets[0]
```

<span data-ttu-id="49225-158">Det här steget tilldelas variabeln $subnet i nästa steg undernätets objekt.</span><span class="sxs-lookup"><span data-stu-id="49225-158">This step assigns the subnet object to variable $subnet for the next steps.</span></span>

## <a name="create-an-application-gateway-configuration-object"></a><span data-ttu-id="49225-159">Skapa ett konfigurationsobjekt för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="49225-159">Create an application gateway configuration object</span></span>

### <a name="step-1"></a><span data-ttu-id="49225-160">Steg 1</span><span class="sxs-lookup"><span data-stu-id="49225-160">Step 1</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

<span data-ttu-id="49225-161">Det här steget skapar ett program gateway IP-konfiguration med namnet ”gatewayIP01”.</span><span class="sxs-lookup"><span data-stu-id="49225-161">This step creates an application gateway IP configuration named "gatewayIP01".</span></span> <span data-ttu-id="49225-162">När Application Gateway startar hämtar den en IP-adress från det konfigurerade undernätet och dirigerar nätverkstrafik till IP-adresserna i backend-IP-poolen.</span><span class="sxs-lookup"><span data-stu-id="49225-162">When Application Gateway starts, it picks up an IP address from the subnet configured and route network traffic to the IP addresses in the back-end IP pool.</span></span> <span data-ttu-id="49225-163">Tänk på att varje instans använder en IP-adress.</span><span class="sxs-lookup"><span data-stu-id="49225-163">Keep in mind that each instance takes one IP address.</span></span>

### <a name="step-2"></a><span data-ttu-id="49225-164">Steg 2</span><span class="sxs-lookup"><span data-stu-id="49225-164">Step 2</span></span>

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.1.1.8,10.1.1.9,10.1.1.10
```

<span data-ttu-id="49225-165">Det här steget konfigurerar backend-IP-adresspool med namnet ”pool01” med IP-adresser ”10.1.1.8, 10.1.1.9, 10.1.1.10”.</span><span class="sxs-lookup"><span data-stu-id="49225-165">This step configures the back-end IP address pool named "pool01" with IP addresses "10.1.1.8, 10.1.1.9, 10.1.1.10".</span></span> <span data-ttu-id="49225-166">Det här är IP-adresserna som tar emot nätverkstrafiken som kommer från frontend-IP-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="49225-166">Those are the IP addresses that receive the network traffic that comes from the front-end IP endpoint.</span></span> <span data-ttu-id="49225-167">Du ersätter de omnämnda IP-adresserna och lägger till ditt eget programs IP-adresslutpunkter.</span><span class="sxs-lookup"><span data-stu-id="49225-167">You replace the preceding IP addresses to add your own application IP address endpoints.</span></span>

### <a name="step-3"></a><span data-ttu-id="49225-168">Steg 3</span><span class="sxs-lookup"><span data-stu-id="49225-168">Step 3</span></span>

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

<span data-ttu-id="49225-169">Det här steget konfigurerar programmet gateway inställningen ”poolsetting01” för den belastningsutjämnade trafik i backend-poolen.</span><span class="sxs-lookup"><span data-stu-id="49225-169">This step configures application gateway setting "poolsetting01" for the load balanced network traffic in the back-end pool.</span></span>

### <a name="step-4"></a><span data-ttu-id="49225-170">Steg 4</span><span class="sxs-lookup"><span data-stu-id="49225-170">Step 4</span></span>

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80
```

<span data-ttu-id="49225-171">Det här steget konfigurerar frontend IP-port med namnet ”frontendport01” för ILB.</span><span class="sxs-lookup"><span data-stu-id="49225-171">This step configures the front-end IP port named "frontendport01" for the ILB.</span></span>

### <a name="step-5"></a><span data-ttu-id="49225-172">Steg 5</span><span class="sxs-lookup"><span data-stu-id="49225-172">Step 5</span></span>

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet
```

<span data-ttu-id="49225-173">Det här steget skapar frontend IP-konfiguration som kallas ”fipconfig01” och associerar den med en privat IP-adress från det aktuella undernätet för virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="49225-173">This step creates the front-end IP configuration called "fipconfig01" and associates it with a private IP from the current virtual network subnet.</span></span>

### <a name="step-6"></a><span data-ttu-id="49225-174">Steg 6</span><span class="sxs-lookup"><span data-stu-id="49225-174">Step 6</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp
```

<span data-ttu-id="49225-175">Det här steget skapar lyssnaren kallas ”listener01” och associerar frontend-port till frontend IP-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="49225-175">This step creates the listener called "listener01" and associates the front-end port to the front-end IP configuration.</span></span>

### <a name="step-7"></a><span data-ttu-id="49225-176">Steg 7</span><span class="sxs-lookup"><span data-stu-id="49225-176">Step 7</span></span>

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

<span data-ttu-id="49225-177">Det här steget skapar routning belastningsutjämningsregeln kallas ”rule01” som konfigurerar belastningsutjämning belastningen.</span><span class="sxs-lookup"><span data-stu-id="49225-177">This step creates the load balancer routing rule called "rule01" that configures the load balancer behavior.</span></span>

### <a name="step-8"></a><span data-ttu-id="49225-178">Steg 8</span><span class="sxs-lookup"><span data-stu-id="49225-178">Step 8</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

<span data-ttu-id="49225-179">Det här steget konfigurerar instansstorleken för programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="49225-179">This step configures the instance size of the application gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="49225-180">Standardvärdet för *InstanceCount* är 2, och det högsta värdet är 10.</span><span class="sxs-lookup"><span data-stu-id="49225-180">The default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="49225-181">Standardvärdet för *GatewaySize* är Medium.</span><span class="sxs-lookup"><span data-stu-id="49225-181">The default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="49225-182">Du kan välja mellan Standard_Small, Standard_Medium och Standard_Large.</span><span class="sxs-lookup"><span data-stu-id="49225-182">You can choose between Standard_Small, Standard_Medium, and Standard_Large.</span></span>

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a><span data-ttu-id="49225-183">Skapa en programgateway med hjälp av New-AzureApplicationGateway</span><span class="sxs-lookup"><span data-stu-id="49225-183">Create an application gateway by using New-AzureApplicationGateway</span></span>

<span data-ttu-id="49225-184">Skapar en Programgateway med alla konfigurationsobjekt från föregående steg.</span><span class="sxs-lookup"><span data-stu-id="49225-184">Creates an application gateway with all configuration items from the preceding steps.</span></span> <span data-ttu-id="49225-185">I det här exemplet heter programgatewayen ”appgwtest”.</span><span class="sxs-lookup"><span data-stu-id="49225-185">In this example, the application gateway is called "appgwtest".</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

<span data-ttu-id="49225-186">Det här steget skapar en Programgateway med alla konfigurationsobjekt från föregående steg.</span><span class="sxs-lookup"><span data-stu-id="49225-186">This step creates an application gateway with all configuration items from the preceding steps.</span></span> <span data-ttu-id="49225-187">I det här exemplet heter programgatewayen ”appgwtest”.</span><span class="sxs-lookup"><span data-stu-id="49225-187">In the example, the application gateway is called "appgwtest".</span></span>

## <a name="delete-an-application-gateway"></a><span data-ttu-id="49225-188">Ta bort en programgateway</span><span class="sxs-lookup"><span data-stu-id="49225-188">Delete an application gateway</span></span>

<span data-ttu-id="49225-189">Om du vill ta bort en Programgateway, måste du göra följande i ordning:</span><span class="sxs-lookup"><span data-stu-id="49225-189">To delete an application gateway, you need to do the following steps in order:</span></span>

1. <span data-ttu-id="49225-190">Stoppa gatewayen med hjälp av cmdleten `Stop-AzureRmApplicationGateway`.</span><span class="sxs-lookup"><span data-stu-id="49225-190">Use the `Stop-AzureRmApplicationGateway` cmdlet to stop the gateway.</span></span>
2. <span data-ttu-id="49225-191">Ta bort gatewayen med hjälp av cmdleten `Remove-AzureRmApplicationGateway`.</span><span class="sxs-lookup"><span data-stu-id="49225-191">Use the `Remove-AzureRmApplicationGateway` cmdlet to remove the gateway.</span></span>
3. <span data-ttu-id="49225-192">Kontrollera att gatewayen har tagits bort med hjälp av cmdleten `Get-AzureApplicationGateway`.</span><span class="sxs-lookup"><span data-stu-id="49225-192">Verify that the gateway has been removed by using the `Get-AzureApplicationGateway` cmdlet.</span></span>

### <a name="step-1"></a><span data-ttu-id="49225-193">Steg 1</span><span class="sxs-lookup"><span data-stu-id="49225-193">Step 1</span></span>

<span data-ttu-id="49225-194">Hämta objektet för programgatewayen och associera det med variabeln ”$getgw”.</span><span class="sxs-lookup"><span data-stu-id="49225-194">Get the application gateway object and associate it to a variable "$getgw".</span></span>

```powershell
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

### <a name="step-2"></a><span data-ttu-id="49225-195">Steg 2</span><span class="sxs-lookup"><span data-stu-id="49225-195">Step 2</span></span>

<span data-ttu-id="49225-196">Använd `Stop-AzureRmApplicationGateway` för att stoppa programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="49225-196">Use `Stop-AzureRmApplicationGateway` to stop the application gateway.</span></span> <span data-ttu-id="49225-197">Det här exemplet visas den `Stop-AzureRmApplicationGateway` cmdleten på den första raden, följt av utdata.</span><span class="sxs-lookup"><span data-stu-id="49225-197">This sample shows the `Stop-AzureRmApplicationGateway` cmdlet on the first line, followed by the output.</span></span>

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

<span data-ttu-id="49225-198">När programgatewayen är i ett stoppat läge kan du använda cmdleten `Remove-AzureRmApplicationGateway` för att ta bort tjänsten.</span><span class="sxs-lookup"><span data-stu-id="49225-198">Once the application gateway is in a stopped state, use the `Remove-AzureRmApplicationGateway` cmdlet to remove the service.</span></span>

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
> <span data-ttu-id="49225-199">Du kan använda växeln **-force** om du inte vill att några bekräftelsemeddelanden ska visas.</span><span class="sxs-lookup"><span data-stu-id="49225-199">The **-force** switch can be used to suppress the remove confirmation message.</span></span>

<span data-ttu-id="49225-200">Du kan kontrollera att tjänsten har tagits bort genom att använda cmdleten `Get-AzureRmApplicationGateway`.</span><span class="sxs-lookup"><span data-stu-id="49225-200">To verify that the service has been removed, you can use the `Get-AzureRmApplicationGateway` cmdlet.</span></span> <span data-ttu-id="49225-201">Det här steget är inte obligatoriskt.</span><span class="sxs-lookup"><span data-stu-id="49225-201">This step is not required.</span></span>

```powershell
Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
```

## <a name="next-steps"></a><span data-ttu-id="49225-202">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="49225-202">Next steps</span></span>

<span data-ttu-id="49225-203">Om du vill konfigurera SSL-avlastning läser du [Konfigurera en programgateway för SSL-avlastning](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="49225-203">If you want to configure SSL offload, see [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="49225-204">Om du vill konfigurera en programgateway för användning med en intern belastningsutjämnare läser du [Skapa en programgateway med en intern belastningsutjämnare (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="49225-204">If you want to configure an application gateway to use with an ILB, see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="49225-205">Om du vill ha mer information om belastningsutjämningsalternativ i allmänhet läser du:</span><span class="sxs-lookup"><span data-stu-id="49225-205">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="49225-206">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="49225-206">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="49225-207">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="49225-207">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

