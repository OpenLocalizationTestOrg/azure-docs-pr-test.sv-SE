---
title: aaaUsing PowerShell toomanage Traffic Manager i Azure | Microsoft Docs
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
ms.openlocfilehash: 018c37db63beb82fdad54cfd7e13ab3cb645723a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-powershell-toomanage-traffic-manager"></a><span data-ttu-id="edd0f-103">Med hjälp av PowerShell toomanage Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="edd0f-103">Using PowerShell toomanage Traffic Manager</span></span>

<span data-ttu-id="edd0f-104">Azure Resource Manager är hello prioriterade hanteringsgränssnitt för tjänster i Azure.</span><span class="sxs-lookup"><span data-stu-id="edd0f-104">Azure Resource Manager is hello preferred management interface for services in Azure.</span></span> <span data-ttu-id="edd0f-105">Azure Traffic Manager-profiler kan hanteras med Azure Resource Manager-baserade API: er och verktyg.</span><span class="sxs-lookup"><span data-stu-id="edd0f-105">Azure Traffic Manager profiles can be managed using Azure Resource Manager-based APIs and tools.</span></span>

## <a name="resource-model"></a><span data-ttu-id="edd0f-106">Resursmodell</span><span class="sxs-lookup"><span data-stu-id="edd0f-106">Resource model</span></span>

<span data-ttu-id="edd0f-107">Azure Traffic Manager konfigureras med hjälp av en samling inställningar som kallas en Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="edd0f-107">Azure Traffic Manager is configured using a collection of settings called a Traffic Manager profile.</span></span> <span data-ttu-id="edd0f-108">Den här profilen innehåller DNS-inställningar, trafikdirigeringsinställningar, slutpunkt övervakningsinställningarna och en lista över tjänstens slutpunkter toowhich trafik dirigeras.</span><span class="sxs-lookup"><span data-stu-id="edd0f-108">This profile contains DNS settings, traffic routing settings, endpoint monitoring settings, and a list of service endpoints toowhich traffic is routed.</span></span>

<span data-ttu-id="edd0f-109">Varje trafikhanterarprofil representeras av en resurs av typen 'TrafficManagerProfiles'.</span><span class="sxs-lookup"><span data-stu-id="edd0f-109">Each Traffic Manager profile is represented by a resource of type 'TrafficManagerProfiles'.</span></span> <span data-ttu-id="edd0f-110">Vid hello REST API-nivå är hello URI för varje profil:</span><span class="sxs-lookup"><span data-stu-id="edd0f-110">At hello REST API level, hello URI for each profile is as follows:</span></span>

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="setting-up-azure-powershell"></a><span data-ttu-id="edd0f-111">Konfigurera Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="edd0f-111">Setting up Azure PowerShell</span></span>

<span data-ttu-id="edd0f-112">Dessa anvisningar använder Microsoft Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="edd0f-112">These instructions use Microsoft Azure PowerShell.</span></span> <span data-ttu-id="edd0f-113">hello följande artikeln förklaras hur tooinstall och konfigurera Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="edd0f-113">hello following article explains how tooinstall and configure Azure PowerShell.</span></span>

* [<span data-ttu-id="edd0f-114">Hur tooinstall och konfigurera Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="edd0f-114">How tooinstall and configure Azure PowerShell</span></span>](/powershell/azure/overview)

<span data-ttu-id="edd0f-115">hello exemplen i den här artikeln förutsätter att du har en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="edd0f-115">hello examples in this article assume that you have an existing resource group.</span></span> <span data-ttu-id="edd0f-116">Du kan skapa en resursgrupp med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="edd0f-116">You can create a resource group using hello following command:</span></span>

