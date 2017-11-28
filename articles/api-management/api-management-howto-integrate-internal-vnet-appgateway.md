---
title: "aaaHow toouse Azure API Management i det virtuella nätverket med Programgateway | Microsoft Docs"
description: "Lär dig hur toosetup och konfigurera Azure API Management i internt virtuellt nätverk med programmet Gateway (Brandvägg) som klientdel"
services: api-management
documentationcenter: 
author: solankisamir
manager: kjoshi
editor: antonba
ms.assetid: a8c982b2-bca5-4312-9367-4a0bbc1082b1
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2017
ms.author: sasolank
ms.openlocfilehash: 74303a2ee8a10db633ab1740ec7267728eacb473
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-api-management-in-an-internal-vnet-with-application-gateway"></a><span data-ttu-id="f7ac1-103">Integrera API Management i ett internt virtuellt nätverk med Programgateway</span><span class="sxs-lookup"><span data-stu-id="f7ac1-103">Integrate API Management in an internal VNET with Application Gateway</span></span> 

##<span data-ttu-id="f7ac1-104"><a name="overview"></a> Översikt</span><span class="sxs-lookup"><span data-stu-id="f7ac1-104"><a name="overview"> </a> Overview</span></span>
 
<span data-ttu-id="f7ac1-105">hello API Management-tjänsten kan konfigureras i ett virtuellt nätverk i internt läge vilket gör att det är enbart tillgänglig från hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-105">hello API Management service can be configured in a Virtual Network in internal mode which makes it accessible only from within hello Virtual Network.</span></span> <span data-ttu-id="f7ac1-106">Azure Application Gateway är en PAAS-tjänst som tillhandahåller en lager 7 belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-106">Azure Application Gateway is a PAAS Service which provides a Layer-7 load balancer.</span></span> <span data-ttu-id="f7ac1-107">Den fungerar som en omvänd proxy-tjänst och innehåller bland erbjudande Web Application Firewall (Brandvägg).</span><span class="sxs-lookup"><span data-stu-id="f7ac1-107">It acts as a reverse-proxy service and provides among its offering a Web Application Firewall (WAF).</span></span>

<span data-ttu-id="f7ac1-108">Kombinera API Management etablerad på ett internt virtuellt nätverk med hello Programgateway klientdel kan hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="f7ac1-108">Combining API Management provisioned in an internal VNET with hello Application Gateway frontend enables hello following scenarios:</span></span>

* <span data-ttu-id="f7ac1-109">Använd hello samma API Management-resurs för förbrukning av både konsumenter interna och externa användare.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-109">Use hello same API Management resource for consumption by both internal consumers and external consumers.</span></span>
* <span data-ttu-id="f7ac1-110">Använda en enskild resurs för API Management och har en delmängd av API: er som definierats i API-hantering som är tillgänglig för externa användare.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-110">Use a single API Management resource and have a subset of APIs defined in API Management available for external consumers.</span></span>
* <span data-ttu-id="f7ac1-111">Ange en nyckelfärdig sätt tooswitch åtkomst tooAPI Management från hello offentliga Internet på och stänga av.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-111">Provide a turn-key way tooswitch access tooAPI Management from hello public Internet on and off.</span></span> 

##<span data-ttu-id="f7ac1-112"><a name="scenario"></a> Scenario</span><span class="sxs-lookup"><span data-stu-id="f7ac1-112"><a name="scenario"> </a> Scenario</span></span>
<span data-ttu-id="f7ac1-113">Den här artikeln kommer upp hur toouse ett enda API Management-tjänsten för både interna och externa användare och gör det fungerar som en enda klientdel för både lokala och molntjänster API: er.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-113">This article will cover how toouse a single API Management service for both internal and external consumers and make it act as a single frontend for both on-prem and cloud APIs.</span></span> <span data-ttu-id="f7ac1-114">Du kan även se hur tooexpose bara en del av din API: er (i hello exempel markeringen i grönt) för extern användning med hello PathBasedRouting tillgängliga funktioner i Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-114">You will also see how tooexpose only a subset of your APIs (in hello example they are highlighted in green) for External Consumption using hello PathBasedRouting functionality available in Application Gateway.</span></span>

<span data-ttu-id="f7ac1-115">I hello första installationen exemplet hanteras alla API: er endast från i ditt virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-115">In hello first setup example all your APIs are managed only from within your Virtual Network.</span></span> <span data-ttu-id="f7ac1-116">Interna användare (markerade i orange) kan komma åt alla dina interna och externa API: er.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-116">Internal consumers (highlighted in orange) can access all your internal and external APIs.</span></span> <span data-ttu-id="f7ac1-117">Trafik som aldrig går ut tooInternet en hög prestanda levereras via Expressroute-kretsar.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-117">Traffic never goes out tooInternet a high performance is delivered via Express Route circuits.</span></span>

![URL-väg](./media/api-management-howto-integrate-internal-vnet-appgateway/api-management-howto-integrate-internal-vnet-appgateway.png)

## <span data-ttu-id="f7ac1-119"><a name="before-you-begin"></a> Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="f7ac1-119"><a name="before-you-begin"> </a> Before you begin</span></span>

