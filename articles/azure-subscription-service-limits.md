---
title: "Azure-prenumerationsbegränsningar och kvoter | Microsoft Docs"
description: "Visar en lista över vanliga Azure-prenumeration och tjänsten gränser, kvoter och begränsningar. Detta omfattar information om hur du ökar gränser tillsammans med högsta värden."
services: 
documentationcenter: 
author: rothja
manager: jeffreyg
editor: 
tags: billing
ms.assetid: 60d848f9-ff26-496e-a5ec-ccf92ad7d125
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: byvinyal
ms.openlocfilehash: a76acd67e9ba7822f2837b3c08e2ede389047f11
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a><span data-ttu-id="79992-104">Azure-prenumeration och tjänstbegränsningar, kvoter och krav</span><span class="sxs-lookup"><span data-stu-id="79992-104">Azure subscription and service limits, quotas, and constraints</span></span>
<span data-ttu-id="79992-105">Det här dokumentet innehåller några av de vanligaste Microsoft Azure-gränser, som ibland kallas kvoter.</span><span class="sxs-lookup"><span data-stu-id="79992-105">This document lists some of the most common Microsoft Azure limits, which are also sometimes called quotas.</span></span> <span data-ttu-id="79992-106">Det här dokumentet omfattar inte för närvarande alla Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="79992-106">This document doesn't currently cover all Azure services.</span></span> <span data-ttu-id="79992-107">Över tiden, kommer i listan att expanderas och uppdateras så att den täcker flera av plattformen.</span><span class="sxs-lookup"><span data-stu-id="79992-107">Over time, the list will be expanded and updated to cover more of the platform.</span></span>

