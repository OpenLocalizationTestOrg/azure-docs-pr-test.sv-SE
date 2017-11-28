---
title: aaaConfigure end tooend SSL med Azure Programgateway | Microsoft Docs
description: "Den här artikeln beskriver hur tooconfigure avslutas tooend SSL med Application Gateway med hjälp av Azure Resource Manager PowerShell"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: e6d80a33-4047-4538-8c83-e88876c8834e
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: gwallace
ms.openlocfilehash: 7c280478e143d309e7665219441cbee8c81d9a80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-end-tooend-ssl-with-application-gateway-using-powershell"></a><span data-ttu-id="2ccc2-103">Konfigurera slutet tooend SSL med hjälp av PowerShell-Programgateway</span><span class="sxs-lookup"><span data-stu-id="2ccc2-103">Configure end tooend SSL with Application Gateway using PowerShell</span></span>

## <a name="overview"></a><span data-ttu-id="2ccc2-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="2ccc2-104">Overview</span></span>

<span data-ttu-id="2ccc2-105">Ett program har stöd för Gateway avslutas tooend kryptering av trafik.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-105">Application Gateway supports end tooend encryption of traffic.</span></span> <span data-ttu-id="2ccc2-106">Programgateway gör detta genom att avsluta hello SSL-anslutningen vid hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-106">Application Gateway does this by terminating hello SSL connection at hello application gateway.</span></span> <span data-ttu-id="2ccc2-107">hello gateway gäller hello routningsregler toohello trafik, krypterar hello paket och vidarebefordrar hello paket toohello lämplig backend utifrån hello routning regler.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-107">hello gateway then applies hello routing rules toohello traffic, re-encrypts hello packet, and forwards hello packet toohello appropriate backend based on hello routing rules defined.</span></span> <span data-ttu-id="2ccc2-108">Alla svar från webbservern hello passerar hello samma process tillbaka toohello slutanvändaren.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-108">Any response from hello web server goes through hello same process back toohello end user.</span></span>