```powershell
New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

> [!NOTE]
> <span data-ttu-id="edd0f-117">Azure Resource Manager kräver att alla resursgrupper har en plats.</span><span class="sxs-lookup"><span data-stu-id="edd0f-117">Azure Resource Manager requires that all resource groups have a location.</span></span> <span data-ttu-id="edd0f-118">Den här platsen används som standard hello för resurser som skapats i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="edd0f-118">This location is used as hello default for resources created in that resource group.</span></span> <span data-ttu-id="edd0f-119">Men eftersom resurser för Traffic Manager-profil är globala, inte regional är påverkar hello valet av resursgruppens plats inte på Azure Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="edd0f-119">However, since Traffic Manager profile resources are global, not regional, hello choice of resource group location has no impact on Azure Traffic Manager.</span></span>

## <a name="create-a-traffic-manager-profile"></a><span data-ttu-id="edd0f-120">Skapa en Trafikhanterarprofil</span><span class="sxs-lookup"><span data-stu-id="edd0f-120">Create a Traffic Manager Profile</span></span>

<span data-ttu-id="edd0f-121">toocreate Traffic Manager-profilen använder hello `New-AzureRmTrafficManagerProfile` cmdlet:</span><span class="sxs-lookup"><span data-stu-id="edd0f-121">toocreate a Traffic Manager profile, use hello `New-AzureRmTrafficManagerProfile` cmdlet:</span></span>

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

<span data-ttu-id="edd0f-122">hello i den följande tabellen beskrivs hello parametrar:</span><span class="sxs-lookup"><span data-stu-id="edd0f-122">hello following table describes hello parameters:</span></span>

| <span data-ttu-id="edd0f-123">Parameter</span><span class="sxs-lookup"><span data-stu-id="edd0f-123">Parameter</span></span> | <span data-ttu-id="edd0f-124">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="edd0f-124">Description</span></span> |
| --- | --- |
| <span data-ttu-id="edd0f-125">Namn</span><span class="sxs-lookup"><span data-stu-id="edd0f-125">Name</span></span> |<span data-ttu-id="edd0f-126">hello-resursnamnet för hello Traffic Manager-profilen för resursen.</span><span class="sxs-lookup"><span data-stu-id="edd0f-126">hello resource name for hello Traffic Manager profile resource.</span></span> <span data-ttu-id="edd0f-127">Profiler i hello samma resursgrupp måste ha unika namn.</span><span class="sxs-lookup"><span data-stu-id="edd0f-127">Profiles in hello same resource group must have unique names.</span></span> <span data-ttu-id="edd0f-128">Det här namnet skiljer sig från hello DNS-namnet för DNS-frågor.</span><span class="sxs-lookup"><span data-stu-id="edd0f-128">This name is separate from hello DNS name used for DNS queries.</span></span> |
| <span data-ttu-id="edd0f-129">resourceGroupName</span><span class="sxs-lookup"><span data-stu-id="edd0f-129">ResourceGroupName</span></span> |<span data-ttu-id="edd0f-130">hello namn på hello resurs grupp som innehåller hello profil resurs.</span><span class="sxs-lookup"><span data-stu-id="edd0f-130">hello name of hello resource group containing hello profile resource.</span></span> |
| <span data-ttu-id="edd0f-131">TrafficRoutingMethod</span><span class="sxs-lookup"><span data-stu-id="edd0f-131">TrafficRoutingMethod</span></span> |<span data-ttu-id="edd0f-132">Anger hello routning av nätverkstrafik metod som används för toodetermine vilken slutpunkt returneras svar en DNS-fråga.</span><span class="sxs-lookup"><span data-stu-id="edd0f-132">Specifies hello traffic-routing method used toodetermine which endpoint is returned in response a DNS query.</span></span> <span data-ttu-id="edd0f-133">Möjliga värden är 'Prestanda', 'Viktat' eller 'Priority'.</span><span class="sxs-lookup"><span data-stu-id="edd0f-133">Possible values are 'Performance', 'Weighted' or 'Priority'.</span></span> |
| <span data-ttu-id="edd0f-134">RelativeDnsName</span><span class="sxs-lookup"><span data-stu-id="edd0f-134">RelativeDnsName</span></span> |<span data-ttu-id="edd0f-135">Anger hello hostname-delen av hello DNS-namn som tillhandahålls av den här trafikhanterarprofilen.</span><span class="sxs-lookup"><span data-stu-id="edd0f-135">Specifies hello hostname portion of hello DNS name provided by this Traffic Manager profile.</span></span> <span data-ttu-id="edd0f-136">Det här värdet kombineras med hello DNS-domännamn som används av Azure Traffic Manager tooform hello fullständigt kvalificerade domännamnet (FQDN) för hello profil.</span><span class="sxs-lookup"><span data-stu-id="edd0f-136">This value is combined with hello DNS domain name used by Azure Traffic Manager tooform hello fully qualified domain name (FQDN) of hello profile.</span></span> <span data-ttu-id="edd0f-137">Till exempel blir hello värdet ”Contoso” contoso.trafficmanager.net.</span><span class="sxs-lookup"><span data-stu-id="edd0f-137">For example, setting hello value of 'contoso' becomes 'contoso.trafficmanager.net.'</span></span> |
| <span data-ttu-id="edd0f-138">TTL-VÄRDE</span><span class="sxs-lookup"><span data-stu-id="edd0f-138">TTL</span></span> |<span data-ttu-id="edd0f-139">Anger hello DNS-Time-to-Live (TTL), i sekunder.</span><span class="sxs-lookup"><span data-stu-id="edd0f-139">Specifies hello DNS Time-to-Live (TTL), in seconds.</span></span> <span data-ttu-id="edd0f-140">Den här TTL informerar hello lokala DNS-matchare och DNS-klienter hur länge toocache DNS-svar för den här Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="edd0f-140">This TTL informs hello Local DNS resolvers and DNS clients how long toocache DNS responses for this Traffic Manager profile.</span></span> |
| <span data-ttu-id="edd0f-141">MonitorProtocol</span><span class="sxs-lookup"><span data-stu-id="edd0f-141">MonitorProtocol</span></span> |<span data-ttu-id="edd0f-142">Anger hello protokollet toouse toomonitor endpoint hälsa.</span><span class="sxs-lookup"><span data-stu-id="edd0f-142">Specifies hello protocol toouse toomonitor endpoint health.</span></span> <span data-ttu-id="edd0f-143">Möjliga värden är 'HTTP' och 'HTTPS'.</span><span class="sxs-lookup"><span data-stu-id="edd0f-143">Possible values are 'HTTP' and 'HTTPS'.</span></span> |
| <span data-ttu-id="edd0f-144">MonitorPort</span><span class="sxs-lookup"><span data-stu-id="edd0f-144">MonitorPort</span></span> |<span data-ttu-id="edd0f-145">Anger hello TCP-port används toomonitor endpoint hälsa.</span><span class="sxs-lookup"><span data-stu-id="edd0f-145">Specifies hello TCP port used toomonitor endpoint health.</span></span> |
| <span data-ttu-id="edd0f-146">MonitorPath</span><span class="sxs-lookup"><span data-stu-id="edd0f-146">MonitorPath</span></span> |<span data-ttu-id="edd0f-147">Anger domännamnet för hello sökväg relativa toohello endpoint tooprobe för slutpunkten health.</span><span class="sxs-lookup"><span data-stu-id="edd0f-147">Specifies hello path relative toohello endpoint domain name used tooprobe for endpoint health.</span></span> |

<span data-ttu-id="edd0f-148">hello cmdlet skapar en Traffic Manager-profilen i Azure och returnerar en motsvarande profil objektet tooPowerShell.</span><span class="sxs-lookup"><span data-stu-id="edd0f-148">hello cmdlet creates a Traffic Manager profile in Azure and returns a corresponding profile object tooPowerShell.</span></span> <span data-ttu-id="edd0f-149">Nu innehåller hello profil inte några slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="edd0f-149">At this point, hello profile does not contain any endpoints.</span></span> <span data-ttu-id="edd0f-150">Mer information om att lägga till slutpunkter tooa Traffic Manager-profilen finns [att lägga till slutpunkter för Traffic Manager](#adding-traffic-manager-endpoints).</span><span class="sxs-lookup"><span data-stu-id="edd0f-150">For more information about adding endpoints tooa Traffic Manager profile, see [Adding Traffic Manager Endpoints](#adding-traffic-manager-endpoints).</span></span>

## <a name="get-a-traffic-manager-profile"></a><span data-ttu-id="edd0f-151">Hämta en Trafikhanterarprofil</span><span class="sxs-lookup"><span data-stu-id="edd0f-151">Get a Traffic Manager Profile</span></span>

<span data-ttu-id="edd0f-152">tooretrieve en befintlig Traffic Manager-profilobjektet använder hello `Get-AzureRmTrafficManagerProfle` cmdlet:</span><span class="sxs-lookup"><span data-stu-id="edd0f-152">tooretrieve an existing Traffic Manager profile object, use hello `Get-AzureRmTrafficManagerProfle` cmdlet:</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

<span data-ttu-id="edd0f-153">Denna cmdlet returnerar ett objekt för Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="edd0f-153">This cmdlet returns a Traffic Manager profile object.</span></span>

## <a name="update-a-traffic-manager-profile"></a><span data-ttu-id="edd0f-154">Uppdatera en Traffic Manager-profil</span><span class="sxs-lookup"><span data-stu-id="edd0f-154">Update a Traffic Manager Profile</span></span>

<span data-ttu-id="edd0f-155">Ändra Traffic Manager-profiler följer en process i steg 3:</span><span class="sxs-lookup"><span data-stu-id="edd0f-155">Modifying Traffic Manager profiles follows a 3-step process:</span></span>

1. <span data-ttu-id="edd0f-156">Hämta hello profil med `Get-AzureRmTrafficManagerProfile` eller Använd hello-profil som returneras av `New-AzureRmTrafficManagerProfile`.</span><span class="sxs-lookup"><span data-stu-id="edd0f-156">Retrieve hello profile using `Get-AzureRmTrafficManagerProfile` or use hello profile returned by `New-AzureRmTrafficManagerProfile`.</span></span>
2. <span data-ttu-id="edd0f-157">Ändra hello profil.</span><span class="sxs-lookup"><span data-stu-id="edd0f-157">Modify hello profile.</span></span> <span data-ttu-id="edd0f-158">Du kan lägga till och ta bort slutpunkter eller ändra slutpunkten eller profil parametrar.</span><span class="sxs-lookup"><span data-stu-id="edd0f-158">You can add and remove endpoints or change endpoint or profile parameters.</span></span> <span data-ttu-id="edd0f-159">Dessa ändringar är offline åtgärder.</span><span class="sxs-lookup"><span data-stu-id="edd0f-159">These changes are off-line operations.</span></span> <span data-ttu-id="edd0f-160">Endast ändrar hello lokalt objekt i minnet som representerar hello profil.</span><span class="sxs-lookup"><span data-stu-id="edd0f-160">You are only changing hello local object in memory that represents hello profile.</span></span>
3. <span data-ttu-id="edd0f-161">Genomför ändringarna med hjälp av hello `Set-AzureRmTrafficManagerProfile` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="edd0f-161">Commit your changes using hello `Set-AzureRmTrafficManagerProfile` cmdlet.</span></span>

<span data-ttu-id="edd0f-162">Alla profilegenskaper kan ändras utom hello profil RelativeDnsName.</span><span class="sxs-lookup"><span data-stu-id="edd0f-162">All profile properties can be changed except hello profile's RelativeDnsName.</span></span> <span data-ttu-id="edd0f-163">toochange Hej RelativeDnsName, måste du ta bort profil och en ny profil med ett nytt namn.</span><span class="sxs-lookup"><span data-stu-id="edd0f-163">toochange hello RelativeDnsName, you must delete profile and a new profile with a new name.</span></span>

<span data-ttu-id="edd0f-164">hello som följande exempel visar hur toochange hello TTL-profilen:</span><span class="sxs-lookup"><span data-stu-id="edd0f-164">hello following example demonstrates how toochange hello profile's TTL:</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
$profile.Ttl = 300
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

<span data-ttu-id="edd0f-165">Det finns tre typer av slutpunkter för Traffic Manager:</span><span class="sxs-lookup"><span data-stu-id="edd0f-165">There are three types of Traffic Manager endpoints:</span></span>

1. <span data-ttu-id="edd0f-166">**Azure-slutpunkter** är tjänster som finns på Azure</span><span class="sxs-lookup"><span data-stu-id="edd0f-166">**Azure endpoints** are services hosted in Azure</span></span>
2. <span data-ttu-id="edd0f-167">**Externa slutpunkter** är tjänster som ligger utanför Azure</span><span class="sxs-lookup"><span data-stu-id="edd0f-167">**External endpoints** are services hosted outside of Azure</span></span>
3. <span data-ttu-id="edd0f-168">**Kapslade slutpunkter** är används tooconstruct kapslade hierarkier av Traffic Manager-profiler.</span><span class="sxs-lookup"><span data-stu-id="edd0f-168">**Nested endpoints** are used tooconstruct nested hierarchies of Traffic Manager profiles.</span></span> <span data-ttu-id="edd0f-169">Kapslade slutpunkter aktivera avancerade routning av nätverkstrafik konfigurationer för komplexa program.</span><span class="sxs-lookup"><span data-stu-id="edd0f-169">Nested endpoints enable advanced traffic-routing configurations for complex applications.</span></span>

<span data-ttu-id="edd0f-170">I samtliga tre fall kan du lägga till slutpunkter på två sätt:</span><span class="sxs-lookup"><span data-stu-id="edd0f-170">In all three cases, endpoints can be added in two ways:</span></span>

1. <span data-ttu-id="edd0f-171">Med hjälp av en process för 3 steg som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="edd0f-171">Using a 3-step process described previously.</span></span> <span data-ttu-id="edd0f-172">hello fördelen med den här metoden är att flera endpoint kan ändras i en enda uppdatering.</span><span class="sxs-lookup"><span data-stu-id="edd0f-172">hello advantage of this method is that several endpoint changes can be made in a single update.</span></span>
2. <span data-ttu-id="edd0f-173">Använda hello ny AzureRmTrafficManagerEndpoint cmdlet.</span><span class="sxs-lookup"><span data-stu-id="edd0f-173">Using hello New-AzureRmTrafficManagerEndpoint cmdlet.</span></span> <span data-ttu-id="edd0f-174">Denna cmdlet lägger till en slutpunkt tooan befintlig Traffic Manager-profil i en enda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="edd0f-174">This cmdlet adds an endpoint tooan existing Traffic Manager profile in a single operation.</span></span>

## <a name="adding-azure-endpoints"></a><span data-ttu-id="edd0f-175">Lägga till Azure-slutpunkter</span><span class="sxs-lookup"><span data-stu-id="edd0f-175">Adding Azure Endpoints</span></span>

<span data-ttu-id="edd0f-176">Azure-slutpunkter referera till tjänster i Azure.</span><span class="sxs-lookup"><span data-stu-id="edd0f-176">Azure endpoints reference services hosted in Azure.</span></span> <span data-ttu-id="edd0f-177">Två typer av Azure-slutpunkter stöds:</span><span class="sxs-lookup"><span data-stu-id="edd0f-177">Two types of Azure endpoints are supported:</span></span>

1. <span data-ttu-id="edd0f-178">Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="edd0f-178">Azure Web Apps</span></span>
2. <span data-ttu-id="edd0f-179">Azure PublicIpAddress-resurser (som kan vara anslutna tooa belastningsutjämnare eller en virtuell dator NIC).</span><span class="sxs-lookup"><span data-stu-id="edd0f-179">Azure PublicIpAddress resources (which can be attached tooa load-balancer or a virtual machine NIC).</span></span> <span data-ttu-id="edd0f-180">Hej PublicIpAddress måste ha ett DNS-namn som tilldelats toobe som används i Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="edd0f-180">hello PublicIpAddress must have a DNS name assigned toobe used in Traffic Manager.</span></span>

<span data-ttu-id="edd0f-181">I varje fall:</span><span class="sxs-lookup"><span data-stu-id="edd0f-181">In each case:</span></span>

* <span data-ttu-id="edd0f-182">hello-tjänsten har angetts med hello 'targetResourceId-parametern för `Add-AzureRmTrafficManagerEndpointConfig` eller `New-AzureRmTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="edd0f-182">hello service is specified using hello 'targetResourceId' parameter of `Add-AzureRmTrafficManagerEndpointConfig` or `New-AzureRmTrafficManagerEndpoint`.</span></span>
* <span data-ttu-id="edd0f-183">hello är 'Target' och 'EndpointLocation' underförstås av hello TargetResourceId.</span><span class="sxs-lookup"><span data-stu-id="edd0f-183">hello 'Target' and 'EndpointLocation' are implied by hello TargetResourceId.</span></span>
* <span data-ttu-id="edd0f-184">Att ange hello är 'Vikt' valfritt.</span><span class="sxs-lookup"><span data-stu-id="edd0f-184">Specifying hello 'Weight' is optional.</span></span> <span data-ttu-id="edd0f-185">Vikten används bara om hello profil är konfigurerade toouse hello 'Viktat' routning av nätverkstrafik metod.</span><span class="sxs-lookup"><span data-stu-id="edd0f-185">Weights are only used if hello profile is configured toouse hello 'Weighted' traffic-routing method.</span></span> <span data-ttu-id="edd0f-186">I annat fall ignoreras de.</span><span class="sxs-lookup"><span data-stu-id="edd0f-186">Otherwise, they are ignored.</span></span> <span data-ttu-id="edd0f-187">Om anges måste hello-värdet vara ett tal mellan 1 och 1000.</span><span class="sxs-lookup"><span data-stu-id="edd0f-187">If specified, hello value must be a number between 1 and 1000.</span></span> <span data-ttu-id="edd0f-188">hello standardvärdet är '1'.</span><span class="sxs-lookup"><span data-stu-id="edd0f-188">hello default value is '1'.</span></span>
* <span data-ttu-id="edd0f-189">Att ange hello 'Priority' är valfritt.</span><span class="sxs-lookup"><span data-stu-id="edd0f-189">Specifying hello 'Priority' is optional.</span></span> <span data-ttu-id="edd0f-190">Prioriteringar används bara om hello profil är konfigurerade toouse hello 'Priority' routning av nätverkstrafik metod.</span><span class="sxs-lookup"><span data-stu-id="edd0f-190">Priorities are only used if hello profile is configured toouse hello 'Priority' traffic-routing method.</span></span> <span data-ttu-id="edd0f-191">I annat fall ignoreras de.</span><span class="sxs-lookup"><span data-stu-id="edd0f-191">Otherwise, they are ignored.</span></span> <span data-ttu-id="edd0f-192">Giltiga värden är från 1 too1000 med lägre värden som anger en högre prioritet.</span><span class="sxs-lookup"><span data-stu-id="edd0f-192">Valid values are from 1 too1000 with lower values indicating a higher priority.</span></span> <span data-ttu-id="edd0f-193">Om för en slutpunkt, måste de anges för alla slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="edd0f-193">If specified for one endpoint, they must be specified for all endpoints.</span></span> <span data-ttu-id="edd0f-194">Om det utelämnas används tillämpas standardvärden från '1' i hello ordning som hello slutpunkter visas.</span><span class="sxs-lookup"><span data-stu-id="edd0f-194">If omitted, default values starting from '1' are applied in hello order that hello endpoints are listed.</span></span>

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a><span data-ttu-id="edd0f-195">Exempel 1: Lägga till slutpunkter för webbprogram med hjälp av`Add-AzureRmTrafficManagerEndpointConfig`</span><span class="sxs-lookup"><span data-stu-id="edd0f-195">Example 1: Adding Web App endpoints using `Add-AzureRmTrafficManagerEndpointConfig`</span></span>

