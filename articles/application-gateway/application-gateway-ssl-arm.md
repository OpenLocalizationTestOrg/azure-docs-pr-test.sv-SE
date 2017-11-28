---
title: aaaConfigure SSL-avlastning - Azure Application Gateway - PowerShell | Microsoft Docs
description: "Den här sidan finns instruktioner toocreate en Programgateway med SSL avlasta med hjälp av Azure Resource Manager"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 3c3681e0-f928-4682-9d97-567f8e278e13
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: gwallace
ms.openlocfilehash: c2855d8d3caaa97ec05475c67ff0f8dce72ef2a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a><span data-ttu-id="0ad72-103">Konfigurera en programgateway för SSL-avlastning med hjälp av Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0ad72-103">Configure an application gateway for SSL offload by using Azure Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="0ad72-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0ad72-104">Azure portal</span></span>](application-gateway-ssl-portal.md)
> * [<span data-ttu-id="0ad72-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0ad72-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ssl-arm.md)
> * [<span data-ttu-id="0ad72-106">PowerShell och den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0ad72-106">Azure Classic PowerShell</span></span>](application-gateway-ssl.md)
> * [<span data-ttu-id="0ad72-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="0ad72-107">Azure CLI 2.0</span></span>](application-gateway-ssl-cli.md)

<span data-ttu-id="0ad72-108">Azure Application Gateway kan vara konfigurerade tooterminate hello Secure Sockets Layer (SSL)-session på hello gateway tooavoid kostsamma SSL dekryptering uppgifter toohappen på hello webbservergrupp.</span><span class="sxs-lookup"><span data-stu-id="0ad72-108">Azure Application Gateway can be configured tooterminate hello Secure Sockets Layer (SSL) session at hello gateway tooavoid costly SSL decryption tasks toohappen at hello web farm.</span></span> <span data-ttu-id="0ad72-109">SSL-avlastning förenklar också hello front-end-serverinstallation och hantering av hello webbprogram.</span><span class="sxs-lookup"><span data-stu-id="0ad72-109">SSL offload also simplifies hello front-end server setup and management of hello web application.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="0ad72-110">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="0ad72-110">Before you begin</span></span>

1. <span data-ttu-id="0ad72-111">Installera hello senaste versionen av hello Azure PowerShell-cmdlets med hello installationsprogram för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="0ad72-111">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="0ad72-112">Du kan hämta och installera hello senaste versionen från hello **Windows PowerShell** avsnitt i hello [Nedladdningssida](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="0ad72-112">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="0ad72-113">Du skapar ett virtuellt nätverk och ett undernät för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="0ad72-113">You create a virtual network and a subnet for hello application gateway.</span></span> <span data-ttu-id="0ad72-114">Se till att inga virtuella datorer eller molndistributioner använder hello undernät.</span><span class="sxs-lookup"><span data-stu-id="0ad72-114">Make sure that no virtual machines or cloud deployments are using hello subnet.</span></span> <span data-ttu-id="0ad72-115">Application Gateway måste vara fristående i ett virtuellt nätverks undernät.</span><span class="sxs-lookup"><span data-stu-id="0ad72-115">Application Gateway must be by itself in a virtual network subnet.</span></span>
3. <span data-ttu-id="0ad72-116">hello-servrar som du konfigurerar toouse hello Programgateway måste finnas eller har skapat sina slutpunkter i hello virtuellt nätverk eller med en offentlig IP-adress/VIP tilldelad.</span><span class="sxs-lookup"><span data-stu-id="0ad72-116">hello servers you configure toouse hello application gateway must exist or have their endpoints created either in hello virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-toocreate-an-application-gateway"></a><span data-ttu-id="0ad72-117">Vad är obligatoriska toocreate en Programgateway?</span><span class="sxs-lookup"><span data-stu-id="0ad72-117">What is required toocreate an application gateway?</span></span>

* <span data-ttu-id="0ad72-118">**Backend-serverpoolen:** hello lista över IP-adresser för hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="0ad72-118">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="0ad72-119">hello IP-adresser som anges antingen ska tillhöra toohello undernät för virtuellt nätverk eller ska vara en offentlig IP-adress/VIP.</span><span class="sxs-lookup"><span data-stu-id="0ad72-119">hello IP addresses listed should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="0ad72-120">**Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet.</span><span class="sxs-lookup"><span data-stu-id="0ad72-120">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="0ad72-121">De här inställningarna är bundet tooa poolen och tillämpade tooall servrar inom hello poolen.</span><span class="sxs-lookup"><span data-stu-id="0ad72-121">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="0ad72-122">**Frontend-port:** den här porten är hello offentliga som öppnas på hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="0ad72-122">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="0ad72-123">Trafik träffar den här porten och sedan hämtar omdirigeras tooone hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="0ad72-123">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="0ad72-124">**Lyssnare:** hello-lyssnare har en frontend-port, ett protokoll (Http eller Https inställningarna är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning).</span><span class="sxs-lookup"><span data-stu-id="0ad72-124">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these settings are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="0ad72-125">**Regel:** hello regeln Binder hello-lyssnare och hello backend-serverpoolen och definierar vilken backend-server pool hello trafik ska vara riktad toowhen den når en viss lyssnare.</span><span class="sxs-lookup"><span data-stu-id="0ad72-125">**Rule:** hello rule binds hello listener and hello back-end server pool and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span> <span data-ttu-id="0ad72-126">För närvarande endast hello *grundläggande* regeln stöds.</span><span class="sxs-lookup"><span data-stu-id="0ad72-126">Currently, only hello *basic* rule is supported.</span></span> <span data-ttu-id="0ad72-127">Hej *grundläggande* regeln är resursallokering belastningsdistribution.</span><span class="sxs-lookup"><span data-stu-id="0ad72-127">hello *basic* rule is round-robin load distribution.</span></span>

<span data-ttu-id="0ad72-128">**Information om ytterligare konfiguration**</span><span class="sxs-lookup"><span data-stu-id="0ad72-128">**Additional configuration notes**</span></span>

<span data-ttu-id="0ad72-129">Konfiguration för SSL-certifikat, hello-protokollet i **HttpListener** bör ändra för*Https* (skiftlägeskänsligt).</span><span class="sxs-lookup"><span data-stu-id="0ad72-129">For SSL certificates configuration, hello protocol in **HttpListener** should change too*Https* (case sensitive).</span></span> <span data-ttu-id="0ad72-130">Hej **SslCertificate** läggs elementet för**HttpListener** med hello variabelvärde som konfigurerats för hello SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="0ad72-130">hello **SslCertificate** element is added too**HttpListener** with hello variable value configured for hello SSL certificate.</span></span> <span data-ttu-id="0ad72-131">hello frontend-porten ska vara uppdaterade too443.</span><span class="sxs-lookup"><span data-stu-id="0ad72-131">hello front-end port should be updated too443.</span></span>

<span data-ttu-id="0ad72-132">**tooenable cookie-baserad tillhörighet**: en Programgateway kan vara konfigurerade tooensure att en begäran från en klientsession är alltid dirigerad toohello samma virtuella dator i hello webbservergrupp.</span><span class="sxs-lookup"><span data-stu-id="0ad72-132">**tooenable cookie-based affinity**: An application gateway can be configured tooensure that a request from a client session is always directed toohello same VM in hello web farm.</span></span> <span data-ttu-id="0ad72-133">Det här scenariot sker genom injektion av en sessions-cookie som tillåter trafik för hello gateway toodirect på lämpligt sätt.</span><span class="sxs-lookup"><span data-stu-id="0ad72-133">This scenario is done by injection of a session cookie that allows hello gateway toodirect traffic appropriately.</span></span> <span data-ttu-id="0ad72-134">Ange tooenable cookie-baserad tillhörighet **CookieBasedAffinity** för*aktiverad* i hello **BackendHttpSettings** element.</span><span class="sxs-lookup"><span data-stu-id="0ad72-134">tooenable cookie-based affinity, set **CookieBasedAffinity** too*Enabled* in hello **BackendHttpSettings** element.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="0ad72-135">Skapa en programgateway</span><span class="sxs-lookup"><span data-stu-id="0ad72-135">Create an application gateway</span></span>

<span data-ttu-id="0ad72-136">hello skillnaden mellan att använda hello Azure klassiska distributionsmodellen och Azure Resource Manager är hello order som du skapar ett program gateway och hello objekt behöver toobe konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="0ad72-136">hello difference between using hello Azure Classic deployment model and Azure Resource Manager is hello order that you create an application gateway and hello items that need toobe configured.</span></span>

<span data-ttu-id="0ad72-137">Med Resource Manager kan alla komponenter i en Programgateway konfigureras individuellt och publicera sedan tillsammans toocreate gatewayresursen ett program.</span><span class="sxs-lookup"><span data-stu-id="0ad72-137">With Resource Manager, all components of an application gateway are configured individually and then put together toocreate an application gateway resource.</span></span>

<span data-ttu-id="0ad72-138">Här följer hello steg som krävs för toocreate en Programgateway:</span><span class="sxs-lookup"><span data-stu-id="0ad72-138">Here are hello steps needed toocreate an application gateway:</span></span>

1. <span data-ttu-id="0ad72-139">Skapa en resursgrupp för Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0ad72-139">Create a resource group for Resource Manager</span></span>
2. <span data-ttu-id="0ad72-140">Skapa virtuella nätverk, undernät och offentliga IP för hello Programgateway</span><span class="sxs-lookup"><span data-stu-id="0ad72-140">Create virtual network, subnet, and public IP for hello application gateway</span></span>
3. <span data-ttu-id="0ad72-141">Skapa ett konfigurationsobjekt för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="0ad72-141">Create an application gateway configuration object</span></span>
4. <span data-ttu-id="0ad72-142">Skapa en resurs för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="0ad72-142">Create an application gateway resource</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="0ad72-143">Skapa en resursgrupp för Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0ad72-143">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="0ad72-144">Kontrollera att du växla PowerShell läge toouse hello Azure Resource Manager-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="0ad72-144">Make sure that you switch PowerShell mode toouse hello Azure Resource Manager cmdlets.</span></span> <span data-ttu-id="0ad72-145">Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="0ad72-145">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="0ad72-146">Steg 1</span><span class="sxs-lookup"><span data-stu-id="0ad72-146">Step 1</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a><span data-ttu-id="0ad72-147">Steg 2</span><span class="sxs-lookup"><span data-stu-id="0ad72-147">Step 2</span></span>

<span data-ttu-id="0ad72-148">Kontrollera hello prenumerationer för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="0ad72-148">Check hello subscriptions for hello account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="0ad72-149">Du kan ange tooauthenticate med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0ad72-149">You are prompted tooauthenticate with your credentials.</span></span>

### <a name="step-3"></a><span data-ttu-id="0ad72-150">Steg 3</span><span class="sxs-lookup"><span data-stu-id="0ad72-150">Step 3</span></span>

<span data-ttu-id="0ad72-151">Välj vilka av dina Azure-prenumerationer toouse.</span><span class="sxs-lookup"><span data-stu-id="0ad72-151">Choose which of your Azure subscriptions toouse.</span></span>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a><span data-ttu-id="0ad72-152">Steg 4</span><span class="sxs-lookup"><span data-stu-id="0ad72-152">Step 4</span></span>

<span data-ttu-id="0ad72-153">Skapa en resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp).</span><span class="sxs-lookup"><span data-stu-id="0ad72-153">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

<span data-ttu-id="0ad72-154">Azure Resource Manager kräver att alla resursgrupper anger en plats.</span><span class="sxs-lookup"><span data-stu-id="0ad72-154">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="0ad72-155">Den här inställningen används som hello standardplatsen för resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="0ad72-155">This setting is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="0ad72-156">Se till att alla kommandon toocreate använder en Programgateway hello samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="0ad72-156">Make sure that all commands toocreate an application gateway uses hello same resource group.</span></span>

<span data-ttu-id="0ad72-157">Vi har skapat en resursgrupp med namnet i hello-exemplet ovan, **appgw RG** och plats **västra USA**.</span><span class="sxs-lookup"><span data-stu-id="0ad72-157">In hello example above, we created a resource group called **appgw-RG** and location **West US**.</span></span>

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a><span data-ttu-id="0ad72-158">Skapa ett virtuellt nätverk och ett undernät för hello Programgateway</span><span class="sxs-lookup"><span data-stu-id="0ad72-158">Create a virtual network and a subnet for hello application gateway</span></span>

<span data-ttu-id="0ad72-159">följande exempel visar hur hello toocreate ett virtuellt nätverk med hjälp av hanteraren för filserverresurser:</span><span class="sxs-lookup"><span data-stu-id="0ad72-159">hello following example shows how toocreate a virtual network by using Resource Manager:</span></span>

### <a name="step-1"></a><span data-ttu-id="0ad72-160">Steg 1</span><span class="sxs-lookup"><span data-stu-id="0ad72-160">Step 1</span></span>

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

<span data-ttu-id="0ad72-161">Det här exemplet tilldelas hello adressintervallet 10.0.0.0/24 tooa undernät variabeln toobe används toocreate ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="0ad72-161">This sample assigns hello address range 10.0.0.0/24 tooa subnet variable toobe used toocreate a virtual network.</span></span>

### <a name="step-2"></a><span data-ttu-id="0ad72-162">Steg 2</span><span class="sxs-lookup"><span data-stu-id="0ad72-162">Step 2</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

<span data-ttu-id="0ad72-163">Det här exemplet skapar ett virtuellt nätverk med namnet **appgwvnet** i resursgruppen **appgw rg** för hello västra USA region med undernätet 10.0.0.0/24 hello prefixet 10.0.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="0ad72-163">This sample creates a virtual network named **appgwvnet** in resource group **appgw-rg** for hello West US region using hello prefix 10.0.0.0/16 with subnet 10.0.0.0/24.</span></span>

### <a name="step-3"></a><span data-ttu-id="0ad72-164">Steg 3</span><span class="sxs-lookup"><span data-stu-id="0ad72-164">Step 3</span></span>

```powershell
$subnet = $vnet.Subnets[0]
```

<span data-ttu-id="0ad72-165">Det här exemplet tilldelas hello undernät objektet toovariable $subnet hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="0ad72-165">This sample assigns hello subnet object toovariable $subnet for hello next steps.</span></span>

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a><span data-ttu-id="0ad72-166">Skapa en offentlig IP-adress för hello frontend-konfiguration</span><span class="sxs-lookup"><span data-stu-id="0ad72-166">Create a public IP address for hello front-end configuration</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

<span data-ttu-id="0ad72-167">Det här exemplet skapar en offentlig IP-resurs **publicIP01** i resursgruppen **appgw rg** för hello västra USA region.</span><span class="sxs-lookup"><span data-stu-id="0ad72-167">This sample creates a public IP resource **publicIP01** in resource group **appgw-rg** for hello West US region.</span></span>

## <a name="create-an-application-gateway-configuration-object"></a><span data-ttu-id="0ad72-168">Skapa ett konfigurationsobjekt för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="0ad72-168">Create an application gateway configuration object</span></span>

### <a name="step-1"></a><span data-ttu-id="0ad72-169">Steg 1</span><span class="sxs-lookup"><span data-stu-id="0ad72-169">Step 1</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

<span data-ttu-id="0ad72-170">Det här exemplet skapar ett program gateway IP-konfiguration med namnet **gatewayIP01**.</span><span class="sxs-lookup"><span data-stu-id="0ad72-170">This sample creates an application gateway IP configuration named **gatewayIP01**.</span></span> <span data-ttu-id="0ad72-171">När Programgateway startar hämtar en IP-adress från hello-undernät som konfigurerats och dirigera trafik toohello IP-adresser på nätverket i hello backend-IP-adresspool.</span><span class="sxs-lookup"><span data-stu-id="0ad72-171">When Application Gateway starts, it picks up an IP address from hello subnet configured and route network traffic toohello IP addresses in hello back-end IP pool.</span></span> <span data-ttu-id="0ad72-172">Tänk på att varje instans använder en IP-adress.</span><span class="sxs-lookup"><span data-stu-id="0ad72-172">Keep in mind that each instance takes one IP address.</span></span>

### <a name="step-2"></a><span data-ttu-id="0ad72-173">Steg 2</span><span class="sxs-lookup"><span data-stu-id="0ad72-173">Step 2</span></span>

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50
```

<span data-ttu-id="0ad72-174">Det här exemplet konfigurerar hello backend-IP-adresspool med namnet **pool01** med IP-adresser **134.170.185.46**, **134.170.188.221**, **134.170.185.50** .</span><span class="sxs-lookup"><span data-stu-id="0ad72-174">This sample configures hello back-end IP address pool named **pool01** with IP addresses **134.170.185.46**, **134.170.188.221**, **134.170.185.50**.</span></span> <span data-ttu-id="0ad72-175">Dessa värden är hello IP-adresser som tar emot hello nätverkstrafik som kommer från hello frontend IP-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="0ad72-175">Those values are hello IP addresses that receive hello network traffic that comes from hello front-end IP endpoint.</span></span> <span data-ttu-id="0ad72-176">Ersätt hello IP-adresser från hello föregående exempel med hello IP-adresser på din web application slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="0ad72-176">Replace hello IP addresses from hello preceding example with hello IP addresses of your web application endpoints.</span></span>

### <a name="step-3"></a><span data-ttu-id="0ad72-177">Steg 3</span><span class="sxs-lookup"><span data-stu-id="0ad72-177">Step 3</span></span>

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled
```

<span data-ttu-id="0ad72-178">Det här exemplet konfigurerar gateway programinställning **poolsetting01** tooload belastningsutjämnade trafik i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="0ad72-178">This sample configures application gateway setting **poolsetting01** tooload-balanced network traffic in hello back-end pool.</span></span>

### <a name="step-4"></a><span data-ttu-id="0ad72-179">Steg 4</span><span class="sxs-lookup"><span data-stu-id="0ad72-179">Step 4</span></span>

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443
```

<span data-ttu-id="0ad72-180">Det här exemplet konfigurerar hello frontend IP-port med namnet **frontendport01** för hello offentlig IP-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="0ad72-180">This sample configures hello front-end IP port named **frontendport01** for hello public IP endpoint.</span></span>

### <a name="step-5"></a><span data-ttu-id="0ad72-181">Steg 5</span><span class="sxs-lookup"><span data-stu-id="0ad72-181">Step 5</span></span>

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password "<password>"
```

<span data-ttu-id="0ad72-182">Det här exemplet konfigurerar hello-certifikat som används för SSL-anslutning.</span><span class="sxs-lookup"><span data-stu-id="0ad72-182">This sample configures hello certificate used for SSL connection.</span></span> <span data-ttu-id="0ad72-183">hello certifikat måste toobe i PFX-format och hello lösenordet måste innehålla mellan 4 too12 tecken.</span><span class="sxs-lookup"><span data-stu-id="0ad72-183">hello certificate needs toobe in .pfx format, and hello password must be between 4 too12 characters.</span></span>

### <a name="step-6"></a><span data-ttu-id="0ad72-184">Steg 6</span><span class="sxs-lookup"><span data-stu-id="0ad72-184">Step 6</span></span>

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
```

<span data-ttu-id="0ad72-185">Det här exemplet skapar hello frontend IP-konfiguration med namnet **fipconfig01** och associerats hello offentlig IP-adress med hello frontend IP-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="0ad72-185">This sample creates hello front-end IP configuration named **fipconfig01** and associates hello public IP address with hello front-end IP configuration.</span></span>

### <a name="step-7"></a><span data-ttu-id="0ad72-186">Steg 7</span><span class="sxs-lookup"><span data-stu-id="0ad72-186">Step 7</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert
```

<span data-ttu-id="0ad72-187">Det här exemplet skapar hello grupplyssnarens namn **listener01** och associerats hello frontend port toohello frontend IP-konfiguration och certifikat.</span><span class="sxs-lookup"><span data-stu-id="0ad72-187">This sample creates hello listener name **listener01** and associates hello front-end port toohello front-end IP configuration and certificate.</span></span>

### <a name="step-8"></a><span data-ttu-id="0ad72-188">Steg 8</span><span class="sxs-lookup"><span data-stu-id="0ad72-188">Step 8</span></span>

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

<span data-ttu-id="0ad72-189">Det här exemplet skapar hello routning-regel för belastningsutjämning med namnet **rule01** som konfigurerar hello belastningsutjämnaren GUID.</span><span class="sxs-lookup"><span data-stu-id="0ad72-189">This sample creates hello load balancer routing rule named **rule01** that configures hello load balancer behavior.</span></span>

### <a name="step-9"></a><span data-ttu-id="0ad72-190">Steg 9</span><span class="sxs-lookup"><span data-stu-id="0ad72-190">Step 9</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

<span data-ttu-id="0ad72-191">Det här exemplet konfigurerar hello instansstorleken för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="0ad72-191">This sample configures hello instance size of hello application gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="0ad72-192">Hej standardvärdet för *InstanceCount* är 2, med ett maximalt värde 10.</span><span class="sxs-lookup"><span data-stu-id="0ad72-192">hello default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="0ad72-193">Hej standardvärdet för *GatewaySize* är Medium.</span><span class="sxs-lookup"><span data-stu-id="0ad72-193">hello default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="0ad72-194">Du kan välja mellan Standard_Small, Standard_Medium och Standard_Large.</span><span class="sxs-lookup"><span data-stu-id="0ad72-194">You can choose between Standard_Small, Standard_Medium, and Standard_Large.</span></span>

### <a name="step-10"></a><span data-ttu-id="0ad72-195">Steg 10</span><span class="sxs-lookup"><span data-stu-id="0ad72-195">Step 10</span></span>

```powershell
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S
```

<span data-ttu-id="0ad72-196">Det här steget definierar hello SSL princip toouse på hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="0ad72-196">This step defines hello SSL policy toouse on hello application gateway.</span></span> <span data-ttu-id="0ad72-197">Besök [Konfigurera SSL princip versioner och krypteringssviter på Programgateway](application-gateway-configure-ssl-policy-powershell.md) toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="0ad72-197">Visit [Configure SSL policy versions and cipher suites on Application Gateway](application-gateway-configure-ssl-policy-powershell.md) toolearn more.</span></span>

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a><span data-ttu-id="0ad72-198">Skapa en programgateway med hjälp av New-AzureApplicationGateway</span><span class="sxs-lookup"><span data-stu-id="0ad72-198">Create an application gateway by using New-AzureApplicationGateway</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert -SslPolicy $policy
```

<span data-ttu-id="0ad72-199">Det här exemplet skapar en Programgateway med alla konfigurationsobjekt från hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="0ad72-199">This sample creates an application gateway with all configuration items from hello preceding steps.</span></span> <span data-ttu-id="0ad72-200">I exemplet hello hello Programgateway kallas **appgwtest**.</span><span class="sxs-lookup"><span data-stu-id="0ad72-200">In hello example, hello application gateway is called **appgwtest**.</span></span>

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="0ad72-201">Hämta DNS-namn för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="0ad72-201">Get application gateway DNS name</span></span>

<span data-ttu-id="0ad72-202">När du har skapat hello gateway är hello nästa steg tooconfigure hello klientdelen för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="0ad72-202">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="0ad72-203">När du använder en offentlig IP-adress krävs ett dynamiskt tilldelat DNS-namn som inte är användarvänligt.</span><span class="sxs-lookup"><span data-stu-id="0ad72-203">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="0ad72-204">tooensure slutanvändare kan träffa hello Programgateway, en CNAME-post kan vara används toopoint toohello offentlig slutpunkt för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="0ad72-204">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="0ad72-205">[Konfigurera ett eget domännamn i Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="0ad72-205">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="0ad72-206">toodo detta, hämta information om hello Programgateway och dess associerade IP DNS-namn med hjälp av hello PublicIPAddress element bifogade toohello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="0ad72-206">toodo this, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="0ad72-207">hello programmet gateway DNS-namn ska använda toocreate en CNAME-post som pekar hello två web program toothis DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="0ad72-207">hello application gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="0ad72-208">hello rekommenderas A-poster inte eftersom hello VIP ändras vid omstart för Programgateway.</span><span class="sxs-lookup"><span data-stu-id="0ad72-208">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>


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

## <a name="next-steps"></a><span data-ttu-id="0ad72-209">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0ad72-209">Next steps</span></span>

<span data-ttu-id="0ad72-210">Om du vill tooconfigure ett program gateway toouse med en intern belastningsutjämnare (ILB), se [skapa en Programgateway med en intern belastningsutjämnare (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="0ad72-210">If you want tooconfigure an application gateway toouse with an internal load balancer (ILB), see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="0ad72-211">Om du vill ha mer information om belastningsutjämningsalternativ i allmänhet läser du:</span><span class="sxs-lookup"><span data-stu-id="0ad72-211">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="0ad72-212">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="0ad72-212">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="0ad72-213">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="0ad72-213">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

