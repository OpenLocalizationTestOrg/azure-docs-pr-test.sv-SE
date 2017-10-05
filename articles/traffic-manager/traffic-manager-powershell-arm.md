---
title: "Använda PowerShell för att hantera Traffic Manager i Azure | Microsoft Docs"
description: "Med hjälp av PowerShell för Traffic Manager med Azure Resource Manager"
services: traffic-manager
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: bc247448-1d2e-4104-ac03-42b59ebde065
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: 1cd7bd7e32c96398d72c7cd3b51e2b456d60f01d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="using-powershell-to-manage-traffic-manager"></a><span data-ttu-id="da7f1-103">Använda PowerShell för att hantera Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="da7f1-103">Using PowerShell to manage Traffic Manager</span></span>

<span data-ttu-id="da7f1-104">Azure Resource Manager är det primära gränssnittet för tjänster i Azure.</span><span class="sxs-lookup"><span data-stu-id="da7f1-104">Azure Resource Manager is the preferred management interface for services in Azure.</span></span> <span data-ttu-id="da7f1-105">Azure Traffic Manager-profiler kan hanteras med Azure Resource Manager-baserade API: er och verktyg.</span><span class="sxs-lookup"><span data-stu-id="da7f1-105">Azure Traffic Manager profiles can be managed using Azure Resource Manager-based APIs and tools.</span></span>

## <a name="resource-model"></a><span data-ttu-id="da7f1-106">Resursmodell</span><span class="sxs-lookup"><span data-stu-id="da7f1-106">Resource model</span></span>

<span data-ttu-id="da7f1-107">Azure Traffic Manager konfigureras med hjälp av en samling inställningar som kallas en Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="da7f1-107">Azure Traffic Manager is configured using a collection of settings called a Traffic Manager profile.</span></span> <span data-ttu-id="da7f1-108">Den här profilen innehåller DNS-inställningar, trafikdirigeringsinställningar, slutpunktsövervakningsinställningar och en lista över slutpunkter som trafik dirigeras.</span><span class="sxs-lookup"><span data-stu-id="da7f1-108">This profile contains DNS settings, traffic routing settings, endpoint monitoring settings, and a list of service endpoints to which traffic is routed.</span></span>

<span data-ttu-id="da7f1-109">Varje trafikhanterarprofil representeras av en resurs av typen 'TrafficManagerProfiles'.</span><span class="sxs-lookup"><span data-stu-id="da7f1-109">Each Traffic Manager profile is represented by a resource of type 'TrafficManagerProfiles'.</span></span> <span data-ttu-id="da7f1-110">På REST API-nivå är URI: N för varje profil:</span><span class="sxs-lookup"><span data-stu-id="da7f1-110">At the REST API level, the URI for each profile is as follows:</span></span>

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="setting-up-azure-powershell"></a><span data-ttu-id="da7f1-111">Konfigurera Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="da7f1-111">Setting up Azure PowerShell</span></span>

<span data-ttu-id="da7f1-112">Dessa anvisningar använder Microsoft Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="da7f1-112">These instructions use Microsoft Azure PowerShell.</span></span> <span data-ttu-id="da7f1-113">I följande artikel förklarar hur du installerar och konfigurerar du Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="da7f1-113">The following article explains how to install and configure Azure PowerShell.</span></span>

* [<span data-ttu-id="da7f1-114">Installera och konfigurera Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="da7f1-114">How to install and configure Azure PowerShell</span></span>](/powershell/azure/overview)

<span data-ttu-id="da7f1-115">Exemplen i den här artikeln förutsätter att du har en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="da7f1-115">The examples in this article assume that you have an existing resource group.</span></span> <span data-ttu-id="da7f1-116">Du kan skapa en resursgrupp med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="da7f1-116">You can create a resource group using the following command:</span></span>