1. <span data-ttu-id="f7ac1-120">Installera hello senaste versionen av hello Azure PowerShell-cmdlets med hello installationsprogram för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-120">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="f7ac1-121">Du kan hämta och installera hello senaste versionen från hello **Windows PowerShell** avsnitt i hello [Nedladdningssida](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="f7ac1-121">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="f7ac1-122">Skapa ett virtuellt nätverk och skapa separata undernät för API Management och Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-122">Create a Virtual Network and create separate subnets for API Management and Application Gateway.</span></span> 
3. <span data-ttu-id="f7ac1-123">Om du planerar toocreate en anpassad DNS-server för hello virtuellt nätverk, kan du göra det innan du börjar hello distribueringen.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-123">If you intend toocreate a custom DNS server for hello Virtual Network, do so before starting hello deployment.</span></span> <span data-ttu-id="f7ac1-124">Kontrollera fungerar genom att säkerställa att en virtuell dator som skapats i ett nytt undernät i hello virtuellt nätverk kan lösa och komma åt alla Azure-Tjänsteslutpunkter.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-124">Double check it works by ensuring a virtual machine created in a new subnet in hello Virtual Network can resolve and access all Azure service endpoints.</span></span>

## <a name="what-is-required-toocreate-an-integration-between-api-management-and-application-gateway"></a><span data-ttu-id="f7ac1-125">Vad är obligatoriska toocreate integration mellan API Management och Programgateway?</span><span class="sxs-lookup"><span data-stu-id="f7ac1-125">What is required toocreate an integration between API Management and Application Gateway?</span></span>

* <span data-ttu-id="f7ac1-126">**Backend-serverpoolen:** hello interna virtuella IP-adress för hello API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-126">**Back-end server pool:** This is hello internal virtual IP address of hello API Management service.</span></span>
* <span data-ttu-id="f7ac1-127">**Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-127">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="f7ac1-128">Dessa inställningar är tillämpade tooall servrar inom hello poolen.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-128">These settings are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="f7ac1-129">**Frontend-port:** hello offentliga porten som öppnas på hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-129">**Front-end port:** This is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="f7ac1-130">Trafik träffa den hämtar omdirigerade tooone av hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-130">Traffic hitting it gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="f7ac1-131">**Lyssnare:** hello-lyssnare har en frontend-port, ett protokoll (Http eller Https dessa värden är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning).</span><span class="sxs-lookup"><span data-stu-id="f7ac1-131">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="f7ac1-132">**Regel:** hello regeln Binder poolen lyssnare tooa backend-server.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-132">**Rule:** hello rule binds a listener tooa back-end server pool.</span></span>
* <span data-ttu-id="f7ac1-133">**Anpassade Hälsoavsökningen:** Application Gateway som standard använder IP-adress baserat avsökningar toofigure reda på vilka servrar i hello BackendAddressPool är aktiva.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-133">**Custom Health Probe:** Application Gateway, by default, uses IP address based probes toofigure out which servers in hello BackendAddressPool are active.</span></span> <span data-ttu-id="f7ac1-134">hello API Management-tjänsten svarar bara toorequests som har hello rätt värdhuvud, därför hello standard avsökningar misslyckas.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-134">hello API Management service only responds toorequests which have hello correct host header, hence hello default probes fail.</span></span> <span data-ttu-id="f7ac1-135">En anpassad hälsoavsökningen måste toobe definierats toohelp Programgateway bestämma att hello-tjänsten är igång och att den ska vidarebefordra begäranden.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-135">A custom health probe needs toobe defined toohelp application gateway determine that hello service is alive and it should forward requests.</span></span>
* <span data-ttu-id="f7ac1-136">**Certifikatet för domänen:** tooaccess API-hantering från hello internet måste toocreate en CNAME-mappning av ett värdnamn toohello Programgateway frontend DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-136">**Custom domain certificate:** tooaccess API Management from hello internet you need toocreate a CNAME mapping of its hostname toohello Application Gateway front-end DNS name.</span></span> <span data-ttu-id="f7ac1-137">Detta garanterar att hello hostname sidhuvud och certifikat som skickats tooApplication Gateway som vidarebefordras tooAPI Management är en APIM kan identifiera som giltig.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-137">This ensures that hello hostname header and certificate sent tooApplication Gateway that is forwarded tooAPI Management is one APIM can recognize as valid.</span></span>

## <span data-ttu-id="f7ac1-138"><a name="overview-steps"></a> Steg som krävs för att integrera API Management och Programgateway</span><span class="sxs-lookup"><span data-stu-id="f7ac1-138"><a name="overview-steps"> </a> Steps required for integrating API Management and Application Gateway</span></span> 