<span data-ttu-id="2ccc2-109">En annan funktion som Programgateway stöder definiera anpassade SSL-alternativ.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-109">Another feature that application gateway supports defining custom SSL options.</span></span> <span data-ttu-id="2ccc2-110">Inaktivera hello följande protokoll, version, har stöd för Programgateway **TLSv1.0**, **TLSv1.1**, och **TLSv1.2** samt definiera hello som cipher paket toouse och hello prioritetsordning.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-110">Application Gateway supports disabling hello following protocol version; **TLSv1.0**, **TLSv1.1**, and **TLSv1.2** as well defining hello which cipher suites toouse and hello order of preference.</span></span>  <span data-ttu-id="2ccc2-111">toolearn mer information om konfigurerbara alternativ som SSL, besök [översikt över SSL-Policy](application-gateway-SSL-policy-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2ccc2-111">toolearn more about configurable SSL options, visit [SSL Policy overview](application-gateway-SSL-policy-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="2ccc2-112">SSL 2.0 och SSL 3.0 är inaktiverade som standard och kan inte aktiveras.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-112">SSL 2.0 and SSL 3.0 are disabled by default and cannot be enabled.</span></span> <span data-ttu-id="2ccc2-113">De betraktas som osäkra och inte kan toobe används med Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-113">They are considered unsecure and are not able toobe used with Application Gateway.</span></span>

![scenario-bild][scenario]

## <a name="scenario"></a><span data-ttu-id="2ccc2-115">Scenario</span><span class="sxs-lookup"><span data-stu-id="2ccc2-115">Scenario</span></span>

<span data-ttu-id="2ccc2-116">I det här scenariot kan du lära dig hur toocreate som en gateway som använder avslutas tooend SSL med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-116">In this scenario, you learn how toocreate an application gateway using end tooend SSL using PowerShell.</span></span>

<span data-ttu-id="2ccc2-117">Det här scenariot kommer:</span><span class="sxs-lookup"><span data-stu-id="2ccc2-117">This scenario will:</span></span>

* <span data-ttu-id="2ccc2-118">Skapa en resursgrupp med namnet **appgw rg**</span><span class="sxs-lookup"><span data-stu-id="2ccc2-118">Create a resource group named **appgw-rg**</span></span>
* <span data-ttu-id="2ccc2-119">Skapa ett virtuellt nätverk med namnet **appgwvnet** med ett adressutrymme för 10.0.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-119">Create a virtual network named **appgwvnet** with an address space of 10.0.0.0/16.</span></span>
* <span data-ttu-id="2ccc2-120">Skapa två undernät kallas **appgwsubnet** och **appsubnet**.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-120">Create two subnets called **appgwsubnet** and **appsubnet**.</span></span>
* <span data-ttu-id="2ccc2-121">Skapa ett litet program gateway stödjande slutet tooend SSL-kryptering som gränser SSL-protokoll versioner och krypteringssviter.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-121">Create a small application gateway supporting end tooend SSL encryption that limits SSL protocols versions and cipher suites.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="2ccc2-122">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="2ccc2-122">Before you begin</span></span>

<span data-ttu-id="2ccc2-123">tooconfigure end tooend SSL med en Programgateway, ett certifikat krävs för hello gateway och certifikat krävs för hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-123">tooconfigure end tooend SSL with an application gateway, a certificate is required for hello gateway and certificates are required for hello backend servers.</span></span> <span data-ttu-id="2ccc2-124">Hej gatewaycertifikatet är används tooencrypt och dekryptera hello trafik som skickas tooit med SSL.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-124">hello gateway certificate is used tooencrypt and decrypt hello traffic sent tooit using SSL.</span></span> <span data-ttu-id="2ccc2-125">hello gateway-certifikat måste toobe Personal Information Exchange (pfx)-format.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-125">hello gateway certificate needs toobe in Personal Information Exchange (pfx) format.</span></span> <span data-ttu-id="2ccc2-126">Det här filformatet tillåter hello privata nyckel toobe exporteras som krävs för hello programmet gateway tooperform hello kryptering och dekryptering av trafik.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-126">This file format allows for hello private key toobe exported which is required by hello application gateway tooperform hello encryption and decryption of traffic.</span></span>

<span data-ttu-id="2ccc2-127">Tooend SSL-kryptering hello backend måste vara godkända med Programgateway för end.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-127">For end tooend SSL encryption hello backend must be whitelisted with application gateway.</span></span> <span data-ttu-id="2ccc2-128">Detta görs genom att överföra hello offentliga certifikat för hello serverdelar toohello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-128">This is done by uploading hello public certificate of hello backends toohello application gateway.</span></span> <span data-ttu-id="2ccc2-129">Detta säkerställer att hello Programgateway kommunicerar endast med kända backend-instanser.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-129">This ensures that hello application gateway only communicates with known backend instances.</span></span> <span data-ttu-id="2ccc2-130">Detta skyddar ytterligare tooend hello slutpunkt-kommunikation.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-130">This further secures hello end tooend communication.</span></span>

<span data-ttu-id="2ccc2-131">Den här processen beskrivs i hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2ccc2-131">This process is described in hello following steps:</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="2ccc2-132">Skapa hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-132">Create hello Resource Group</span></span>

<span data-ttu-id="2ccc2-133">Det här avsnittet vägleder dig genom att skapa en resursgrupp, som innehåller hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-133">This section walks you through creating a resource group, that contains hello application gateway.</span></span>

### <a name="step-1"></a><span data-ttu-id="2ccc2-134">Steg 1</span><span class="sxs-lookup"><span data-stu-id="2ccc2-134">Step 1</span></span>

<span data-ttu-id="2ccc2-135">Logga in tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-135">Log in tooyour Azure Account.</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a><span data-ttu-id="2ccc2-136">Steg 2</span><span class="sxs-lookup"><span data-stu-id="2ccc2-136">Step 2</span></span>

<span data-ttu-id="2ccc2-137">Välj hello prenumeration toouse för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-137">Select hello subscription toouse for this scenario.</span></span>

```powershell
Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
```

### <a name="step-3"></a><span data-ttu-id="2ccc2-138">Steg 3</span><span class="sxs-lookup"><span data-stu-id="2ccc2-138">Step 3</span></span>

<span data-ttu-id="2ccc2-139">Skapa en resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp).</span><span class="sxs-lookup"><span data-stu-id="2ccc2-139">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a><span data-ttu-id="2ccc2-140">Skapa ett virtuellt nätverk och ett undernät för hello Programgateway</span><span class="sxs-lookup"><span data-stu-id="2ccc2-140">Create a virtual network and a subnet for hello application gateway</span></span>

