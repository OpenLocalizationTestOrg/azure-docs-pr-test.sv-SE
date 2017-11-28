---
title: aaaManage Azure CDN med PowerShell | Microsoft Docs
description: "Lär dig hur toouse hello Azure PowerShell-cmdlets toomanage Azure CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: fb6f57a5-6e26-4847-8fd9-b51fb05a79eb
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 269373136d4ef018e4d31f147456b4be2253b463
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-cdn-with-powershell"></a><span data-ttu-id="00736-103">Hantera Azure CDN med PowerShell</span><span class="sxs-lookup"><span data-stu-id="00736-103">Manage Azure CDN with PowerShell</span></span>
<span data-ttu-id="00736-104">PowerShell tillhandahåller en hello mest flexibla metoder toomanage dina Azure CDN-profiler och slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="00736-104">PowerShell provides one of hello most flexible methods toomanage your Azure CDN profiles and endpoints.</span></span>  <span data-ttu-id="00736-105">Du kan använda PowerShell interaktivt eller genom att skriva skript tooautomate hanteringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="00736-105">You can use PowerShell interactively or by writing scripts tooautomate management tasks.</span></span>  <span data-ttu-id="00736-106">Den här självstudiekursen visas flera av de vanligaste hello-uppgifter du kan utföra med PowerShell toomanage dina Azure CDN-profiler och slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="00736-106">This tutorial demonstrates several of hello most common tasks you can accomplish with PowerShell toomanage your Azure CDN profiles and endpoints.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="00736-107">Krav</span><span class="sxs-lookup"><span data-stu-id="00736-107">Prerequisites</span></span>
<span data-ttu-id="00736-108">toouse PowerShell toomanage din Azure CDN-profiler och slutpunkter, måste du ha hello Azure PowerShell module installerad.</span><span class="sxs-lookup"><span data-stu-id="00736-108">toouse PowerShell toomanage your Azure CDN profiles and endpoints, you must have hello Azure PowerShell module installed.</span></span>  <span data-ttu-id="00736-109">toolearn hur tooinstall Azure PowerShell och ansluta med hello tooAzure `Login-AzureRmAccount` cmdlet, se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="00736-109">toolearn how tooinstall Azure PowerShell and connect tooAzure using hello `Login-AzureRmAccount` cmdlet, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="00736-110">Du måste logga in med `Login-AzureRmAccount` innan du kan köra Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="00736-110">You must log in with `Login-AzureRmAccount` before you can execute Azure PowerShell cmdlets.</span></span>
> 
> 

## <a name="listing-hello-azure-cdn-cmdlets"></a><span data-ttu-id="00736-111">Visar en lista över hello Azure CDN-cmdlets</span><span class="sxs-lookup"><span data-stu-id="00736-111">Listing hello Azure CDN cmdlets</span></span>
<span data-ttu-id="00736-112">Du kan visa en lista med alla hello Azure CDN cmdlet: ar med hello `Get-Command` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="00736-112">You can list all hello Azure CDN cmdlets using hello `Get-Command` cmdlet.</span></span>

```text
PS C:\> Get-Command -Module AzureRM.Cdn

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpointNameAvailability             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfileSsoUrl                        2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Publish-AzureRmCdnEndpointContent                  2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnCustomDomain                      2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnEndpoint                          2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnProfile                           2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Start-AzureRmCdnEndpoint                           2.0.0      AzureRm.Cdn
Cmdlet          Stop-AzureRmCdnEndpoint                            2.0.0      AzureRm.Cdn
Cmdlet          Test-AzureRmCdnCustomDomain                        2.0.0      AzureRm.Cdn
Cmdlet          Unpublish-AzureRmCdnEndpointContent                2.0.0      AzureRm.Cdn
```

## <a name="getting-help"></a><span data-ttu-id="00736-113">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="00736-113">Getting help</span></span>
<span data-ttu-id="00736-114">Du kan få hjälp med någon av dessa cmdlet: ar med hello `Get-Help` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="00736-114">You can get help with any of these cmdlets using hello `Get-Help` cmdlet.</span></span>  <span data-ttu-id="00736-115">`Get-Help`ger användning och syntax och du kan också visas exempel.</span><span class="sxs-lookup"><span data-stu-id="00736-115">`Get-Help` provides usage and syntax, and optionally shows examples.</span></span>

```text
PS C:\> Get-Help Get-AzureRmCdnProfile

NAME
    Get-AzureRmCdnProfile

SYNOPSIS
    Gets an Azure CDN profile.


SYNTAX
    Get-AzureRmCdnProfile [-ProfileName <String>] [-ResourceGroupName <String>] [-InformationAction
    <ActionPreference>] [-InformationVariable <String>] [<CommonParameters>]


DESCRIPTION
    Gets an Azure CDN profile and all related information.


RELATED LINKS

REMARKS
    toosee hello examples, type: "get-help Get-AzureRmCdnProfile -examples".
    For more information, type: "get-help Get-AzureRmCdnProfile -detailed".
    For technical information, type: "get-help Get-AzureRmCdnProfile -full".

```

