---
title: Hantera Azure CDN med PowerShell | Microsoft Docs
description: "Lär dig hur du använder Azure PowerShell-cmdlets för att hantera Azure CDN."
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
ms.openlocfilehash: 5bd2eed7b34cafa43e8f38279890405d4ae55568
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-azure-cdn-with-powershell"></a><span data-ttu-id="8fa93-103">Hantera Azure CDN med PowerShell</span><span class="sxs-lookup"><span data-stu-id="8fa93-103">Manage Azure CDN with PowerShell</span></span>
<span data-ttu-id="8fa93-104">PowerShell innehåller något av de mest flexibla metoderna för att hantera dina Azure CDN-profiler och slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="8fa93-104">PowerShell provides one of the most flexible methods to manage your Azure CDN profiles and endpoints.</span></span>  <span data-ttu-id="8fa93-105">Du kan använda PowerShell interaktivt eller genom att skriva skript för att automatisera hanteringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="8fa93-105">You can use PowerShell interactively or by writing scripts to automate management tasks.</span></span>  <span data-ttu-id="8fa93-106">Den här självstudiekursen visas flera av de vanligaste uppgifterna som du kan utföra med PowerShell för att hantera dina Azure CDN-profiler och slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="8fa93-106">This tutorial demonstrates several of the most common tasks you can accomplish with PowerShell to manage your Azure CDN profiles and endpoints.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8fa93-107">Krav</span><span class="sxs-lookup"><span data-stu-id="8fa93-107">Prerequisites</span></span>
<span data-ttu-id="8fa93-108">Om du vill använda PowerShell för att hantera dina Azure CDN-profiler och slutpunkter, måste du ha installerat Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="8fa93-108">To use PowerShell to manage your Azure CDN profiles and endpoints, you must have the Azure PowerShell module installed.</span></span>  <span data-ttu-id="8fa93-109">Mer information om hur du installerar Azure PowerShell och ansluta till Azure med hjälp av `Login-AzureRmAccount` cmdlet, finns [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8fa93-109">To learn how to install Azure PowerShell and connect to Azure using the `Login-AzureRmAccount` cmdlet, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8fa93-110">Du måste logga in med `Login-AzureRmAccount` innan du kan köra Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="8fa93-110">You must log in with `Login-AzureRmAccount` before you can execute Azure PowerShell cmdlets.</span></span>
> 
> 

## <a name="listing-the-azure-cdn-cmdlets"></a><span data-ttu-id="8fa93-111">Visar en lista över Azure CDN-cmdlets</span><span class="sxs-lookup"><span data-stu-id="8fa93-111">Listing the Azure CDN cmdlets</span></span>
<span data-ttu-id="8fa93-112">Lista alla Azure CDN cmdlet: ar med den `Get-Command` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="8fa93-112">You can list all the Azure CDN cmdlets using the `Get-Command` cmdlet.</span></span>

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

## <a name="getting-help"></a><span data-ttu-id="8fa93-113">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="8fa93-113">Getting help</span></span>
<span data-ttu-id="8fa93-114">Du kan få hjälp med dessa cmdlets med hjälp av den `Get-Help` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="8fa93-114">You can get help with any of these cmdlets using the `Get-Help` cmdlet.</span></span>  <span data-ttu-id="8fa93-115">`Get-Help`ger användning och syntax och du kan också visas exempel.</span><span class="sxs-lookup"><span data-stu-id="8fa93-115">`Get-Help` provides usage and syntax, and optionally shows examples.</span></span>

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
    To see the examples, type: "get-help Get-AzureRmCdnProfile -examples".
    For more information, type: "get-help Get-AzureRmCdnProfile -detailed".
    For technical information, type: "get-help Get-AzureRmCdnProfile -full".

```

## <a name="listing-existing-azure-cdn-profiles"></a><span data-ttu-id="8fa93-116">Lista över befintliga Azure CDN-profiler</span><span class="sxs-lookup"><span data-stu-id="8fa93-116">Listing existing Azure CDN profiles</span></span>
<span data-ttu-id="8fa93-117">Den `Get-AzureRmCdnProfile` cmdlet utan några parametrar hämtar alla dina befintliga CDN-profiler.</span><span class="sxs-lookup"><span data-stu-id="8fa93-117">The `Get-AzureRmCdnProfile` cmdlet without any parameters retrieves all your existing CDN profiles.</span></span>

```powershell
Get-AzureRmCdnProfile
```

<span data-ttu-id="8fa93-118">Den här utdatan kan skickas till cmdletar för uppräkningen.</span><span class="sxs-lookup"><span data-stu-id="8fa93-118">This output can be piped to cmdlets for enumeration.</span></span>

```powershell
# Output the name of all profiles on this subscription.
Get-AzureRmCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzureRmCdnProfile | Where-Object { $_.Sku.Name -eq "StandardVerizon" }
```

<span data-ttu-id="8fa93-119">Du kan också returnera en enskild profil genom att ange namn och resursen profilgruppen.</span><span class="sxs-lookup"><span data-stu-id="8fa93-119">You can also return a single profile by specifying the profile name and resource group.</span></span>

```powershell
Get-AzureRmCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

> [!TIP]
> <span data-ttu-id="8fa93-120">Det är möjligt att ha flera CDN-profiler med samma namn, så länge de är i olika resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="8fa93-120">It is possible to have multiple CDN profiles with the same name, so long as they are in different resource groups.</span></span>  <span data-ttu-id="8fa93-121">Om du utesluter den `ResourceGroupName` parametern returnerar alla profiler med ett matchande namn.</span><span class="sxs-lookup"><span data-stu-id="8fa93-121">Omitting the `ResourceGroupName` parameter returns all profiles with a matching name.</span></span>
> 
> 

## <a name="listing-existing-cdn-endpoints"></a><span data-ttu-id="8fa93-122">Lista över befintliga CDN-slutpunkter</span><span class="sxs-lookup"><span data-stu-id="8fa93-122">Listing existing CDN endpoints</span></span>
<span data-ttu-id="8fa93-123">`Get-AzureRmCdnEndpoint`Hämta en enskild slutpunkt eller alla slutpunkter för en profil.</span><span class="sxs-lookup"><span data-stu-id="8fa93-123">`Get-AzureRmCdnEndpoint` can retrieve an individual endpoint or all the endpoints on a profile.</span></span>  

```powershell
# Get a single endpoint.
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Get all of the endpoints on a given profile. 
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG

# Return all of the endpoints on all of the profiles.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint

# Return all of the endpoints in this subscription that are currently running.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Where-Object { $_.ResourceState -eq "Running" }
```

## <a name="creating-cdn-profiles-and-endpoints"></a><span data-ttu-id="8fa93-124">Skapa CDN-profiler och slutpunkter</span><span class="sxs-lookup"><span data-stu-id="8fa93-124">Creating CDN profiles and endpoints</span></span>
<span data-ttu-id="8fa93-125">`New-AzureRmCdnProfile`och `New-AzureRmCdnEndpoint` används för att skapa CDN-profiler och slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="8fa93-125">`New-AzureRmCdnProfile` and `New-AzureRmCdnEndpoint` are used to create CDN profiles and endpoints.</span></span>

```powershell
# Create a new profile
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US"

# Create a new endpoint
New-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US" | New-AzureRmCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a><span data-ttu-id="8fa93-126">Kontrollera tillgänglighet för slutpunkt-namn</span><span class="sxs-lookup"><span data-stu-id="8fa93-126">Checking endpoint name availability</span></span>
<span data-ttu-id="8fa93-127">`Get-AzureRmCdnEndpointNameAvailability`Returnerar ett objekt som anger om det finns en namnet på slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="8fa93-127">`Get-AzureRmCdnEndpointNameAvailability` returns an object indicating if an endpoint name is available.</span></span>

```powershell
# Retrieve availability
$availability = Get-AzureRmCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message to the console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a><span data-ttu-id="8fa93-128">Lägga till en anpassad domän</span><span class="sxs-lookup"><span data-stu-id="8fa93-128">Adding a custom domain</span></span>
<span data-ttu-id="8fa93-129">`New-AzureRmCdnCustomDomain`lägger till ett anpassat domännamn i en befintlig slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="8fa93-129">`New-AzureRmCdnCustomDomain` adds a custom domain name to an existing endpoint.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8fa93-130">Du måste ställa in CNAME hos din DNS-leverantör som beskrivs i [hur du mappar en anpassad domän till innehåll innehållsleveransnätverk (CDN) slutpunkt](cdn-map-content-to-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="8fa93-130">You must set up the CNAME with your DNS provider as described in [How to map Custom Domain to Content Delivery Network (CDN) endpoint](cdn-map-content-to-custom-domain.md).</span></span>  <span data-ttu-id="8fa93-131">Du kan testa mappningen innan du ändrar slutpunkten med `Test-AzureRmCdnCustomDomain`.</span><span class="sxs-lookup"><span data-stu-id="8fa93-131">You can test the mapping before modifying your endpoint using `Test-AzureRmCdnCustomDomain`.</span></span>
> 
> 

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Check the mapping
$result = Test-AzureRmCdnCustomDomain -CdnEndpoint $endpoint -CustomDomainHostName "cdn.contoso.com"

# Create the custom domain on the endpoint
If($result.CustomDomainValidated){ New-AzureRmCdnCustomDomain -CustomDomainName Contoso -HostName "cdn.contoso.com" -CdnEndpoint $endpoint }
```

## <a name="modifying-an-endpoint"></a><span data-ttu-id="8fa93-132">Ändra en slutpunkt</span><span class="sxs-lookup"><span data-stu-id="8fa93-132">Modifying an endpoint</span></span>
<span data-ttu-id="8fa93-133">`Set-AzureRmCdnEndpoint`ändrar en befintlig slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="8fa93-133">`Set-AzureRmCdnEndpoint` modifies an existing endpoint.</span></span>

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save the changed endpoint and apply the changes
Set-AzureRmCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a><span data-ttu-id="8fa93-134">Rensa/Förproduktion-loading CDN tillgångar</span><span class="sxs-lookup"><span data-stu-id="8fa93-134">Purging/Pre-loading CDN assets</span></span>
<span data-ttu-id="8fa93-135">`Unpublish-AzureRmCdnEndpointContent`återställningspunkter cachelagras tillgångar, medan `Publish-AzureRmCdnEndpointContent` före läser in tillgångar på slutpunkter som stöds.</span><span class="sxs-lookup"><span data-stu-id="8fa93-135">`Unpublish-AzureRmCdnEndpointContent` purges cached assets, while `Publish-AzureRmCdnEndpointContent` pre-loads assets on supported endpoints.</span></span>

```powershell
# Purge some assets.
Unpublish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Unpublish-AzureRmCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a><span data-ttu-id="8fa93-136">Starta/Stoppa CDN-slutpunkter</span><span class="sxs-lookup"><span data-stu-id="8fa93-136">Starting/Stopping CDN endpoints</span></span>
<span data-ttu-id="8fa93-137">`Start-AzureRmCdnEndpoint`och `Stop-AzureRmCdnEndpoint` kan användas för att starta och stoppa enskilda slutpunkter eller grupper av slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="8fa93-137">`Start-AzureRmCdnEndpoint` and `Stop-AzureRmCdnEndpoint` can be used to start and stop individual endpoints or groups of endpoints.</span></span>

```powershell
# Stop the cdndocdemo endpoint
Stop-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Stop-AzureRmCdnEndpoint

# Start all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Start-AzureRmCdnEndpoint
```

## <a name="deleting-cdn-resources"></a><span data-ttu-id="8fa93-138">Ta bort CDN-resurser</span><span class="sxs-lookup"><span data-stu-id="8fa93-138">Deleting CDN resources</span></span>
<span data-ttu-id="8fa93-139">`Remove-AzureRmCdnProfile`och `Remove-AzureRmCdnEndpoint` kan användas för att ta bort profiler och slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="8fa93-139">`Remove-AzureRmCdnProfile` and `Remove-AzureRmCdnEndpoint` can be used to remove profiles and endpoints.</span></span>

```powershell
# Remove a single endpoint
Remove-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all the endpoints on a profile and skip confirmation (-Force)
Get-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzureRmCdnEndpoint | Remove-AzureRmCdnEndpoint -Force

# Remove a single profile
Remove-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a><span data-ttu-id="8fa93-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8fa93-140">Next Steps</span></span>
<span data-ttu-id="8fa93-141">Läs mer om hur man automatiserar Azure CDN med [.NET](cdn-app-dev-net.md) eller [Node.js](cdn-app-dev-node.md).</span><span class="sxs-lookup"><span data-stu-id="8fa93-141">Learn how to automate Azure CDN with [.NET](cdn-app-dev-net.md) or [Node.js](cdn-app-dev-node.md).</span></span>

<span data-ttu-id="8fa93-142">Läs om CDN funktioner i [CDN-översikt](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8fa93-142">To learn about CDN features, see [CDN Overview](cdn-overview.md).</span></span>