<span data-ttu-id="79992-108">Besök [översikt över priser för Azure](https://azure.microsoft.com/pricing/) för mer information om priser för Azure.</span><span class="sxs-lookup"><span data-stu-id="79992-108">Please visit [Azure Pricing Overview](https://azure.microsoft.com/pricing/) to learn more about Azure pricing.</span></span> <span data-ttu-id="79992-109">Det, kan du uppskatta dina kostnader med hjälp av den [Priskalkylatorn](https://azure.microsoft.com/pricing/calculator/) eller genom att besöka sidan prisnivå information för en tjänst (till exempel [virtuella Windows-datorer](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)).</span><span class="sxs-lookup"><span data-stu-id="79992-109">There, you can estimate your costs using the [Pricing Calculator](https://azure.microsoft.com/pricing/calculator/) or by visiting the pricing details page for a service (for example, [Windows VMs](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)).</span></span> <span data-ttu-id="79992-110">Tips för att hantera dina kostnader finns [förhindrar oväntade kostnader med Azure fakturerings- och kostnaden management](billing/billing-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="79992-110">For tips to help manage your costs, see [Prevent unexpected costs with Azure billing and cost management](billing/billing-getting-started.md).</span></span>

> [!NOTE]
> <span data-ttu-id="79992-111">Om du vill höja gränsen eller kvoten ovan den **standard gränsen**, [öppna en supportbegäran för online customer utan kostnad](azure-supportability/resource-manager-core-quotas-request.md).</span><span class="sxs-lookup"><span data-stu-id="79992-111">If you want to raise the limit or quota above the **Default Limit**, [open an online customer support request at no charge](azure-supportability/resource-manager-core-quotas-request.md).</span></span> <span data-ttu-id="79992-112">Ramen kan inte ökas ovanför den **maxgränsen** värdet som visas i följande tabeller.</span><span class="sxs-lookup"><span data-stu-id="79992-112">The limits can't be raised above the **Maximum Limit** value shown in the following tables.</span></span> <span data-ttu-id="79992-113">Om det finns inga **maxgränsen** kolumn, och sedan resursen saknar justerbara gränser.</span><span class="sxs-lookup"><span data-stu-id="79992-113">If there is no **Maximum Limit** column, then the resource doesn't have adjustable limits.</span></span> 
> 
> <span data-ttu-id="79992-114">Kostnadsfri utvärderingsversion prenumerationer är inte berättigad till gränsen eller kvoten ökar.</span><span class="sxs-lookup"><span data-stu-id="79992-114">Free Trial subscriptions are not eligible for limit or quota increases.</span></span> <span data-ttu-id="79992-115">Om du har en kostnadsfri utvärderingsversion, du kan uppgradera till en [betala per användning](https://azure.microsoft.com/offers/ms-azr-0003p/) prenumeration.</span><span class="sxs-lookup"><span data-stu-id="79992-115">If you have a Free Trial, you can upgrade to a [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) subscription.</span></span> <span data-ttu-id="79992-116">Mer information finns i [uppgradera kostnadsfri utvärderingsversion av Azure till betala per användning](billing/billing-upgrade-azure-subscription.md).</span><span class="sxs-lookup"><span data-stu-id="79992-116">For more information, see [Upgrade Azure Free Trial to Pay-As-You-Go](billing/billing-upgrade-azure-subscription.md).</span></span>
> 

## <a name="limits-and-the-azure-resource-manager"></a><span data-ttu-id="79992-117">Gränser och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="79992-117">Limits and the Azure Resource Manager</span></span>
<span data-ttu-id="79992-118">Nu är det möjligt att kombinera flera Azure-resurser i en enda Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="79992-118">It is now possible to combine multiple Azure resources in to a single Azure Resource Group.</span></span> <span data-ttu-id="79992-119">När du använder resursgrupper hanteras gränser som en gång var globala på regional nivå med Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="79992-119">When using Resource Groups, limits that once were global become managed at a regional level with the Azure Resource Manager.</span></span> <span data-ttu-id="79992-120">Läs mer om Azure-resursgrupper [översikt över Azure Resource Manager](azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="79992-120">For more information about Azure Resource Groups, see [Azure Resource Manager overview](azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="79992-121">I gränserna som nedan, har en ny tabell lagts till återspeglar eventuella skillnader i begränsningar när du använder Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="79992-121">In the limits below, a new table has been added to reflect any differences in limits when using the Azure Resource Manager.</span></span> <span data-ttu-id="79992-122">Det finns till exempel en **prenumerationsbegränsningar** tabell och en **prenumerationsbegränsningar - Azure Resource Manager** tabell.</span><span class="sxs-lookup"><span data-stu-id="79992-122">For example, there is a **Subscription Limits** table and a **Subscription Limits - Azure Resource Manager** table.</span></span> <span data-ttu-id="79992-123">När det gäller en gräns för båda scenarierna, visas den bara i den första tabellen.</span><span class="sxs-lookup"><span data-stu-id="79992-123">When a limit applies to both scenarios, it is only shown in the first table.</span></span> <span data-ttu-id="79992-124">Om inget annat anges är gränserna globala för alla regioner.</span><span class="sxs-lookup"><span data-stu-id="79992-124">Unless otherwise indicated, limits are global across all regions.</span></span>

> [!NOTE]
> <span data-ttu-id="79992-125">Det är viktigt att betona att kvoter för Azure-resursgrupper är per region få åtkomst till din prenumeration och är inte per prenumeration som service management kvoter.</span><span class="sxs-lookup"><span data-stu-id="79992-125">It is important to emphasize that quotas for resources in Azure Resource Groups are per-region accessible by your subscription, and are not per-subscription, as the service management quotas are.</span></span> <span data-ttu-id="79992-126">Nu ska vi använda core kvoter som exempel.</span><span class="sxs-lookup"><span data-stu-id="79992-126">Let's use core quotas as an example.</span></span> <span data-ttu-id="79992-127">Om du måste begära en ökad kvot med stöd för kärnor, måste du bestämma hur många kärnor som du vill använda i vilka regioner och sedan göra en specifik begäran för Azure-resursgrupp core kvoter för belopp och regioner som du vill.</span><span class="sxs-lookup"><span data-stu-id="79992-127">If you need to request a quota increase with support for cores, you need to decide how many cores you want to use in which regions, and then make a specific request for Azure Resource Group core quotas for the amounts and regions that you want.</span></span> <span data-ttu-id="79992-128">Därför, om du behöver använda 30 kärnor i västra Europa för att köra programmet. Du bör begär 30 kärnor i västra Europa.</span><span class="sxs-lookup"><span data-stu-id="79992-128">Therefore, if you need to use 30 cores in West Europe to run your application there; you should specifically request 30 cores in West Europe.</span></span> <span data-ttu-id="79992-129">Men du har inte en kärnkvot som ökar med en annan region – endast Västeuropa har 30 kärnor kvoten.</span><span class="sxs-lookup"><span data-stu-id="79992-129">But you will not have a core quota increase in any other region -- only West Europe will have the 30-core quota.</span></span>
> <!-- -->
> <span data-ttu-id="79992-130">Du kan därför vara bra att tänka på bestämmer vad din Azure-resursgrupp kvoter måste vara för din arbetsbelastning i en en region och begära beloppet i varje region som du planerar distributionen.</span><span class="sxs-lookup"><span data-stu-id="79992-130">As a result, you may find it useful to consider deciding what your Azure Resource Group quotas need to be for your workload in any one region, and request that amount in each region into which you are considering deployment.</span></span> <span data-ttu-id="79992-131">Se [felsöka distributionsproblem](resource-manager-common-deployment-errors.md) för mer hjälp för att identifiera din aktuella kvoter för vissa regioner.</span><span class="sxs-lookup"><span data-stu-id="79992-131">See [troubleshooting deployment issues](resource-manager-common-deployment-errors.md) for more help discovering your current quotas for specific regions.</span></span>
> 
> 

## <a name="service-specific-limits"></a><span data-ttu-id="79992-132">Tjänstspecifika gränser</span><span class="sxs-lookup"><span data-stu-id="79992-132">Service-specific limits</span></span>
* [<span data-ttu-id="79992-133">Active Directory</span><span class="sxs-lookup"><span data-stu-id="79992-133">Active Directory</span></span>](#active-directory-limits)
* [<span data-ttu-id="79992-134">API-hantering</span><span class="sxs-lookup"><span data-stu-id="79992-134">API Management</span></span>](#api-management-limits)
* [<span data-ttu-id="79992-135">App Service</span><span class="sxs-lookup"><span data-stu-id="79992-135">App Service</span></span>](#app-service-limits)
* [<span data-ttu-id="79992-136">Application Gateway</span><span class="sxs-lookup"><span data-stu-id="79992-136">Application Gateway</span></span>](#application-gateway-limits)
* [<span data-ttu-id="79992-137">Application Insights</span><span class="sxs-lookup"><span data-stu-id="79992-137">Application Insights</span></span>](#application-insights-limits)
* [<span data-ttu-id="79992-138">Automation</span><span class="sxs-lookup"><span data-stu-id="79992-138">Automation</span></span>](#automation-limits)
* [<span data-ttu-id="79992-139">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="79992-139">Azure Cosmos DB</span></span>](#azure-cosmos-db-limits)
* [<span data-ttu-id="79992-140">Azure händelse rutnätet</span><span class="sxs-lookup"><span data-stu-id="79992-140">Azure Event Grid</span></span>](#azure-event-grid-limits)
* [<span data-ttu-id="79992-141">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="79992-141">Azure Redis Cache</span></span>](#azure-redis-cache-limits)
* [<span data-ttu-id="79992-142">Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="79992-142">Azure RemoteApp</span></span>](#azure-remoteapp-limits)
* [<span data-ttu-id="79992-143">Säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="79992-143">Backup</span></span>](#backup-limits)
* [<span data-ttu-id="79992-144">Batch</span><span class="sxs-lookup"><span data-stu-id="79992-144">Batch</span></span>](#batch-limits)
* [<span data-ttu-id="79992-145">BizTalk-tjänst</span><span class="sxs-lookup"><span data-stu-id="79992-145">BizTalk Services</span></span>](#biztalk-services-limits)
* [<span data-ttu-id="79992-146">CDN</span><span class="sxs-lookup"><span data-stu-id="79992-146">CDN</span></span>](#cdn-limits)
* [<span data-ttu-id="79992-147">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="79992-147">Cloud Services</span></span>](#cloud-services-limits)
* [<span data-ttu-id="79992-148">Container Instances</span><span class="sxs-lookup"><span data-stu-id="79992-148">Container Instances</span></span>](#container-instances-limits)
* [<span data-ttu-id="79992-149">Data Factory</span><span class="sxs-lookup"><span data-stu-id="79992-149">Data Factory</span></span>](#data-factory-limits)
* [<span data-ttu-id="79992-150">Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="79992-150">Data Lake Analytics</span></span>](#data-lake-analytics-limits)
* [<span data-ttu-id="79992-151">Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="79992-151">Data Lake Store</span></span>](#data-lake-store-limits)
* [<span data-ttu-id="79992-152">DNS</span><span class="sxs-lookup"><span data-stu-id="79992-152">DNS</span></span>](#dns-limits)
* [<span data-ttu-id="79992-153">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="79992-153">Event Hubs</span></span>](#event-hubs-limits)
* [<span data-ttu-id="79992-154">IoT Hub</span><span class="sxs-lookup"><span data-stu-id="79992-154">IoT Hub</span></span>](#iot-hub-limits)
* [<span data-ttu-id="79992-155">Key Vault</span><span class="sxs-lookup"><span data-stu-id="79992-155">Key Vault</span></span>](#key-vault-limits)
* [<span data-ttu-id="79992-156">Logga Analytics / Operational Insights</span><span class="sxs-lookup"><span data-stu-id="79992-156">Log Analytics / Operational Insights</span></span>](#log-analytics-limits)
* [<span data-ttu-id="79992-157">Media Services</span><span class="sxs-lookup"><span data-stu-id="79992-157">Media Services</span></span>](#media-services-limits)
* [<span data-ttu-id="79992-158">Mobilt engagemang</span><span class="sxs-lookup"><span data-stu-id="79992-158">Mobile Engagement</span></span>](#mobile-engagement-limits)
* [<span data-ttu-id="79992-159">Mobile Services</span><span class="sxs-lookup"><span data-stu-id="79992-159">Mobile Services</span></span>](#mobile-services-limits)
* [<span data-ttu-id="79992-160">Övervaka</span><span class="sxs-lookup"><span data-stu-id="79992-160">Monitor</span></span>](#monitor-limits)
* [<span data-ttu-id="79992-161">Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="79992-161">Multi-Factor Authentication</span></span>](#multi-factor-authentication)
* [<span data-ttu-id="79992-162">Nätverk</span><span class="sxs-lookup"><span data-stu-id="79992-162">Networking</span></span>](#networking-limits)
* [<span data-ttu-id="79992-163">Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="79992-163">Network Watcher</span></span>](#network-watcher-limits)
* [<span data-ttu-id="79992-164">Notification Hub Service</span><span class="sxs-lookup"><span data-stu-id="79992-164">Notification Hub Service</span></span>](#notification-hub-service-limits)
* [<span data-ttu-id="79992-165">Resursgrupp</span><span class="sxs-lookup"><span data-stu-id="79992-165">Resource Group</span></span>](#resource-group-limits)
* [<span data-ttu-id="79992-166">Scheduler</span><span class="sxs-lookup"><span data-stu-id="79992-166">Scheduler</span></span>](#scheduler-limits)
* [<span data-ttu-id="79992-167">Sök</span><span class="sxs-lookup"><span data-stu-id="79992-167">Search</span></span>](#search-limits)
* [<span data-ttu-id="79992-168">Service Bus</span><span class="sxs-lookup"><span data-stu-id="79992-168">Service Bus</span></span>](#service-bus-limits)
* [<span data-ttu-id="79992-169">Site Recovery</span><span class="sxs-lookup"><span data-stu-id="79992-169">Site Recovery</span></span>](#site-recovery-limits)
* [<span data-ttu-id="79992-170">SQL Database</span><span class="sxs-lookup"><span data-stu-id="79992-170">SQL Database</span></span>](#sql-database-limits)
* [<span data-ttu-id="79992-171">Storage</span><span class="sxs-lookup"><span data-stu-id="79992-171">Storage</span></span>](#storage-limits)
* [<span data-ttu-id="79992-172">StorSimple-System</span><span class="sxs-lookup"><span data-stu-id="79992-172">StorSimple System</span></span>](#storsimple-system-limits)
* [<span data-ttu-id="79992-173">Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="79992-173">Stream Analytics</span></span>](#stream-analytics-limits)
* [<span data-ttu-id="79992-174">Prenumeration</span><span class="sxs-lookup"><span data-stu-id="79992-174">Subscription</span></span>](#subscription-limits)
* [<span data-ttu-id="79992-175">Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="79992-175">Traffic Manager</span></span>](#traffic-manager-limits)
* [<span data-ttu-id="79992-176">Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="79992-176">Virtual Machines</span></span>](#virtual-machines-limits)
* [<span data-ttu-id="79992-177">Skaluppsättningar för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="79992-177">Virtual Machine Scale Sets</span></span>](#virtual-machine-scale-sets-limits)

### <a name="subscription-limits"></a><span data-ttu-id="79992-178">Prenumerationsbegränsningar</span><span class="sxs-lookup"><span data-stu-id="79992-178">Subscription limits</span></span>
#### <a name="subscription-limits"></a><span data-ttu-id="79992-179">Prenumerationsbegränsningar</span><span class="sxs-lookup"><span data-stu-id="79992-179">Subscription limits</span></span>
[!INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a><span data-ttu-id="79992-180">Prenumerationsbegränsningar - Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="79992-180">Subscription limits - Azure Resource Manager</span></span>
<span data-ttu-id="79992-181">Följande begränsningar gäller när du använder Azure Resource Manager och Azure-resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="79992-181">The following limits apply when using the Azure Resource Manager and Azure Resource Groups.</span></span> <span data-ttu-id="79992-182">Nedan visas inte gränser som inte har ändrats med Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="79992-182">Limits that have not changed with the Azure Resource Manager are not listed below.</span></span> <span data-ttu-id="79992-183">Hänvisa till den föregående tabellen för dessa begränsningar.</span><span class="sxs-lookup"><span data-stu-id="79992-183">Please refer to the previous table for those limits.</span></span>

<span data-ttu-id="79992-184">Information om hur du hanterar gränser för Resource Manager-begäranden finns [begränsning Resource Manager begär](resource-manager-request-limits.md).</span><span class="sxs-lookup"><span data-stu-id="79992-184">For information about handling limits on Resource Manager requests, see [Throttling Resource Manager requests](resource-manager-request-limits.md).</span></span>

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="resource-group-limits"></a><span data-ttu-id="79992-185">Resursgruppen gränser</span><span class="sxs-lookup"><span data-stu-id="79992-185">Resource Group limits</span></span>
[!INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a><span data-ttu-id="79992-186">Begränsningar för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="79992-186">Virtual Machines limits</span></span>
#### <a name="virtual-machine-limits"></a><span data-ttu-id="79992-187">Begränsningar för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="79992-187">Virtual Machine limits</span></span>
[!INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]

#### <a name="virtual-machines-limits---azure-resource-manager"></a><span data-ttu-id="79992-188">Virtuella datorer gränser - Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="79992-188">Virtual Machines limits - Azure Resource Manager</span></span>
<span data-ttu-id="79992-189">Följande begränsningar gäller när du använder Azure Resource Manager och Azure-resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="79992-189">The following limits apply when using the Azure Resource Manager and Azure Resource Groups.</span></span> <span data-ttu-id="79992-190">Nedan visas inte gränser som inte har ändrats med Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="79992-190">Limits that have not changed with the Azure Resource Manager are not listed below.</span></span> <span data-ttu-id="79992-191">Hänvisa till den föregående tabellen för dessa begränsningar.</span><span class="sxs-lookup"><span data-stu-id="79992-191">Please refer to the previous table for those limits.</span></span>

[!INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a><span data-ttu-id="79992-192">Skaluppsättningar för den virtuella datorn gränser</span><span class="sxs-lookup"><span data-stu-id="79992-192">Virtual Machine Scale Sets limits</span></span>
[!INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="container-instances-limits"></a><span data-ttu-id="79992-193">Behållaren instanser gränser</span><span class="sxs-lookup"><span data-stu-id="79992-193">Container Instances Limits</span></span>
[!INCLUDE [container-instances-limits](../includes/container-instances-limits.md)]

### <a name="networking-limits"></a><span data-ttu-id="79992-194">Begränsningar för nätverk</span><span class="sxs-lookup"><span data-stu-id="79992-194">Networking limits</span></span>
[!INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a><span data-ttu-id="79992-195">Begränsningar för nätverk</span><span class="sxs-lookup"><span data-stu-id="79992-195">Networking limits</span></span>
[!INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a><span data-ttu-id="79992-196">Begränsar Programgateway</span><span class="sxs-lookup"><span data-stu-id="79992-196">Application Gateway limits</span></span>
[!INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="network-watcher-limits"></a><span data-ttu-id="79992-197">Nätverksbevakaren begränsar</span><span class="sxs-lookup"><span data-stu-id="79992-197">Network Watcher limits</span></span>
[!INCLUDE [network-watcher-limits](../includes/network-watcher-limits.md)]

#### <a name="traffic-manager-limits"></a><span data-ttu-id="79992-198">Traffic Manager-gränser</span><span class="sxs-lookup"><span data-stu-id="79992-198">Traffic Manager limits</span></span>
[!INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a><span data-ttu-id="79992-199">DNS-begränsningar</span><span class="sxs-lookup"><span data-stu-id="79992-199">DNS limits</span></span>
[!INCLUDE [dns-limits](../includes/dns-limits.md)]

### <a name="storage-limits"></a><span data-ttu-id="79992-200">Lagringsgränser</span><span class="sxs-lookup"><span data-stu-id="79992-200">Storage limits</span></span>
<span data-ttu-id="79992-201">Ytterligare information om lagringskontogränser finns [Azure Storage skalbarhets- och prestandamål](storage/common/storage-scalability-targets.md).</span><span class="sxs-lookup"><span data-stu-id="79992-201">For additional details on storage account limits, see [Azure Storage Scalability and Performance Targets](storage/common/storage-scalability-targets.md).</span></span>
<!--like # storage accts --> 
#### <a name="storage-service-limits"></a><span data-ttu-id="79992-202">Lagringsgränser för tjänsten</span><span class="sxs-lookup"><span data-stu-id="79992-202">Storage Service limits</span></span>
[!INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

<!-- conceptual info about disk limits -- applies to unmanaged and managed -->
#### <a name="virtual-machine-disk-limits"></a><span data-ttu-id="79992-203">Virtuell disk gränser</span><span class="sxs-lookup"><span data-stu-id="79992-203">Virtual machine disk limits</span></span> 
[!INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

<span data-ttu-id="79992-204">Se [storlekar för virtuella datorer](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) för ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="79992-204">See [Virtual machine sizes](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for additional details.</span></span>

#### <a name="managed-virtual-machine-disks"></a><span data-ttu-id="79992-205">Hanterade virtuella diskar</span><span class="sxs-lookup"><span data-stu-id="79992-205">Managed virtual machine disks</span></span>

[!INCLUDE [azure-storage-limits-vm-disks-managed](../includes/azure-storage-limits-vm-disks-managed.md)]

#### <a name="unmanaged-virtual-machine-disks"></a><span data-ttu-id="79992-206">Ohanterad virtuella diskar</span><span class="sxs-lookup"><span data-stu-id="79992-206">Unmanaged virtual machine disks</span></span>

[!INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### <a name="storage-resource-provider-limits"></a><span data-ttu-id="79992-207">Begränsar Lagringsresursprovidern</span><span class="sxs-lookup"><span data-stu-id="79992-207">Storage Resource Provider limits</span></span>
[!INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]

### <a name="cloud-services-limits"></a><span data-ttu-id="79992-208">Cloud Services gränser</span><span class="sxs-lookup"><span data-stu-id="79992-208">Cloud Services limits</span></span>
[!INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]

### <a name="app-service-limits"></a><span data-ttu-id="79992-209">Gränser för Apptjänst</span><span class="sxs-lookup"><span data-stu-id="79992-209">App Service limits</span></span>
<span data-ttu-id="79992-210">Följande gränser för Apptjänst innehåller gränser för Webbappar, Mobilappar, API Apps och Logikappar.</span><span class="sxs-lookup"><span data-stu-id="79992-210">The following App Service limits include limits for Web Apps, Mobile Apps, API Apps, and Logic Apps.</span></span>

[!INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a><span data-ttu-id="79992-211">Schemaläggaren gränser</span><span class="sxs-lookup"><span data-stu-id="79992-211">Scheduler limits</span></span>
[!INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a><span data-ttu-id="79992-212">Batch-gränser</span><span class="sxs-lookup"><span data-stu-id="79992-212">Batch limits</span></span>
[!INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

### <a name="biztalk-services-limits"></a><span data-ttu-id="79992-213">BizTalk-tjänst gränser</span><span class="sxs-lookup"><span data-stu-id="79992-213">BizTalk Services limits</span></span>
<span data-ttu-id="79992-214">Följande tabell visar gränserna för Azure Biztalk-tjänst.</span><span class="sxs-lookup"><span data-stu-id="79992-214">The following table shows the limits for Azure Biztalk Services.</span></span>

[!INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]

### <a name="azure-cosmos-db-limits"></a><span data-ttu-id="79992-215">Azure DB Cosmos-gränser</span><span class="sxs-lookup"><span data-stu-id="79992-215">Azure Cosmos DB limits</span></span>
<span data-ttu-id="79992-216">Azure Cosmos-DB är en global skala databas där dataflöden och lagringsutrymmen kan skalas för att hantera det krävs för ditt program.</span><span class="sxs-lookup"><span data-stu-id="79992-216">Azure Cosmos DB is a global scale database in which throughput and storage can be scaled to handle whatever your application requires.</span></span> <span data-ttu-id="79992-217">Om du har frågor om Azure Cosmos DB tillhandahåller skalan skickar e-postmeddelande till askcosmosdb@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="79992-217">If you have any questions about the scale Azure Cosmos DB provides, please send email to askcosmosdb@microsoft.com.</span></span>

### <a name="mobile-engagement-limits"></a><span data-ttu-id="79992-218">Mobile Engagement gränser</span><span class="sxs-lookup"><span data-stu-id="79992-218">Mobile Engagement limits</span></span>
[!INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]

### <a name="search-limits"></a><span data-ttu-id="79992-219">Sök gränser</span><span class="sxs-lookup"><span data-stu-id="79992-219">Search limits</span></span>
<span data-ttu-id="79992-220">Prisnivåer avgöra kapaciteten och gränserna för din söktjänst.</span><span class="sxs-lookup"><span data-stu-id="79992-220">Pricing tiers determine the capacity and limits of your search service.</span></span> <span data-ttu-id="79992-221">Nivåer omfattar:</span><span class="sxs-lookup"><span data-stu-id="79992-221">Tiers include:</span></span>

* <span data-ttu-id="79992-222">*Ledigt* tjänst med flera klienter, delas med andra Azure-prenumeranter avsedd för utvärdering och utveckling av små projekt.</span><span class="sxs-lookup"><span data-stu-id="79992-222">*Free* multi-tenant service, shared with other Azure subscribers, intended for evaluation and small development projects.</span></span>
* <span data-ttu-id="79992-223">*Grundläggande* ger dedikerade datorresurser för produktionsarbetsbelastningar i mindre skala med upp till tre repliker för hög tillgänglighet frågan arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="79992-223">*Basic* provides dedicated computing resources for production workloads at a smaller scale, with up to three replicas for highly available query workloads.</span></span>
* <span data-ttu-id="79992-224">*Standard (S1, S2, S3, S3 hög densitet)* är för större produktionsarbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="79992-224">*Standard (S1, S2, S3, S3 High Density)* is for larger production workloads.</span></span> <span data-ttu-id="79992-225">Det finns flera nivåer inom standardnivån så att du kan välja en resurskonfigurationen som bäst passar din arbetsbelastning profil.</span><span class="sxs-lookup"><span data-stu-id="79992-225">Multiple levels exist within the standard tier so that you can choose a resource configuration that best matches your workload profile.</span></span>

<span data-ttu-id="79992-226">**Gränser per prenumeration**</span><span class="sxs-lookup"><span data-stu-id="79992-226">**Limits per subscription**</span></span>

[!INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

<span data-ttu-id="79992-227">**Gränser för search-tjänsten**</span><span class="sxs-lookup"><span data-stu-id="79992-227">**Limits per search service**</span></span>

[!INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

<span data-ttu-id="79992-228">Läs mer om gränserna för en mer detaljerad nivå, till exempel storlek, frågor per sekund, nycklar, begäranden och svar, [gränser i Azure Search](search/search-limits-quotas-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="79992-228">To learn more about limits on a more granular level, such as document size, queries per second, keys, requests, and responses, see [Service limits in Azure Search](search/search-limits-quotas-capacity.md).</span></span>

### <a name="media-services-limits"></a><span data-ttu-id="79992-229">Media Services-gränser</span><span class="sxs-lookup"><span data-stu-id="79992-229">Media Services limits</span></span>
[!INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a><span data-ttu-id="79992-230">CDN-gränser</span><span class="sxs-lookup"><span data-stu-id="79992-230">CDN limits</span></span>
[!INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a><span data-ttu-id="79992-231">Begränsningar för Mobile Services</span><span class="sxs-lookup"><span data-stu-id="79992-231">Mobile Services limits</span></span>
[!INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitor-limits"></a><span data-ttu-id="79992-232">Övervakaren gränser</span><span class="sxs-lookup"><span data-stu-id="79992-232">Monitor limits</span></span>
[!INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a><span data-ttu-id="79992-233">Tjänstbegränsningarna för Notification Hub</span><span class="sxs-lookup"><span data-stu-id="79992-233">Notification Hub Service limits</span></span>
[!INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a><span data-ttu-id="79992-234">Event Hubs gränser</span><span class="sxs-lookup"><span data-stu-id="79992-234">Event Hubs limits</span></span>
[!INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a><span data-ttu-id="79992-235">Service Bus-gränser</span><span class="sxs-lookup"><span data-stu-id="79992-235">Service Bus limits</span></span>
[!INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a><span data-ttu-id="79992-236">IoT-hubb gränser</span><span class="sxs-lookup"><span data-stu-id="79992-236">IoT Hub limits</span></span>
[!INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="data-factory-limits"></a><span data-ttu-id="79992-237">Data Factory begränsar</span><span class="sxs-lookup"><span data-stu-id="79992-237">Data Factory limits</span></span>
[!INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a><span data-ttu-id="79992-238">Data Lake Analytics-gränser</span><span class="sxs-lookup"><span data-stu-id="79992-238">Data Lake Analytics limits</span></span>
[!INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="data-lake-store-limits"></a><span data-ttu-id="79992-239">Data Lake Store-gränser</span><span class="sxs-lookup"><span data-stu-id="79992-239">Data Lake Store limits</span></span>
[!INCLUDE [azure-data-lake-store-limits](../includes/azure-data-lake-store-limits.md)]

### <a name="stream-analytics-limits"></a><span data-ttu-id="79992-240">Stream Analytics begränsar</span><span class="sxs-lookup"><span data-stu-id="79992-240">Stream Analytics limits</span></span>
[!INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a><span data-ttu-id="79992-241">Active Directory-gränser</span><span class="sxs-lookup"><span data-stu-id="79992-241">Active Directory limits</span></span>
[!INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]

### <a name="azure-event-grid-limits"></a><span data-ttu-id="79992-242">Azure händelse rutnätet gränser</span><span class="sxs-lookup"><span data-stu-id="79992-242">Azure Event Grid limits</span></span>
[!INCLUDE [event-grid-limits](../includes/event-grid-limits.md)]

### <a name="azure-remoteapp-limits"></a><span data-ttu-id="79992-243">Azure RemoteApp-gränser</span><span class="sxs-lookup"><span data-stu-id="79992-243">Azure RemoteApp limits</span></span>
[!INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### <a name="storsimple-system-limits"></a><span data-ttu-id="79992-244">Begränsar StorSimple-System</span><span class="sxs-lookup"><span data-stu-id="79992-244">StorSimple System limits</span></span>
[!INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]

### <a name="log-analytics-limits"></a><span data-ttu-id="79992-245">Begränsar logganalys</span><span class="sxs-lookup"><span data-stu-id="79992-245">Log Analytics limits</span></span>
[!INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a><span data-ttu-id="79992-246">Säkerhetskopiering gränser</span><span class="sxs-lookup"><span data-stu-id="79992-246">Backup limits</span></span>
[!INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a><span data-ttu-id="79992-247">Gränser för Site Recovery</span><span class="sxs-lookup"><span data-stu-id="79992-247">Site Recovery limits</span></span>
[!INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a><span data-ttu-id="79992-248">Application Insights gränser</span><span class="sxs-lookup"><span data-stu-id="79992-248">Application Insights limits</span></span>
[!INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a><span data-ttu-id="79992-249">API Management-gränser</span><span class="sxs-lookup"><span data-stu-id="79992-249">API Management limits</span></span>
[!INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a><span data-ttu-id="79992-250">Azure Redis-Cache-gränser</span><span class="sxs-lookup"><span data-stu-id="79992-250">Azure Redis Cache limits</span></span>
[!INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a><span data-ttu-id="79992-251">Key Vault-gränser</span><span class="sxs-lookup"><span data-stu-id="79992-251">Key Vault limits</span></span>
[!INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a><span data-ttu-id="79992-252">Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="79992-252">Multi-Factor Authentication</span></span>
[!INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a><span data-ttu-id="79992-253">Automation-gränser</span><span class="sxs-lookup"><span data-stu-id="79992-253">Automation limits</span></span>
[!INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="sql-database-limits"></a><span data-ttu-id="79992-254">Begränsar SQL-databas</span><span class="sxs-lookup"><span data-stu-id="79992-254">SQL Database limits</span></span>
<span data-ttu-id="79992-255">SQL-databas gränser, se [gränserna för SQL-databasen](sql-database/sql-database-resource-limits.md).</span><span class="sxs-lookup"><span data-stu-id="79992-255">For SQL Database limits, see [SQL Database Resource Limits](sql-database/sql-database-resource-limits.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="79992-256">Se även</span><span class="sxs-lookup"><span data-stu-id="79992-256">See also</span></span>
[<span data-ttu-id="79992-257">Förstå Azure gränser och ökar</span><span class="sxs-lookup"><span data-stu-id="79992-257">Understanding Azure Limits and Increases</span></span>](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[<span data-ttu-id="79992-258">Virtuell dator och Molntjänststorlekar för Azure</span><span class="sxs-lookup"><span data-stu-id="79992-258">Virtual Machine and Cloud Service Sizes for Azure</span></span>](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="79992-259">Storlekar för molntjänster</span><span class="sxs-lookup"><span data-stu-id="79992-259">Sizes for Cloud Services</span></span>](cloud-services/cloud-services-sizes-specs.md)