<span data-ttu-id="2ccc2-141">hello följande exempel skapas ett virtuellt nätverk och två undernät.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-141">hello following example creates a virtual network and two subnets.</span></span> <span data-ttu-id="2ccc2-142">Ett undernät är används toohold hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-142">One subnet is used toohold hello application gateway.</span></span> <span data-ttu-id="2ccc2-143">hello används andra undernät för hello serverdelar värd hello webbprogram.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-143">hello other subnet is used for hello backends hosting hello web application.</span></span>

### <a name="step-1"></a><span data-ttu-id="2ccc2-144">Steg 1</span><span class="sxs-lookup"><span data-stu-id="2ccc2-144">Step 1</span></span>

<span data-ttu-id="2ccc2-145">Tilldela ett adressintervall för undernätet hello användas för hello Programgateway sig själv.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-145">Assign an address range for hello subnet be used for hello Application Gateway itself.</span></span>

```powershell
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24
```

> [!NOTE]
> <span data-ttu-id="2ccc2-146">Undernät som konfigurerats för Programgateway bör vara rätt storlek.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-146">Subnets configured for application gateway should be properly sized.</span></span> <span data-ttu-id="2ccc2-147">En Programgateway kan konfigureras för in too10 instanser.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-147">An application gateway can be configured for up too10 instances.</span></span> <span data-ttu-id="2ccc2-148">Varje instans tar en IP-adress från hello undernät.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-148">Each instance takes one IP address from hello subnet.</span></span> <span data-ttu-id="2ccc2-149">För litet för ett undernät som kan påverkas negativt och skala ut en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-149">Too small of a subnet can adversely affect scaling out an application gateway.</span></span>
> 
> 

### <a name="step-2"></a><span data-ttu-id="2ccc2-150">Steg 2</span><span class="sxs-lookup"><span data-stu-id="2ccc2-150">Step 2</span></span>

<span data-ttu-id="2ccc2-151">Tilldela en adressintervallet toobe som används för hello serverdelen för adresspoolen.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-151">Assign an address range toobe used for hello Backend address pool.</span></span>

```powershell
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24
```

### <a name="step-3"></a><span data-ttu-id="2ccc2-152">Steg 3</span><span class="sxs-lookup"><span data-stu-id="2ccc2-152">Step 3</span></span>

<span data-ttu-id="2ccc2-153">Skapa ett virtuellt nätverk med hello undernät som definierats i föregående steg hello.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-153">Create a virtual network with hello subnets defined in hello preceding steps.</span></span>

```powershell
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="step-4"></a><span data-ttu-id="2ccc2-154">Steg 4</span><span class="sxs-lookup"><span data-stu-id="2ccc2-154">Step 4</span></span>

<span data-ttu-id="2ccc2-155">Hämta hello virtuellt nätverk resurs och undernät resurser toobe används i hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2ccc2-155">Retrieve hello virtual network resource and subnet resources toobe used in hello following steps:</span></span>

```powershell
$vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
$gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
$nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a><span data-ttu-id="2ccc2-156">Skapa en offentlig IP-adress för hello frontend-konfiguration</span><span class="sxs-lookup"><span data-stu-id="2ccc2-156">Create a public IP address for hello front-end configuration</span></span>