<span data-ttu-id="edd0f-196">I det här exemplet vi skapa en Traffic Manager-profilen och lägga till två slutpunkter för webbprogram som använder hello `Add-AzureRmTrafficManagerEndpointConfig` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="edd0f-196">In this example, we create a Traffic Manager profile and add two Web App endpoints using hello `Add-AzureRmTrafficManagerEndpointConfig` cmdlet.</span></span>

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$webapp1 = Get-AzureRMWebApp -Name webapp1
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
$webapp2 = Get-AzureRMWebApp -Name webapp2
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```
### <a name="example-2-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a><span data-ttu-id="edd0f-197">Exempel 2: Lägga till en publicIpAddress-slutpunkten med`New-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="edd0f-197">Example 2: Adding a publicIpAddress endpoint using `New-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="edd0f-198">I det här exemplet läggs en offentlig IP-adressresurs toohello Traffic Manager-profilen.</span><span class="sxs-lookup"><span data-stu-id="edd0f-198">In this example, a public IP address resource is added toohello Traffic Manager profile.</span></span> <span data-ttu-id="edd0f-199">hello offentliga IP-adressen måste ha ett DNS-namn som har konfigurerats och kan vara bundna antingen toohello nätverkskort på en virtuell dator eller tooa belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="edd0f-199">hello public IP address must have a DNS name configured, and can be bound either toohello NIC of a VM or tooa load balancer.</span></span>

```powershell
$ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled
```

## <a name="adding-external-endpoints"></a><span data-ttu-id="edd0f-200">Lägga till externa slutpunkter</span><span class="sxs-lookup"><span data-stu-id="edd0f-200">Adding External Endpoints</span></span>

<span data-ttu-id="edd0f-201">Manager använder externa slutpunkter toodirect trafik tooservices finns utanför Azure-trafik.</span><span class="sxs-lookup"><span data-stu-id="edd0f-201">Traffic Manager uses external endpoints toodirect traffic tooservices hosted outside of Azure.</span></span> <span data-ttu-id="edd0f-202">Som med Azure-slutpunkter externa slutpunkter kan läggas till antingen med hjälp av `Add-AzureRmTrafficManagerEndpointConfig` följt av `Set-AzureRmTrafficManagerProfile`, eller `New-AzureRMTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="edd0f-202">As with Azure endpoints, external endpoints can be added either using `Add-AzureRmTrafficManagerEndpointConfig` followed by `Set-AzureRmTrafficManagerProfile`, or `New-AzureRMTrafficManagerEndpoint`.</span></span>

