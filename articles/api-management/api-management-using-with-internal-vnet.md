---
title: "aaaHow toouse Azure API Management med internt virtuellt nätverk | Microsoft Docs"
description: "Lär dig hur toosetup och konfigurera Azure API Management i internt virtuellt nätverk."
services: api-management
documentationcenter: 
author: solankisamir
manager: kjoshi
editor: 
ms.assetid: dac28ccf-2550-45a5-89cf-192d87369bc3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 8d25de14e0c0bebe7ba7b47ca432ea4e45dde312
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-api-management-service-with-internal-virtual-network"></a><span data-ttu-id="2d352-103">Med hjälp av Azure API Management-tjänsten med internt virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="2d352-103">Using Azure API Management service with Internal Virtual Network</span></span>
<span data-ttu-id="2d352-104">Med virtuella Azure-nätverk (Vnet), kan API-hantering hantera API: er inte kan nås på hello Internet.</span><span class="sxs-lookup"><span data-stu-id="2d352-104">With Azure Virtual Networks (VNETs), API Management can manage APIs not accessible on hello Internet.</span></span> <span data-ttu-id="2d352-105">Ett antal VPN-tekniker är tillgängliga toomake hello anslutning och API-hantering kan distribueras i två huvudsakliga lägen i hello VNET:</span><span class="sxs-lookup"><span data-stu-id="2d352-105">A number of VPN technologies are available toomake hello connection and API Management can be deployed in two main modes inside hello VNET:</span></span>
* <span data-ttu-id="2d352-106">Extern</span><span class="sxs-lookup"><span data-stu-id="2d352-106">External</span></span>
* <span data-ttu-id="2d352-107">internt</span><span class="sxs-lookup"><span data-stu-id="2d352-107">Internal</span></span>

## <span data-ttu-id="2d352-108"><a name="overview"> </a>Översikt</span><span class="sxs-lookup"><span data-stu-id="2d352-108"><a name="overview"> </a>Overview</span></span>
<span data-ttu-id="2d352-109">När API Management har distribuerats i ett internt virtuellt nätverk-läge måste är alla hello slutpunkter (gateway, developer-portalen, publisher portal, direkthantering och Git) bara synliga i ett virtuellt nätverk som du styr åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="2d352-109">When API Management is deployed in an Internal Virtual network mode, all hello service endpoints (gateway, developer portal, publisher portal, direct management and Git) are only visible inside a Virtual network that you control access to.</span></span> <span data-ttu-id="2d352-110">Inget av slutpunkter hello är registrerad på hello offentliga DNS-servern.</span><span class="sxs-lookup"><span data-stu-id="2d352-110">None of hello service endpoints are registered on hello Public DNS Server.</span></span>

<span data-ttu-id="2d352-111">Använder API-hantering i internt läge kan uppnå du hello följande scenarier</span><span class="sxs-lookup"><span data-stu-id="2d352-111">Using API Management in Internal mode, you can achieve hello following scenarios</span></span>
* <span data-ttu-id="2d352-112">På ett säkert sätt göra API: er som finns i ditt privata datacenter som är tillgängliga med 3 parter utanför den med hjälp av plats-till-plats eller ExpressRoute VPN-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="2d352-112">Securely make APIs hosted in your private datacenter accessible by 3rd parties outside of it using Site-to-Site or ExpressRoute VPN connections.</span></span>
* <span data-ttu-id="2d352-113">Aktivera hybrid cloud scenarier genom att exponera dina molnbaserade API: er och lokala API: er via en vanliga gateway.</span><span class="sxs-lookup"><span data-stu-id="2d352-113">Enable hybrid cloud scenarios by exposing your cloud-based APIs and on-prem APIs through a common gateway.</span></span>
* <span data-ttu-id="2d352-114">Hantera dina API: er som finns i flera geografiska platser med hjälp av en enda Gateway-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="2d352-114">Manage your APIs hosted in multiple geographic locations using a single Gateway endpoint.</span></span> 