<span data-ttu-id="2ccc2-157">Skapa en offentlig IP-resurs toobe användas för Programgateway hello.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-157">Create a public IP resource toobe used for hello application gateway.</span></span> <span data-ttu-id="2ccc2-158">Den här offentliga IP-adressen används följande steg.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-158">This public IP address is used a following step.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic
```

> [!IMPORTANT]
> <span data-ttu-id="2ccc2-159">Programgateway stöder inte hello användning av en offentlig IP-adress som skapats med en domänetikett.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-159">Application Gateway does not support hello use of a public IP address created with a domain label defined.</span></span> <span data-ttu-id="2ccc2-160">Endast en offentlig IP-adress med en dynamiskt skapade domänetikett stöds.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-160">Only a public IP address with a dynamically created domain label is supported.</span></span> <span data-ttu-id="2ccc2-161">Om du behöver ett eget dns-namn för hello Programgateway rekommenderas toouse en CNAME-post registreras som ett alias.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-161">If you require a friendly dns name for hello application gateway, it is recommended toouse a CNAME record as an alias.</span></span>

## <a name="create-an-application-gateway-configuration-object"></a><span data-ttu-id="2ccc2-162">Skapa ett konfigurationsobjekt för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="2ccc2-162">Create an application gateway configuration object</span></span>

<span data-ttu-id="2ccc2-163">Innan du skapar hello Programgateway anges alla konfigurationsobjekt.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-163">All configuration items are set before creating hello application gateway.</span></span> <span data-ttu-id="2ccc2-164">hello med följande steg skapar hello konfigurationsobjekt som behövs för en gateway programresursen.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-164">hello following steps create hello configuration items that are needed for an application gateway resource.</span></span>

### <a name="step-1"></a><span data-ttu-id="2ccc2-165">Steg 1</span><span class="sxs-lookup"><span data-stu-id="2ccc2-165">Step 1</span></span>

<span data-ttu-id="2ccc2-166">Skapa ett program gateway IP-konfiguration, den här inställningen konfigurerar vilka undernät hello programmet gateway använder.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-166">Create an application gateway IP configuration, this setting configures what subnet hello application gateway uses.</span></span> <span data-ttu-id="2ccc2-167">När Programgateway startar hämtar en IP-adress från hello-undernät som konfigurerats och dirigerar trafik toohello IP-adresser på nätverket i hello backend-IP-adresspool.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-167">When application gateway starts, it picks up an IP address from hello subnet configured and routes network traffic toohello IP addresses in hello back-end IP pool.</span></span> <span data-ttu-id="2ccc2-168">Tänk på att varje instans använder en IP-adress.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-168">Keep in mind that each instance takes one IP address.</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet
```

### <a name="step-2"></a><span data-ttu-id="2ccc2-169">Steg 2</span><span class="sxs-lookup"><span data-stu-id="2ccc2-169">Step 2</span></span>

<span data-ttu-id="2ccc2-170">Skapa en frontend IP-konfiguration, inställningen mappar en privat eller offentlig IP-adress toohello klientdel för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-170">Create a front-end IP configuration, this setting maps a private or public ip address toohello front end of hello application gateway.</span></span> <span data-ttu-id="2ccc2-171">hello efter steg associerar hello offentliga IP-adressen i hello föregående steg med hello frontend IP-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-171">hello following step associates hello public IP address in hello preceding step with hello front-end IP configuration.</span></span>

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip
```

### <a name="step-3"></a><span data-ttu-id="2ccc2-172">Steg 3</span><span class="sxs-lookup"><span data-stu-id="2ccc2-172">Step 3</span></span>

<span data-ttu-id="2ccc2-173">Konfigurera hello backend-IP-adresspool med hello IP-adresser i hello backend-webbservrar.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-173">Configure hello back-end IP address pool with hello IP addresses of hello backend web servers.</span></span> <span data-ttu-id="2ccc2-174">IP-adresserna är hello IP-adresser som tar emot hello nätverkstrafik som kommer från hello frontend IP-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-174">These IP addresses are hello IP addresses that receive hello network traffic that comes from hello front-end IP endpoint.</span></span> <span data-ttu-id="2ccc2-175">Ersätt hello efter IP-adresser tooadd egna programslutpunkter IP-adress.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-175">You replace hello following IP addresses tooadd your own application IP address endpoints.</span></span>

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3
```