```powershell
New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

> [!NOTE]
> <span data-ttu-id="da7f1-117">Azure Resource Manager kräver att alla resursgrupper har en plats.</span><span class="sxs-lookup"><span data-stu-id="da7f1-117">Azure Resource Manager requires that all resource groups have a location.</span></span> <span data-ttu-id="da7f1-118">Den här platsen används som standard för resurser som skapats i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="da7f1-118">This location is used as the default for resources created in that resource group.</span></span> <span data-ttu-id="da7f1-119">Men eftersom resurser för Traffic Manager-profil är globala, inte regional är påverkar valet av resursgruppens plats inte på Azure Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="da7f1-119">However, since Traffic Manager profile resources are global, not regional, the choice of resource group location has no impact on Azure Traffic Manager.</span></span>

## <a name="create-a-traffic-manager-profile"></a><span data-ttu-id="da7f1-120">Skapa en Trafikhanterarprofil</span><span class="sxs-lookup"><span data-stu-id="da7f1-120">Create a Traffic Manager Profile</span></span>

<span data-ttu-id="da7f1-121">Du kan skapa en Traffic Manager-profil med den `New-AzureRmTrafficManagerProfile` cmdlet:</span><span class="sxs-lookup"><span data-stu-id="da7f1-121">To create a Traffic Manager profile, use the `New-AzureRmTrafficManagerProfile` cmdlet:</span></span>

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

<span data-ttu-id="da7f1-122">I följande tabell beskrivs parametrarna:</span><span class="sxs-lookup"><span data-stu-id="da7f1-122">The following table describes the parameters:</span></span>

| <span data-ttu-id="da7f1-123">Parameter</span><span class="sxs-lookup"><span data-stu-id="da7f1-123">Parameter</span></span> | <span data-ttu-id="da7f1-124">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="da7f1-124">Description</span></span> |
| --- | --- |
| <span data-ttu-id="da7f1-125">Namn</span><span class="sxs-lookup"><span data-stu-id="da7f1-125">Name</span></span> |<span data-ttu-id="da7f1-126">Resursnamnet för resursen Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="da7f1-126">The resource name for the Traffic Manager profile resource.</span></span> <span data-ttu-id="da7f1-127">Profiler i samma resursgrupp måste ha unika namn.</span><span class="sxs-lookup"><span data-stu-id="da7f1-127">Profiles in the same resource group must have unique names.</span></span> <span data-ttu-id="da7f1-128">Det här namnet skiljer sig från DNS-namn som används för DNS-frågor.</span><span class="sxs-lookup"><span data-stu-id="da7f1-128">This name is separate from the DNS name used for DNS queries.</span></span> |
| <span data-ttu-id="da7f1-129">resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="da7f1-129">ResourceGroupName</span></span> |<span data-ttu-id="da7f1-130">Namnet på resursgruppen som innehåller profilen för resursen.</span><span class="sxs-lookup"><span data-stu-id="da7f1-130">The name of the resource group containing the profile resource.</span></span> |
| <span data-ttu-id="da7f1-131">TrafficRoutingMethod</span><span class="sxs-lookup"><span data-stu-id="da7f1-131">TrafficRoutingMethod</span></span> |<span data-ttu-id="da7f1-132">Anger den metod för routning av nätverkstrafik som används för att avgöra vilken slutpunkt returneras svar en DNS-fråga.</span><span class="sxs-lookup"><span data-stu-id="da7f1-132">Specifies the traffic-routing method used to determine which endpoint is returned in response a DNS query.</span></span> <span data-ttu-id="da7f1-133">Möjliga värden är 'Prestanda', 'Viktat' eller 'Priority'.</span><span class="sxs-lookup"><span data-stu-id="da7f1-133">Possible values are 'Performance', 'Weighted' or 'Priority'.</span></span> |
| <span data-ttu-id="da7f1-134">RelativeDnsName</span><span class="sxs-lookup"><span data-stu-id="da7f1-134">RelativeDnsName</span></span> |<span data-ttu-id="da7f1-135">Anger hostname-delen av DNS-namn som tillhandahålls av den här trafikhanterarprofilen.</span><span class="sxs-lookup"><span data-stu-id="da7f1-135">Specifies the hostname portion of the DNS name provided by this Traffic Manager profile.</span></span> <span data-ttu-id="da7f1-136">Det här värdet kombineras med DNS-domännamnet som används av Azure Traffic Manager för att bilda fullständigt kvalificerade domännamnet (FQDN) för profilen.</span><span class="sxs-lookup"><span data-stu-id="da7f1-136">This value is combined with the DNS domain name used by Azure Traffic Manager to form the fully qualified domain name (FQDN) of the profile.</span></span> <span data-ttu-id="da7f1-137">Till exempel blir ange värdet för ”contoso” contoso.trafficmanager.net.</span><span class="sxs-lookup"><span data-stu-id="da7f1-137">For example, setting the value of 'contoso' becomes 'contoso.trafficmanager.net.'</span></span> |
| <span data-ttu-id="da7f1-138">TTL-VÄRDE</span><span class="sxs-lookup"><span data-stu-id="da7f1-138">TTL</span></span> |<span data-ttu-id="da7f1-139">Anger DNS-Time-to-Live (TTL), i sekunder.</span><span class="sxs-lookup"><span data-stu-id="da7f1-139">Specifies the DNS Time-to-Live (TTL), in seconds.</span></span> <span data-ttu-id="da7f1-140">Den här TTL informerar den lokala DNS-matchare och DNS-klienter, hur lång tid att cache DNS-svar för den här Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="da7f1-140">This TTL informs the Local DNS resolvers and DNS clients how long to cache DNS responses for this Traffic Manager profile.</span></span> |
| <span data-ttu-id="da7f1-141">MonitorProtocol</span><span class="sxs-lookup"><span data-stu-id="da7f1-141">MonitorProtocol</span></span> |<span data-ttu-id="da7f1-142">Anger vilket protokoll som ska använda för att övervaka hälsotillståndet för slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="da7f1-142">Specifies the protocol to use to monitor endpoint health.</span></span> <span data-ttu-id="da7f1-143">Möjliga värden är 'HTTP' och 'HTTPS'.</span><span class="sxs-lookup"><span data-stu-id="da7f1-143">Possible values are 'HTTP' and 'HTTPS'.</span></span> |
| <span data-ttu-id="da7f1-144">MonitorPort</span><span class="sxs-lookup"><span data-stu-id="da7f1-144">MonitorPort</span></span> |<span data-ttu-id="da7f1-145">Anger den TCP-port som används för att övervaka hälsotillståndet för slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="da7f1-145">Specifies the TCP port used to monitor endpoint health.</span></span> |
| <span data-ttu-id="da7f1-146">MonitorPath</span><span class="sxs-lookup"><span data-stu-id="da7f1-146">MonitorPath</span></span> |<span data-ttu-id="da7f1-147">Anger sökväg i förhållande till slutpunkten domännamnet till avsökning för slutpunkten health.</span><span class="sxs-lookup"><span data-stu-id="da7f1-147">Specifies the path relative to the endpoint domain name used to probe for endpoint health.</span></span> |

<span data-ttu-id="da7f1-148">Cmdleten skapar en Traffic Manager-profil i Azure och returnerar ett motsvarande profil-objekt till PowerShell.</span><span class="sxs-lookup"><span data-stu-id="da7f1-148">The cmdlet creates a Traffic Manager profile in Azure and returns a corresponding profile object to PowerShell.</span></span> <span data-ttu-id="da7f1-149">Profilen som innehåller nu inte några slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="da7f1-149">At this point, the profile does not contain any endpoints.</span></span> <span data-ttu-id="da7f1-150">Mer information om att lägga till slutpunkter i en Traffic Manager-profilen finns [att lägga till slutpunkter för Traffic Manager](#adding-traffic-manager-endpoints).</span><span class="sxs-lookup"><span data-stu-id="da7f1-150">For more information about adding endpoints to a Traffic Manager profile, see [Adding Traffic Manager Endpoints](#adding-traffic-manager-endpoints).</span></span>

## <a name="get-a-traffic-manager-profile"></a><span data-ttu-id="da7f1-151">Hämta en Trafikhanterarprofil</span><span class="sxs-lookup"><span data-stu-id="da7f1-151">Get a Traffic Manager Profile</span></span>

<span data-ttu-id="da7f1-152">Använd för att hämta ett befintligt objekt för Traffic Manager-profilen i `Get-AzureRmTrafficManagerProfle` cmdlet:</span><span class="sxs-lookup"><span data-stu-id="da7f1-152">To retrieve an existing Traffic Manager profile object, use the `Get-AzureRmTrafficManagerProfle` cmdlet:</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

<span data-ttu-id="da7f1-153">Denna cmdlet returnerar ett objekt för Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="da7f1-153">This cmdlet returns a Traffic Manager profile object.</span></span>

## <a name="update-a-traffic-manager-profile"></a><span data-ttu-id="da7f1-154">Uppdatera en Traffic Manager-profil</span><span class="sxs-lookup"><span data-stu-id="da7f1-154">Update a Traffic Manager Profile</span></span>

<span data-ttu-id="da7f1-155">Ändra Traffic Manager-profiler följer en process i steg 3:</span><span class="sxs-lookup"><span data-stu-id="da7f1-155">Modifying Traffic Manager profiles follows a 3-step process:</span></span>

1. <span data-ttu-id="da7f1-156">Hämta en profil med hjälp av `Get-AzureRmTrafficManagerProfile` eller använda den profil som returneras av `New-AzureRmTrafficManagerProfile`.</span><span class="sxs-lookup"><span data-stu-id="da7f1-156">Retrieve the profile using `Get-AzureRmTrafficManagerProfile` or use the profile returned by `New-AzureRmTrafficManagerProfile`.</span></span>
2. <span data-ttu-id="da7f1-157">Ändra profilen.</span><span class="sxs-lookup"><span data-stu-id="da7f1-157">Modify the profile.</span></span> <span data-ttu-id="da7f1-158">Du kan lägga till och ta bort slutpunkter eller ändra slutpunkten eller profil parametrar.</span><span class="sxs-lookup"><span data-stu-id="da7f1-158">You can add and remove endpoints or change endpoint or profile parameters.</span></span> <span data-ttu-id="da7f1-159">Dessa ändringar är offline åtgärder.</span><span class="sxs-lookup"><span data-stu-id="da7f1-159">These changes are off-line operations.</span></span> <span data-ttu-id="da7f1-160">Endast ändrar det lokala objektet i minnet som representerar profilen.</span><span class="sxs-lookup"><span data-stu-id="da7f1-160">You are only changing the local object in memory that represents the profile.</span></span>
3. <span data-ttu-id="da7f1-161">Genomför ändringarna med hjälp av den `Set-AzureRmTrafficManagerProfile` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="da7f1-161">Commit your changes using the `Set-AzureRmTrafficManagerProfile` cmdlet.</span></span>

<span data-ttu-id="da7f1-162">Alla profilegenskaper kan ändras utom profilens RelativeDnsName.</span><span class="sxs-lookup"><span data-stu-id="da7f1-162">All profile properties can be changed except the profile's RelativeDnsName.</span></span> <span data-ttu-id="da7f1-163">Om du vill ändra RelativeDnsName, måste du ta bort profil och en ny profil med ett nytt namn.</span><span class="sxs-lookup"><span data-stu-id="da7f1-163">To change the RelativeDnsName, you must delete profile and a new profile with a new name.</span></span>

<span data-ttu-id="da7f1-164">Exemplet nedan visar hur du ändrar den profilen TTL-värde:</span><span class="sxs-lookup"><span data-stu-id="da7f1-164">The following example demonstrates how to change the profile's TTL:</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
$profile.Ttl = 300
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

<span data-ttu-id="da7f1-165">Det finns tre typer av slutpunkter för Traffic Manager:</span><span class="sxs-lookup"><span data-stu-id="da7f1-165">There are three types of Traffic Manager endpoints:</span></span>

1. <span data-ttu-id="da7f1-166">**Azure-slutpunkter** är tjänster som finns på Azure</span><span class="sxs-lookup"><span data-stu-id="da7f1-166">**Azure endpoints** are services hosted in Azure</span></span>
2. <span data-ttu-id="da7f1-167">**Externa slutpunkter** är tjänster som ligger utanför Azure</span><span class="sxs-lookup"><span data-stu-id="da7f1-167">**External endpoints** are services hosted outside of Azure</span></span>
3. <span data-ttu-id="da7f1-168">**Kapslade slutpunkter** används för att skapa kapslade hierarkier av Traffic Manager-profiler.</span><span class="sxs-lookup"><span data-stu-id="da7f1-168">**Nested endpoints** are used to construct nested hierarchies of Traffic Manager profiles.</span></span> <span data-ttu-id="da7f1-169">Kapslade slutpunkter aktivera avancerade routning av nätverkstrafik konfigurationer för komplexa program.</span><span class="sxs-lookup"><span data-stu-id="da7f1-169">Nested endpoints enable advanced traffic-routing configurations for complex applications.</span></span>

<span data-ttu-id="da7f1-170">I samtliga tre fall kan du lägga till slutpunkter på två sätt:</span><span class="sxs-lookup"><span data-stu-id="da7f1-170">In all three cases, endpoints can be added in two ways:</span></span>

1. <span data-ttu-id="da7f1-171">Med hjälp av en process för 3 steg som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="da7f1-171">Using a 3-step process described previously.</span></span> <span data-ttu-id="da7f1-172">Fördelen med den här metoden är att flera endpoint kan ändras i en enda uppdatering.</span><span class="sxs-lookup"><span data-stu-id="da7f1-172">The advantage of this method is that several endpoint changes can be made in a single update.</span></span>
2. <span data-ttu-id="da7f1-173">Med hjälp av cmdlet New-AzureRmTrafficManagerEndpoint.</span><span class="sxs-lookup"><span data-stu-id="da7f1-173">Using the New-AzureRmTrafficManagerEndpoint cmdlet.</span></span> <span data-ttu-id="da7f1-174">Denna cmdlet lägger till en slutpunkt i en befintlig Traffic Manager-profil i en enda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="da7f1-174">This cmdlet adds an endpoint to an existing Traffic Manager profile in a single operation.</span></span>

## <a name="adding-azure-endpoints"></a><span data-ttu-id="da7f1-175">Lägga till Azure-slutpunkter</span><span class="sxs-lookup"><span data-stu-id="da7f1-175">Adding Azure Endpoints</span></span>

<span data-ttu-id="da7f1-176">Azure-slutpunkter referera till tjänster i Azure.</span><span class="sxs-lookup"><span data-stu-id="da7f1-176">Azure endpoints reference services hosted in Azure.</span></span> <span data-ttu-id="da7f1-177">Två typer av Azure-slutpunkter stöds:</span><span class="sxs-lookup"><span data-stu-id="da7f1-177">Two types of Azure endpoints are supported:</span></span>

1. <span data-ttu-id="da7f1-178">Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="da7f1-178">Azure Web Apps</span></span>
2. <span data-ttu-id="da7f1-179">Azure PublicIpAddress-resurser (som kan kopplas till en belastningsutjämnare eller en virtuell dator NIC).</span><span class="sxs-lookup"><span data-stu-id="da7f1-179">Azure PublicIpAddress resources (which can be attached to a load-balancer or a virtual machine NIC).</span></span> <span data-ttu-id="da7f1-180">PublicIpAddress måste ha ett DNS-namn som tilldelats som ska användas i Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="da7f1-180">The PublicIpAddress must have a DNS name assigned to be used in Traffic Manager.</span></span>

<span data-ttu-id="da7f1-181">I varje fall:</span><span class="sxs-lookup"><span data-stu-id="da7f1-181">In each case:</span></span>

* <span data-ttu-id="da7f1-182">Tjänsten har angetts med parametern 'targetResourceId' för `Add-AzureRmTrafficManagerEndpointConfig` eller `New-AzureRmTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="da7f1-182">The service is specified using the 'targetResourceId' parameter of `Add-AzureRmTrafficManagerEndpointConfig` or `New-AzureRmTrafficManagerEndpoint`.</span></span>
* <span data-ttu-id="da7f1-183">'Target' och 'EndpointLocation' underförstås av TargetResourceId.</span><span class="sxs-lookup"><span data-stu-id="da7f1-183">The 'Target' and 'EndpointLocation' are implied by the TargetResourceId.</span></span>
* <span data-ttu-id="da7f1-184">Ange 'vikten' är valfritt.</span><span class="sxs-lookup"><span data-stu-id="da7f1-184">Specifying the 'Weight' is optional.</span></span> <span data-ttu-id="da7f1-185">Vikten används bara om profilen är konfigurerad för att använda metoden 'Viktat' routning av nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="da7f1-185">Weights are only used if the profile is configured to use the 'Weighted' traffic-routing method.</span></span> <span data-ttu-id="da7f1-186">I annat fall ignoreras de.</span><span class="sxs-lookup"><span data-stu-id="da7f1-186">Otherwise, they are ignored.</span></span> <span data-ttu-id="da7f1-187">Om anges måste värdet vara ett tal mellan 1 och 1000.</span><span class="sxs-lookup"><span data-stu-id="da7f1-187">If specified, the value must be a number between 1 and 1000.</span></span> <span data-ttu-id="da7f1-188">Standardvärdet är '1'.</span><span class="sxs-lookup"><span data-stu-id="da7f1-188">The default value is '1'.</span></span>
* <span data-ttu-id="da7f1-189">Ange den 'prioriteten' är valfritt.</span><span class="sxs-lookup"><span data-stu-id="da7f1-189">Specifying the 'Priority' is optional.</span></span> <span data-ttu-id="da7f1-190">Prioriteringar används bara om profilen är konfigurerad för att använda metoden 'Priority' routning av nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="da7f1-190">Priorities are only used if the profile is configured to use the 'Priority' traffic-routing method.</span></span> <span data-ttu-id="da7f1-191">I annat fall ignoreras de.</span><span class="sxs-lookup"><span data-stu-id="da7f1-191">Otherwise, they are ignored.</span></span> <span data-ttu-id="da7f1-192">Giltiga värden är från 1 till 1000 med lägre värden som anger en högre prioritet.</span><span class="sxs-lookup"><span data-stu-id="da7f1-192">Valid values are from 1 to 1000 with lower values indicating a higher priority.</span></span> <span data-ttu-id="da7f1-193">Om för en slutpunkt, måste de anges för alla slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="da7f1-193">If specified for one endpoint, they must be specified for all endpoints.</span></span> <span data-ttu-id="da7f1-194">Om det utelämnas används standardvärden från '1' i ordningsföljden av slutpunkterna.</span><span class="sxs-lookup"><span data-stu-id="da7f1-194">If omitted, default values starting from '1' are applied in the order that the endpoints are listed.</span></span>

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a><span data-ttu-id="da7f1-195">Exempel 1: Lägga till slutpunkter för webbprogram med hjälp av`Add-AzureRmTrafficManagerEndpointConfig`</span><span class="sxs-lookup"><span data-stu-id="da7f1-195">Example 1: Adding Web App endpoints using `Add-AzureRmTrafficManagerEndpointConfig`</span></span>