1. <span data-ttu-id="f7ac1-139">Skapa en resursgrupp för Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-139">Create a resource group for Resource Manager.</span></span>
2. <span data-ttu-id="f7ac1-140">Skapa ett virtuellt nätverk och undernät offentlig IP för hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-140">Create a Virtual Network, subnet, and public IP for hello Application Gateway.</span></span> <span data-ttu-id="f7ac1-141">Skapa ett annat undernät för API-hantering.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-141">Create another subnet for API Management.</span></span>
3. <span data-ttu-id="f7ac1-142">Skapa en API Management-tjänst i hello VNET subnet skapade ovan och se till att du använder intern hello-läget.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-142">Create an API Management service inside hello VNET subnet created above and ensure you use hello Internal mode.</span></span>
4. <span data-ttu-id="f7ac1-143">Konfigurera hello domännamn i hello API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-143">Setup hello custom domain name in hello API Management service.</span></span>
5. <span data-ttu-id="f7ac1-144">Skapa ett konfigurationsobjekt för Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-144">Create an Application Gateway configuration object.</span></span>
6. <span data-ttu-id="f7ac1-145">Skapa en Programgateway resurs.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-145">Create an Application Gateway resource.</span></span>
7. <span data-ttu-id="f7ac1-146">Skapa en CNAME-post från hello offentliga DNS-namnet på hello Programgateway toohello API Management proxy värdnamn.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-146">Create a CNAME from hello public DNS name of hello Application Gateway toohello API Management proxy hostname.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="f7ac1-147">Skapa en resursgrupp för Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f7ac1-147">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="f7ac1-148">Kontrollera att du använder hello senaste versionen av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-148">Make sure that you are using hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="f7ac1-149">Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="f7ac1-149">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="f7ac1-150">Steg 1</span><span class="sxs-lookup"><span data-stu-id="f7ac1-150">Step 1</span></span>

<span data-ttu-id="f7ac1-151">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="f7ac1-151">Log in tooAzure</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="f7ac1-152">Autentisera med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-152">Authenticate with your credentials.</span></span><BR>

### <a name="step-2"></a><span data-ttu-id="f7ac1-153">Steg 2</span><span class="sxs-lookup"><span data-stu-id="f7ac1-153">Step 2</span></span>

<span data-ttu-id="f7ac1-154">Kontrollera hello prenumerationer för hello-kontot och markera den.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-154">Check hello subscriptions for hello account and select it.</span></span>

```powershell
Get-AzureRmSubscription -Subscriptionid "GUID of subscription" | Select-AzureRmSubscription
```

### <a name="step-3"></a><span data-ttu-id="f7ac1-155">Steg 3</span><span class="sxs-lookup"><span data-stu-id="f7ac1-155">Step 3</span></span>

<span data-ttu-id="f7ac1-156">Skapa en resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp).</span><span class="sxs-lookup"><span data-stu-id="f7ac1-156">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name "apim-appGw-RG" -Location "West US"
```
<span data-ttu-id="f7ac1-157">Azure Resource Manager kräver att alla resursgrupper anger en plats.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-157">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="f7ac1-158">Detta används som hello standardplatsen för resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-158">This is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="f7ac1-159">Se till att alla kommandon toocreate ett program gateway används hello samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-159">Make sure that all commands toocreate an application gateway use hello same resource group.</span></span>

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a><span data-ttu-id="f7ac1-160">Skapa ett virtuellt nätverk och ett undernät för hello Programgateway</span><span class="sxs-lookup"><span data-stu-id="f7ac1-160">Create a Virtual Network and a subnet for hello application gateway</span></span>

<span data-ttu-id="f7ac1-161">hello som följande exempel visar hur hello toocreate ett virtuellt nätverk med hjälp av hanteraren för filserverresurser.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-161">hello following example shows how toocreate a Virtual Network using hello resource manager.</span></span>

### <a name="step-1"></a><span data-ttu-id="f7ac1-162">Steg 1</span><span class="sxs-lookup"><span data-stu-id="f7ac1-162">Step 1</span></span>

<span data-ttu-id="f7ac1-163">Tilldela hello adressintervallet 10.0.0.0/24 toohello undernät variabeln toobe användas för Programgateway när du skapar ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-163">Assign hello address range 10.0.0.0/24 toohello subnet variable toobe used for Application Gateway while creating a Virtual Network.</span></span>

```powershell
$appgatewaysubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim01" -AddressPrefix "10.0.0.0/24"
```

### <a name="step-2"></a><span data-ttu-id="f7ac1-164">Steg 2</span><span class="sxs-lookup"><span data-stu-id="f7ac1-164">Step 2</span></span>

<span data-ttu-id="f7ac1-165">Tilldela hello adressintervallet 10.0.1.0/24 toohello undernät variabeln toobe används för API Management när du skapar ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-165">Assign hello address range 10.0.1.0/24 toohello subnet variable toobe used for API Management while creating a Virtual Network.</span></span>

```powershell
$apimsubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim02" -AddressPrefix "10.0.1.0/24"
```

### <a name="step-3"></a><span data-ttu-id="f7ac1-166">Steg 3</span><span class="sxs-lookup"><span data-stu-id="f7ac1-166">Step 3</span></span>

<span data-ttu-id="f7ac1-167">Skapa ett virtuellt nätverk med namnet **appgwvnet** i resursgruppen **apim-appGw-RG** för hello västra USA region med hello prefixet 10.0.0.0/16 med undernät 10.0.0.0/24 och 10.0.1.0/24.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-167">Create a Virtual Network named **appgwvnet** in resource group **apim-appGw-RG** for hello West US region using hello prefix 10.0.0.0/16 with subnets 10.0.0.0/24 and 10.0.1.0/24.</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name "appgwvnet" -ResourceGroupName "apim-appGw-RG" -Location "West US" -AddressPrefix "10.0.0.0/16" -Subnet $appgatewaysubnet,$apimsubnet
```

