---
title: "Hur du använder Azure API Management i det virtuella nätverket med Programgateway | Microsoft Docs"
description: "Lär dig att installera och konfigurera Azure API Management i internt virtuellt nätverk med programmet Gateway (Brandvägg) som klientdel"
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
ms.openlocfilehash: 8131ded6b74e9c544bf70b1a4659ed07e5def04d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="integrate-api-management-in-an-internal-vnet-with-application-gateway"></a><span data-ttu-id="fbb29-103">Integrera API Management i ett internt virtuellt nätverk med Programgateway</span><span class="sxs-lookup"><span data-stu-id="fbb29-103">Integrate API Management in an internal VNET with Application Gateway</span></span> 

##<span data-ttu-id="fbb29-104"><a name="overview"></a> Översikt</span><span class="sxs-lookup"><span data-stu-id="fbb29-104"><a name="overview"> </a> Overview</span></span>
 
<span data-ttu-id="fbb29-105">API Management-tjänsten kan konfigureras i ett virtuellt nätverk i internt läge vilket gör att det är enbart tillgänglig från det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="fbb29-105">The API Management service can be configured in a Virtual Network in internal mode which makes it accessible only from within the Virtual Network.</span></span> <span data-ttu-id="fbb29-106">Azure Application Gateway är en PAAS-tjänst som tillhandahåller en lager 7 belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="fbb29-106">Azure Application Gateway is a PAAS Service which provides a Layer-7 load balancer.</span></span> <span data-ttu-id="fbb29-107">Den fungerar som en omvänd proxy-tjänst och innehåller bland erbjudande Web Application Firewall (Brandvägg).</span><span class="sxs-lookup"><span data-stu-id="fbb29-107">It acts as a reverse-proxy service and provides among its offering a Web Application Firewall (WAF).</span></span>

<span data-ttu-id="fbb29-108">Kombinera API Management etablerad på ett internt virtuellt nätverk med Programgateway klientdel kan följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="fbb29-108">Combining API Management provisioned in an internal VNET with the Application Gateway frontend enables the following scenarios:</span></span>

* <span data-ttu-id="fbb29-109">Använd samma API Management-resurs för förbrukning av både konsumenter interna och externa användare.</span><span class="sxs-lookup"><span data-stu-id="fbb29-109">Use the same API Management resource for consumption by both internal consumers and external consumers.</span></span>
* <span data-ttu-id="fbb29-110">Använda en enskild resurs för API Management och har en delmängd av API: er som definierats i API-hantering som är tillgänglig för externa användare.</span><span class="sxs-lookup"><span data-stu-id="fbb29-110">Use a single API Management resource and have a subset of APIs defined in API Management available for external consumers.</span></span>
* <span data-ttu-id="fbb29-111">Ger en nyckelfärdig sätt att aktivera åtkomst till API-hantering från Internet och inaktivera.</span><span class="sxs-lookup"><span data-stu-id="fbb29-111">Provide a turn-key way to switch access to API Management from the public Internet on and off.</span></span> 

##<span data-ttu-id="fbb29-112"><a name="scenario"></a> Scenario</span><span class="sxs-lookup"><span data-stu-id="fbb29-112"><a name="scenario"> </a> Scenario</span></span>
<span data-ttu-id="fbb29-113">Den här artikeln beskriver hur du använder en enda API Management-tjänst för både interna och externa användare och att den fungerar som en enda klientdel för både lokala och molntjänster API: er.</span><span class="sxs-lookup"><span data-stu-id="fbb29-113">This article will cover how to use a single API Management service for both internal and external consumers and make it act as a single frontend for both on-prem and cloud APIs.</span></span> <span data-ttu-id="fbb29-114">Du kan även se hur du exponera en delmängd av dina API: er (i exemplet som de är markerade i grönt) för extern användning med PathBasedRouting tillgängliga funktioner i Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="fbb29-114">You will also see how to expose only a subset of your APIs (in the example they are highlighted in green) for External Consumption using the PathBasedRouting functionality available in Application Gateway.</span></span>

<span data-ttu-id="fbb29-115">I det första exemplet installationsprogrammet hanteras alla API: er endast från i ditt virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="fbb29-115">In the first setup example all your APIs are managed only from within your Virtual Network.</span></span> <span data-ttu-id="fbb29-116">Interna användare (markerade i orange) kan komma åt alla dina interna och externa API: er.</span><span class="sxs-lookup"><span data-stu-id="fbb29-116">Internal consumers (highlighted in orange) can access all your internal and external APIs.</span></span> <span data-ttu-id="fbb29-117">Trafik överförs aldrig till Internet, en hög prestanda levereras via Expressroute-kretsar.</span><span class="sxs-lookup"><span data-stu-id="fbb29-117">Traffic never goes out to Internet a high performance is delivered via Express Route circuits.</span></span>

![URL-väg](./media/api-management-howto-integrate-internal-vnet-appgateway/api-management-howto-integrate-internal-vnet-appgateway.png)

## <span data-ttu-id="fbb29-119"><a name="before-you-begin"></a> Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="fbb29-119"><a name="before-you-begin"> </a> Before you begin</span></span>