<span data-ttu-id="da7f1-196">I det här exemplet vi skapa en Traffic Manager-profilen och lägga till två slutpunkter för webbprogram som använder den `Add-AzureRmTrafficManagerEndpointConfig` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="da7f1-196">In this example, we create a Traffic Manager profile and add two Web App endpoints using the `Add-AzureRmTrafficManagerEndpointConfig` cmdlet.</span></span>

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$webapp1 = Get-AzureRMWebApp -Name webapp1
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
$webapp2 = Get-AzureRMWebApp -Name webapp2
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```
### <a name="example-2-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a><span data-ttu-id="da7f1-197">Exempel 2: Lägga till en publicIpAddress-slutpunkten med`New-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="da7f1-197">Example 2: Adding a publicIpAddress endpoint using `New-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="da7f1-198">I det här exemplet läggs en offentlig IP-adressresurs till Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="da7f1-198">In this example, a public IP address resource is added to the Traffic Manager profile.</span></span> <span data-ttu-id="da7f1-199">Den offentliga IP-adressen måste ha ett DNS-namn som har konfigurerats och kan bindas till nätverkskort på en virtuell dator eller till en belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="da7f1-199">The public IP address must have a DNS name configured, and can be bound either to the NIC of a VM or to a load balancer.</span></span>

```powershell
$ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled
```

## <a name="adding-external-endpoints"></a><span data-ttu-id="da7f1-200">Lägga till externa slutpunkter</span><span class="sxs-lookup"><span data-stu-id="da7f1-200">Adding External Endpoints</span></span>

<span data-ttu-id="da7f1-201">Traffic Manager använder externa slutpunkter för att dirigera trafik till tjänster som ligger utanför Azure.</span><span class="sxs-lookup"><span data-stu-id="da7f1-201">Traffic Manager uses external endpoints to direct traffic to services hosted outside of Azure.</span></span> <span data-ttu-id="da7f1-202">Som med Azure-slutpunkter externa slutpunkter kan läggas till antingen med hjälp av `Add-AzureRmTrafficManagerEndpointConfig` följt av `Set-AzureRmTrafficManagerProfile`, eller `New-AzureRMTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="da7f1-202">As with Azure endpoints, external endpoints can be added either using `Add-AzureRmTrafficManagerEndpointConfig` followed by `Set-AzureRmTrafficManagerProfile`, or `New-AzureRMTrafficManagerEndpoint`.</span></span>