### <a name="step-4"></a><span data-ttu-id="f7ac1-168">Steg 4</span><span class="sxs-lookup"><span data-stu-id="f7ac1-168">Step 4</span></span>

<span data-ttu-id="f7ac1-169">Tilldela en variabel för undernätet för hello nästa steg</span><span class="sxs-lookup"><span data-stu-id="f7ac1-169">Assign a subnet variable for hello next steps</span></span>

```powershell
$appgatewaysubnetdata=$vnet.Subnets[0]
$apimsubnetdata=$vnet.Subnets[1]
```
## <a name="create-an-api-management-service-inside-a-vnet-configured-in-internal-mode"></a><span data-ttu-id="f7ac1-170">Skapa en API Management-tjänst i ett virtuellt nätverk som konfigurerats i internt läge</span><span class="sxs-lookup"><span data-stu-id="f7ac1-170">Create an API Management service inside a VNET configured in internal mode</span></span>

<span data-ttu-id="f7ac1-171">hello följande exempel visas hur toocreate en API Management-tjänsten i ett VNET konfigurerats för intern åtkomst.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-171">hello following example shows how toocreate an API Management service in a VNET configured for internal access only.</span></span>

### <a name="step-1"></a><span data-ttu-id="f7ac1-172">Steg 1</span><span class="sxs-lookup"><span data-stu-id="f7ac1-172">Step 1</span></span>
<span data-ttu-id="f7ac1-173">Skapa ett virtuellt nätverk för API Management-objekt med hello undernät $apimsubnetdata skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-173">Create an API Management Virtual Network object using hello subnet $apimsubnetdata created above.</span></span>

```powershell
$apimVirtualNetwork = New-AzureRmApiManagementVirtualNetwork -Location "West US" -SubnetResourceId $apimsubnetdata.Id
```
### <a name="step-2"></a><span data-ttu-id="f7ac1-174">Steg 2</span><span class="sxs-lookup"><span data-stu-id="f7ac1-174">Step 2</span></span>
<span data-ttu-id="f7ac1-175">Skapa en API Management service i hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-175">Create an API Management service inside hello Virtual Network.</span></span>