> [!NOTE]
> <span data-ttu-id="2ccc2-176">Ett fullständigt kvalificerat domännamn (FQDN) är också ett giltigt värde i stället för en ip-adress för hello backend-servrar med hjälp av hello - BackendFqdns växel.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-176">A fully qualified domain name (FQDN) is also a valid value in place of an ip address for hello backend servers by using hello -BackendFqdns switch.</span></span> 

### <a name="step-4"></a><span data-ttu-id="2ccc2-177">Steg 4</span><span class="sxs-lookup"><span data-stu-id="2ccc2-177">Step 4</span></span>

<span data-ttu-id="2ccc2-178">Konfigurera hello frontend IP-port för hello offentlig IP-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-178">Configure hello front-end IP port for hello public IP endpoint.</span></span> <span data-ttu-id="2ccc2-179">Den här porten är hello som användare ansluta till.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-179">This port is hello port that end users connect to.</span></span>

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443
```

### <a name="step-5"></a><span data-ttu-id="2ccc2-180">Steg 5</span><span class="sxs-lookup"><span data-stu-id="2ccc2-180">Step 5</span></span>

<span data-ttu-id="2ccc2-181">Konfigurera hello certifikat för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-181">Configure hello certificate for hello application gateway.</span></span> <span data-ttu-id="2ccc2-182">Det här certifikatet används toodecrypt och kryptera hello-trafik på hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-182">This certificate is used toodecrypt and re-encrypt hello traffic on hello application gateway.</span></span>

```powershell
$cert = New-AzureRmApplicationGatewaySSLCertificate -Name cert01 -CertificateFile <full path too.pfx file> -Password <password for certificate file>
```

> [!NOTE]
> <span data-ttu-id="2ccc2-183">Det här exemplet konfigurerar hello-certifikat som används för SSL-anslutning.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-183">This sample configures hello certificate used for SSL connection.</span></span> <span data-ttu-id="2ccc2-184">hello certifikat måste toobe i PFX-format och hello lösenordet måste innehålla mellan 4 too12 tecken.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-184">hello certificate needs toobe in .pfx format, and hello password must be between 4 too12 characters.</span></span>

### <a name="step-6"></a><span data-ttu-id="2ccc2-185">Steg 6</span><span class="sxs-lookup"><span data-stu-id="2ccc2-185">Step 6</span></span>

<span data-ttu-id="2ccc2-186">Skapa hello HTTP-lyssnare för Programgateway hello.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-186">Create hello HTTP listener for hello application gateway.</span></span> <span data-ttu-id="2ccc2-187">Tilldela hello frontend ip-konfiguration, port och toouse för SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-187">Assign hello front-end ip configuration, port, and SSL certificate toouse.</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SSLCertificate $cert
```

### <a name="step-7"></a><span data-ttu-id="2ccc2-188">Steg 7</span><span class="sxs-lookup"><span data-stu-id="2ccc2-188">Step 7</span></span>

<span data-ttu-id="2ccc2-189">Överför hello certifikatet toobe används på hello SSL aktiverat backend resurser.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-189">Upload hello certificate toobe used on hello SSL enabled backend pool resources.</span></span>