## <a name="listing-existing-azure-cdn-profiles"></a><span data-ttu-id="00736-116">Lista över befintliga Azure CDN-profiler</span><span class="sxs-lookup"><span data-stu-id="00736-116">Listing existing Azure CDN profiles</span></span>
<span data-ttu-id="00736-117">Hej `Get-AzureRmCdnProfile` cmdlet utan några parametrar hämtar alla dina befintliga CDN-profiler.</span><span class="sxs-lookup"><span data-stu-id="00736-117">hello `Get-AzureRmCdnProfile` cmdlet without any parameters retrieves all your existing CDN profiles.</span></span>

```powershell
Get-AzureRmCdnProfile
```

<span data-ttu-id="00736-118">Den här utdatan kan vara via rörledningar toocmdlets för uppräkningen.</span><span class="sxs-lookup"><span data-stu-id="00736-118">This output can be piped toocmdlets for enumeration.</span></span>

```powershell
# Output hello name of all profiles on this subscription.
Get-AzureRmCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzureRmCdnProfile | Where-Object { $_.Sku.Name -eq "StandardVerizon" }
```

<span data-ttu-id="00736-119">Du kan också returnera en enskild profil genom att ange hello namn och resursen profilgrupp.</span><span class="sxs-lookup"><span data-stu-id="00736-119">You can also return a single profile by specifying hello profile name and resource group.</span></span>

```powershell
Get-AzureRmCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

> [!TIP]
> <span data-ttu-id="00736-120">Det är möjligt toohave flera CDN-profiler med samma namn hello så länge de är i olika resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="00736-120">It is possible toohave multiple CDN profiles with hello same name, so long as they are in different resource groups.</span></span>  <span data-ttu-id="00736-121">Om du utesluter hello `ResourceGroupName` parametern returnerar alla profiler med ett matchande namn.</span><span class="sxs-lookup"><span data-stu-id="00736-121">Omitting hello `ResourceGroupName` parameter returns all profiles with a matching name.</span></span>
> 
> 

## <a name="listing-existing-cdn-endpoints"></a><span data-ttu-id="00736-122">Lista över befintliga CDN-slutpunkter</span><span class="sxs-lookup"><span data-stu-id="00736-122">Listing existing CDN endpoints</span></span>
<span data-ttu-id="00736-123">`Get-AzureRmCdnEndpoint`Hämta en enskild slutpunkt eller alla hello slutpunkter för en profil.</span><span class="sxs-lookup"><span data-stu-id="00736-123">`Get-AzureRmCdnEndpoint` can retrieve an individual endpoint or all hello endpoints on a profile.</span></span>  

```powershell
# Get a single endpoint.
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Get all of hello endpoints on a given profile. 
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG

# Return all of hello endpoints on all of hello profiles.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint

# Return all of hello endpoints in this subscription that are currently running.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Where-Object { $_.ResourceState -eq "Running" }
```

## <a name="creating-cdn-profiles-and-endpoints"></a><span data-ttu-id="00736-124">Skapa CDN-profiler och slutpunkter</span><span class="sxs-lookup"><span data-stu-id="00736-124">Creating CDN profiles and endpoints</span></span>
<span data-ttu-id="00736-125">`New-AzureRmCdnProfile`och `New-AzureRmCdnEndpoint` används toocreate CDN profiler och slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="00736-125">`New-AzureRmCdnProfile` and `New-AzureRmCdnEndpoint` are used toocreate CDN profiles and endpoints.</span></span>

```powershell
# Create a new profile
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US"

# Create a new endpoint
New-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US" | New-AzureRmCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a><span data-ttu-id="00736-126">Kontrollera tillgänglighet för slutpunkt-namn</span><span class="sxs-lookup"><span data-stu-id="00736-126">Checking endpoint name availability</span></span>
<span data-ttu-id="00736-127">`Get-AzureRmCdnEndpointNameAvailability`Returnerar ett objekt som anger om det finns en namnet på slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="00736-127">`Get-AzureRmCdnEndpointNameAvailability` returns an object indicating if an endpoint name is available.</span></span>