```powershell
$apimService = New-AzureRmApiManagement -ResourceGroupName "apim-appGw-RG" -Location "West US" -Name "ContosoApi" -Organization "Contoso" -AdminEmail "admin@contoso.com" -VirtualNetwork $apimVirtualNetwork -VpnType "Internal" -Sku "Developer"
```
<span data-ttu-id="f7ac1-176">När hello ovanstående kommando lyckas Läs för[DNS-konfiguration krävs tooaccess interna virtuella nätverk API Management-tjänsten](api-management-using-with-internal-vnet.md#apim-dns-configuration) tooaccess den.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-176">After hello above command succeeds refer too[DNS Configuration required tooaccess internal VNET API Management service](api-management-using-with-internal-vnet.md#apim-dns-configuration) tooaccess it.</span></span>

## <a name="set-up-a-custom-domain-name-in-api-management"></a><span data-ttu-id="f7ac1-177">Ställa in ett anpassat domännamn i API Management</span><span class="sxs-lookup"><span data-stu-id="f7ac1-177">Set-up a custom domain name in API Management</span></span>

### <a name="step-1"></a><span data-ttu-id="f7ac1-178">Steg 1</span><span class="sxs-lookup"><span data-stu-id="f7ac1-178">Step 1</span></span>
<span data-ttu-id="f7ac1-179">Ladda upp hello certifikat med privat nyckel för hello domän.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-179">Upload hello certificate with private key for hello domain.</span></span> <span data-ttu-id="f7ac1-180">Det här exemplet blir `*.contoso.net`.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-180">For this example it will be `*.contoso.net`.</span></span> 

```powershell
$certUploadResult = Import-AzureRmApiManagementHostnameCertificate -ResourceGroupName "apim-appGw-RG" -Name "ContosoApi" -HostnameType "Proxy" -PfxPath <full path too.pfx file> -PfxPassword <password for certificate file> -PassThru
```

### <a name="step-2"></a><span data-ttu-id="f7ac1-181">Steg 2</span><span class="sxs-lookup"><span data-stu-id="f7ac1-181">Step 2</span></span>
<span data-ttu-id="f7ac1-182">När hello certifikat har överförts, skapa ett konfigurationsobjekt för värdnamn för hello proxy med ett värdnamn för `api.contoso.net`, enligt hello exempel certifikatet ger behörighet för hello `*.contoso.net` domän.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-182">Once hello certificate is uploaded, create a hostname configuration object for hello proxy with a hostname of `api.contoso.net`, as hello example certificate provides authority for hello  `*.contoso.net` domain.</span></span> 

```powershell
$proxyHostnameConfig = New-AzureRmApiManagementHostnameConfiguration -CertificateThumbprint $certUploadResult.Thumbprint -Hostname "api.contoso.net"
$result = Set-AzureRmApiManagementHostnames -Name "ContosoApi" -ResourceGroupName "apim-appGw-RG" -ProxyHostnameConfiguration $proxyHostnameConfig
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a><span data-ttu-id="f7ac1-183">Skapa en offentlig IP-adress för hello frontend-konfiguration</span><span class="sxs-lookup"><span data-stu-id="f7ac1-183">Create a public IP address for hello front-end configuration</span></span>

<span data-ttu-id="f7ac1-184">Skapa en offentlig IP-resurs **publicIP01** i resursgruppen **apim-appGw-RG** för hello västra USA region.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-184">Create a public IP resource **publicIP01** in resource group **apim-appGw-RG** for hello West US region.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -name "publicIP01" -location "West US" -AllocationMethod Dynamic
```

<span data-ttu-id="f7ac1-185">En IP-adress tilldelas toohello Programgateway när hello-tjänsten startas.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-185">An IP address is assigned toohello application gateway when hello service starts.</span></span>

## <a name="create-application-gateway-configuration"></a><span data-ttu-id="f7ac1-186">Skapa program gateway-konfiguration</span><span class="sxs-lookup"><span data-stu-id="f7ac1-186">Create application gateway configuration</span></span>

<span data-ttu-id="f7ac1-187">Alla konfigurationsobjekt måste ställas in innan du skapar hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-187">All configuration items must be set up before creating hello application gateway.</span></span> <span data-ttu-id="f7ac1-188">hello med följande steg skapar hello konfigurationsobjekt som behövs för en gateway programresursen.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-188">hello following steps create hello configuration items that are needed for an application gateway resource.</span></span>

### <a name="step-1"></a><span data-ttu-id="f7ac1-189">Steg 1</span><span class="sxs-lookup"><span data-stu-id="f7ac1-189">Step 1</span></span>

<span data-ttu-id="f7ac1-190">Skapa en IP-konfiguration för programgatewayen med namnet **gatewayIP01**.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-190">Create an application gateway IP configuration named **gatewayIP01**.</span></span> <span data-ttu-id="f7ac1-191">När Programgateway startar hämtar en IP-adress från hello-undernät som konfigurerats och dirigera trafik toohello IP-adresser på nätverket i hello backend-IP-adresspool.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-191">When Application Gateway starts, it picks up an IP address from hello subnet configured and route network traffic toohello IP addresses in hello back-end IP pool.</span></span> <span data-ttu-id="f7ac1-192">Tänk på att varje instans använder en IP-adress.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-192">Keep in mind that each instance takes one IP address.</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name "gatewayIP01" -Subnet $appgatewaysubnetdata
```

### <a name="step-2"></a><span data-ttu-id="f7ac1-193">Steg 2</span><span class="sxs-lookup"><span data-stu-id="f7ac1-193">Step 2</span></span>

<span data-ttu-id="f7ac1-194">Konfigurera hello frontend IP-port för hello offentlig IP-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-194">Configure hello front-end IP port for hello public IP endpoint.</span></span> <span data-ttu-id="f7ac1-195">Den här porten är hello som användare ansluta till.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-195">This port is hello port that end users connect to.</span></span>

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "port01"  -Port 443
```
### <a name="step-3"></a><span data-ttu-id="f7ac1-196">Steg 3</span><span class="sxs-lookup"><span data-stu-id="f7ac1-196">Step 3</span></span>

<span data-ttu-id="f7ac1-197">Konfigurera IP-frontend-hello med offentliga IP-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-197">Configure hello front-end IP with public IP endpoint.</span></span>

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-4"></a><span data-ttu-id="f7ac1-198">Steg 4</span><span class="sxs-lookup"><span data-stu-id="f7ac1-198">Step 4</span></span>

<span data-ttu-id="f7ac1-199">Konfigurera hello certifikat för hello Application Gateway används toodecrypt och kryptera hello-trafik som passerar genom.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-199">Configure hello certificate for hello Application Gateway, used toodecrypt and re-encrypt hello traffic passing through.</span></span>

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name "cert01" -CertificateFile <full path too.pfx file> -Password <password for certificate file>
```

### <a name="step-5"></a><span data-ttu-id="f7ac1-200">Steg 5</span><span class="sxs-lookup"><span data-stu-id="f7ac1-200">Step 5</span></span>

<span data-ttu-id="f7ac1-201">Skapa hello HTTP-lyssnare för hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-201">Create hello HTTP listener for hello Application Gateway.</span></span> <span data-ttu-id="f7ac1-202">Tilldela hello frontend IP-konfiguration, port och tooit för ssl-certifikat.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-202">Assign hello front-end IP configuration, port, and ssl certificate tooit.</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $cert
```

### <a name="step-6"></a><span data-ttu-id="f7ac1-203">Steg 6</span><span class="sxs-lookup"><span data-stu-id="f7ac1-203">Step 6</span></span>

<span data-ttu-id="f7ac1-204">Skapa en anpassad avsökningsåtgärd toohello API Management-tjänsten `ContosoApi` proxy domän slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-204">Create a custom probe toohello API Management service `ContosoApi` proxy domain endpoint.</span></span> <span data-ttu-id="f7ac1-205">hello sökvägen `/status-0123456789abcdef` är en standardslutpunkt för hälsotillstånd som finns på alla hello API Management-tjänster.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-205">hello path `/status-0123456789abcdef` is a default health endpoint hosted on all hello API Management services.</span></span> <span data-ttu-id="f7ac1-206">Ange `api.contoso.net` som en anpassad avsökningsåtgärd värdnamn toosecure med SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-206">Set `api.contoso.net` as a custom probe hostname toosecure it with SSL certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="f7ac1-207">Hej hostname `contosoapi.azure-api.net` konfigureras hello standard proxy värdnamn när en tjänst med namnet `contosoapi` skapas i offentliga Azure.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-207">hello hostname `contosoapi.azure-api.net` is hello default proxy hostname configured when a service named `contosoapi` is created in public Azure.</span></span> 
> 

```powershell
$apimprobe = New-AzureRmApplicationGatewayProbeConfig -Name "apimproxyprobe" -Protocol "Https" -HostName "api.contoso.net" -Path "/status-0123456789abcdef" -Interval 30 -Timeout 120 -UnhealthyThreshold 8
```

### <a name="step-7"></a><span data-ttu-id="f7ac1-208">Steg 7</span><span class="sxs-lookup"><span data-stu-id="f7ac1-208">Step 7</span></span>

<span data-ttu-id="f7ac1-209">Överför hello certifikatet toobe används på hello backend SSL-aktiverade resurser.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-209">Upload hello certificate toobe used on hello SSL-enabled backend pool resources.</span></span> <span data-ttu-id="f7ac1-210">Detta är hello samma certifikat som du angav i steg 4 ovan.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-210">This is hello same certificate which you provided in Step 4 above.</span></span>

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name "whitelistcert1" -CertificateFile <full path too.cer file>
```

### <a name="step-8"></a><span data-ttu-id="f7ac1-211">Steg 8</span><span class="sxs-lookup"><span data-stu-id="f7ac1-211">Step 8</span></span>

<span data-ttu-id="f7ac1-212">Konfigurera inställningar för HTTP-serverdelen för hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-212">Configure HTTP backend settings for hello Application Gateway.</span></span> <span data-ttu-id="f7ac1-213">Detta innefattar att ställa in en timeout-gränsen för backend-begäran som de avbryts.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-213">This includes setting a time-out limit for backend request after which they are cancelled.</span></span> <span data-ttu-id="f7ac1-214">Det här värdet skiljer sig från hello avsökningen timeout.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-214">This value is different from hello probe time-out.</span></span>

```powershell
$apimPoolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "apimPoolSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimprobe -AuthenticationCertificates $authcert -RequestTimeout 180
```

### <a name="step-9"></a><span data-ttu-id="f7ac1-215">Steg 9</span><span class="sxs-lookup"><span data-stu-id="f7ac1-215">Step 9</span></span>

<span data-ttu-id="f7ac1-216">Konfigurera backend-IP-adresspool med namnet **apimbackend** med hello interna virtuella IP-adressen för hello API Management-tjänsten skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-216">Configure a back-end IP address pool named **apimbackend**  with hello internal virtual IP address of hello API Management service created above.</span></span>

```powershell
$apimProxyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "apimbackend" -BackendIPAddresses $apimService.StaticIPs[0]
```

### <a name="step-10"></a><span data-ttu-id="f7ac1-217">Steg 10</span><span class="sxs-lookup"><span data-stu-id="f7ac1-217">Step 10</span></span>

<span data-ttu-id="f7ac1-218">Skapa inställningar för en dummy (obefintligt)-serverdel.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-218">Create settings for a dummy (non-existent) backend.</span></span> <span data-ttu-id="f7ac1-219">Begäranden tooAPI sökvägar som vi inte vill tooexpose från API-hantering via Programgateway kommer uppnått denna backend och returnera 404.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-219">Requests tooAPI paths that we do not want tooexpose from API Management via Application Gateway will hit this backend and return 404.</span></span>

<span data-ttu-id="f7ac1-220">Konfigurera HTTP-inställningar för hello dummy serverdel.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-220">Configure HTTP settings for hello dummy backend.</span></span>

```powershell
$dummyBackendSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "dummySetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

<span data-ttu-id="f7ac1-221">Konfigurera en dummy serverdel **dummyBackendPool**, som anger tooa FQDN-adressen **dummybackend.com**. Den här FQDN-adressen finns inte i hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-221">Configure a dummy backend **dummyBackendPool**, which points tooa FQDN address **dummybackend.com**. This FQDN address does not exist in hello virtual network.</span></span>

```powershell
$dummyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "dummyBackendPool" -BackendFqdns "dummybackend.com"
```

<span data-ttu-id="f7ac1-222">Skapa en regel som anger att hello Programgateway används som standard som pekar toohello obefintlig backend **dummybackend.com** i hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-222">Create a rule setting that hello Application Gateway will use by default which points toohello non-existent backend **dummybackend.com** in hello Virtual Network.</span></span>

```powershell
$dummyPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "nonexistentapis" -Paths "/*" -BackendAddressPool $dummyBackendPool -BackendHttpSettings $dummyBackendSetting
```

### <a name="step-11"></a><span data-ttu-id="f7ac1-223">Steg 11</span><span class="sxs-lookup"><span data-stu-id="f7ac1-223">Step 11</span></span>

<span data-ttu-id="f7ac1-224">Konfigurera URL: en regel sökvägar för hello backend-pooler.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-224">Configure URL rule paths for hello back-end pools.</span></span> <span data-ttu-id="f7ac1-225">Detta gör att bara vissa av hello API: er från API-hantering för att vara exponeras toohello offentliga Markera.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-225">This enables selecting only some of hello APIs from API Management for being exposed toohello public.</span></span> <span data-ttu-id="f7ac1-226">Till exempel om det finns `Echo API` (echo-), `Calculator API` (/calc/) o.s.v. Kontrollera endast `Echo API` tillgänglig från Internet).</span><span class="sxs-lookup"><span data-stu-id="f7ac1-226">For example, if there are `Echo API` (/echo/), `Calculator API` (/calc/) etc. make only `Echo API` accessible from Internet).</span></span> 