> [!NOTE]
> <span data-ttu-id="2ccc2-190">hello standard avsökningen hämtar hello offentlig nyckel från hello **standard** SSL-bindning på hello backend-'s IP-adress och jämför hello offentliga nyckelvärde den tar emot toohello offentliga nyckel/värde du anger här.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-190">hello default probe gets hello public key from hello **default** SSL binding on hello back-end's IP address and compares hello public key value it receives toohello public key value you provide here.</span></span> <span data-ttu-id="2ccc2-191">hello hämtade offentliga nyckel kan inte nödvändigtvis hello avsedda platsen toowhich trafikflöden **om** du använder värdhuvuden och SNI på hello serverdel.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-191">hello retrieved public key may not necessarily be hello intended site toowhich traffic flows **if** you are using host headers and SNI on hello back-end.</span></span> <span data-ttu-id="2ccc2-192">Om du tvekar besöka https://127.0.0.1/ på hello-servrar tooconfirm vilket certifikat som används för hello **standard** SSL-bindning.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-192">If in doubt, visit https://127.0.0.1/ on hello back-ends tooconfirm which certificate is used for hello **default** SSL binding.</span></span> <span data-ttu-id="2ccc2-193">Använd hello offentliga nyckeln från denna begäran i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-193">Use hello public key from that request in this section.</span></span> <span data-ttu-id="2ccc2-194">Om du använder värdhuvuden och SNI på HTTPS-bindningar och du får ett svar och certifikat från en manuell webbläsare begäran toohttps://127.0.0.1/ på hello-servrar, måste du ställa in en standard-SSL-bindning på hello-servrar.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-194">If you are using host-headers and SNI on HTTPS bindings and you do not receive a response and certificate from a manual browser request toohttps://127.0.0.1/ on hello back-ends, you must set up a default SSL binding on hello back-ends.</span></span> <span data-ttu-id="2ccc2-195">Om du inte gör det avsökningar misslyckas och hello backend-är inte godkända.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-195">If you do not do so, probes fail and hello back-end is not whitelisted.</span></span>

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer
```

> [!NOTE]
> <span data-ttu-id="2ccc2-196">hello-certifikat som angetts i det här steget ska hello offentliga nyckeln för hello pfx-certifikat finns på hello serverdel.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-196">hello certificate provided in this step should be hello public key of hello pfx cert present on hello backend.</span></span> <span data-ttu-id="2ccc2-197">Exportera hello-certifikat (inte hello rotcertifikat) installerat på hello backend-servern i. CER-format och använda den i det här steget.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-197">Export hello certificate (not hello root certificate) installed on hello backend server in .CER format and use it in this step.</span></span> <span data-ttu-id="2ccc2-198">Det här steget whitelists hello-serverdelen med hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-198">This step whitelists hello backend with hello application gateway.</span></span>

### <a name="step-8"></a><span data-ttu-id="2ccc2-199">Steg 8</span><span class="sxs-lookup"><span data-stu-id="2ccc2-199">Step 8</span></span>

<span data-ttu-id="2ccc2-200">Konfigurera hello inställningar för Programgateway backend-http.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-200">Configure hello application gateway back-end http settings.</span></span> <span data-ttu-id="2ccc2-201">Tilldela hello certifikat på hello föregående steg toohello http-inställningar.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-201">Assign hello certificate uploaded in hello preceding step toohello http settings.</span></span>

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert
```

### <a name="step-9"></a><span data-ttu-id="2ccc2-202">Steg 9</span><span class="sxs-lookup"><span data-stu-id="2ccc2-202">Step 9</span></span>

<span data-ttu-id="2ccc2-203">Skapa en routning regel för belastningsutjämnare som konfigurerar hello belastningsutjämnaren GUID.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-203">Create a load balancer routing rule that configures hello load balancer behavior.</span></span> <span data-ttu-id="2ccc2-204">I det här exemplet skapas en regel för grundläggande resursallokering.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-204">In this example, a basic round robin rule is created.</span></span>

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

### <a name="step-10"></a><span data-ttu-id="2ccc2-205">Steg 10</span><span class="sxs-lookup"><span data-stu-id="2ccc2-205">Step 10</span></span>

<span data-ttu-id="2ccc2-206">Konfigurera hello instansstorleken för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-206">Configure hello instance size of hello application gateway.</span></span>  <span data-ttu-id="2ccc2-207">hello tillgängliga storlekar är **Standard\_små**, **Standard\_medel**, och **Standard\_stor**.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-207">hello available sizes are **Standard\_Small**, **Standard\_Medium**, and **Standard\_Large**.</span></span>  <span data-ttu-id="2ccc2-208">Hello tillgängliga värden är 1 till 10 för kapacitet.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-208">For capacity, hello available values are 1 through 10.</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