<span data-ttu-id="edd0f-203">När du anger externa slutpunkter:</span><span class="sxs-lookup"><span data-stu-id="edd0f-203">When specifying external endpoints:</span></span>

* <span data-ttu-id="edd0f-204">hello endpoint domännamnet måste anges med hello 'Target' parametern</span><span class="sxs-lookup"><span data-stu-id="edd0f-204">hello endpoint domain name must be specified using hello 'Target' parameter</span></span>
* <span data-ttu-id="edd0f-205">Om hello 'Prestanda' routning av nätverkstrafik metoden används, krävs hello 'EndpointLocation'.</span><span class="sxs-lookup"><span data-stu-id="edd0f-205">If hello 'Performance' traffic-routing method is used, hello 'EndpointLocation' is required.</span></span> <span data-ttu-id="edd0f-206">Annars är valfria.</span><span class="sxs-lookup"><span data-stu-id="edd0f-206">Otherwise it is optional.</span></span> <span data-ttu-id="edd0f-207">hello-värdet måste vara en [giltig Azure regionnamn](https://azure.microsoft.com/regions/).</span><span class="sxs-lookup"><span data-stu-id="edd0f-207">hello value must be a [valid Azure region name](https://azure.microsoft.com/regions/).</span></span>
* <span data-ttu-id="edd0f-208">Hej 'Vikt' och 'Priority' är valfria.</span><span class="sxs-lookup"><span data-stu-id="edd0f-208">hello 'Weight' and 'Priority' are optional.</span></span>

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a><span data-ttu-id="edd0f-209">Exempel 1: Lägga till externa slutpunkter som använder `Add-AzureRmTrafficManagerEndpointConfig` och`Set-AzureRmTrafficManagerProfile`</span><span class="sxs-lookup"><span data-stu-id="edd0f-209">Example 1: Adding external endpoints using `Add-AzureRmTrafficManagerEndpointConfig` and `Set-AzureRmTrafficManagerProfile`</span></span>

<span data-ttu-id="edd0f-210">I det här exemplet vi skapa en Traffic Manager-profil, lägga till två externa slutpunkter och genomför hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="edd0f-210">In this example, we create a Traffic Manager profile, add two external endpoints, and commit hello changes.</span></span>

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointLocation "North Europe" -EndpointStatus Enabled
Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointLocation "Central US" -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a><span data-ttu-id="edd0f-211">Exempel 2: Lägga till externa slutpunkter med hjälp av`New-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="edd0f-211">Example 2: Adding external endpoints using `New-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="edd0f-212">Vi lägga till en extern slutpunkt tooan befintlig profil i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="edd0f-212">In this example, we add an external endpoint tooan existing profile.</span></span> <span data-ttu-id="edd0f-213">hello-profilen har angetts med hello-profil och resursen gruppnamn.</span><span class="sxs-lookup"><span data-stu-id="edd0f-213">hello profile is specified using hello profile and resource group names.</span></span>

```powershell
New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
```

## <a name="adding-nested-endpoints"></a><span data-ttu-id="edd0f-214">Lägga till 'kapslade-slutpunkter</span><span class="sxs-lookup"><span data-stu-id="edd0f-214">Adding 'Nested' endpoints</span></span>

<span data-ttu-id="edd0f-215">Varje Traffic Manager-profil anger en enda metod för routning av nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="edd0f-215">Each Traffic Manager profile specifies a single traffic-routing method.</span></span> <span data-ttu-id="edd0f-216">Det finns emellertid scenarier som kräver mer avancerad routning av nätverkstrafik än hello routning tillhandahålls som en enskild Traffic Manager-profil.</span><span class="sxs-lookup"><span data-stu-id="edd0f-216">However, there are scenarios that require more sophisticated traffic routing than hello routing provided by a single Traffic Manager profile.</span></span> <span data-ttu-id="edd0f-217">Du kan kapsla Traffic Manager profiler toocombine hello fördelarna med fler än en metod för routning av nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="edd0f-217">You can nest Traffic Manager profiles toocombine hello benefits of more than one traffic-routing method.</span></span> <span data-ttu-id="edd0f-218">Kapslade profiler kan du toooverride hello standard Traffic Manager beteende toosupport större och mer komplexa programdistributioner.</span><span class="sxs-lookup"><span data-stu-id="edd0f-218">Nested profiles allow you toooverride hello default Traffic Manager behavior toosupport larger and more complex application deployments.</span></span> <span data-ttu-id="edd0f-219">Mer detaljerad exemplen finns [kapslade Traffic Manager-profiler](traffic-manager-nested-profiles.md).</span><span class="sxs-lookup"><span data-stu-id="edd0f-219">For more detailed examples, see [Nested Traffic Manager profiles](traffic-manager-nested-profiles.md).</span></span>

<span data-ttu-id="edd0f-220">Kapslade slutpunkter har konfigurerats på hello överordnade profil, med hjälp av en viss slutpunkts-typ, 'NestedEndpoints'.</span><span class="sxs-lookup"><span data-stu-id="edd0f-220">Nested endpoints are configured at hello parent profile, using a specific endpoint type, 'NestedEndpoints'.</span></span> <span data-ttu-id="edd0f-221">När du anger kapslade slutpunkter:</span><span class="sxs-lookup"><span data-stu-id="edd0f-221">When specifying nested endpoints:</span></span>

* <span data-ttu-id="edd0f-222">hello slutpunkt måste anges med hello 'targetResourceId' parametern</span><span class="sxs-lookup"><span data-stu-id="edd0f-222">hello endpoint must be specified using hello 'targetResourceId' parameter</span></span>
* <span data-ttu-id="edd0f-223">Om hello 'Prestanda' routning av nätverkstrafik metoden används, krävs hello 'EndpointLocation'.</span><span class="sxs-lookup"><span data-stu-id="edd0f-223">If hello 'Performance' traffic-routing method is used, hello 'EndpointLocation' is required.</span></span> <span data-ttu-id="edd0f-224">Annars är valfria.</span><span class="sxs-lookup"><span data-stu-id="edd0f-224">Otherwise it is optional.</span></span> <span data-ttu-id="edd0f-225">hello-värdet måste vara en [giltig Azure regionnamn](http://azure.microsoft.com/regions/).</span><span class="sxs-lookup"><span data-stu-id="edd0f-225">hello value must be a [valid Azure region name](http://azure.microsoft.com/regions/).</span></span>
* <span data-ttu-id="edd0f-226">Hej 'Vikt' och 'Priority' är valfria för Azure-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="edd0f-226">hello 'Weight' and 'Priority' are optional, as for Azure endpoints.</span></span>
* <span data-ttu-id="edd0f-227">hello 'MinChildEndpoints-parametern är valfri.</span><span class="sxs-lookup"><span data-stu-id="edd0f-227">hello 'MinChildEndpoints' parameter is optional.</span></span> <span data-ttu-id="edd0f-228">hello standardvärdet är '1'.</span><span class="sxs-lookup"><span data-stu-id="edd0f-228">hello default value is '1'.</span></span> <span data-ttu-id="edd0f-229">Om hello antalet tillgängliga slutpunkterna understiger tröskelvärdet, anses hello överordnade profil hello underordnade profilen 'försämrad' och diverts trafik toohello slutpunkter i hello överordnade profilen.</span><span class="sxs-lookup"><span data-stu-id="edd0f-229">If hello number of available endpoints falls below this threshold, hello parent profile considers hello child profile 'degraded' and diverts traffic toohello other endpoints in hello parent profile.</span></span>

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a><span data-ttu-id="edd0f-230">Exempel 1: Lägga till kapslade slutpunkter som använder `Add-AzureRmTrafficManagerEndpointConfig` och`Set-AzureRmTrafficManagerProfile`</span><span class="sxs-lookup"><span data-stu-id="edd0f-230">Example 1: Adding nested endpoints using `Add-AzureRmTrafficManagerEndpointConfig` and `Set-AzureRmTrafficManagerProfile`</span></span>

<span data-ttu-id="edd0f-231">I det här exemplet vi skapa ny Traffic Manager-över- och underordnade profiler, Lägg till hello underordnad som överordnad toohello kapslade slutpunkt och genomför hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="edd0f-231">In this example, we create new Traffic Manager child and parent profiles, add hello child as a nested endpoint toohello parent, and commit hello changes.</span></span>

```powershell
$child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

<span data-ttu-id="edd0f-232">Vi inte lägga till några andra slutpunkter toohello underordnade eller överordnade profiler planeringsaspekter i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="edd0f-232">For brevity in this example, we did not add any other endpoints toohello child or parent profiles.</span></span>

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a><span data-ttu-id="edd0f-233">Exempel 2: Lägga till kapslade slutpunkter med hjälp av`New-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="edd0f-233">Example 2: Adding nested endpoints using `New-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="edd0f-234">I det här exemplet vi lägga till en befintlig profil för underordnade som en kapslad endpoint tooan befintlig överordnad profil.</span><span class="sxs-lookup"><span data-stu-id="edd0f-234">In this example, we add an existing child profile as a nested endpoint tooan existing parent profile.</span></span> <span data-ttu-id="edd0f-235">hello-profilen har angetts med hello-profil och resursen gruppnamn.</span><span class="sxs-lookup"><span data-stu-id="edd0f-235">hello profile is specified using hello profile and resource group names.</span></span>

```powershell
$child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
```

## <a name="update-a-traffic-manager-endpoint"></a><span data-ttu-id="edd0f-236">Uppdatera en Traffic Manager-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="edd0f-236">Update a Traffic Manager Endpoint</span></span>

<span data-ttu-id="edd0f-237">Det finns två sätt tooupdate en befintlig Traffic Manager-slutpunkt:</span><span class="sxs-lookup"><span data-stu-id="edd0f-237">There are two ways tooupdate an existing Traffic Manager endpoint:</span></span>

1. <span data-ttu-id="edd0f-238">Hämta hello Traffic Manager-profil med `Get-AzureRmTrafficManagerProfile`, uppdatera hello endpoint egenskaper i hello profil och sedan spara hello ändringar med hjälp av `Set-AzureRmTrafficManagerProfile`.</span><span class="sxs-lookup"><span data-stu-id="edd0f-238">Get hello Traffic Manager profile using `Get-AzureRmTrafficManagerProfile`, update hello endpoint properties within hello profile, and commit hello changes using `Set-AzureRmTrafficManagerProfile`.</span></span> <span data-ttu-id="edd0f-239">Den här metoden har hello fördelen att den kan tooupdate mer än en slutpunkt i en enda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="edd0f-239">This method has hello advantage of being able tooupdate more than one endpoint in a single operation.</span></span>
2. <span data-ttu-id="edd0f-240">Hämta hello Traffic Manager-slutpunkten med `Get-AzureRmTrafficManagerEndpoint`, uppdatera hello endpoint egenskaper och sedan spara hello ändringar med hjälp av `Set-AzureRmTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="edd0f-240">Get hello Traffic Manager endpoint using `Get-AzureRmTrafficManagerEndpoint`, update hello endpoint properties, and commit hello changes using `Set-AzureRmTrafficManagerEndpoint`.</span></span> <span data-ttu-id="edd0f-241">Den här metoden är enklare, eftersom det inte kräver att indexera i hello slutpunkter array i hello profilen.</span><span class="sxs-lookup"><span data-stu-id="edd0f-241">This method is simpler, since it does not require indexing into hello Endpoints array in hello profile.</span></span>

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a><span data-ttu-id="edd0f-242">Exempel 1: Uppdatera slutpunkter med hjälp av `Get-AzureRmTrafficManagerProfile` och`Set-AzureRmTrafficManagerProfile`</span><span class="sxs-lookup"><span data-stu-id="edd0f-242">Example 1: Updating endpoints using `Get-AzureRmTrafficManagerProfile` and `Set-AzureRmTrafficManagerProfile`</span></span>

<span data-ttu-id="edd0f-243">I det här exemplet ändrar vi hello prioritet på två slutpunkter i en befintlig profil.</span><span class="sxs-lookup"><span data-stu-id="edd0f-243">In this example, we modify hello priority on two endpoints within an existing profile.</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
$profile.Endpoints[0].Priority = 2
$profile.Endpoints[1].Priority = 1
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a><span data-ttu-id="edd0f-244">Exempel 2: Uppdatera slutpunkten med `Get-AzureRmTrafficManagerEndpoint` och`Set-AzureRmTrafficManagerEndpoint`</span><span class="sxs-lookup"><span data-stu-id="edd0f-244">Example 2: Updating an endpoint using `Get-AzureRmTrafficManagerEndpoint` and `Set-AzureRmTrafficManagerEndpoint`</span></span>

<span data-ttu-id="edd0f-245">I det här exemplet ändrar vi hello vikten av en enda slutpunkt i en befintlig profil.</span><span class="sxs-lookup"><span data-stu-id="edd0f-245">In this example, we modify hello weight of a single endpoint in an existing profile.</span></span>

```powershell
$endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
$endpoint.Weight = 20
Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint
```

## <a name="enabling-and-disabling-endpoints-and-profiles"></a><span data-ttu-id="edd0f-246">Aktiverar och inaktiverar slutpunkter och profiler</span><span class="sxs-lookup"><span data-stu-id="edd0f-246">Enabling and Disabling Endpoints and Profiles</span></span>

<span data-ttu-id="edd0f-247">Traffic Manager kan enskilda slutpunkter toobe aktiverade och inaktiverade samt att tillåta aktivering och inaktivering av hela profiler.</span><span class="sxs-lookup"><span data-stu-id="edd0f-247">Traffic Manager allows individual endpoints toobe enabled and disabled, as well as allowing enabling and disabling of entire profiles.</span></span>
<span data-ttu-id="edd0f-248">Dessa ändringar kan göras av hämta/uppdatera/inställningen hello endpoint eller profil resurser.</span><span class="sxs-lookup"><span data-stu-id="edd0f-248">These changes can be made by getting/updating/setting hello endpoint or profile resources.</span></span> <span data-ttu-id="edd0f-249">toostreamline dessa vanliga åtgärder stöds också via dedikerade cmdlets.</span><span class="sxs-lookup"><span data-stu-id="edd0f-249">toostreamline these common operations, they are also supported via dedicated cmdlets.</span></span>

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a><span data-ttu-id="edd0f-250">Exempel 1: Aktivera och inaktivera Traffic Manager-profilen</span><span class="sxs-lookup"><span data-stu-id="edd0f-250">Example 1: Enabling and disabling a Traffic Manager profile</span></span>

<span data-ttu-id="edd0f-251">Använd tooenable Traffic Manager-profilen `Enable-AzureRmTrafficManagerProfile`.</span><span class="sxs-lookup"><span data-stu-id="edd0f-251">tooenable a Traffic Manager profile, use `Enable-AzureRmTrafficManagerProfile`.</span></span> <span data-ttu-id="edd0f-252">hello-profil kan anges med en profilobjektet.</span><span class="sxs-lookup"><span data-stu-id="edd0f-252">hello profile can be specified using a profile object.</span></span> <span data-ttu-id="edd0f-253">Hej profilobjektet kan skickas via hello pipeline eller genom att använda hello '-TrafficManagerProfile-parametern.</span><span class="sxs-lookup"><span data-stu-id="edd0f-253">hello profile object can be passed via hello pipeline or by using hello '-TrafficManagerProfile' parameter.</span></span> <span data-ttu-id="edd0f-254">I det här exemplet anger vi hello profil med hello-profil och resursen gruppnamn.</span><span class="sxs-lookup"><span data-stu-id="edd0f-254">In this example, we specify hello profile by hello profile and resource group name.</span></span>

```powershell
Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

<span data-ttu-id="edd0f-255">toodisable Traffic Manager-profilen:</span><span class="sxs-lookup"><span data-stu-id="edd0f-255">toodisable a Traffic Manager profile:</span></span>

```powershell
Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

<span data-ttu-id="edd0f-256">hello inaktivera AzureRmTrafficManagerProfile cmdlet uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="edd0f-256">hello Disable-AzureRmTrafficManagerProfile cmdlet prompts for confirmation.</span></span> <span data-ttu-id="edd0f-257">Den här uppmaningen kan undertryckas med hjälp av hello '-Force-parametern.</span><span class="sxs-lookup"><span data-stu-id="edd0f-257">This prompt can be suppressed using hello '-Force' parameter.</span></span>

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a><span data-ttu-id="edd0f-258">Exempel 2: Aktivera och inaktivera Traffic Manager-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="edd0f-258">Example 2: Enabling and disabling a Traffic Manager endpoint</span></span>

<span data-ttu-id="edd0f-259">tooenable en Traffic Manager-slutpunkt, Använd `Enable-AzureRmTrafficManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="edd0f-259">tooenable a Traffic Manager endpoint, use `Enable-AzureRmTrafficManagerEndpoint`.</span></span> <span data-ttu-id="edd0f-260">Det finns två sätt toospecify hello slutpunkt</span><span class="sxs-lookup"><span data-stu-id="edd0f-260">There are two ways toospecify hello endpoint</span></span>

1. <span data-ttu-id="edd0f-261">Med hjälp av en TrafficManagerEndpoint-objektet som överförs via hello pipeline eller hello '-TrafficManagerEndpoint-parameter</span><span class="sxs-lookup"><span data-stu-id="edd0f-261">Using a TrafficManagerEndpoint object passed via hello pipeline or using hello '-TrafficManagerEndpoint' parameter</span></span>
2. <span data-ttu-id="edd0f-262">Med hjälp av hello slutpunktsnamn, typen av slutpunkt, profilnamn och resursgruppens namn:</span><span class="sxs-lookup"><span data-stu-id="edd0f-262">Using hello endpoint name, endpoint type, profile name, and resource group name:</span></span>

```powershell
Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

<span data-ttu-id="edd0f-263">På liknande sätt toodisable Traffic Manager-slutpunkten:</span><span class="sxs-lookup"><span data-stu-id="edd0f-263">Similarly, toodisable a Traffic Manager endpoint:</span></span>

```powershell
Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

<span data-ttu-id="edd0f-264">Precis som med `Disable-AzureRmTrafficManagerProfile`, hello `Disable-AzureRmTrafficManagerEndpoint` cmdlet uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="edd0f-264">As with `Disable-AzureRmTrafficManagerProfile`, hello `Disable-AzureRmTrafficManagerEndpoint` cmdlet prompts for confirmation.</span></span> <span data-ttu-id="edd0f-265">Den här uppmaningen kan undertryckas med hjälp av hello '-Force-parametern.</span><span class="sxs-lookup"><span data-stu-id="edd0f-265">This prompt can be suppressed using hello '-Force' parameter.</span></span>

## <a name="delete-a-traffic-manager-endpoint"></a><span data-ttu-id="edd0f-266">Ta bort en Traffic Manager-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="edd0f-266">Delete a Traffic Manager Endpoint</span></span>

<span data-ttu-id="edd0f-267">tooremove enskilda slutpunkter, använda hello `Remove-AzureRmTrafficManagerEndpoint` cmdlet:</span><span class="sxs-lookup"><span data-stu-id="edd0f-267">tooremove individual endpoints, use hello `Remove-AzureRmTrafficManagerEndpoint` cmdlet:</span></span>

```powershell
Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

<span data-ttu-id="edd0f-268">Denna cmdlet uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="edd0f-268">This cmdlet prompts for confirmation.</span></span> <span data-ttu-id="edd0f-269">Den här uppmaningen kan undertryckas med hjälp av hello '-Force-parametern.</span><span class="sxs-lookup"><span data-stu-id="edd0f-269">This prompt can be suppressed using hello '-Force' parameter.</span></span>

## <a name="delete-a-traffic-manager-profile"></a><span data-ttu-id="edd0f-270">Ta bort en Traffic Manager-profil</span><span class="sxs-lookup"><span data-stu-id="edd0f-270">Delete a Traffic Manager Profile</span></span>

<span data-ttu-id="edd0f-271">toodelete Traffic Manager-profilen använder hello `Remove-AzureRmTrafficManagerProfile` cmdlet, anger hello-profil och resursen gruppnamn:</span><span class="sxs-lookup"><span data-stu-id="edd0f-271">toodelete a Traffic Manager profile, use hello `Remove-AzureRmTrafficManagerProfile` cmdlet, specifying hello profile and resource group names:</span></span>

```powershell
Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

<span data-ttu-id="edd0f-272">Denna cmdlet uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="edd0f-272">This cmdlet prompts for confirmation.</span></span> <span data-ttu-id="edd0f-273">Den här uppmaningen kan undertryckas med hjälp av hello '-Force-parametern.</span><span class="sxs-lookup"><span data-stu-id="edd0f-273">This prompt can be suppressed using hello '-Force' parameter.</span></span>

<span data-ttu-id="edd0f-274">hello profil toobe bort kan också anges med en för profilobjektet:</span><span class="sxs-lookup"><span data-stu-id="edd0f-274">hello profile toobe deleted can also be specified using a profile object:</span></span>

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

<span data-ttu-id="edd0f-275">Den här sekvensen kan också skickas:</span><span class="sxs-lookup"><span data-stu-id="edd0f-275">This sequence can also be piped:</span></span>

```powershell
Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a><span data-ttu-id="edd0f-276">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="edd0f-276">Next steps</span></span>

[<span data-ttu-id="edd0f-277">Traffic Manager-övervakning</span><span class="sxs-lookup"><span data-stu-id="edd0f-277">Traffic Manager monitoring</span></span>](traffic-manager-monitoring.md)

[<span data-ttu-id="edd0f-278">Prestandaöverväganden för Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="edd0f-278">Traffic Manager performance considerations</span></span>](traffic-manager-performance-considerations.md)