<span data-ttu-id="f7ac1-227">hello skapas följande exempel en enkel regel för hello ”echo-” sökväg routning trafik toohello serverdel ”apimProxyBackendPool”.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-227">hello following example creates a simple rule for hello "/echo/" path routing traffic toohello back-end "apimProxyBackendPool".</span></span>

```powershell
$echoapiRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "externalapis" -Paths "/echo/*" -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolSetting
```

<span data-ttu-id="f7ac1-228">Om hello sökväg inte matchar hello sökväg regler vi vill tooenable från API Management hello regeln kartan flervägskonfiguration konfigurerar också en standard backend-adresspool med namnet **dummyBackendPool**.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-228">If hello path doesn't match hello path rules we want tooenable from API Management, hello rule path map configuration also configures a default back-end address pool named **dummyBackendPool**.</span></span> <span data-ttu-id="f7ac1-229">Till exempel http://api.contoso.net/calc/ * går för**dummyBackendPool** som har definierats som hello Standardpool för icke matchade trafik.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-229">For example, http://api.contoso.net/calc/* goes too**dummyBackendPool** as it is defined as hello default pool for un-matched traffic.</span></span>

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $echoapiRule, $dummyPathRule -DefaultBackendAddressPool $dummyBackendPool -DefaultBackendHttpSettings $dummyBackendSetting
```

<span data-ttu-id="f7ac1-230">hello senare steg garanterar som endast begär för hello sökvägen ”/ echo” tillåts via hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-230">hello above step ensures that only requests for hello path "/echo" are allowed through hello Application Gateway.</span></span> <span data-ttu-id="f7ac1-231">Begäranden tooother API: er som konfigurerats i API Management genereras 404-fel från Programgateway vid åtkomst från hello Internet.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-231">Requests tooother APIs configured in API Management will throw 404 errors from Application Gateway when accessed from hello Internet.</span></span> 

### <a name="step-12"></a><span data-ttu-id="f7ac1-232">Steg 12</span><span class="sxs-lookup"><span data-stu-id="f7ac1-232">Step 12</span></span>

<span data-ttu-id="f7ac1-233">Skapa en regel inställning för hello Programgateway toouse URL-sökväg-baserad routning.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-233">Create a rule setting for hello Application Gateway toouse URL path-based routing.</span></span>

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-13"></a><span data-ttu-id="f7ac1-234">Steg 13</span><span class="sxs-lookup"><span data-stu-id="f7ac1-234">Step 13</span></span>

<span data-ttu-id="f7ac1-235">Konfigurera hello antal förekomster och storleken för hello Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-235">Configure hello number of instances and size for hello Application Gateway.</span></span> <span data-ttu-id="f7ac1-236">Här använder vi hello [Brandvägg SKU](../application-gateway/application-gateway-webapplicationfirewall-overview.md) för ökad säkerhet för hello API Management-resurs.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-236">Here we are using hello [WAF SKU](../application-gateway/application-gateway-webapplicationfirewall-overview.md) for increased security of hello API Management resource.</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "WAF_Medium" -Tier "WAF" -Capacity 2
```