> [!NOTE]
> <span data-ttu-id="2ccc2-209">Du kan välja ett instansantal på 1 för testning.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-209">An instance count of 1 can be chosen for testing purposes.</span></span> <span data-ttu-id="2ccc2-210">Det är viktigt att valfri instans antalet under två instanser tooknow omfattas inte av hello SLA och därför rekommenderas inte.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-210">It is important tooknow that any instance count under two instances is not covered by hello SLA and are therefore not recommended.</span></span> <span data-ttu-id="2ccc2-211">Liten gateways är toobe som används för utveckling testning och inte för produktion.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-211">Small gateways are toobe used for dev test and not for production purposes.</span></span>

### <a name="step-11"></a><span data-ttu-id="2ccc2-212">Steg 11</span><span class="sxs-lookup"><span data-stu-id="2ccc2-212">Step 11</span></span>

<span data-ttu-id="2ccc2-213">Konfigurera hello SSL princip toobe används på hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-213">Configure hello SSL policy toobe used on hello Application Gateway.</span></span> <span data-ttu-id="2ccc2-214">Programgateway stöder hello möjlighet tooset en lägsta version för SSL-protokollversioner.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-214">Application Gateway supports hello ability tooset a minimum version for SSL protocol versions.</span></span>

<span data-ttu-id="2ccc2-215">hello är följande värden en lista över protokollversioner som kan definieras.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-215">hello following values are a list of protocol versions that can be defined.</span></span>

* <span data-ttu-id="2ccc2-216">**TLSv1_0**</span><span class="sxs-lookup"><span data-stu-id="2ccc2-216">**TLSv1_0**</span></span>
* <span data-ttu-id="2ccc2-217">**TLSv1_1**</span><span class="sxs-lookup"><span data-stu-id="2ccc2-217">**TLSv1_1**</span></span>
* <span data-ttu-id="2ccc2-218">**TLSv1_2**</span><span class="sxs-lookup"><span data-stu-id="2ccc2-218">**TLSv1_2**</span></span>

<span data-ttu-id="2ccc2-219">Anger hello minsta protokollversion för**TLSv1_2** och möjliggör **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_ SHA256**, **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, och **TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** endast.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-219">Sets hello minimum protocol version too**TLSv1_2** and enables **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256**, **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, and **TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** only.</span></span>

```powershell
$SSLPolicy = New-AzureRmApplicationGatewaySSLPolicy -MinProtocolVersion TLSv1_2 -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256"
```

## <a name="create-hello-application-gateway"></a><span data-ttu-id="2ccc2-220">Skapa hello Programgateway</span><span class="sxs-lookup"><span data-stu-id="2ccc2-220">Create hello Application Gateway</span></span>

<span data-ttu-id="2ccc2-221">Skapa hello Programgateway alla hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-221">Using all hello preceding steps, create hello Application Gateway.</span></span> <span data-ttu-id="2ccc2-222">hello skapa hello gateway är en tidskrävande process.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-222">hello creation of hello gateway is a long running process.</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgateway -SSLCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SSLPolicy $SSLPolicy -AuthenticationCertificates $authcert -Verbose
```

## <a name="limit-ssl-protocol-versions-on-an-existing-application-gateway"></a><span data-ttu-id="2ccc2-223">Begränsa SSL-protokoll version på en befintlig Programgateway</span><span class="sxs-lookup"><span data-stu-id="2ccc2-223">Limit SSL protocol versions on an existing Application Gateway</span></span>

<span data-ttu-id="2ccc2-224">hello föregående steg leder dig genom att skapa ett program med end tooend SSL och inaktivera vissa versioner för SSL-protokollet.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-224">hello preceding steps take you through creating an application with end tooend SSL and disabling certain SSL protocol versions.</span></span> <span data-ttu-id="2ccc2-225">hello inaktiverar följande exempel vissa SSL-principer på en befintlig gateway för programmet.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-225">hello following example disables certain SSL policies on an existing application gateway.</span></span>

### <a name="step-1"></a><span data-ttu-id="2ccc2-226">Steg 1</span><span class="sxs-lookup"><span data-stu-id="2ccc2-226">Step 1</span></span>

<span data-ttu-id="2ccc2-227">Hämta hello programmet gateway tooupdate.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-227">Retrieve hello application gateway tooupdate.</span></span>

```powershell
$gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG
```

### <a name="step-2"></a><span data-ttu-id="2ccc2-228">Steg 2</span><span class="sxs-lookup"><span data-stu-id="2ccc2-228">Step 2</span></span>

<span data-ttu-id="2ccc2-229">Definiera ett SSL-policy.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-229">Define an SSL policy.</span></span> <span data-ttu-id="2ccc2-230">I följande exempel hello, TLSv1.0 och TLSv1.1 är inaktiverade och hello krypteringssviter **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256** , **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, och  **TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** är hello enda tillåtna.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-230">In hello following example, TLSv1.0 and TLSv1.1 are disabled and hello cipher suites **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256**, **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, and **TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** are hello only ones allowed.</span></span>

```powershell
Set-AzureRmApplicationGatewaySSLPolicy -MinProtocolVersion -PolicyType Custom -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256" -ApplicationGateway $gw

