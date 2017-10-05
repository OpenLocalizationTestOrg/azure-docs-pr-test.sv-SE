---
title: "Hur du använder Azure API Management med internt virtuellt nätverk | Microsoft Docs"
description: "Lär dig mer om att installera och konfigurera Azure API Management i internt virtuellt nätverk."
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
ms.openlocfilehash: 55248387c7e78d05c1cf1afd615b7b921e9669d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-api-management-service-with-internal-virtual-network"></a><span data-ttu-id="91b21-103">Med hjälp av Azure API Management-tjänsten med internt virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="91b21-103">Using Azure API Management service with Internal Virtual Network</span></span>
<span data-ttu-id="91b21-104">Med virtuella Azure-nätverk (Vnet), kan API-hantering hantera API: er som inte är tillgänglig på Internet.</span><span class="sxs-lookup"><span data-stu-id="91b21-104">With Azure Virtual Networks (VNETs), API Management can manage APIs not accessible on the Internet.</span></span> <span data-ttu-id="91b21-105">Ett antal VPN-tekniker som är tillgängliga för att ansluta och API-hantering kan distribueras i två huvudsakliga lägen i VNET:</span><span class="sxs-lookup"><span data-stu-id="91b21-105">A number of VPN technologies are available to make the connection and API Management can be deployed in two main modes inside the VNET:</span></span>
* <span data-ttu-id="91b21-106">Extern</span><span class="sxs-lookup"><span data-stu-id="91b21-106">External</span></span>
* <span data-ttu-id="91b21-107">internt</span><span class="sxs-lookup"><span data-stu-id="91b21-107">Internal</span></span>

## <span data-ttu-id="91b21-108"><a name="overview"> </a>Översikt</span><span class="sxs-lookup"><span data-stu-id="91b21-108"><a name="overview"> </a>Overview</span></span>
<span data-ttu-id="91b21-109">När API Management har distribuerats i ett internt virtuellt nätverk-läge måste åtkomst alla Tjänsteslutpunkter (gateway, developer-portalen, publisher portal, direkthantering och Git) visas bara i ett virtuellt nätverk som du styr till.</span><span class="sxs-lookup"><span data-stu-id="91b21-109">When API Management is deployed in an Internal Virtual network mode, all the service endpoints (gateway, developer portal, publisher portal, direct management and Git) are only visible inside a Virtual network that you control access to.</span></span> <span data-ttu-id="91b21-110">Inget av Tjänsteslutpunkter registreras på den offentliga DNS-servern.</span><span class="sxs-lookup"><span data-stu-id="91b21-110">None of the service endpoints are registered on the Public DNS Server.</span></span>

<span data-ttu-id="91b21-111">Använder API-hantering i internt läge kan uppnå du följande scenarier</span><span class="sxs-lookup"><span data-stu-id="91b21-111">Using API Management in Internal mode, you can achieve the following scenarios</span></span>
* <span data-ttu-id="91b21-112">På ett säkert sätt göra API: er som finns i ditt privata datacenter som är tillgängliga med 3 parter utanför den med hjälp av plats-till-plats eller ExpressRoute VPN-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="91b21-112">Securely make APIs hosted in your private datacenter accessible by 3rd parties outside of it using Site-to-Site or ExpressRoute VPN connections.</span></span>
* <span data-ttu-id="91b21-113">Aktivera hybrid cloud scenarier genom att exponera dina molnbaserade API: er och lokala API: er via en vanliga gateway.</span><span class="sxs-lookup"><span data-stu-id="91b21-113">Enable hybrid cloud scenarios by exposing your cloud-based APIs and on-prem APIs through a common gateway.</span></span>
* <span data-ttu-id="91b21-114">Hantera dina API: er som finns i flera geografiska platser med hjälp av en enda Gateway-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="91b21-114">Manage your APIs hosted in multiple geographic locations using a single Gateway endpoint.</span></span> 