### <a name="step-14"></a><span data-ttu-id="f7ac1-237">Steg 14</span><span class="sxs-lookup"><span data-stu-id="f7ac1-237">Step 14</span></span>

<span data-ttu-id="f7ac1-238">Konfigurera Brandvägg toobe i ”förebyggande” läge.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-238">Configure WAF toobe in "Prevention" mode.</span></span>
```powershell
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

## <a name="create-application-gateway"></a><span data-ttu-id="f7ac1-239">Skapa Programgateway</span><span class="sxs-lookup"><span data-stu-id="f7ac1-239">Create Application Gateway</span></span>

<span data-ttu-id="f7ac1-240">Skapa en Programgateway med alla hello-konfigurationsobjekt från hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-240">Create an Application Gateway with all hello configuration objects from hello preceding steps.</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name $applicationGatewayName -ResourceGroupName $resourceGroupName  -Location $location -BackendAddressPools $apimProxyBackendPool, $dummyBackendPool -BackendHttpSettingsCollection $apimPoolSetting, $dummyBackendSetting  -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert -Probes $apimprobe
```

## <a name="cname-hello-api-management-proxy-hostname-toohello-public-dns-name-of-hello-application-gateway-resource"></a><span data-ttu-id="f7ac1-241">CNAME hello API Management proxy värdnamn toohello offentliga DNS-namnet på hello Programgateway resurs</span><span class="sxs-lookup"><span data-stu-id="f7ac1-241">CNAME hello API Management proxy hostname toohello public DNS name of hello Application Gateway resource</span></span>