```

### <a name="step-3"></a><span data-ttu-id="2ccc2-231">Steg 3</span><span class="sxs-lookup"><span data-stu-id="2ccc2-231">Step 3</span></span>

<span data-ttu-id="2ccc2-232">Slutligen uppdatera hello gateway.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-232">Finally, update hello gateway.</span></span> <span data-ttu-id="2ccc2-233">Det är viktigt toonote att det sista steget är en tidskrävande uppgift.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-233">It is important toonote that this last step is a long running task.</span></span> <span data-ttu-id="2ccc2-234">Avsluta tooend SSL är konfigurerat på hello Programgateway när du är klar.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-234">When it is done, end tooend SSL is configured on hello application gateway.</span></span>

```powershell
$gw | Set-AzureRmApplicationGateway
```

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="2ccc2-235">Hämta DNS-namn för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="2ccc2-235">Get application gateway DNS name</span></span>

<span data-ttu-id="2ccc2-236">När du har skapat hello gateway är hello nästa steg tooconfigure hello klientdelen för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-236">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="2ccc2-237">När du använder en offentlig IP-adress krävs ett dynamiskt tilldelat DNS-namn som inte är användarvänligt.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-237">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="2ccc2-238">tooensure slutanvändare kan träffa hello Programgateway, en CNAME-post kan vara används toopoint toohello offentlig slutpunkt för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-238">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="2ccc2-239">[Konfigurera ett eget domännamn i Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="2ccc2-239">[Configuring a custom domain name for in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).</span></span> <span data-ttu-id="2ccc2-240">toodo detta, hämta information om hello Programgateway och dess associerade IP DNS-namn med hjälp av hello PublicIPAddress element bifogade toohello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-240">toodo this, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="2ccc2-241">hello programmet gateway DNS-namn ska använda toocreate en CNAME-post som pekar hello två web program toothis DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-241">hello application gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="2ccc2-242">hello rekommenderas A-poster inte eftersom hello VIP ändras vid omstart för Programgateway.</span><span class="sxs-lookup"><span data-stu-id="2ccc2-242">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="2ccc2-243">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2ccc2-243">Next steps</span></span>

<span data-ttu-id="2ccc2-244">Lär dig mer om härdning hello säkerheten för ditt webbprogram med Brandvägg för webbaserade program via Programgateway genom att besöka [översikt över webbprogram brandväggen](application-gateway-webapplicationfirewall-overview.md)</span><span class="sxs-lookup"><span data-stu-id="2ccc2-244">Learn about hardening hello security of your web applications with Web Application Firewall through Application Gateway by visiting [Web Application Firewall Overview](application-gateway-webapplicationfirewall-overview.md)</span></span>

[scenario]: ./media/application-gateway-end-to-end-SSL-powershell/scenario.png