## <span data-ttu-id="91b21-115"><a name="enable-vpn"></a>Skapar en API-hantering i internt virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="91b21-115"><a name="enable-vpn"> </a>Creating an API Management in Internal Virtual network</span></span>
<span data-ttu-id="91b21-116">API Management-tjänst i interna virtuella nätverk ligger bakom en intern belastning Balancer(ILB).</span><span class="sxs-lookup"><span data-stu-id="91b21-116">The API Management service in Internal Virtual network is hosted behind an Internal Load Balancer(ILB).</span></span> <span data-ttu-id="91b21-117">IP-adressen för ILB finns i den [RFC1918](http://www.faqs.org/rfcs/rfc1918.html) intervall.</span><span class="sxs-lookup"><span data-stu-id="91b21-117">The IP Address of the ILB is in the [RFC1918](http://www.faqs.org/rfcs/rfc1918.html) range.</span></span>  

### <a name="enable-vnet-connection-using-azure-portal"></a><span data-ttu-id="91b21-118">Aktivera anslutning till virtuellt nätverk med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="91b21-118">Enable VNET connection using Azure portal</span></span>
<span data-ttu-id="91b21-119">Skapa först API Management-tjänsten genom att följa stegen [skapa API Management-tjänsten][Create API Management service].</span><span class="sxs-lookup"><span data-stu-id="91b21-119">First create the API Management service by following the steps [Create API Management service][Create API Management service].</span></span> <span data-ttu-id="91b21-120">Sedan ställa in API-hantering som ska distribueras i ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="91b21-120">Then set-up API Management to be deployed inside a Virtual network.</span></span>

![Menyn för att ställa in APIM i internt virtuellt nätverk][api-management-using-internal-vnet-menu]

<span data-ttu-id="91b21-122">När distributionen lyckas, bör du se interna virtuella IP-adressen för din tjänst på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="91b21-122">After the deployment succeeds, you should see the Internal Virtual IP Address of your service on the dashboard.</span></span>

![API Management-instrumentpanel med interna virtuella nätverk konfigureras][api-management-internal-vnet-dashboard]

### <a name="enable-vnet-connection-using-powershell-cmdlets"></a><span data-ttu-id="91b21-124">Aktivera anslutning till VNET med Powershell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="91b21-124">Enable VNET connection using Powershell cmdlets</span></span>
<span data-ttu-id="91b21-125">Du kan också aktivera VNET-anslutning med hjälp av PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="91b21-125">You can also enable VNET connectivity using the PowerShell cmdlets.</span></span>

* <span data-ttu-id="91b21-126">**Skapa en API Management-tjänst i ett VNET**: Använd cmdlet [ny AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) att skapa en Azure API Management-tjänst i ett VNET och konfigurera den att använda typen interna virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="91b21-126">**Create an API Management service inside a VNET**: Use the cmdlet [New-AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) to create an Azure API Management service inside a VNET and configure it to use the Internal VNET type.</span></span>

* <span data-ttu-id="91b21-127">**Distribuera en befintlig API Management-tjänst i ett VNET**: Använd cmdlet [uppdatering AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) att flytta en befintlig Azure API Management-tjänst i ett virtuellt nätverk och konfigurera den att använda Internt VNET-typen.</span><span class="sxs-lookup"><span data-stu-id="91b21-127">**Deploy an existing API Management service inside a VNET**: Use the cmdlet [Update-AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) to move an existing Azure API Management service inside a Virtual network and configure it to use Internal VNET type.</span></span>

## <span data-ttu-id="91b21-128"><a name="apim-dns-configuration"></a>DNS-konfiguration</span><span class="sxs-lookup"><span data-stu-id="91b21-128"><a name="apim-dns-configuration"></a>DNS configuration</span></span>
<span data-ttu-id="91b21-129">När du använder API-hantering i läget för externa virtuella nätverk, hanteras DNS av Azure.</span><span class="sxs-lookup"><span data-stu-id="91b21-129">When using API Management in External Virtual network mode, DNS is managed by Azure.</span></span> <span data-ttu-id="91b21-130">Du måste hantera egna DNS för den interna virtuella nätverk läge.</span><span class="sxs-lookup"><span data-stu-id="91b21-130">For Internal Virtual network mode, you have to manage your own DNS.</span></span>

> [!NOTE]
> <span data-ttu-id="91b21-131">API Management-tjänsten lyssnar inte till lyssnar på IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="91b21-131">API Management service does not listen to requests coming on IP addresses.</span></span> <span data-ttu-id="91b21-132">Den bara svarar på förfrågningar om att värdnamnet som konfigurerats på dess slutpunkter (som innehåller gateway, developer-portalen, publisher Portal, direkthantering slutpunkt och git).</span><span class="sxs-lookup"><span data-stu-id="91b21-132">It only responds to requests to the Hostname configured on its service endpoints (which includes gateway, developer portal, publisher Portal, direct management endpoint, and git).</span></span>

### <a name="access-on-default-host-names"></a><span data-ttu-id="91b21-133">Åtkomst på standard värdnamn:</span><span class="sxs-lookup"><span data-stu-id="91b21-133">Access on default host names:</span></span>
<span data-ttu-id="91b21-134">När du skapar en API Management-tjänst i offentliga Azure-molnet, till exempel med namnet ”contoso”, konfigureras följande Tjänsteslutpunkter som standard.</span><span class="sxs-lookup"><span data-stu-id="91b21-134">When you create an API Management service in public Azure cloud, named "contoso" for example, the following service endpoints are configured by default.</span></span>

>   <span data-ttu-id="91b21-135">Gateway / Proxy – contoso.azure api.net</span><span class="sxs-lookup"><span data-stu-id="91b21-135">Gateway / Proxy - contoso.azure-api.net</span></span>

> <span data-ttu-id="91b21-136">Publisher Portal och Developer Portal - contoso.portal.azure api.net</span><span class="sxs-lookup"><span data-stu-id="91b21-136">Publisher Portal and Developer Portal - contoso.portal.azure-api.net</span></span>

> <span data-ttu-id="91b21-137">Direkthantering slutpunkt - contoso.management.azure api.net</span><span class="sxs-lookup"><span data-stu-id="91b21-137">Direct Management Endpoint - contoso.management.azure-api.net</span></span>

>   <span data-ttu-id="91b21-138">GIT - contoso.scm.azure api.net</span><span class="sxs-lookup"><span data-stu-id="91b21-138">GIT - contoso.scm.azure-api.net</span></span>

<span data-ttu-id="91b21-139">Om du vill få åtkomst till dessa slutpunkter för API Management-tjänsten måste skapa du en virtuell dator i ett undernät som är anslutet till det virtuella nätverket som API Management har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="91b21-139">To access these API Management service endpoints, you can create a Virtual Machine in a subnet connected to the Virtual network in which API Management is deployed.</span></span> <span data-ttu-id="91b21-140">Under förutsättning att den interna virtuella IP-adressen för din tjänst är 10.0.0.5, kan du göra värdarna filen mappning (% SystemDrive%\drivers\etc\hosts) enligt följande:</span><span class="sxs-lookup"><span data-stu-id="91b21-140">Assuming the Internal Virtual IP Address for your service is 10.0.0.5, you can do the hosts file mapping (%SystemDrive%\drivers\etc\hosts) as follows:</span></span>

> <span data-ttu-id="91b21-141">10.0.0.5 contoso.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="91b21-141">10.0.0.5    contoso.azure-api.net</span></span>

> <span data-ttu-id="91b21-142">10.0.0.5 contoso.portal.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="91b21-142">10.0.0.5    contoso.portal.azure-api.net</span></span>

> <span data-ttu-id="91b21-143">10.0.0.5 contoso.management.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="91b21-143">10.0.0.5    contoso.management.azure-api.net</span></span>

> <span data-ttu-id="91b21-144">10.0.0.5 contoso.scm.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="91b21-144">10.0.0.5    contoso.scm.azure-api.net</span></span>

<span data-ttu-id="91b21-145">Du kan sedan komma åt alla Tjänsteslutpunkter från den virtuella datorn som du skapade.</span><span class="sxs-lookup"><span data-stu-id="91b21-145">You can then access all the service endpoints from the Virtual Machine you created.</span></span> <span data-ttu-id="91b21-146">Om du använder en anpassad DNS-server i ett virtuellt nätverk kan du också skapa en DNS-poster och åtkomst till dessa slutpunkter från var som helst i ditt virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="91b21-146">If using a Custom DNS server in a Virtual network, you can also create A DNS records and access these endpoints from anywhere in your Virtual network.</span></span> 

### <a name="access-on-custom-domain-names"></a><span data-ttu-id="91b21-147">Åtkomst på anpassade domännamn:</span><span class="sxs-lookup"><span data-stu-id="91b21-147">Access on custom domain names:</span></span>
<span data-ttu-id="91b21-148">Om du inte vill att få åtkomst till API Management-tjänsten med standard-värdnamn, kan du konfigurera anpassade domännamn för alla dina slutpunkter som nedan</span><span class="sxs-lookup"><span data-stu-id="91b21-148">If you don’t want to access the API Management service with the default host names, you can setup custom domain names for all your service endpoints like below</span></span>

![Ställa in anpassad domän för API Management][api-management-custom-domain-name]

<span data-ttu-id="91b21-150">Du kan skapa en poster i DNS-servern för att få åtkomst till dessa slutpunkter som endast är tillgänglig från din virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="91b21-150">Then you can create A records in your DNS Server to access these endpoints which are only accessible from within your Virtual network.</span></span>

## <span data-ttu-id="91b21-151"><a name="related-content"></a>Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="91b21-151"><a name="related-content"> </a>Related content</span></span>
* <span data-ttu-id="91b21-152">[Vanliga problem med nätverket konfiguration när du konfigurerar APIM i virtuella nätverk][Common Network Configuration Issues]</span><span class="sxs-lookup"><span data-stu-id="91b21-152">[Common Network configuration issues while setting up APIM in VNET][Common Network Configuration Issues]</span></span>
* [<span data-ttu-id="91b21-153">Virtuella nätverk vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="91b21-153">Virtual Network faqs</span></span>](../virtual-network/virtual-networks-faq.md)
* [<span data-ttu-id="91b21-154">Skapa en post i DNS</span><span class="sxs-lookup"><span data-stu-id="91b21-154">Creating A record in DNS</span></span>](https://msdn.microsoft.com/en-us/library/bb727018.aspx)

[api-management-using-internal-vnet-menu]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-menu.png
[api-management-internal-vnet-dashboard]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-dashboard.png
[api-management-custom-domain-name]: ./media/api-management-using-with-internal-vnet/api-management-custom-domain-name.png

[Create API Management service]: api-management-get-started.md#create-service-instance
[Common Network Configuration Issues]: api-management-using-with-vnet.md#network-configuration-issues