```powershell
# Retrieve availability
$availability = Get-AzureRmCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message toohello console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a><span data-ttu-id="00736-128">Lägga till en anpassad domän</span><span class="sxs-lookup"><span data-stu-id="00736-128">Adding a custom domain</span></span>
<span data-ttu-id="00736-129">`New-AzureRmCdnCustomDomain`lägger till en befintlig slutpunkt för domänen namnet tooan.</span><span class="sxs-lookup"><span data-stu-id="00736-129">`New-AzureRmCdnCustomDomain` adds a custom domain name tooan existing endpoint.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="00736-130">Du måste ställa in hello CNAME-post hos din DNS-leverantör som beskrivs i [hur toomap anpassad domän tooContent innehållsleveransnätverk (CDN) slutpunkt](cdn-map-content-to-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="00736-130">You must set up hello CNAME with your DNS provider as described in [How toomap Custom Domain tooContent Delivery Network (CDN) endpoint](cdn-map-content-to-custom-domain.md).</span></span>  <span data-ttu-id="00736-131">Du kan testa hello mappningen innan du ändrar slutpunkten med `Test-AzureRmCdnCustomDomain`.</span><span class="sxs-lookup"><span data-stu-id="00736-131">You can test hello mapping before modifying your endpoint using `Test-AzureRmCdnCustomDomain`.</span></span>
> 
> 

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Check hello mapping
$result = Test-AzureRmCdnCustomDomain -CdnEndpoint $endpoint -CustomDomainHostName "cdn.contoso.com"

# Create hello custom domain on hello endpoint
If($result.CustomDomainValidated){ New-AzureRmCdnCustomDomain -CustomDomainName Contoso -HostName "cdn.contoso.com" -CdnEndpoint $endpoint }
```

## <a name="modifying-an-endpoint"></a><span data-ttu-id="00736-132">Ändra en slutpunkt</span><span class="sxs-lookup"><span data-stu-id="00736-132">Modifying an endpoint</span></span>
<span data-ttu-id="00736-133">`Set-AzureRmCdnEndpoint`ändrar en befintlig slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="00736-133">`Set-AzureRmCdnEndpoint` modifies an existing endpoint.</span></span>

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save hello changed endpoint and apply hello changes
Set-AzureRmCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a><span data-ttu-id="00736-134">Rensa/Förproduktion-loading CDN tillgångar</span><span class="sxs-lookup"><span data-stu-id="00736-134">Purging/Pre-loading CDN assets</span></span>
<span data-ttu-id="00736-135">`Unpublish-AzureRmCdnEndpointContent`återställningspunkter cachelagras tillgångar, medan `Publish-AzureRmCdnEndpointContent` före läser in tillgångar på slutpunkter som stöds.</span><span class="sxs-lookup"><span data-stu-id="00736-135">`Unpublish-AzureRmCdnEndpointContent` purges cached assets, while `Publish-AzureRmCdnEndpointContent` pre-loads assets on supported endpoints.</span></span>

```powershell
# Purge some assets.
Unpublish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Unpublish-AzureRmCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a><span data-ttu-id="00736-136">Starta/Stoppa CDN-slutpunkter</span><span class="sxs-lookup"><span data-stu-id="00736-136">Starting/Stopping CDN endpoints</span></span>
<span data-ttu-id="00736-137">`Start-AzureRmCdnEndpoint`och `Stop-AzureRmCdnEndpoint` kan använda toostart och stoppa enskilda slutpunkter eller grupper av slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="00736-137">`Start-AzureRmCdnEndpoint` and `Stop-AzureRmCdnEndpoint` can be used toostart and stop individual endpoints or groups of endpoints.</span></span>

```powershell
# Stop hello cdndocdemo endpoint
Stop-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Stop-AzureRmCdnEndpoint

# Start all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Start-AzureRmCdnEndpoint
```

## <a name="deleting-cdn-resources"></a><span data-ttu-id="00736-138">Ta bort CDN-resurser</span><span class="sxs-lookup"><span data-stu-id="00736-138">Deleting CDN resources</span></span>
<span data-ttu-id="00736-139">`Remove-AzureRmCdnProfile`och `Remove-AzureRmCdnEndpoint` kan vara används tooremove profiler och slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="00736-139">`Remove-AzureRmCdnProfile` and `Remove-AzureRmCdnEndpoint` can be used tooremove profiles and endpoints.</span></span>

```powershell
# Remove a single endpoint
Remove-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all hello endpoints on a profile and skip confirmation (-Force)
Get-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzureRmCdnEndpoint | Remove-AzureRmCdnEndpoint -Force

# Remove a single profile
Remove-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a><span data-ttu-id="00736-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="00736-140">Next Steps</span></span>
<span data-ttu-id="00736-141">Lär dig hur tooautomate Azure CDN med [.NET](cdn-app-dev-net.md) eller [Node.js](cdn-app-dev-node.md).</span><span class="sxs-lookup"><span data-stu-id="00736-141">Learn how tooautomate Azure CDN with [.NET](cdn-app-dev-net.md) or [Node.js](cdn-app-dev-node.md).</span></span>

<span data-ttu-id="00736-142">toolearn om CDN-funktioner, se [CDN-översikt](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="00736-142">toolearn about CDN features, see [CDN Overview](cdn-overview.md).</span></span>