<span data-ttu-id="f7ac1-242">När du har skapat hello gateway är hello nästa steg tooconfigure hello klientdelen för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-242">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="f7ac1-243">När du använder en offentlig IP-adress, kräver Programgateway en dynamiskt tilldelad DNS-namn som inte får enkelt toouse.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-243">When using a public IP, Application Gateway requires a dynamically assigned DNS name, which may not be easy toouse.</span></span> 

<span data-ttu-id="f7ac1-244">hello Application Gateway DNS-namn ska använda toocreate en CNAME-post som pekar hello APIM proxyvärdnamn (t.ex. `api.contoso.net` i hello exemplen ovan) toothis DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-244">hello Application Gateway's DNS name should be used toocreate a CNAME record which points hello APIM proxy host name (e.g. `api.contoso.net` in hello examples above) toothis DNS name.</span></span> <span data-ttu-id="f7ac1-245">tooconfigure hello klientdelens IP-CNAME-post, hämta hello detaljer om hello Programgateway och dess tillhörande IP DNS-namn med hello PublicIPAddress-element.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-245">tooconfigure hello frontend IP CNAME record, retrieve hello details of hello Application Gateway and its associated IP/DNS name using hello PublicIPAddress element.</span></span> <span data-ttu-id="f7ac1-246">hello rekommenderas A-poster inte eftersom hello VIP ändras vid omstart för gateway.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-246">hello use of A-records is not recommended since hello VIP may change on restart of gateway.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -Name "publicIP01"
```

##<span data-ttu-id="f7ac1-247"><a name="summary"></a> Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="f7ac1-247"><a name="summary"> </a> Summary</span></span>
<span data-ttu-id="f7ac1-248">Azure API-hantering som konfigurerats i ett virtuellt nätverk har ett enda gateway-gränssnitt för alla konfigurerade API: er, oavsett om de är värdbaserade lokalt eller i molnet hello.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-248">Azure API Management configured in a VNET provides a single gateway interface for all configured APIs, whether they are hosted on-prem or in hello cloud.</span></span> <span data-ttu-id="f7ac1-249">Integrera Programgateway med API Management ger hello flexibilitet för att selektivt aktivera specifika API: er toobe som är tillgängliga på hello Internet samt ge en brandvägg för webbaserade program som en klientdel tooyour API Management-instans.</span><span class="sxs-lookup"><span data-stu-id="f7ac1-249">Integrating Application Gateway with API Management provides hello flexibility of selectively enabling particular APIs toobe accessible on hello Internet, as well as providing a Web Application Firewall as a frontend tooyour API Management instance.</span></span>

##<span data-ttu-id="f7ac1-250"><a name="next-steps"></a> Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f7ac1-250"><a name="next-steps"> </a> Next steps</span></span>
* <span data-ttu-id="f7ac1-251">Lär dig mer om Azure Programgateway</span><span class="sxs-lookup"><span data-stu-id="f7ac1-251">Learn more about Azure Application Gateway</span></span>
  * [<span data-ttu-id="f7ac1-252">Översikt över Gateway</span><span class="sxs-lookup"><span data-stu-id="f7ac1-252">Application Gateway Overview</span></span>](../application-gateway/application-gateway-introduction.md)
  * [<span data-ttu-id="f7ac1-253">Programmet Gateway Brandvägg för webbaserade program</span><span class="sxs-lookup"><span data-stu-id="f7ac1-253">Application Gateway Web Application Firewall</span></span>](../application-gateway/application-gateway-webapplicationfirewall-overview.md)
  * [<span data-ttu-id="f7ac1-254">Programgateway med sökväg-baserade Routning</span><span class="sxs-lookup"><span data-stu-id="f7ac1-254">Application Gateway using Path-based Routing</span></span>](../application-gateway/application-gateway-create-url-route-arm-ps.md)
* <span data-ttu-id="f7ac1-255">Lär dig mer om API-hantering och Vnet</span><span class="sxs-lookup"><span data-stu-id="f7ac1-255">Learn more about API Management and VNETs</span></span>
  * [<span data-ttu-id="f7ac1-256">Använda API Management bara användas i hello VNET</span><span class="sxs-lookup"><span data-stu-id="f7ac1-256">Using API Management available only within hello VNET</span></span>](api-management-using-with-internal-vnet.md)
  * [<span data-ttu-id="f7ac1-257">Med hjälp av API-hantering i virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="f7ac1-257">Using API Management in VNET</span></span>](api-management-using-with-vnet.md)