<span data-ttu-id="da7f1-203">När du anger externa slutpunkter:</span><span class="sxs-lookup"><span data-stu-id="da7f1-203">When specifying external endpoints:</span></span>

* <span data-ttu-id="da7f1-204">Slutpunkten domännamnet måste anges med parametern 'Target'</span><span class="sxs-lookup"><span data-stu-id="da7f1-204">The endpoint domain name must be specified using the 'Target' parameter</span></span>
* <span data-ttu-id="da7f1-205">Om metoden routning av nätverkstrafik, prestanda, används krävs EndpointLocation.</span><span class="sxs-lookup"><span data-stu-id="da7f1-205">If the 'Performance' traffic-routing method is used, the 'EndpointLocation' is required.</span></span> <span data-ttu-id="da7f1-206">Annars är valfria.</span><span class="sxs-lookup"><span data-stu-id="da7f1-206">Otherwise it is optional.</span></span> <span data-ttu-id="da7f1-207">Värdet måste vara en [giltig Azure regionnamn](https://azure.microsoft.com/regions/).</span><span class="sxs-lookup"><span data-stu-id="da7f1-207">The value must be a [valid Azure region name](https://azure.microsoft.com/regions/).</span></span>
* <span data-ttu-id="da7f1-208">'Vikt' och 'Priority' är valfria.</span><span class="sxs-lookup"><span data-stu-id="da7f1-208">The 'Weight' and 'Priority' are optional.</span></span>

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a><span data-ttu-id="da7f1-209">Exempel 1: Lägga till externa slutpunkter som använder `Add-AzureRmTrafficManagerEndpointConfig` och`Set-AzureRmTrafficManagerProfile`</span><span class="sxs-lookup"><span data-stu-id="da7f1-209">Example 1: Adding external endpoints using `Add-AzureRmTrafficManagerEndpointConfig` and `Set-AzureRmTrafficManagerProfile`</span></span>

<span data-ttu-id="da7f1-210">I det här exemplet vi skapa en Traffic Manager-profil, lägga till två externa slutpunkter och spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="da7f1-210">In this example, we create a Traffic Manager profile, add two external endpoints, and commit the changes.</span></span>

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointLocation "North Europe" -EndpointStatus Enabled
Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointLocation "Central US" -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a><span data-ttu-id="da7f1-211">Exempel 2: Lägga till externa slutpunkter med hjälp av`New-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="da7f1-211">Example 2: Adding external endpoints using `New-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="da7f1-212">I det här exemplet vi lägga till en extern slutpunkt i en befintlig profil.</span><span class="sxs-lookup"><span data-stu-id="da7f1-212">In this example, we add an external endpoint to an existing profile.</span></span> <span data-ttu-id="da7f1-213">Profilen har angetts med hjälp av gruppnamn profil och resurs.</span><span class="sxs-lookup"><span data-stu-id="da7f1-213">The profile is specified using the profile and resource group names.</span></span>

```powershell
New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
```

## <a name="adding-nested-endpoints"></a><span data-ttu-id="da7f1-214">Lägga till 'kapslade-slutpunkter</span><span class="sxs-lookup"><span data-stu-id="da7f1-214">Adding 'Nested' endpoints</span></span>

<span data-ttu-id="da7f1-215">Varje Traffic Manager-profil anger en enda metod för routning av nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="da7f1-215">Each Traffic Manager profile specifies a single traffic-routing method.</span></span> <span data-ttu-id="da7f1-216">Det finns emellertid scenarier som kräver mer avancerad routning av nätverkstrafik än routning tillhandahålls som en enskild Traffic Manager-profil.</span><span class="sxs-lookup"><span data-stu-id="da7f1-216">However, there are scenarios that require more sophisticated traffic routing than the routing provided by a single Traffic Manager profile.</span></span> <span data-ttu-id="da7f1-217">Du kan kapsla Traffic Manager-profiler för att kombinera fördelarna med fler än en metod för routning av nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="da7f1-217">You can nest Traffic Manager profiles to combine the benefits of more than one traffic-routing method.</span></span> <span data-ttu-id="da7f1-218">Kapslade profiler kan du åsidosätta standardbeteendet för Traffic Manager att stödja större och mer komplexa programdistributioner.</span><span class="sxs-lookup"><span data-stu-id="da7f1-218">Nested profiles allow you to override the default Traffic Manager behavior to support larger and more complex application deployments.</span></span> <span data-ttu-id="da7f1-219">Mer detaljerad exemplen finns [kapslade Traffic Manager-profiler](traffic-manager-nested-profiles.md).</span><span class="sxs-lookup"><span data-stu-id="da7f1-219">For more detailed examples, see [Nested Traffic Manager profiles](traffic-manager-nested-profiles.md).</span></span>

<span data-ttu-id="da7f1-220">Kapslade slutpunkter har konfigurerats på den överordnade-profil med hjälp av en viss slutpunkts-typ, 'NestedEndpoints'.</span><span class="sxs-lookup"><span data-stu-id="da7f1-220">Nested endpoints are configured at the parent profile, using a specific endpoint type, 'NestedEndpoints'.</span></span> <span data-ttu-id="da7f1-221">När du anger kapslade slutpunkter:</span><span class="sxs-lookup"><span data-stu-id="da7f1-221">When specifying nested endpoints:</span></span>

* <span data-ttu-id="da7f1-222">Slutpunkten måste anges med parametern 'targetResourceId'</span><span class="sxs-lookup"><span data-stu-id="da7f1-222">The endpoint must be specified using the 'targetResourceId' parameter</span></span>
* <span data-ttu-id="da7f1-223">Om metoden routning av nätverkstrafik, prestanda, används krävs EndpointLocation.</span><span class="sxs-lookup"><span data-stu-id="da7f1-223">If the 'Performance' traffic-routing method is used, the 'EndpointLocation' is required.</span></span> <span data-ttu-id="da7f1-224">Annars är valfria.</span><span class="sxs-lookup"><span data-stu-id="da7f1-224">Otherwise it is optional.</span></span> <span data-ttu-id="da7f1-225">Värdet måste vara en [giltig Azure regionnamn](http://azure.microsoft.com/regions/).</span><span class="sxs-lookup"><span data-stu-id="da7f1-225">The value must be a [valid Azure region name](http://azure.microsoft.com/regions/).</span></span>
* <span data-ttu-id="da7f1-226">'Vikt' och 'Priority' är valfria, som Azure-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="da7f1-226">The 'Weight' and 'Priority' are optional, as for Azure endpoints.</span></span>
* <span data-ttu-id="da7f1-227">'MinChildEndpoints'-parametern är valfri.</span><span class="sxs-lookup"><span data-stu-id="da7f1-227">The 'MinChildEndpoints' parameter is optional.</span></span> <span data-ttu-id="da7f1-228">Standardvärdet är '1'.</span><span class="sxs-lookup"><span data-stu-id="da7f1-228">The default value is '1'.</span></span> <span data-ttu-id="da7f1-229">Om antalet tillgängliga slutpunkter understiger tröskelvärdet, anses överordnade profilen underordnade profilen 'försämrad' och diverts trafik till andra slutpunkter i profilen för överordnade.</span><span class="sxs-lookup"><span data-stu-id="da7f1-229">If the number of available endpoints falls below this threshold, the parent profile considers the child profile 'degraded' and diverts traffic to the other endpoints in the parent profile.</span></span>

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a><span data-ttu-id="da7f1-230">Exempel 1: Lägga till kapslade slutpunkter som använder `Add-AzureRmTrafficManagerEndpointConfig` och`Set-AzureRmTrafficManagerProfile`</span><span class="sxs-lookup"><span data-stu-id="da7f1-230">Example 1: Adding nested endpoints using `Add-AzureRmTrafficManagerEndpointConfig` and `Set-AzureRmTrafficManagerProfile`</span></span>

<span data-ttu-id="da7f1-231">I det här exemplet vi skapa ny Traffic Manager underordnade och överordnade profiler, lägga till underordnad som en kapslad slutpunkt till överordnat och spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="da7f1-231">In this example, we create new Traffic Manager child and parent profiles, add the child as a nested endpoint to the parent, and commit the changes.</span></span>

```powershell
$child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

<span data-ttu-id="da7f1-232">Planeringsaspekter i det här exemplet vi inte till andra slutpunkter i över- eller underordnad-profiler.</span><span class="sxs-lookup"><span data-stu-id="da7f1-232">For brevity in this example, we did not add any other endpoints to the child or parent profiles.</span></span>

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a><span data-ttu-id="da7f1-233">Exempel 2: Lägga till kapslade slutpunkter med hjälp av`New-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="da7f1-233">Example 2: Adding nested endpoints using `New-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="da7f1-234">I det här exemplet vi lägga till en befintlig profil för underordnade som en kapslad slutpunkt i en befintlig överordnad profil.</span><span class="sxs-lookup"><span data-stu-id="da7f1-234">In this example, we add an existing child profile as a nested endpoint to an existing parent profile.</span></span> <span data-ttu-id="da7f1-235">Profilen har angetts med hjälp av gruppnamn profil och resurs.</span><span class="sxs-lookup"><span data-stu-id="da7f1-235">The profile is specified using the profile and resource group names.</span></span>

```powershell
$child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
```

## <a name="update-a-traffic-manager-endpoint"></a><span data-ttu-id="da7f1-236">Uppdatera en Traffic Manager-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="da7f1-236">Update a Traffic Manager Endpoint</span></span>

<span data-ttu-id="da7f1-237">Det finns två sätt att uppdatera en befintlig Traffic Manager-slutpunkt:</span><span class="sxs-lookup"><span data-stu-id="da7f1-237">There are two ways to update an existing Traffic Manager endpoint:</span></span>

1. <span data-ttu-id="da7f1-238">Hämta Traffic Manager-profil med `Get-AzureRmTrafficManagerProfile`, uppdatera egenskaperna för slutpunkten inom profilen och genomför ändringarna med `Set-AzureRmTrafficManagerProfile`.</span><span class="sxs-lookup"><span data-stu-id="da7f1-238">Get the Traffic Manager profile using `Get-AzureRmTrafficManagerProfile`, update the endpoint properties within the profile, and commit the changes using `Set-AzureRmTrafficManagerProfile`.</span></span> <span data-ttu-id="da7f1-239">Den här metoden har fördelen att kunna uppdatera mer än en slutpunkt i en enda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="da7f1-239">This method has the advantage of being able to update more than one endpoint in a single operation.</span></span>
2. <span data-ttu-id="da7f1-240">Hämta Traffic Manager-slutpunkten med `Get-AzureRmTrafficManagerEndpoint`, uppdaterar slutpunktsegenskaperna och genomför ändringarna med `Set-AzureRmTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="da7f1-240">Get the Traffic Manager endpoint using `Get-AzureRmTrafficManagerEndpoint`, update the endpoint properties, and commit the changes using `Set-AzureRmTrafficManagerEndpoint`.</span></span> <span data-ttu-id="da7f1-241">Den här metoden är enklare, eftersom det inte kräver att indexera i matrisen slutpunkter i profilen.</span><span class="sxs-lookup"><span data-stu-id="da7f1-241">This method is simpler, since it does not require indexing into the Endpoints array in the profile.</span></span>

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a><span data-ttu-id="da7f1-242">Exempel 1: Uppdatera slutpunkter med hjälp av `Get-AzureRmTrafficManagerProfile` och`Set-AzureRmTrafficManagerProfile`</span><span class="sxs-lookup"><span data-stu-id="da7f1-242">Example 1: Updating endpoints using `Get-AzureRmTrafficManagerProfile` and `Set-AzureRmTrafficManagerProfile`</span></span>

<span data-ttu-id="da7f1-243">I det här exemplet ändrar vi prioritet på två slutpunkter i en befintlig profil.</span><span class="sxs-lookup"><span data-stu-id="da7f1-243">In this example, we modify the priority on two endpoints within an existing profile.</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
$profile.Endpoints[0].Priority = 2
$profile.Endpoints[1].Priority = 1
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a><span data-ttu-id="da7f1-244">Exempel 2: Uppdatera slutpunkten med `Get-AzureRmTrafficManagerEndpoint` och`Set-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="da7f1-244">Example 2: Updating an endpoint using `Get-AzureRmTrafficManagerEndpoint` and `Set-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="da7f1-245">I det här exemplet ändrar vi vikten för en enda slutpunkt i en befintlig profil.</span><span class="sxs-lookup"><span data-stu-id="da7f1-245">In this example, we modify the weight of a single endpoint in an existing profile.</span></span>

```powershell
$endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
$endpoint.Weight = 20
Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint
```

## <a name="enabling-and-disabling-endpoints-and-profiles"></a><span data-ttu-id="da7f1-246">Aktiverar och inaktiverar slutpunkter och profiler</span><span class="sxs-lookup"><span data-stu-id="da7f1-246">Enabling and Disabling Endpoints and Profiles</span></span>

<span data-ttu-id="da7f1-247">Traffic Manager kan enskilda slutpunkter aktiverade och inaktiverade, samt att tillåta aktivering och inaktivering av hela profiler.</span><span class="sxs-lookup"><span data-stu-id="da7f1-247">Traffic Manager allows individual endpoints to be enabled and disabled, as well as allowing enabling and disabling of entire profiles.</span></span>
<span data-ttu-id="da7f1-248">Dessa ändringar kan göras genom att hämta/uppdatera/inställningen endpoint eller profil resurser.</span><span class="sxs-lookup"><span data-stu-id="da7f1-248">These changes can be made by getting/updating/setting the endpoint or profile resources.</span></span> <span data-ttu-id="da7f1-249">Om du vill förenkla dessa vanliga åtgärder stöds de också via dedikerade cmdlets.</span><span class="sxs-lookup"><span data-stu-id="da7f1-249">To streamline these common operations, they are also supported via dedicated cmdlets.</span></span>

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a><span data-ttu-id="da7f1-250">Exempel 1: Aktivera och inaktivera Traffic Manager-profilen</span><span class="sxs-lookup"><span data-stu-id="da7f1-250">Example 1: Enabling and disabling a Traffic Manager profile</span></span>

<span data-ttu-id="da7f1-251">Aktivera en Traffic Manager-profil med `Enable-AzureRmTrafficManagerProfile`.</span><span class="sxs-lookup"><span data-stu-id="da7f1-251">To enable a Traffic Manager profile, use `Enable-AzureRmTrafficManagerProfile`.</span></span> <span data-ttu-id="da7f1-252">Profilen kan anges med en profilobjektet.</span><span class="sxs-lookup"><span data-stu-id="da7f1-252">The profile can be specified using a profile object.</span></span> <span data-ttu-id="da7f1-253">Profil-objekt kan skickas via pipeline eller genom att använda den '-TrafficManagerProfile-parametern.</span><span class="sxs-lookup"><span data-stu-id="da7f1-253">The profile object can be passed via the pipeline or by using the '-TrafficManagerProfile' parameter.</span></span> <span data-ttu-id="da7f1-254">I det här exemplet anger vi profil med namnet på profilen och resurs.</span><span class="sxs-lookup"><span data-stu-id="da7f1-254">In this example, we specify the profile by the profile and resource group name.</span></span>

```powershell
Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

<span data-ttu-id="da7f1-255">Inaktivera Traffic Manager-profilen:</span><span class="sxs-lookup"><span data-stu-id="da7f1-255">To disable a Traffic Manager profile:</span></span>

```powershell
Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

<span data-ttu-id="da7f1-256">Inaktivera AzureRmTrafficManagerProfile cmdlet uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="da7f1-256">The Disable-AzureRmTrafficManagerProfile cmdlet prompts for confirmation.</span></span> <span data-ttu-id="da7f1-257">Den här uppmaningen kan undertryckas med hjälp av den '-Force-parametern.</span><span class="sxs-lookup"><span data-stu-id="da7f1-257">This prompt can be suppressed using the '-Force' parameter.</span></span>

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a><span data-ttu-id="da7f1-258">Exempel 2: Aktivera och inaktivera Traffic Manager-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="da7f1-258">Example 2: Enabling and disabling a Traffic Manager endpoint</span></span>

<span data-ttu-id="da7f1-259">Aktivera en Traffic Manager-slutpunkt med `Enable-AzureRmTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="da7f1-259">To enable a Traffic Manager endpoint, use `Enable-AzureRmTrafficManagerEndpoint`.</span></span> <span data-ttu-id="da7f1-260">Det finns två sätt att ange slutpunkten</span><span class="sxs-lookup"><span data-stu-id="da7f1-260">There are two ways to specify the endpoint</span></span>

1. <span data-ttu-id="da7f1-261">Med hjälp av en TrafficManagerEndpoint-objektet som överförs via pipeline eller den '-TrafficManagerEndpoint-parameter</span><span class="sxs-lookup"><span data-stu-id="da7f1-261">Using a TrafficManagerEndpoint object passed via the pipeline or using the '-TrafficManagerEndpoint' parameter</span></span>
2. <span data-ttu-id="da7f1-262">Med hjälp av namnet på slutpunkten, typen av slutpunkt, profilnamn och resursgruppens namn:</span><span class="sxs-lookup"><span data-stu-id="da7f1-262">Using the endpoint name, endpoint type, profile name, and resource group name:</span></span>

```powershell
Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

<span data-ttu-id="da7f1-263">På liknande sätt kan du inaktivera Traffic Manager-slutpunkten:</span><span class="sxs-lookup"><span data-stu-id="da7f1-263">Similarly, to disable a Traffic Manager endpoint:</span></span>

```powershell
Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

<span data-ttu-id="da7f1-264">Precis som med `Disable-AzureRmTrafficManagerProfile`, `Disable-AzureRmTrafficManagerEndpoint` cmdlet uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="da7f1-264">As with `Disable-AzureRmTrafficManagerProfile`, the `Disable-AzureRmTrafficManagerEndpoint` cmdlet prompts for confirmation.</span></span> <span data-ttu-id="da7f1-265">Den här uppmaningen kan undertryckas med hjälp av den '-Force-parametern.</span><span class="sxs-lookup"><span data-stu-id="da7f1-265">This prompt can be suppressed using the '-Force' parameter.</span></span>

## <a name="delete-a-traffic-manager-endpoint"></a><span data-ttu-id="da7f1-266">Ta bort en Traffic Manager-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="da7f1-266">Delete a Traffic Manager Endpoint</span></span>

<span data-ttu-id="da7f1-267">Ta bort enskilda slutpunkter med den `Remove-AzureRmTrafficManagerEndpoint` cmdlet:</span><span class="sxs-lookup"><span data-stu-id="da7f1-267">To remove individual endpoints, use the `Remove-AzureRmTrafficManagerEndpoint` cmdlet:</span></span>

```powershell
Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

<span data-ttu-id="da7f1-268">Denna cmdlet uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="da7f1-268">This cmdlet prompts for confirmation.</span></span> <span data-ttu-id="da7f1-269">Den här uppmaningen kan undertryckas med hjälp av den '-Force-parametern.</span><span class="sxs-lookup"><span data-stu-id="da7f1-269">This prompt can be suppressed using the '-Force' parameter.</span></span>

## <a name="delete-a-traffic-manager-profile"></a><span data-ttu-id="da7f1-270">Ta bort en Traffic Manager-profil</span><span class="sxs-lookup"><span data-stu-id="da7f1-270">Delete a Traffic Manager Profile</span></span>

<span data-ttu-id="da7f1-271">Ta bort en Traffic Manager-profilen genom att använda den `Remove-AzureRmTrafficManagerProfile` cmdlet, anger gruppnamn profil och resursen:</span><span class="sxs-lookup"><span data-stu-id="da7f1-271">To delete a Traffic Manager profile, use the `Remove-AzureRmTrafficManagerProfile` cmdlet, specifying the profile and resource group names:</span></span>

```powershell
Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

<span data-ttu-id="da7f1-272">Denna cmdlet uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="da7f1-272">This cmdlet prompts for confirmation.</span></span> <span data-ttu-id="da7f1-273">Den här uppmaningen kan undertryckas med hjälp av den '-Force-parametern.</span><span class="sxs-lookup"><span data-stu-id="da7f1-273">This prompt can be suppressed using the '-Force' parameter.</span></span>

<span data-ttu-id="da7f1-274">Också du kan ange profilen som ska tas bort med hjälp av en profil-objekt:</span><span class="sxs-lookup"><span data-stu-id="da7f1-274">The profile to be deleted can also be specified using a profile object:</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

<span data-ttu-id="da7f1-275">Den här sekvensen kan också skickas:</span><span class="sxs-lookup"><span data-stu-id="da7f1-275">This sequence can also be piped:</span></span>

```powershell
Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a><span data-ttu-id="da7f1-276">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="da7f1-276">Next steps</span></span>

[<span data-ttu-id="da7f1-277">Traffic Manager-övervakning</span><span class="sxs-lookup"><span data-stu-id="da7f1-277">Traffic Manager monitoring</span></span>](traffic-manager-monitoring.md)

[<span data-ttu-id="da7f1-278">Prestandaöverväganden för Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="da7f1-278">Traffic Manager performance considerations</span></span>](traffic-manager-performance-considerations.md)