## <span data-ttu-id="2d352-115"><a name="enable-vpn"></a>Skapar en API-hantering i internt virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="2d352-115"><a name="enable-vpn"> </a>Creating an API Management in Internal Virtual network</span></span>
<span data-ttu-id="2d352-116">hello API Management-tjänsten i interna virtuella nätverk finns bakom en intern Balancer(ILB) belastningen.</span><span class="sxs-lookup"><span data-stu-id="2d352-116">hello API Management service in Internal Virtual network is hosted behind an Internal Load Balancer(ILB).</span></span> <span data-ttu-id="2d352-117">hello hello ILB IP-adress är i hello [RFC1918](http://www.faqs.org/rfcs/rfc1918.html) intervall.</span><span class="sxs-lookup"><span data-stu-id="2d352-117">hello IP Address of hello ILB is in hello [RFC1918](http://www.faqs.org/rfcs/rfc1918.html) range.</span></span>  

### <a name="enable-vnet-connection-using-azure-portal"></a><span data-ttu-id="2d352-118">Aktivera anslutning till virtuellt nätverk med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="2d352-118">Enable VNET connection using Azure portal</span></span>
<span data-ttu-id="2d352-119">Först skapa hello API Management-tjänsten genom att följa stegen i hello [skapa API Management-tjänsten][Create API Management service].</span><span class="sxs-lookup"><span data-stu-id="2d352-119">First create hello API Management service by following hello steps [Create API Management service][Create API Management service].</span></span> <span data-ttu-id="2d352-120">Sedan ställa in API-hantering toobe distribueras i ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="2d352-120">Then set-up API Management toobe deployed inside a Virtual network.</span></span>

![Menyn för att ställa in APIM i internt virtuellt nätverk][api-management-using-internal-vnet-menu]

<span data-ttu-id="2d352-122">När hello distribution lyckas, bör du se hello interna virtuella IP-adressen för din tjänst på hello instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="2d352-122">After hello deployment succeeds, you should see hello Internal Virtual IP Address of your service on hello dashboard.</span></span>

![API Management-instrumentpanel med interna virtuella nätverk konfigureras][api-management-internal-vnet-dashboard]

### <a name="enable-vnet-connection-using-powershell-cmdlets"></a><span data-ttu-id="2d352-124">Aktivera anslutning till VNET med Powershell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="2d352-124">Enable VNET connection using Powershell cmdlets</span></span>
<span data-ttu-id="2d352-125">Du kan också aktivera VNET-anslutning med hello PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="2d352-125">You can also enable VNET connectivity using hello PowerShell cmdlets.</span></span>

* <span data-ttu-id="2d352-126">**Skapa en API Management-tjänst i ett VNET**: Använd hello cmdlet [ny AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) toocreate ett Azure API Management-tjänst i ett VNET och konfigurera den toouse hello interna virtuella nätverk typen.</span><span class="sxs-lookup"><span data-stu-id="2d352-126">**Create an API Management service inside a VNET**: Use hello cmdlet [New-AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) toocreate an Azure API Management service inside a VNET and configure it toouse hello Internal VNET type.</span></span>

* <span data-ttu-id="2d352-127">**Distribuera en befintlig API Management-tjänst i ett VNET**: Använd cmdlet hello [uppdatering AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) toomove en befintlig Azure API Management-tjänsten i ett virtuellt nätverk och konfigurera den toouse Internt VNET-typen.</span><span class="sxs-lookup"><span data-stu-id="2d352-127">**Deploy an existing API Management service inside a VNET**: Use hello cmdlet [Update-AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) toomove an existing Azure API Management service inside a Virtual network and configure it toouse Internal VNET type.</span></span>

## <span data-ttu-id="2d352-128"><a name="apim-dns-configuration"></a>DNS-konfiguration</span><span class="sxs-lookup"><span data-stu-id="2d352-128"><a name="apim-dns-configuration"></a>DNS configuration</span></span>
<span data-ttu-id="2d352-129">När du använder API-hantering i läget för externa virtuella nätverk, hanteras DNS av Azure.</span><span class="sxs-lookup"><span data-stu-id="2d352-129">When using API Management in External Virtual network mode, DNS is managed by Azure.</span></span> <span data-ttu-id="2d352-130">Internt virtuellt nätverk läge har du toomanage egna DNS.</span><span class="sxs-lookup"><span data-stu-id="2d352-130">For Internal Virtual network mode, you have toomanage your own DNS.</span></span>

> [!NOTE]
> <span data-ttu-id="2d352-131">API Management-tjänsten lyssnar inte toorequests kommer på IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="2d352-131">API Management service does not listen toorequests coming on IP addresses.</span></span> <span data-ttu-id="2d352-132">Endast svarar den toorequests toohello värdnamn som konfigurerats på dess slutpunkter (som innehåller gateway, developer-portalen, publisher Portal, direkthantering slutpunkt och git).</span><span class="sxs-lookup"><span data-stu-id="2d352-132">It only responds toorequests toohello Hostname configured on its service endpoints (which includes gateway, developer portal, publisher Portal, direct management endpoint, and git).</span></span>

### <a name="access-on-default-host-names"></a><span data-ttu-id="2d352-133">Åtkomst på standard värdnamn:</span><span class="sxs-lookup"><span data-stu-id="2d352-133">Access on default host names:</span></span>
<span data-ttu-id="2d352-134">När du skapar en API Management-tjänst i offentliga Azure-molnet, till exempel med namnet ”contoso”, konfigureras hello följande slutpunkter som standard.</span><span class="sxs-lookup"><span data-stu-id="2d352-134">When you create an API Management service in public Azure cloud, named "contoso" for example, hello following service endpoints are configured by default.</span></span>

>   <span data-ttu-id="2d352-135">Gateway / Proxy – contoso.azure api.net</span><span class="sxs-lookup"><span data-stu-id="2d352-135">Gateway / Proxy - contoso.azure-api.net</span></span>

> <span data-ttu-id="2d352-136">Publisher Portal och Developer Portal - contoso.portal.azure api.net</span><span class="sxs-lookup"><span data-stu-id="2d352-136">Publisher Portal and Developer Portal - contoso.portal.azure-api.net</span></span>

> <span data-ttu-id="2d352-137">Direkthantering slutpunkt - contoso.management.azure api.net</span><span class="sxs-lookup"><span data-stu-id="2d352-137">Direct Management Endpoint - contoso.management.azure-api.net</span></span>

>   <span data-ttu-id="2d352-138">GIT - contoso.scm.azure api.net</span><span class="sxs-lookup"><span data-stu-id="2d352-138">GIT - contoso.scm.azure-api.net</span></span>

<span data-ttu-id="2d352-139">tooaccess dessa slutpunkter för API Management-tjänsten, kan du skapa en virtuell dator i en ansluten toohello virtuella undernätverk som API Management har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="2d352-139">tooaccess these API Management service endpoints, you can create a Virtual Machine in a subnet connected toohello Virtual network in which API Management is deployed.</span></span> <span data-ttu-id="2d352-140">Under förutsättning att hello interna virtuella IP-adressen för din tjänst är 10.0.0.5, kan du göra hello mappning hosts-filen (% SystemDrive%\drivers\etc\hosts) enligt följande:</span><span class="sxs-lookup"><span data-stu-id="2d352-140">Assuming hello Internal Virtual IP Address for your service is 10.0.0.5, you can do hello hosts file mapping (%SystemDrive%\drivers\etc\hosts) as follows:</span></span>

> <span data-ttu-id="2d352-141">10.0.0.5 contoso.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="2d352-141">10.0.0.5    contoso.azure-api.net</span></span>

> <span data-ttu-id="2d352-142">10.0.0.5 contoso.portal.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="2d352-142">10.0.0.5    contoso.portal.azure-api.net</span></span>

> <span data-ttu-id="2d352-143">10.0.0.5 contoso.management.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="2d352-143">10.0.0.5    contoso.management.azure-api.net</span></span>

> <span data-ttu-id="2d352-144">10.0.0.5 contoso.scm.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="2d352-144">10.0.0.5    contoso.scm.azure-api.net</span></span>

<span data-ttu-id="2d352-145">Du kan sedan komma åt alla hello slutpunkter från hello virtuell dator som du skapade.</span><span class="sxs-lookup"><span data-stu-id="2d352-145">You can then access all hello service endpoints from hello Virtual Machine you created.</span></span> <span data-ttu-id="2d352-146">Om du använder en anpassad DNS-server i ett virtuellt nätverk kan du också skapa en DNS-poster och åtkomst till dessa slutpunkter från var som helst i ditt virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="2d352-146">If using a Custom DNS server in a Virtual network, you can also create A DNS records and access these endpoints from anywhere in your Virtual network.</span></span> 

### <a name="access-on-custom-domain-names"></a><span data-ttu-id="2d352-147">Åtkomst på anpassade domännamn:</span><span class="sxs-lookup"><span data-stu-id="2d352-147">Access on custom domain names:</span></span>
<span data-ttu-id="2d352-148">Om du inte vill tooaccess hello API Management-tjänsten med hello standard värdnamn, kan du konfigurera anpassade domännamn för alla dina slutpunkter som nedan</span><span class="sxs-lookup"><span data-stu-id="2d352-148">If you don’t want tooaccess hello API Management service with hello default host names, you can setup custom domain names for all your service endpoints like below</span></span>

![Ställa in anpassad domän för API Management][api-management-custom-domain-name]

<span data-ttu-id="2d352-150">Du kan skapa A-poster i DNS-Server-tooaccess dessa slutpunkter som endast är tillgänglig från din virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="2d352-150">Then you can create A records in your DNS Server tooaccess these endpoints which are only accessible from within your Virtual network.</span></span>

## <span data-ttu-id="2d352-151"><a name="related-content"></a>Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="2d352-151"><a name="related-content"> </a>Related content</span></span>
* <span data-ttu-id="2d352-152">[Vanliga problem med nätverket konfiguration när du konfigurerar APIM i virtuella nätverk][Common Network Configuration Issues]</span><span class="sxs-lookup"><span data-stu-id="2d352-152">[Common Network configuration issues while setting up APIM in VNET][Common Network Configuration Issues]</span></span>
* [<span data-ttu-id="2d352-153">Virtuella nätverk vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="2d352-153">Virtual Network faqs</span></span>](../virtual-network/virtual-networks-faq.md)
* [<span data-ttu-id="2d352-154">Skapa en post i DNS</span><span class="sxs-lookup"><span data-stu-id="2d352-154">Creating A record in DNS</span></span>](https://msdn.microsoft.com/en-us/library/bb727018.aspx)

[api-management-using-internal-vnet-menu]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-menu.png
[api-management-internal-vnet-dashboard]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-dashboard.png
[api-management-custom-domain-name]: ./media/api-management-using-with-internal-vnet/api-management-custom-domain-name.png

[Create API Management service]: api-management-get-started.md#create-service-instance
[Common Network Configuration Issues]: api-management-using-with-vnet.md#network-configuration-issues