1. <span data-ttu-id="fbb29-120">Installera den senaste versionen av Azure PowerShell-cmdlets med hjälp av installationsprogrammet för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="fbb29-120">Install the latest version of the Azure PowerShell cmdlets by using the Web Platform Installer.</span></span> <span data-ttu-id="fbb29-121">Du kan hämta och installera den senaste versionen från avsnittet om **Windows PowerShell** på [hämtningssidan](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="fbb29-121">You can download and install the latest version from the **Windows PowerShell** section of the [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="fbb29-122">Skapa ett virtuellt nätverk och skapa separata undernät för API Management och Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="fbb29-122">Create a Virtual Network and create separate subnets for API Management and Application Gateway.</span></span> 
3. <span data-ttu-id="fbb29-123">Om du vill skapa en anpassad DNS-server för det virtuella nätverket kan du göra det innan du påbörjar distribueringen.</span><span class="sxs-lookup"><span data-stu-id="fbb29-123">If you intend to create a custom DNS server for the Virtual Network, do so before starting the deployment.</span></span> <span data-ttu-id="fbb29-124">Kontrollera det fungerar genom att säkerställa att en virtuell dator som skapats i ett nytt undernät i det virtuella nätverket kan matcha och åtkomst till alla Azure-Tjänsteslutpunkter.</span><span class="sxs-lookup"><span data-stu-id="fbb29-124">Double check it works by ensuring a virtual machine created in a new subnet in the Virtual Network can resolve and access all Azure service endpoints.</span></span>

## <a name="what-is-required-to-create-an-integration-between-api-management-and-application-gateway"></a><span data-ttu-id="fbb29-125">Vad krävs för att skapa en integration mellan API Management och Programgateway?</span><span class="sxs-lookup"><span data-stu-id="fbb29-125">What is required to create an integration between API Management and Application Gateway?</span></span>

* <span data-ttu-id="fbb29-126">**Backend-serverpoolen:** interna virtuella IP-adress för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="fbb29-126">**Back-end server pool:** This is the internal virtual IP address of the API Management service.</span></span>
* <span data-ttu-id="fbb29-127">**Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet.</span><span class="sxs-lookup"><span data-stu-id="fbb29-127">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="fbb29-128">Dessa inställningar tillämpas på alla servrar i poolen.</span><span class="sxs-lookup"><span data-stu-id="fbb29-128">These settings are applied to all servers within the pool.</span></span>
* <span data-ttu-id="fbb29-129">**Frontend-port:** den offentliga porten som öppnas på programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="fbb29-129">**Front-end port:** This is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="fbb29-130">Träffa den trafik omdirigeras till en backend-server.</span><span class="sxs-lookup"><span data-stu-id="fbb29-130">Traffic hitting it gets redirected to one of the back-end servers.</span></span>
* <span data-ttu-id="fbb29-131">**Lyssnare:** Lyssnaren har en frontend-port, ett protokoll (Http eller Https; dessa värden är skiftlägeskänsliga) och SSL-certifikatnamnet (om du konfigurerar SSL-avlastning).</span><span class="sxs-lookup"><span data-stu-id="fbb29-131">**Listener:** The listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="fbb29-132">**Regel:** regeln Binder en lyssnare till en pool med backend-server.</span><span class="sxs-lookup"><span data-stu-id="fbb29-132">**Rule:** The rule binds a listener to a back-end server pool.</span></span>
* <span data-ttu-id="fbb29-133">**Anpassade Hälsoavsökningen:** Application Gateway som standard använder IP-adress baserat avsökningar för att ta reda på vilka servrar i BackendAddressPool är aktiva.</span><span class="sxs-lookup"><span data-stu-id="fbb29-133">**Custom Health Probe:** Application Gateway, by default, uses IP address based probes to figure out which servers in the BackendAddressPool are active.</span></span> <span data-ttu-id="fbb29-134">API-hantering är tjänsten bara svarar på begäranden som har rätt värdadressen därför standard avsökningar misslyckas.</span><span class="sxs-lookup"><span data-stu-id="fbb29-134">The API Management service only responds to requests which have the correct host header, hence the default probes fail.</span></span> <span data-ttu-id="fbb29-135">En anpassad hälsoavsökningen måste definieras för att bestämma att tjänsten är igång och att den ska vidarebefordra begäranden Programgateway.</span><span class="sxs-lookup"><span data-stu-id="fbb29-135">A custom health probe needs to be defined to help application gateway determine that the service is alive and it should forward requests.</span></span>
* <span data-ttu-id="fbb29-136">**Certifikatet för domänen:** åtkomst till API-hantering från internet måste du skapa en CNAME-mappning av dess värdnamn Programgateway frontend DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="fbb29-136">**Custom domain certificate:** To access API Management from the internet you need to create a CNAME mapping of its hostname to the Application Gateway front-end DNS name.</span></span> <span data-ttu-id="fbb29-137">Detta garanterar att hostname sidhuvud och certifikat som skickats till Programgateway som vidarebefordras till API-hantering är en APIM kan identifiera som giltig.</span><span class="sxs-lookup"><span data-stu-id="fbb29-137">This ensures that the hostname header and certificate sent to Application Gateway that is forwarded to API Management is one APIM can recognize as valid.</span></span>

## <span data-ttu-id="fbb29-138"><a name="overview-steps"></a> Steg som krävs för att integrera API Management och Programgateway</span><span class="sxs-lookup"><span data-stu-id="fbb29-138"><a name="overview-steps"> </a> Steps required for integrating API Management and Application Gateway</span></span> 

1. <span data-ttu-id="fbb29-139">Skapa en resursgrupp för Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="fbb29-139">Create a resource group for Resource Manager.</span></span>
2. <span data-ttu-id="fbb29-140">Skapa ett virtuellt nätverk och undernät offentliga IP-Adressen för Programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="fbb29-140">Create a Virtual Network, subnet, and public IP for the Application Gateway.</span></span> <span data-ttu-id="fbb29-141">Skapa ett annat undernät för API-hantering.</span><span class="sxs-lookup"><span data-stu-id="fbb29-141">Create another subnet for API Management.</span></span>
3. <span data-ttu-id="fbb29-142">Skapa en API Management-tjänst i VNET-undernätet som skapade ovan och se till att du använder intern-läget.</span><span class="sxs-lookup"><span data-stu-id="fbb29-142">Create an API Management service inside the VNET subnet created above and ensure you use the Internal mode.</span></span>
4. <span data-ttu-id="fbb29-143">Konfigurera det anpassade domännamnet i API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="fbb29-143">Setup the custom domain name in the API Management service.</span></span>
5. <span data-ttu-id="fbb29-144">Skapa ett konfigurationsobjekt för Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="fbb29-144">Create an Application Gateway configuration object.</span></span>
6. <span data-ttu-id="fbb29-145">Skapa en Programgateway resurs.</span><span class="sxs-lookup"><span data-stu-id="fbb29-145">Create an Application Gateway resource.</span></span>
7. <span data-ttu-id="fbb29-146">Skapa en CNAME-post från det offentliga DNS-namnet för Programgateway till API-hantering proxy värdnamn.</span><span class="sxs-lookup"><span data-stu-id="fbb29-146">Create a CNAME from the public DNS name of the Application Gateway to the API Management proxy hostname.</span></span>

## <a name="create-a-resource-group-for-resource-manager"></a><span data-ttu-id="fbb29-147">Skapa en resursgrupp för Resource Manager</span><span class="sxs-lookup"><span data-stu-id="fbb29-147">Create a resource group for Resource Manager</span></span>

<span data-ttu-id="fbb29-148">Kontrollera att du använder den senaste versionen av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fbb29-148">Make sure that you are using the latest version of Azure PowerShell.</span></span> <span data-ttu-id="fbb29-149">Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="fbb29-149">More info is available at [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

### <a name="step-1"></a><span data-ttu-id="fbb29-150">Steg 1</span><span class="sxs-lookup"><span data-stu-id="fbb29-150">Step 1</span></span>

<span data-ttu-id="fbb29-151">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="fbb29-151">Log in to Azure</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="fbb29-152">Autentisera med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="fbb29-152">Authenticate with your credentials.</span></span><BR>

### <a name="step-2"></a><span data-ttu-id="fbb29-153">Steg 2</span><span class="sxs-lookup"><span data-stu-id="fbb29-153">Step 2</span></span>

<span data-ttu-id="fbb29-154">Kontrollera prenumerationerna för kontot och markera den.</span><span class="sxs-lookup"><span data-stu-id="fbb29-154">Check the subscriptions for the account and select it.</span></span>

```powershell
Get-AzureRmSubscription -Subscriptionid "GUID of subscription" | Select-AzureRmSubscription
```

### <a name="step-3"></a><span data-ttu-id="fbb29-155">Steg 3</span><span class="sxs-lookup"><span data-stu-id="fbb29-155">Step 3</span></span>

<span data-ttu-id="fbb29-156">Skapa en resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp).</span><span class="sxs-lookup"><span data-stu-id="fbb29-156">Create a resource group (skip this step if you're using an existing resource group).</span></span>

```powershell
New-AzureRmResourceGroup -Name "apim-appGw-RG" -Location "West US"
```
<span data-ttu-id="fbb29-157">Azure Resource Manager kräver att alla resursgrupper anger en plats.</span><span class="sxs-lookup"><span data-stu-id="fbb29-157">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="fbb29-158">Detta används som standardplatsen för resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="fbb29-158">This is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="fbb29-159">Se till att alla kommandon för att skapa en Programgateway använder samma resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="fbb29-159">Make sure that all commands to create an application gateway use the same resource group.</span></span>

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a><span data-ttu-id="fbb29-160">Skapa ett virtuellt nätverk och ett undernät för Programgateway</span><span class="sxs-lookup"><span data-stu-id="fbb29-160">Create a Virtual Network and a subnet for the application gateway</span></span>

<span data-ttu-id="fbb29-161">I följande exempel visas hur du skapar ett virtuellt nätverk med resursen manager.</span><span class="sxs-lookup"><span data-stu-id="fbb29-161">The following example shows how to create a Virtual Network using the resource manager.</span></span>

### <a name="step-1"></a><span data-ttu-id="fbb29-162">Steg 1</span><span class="sxs-lookup"><span data-stu-id="fbb29-162">Step 1</span></span>

<span data-ttu-id="fbb29-163">Tilldela variabeln undernät som ska användas för Programgateway när du skapar ett virtuellt nätverk adressintervallet 10.0.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="fbb29-163">Assign the address range 10.0.0.0/24 to the subnet variable to be used for Application Gateway while creating a Virtual Network.</span></span>

```powershell
$appgatewaysubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim01" -AddressPrefix "10.0.0.0/24"
```

### <a name="step-2"></a><span data-ttu-id="fbb29-164">Steg 2</span><span class="sxs-lookup"><span data-stu-id="fbb29-164">Step 2</span></span>

<span data-ttu-id="fbb29-165">Tilldela adressintervallet 10.0.1.0/24 variabeln undernät som ska användas för API Management när du skapar ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="fbb29-165">Assign the address range 10.0.1.0/24 to the subnet variable to be used for API Management while creating a Virtual Network.</span></span>

```powershell
$apimsubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim02" -AddressPrefix "10.0.1.0/24"
```

### <a name="step-3"></a><span data-ttu-id="fbb29-166">Steg 3</span><span class="sxs-lookup"><span data-stu-id="fbb29-166">Step 3</span></span>

<span data-ttu-id="fbb29-167">Skapa ett virtuellt nätverk med namnet **appgwvnet** i resursgruppen **apim-appGw-RG** för regionen USA, västra med prefixet 10.0.0.0/16 med undernät 10.0.0.0/24 och 10.0.1.0/24.</span><span class="sxs-lookup"><span data-stu-id="fbb29-167">Create a Virtual Network named **appgwvnet** in resource group **apim-appGw-RG** for the West US region using the prefix 10.0.0.0/16 with subnets 10.0.0.0/24 and 10.0.1.0/24.</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork -Name "appgwvnet" -ResourceGroupName "apim-appGw-RG" -Location "West US" -AddressPrefix "10.0.0.0/16" -Subnet $appgatewaysubnet,$apimsubnet
```

### <a name="step-4"></a><span data-ttu-id="fbb29-168">Steg 4</span><span class="sxs-lookup"><span data-stu-id="fbb29-168">Step 4</span></span>

<span data-ttu-id="fbb29-169">Tilldela en variabel för undernätet i nästa steg</span><span class="sxs-lookup"><span data-stu-id="fbb29-169">Assign a subnet variable for the next steps</span></span>

```powershell
$appgatewaysubnetdata=$vnet.Subnets[0]
$apimsubnetdata=$vnet.Subnets[1]
```
## <a name="create-an-api-management-service-inside-a-vnet-configured-in-internal-mode"></a><span data-ttu-id="fbb29-170">Skapa en API Management-tjänst i ett virtuellt nätverk som konfigurerats i internt läge</span><span class="sxs-lookup"><span data-stu-id="fbb29-170">Create an API Management service inside a VNET configured in internal mode</span></span>

<span data-ttu-id="fbb29-171">I följande exempel visas hur du skapar en API Management-tjänst i ett virtuellt nätverk som konfigurerats för intern åtkomst.</span><span class="sxs-lookup"><span data-stu-id="fbb29-171">The following example shows how to create an API Management service in a VNET configured for internal access only.</span></span>

### <a name="step-1"></a><span data-ttu-id="fbb29-172">Steg 1</span><span class="sxs-lookup"><span data-stu-id="fbb29-172">Step 1</span></span>
<span data-ttu-id="fbb29-173">Skapa ett virtuellt nätverk för API Management-objekt med undernätet $apimsubnetdata skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="fbb29-173">Create an API Management Virtual Network object using the subnet $apimsubnetdata created above.</span></span>

```powershell
$apimVirtualNetwork = New-AzureRmApiManagementVirtualNetwork -Location "West US" -SubnetResourceId $apimsubnetdata.Id
```
### <a name="step-2"></a><span data-ttu-id="fbb29-174">Steg 2</span><span class="sxs-lookup"><span data-stu-id="fbb29-174">Step 2</span></span>
<span data-ttu-id="fbb29-175">Skapa en API Management-tjänsten i det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="fbb29-175">Create an API Management service inside the Virtual Network.</span></span>

```powershell
$apimService = New-AzureRmApiManagement -ResourceGroupName "apim-appGw-RG" -Location "West US" -Name "ContosoApi" -Organization "Contoso" -AdminEmail "admin@contoso.com" -VirtualNetwork $apimVirtualNetwork -VpnType "Internal" -Sku "Developer"
```
<span data-ttu-id="fbb29-176">När ovanstående kommando lyckas avser [DNS-konfiguration som krävs för att komma åt interna virtuella nätverk API Management-tjänsten](api-management-using-with-internal-vnet.md#apim-dns-configuration) att komma åt den.</span><span class="sxs-lookup"><span data-stu-id="fbb29-176">After the above command succeeds refer to [DNS Configuration required to access internal VNET API Management service](api-management-using-with-internal-vnet.md#apim-dns-configuration) to access it.</span></span>

## <a name="set-up-a-custom-domain-name-in-api-management"></a><span data-ttu-id="fbb29-177">Ställa in ett anpassat domännamn i API Management</span><span class="sxs-lookup"><span data-stu-id="fbb29-177">Set-up a custom domain name in API Management</span></span>

### <a name="step-1"></a><span data-ttu-id="fbb29-178">Steg 1</span><span class="sxs-lookup"><span data-stu-id="fbb29-178">Step 1</span></span>
<span data-ttu-id="fbb29-179">Överför certifikatet med privat nyckel för domänen.</span><span class="sxs-lookup"><span data-stu-id="fbb29-179">Upload the certificate with private key for the domain.</span></span> <span data-ttu-id="fbb29-180">Det här exemplet blir `*.contoso.net`.</span><span class="sxs-lookup"><span data-stu-id="fbb29-180">For this example it will be `*.contoso.net`.</span></span> 

```powershell
$certUploadResult = Import-AzureRmApiManagementHostnameCertificate -ResourceGroupName "apim-appGw-RG" -Name "ContosoApi" -HostnameType "Proxy" -PfxPath <full path to .pfx file> -PfxPassword <password for certificate file> -PassThru
```

### <a name="step-2"></a><span data-ttu-id="fbb29-181">Steg 2</span><span class="sxs-lookup"><span data-stu-id="fbb29-181">Step 2</span></span>
<span data-ttu-id="fbb29-182">När certifikatet har överförts, skapa ett konfigurationsobjekt för värdnamn för proxy med ett värdnamn för `api.contoso.net`, enligt exemplet certifikatet ger behörighet för den `*.contoso.net` domän.</span><span class="sxs-lookup"><span data-stu-id="fbb29-182">Once the certificate is uploaded, create a hostname configuration object for the proxy with a hostname of `api.contoso.net`, as the example certificate provides authority for the  `*.contoso.net` domain.</span></span> 

```powershell
$proxyHostnameConfig = New-AzureRmApiManagementHostnameConfiguration -CertificateThumbprint $certUploadResult.Thumbprint -Hostname "api.contoso.net"
$result = Set-AzureRmApiManagementHostnames -Name "ContosoApi" -ResourceGroupName "apim-appGw-RG" -ProxyHostnameConfiguration $proxyHostnameConfig
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a><span data-ttu-id="fbb29-183">Skapa en offentlig IP-adress för frontend-konfigurationen</span><span class="sxs-lookup"><span data-stu-id="fbb29-183">Create a public IP address for the front-end configuration</span></span>

<span data-ttu-id="fbb29-184">Skapa en offentlig IP-resurs **publicIP01** i resursgruppen **apim-appGw-RG** för regionen USA, västra.</span><span class="sxs-lookup"><span data-stu-id="fbb29-184">Create a public IP resource **publicIP01** in resource group **apim-appGw-RG** for the West US region.</span></span>

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -name "publicIP01" -location "West US" -AllocationMethod Dynamic
```

<span data-ttu-id="fbb29-185">En IP-adress tilldelas till programgatewayen när tjänsten startas.</span><span class="sxs-lookup"><span data-stu-id="fbb29-185">An IP address is assigned to the application gateway when the service starts.</span></span>

## <a name="create-application-gateway-configuration"></a><span data-ttu-id="fbb29-186">Skapa program gateway-konfiguration</span><span class="sxs-lookup"><span data-stu-id="fbb29-186">Create application gateway configuration</span></span>

<span data-ttu-id="fbb29-187">Alla konfigurationsobjekt måste konfigureras innan programgatewayen skapas.</span><span class="sxs-lookup"><span data-stu-id="fbb29-187">All configuration items must be set up before creating the application gateway.</span></span> <span data-ttu-id="fbb29-188">Följande steg skapar konfigurationsobjekten som behövs för en programgatewayresurs.</span><span class="sxs-lookup"><span data-stu-id="fbb29-188">The following steps create the configuration items that are needed for an application gateway resource.</span></span>

### <a name="step-1"></a><span data-ttu-id="fbb29-189">Steg 1</span><span class="sxs-lookup"><span data-stu-id="fbb29-189">Step 1</span></span>

<span data-ttu-id="fbb29-190">Skapa en IP-konfiguration för programgatewayen med namnet **gatewayIP01**.</span><span class="sxs-lookup"><span data-stu-id="fbb29-190">Create an application gateway IP configuration named **gatewayIP01**.</span></span> <span data-ttu-id="fbb29-191">När Application Gateway startar hämtar den en IP-adress från det konfigurerade undernätet och dirigerar nätverkstrafik till IP-adresserna i backend-IP-poolen.</span><span class="sxs-lookup"><span data-stu-id="fbb29-191">When Application Gateway starts, it picks up an IP address from the subnet configured and route network traffic to the IP addresses in the back-end IP pool.</span></span> <span data-ttu-id="fbb29-192">Tänk på att varje instans använder en IP-adress.</span><span class="sxs-lookup"><span data-stu-id="fbb29-192">Keep in mind that each instance takes one IP address.</span></span>

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name "gatewayIP01" -Subnet $appgatewaysubnetdata
```

### <a name="step-2"></a><span data-ttu-id="fbb29-193">Steg 2</span><span class="sxs-lookup"><span data-stu-id="fbb29-193">Step 2</span></span>

<span data-ttu-id="fbb29-194">Konfigurera frontend IP-port för den offentliga IP-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="fbb29-194">Configure the front-end IP port for the public IP endpoint.</span></span> <span data-ttu-id="fbb29-195">Den här porten är den port som användarna ansluter till.</span><span class="sxs-lookup"><span data-stu-id="fbb29-195">This port is the port that end users connect to.</span></span>

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "port01"  -Port 443
```
### <a name="step-3"></a><span data-ttu-id="fbb29-196">Steg 3</span><span class="sxs-lookup"><span data-stu-id="fbb29-196">Step 3</span></span>

<span data-ttu-id="fbb29-197">Konfigurera klientdelens IP med den offentliga IP-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="fbb29-197">Configure the front-end IP with public IP endpoint.</span></span>

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-4"></a><span data-ttu-id="fbb29-198">Steg 4</span><span class="sxs-lookup"><span data-stu-id="fbb29-198">Step 4</span></span>

<span data-ttu-id="fbb29-199">Konfigurera certifikatet för Programgateway används för att dekryptera och kryptera trafik som passerar genom.</span><span class="sxs-lookup"><span data-stu-id="fbb29-199">Configure the certificate for the Application Gateway, used to decrypt and re-encrypt the traffic passing through.</span></span>

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name "cert01" -CertificateFile <full path to .pfx file> -Password <password for certificate file>
```

### <a name="step-5"></a><span data-ttu-id="fbb29-200">Steg 5</span><span class="sxs-lookup"><span data-stu-id="fbb29-200">Step 5</span></span>

<span data-ttu-id="fbb29-201">Skapa HTTP-lyssnare för Programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="fbb29-201">Create the HTTP listener for the Application Gateway.</span></span> <span data-ttu-id="fbb29-202">Tilldela frontend IP-konfiguration, port och ssl-certifikatet till den.</span><span class="sxs-lookup"><span data-stu-id="fbb29-202">Assign the front-end IP configuration, port, and ssl certificate to it.</span></span>

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $cert
```

### <a name="step-6"></a><span data-ttu-id="fbb29-203">Steg 6</span><span class="sxs-lookup"><span data-stu-id="fbb29-203">Step 6</span></span>

<span data-ttu-id="fbb29-204">Skapa en anpassad avsökning till API Management-tjänsten `ContosoApi` proxy domän slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="fbb29-204">Create a custom probe to the API Management service `ContosoApi` proxy domain endpoint.</span></span> <span data-ttu-id="fbb29-205">Sökvägen `/status-0123456789abcdef` är en standardslutpunkt hälsa som är värd för API Management-tjänster.</span><span class="sxs-lookup"><span data-stu-id="fbb29-205">The path `/status-0123456789abcdef` is a default health endpoint hosted on all the API Management services.</span></span> <span data-ttu-id="fbb29-206">Ange `api.contoso.net` som ett värdnamn för anpassad avsökningsåtgärd för säker med SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="fbb29-206">Set `api.contoso.net` as a custom probe hostname to secure it with SSL certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="fbb29-207">Värdnamnet `contosoapi.azure-api.net` är standard proxy värdnamnet konfigureras när en tjänst med namnet `contosoapi` skapas i offentliga Azure.</span><span class="sxs-lookup"><span data-stu-id="fbb29-207">The hostname `contosoapi.azure-api.net` is the default proxy hostname configured when a service named `contosoapi` is created in public Azure.</span></span> 
> 

```powershell
$apimprobe = New-AzureRmApplicationGatewayProbeConfig -Name "apimproxyprobe" -Protocol "Https" -HostName "api.contoso.net" -Path "/status-0123456789abcdef" -Interval 30 -Timeout 120 -UnhealthyThreshold 8
```

### <a name="step-7"></a><span data-ttu-id="fbb29-208">Steg 7</span><span class="sxs-lookup"><span data-stu-id="fbb29-208">Step 7</span></span>

<span data-ttu-id="fbb29-209">Överför certifikatet som ska användas på resurserna som SSL-aktiverade backend-pool.</span><span class="sxs-lookup"><span data-stu-id="fbb29-209">Upload the certificate to be used on the SSL-enabled backend pool resources.</span></span> <span data-ttu-id="fbb29-210">Det här är samma certifikat som du angav i steg 4 ovan.</span><span class="sxs-lookup"><span data-stu-id="fbb29-210">This is the same certificate which you provided in Step 4 above.</span></span>

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name "whitelistcert1" -CertificateFile <full path to .cer file>
```

### <a name="step-8"></a><span data-ttu-id="fbb29-211">Steg 8</span><span class="sxs-lookup"><span data-stu-id="fbb29-211">Step 8</span></span>

<span data-ttu-id="fbb29-212">Konfigurera inställningar för HTTP-serverdelen för Programgateway.</span><span class="sxs-lookup"><span data-stu-id="fbb29-212">Configure HTTP backend settings for the Application Gateway.</span></span> <span data-ttu-id="fbb29-213">Detta innefattar att ställa in en timeout-gränsen för backend-begäran som de avbryts.</span><span class="sxs-lookup"><span data-stu-id="fbb29-213">This includes setting a time-out limit for backend request after which they are cancelled.</span></span> <span data-ttu-id="fbb29-214">Det här värdet skiljer sig från avsökningen timeout-värdet.</span><span class="sxs-lookup"><span data-stu-id="fbb29-214">This value is different from the probe time-out.</span></span>

```powershell
$apimPoolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "apimPoolSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimprobe -AuthenticationCertificates $authcert -RequestTimeout 180
```

### <a name="step-9"></a><span data-ttu-id="fbb29-215">Steg 9</span><span class="sxs-lookup"><span data-stu-id="fbb29-215">Step 9</span></span>

<span data-ttu-id="fbb29-216">Konfigurera backend-IP-adresspool med namnet **apimbackend** med den interna virtuella IP-adressen för API Management-tjänsten skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="fbb29-216">Configure a back-end IP address pool named **apimbackend**  with the internal virtual IP address of the API Management service created above.</span></span>

```powershell
$apimProxyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "apimbackend" -BackendIPAddresses $apimService.StaticIPs[0]
```

### <a name="step-10"></a><span data-ttu-id="fbb29-217">Steg 10</span><span class="sxs-lookup"><span data-stu-id="fbb29-217">Step 10</span></span>

<span data-ttu-id="fbb29-218">Skapa inställningar för en dummy (obefintligt)-serverdel.</span><span class="sxs-lookup"><span data-stu-id="fbb29-218">Create settings for a dummy (non-existent) backend.</span></span> <span data-ttu-id="fbb29-219">Begäranden till API-sökvägar som vi inte vill exponera från API-hantering via Programgateway nått den här backend och returnera 404.</span><span class="sxs-lookup"><span data-stu-id="fbb29-219">Requests to API paths that we do not want to expose from API Management via Application Gateway will hit this backend and return 404.</span></span>

<span data-ttu-id="fbb29-220">Konfigurera HTTP-inställningar för dummy backend.</span><span class="sxs-lookup"><span data-stu-id="fbb29-220">Configure HTTP settings for the dummy backend.</span></span>

```powershell
$dummyBackendSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "dummySetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

<span data-ttu-id="fbb29-221">Konfigurera en dummy serverdel **dummyBackendPool**, som pekar på ett FQDN-adressen **dummybackend.com**. Den här FQDN-adressen finns inte i det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="fbb29-221">Configure a dummy backend **dummyBackendPool**, which points to a FQDN address **dummybackend.com**. This FQDN address does not exist in the virtual network.</span></span>

```powershell
$dummyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "dummyBackendPool" -BackendFqdns "dummybackend.com"
```

<span data-ttu-id="fbb29-222">Skapa en regel inställning som Application Gateway ska använda som standard som pekar på obefintligt serverdelen **dummybackend.com** i det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="fbb29-222">Create a rule setting that the Application Gateway will use by default which points to the non-existent backend **dummybackend.com** in the Virtual Network.</span></span>

```powershell
$dummyPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "nonexistentapis" -Paths "/*" -BackendAddressPool $dummyBackendPool -BackendHttpSettings $dummyBackendSetting
```

### <a name="step-11"></a><span data-ttu-id="fbb29-223">Steg 11</span><span class="sxs-lookup"><span data-stu-id="fbb29-223">Step 11</span></span>

<span data-ttu-id="fbb29-224">Konfigurera URL: en regel sökvägar för backend-pooler.</span><span class="sxs-lookup"><span data-stu-id="fbb29-224">Configure URL rule paths for the back-end pools.</span></span> <span data-ttu-id="fbb29-225">Detta gör att välja endast en del av API: er i API-hantering för att exponeras för allmänheten.</span><span class="sxs-lookup"><span data-stu-id="fbb29-225">This enables selecting only some of the APIs from API Management for being exposed to the public.</span></span> <span data-ttu-id="fbb29-226">Till exempel om det finns `Echo API` (echo-), `Calculator API` (/calc/) o.s.v. Kontrollera endast `Echo API` tillgänglig från Internet).</span><span class="sxs-lookup"><span data-stu-id="fbb29-226">For example, if there are `Echo API` (/echo/), `Calculator API` (/calc/) etc. make only `Echo API` accessible from Internet).</span></span> 

<span data-ttu-id="fbb29-227">I följande exempel skapas en enkel regel för ”echo-” sökväg routning trafiken till backend ”apimProxyBackendPool”.</span><span class="sxs-lookup"><span data-stu-id="fbb29-227">The following example creates a simple rule for the "/echo/" path routing traffic to the back-end "apimProxyBackendPool".</span></span>

```powershell
$echoapiRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "externalapis" -Paths "/echo/*" -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolSetting
```

<span data-ttu-id="fbb29-228">Om sökvägen inte matchar sökvägsregler som vi vill aktivera API-hantering, konfigurerar regelkonfigurationen sökväg kartan också en standard backend-adresspool med namnet **dummyBackendPool**.</span><span class="sxs-lookup"><span data-stu-id="fbb29-228">If the path doesn't match the path rules we want to enable from API Management, the rule path map configuration also configures a default back-end address pool named **dummyBackendPool**.</span></span> <span data-ttu-id="fbb29-229">Till exempel http://api.contoso.net/calc/ * går till **dummyBackendPool** som har definierats som standard för icke matchade trafik.</span><span class="sxs-lookup"><span data-stu-id="fbb29-229">For example, http://api.contoso.net/calc/* goes to **dummyBackendPool** as it is defined as the default pool for un-matched traffic.</span></span>

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $echoapiRule, $dummyPathRule -DefaultBackendAddressPool $dummyBackendPool -DefaultBackendHttpSettings $dummyBackendSetting
```

<span data-ttu-id="fbb29-230">Ovanstående steg garanterar som endast begäranden om sökvägen ”/ echo” tillåts via Programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="fbb29-230">The above step ensures that only requests for the path "/echo" are allowed through the Application Gateway.</span></span> <span data-ttu-id="fbb29-231">Förfrågningar till andra API: er som konfigurerats i API Management genereras 404-fel från Programgateway när nås från Internet.</span><span class="sxs-lookup"><span data-stu-id="fbb29-231">Requests to other APIs configured in API Management will throw 404 errors from Application Gateway when accessed from the Internet.</span></span> 

### <a name="step-12"></a><span data-ttu-id="fbb29-232">Steg 12</span><span class="sxs-lookup"><span data-stu-id="fbb29-232">Step 12</span></span>

<span data-ttu-id="fbb29-233">Skapa en regel inställning för Programgateway att använda URL-sökväg-baserad routning.</span><span class="sxs-lookup"><span data-stu-id="fbb29-233">Create a rule setting for the Application Gateway to use URL path-based routing.</span></span>

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-13"></a><span data-ttu-id="fbb29-234">Steg 13</span><span class="sxs-lookup"><span data-stu-id="fbb29-234">Step 13</span></span>

<span data-ttu-id="fbb29-235">Konfigurera antalet instanser och storlek för Programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="fbb29-235">Configure the number of instances and size for the Application Gateway.</span></span> <span data-ttu-id="fbb29-236">Här använder vi den [Brandvägg SKU](../application-gateway/application-gateway-webapplicationfirewall-overview.md) för ökad säkerhet för API Management-resurs.</span><span class="sxs-lookup"><span data-stu-id="fbb29-236">Here we are using the [WAF SKU](../application-gateway/application-gateway-webapplicationfirewall-overview.md) for increased security of the API Management resource.</span></span>

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "WAF_Medium" -Tier "WAF" -Capacity 2
```

### <a name="step-14"></a><span data-ttu-id="fbb29-237">Steg 14</span><span class="sxs-lookup"><span data-stu-id="fbb29-237">Step 14</span></span>

<span data-ttu-id="fbb29-238">Konfigurera Brandvägg i ”förebyggande” läge.</span><span class="sxs-lookup"><span data-stu-id="fbb29-238">Configure WAF to be in "Prevention" mode.</span></span>
```powershell
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

## <a name="create-application-gateway"></a><span data-ttu-id="fbb29-239">Skapa Programgateway</span><span class="sxs-lookup"><span data-stu-id="fbb29-239">Create Application Gateway</span></span>

<span data-ttu-id="fbb29-240">Skapa en Programgateway med alla konfigurationsobjekt från föregående steg.</span><span class="sxs-lookup"><span data-stu-id="fbb29-240">Create an Application Gateway with all the configuration objects from the preceding steps.</span></span>

```powershell
$appgw = New-AzureRmApplicationGateway -Name $applicationGatewayName -ResourceGroupName $resourceGroupName  -Location $location -BackendAddressPools $apimProxyBackendPool, $dummyBackendPool -BackendHttpSettingsCollection $apimPoolSetting, $dummyBackendSetting  -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert -Probes $apimprobe
```

## <a name="cname-the-api-management-proxy-hostname-to-the-public-dns-name-of-the-application-gateway-resource"></a><span data-ttu-id="fbb29-241">CNAME värdnamnet för API Management-proxy till det offentliga DNS-namnet på resursen Programgateway</span><span class="sxs-lookup"><span data-stu-id="fbb29-241">CNAME the API Management proxy hostname to the public DNS name of the Application Gateway resource</span></span>

<span data-ttu-id="fbb29-242">När du har skapat gatewayen, är nästa steg att konfigurera klientprogrammet för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="fbb29-242">Once the gateway is created, the next step is to configure the front end for communication.</span></span> <span data-ttu-id="fbb29-243">När du använder en offentlig IP-adress, kräver en dynamiskt tilldelad DNS-namn, vilket inte kanske är enkla att använda Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="fbb29-243">When using a public IP, Application Gateway requires a dynamically assigned DNS name, which may not be easy to use.</span></span> 

<span data-ttu-id="fbb29-244">Den Programgateway DNS-namn som ska användas för att skapa en CNAME-post som pekar APIM proxyvärdnamn (t.ex. `api.contoso.net` i exemplen ovan) till DNS-namnet.</span><span class="sxs-lookup"><span data-stu-id="fbb29-244">The Application Gateway's DNS name should be used to create a CNAME record which points the APIM proxy host name (e.g. `api.contoso.net` in the examples above) to this DNS name.</span></span> <span data-ttu-id="fbb29-245">Hämta information om Application Gateway och dess associerade IP DNS-namn med PublicIPAddress-element för att konfigurera klientdelens IP-CNAME-post.</span><span class="sxs-lookup"><span data-stu-id="fbb29-245">To configure the frontend IP CNAME record, retrieve the details of the Application Gateway and its associated IP/DNS name using the PublicIPAddress element.</span></span> <span data-ttu-id="fbb29-246">Användning av A-poster rekommenderas inte eftersom VIP ändras vid omstart för gateway.</span><span class="sxs-lookup"><span data-stu-id="fbb29-246">The use of A-records is not recommended since the VIP may change on restart of gateway.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -Name "publicIP01"
```

##<span data-ttu-id="fbb29-247"><a name="summary"></a> Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="fbb29-247"><a name="summary"> </a> Summary</span></span>
<span data-ttu-id="fbb29-248">Azure API-hantering som konfigurerats i ett virtuellt nätverk innehåller en enda gateway-gränssnitt för alla konfigurerade API: er, oavsett om de är värdbaserade lokalt eller i molnet.</span><span class="sxs-lookup"><span data-stu-id="fbb29-248">Azure API Management configured in a VNET provides a single gateway interface for all configured APIs, whether they are hosted on-prem or in the cloud.</span></span> <span data-ttu-id="fbb29-249">Integrera Programgateway med API Management ger flexibilitet för att selektivt aktivera specifika API: er ska vara tillgänglig på Internet, samt att tillhandahålla en brandvägg för webbaserade program som en klientdel till API Management-instans.</span><span class="sxs-lookup"><span data-stu-id="fbb29-249">Integrating Application Gateway with API Management provides the flexibility of selectively enabling particular APIs to be accessible on the Internet, as well as providing a Web Application Firewall as a frontend to your API Management instance.</span></span>

##<span data-ttu-id="fbb29-250"><a name="next-steps"></a> Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fbb29-250"><a name="next-steps"> </a> Next steps</span></span>
* <span data-ttu-id="fbb29-251">Lär dig mer om Azure Programgateway</span><span class="sxs-lookup"><span data-stu-id="fbb29-251">Learn more about Azure Application Gateway</span></span>
  * [<span data-ttu-id="fbb29-252">Översikt över Gateway</span><span class="sxs-lookup"><span data-stu-id="fbb29-252">Application Gateway Overview</span></span>](../application-gateway/application-gateway-introduction.md)
  * [<span data-ttu-id="fbb29-253">Programmet Gateway Brandvägg för webbaserade program</span><span class="sxs-lookup"><span data-stu-id="fbb29-253">Application Gateway Web Application Firewall</span></span>](../application-gateway/application-gateway-webapplicationfirewall-overview.md)
  * [<span data-ttu-id="fbb29-254">Programgateway med sökväg-baserade Routning</span><span class="sxs-lookup"><span data-stu-id="fbb29-254">Application Gateway using Path-based Routing</span></span>](../application-gateway/application-gateway-create-url-route-arm-ps.md)
* <span data-ttu-id="fbb29-255">Lär dig mer om API-hantering och Vnet</span><span class="sxs-lookup"><span data-stu-id="fbb29-255">Learn more about API Management and VNETs</span></span>
  * [<span data-ttu-id="fbb29-256">Använda API Management tillgänglig endast inom VNET</span><span class="sxs-lookup"><span data-stu-id="fbb29-256">Using API Management available only within the VNET</span></span>](api-management-using-with-internal-vnet.md)
  * [<span data-ttu-id="fbb29-257">Med hjälp av API-hantering i virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="fbb29-257">Using API Management in VNET</span></span>](api-management-using-with-vnet.md)
