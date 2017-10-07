---
title: aaaManage Azure Media Services-konton med PowerShell
description: "Lär dig hur toomanage Azure Media Services-konton med PowerShell-cmdlets."
author: Juliako
manager: erikre
editor: 
services: media-services
documentationcenter: 
ms.assetid: 17a10c25-d94f-421c-b6bc-ae0958e2ac96
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2016
ms.author: juliako
ms.openlocfilehash: e8f97bb2393343e45fabf9c437b4fc09f2525dc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-media-services-accounts-with-powershell"></a><span data-ttu-id="54540-103">Hantera Azure Media Services-konton med PowerShell</span><span class="sxs-lookup"><span data-stu-id="54540-103">Manage Azure Media Services Accounts with PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="54540-104">Portal</span><span class="sxs-lookup"><span data-stu-id="54540-104">Portal</span></span>](media-services-portal-create-account.md)
> * [<span data-ttu-id="54540-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="54540-105">PowerShell</span></span>](media-services-manage-with-powershell.md)
> * [<span data-ttu-id="54540-106">REST</span><span class="sxs-lookup"><span data-stu-id="54540-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/mediaservice)
> 
> [!NOTE]
> <span data-ttu-id="54540-107">toobe kan toocreate ett Azure Media Services-konto, måste du ha ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="54540-107">toobe able toocreate an Azure Media Services account, you must have an Azure account.</span></span> <span data-ttu-id="54540-108">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="54540-108">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="54540-109">Mer information om den <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">kostnadsfria utvärderingsversionen av Azure</a>.</span><span class="sxs-lookup"><span data-stu-id="54540-109">For details, see <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Azure Free Trial</a>.</span></span>
> 
> 

## <a name="overview"></a><span data-ttu-id="54540-110">Översikt</span><span class="sxs-lookup"><span data-stu-id="54540-110">Overview</span></span>
<span data-ttu-id="54540-111">Den här artikeln innehåller hello Azure PowerShell-cmdlets för Azure Media Services (AMS) hello Azure Resource Manager framework.</span><span class="sxs-lookup"><span data-stu-id="54540-111">This article lists hello Azure PowerShell cmdlets for Azure Media Services (AMS) in hello Azure Resource Manager framework.</span></span> <span data-ttu-id="54540-112">hello-cmdlets finns i hello **Microsoft.Azure.Commands.Media** namnområde.</span><span class="sxs-lookup"><span data-stu-id="54540-112">hello cmdlets exist in hello **Microsoft.Azure.Commands.Media** namespace.</span></span>

## <a name="versions"></a><span data-ttu-id="54540-113">Versioner</span><span class="sxs-lookup"><span data-stu-id="54540-113">Versions</span></span>
<span data-ttu-id="54540-114">**ApiVersion**: ”2015-10-01”</span><span class="sxs-lookup"><span data-stu-id="54540-114">**ApiVersion**:   "2015-10-01"</span></span>

## <a name="new-azurermmediaservice"></a><span data-ttu-id="54540-115">New-AzureRmMediaService</span><span class="sxs-lookup"><span data-stu-id="54540-115">New-AzureRmMediaService</span></span>
<span data-ttu-id="54540-116">Skapar en medietjänst.</span><span class="sxs-lookup"><span data-stu-id="54540-116">Creates a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="54540-117">Syntax</span><span class="sxs-lookup"><span data-stu-id="54540-117">Syntax</span></span>
<span data-ttu-id="54540-118">Parameteruppsättningen: StorageAccountIdParamSet</span><span class="sxs-lookup"><span data-stu-id="54540-118">Parameter Set: StorageAccountIdParamSet</span></span>

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccountId] <string> [-Tags <hashtable>]  [<CommonParameters>]

<span data-ttu-id="54540-119">Parameteruppsättningen: StorageAccountsParamSet</span><span class="sxs-lookup"><span data-stu-id="54540-119">Parameter Set: StorageAccountsParamSet</span></span>

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccounts] <PSStorageAccount[]> [-Tags <hashtable>]  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="54540-120">Parametrar</span><span class="sxs-lookup"><span data-stu-id="54540-120">Parameters</span></span>
<span data-ttu-id="54540-121">**-ResourceGroupName &lt;sträng&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-121">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="54540-122">Anger hello hello resurs grupp toowhich media tjänsten tillhör namn.</span><span class="sxs-lookup"><span data-stu-id="54540-122">Specifies hello name of hello resource group toowhich this media service belongs.</span></span>

| <span data-ttu-id="54540-123">Alias</span><span class="sxs-lookup"><span data-stu-id="54540-123">Aliases</span></span> | <span data-ttu-id="54540-124">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-124">none</span></span> |
| --- | --- |
| <span data-ttu-id="54540-125">Krävs?</span><span class="sxs-lookup"><span data-stu-id="54540-125">Required?</span></span> |<span data-ttu-id="54540-126">SANT</span><span class="sxs-lookup"><span data-stu-id="54540-126">true</span></span> |
| <span data-ttu-id="54540-127">Placera?</span><span class="sxs-lookup"><span data-stu-id="54540-127">Position?</span></span> |<span data-ttu-id="54540-128">0</span><span class="sxs-lookup"><span data-stu-id="54540-128">0</span></span> |
| <span data-ttu-id="54540-129">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="54540-129">Default value</span></span> |<span data-ttu-id="54540-130">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-130">none</span></span> |
| <span data-ttu-id="54540-131">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="54540-131">Accept pipeline input?</span></span> |<span data-ttu-id="54540-132">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="54540-132">true(ByPropertyName)</span></span> |
| <span data-ttu-id="54540-133">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="54540-133">Accept wildcard characters?</span></span> |<span data-ttu-id="54540-134">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-134">false</span></span> |

<span data-ttu-id="54540-135">**-AccountName &lt;sträng&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-135">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="54540-136">Anger hello för hello media service.</span><span class="sxs-lookup"><span data-stu-id="54540-136">Specifies hello name of hello media service.</span></span>

| <span data-ttu-id="54540-137">Alias</span><span class="sxs-lookup"><span data-stu-id="54540-137">Aliases</span></span> | <span data-ttu-id="54540-138">Namn</span><span class="sxs-lookup"><span data-stu-id="54540-138">Name</span></span> |
| --- | --- |
| <span data-ttu-id="54540-139">Krävs?</span><span class="sxs-lookup"><span data-stu-id="54540-139">Required?</span></span> |<span data-ttu-id="54540-140">SANT</span><span class="sxs-lookup"><span data-stu-id="54540-140">true</span></span> |
| <span data-ttu-id="54540-141">Placera?</span><span class="sxs-lookup"><span data-stu-id="54540-141">Position?</span></span> |<span data-ttu-id="54540-142">1</span><span class="sxs-lookup"><span data-stu-id="54540-142">1</span></span> |
| <span data-ttu-id="54540-143">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="54540-143">Default value</span></span> |<span data-ttu-id="54540-144">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-144">none</span></span> |
| <span data-ttu-id="54540-145">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="54540-145">Accept pipeline input?</span></span> |<span data-ttu-id="54540-146">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-146">false</span></span> |
| <span data-ttu-id="54540-147">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="54540-147">Accept wildcard characters?</span></span> |<span data-ttu-id="54540-148">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-148">false</span></span> |

<span data-ttu-id="54540-149">**-Plats &lt;sträng&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-149">**-Location &lt;String&gt;**</span></span>

<span data-ttu-id="54540-150">Anger hello resursplats av hello media service.</span><span class="sxs-lookup"><span data-stu-id="54540-150">Specifies hello resource location of hello media service.</span></span>

| <span data-ttu-id="54540-151">Alias</span><span class="sxs-lookup"><span data-stu-id="54540-151">Aliases</span></span> | <span data-ttu-id="54540-152">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-152">none</span></span> |
| --- | --- |
| <span data-ttu-id="54540-153">Krävs?</span><span class="sxs-lookup"><span data-stu-id="54540-153">Required?</span></span> |<span data-ttu-id="54540-154">SANT</span><span class="sxs-lookup"><span data-stu-id="54540-154">true</span></span> |
| <span data-ttu-id="54540-155">Placera?</span><span class="sxs-lookup"><span data-stu-id="54540-155">Position?</span></span> |<span data-ttu-id="54540-156">2</span><span class="sxs-lookup"><span data-stu-id="54540-156">2</span></span> |
| <span data-ttu-id="54540-157">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="54540-157">Default value</span></span> |<span data-ttu-id="54540-158">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-158">none</span></span> |
| <span data-ttu-id="54540-159">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="54540-159">Accept pipeline input?</span></span> |<span data-ttu-id="54540-160">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="54540-160">true(ByPropertyName)</span></span> |
| <span data-ttu-id="54540-161">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="54540-161">Accept wildcard characters?</span></span> |<span data-ttu-id="54540-162">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-162">false</span></span> |

<span data-ttu-id="54540-163">**-StorageAccountId &lt;sträng&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-163">**-StorageAccountId &lt;String&gt;**</span></span>

<span data-ttu-id="54540-164">Anger en primär storage-konto som tillhör hello media service.</span><span class="sxs-lookup"><span data-stu-id="54540-164">Specifies a primary storage account that associated with hello media service.</span></span>

* <span data-ttu-id="54540-165">Nytt lagringskonto (som skapats med hello Resource Manager API) stöds endast.</span><span class="sxs-lookup"><span data-stu-id="54540-165">New storage account (created with hello Resource Manager API) supported only.</span></span>
* <span data-ttu-id="54540-166">hello storage-konto måste finnas och har hello hello media service samma plats.</span><span class="sxs-lookup"><span data-stu-id="54540-166">hello storage account must exist and has hello same location with hello media service.</span></span>

| <span data-ttu-id="54540-167">Alias</span><span class="sxs-lookup"><span data-stu-id="54540-167">Aliases</span></span> | <span data-ttu-id="54540-168">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-168">none</span></span> |
| --- | --- |
| <span data-ttu-id="54540-169">Krävs?</span><span class="sxs-lookup"><span data-stu-id="54540-169">Required?</span></span> |<span data-ttu-id="54540-170">SANT</span><span class="sxs-lookup"><span data-stu-id="54540-170">true</span></span> |
| <span data-ttu-id="54540-171">Placera?</span><span class="sxs-lookup"><span data-stu-id="54540-171">Position?</span></span> |<span data-ttu-id="54540-172">3</span><span class="sxs-lookup"><span data-stu-id="54540-172">3</span></span> |
| <span data-ttu-id="54540-173">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="54540-173">Default value</span></span> |<span data-ttu-id="54540-174">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-174">none</span></span> |
| <span data-ttu-id="54540-175">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="54540-175">Accept pipeline input?</span></span> |<span data-ttu-id="54540-176">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="54540-176">true(ByPropertyName)</span></span> |
| <span data-ttu-id="54540-177">Ange namn för parametern</span><span class="sxs-lookup"><span data-stu-id="54540-177">Parameter set name</span></span> |<span data-ttu-id="54540-178">StorageAccountIdParamSet</span><span class="sxs-lookup"><span data-stu-id="54540-178">StorageAccountIdParamSet</span></span> |
| <span data-ttu-id="54540-179">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="54540-179">Accept wildcard characters?</span></span> |<span data-ttu-id="54540-180">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-180">false</span></span> |

<span data-ttu-id="54540-181">**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-181">**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**</span></span>

<span data-ttu-id="54540-182">Anger storage-konton som är associerade med hello media service.</span><span class="sxs-lookup"><span data-stu-id="54540-182">Specifies storage accounts that associated with hello media service.</span></span>

* <span data-ttu-id="54540-183">Nytt lagringskonto (som skapats med hello Resource Manager API) stöds endast.</span><span class="sxs-lookup"><span data-stu-id="54540-183">New storage account (created with hello Resource Manager API) supported only.</span></span>
* <span data-ttu-id="54540-184">hello storage-konto måste finnas och har hello hello media service samma plats.</span><span class="sxs-lookup"><span data-stu-id="54540-184">hello storage account must exist and has hello same location with hello media service.</span></span>
* <span data-ttu-id="54540-185">Endast ett lagringskonto kan anges som primär.</span><span class="sxs-lookup"><span data-stu-id="54540-185">Only one storage account can be specified as primary.</span></span>

| <span data-ttu-id="54540-186">Alias</span><span class="sxs-lookup"><span data-stu-id="54540-186">Aliases</span></span> | <span data-ttu-id="54540-187">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-187">none</span></span> |
| --- | --- |
| <span data-ttu-id="54540-188">Krävs?</span><span class="sxs-lookup"><span data-stu-id="54540-188">Required?</span></span> |<span data-ttu-id="54540-189">SANT</span><span class="sxs-lookup"><span data-stu-id="54540-189">true</span></span> |
| <span data-ttu-id="54540-190">Placera?</span><span class="sxs-lookup"><span data-stu-id="54540-190">Position?</span></span> |<span data-ttu-id="54540-191">3</span><span class="sxs-lookup"><span data-stu-id="54540-191">3</span></span> |
| <span data-ttu-id="54540-192">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="54540-192">Default value</span></span> |<span data-ttu-id="54540-193">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-193">none</span></span> |
| <span data-ttu-id="54540-194">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="54540-194">Accept pipeline input?</span></span> |<span data-ttu-id="54540-195">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="54540-195">true(ByPropertyName)</span></span> |
| <span data-ttu-id="54540-196">Ange namn för parametern</span><span class="sxs-lookup"><span data-stu-id="54540-196">Parameter set name</span></span> |<span data-ttu-id="54540-197">StorageAccountsParamSet</span><span class="sxs-lookup"><span data-stu-id="54540-197">StorageAccountsParamSet</span></span> |
| <span data-ttu-id="54540-198">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="54540-198">Accept wildcard characters?</span></span> |<span data-ttu-id="54540-199">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-199">false</span></span> |

<span data-ttu-id="54540-200">**-Taggar &lt;hash-tabell&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-200">**-Tags &lt;Hashtable&gt;**</span></span>

<span data-ttu-id="54540-201">Anger en hash-tabell hello-taggar som är associerade med hello media service.</span><span class="sxs-lookup"><span data-stu-id="54540-201">Specifies a hash table of hello tags that are associated with hello media service.</span></span>

* <span data-ttu-id="54540-202">Exempel: @{”tag1” = ”value1” ”; tag2 ”=: value2”}</span><span class="sxs-lookup"><span data-stu-id="54540-202">Example: @{"tag1"="value1";"tag2"=:value2"}</span></span>

| <span data-ttu-id="54540-203">Alias</span><span class="sxs-lookup"><span data-stu-id="54540-203">Aliases</span></span> | <span data-ttu-id="54540-204">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-204">none</span></span> |
| --- | --- |
| <span data-ttu-id="54540-205">Krävs?</span><span class="sxs-lookup"><span data-stu-id="54540-205">Required?</span></span> |<span data-ttu-id="54540-206">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-206">false</span></span> |
| <span data-ttu-id="54540-207">Placera?</span><span class="sxs-lookup"><span data-stu-id="54540-207">Position?</span></span> |<span data-ttu-id="54540-208">Med namnet</span><span class="sxs-lookup"><span data-stu-id="54540-208">named</span></span> |
| <span data-ttu-id="54540-209">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="54540-209">Default value</span></span> |<span data-ttu-id="54540-210">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-210">none</span></span> |
| <span data-ttu-id="54540-211">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="54540-211">Accept pipeline input?</span></span> |<span data-ttu-id="54540-212">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-212">false</span></span> |
| <span data-ttu-id="54540-213">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="54540-213">Accept wildcard characters?</span></span> |<span data-ttu-id="54540-214">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-214">false</span></span> |

<span data-ttu-id="54540-215">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-215">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="54540-216">Denna cmdlet stöder hello vanliga parametrar:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction och -WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="54540-216">This cmdlet supports hello common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="54540-217">Indata</span><span class="sxs-lookup"><span data-stu-id="54540-217">Inputs</span></span>
<span data-ttu-id="54540-218">hello indatatypen är hello typ av hello objekt kan du skicka toohello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="54540-218">hello input type is hello type of hello objects that you can pipe toohello cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="54540-219">utdata</span><span class="sxs-lookup"><span data-stu-id="54540-219">Outputs</span></span>
<span data-ttu-id="54540-220">hello utdatatypen är hello typ av hello-objekt som hello cmdleten genererar.</span><span class="sxs-lookup"><span data-stu-id="54540-220">hello output type is hello type of hello objects that hello cmdlet emits.</span></span>

## <a name="set-azurermmediaservice"></a><span data-ttu-id="54540-221">Set-AzureRmMediaService</span><span class="sxs-lookup"><span data-stu-id="54540-221">Set-AzureRmMediaService</span></span>
<span data-ttu-id="54540-222">Uppdaterar en medietjänst.</span><span class="sxs-lookup"><span data-stu-id="54540-222">Updates a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="54540-223">Syntax</span><span class="sxs-lookup"><span data-stu-id="54540-223">Syntax</span></span>
    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="54540-224">Parametrar</span><span class="sxs-lookup"><span data-stu-id="54540-224">Parameters</span></span>
<span data-ttu-id="54540-225">**-ResourceGroupName &lt;sträng&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-225">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="54540-226">Anger hello hello resurs grupp toowhich media tjänsten tillhör namn.</span><span class="sxs-lookup"><span data-stu-id="54540-226">Specifies hello name of hello resource group toowhich this media service belongs.</span></span>

| <span data-ttu-id="54540-227">Alias</span><span class="sxs-lookup"><span data-stu-id="54540-227">Aliases</span></span> | <span data-ttu-id="54540-228">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-228">none</span></span> |
| --- | --- |
| <span data-ttu-id="54540-229">Krävs?</span><span class="sxs-lookup"><span data-stu-id="54540-229">Required?</span></span> |<span data-ttu-id="54540-230">SANT</span><span class="sxs-lookup"><span data-stu-id="54540-230">true</span></span> |
| <span data-ttu-id="54540-231">Placera?</span><span class="sxs-lookup"><span data-stu-id="54540-231">Position?</span></span> |<span data-ttu-id="54540-232">0</span><span class="sxs-lookup"><span data-stu-id="54540-232">0</span></span> |
| <span data-ttu-id="54540-233">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="54540-233">Default Value</span></span> |<span data-ttu-id="54540-234">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-234">none</span></span> |
| <span data-ttu-id="54540-235">Acceptera indata från Pipeline?</span><span class="sxs-lookup"><span data-stu-id="54540-235">Accept Pipeline Input?</span></span> |<span data-ttu-id="54540-236">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="54540-236">true(ByPropertyName)</span></span> |
| <span data-ttu-id="54540-237">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="54540-237">Accept wildcard characters?</span></span> |<span data-ttu-id="54540-238">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-238">false</span></span> |

<span data-ttu-id="54540-239">**-AccountName &lt;sträng&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-239">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="54540-240">Anger hello för hello media service.</span><span class="sxs-lookup"><span data-stu-id="54540-240">Specifies hello name of hello media service.</span></span>

| <span data-ttu-id="54540-241">Alias</span><span class="sxs-lookup"><span data-stu-id="54540-241">Aliases</span></span> | <span data-ttu-id="54540-242">Namn</span><span class="sxs-lookup"><span data-stu-id="54540-242">Name</span></span> |
| --- | --- |
| <span data-ttu-id="54540-243">Krävs?</span><span class="sxs-lookup"><span data-stu-id="54540-243">Required?</span></span> |<span data-ttu-id="54540-244">True</span><span class="sxs-lookup"><span data-stu-id="54540-244">True</span></span> |
| <span data-ttu-id="54540-245">Placera?</span><span class="sxs-lookup"><span data-stu-id="54540-245">Position?</span></span> |<span data-ttu-id="54540-246">1</span><span class="sxs-lookup"><span data-stu-id="54540-246">1</span></span> |
| <span data-ttu-id="54540-247">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="54540-247">Default value</span></span> |<span data-ttu-id="54540-248">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-248">None</span></span> |
| <span data-ttu-id="54540-249">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="54540-249">Accept pipeline input?</span></span> |<span data-ttu-id="54540-250">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="54540-250">true(ByPropertyName)</span></span> |
| <span data-ttu-id="54540-251">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="54540-251">Accept wildcard characters?</span></span> |<span data-ttu-id="54540-252">False</span><span class="sxs-lookup"><span data-stu-id="54540-252">False</span></span> |

<span data-ttu-id="54540-253">**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-253">**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**</span></span>

<span data-ttu-id="54540-254">Anger storage-konton som är associerade med hello media service.</span><span class="sxs-lookup"><span data-stu-id="54540-254">Specifies storage accounts that associated with hello media service.</span></span>

* <span data-ttu-id="54540-255">Nytt lagringskonto (som skapats med hello Resource Manager API) stöds endast.</span><span class="sxs-lookup"><span data-stu-id="54540-255">New storage account (created with hello Resource Manager API) supported only.</span></span>
* <span data-ttu-id="54540-256">hello storage-konto måste finnas och har hello hello media service samma plats.</span><span class="sxs-lookup"><span data-stu-id="54540-256">hello storage account must exist and has hello same location with hello media service.</span></span>
* <span data-ttu-id="54540-257">Endast ett lagringskonto kan anges som primär.</span><span class="sxs-lookup"><span data-stu-id="54540-257">Only one storage account can be specified as primary.</span></span>

| <span data-ttu-id="54540-258">Alias</span><span class="sxs-lookup"><span data-stu-id="54540-258">Aliases</span></span> | <span data-ttu-id="54540-259">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-259">none</span></span> |
| --- | --- |
| <span data-ttu-id="54540-260">Krävs?</span><span class="sxs-lookup"><span data-stu-id="54540-260">Required?</span></span> |<span data-ttu-id="54540-261">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-261">false</span></span> |
| <span data-ttu-id="54540-262">Placera?</span><span class="sxs-lookup"><span data-stu-id="54540-262">Position?</span></span> |<span data-ttu-id="54540-263">Med namnet</span><span class="sxs-lookup"><span data-stu-id="54540-263">Named</span></span> |
| <span data-ttu-id="54540-264">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="54540-264">Default value</span></span> |<span data-ttu-id="54540-265">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-265">none</span></span> |
| <span data-ttu-id="54540-266">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="54540-266">Accept pipeline input?</span></span> |<span data-ttu-id="54540-267">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="54540-267">true(ByPropertyName)</span></span> |
| <span data-ttu-id="54540-268">Ange namn för parametern</span><span class="sxs-lookup"><span data-stu-id="54540-268">Parameter set name</span></span> |<span data-ttu-id="54540-269">StorageAccountsParamSet</span><span class="sxs-lookup"><span data-stu-id="54540-269">StorageAccountsParamSet</span></span> |
| <span data-ttu-id="54540-270">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="54540-270">Accept wildcard characters?</span></span> |<span data-ttu-id="54540-271">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-271">false</span></span> |

<span data-ttu-id="54540-272">**-Taggar &lt;hash-tabell&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-272">**-Tags &lt;Hashtable&gt;**</span></span>

<span data-ttu-id="54540-273">Anger en hash-tabell hello-taggar som är associerade med den här media-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="54540-273">Specifies a hash table of hello tags that are associated with this media service.</span></span>

* <span data-ttu-id="54540-274">hello-taggar som är associerade med hello media service ersätts med värdet som anges av hello kunden.</span><span class="sxs-lookup"><span data-stu-id="54540-274">hello tags that are associated with hello media service are replaced with value specified by hello customer.</span></span>

| <span data-ttu-id="54540-275">Alias</span><span class="sxs-lookup"><span data-stu-id="54540-275">Aliases</span></span> | <span data-ttu-id="54540-276">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-276">none</span></span> |
| --- | --- |
| <span data-ttu-id="54540-277">Krävs?</span><span class="sxs-lookup"><span data-stu-id="54540-277">Required?</span></span> |<span data-ttu-id="54540-278">False</span><span class="sxs-lookup"><span data-stu-id="54540-278">False</span></span> |
| <span data-ttu-id="54540-279">Placera?</span><span class="sxs-lookup"><span data-stu-id="54540-279">Position?</span></span> |<span data-ttu-id="54540-280">Med namnet</span><span class="sxs-lookup"><span data-stu-id="54540-280">Named</span></span> |
| <span data-ttu-id="54540-281">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="54540-281">Default value</span></span> |<span data-ttu-id="54540-282">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-282">None</span></span> |
| <span data-ttu-id="54540-283">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="54540-283">Accept pipeline input?</span></span> |<span data-ttu-id="54540-284">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="54540-284">true(ByPropertyName)</span></span> |
| <span data-ttu-id="54540-285">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="54540-285">Accept wildcard characters?</span></span> |<span data-ttu-id="54540-286">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-286">false</span></span> |

<span data-ttu-id="54540-287">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-287">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="54540-288">Denna cmdlet stöder hello vanliga parametrar:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction och -WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="54540-288">This cmdlet supports hello common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="54540-289">Indata</span><span class="sxs-lookup"><span data-stu-id="54540-289">Inputs</span></span>
<span data-ttu-id="54540-290">hello indatatypen är hello typ av hello objekt kan du skicka toohello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="54540-290">hello input type is hello type of hello objects that you can pipe toohello cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="54540-291">utdata</span><span class="sxs-lookup"><span data-stu-id="54540-291">Outputs</span></span>
<span data-ttu-id="54540-292">hello utdatatypen är hello typ av hello-objekt som hello cmdleten genererar.</span><span class="sxs-lookup"><span data-stu-id="54540-292">hello output type is hello type of hello objects that hello cmdlet emits.</span></span>

## <a name="remove-azurermmediaservice"></a><span data-ttu-id="54540-293">Remove-AzureRmMediaService</span><span class="sxs-lookup"><span data-stu-id="54540-293">Remove-AzureRmMediaService</span></span>
<span data-ttu-id="54540-294">Tar bort en medietjänst.</span><span class="sxs-lookup"><span data-stu-id="54540-294">Removes a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="54540-295">Syntax</span><span class="sxs-lookup"><span data-stu-id="54540-295">Syntax</span></span>
    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="54540-296">Parametrar</span><span class="sxs-lookup"><span data-stu-id="54540-296">Parameters</span></span>
<span data-ttu-id="54540-297">**-ResourceGroupName &lt;sträng&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-297">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="54540-298">Anger hello hello resurs grupp toowhich media tjänsten tillhör namn.</span><span class="sxs-lookup"><span data-stu-id="54540-298">Specifies hello name of hello resource group toowhich this media service belongs.</span></span>

| <span data-ttu-id="54540-299">Alias</span><span class="sxs-lookup"><span data-stu-id="54540-299">Aliases</span></span> | <span data-ttu-id="54540-300">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-300">none</span></span> |
| --- | --- |
| <span data-ttu-id="54540-301">Krävs?</span><span class="sxs-lookup"><span data-stu-id="54540-301">Required?</span></span> |<span data-ttu-id="54540-302">SANT</span><span class="sxs-lookup"><span data-stu-id="54540-302">true</span></span> |
| <span data-ttu-id="54540-303">Placera?</span><span class="sxs-lookup"><span data-stu-id="54540-303">Position?</span></span> |<span data-ttu-id="54540-304">0</span><span class="sxs-lookup"><span data-stu-id="54540-304">0</span></span> |
| <span data-ttu-id="54540-305">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="54540-305">Default value</span></span> |<span data-ttu-id="54540-306">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-306">none</span></span> |
| <span data-ttu-id="54540-307">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="54540-307">Accept pipeline input?</span></span> |<span data-ttu-id="54540-308">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="54540-308">true(ByPropertyName)</span></span> |
| <span data-ttu-id="54540-309">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="54540-309">Accept wildcard characters?</span></span> |<span data-ttu-id="54540-310">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-310">false</span></span> |

<span data-ttu-id="54540-311">**-AccountName &lt;sträng&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-311">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="54540-312">Anger hello för hello media service.</span><span class="sxs-lookup"><span data-stu-id="54540-312">Specifies hello name of hello media service.</span></span>

| <span data-ttu-id="54540-313">Alias</span><span class="sxs-lookup"><span data-stu-id="54540-313">Aliases</span></span> | <span data-ttu-id="54540-314">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-314">none</span></span> |
| --- | --- |
| <span data-ttu-id="54540-315">Krävs?</span><span class="sxs-lookup"><span data-stu-id="54540-315">Required?</span></span> |<span data-ttu-id="54540-316">SANT</span><span class="sxs-lookup"><span data-stu-id="54540-316">true</span></span> |
| <span data-ttu-id="54540-317">Placera?</span><span class="sxs-lookup"><span data-stu-id="54540-317">Position?</span></span> |<span data-ttu-id="54540-318">2</span><span class="sxs-lookup"><span data-stu-id="54540-318">2</span></span> |
| <span data-ttu-id="54540-319">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="54540-319">Default value</span></span> |<span data-ttu-id="54540-320">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-320">None</span></span> |
| <span data-ttu-id="54540-321">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="54540-321">Accept pipeline input?</span></span> |<span data-ttu-id="54540-322">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="54540-322">true(ByPropertyName)</span></span> |
| <span data-ttu-id="54540-323">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="54540-323">Accept wildcard characters?</span></span> |<span data-ttu-id="54540-324">False</span><span class="sxs-lookup"><span data-stu-id="54540-324">False</span></span> |

<span data-ttu-id="54540-325">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-325">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="54540-326">Denna cmdlet stöder hello vanliga parametrar:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction och -WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="54540-326">This cmdlet supports hello common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="54540-327">Indata</span><span class="sxs-lookup"><span data-stu-id="54540-327">Inputs</span></span>
<span data-ttu-id="54540-328">hello indatatypen är hello typ av hello objekt kan du skicka toohello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="54540-328">hello input type is hello type of hello objects that you can pipe toohello cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="54540-329">utdata</span><span class="sxs-lookup"><span data-stu-id="54540-329">Outputs</span></span>
<span data-ttu-id="54540-330">hello utdatatypen är hello typ av hello-objekt som hello cmdleten genererar.</span><span class="sxs-lookup"><span data-stu-id="54540-330">hello output type is hello type of hello objects that hello cmdlet emits.</span></span>

## <a name="get-azurermmediaservice"></a><span data-ttu-id="54540-331">Get-AzureRmMediaService</span><span class="sxs-lookup"><span data-stu-id="54540-331">Get-AzureRmMediaService</span></span>
<span data-ttu-id="54540-332">Hämtar alla media services i en resursgrupp eller en medietjänst med ett angivet namn.</span><span class="sxs-lookup"><span data-stu-id="54540-332">Gets all media services in a resource group or a media service with a given name.</span></span>

### <a name="syntax"></a><span data-ttu-id="54540-333">Syntax</span><span class="sxs-lookup"><span data-stu-id="54540-333">Syntax</span></span>
<span data-ttu-id="54540-334">ParameterSet: ResourceGroupParameterSet</span><span class="sxs-lookup"><span data-stu-id="54540-334">ParameterSet: ResourceGroupParameterSet</span></span>

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>]    

<span data-ttu-id="54540-335">ParameterSet: AccountNameParameterSet</span><span class="sxs-lookup"><span data-stu-id="54540-335">ParameterSet: AccountNameParameterSet</span></span>

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="54540-336">Parametrar</span><span class="sxs-lookup"><span data-stu-id="54540-336">Parameters</span></span>
<span data-ttu-id="54540-337">**-ResourceGroupName &lt;sträng&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-337">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="54540-338">Anger hello hello resurs grupp toowhich media tjänsten tillhör namn.</span><span class="sxs-lookup"><span data-stu-id="54540-338">Specifies hello name of hello resource group toowhich this media service belongs.</span></span>

| <span data-ttu-id="54540-339">Alias</span><span class="sxs-lookup"><span data-stu-id="54540-339">Aliases</span></span> | <span data-ttu-id="54540-340">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-340">none</span></span> |
| --- | --- |
| <span data-ttu-id="54540-341">Krävs?</span><span class="sxs-lookup"><span data-stu-id="54540-341">Required?</span></span> |<span data-ttu-id="54540-342">SANT</span><span class="sxs-lookup"><span data-stu-id="54540-342">true</span></span> |
| <span data-ttu-id="54540-343">Placera?</span><span class="sxs-lookup"><span data-stu-id="54540-343">Position?</span></span> |<span data-ttu-id="54540-344">0</span><span class="sxs-lookup"><span data-stu-id="54540-344">0</span></span> |
| <span data-ttu-id="54540-345">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="54540-345">Default value</span></span> |<span data-ttu-id="54540-346">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-346">none</span></span> |
| <span data-ttu-id="54540-347">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="54540-347">Accept pipeline input?</span></span> |<span data-ttu-id="54540-348">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="54540-348">true(ByPropertyName)</span></span> |
| <span data-ttu-id="54540-349">Ange namn för parametern</span><span class="sxs-lookup"><span data-stu-id="54540-349">Parameter set name</span></span> |<span data-ttu-id="54540-350">ResourceGroupParameterSet AccountNameParameterSet</span><span class="sxs-lookup"><span data-stu-id="54540-350">ResourceGroupParameterSet, AccountNameParameterSet</span></span> |

<span data-ttu-id="54540-351">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="54540-351">Accept wildcard characters?</span></span>   <span data-ttu-id="54540-352">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-352">false</span></span>

<span data-ttu-id="54540-353">**-AccountName &lt;sträng&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-353">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="54540-354">Anger hello för hello media service.</span><span class="sxs-lookup"><span data-stu-id="54540-354">Specifies hello name of hello media service.</span></span>

| <span data-ttu-id="54540-355">Alias</span><span class="sxs-lookup"><span data-stu-id="54540-355">Aliases</span></span> | <span data-ttu-id="54540-356">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-356">none</span></span> |
| --- | --- |
| <span data-ttu-id="54540-357">Krävs?</span><span class="sxs-lookup"><span data-stu-id="54540-357">Required?</span></span> |<span data-ttu-id="54540-358">SANT</span><span class="sxs-lookup"><span data-stu-id="54540-358">true</span></span> |
| <span data-ttu-id="54540-359">Placera?</span><span class="sxs-lookup"><span data-stu-id="54540-359">Position?</span></span> |<span data-ttu-id="54540-360">1</span><span class="sxs-lookup"><span data-stu-id="54540-360">1</span></span> |
| <span data-ttu-id="54540-361">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="54540-361">Default value</span></span> |<span data-ttu-id="54540-362">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-362">none</span></span> |
| <span data-ttu-id="54540-363">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="54540-363">Accept pipeline input?</span></span> |<span data-ttu-id="54540-364">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="54540-364">true(ByPropertyName)</span></span> |
| <span data-ttu-id="54540-365">Ange namn för parametern</span><span class="sxs-lookup"><span data-stu-id="54540-365">Parameter set name</span></span> |<span data-ttu-id="54540-366">AccountNameParameterSet</span><span class="sxs-lookup"><span data-stu-id="54540-366">AccountNameParameterSet</span></span> |
| <span data-ttu-id="54540-367">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="54540-367">Accept wildcard characters?</span></span> |<span data-ttu-id="54540-368">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-368">false</span></span> |

<span data-ttu-id="54540-369">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-369">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="54540-370">Denna cmdlet stöder hello vanliga parametrar:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction och -WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="54540-370">This cmdlet supports hello common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="54540-371">Indata</span><span class="sxs-lookup"><span data-stu-id="54540-371">Inputs</span></span>
<span data-ttu-id="54540-372">hello indatatypen är hello typ av hello objekt kan du skicka toohello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="54540-372">hello input type is hello type of hello objects that you can pipe toohello cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="54540-373">utdata</span><span class="sxs-lookup"><span data-stu-id="54540-373">Outputs</span></span>
<span data-ttu-id="54540-374">hello utdatatypen är hello typ av hello-objekt som hello cmdleten genererar.</span><span class="sxs-lookup"><span data-stu-id="54540-374">hello output type is hello type of hello objects that hello cmdlet emits.</span></span>

## <a name="get-azurermmediaservicekeys"></a><span data-ttu-id="54540-375">Get-AzureRmMediaServiceKeys</span><span class="sxs-lookup"><span data-stu-id="54540-375">Get-AzureRmMediaServiceKeys</span></span>
<span data-ttu-id="54540-376">Hämtar en medietjänst nycklar.</span><span class="sxs-lookup"><span data-stu-id="54540-376">Gets keys of a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="54540-377">Syntax</span><span class="sxs-lookup"><span data-stu-id="54540-377">Syntax</span></span>
    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="54540-378">Parametrar</span><span class="sxs-lookup"><span data-stu-id="54540-378">Parameters</span></span>
<span data-ttu-id="54540-379">**-ResourceGroupName &lt;sträng&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-379">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="54540-380">Anger hello hello resurs grupp toowhich media tjänsten tillhör namn.</span><span class="sxs-lookup"><span data-stu-id="54540-380">Specifies hello name of hello resource group toowhich this media service belongs.</span></span>

| <span data-ttu-id="54540-381">Alias</span><span class="sxs-lookup"><span data-stu-id="54540-381">Aliases</span></span> | <span data-ttu-id="54540-382">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-382">none</span></span> |
| --- | --- |
| <span data-ttu-id="54540-383">Krävs?</span><span class="sxs-lookup"><span data-stu-id="54540-383">Required?</span></span> |<span data-ttu-id="54540-384">SANT</span><span class="sxs-lookup"><span data-stu-id="54540-384">true</span></span> |
| <span data-ttu-id="54540-385">Placera?</span><span class="sxs-lookup"><span data-stu-id="54540-385">Position?</span></span> |<span data-ttu-id="54540-386">0</span><span class="sxs-lookup"><span data-stu-id="54540-386">0</span></span> |
| <span data-ttu-id="54540-387">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="54540-387">Default value</span></span> |<span data-ttu-id="54540-388">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-388">none</span></span> |
| <span data-ttu-id="54540-389">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="54540-389">Accept pipeline input?</span></span> |<span data-ttu-id="54540-390">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="54540-390">true(ByPropertyName)</span></span> |
| <span data-ttu-id="54540-391">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="54540-391">Accept wildcard characters?</span></span> |<span data-ttu-id="54540-392">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-392">false</span></span> |

<span data-ttu-id="54540-393">**-AccountName &lt;sträng&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-393">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="54540-394">Anger hello för hello media service.</span><span class="sxs-lookup"><span data-stu-id="54540-394">Specifies hello name of hello media service.</span></span>

| <span data-ttu-id="54540-395">Alias</span><span class="sxs-lookup"><span data-stu-id="54540-395">Aliases</span></span> | <span data-ttu-id="54540-396">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-396">none</span></span> |
| --- | --- |
| <span data-ttu-id="54540-397">Krävs?</span><span class="sxs-lookup"><span data-stu-id="54540-397">Required?</span></span> |<span data-ttu-id="54540-398">SANT</span><span class="sxs-lookup"><span data-stu-id="54540-398">true</span></span> |
| <span data-ttu-id="54540-399">Placera?</span><span class="sxs-lookup"><span data-stu-id="54540-399">Position?</span></span> |<span data-ttu-id="54540-400">1</span><span class="sxs-lookup"><span data-stu-id="54540-400">1</span></span> |
| <span data-ttu-id="54540-401">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="54540-401">Default value</span></span> |<span data-ttu-id="54540-402">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-402">none</span></span> |
| <span data-ttu-id="54540-403">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="54540-403">Accept pipeline input?</span></span> |<span data-ttu-id="54540-404">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="54540-404">true(ByPropertyName)</span></span> |
| <span data-ttu-id="54540-405">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="54540-405">Accept wildcard characters?</span></span> |<span data-ttu-id="54540-406">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-406">false</span></span> |

<span data-ttu-id="54540-407">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-407">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="54540-408">Denna cmdlet stöder hello vanliga parametrar:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction och -WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="54540-408">This cmdlet supports hello common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="54540-409">Indata</span><span class="sxs-lookup"><span data-stu-id="54540-409">Inputs</span></span>
<span data-ttu-id="54540-410">hello indatatypen är hello typ av hello objekt kan du skicka toohello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="54540-410">hello input type is hello type of hello objects that you can pipe toohello cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="54540-411">utdata</span><span class="sxs-lookup"><span data-stu-id="54540-411">Outputs</span></span>
<span data-ttu-id="54540-412">hello utdatatypen är hello typ av hello-objekt som hello cmdleten genererar.</span><span class="sxs-lookup"><span data-stu-id="54540-412">hello output type is hello type of hello objects that hello cmdlet emits.</span></span>

## <a name="set-azurermmediaservicekey"></a><span data-ttu-id="54540-413">Set-AzureRmMediaServiceKey</span><span class="sxs-lookup"><span data-stu-id="54540-413">Set-AzureRmMediaServiceKey</span></span>
<span data-ttu-id="54540-414">Återskapar en primär eller sekundär nyckel för en medietjänst.</span><span class="sxs-lookup"><span data-stu-id="54540-414">Regenerates a primary or secondary key of a media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="54540-415">Syntax</span><span class="sxs-lookup"><span data-stu-id="54540-415">Syntax</span></span>
    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="54540-416">Parametrar</span><span class="sxs-lookup"><span data-stu-id="54540-416">Parameters</span></span>
<span data-ttu-id="54540-417">**-ResourceGroupName &lt;sträng&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-417">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="54540-418">Anger hello hello resurs grupp toowhich media tjänsten tillhör namn.</span><span class="sxs-lookup"><span data-stu-id="54540-418">Specifies hello name of hello resource group toowhich this media service belongs.</span></span>

| <span data-ttu-id="54540-419">Alias</span><span class="sxs-lookup"><span data-stu-id="54540-419">Aliases</span></span> | <span data-ttu-id="54540-420">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-420">none</span></span> |
| --- | --- |
| <span data-ttu-id="54540-421">Krävs?</span><span class="sxs-lookup"><span data-stu-id="54540-421">Required?</span></span> |<span data-ttu-id="54540-422">SANT</span><span class="sxs-lookup"><span data-stu-id="54540-422">true</span></span> |
| <span data-ttu-id="54540-423">Placera?</span><span class="sxs-lookup"><span data-stu-id="54540-423">Position?</span></span> |<span data-ttu-id="54540-424">0</span><span class="sxs-lookup"><span data-stu-id="54540-424">0</span></span> |
| <span data-ttu-id="54540-425">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="54540-425">Default value</span></span> |<span data-ttu-id="54540-426">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-426">none</span></span> |
| <span data-ttu-id="54540-427">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="54540-427">Accept pipeline input?</span></span> |<span data-ttu-id="54540-428">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="54540-428">true(ByPropertyName)</span></span> |
| <span data-ttu-id="54540-429">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="54540-429">Accept wildcard characters?</span></span> |<span data-ttu-id="54540-430">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-430">false</span></span> |

<span data-ttu-id="54540-431">**-AccountName &lt;sträng&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-431">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="54540-432">Anger hello för hello media service.</span><span class="sxs-lookup"><span data-stu-id="54540-432">Specifies hello name of hello media service.</span></span>

| <span data-ttu-id="54540-433">Alias</span><span class="sxs-lookup"><span data-stu-id="54540-433">Aliases</span></span> | <span data-ttu-id="54540-434">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-434">none</span></span> |
| --- | --- |
| <span data-ttu-id="54540-435">Krävs?</span><span class="sxs-lookup"><span data-stu-id="54540-435">Required?</span></span> |<span data-ttu-id="54540-436">SANT</span><span class="sxs-lookup"><span data-stu-id="54540-436">true</span></span> |
| <span data-ttu-id="54540-437">Placera?</span><span class="sxs-lookup"><span data-stu-id="54540-437">Position?</span></span> |<span data-ttu-id="54540-438">1</span><span class="sxs-lookup"><span data-stu-id="54540-438">1</span></span> |
| <span data-ttu-id="54540-439">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="54540-439">Default value</span></span> |<span data-ttu-id="54540-440">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-440">none</span></span> |
| <span data-ttu-id="54540-441">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="54540-441">Accept pipeline input?</span></span> |<span data-ttu-id="54540-442">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="54540-442">true(ByPropertyName)</span></span> |
| <span data-ttu-id="54540-443">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="54540-443">Accept wildcard characters?</span></span> |<span data-ttu-id="54540-444">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-444">false</span></span> |

<span data-ttu-id="54540-445">**KeyType - &lt;KeyType&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-445">**-KeyType &lt;KeyType&gt;**</span></span>

<span data-ttu-id="54540-446">Anger hello viktiga hello media service.</span><span class="sxs-lookup"><span data-stu-id="54540-446">Specifies hello key type of hello media service.</span></span>

* <span data-ttu-id="54540-447">Primär eller sekundär</span><span class="sxs-lookup"><span data-stu-id="54540-447">Primary or Secondary</span></span>

| <span data-ttu-id="54540-448">Alias</span><span class="sxs-lookup"><span data-stu-id="54540-448">Aliases</span></span> | <span data-ttu-id="54540-449">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-449">none</span></span> |
| --- | --- |
| <span data-ttu-id="54540-450">Krävs?</span><span class="sxs-lookup"><span data-stu-id="54540-450">Required?</span></span> |<span data-ttu-id="54540-451">SANT</span><span class="sxs-lookup"><span data-stu-id="54540-451">true</span></span> |
| <span data-ttu-id="54540-452">Placera?</span><span class="sxs-lookup"><span data-stu-id="54540-452">Position?</span></span> |<span data-ttu-id="54540-453">2</span><span class="sxs-lookup"><span data-stu-id="54540-453">2</span></span> |
| <span data-ttu-id="54540-454">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="54540-454">Default value</span></span> |<span data-ttu-id="54540-455">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-455">none</span></span> |
| <span data-ttu-id="54540-456">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="54540-456">Accept pipeline input?</span></span> |<span data-ttu-id="54540-457">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-457">false</span></span> |
| <span data-ttu-id="54540-458">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="54540-458">Accept wildcard characters?</span></span> |<span data-ttu-id="54540-459">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-459">false</span></span> |

<span data-ttu-id="54540-460">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-460">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="54540-461">Denna cmdlet stöder hello vanliga parametrar:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction och -WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="54540-461">This cmdlet supports hello common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="54540-462">Indata</span><span class="sxs-lookup"><span data-stu-id="54540-462">Inputs</span></span>
<span data-ttu-id="54540-463">hello indatatypen är hello typ av hello objekt kan du skicka toothe cmdlet.</span><span class="sxs-lookup"><span data-stu-id="54540-463">hello input type is hello type of hello objects that you can pipe toothe cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="54540-464">utdata</span><span class="sxs-lookup"><span data-stu-id="54540-464">Outputs</span></span>
<span data-ttu-id="54540-465">hello utdatatypen är hello typ av hello-objekt som hello cmdleten genererar.</span><span class="sxs-lookup"><span data-stu-id="54540-465">hello output type is hello type of hello objects that hello cmdlet emits.</span></span>

## <a name="sync-azurermmediaservicestoragekeys"></a><span data-ttu-id="54540-466">Sync-AzureRmMediaServiceStorageKeys</span><span class="sxs-lookup"><span data-stu-id="54540-466">Sync-AzureRmMediaServiceStorageKeys</span></span>
<span data-ttu-id="54540-467">Synkroniserar lagringskontonycklar för ett lagringskonto som är associerade med hello media service.</span><span class="sxs-lookup"><span data-stu-id="54540-467">Synchronizes storage account keys for a storage account associated with hello media service.</span></span>

### <a name="syntax"></a><span data-ttu-id="54540-468">Syntax</span><span class="sxs-lookup"><span data-stu-id="54540-468">Syntax</span></span>
    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountId] <string>  [<CommonParameters>]

### <a name="parameters"></a><span data-ttu-id="54540-469">Parametrar</span><span class="sxs-lookup"><span data-stu-id="54540-469">Parameters</span></span>
<span data-ttu-id="54540-470">**-ResourceGroupName &lt;sträng&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-470">**-ResourceGroupName &lt;String&gt;**</span></span>

<span data-ttu-id="54540-471">Anger hello hello resurs grupp toowhich media tjänsten tillhör namn.</span><span class="sxs-lookup"><span data-stu-id="54540-471">Specifies hello name of hello resource group toowhich this media service belongs.</span></span>

| <span data-ttu-id="54540-472">Alias</span><span class="sxs-lookup"><span data-stu-id="54540-472">Aliases</span></span> | <span data-ttu-id="54540-473">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-473">none</span></span> |
| --- | --- |
| <span data-ttu-id="54540-474">Krävs?</span><span class="sxs-lookup"><span data-stu-id="54540-474">Required?</span></span> |<span data-ttu-id="54540-475">SANT</span><span class="sxs-lookup"><span data-stu-id="54540-475">true</span></span> |
| <span data-ttu-id="54540-476">Placera?</span><span class="sxs-lookup"><span data-stu-id="54540-476">Position?</span></span> |<span data-ttu-id="54540-477">0</span><span class="sxs-lookup"><span data-stu-id="54540-477">0</span></span> |
| <span data-ttu-id="54540-478">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="54540-478">Default value</span></span> |<span data-ttu-id="54540-479">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-479">none</span></span> |
| <span data-ttu-id="54540-480">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="54540-480">Accept pipeline input?</span></span> |<span data-ttu-id="54540-481">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="54540-481">true(ByPropertyName)</span></span> |
| <span data-ttu-id="54540-482">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="54540-482">Accept wildcard characters?</span></span> |<span data-ttu-id="54540-483">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-483">false</span></span> |

<span data-ttu-id="54540-484">**-AccountName &lt;sträng&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-484">**-AccountName &lt;String&gt;**</span></span>

<span data-ttu-id="54540-485">Anger hello för hello media service.</span><span class="sxs-lookup"><span data-stu-id="54540-485">Specifies hello name of hello media service.</span></span>

| <span data-ttu-id="54540-486">Alias</span><span class="sxs-lookup"><span data-stu-id="54540-486">Aliases</span></span> | <span data-ttu-id="54540-487">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-487">none</span></span> |
| --- | --- |
| <span data-ttu-id="54540-488">Krävs?</span><span class="sxs-lookup"><span data-stu-id="54540-488">Required?</span></span> |<span data-ttu-id="54540-489">SANT</span><span class="sxs-lookup"><span data-stu-id="54540-489">true</span></span> |
| <span data-ttu-id="54540-490">Placera?</span><span class="sxs-lookup"><span data-stu-id="54540-490">Position?</span></span> |<span data-ttu-id="54540-491">1</span><span class="sxs-lookup"><span data-stu-id="54540-491">1</span></span> |
| <span data-ttu-id="54540-492">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="54540-492">Default value</span></span> |<span data-ttu-id="54540-493">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-493">none</span></span> |
| <span data-ttu-id="54540-494">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="54540-494">Accept pipeline input?</span></span> |<span data-ttu-id="54540-495">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="54540-495">true(ByPropertyName)</span></span> |
| <span data-ttu-id="54540-496">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="54540-496">Accept wildcard characters?</span></span> |<span data-ttu-id="54540-497">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-497">false</span></span> |

<span data-ttu-id="54540-498">**-StorageAccountId &lt;sträng&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-498">**-StorageAccountId &lt;String&gt;**</span></span>

<span data-ttu-id="54540-499">Anger hello storage-konto som är associerade med hello media service.</span><span class="sxs-lookup"><span data-stu-id="54540-499">Specifies hello storage account associated with hello media service.</span></span>

| <span data-ttu-id="54540-500">Alias</span><span class="sxs-lookup"><span data-stu-id="54540-500">Aliases</span></span> | <span data-ttu-id="54540-501">Id</span><span class="sxs-lookup"><span data-stu-id="54540-501">Id</span></span> |
| --- | --- |
| <span data-ttu-id="54540-502">Krävs?</span><span class="sxs-lookup"><span data-stu-id="54540-502">Required?</span></span> |<span data-ttu-id="54540-503">SANT</span><span class="sxs-lookup"><span data-stu-id="54540-503">true</span></span> |
| <span data-ttu-id="54540-504">Placera?</span><span class="sxs-lookup"><span data-stu-id="54540-504">Position?</span></span> |<span data-ttu-id="54540-505">2</span><span class="sxs-lookup"><span data-stu-id="54540-505">2</span></span> |
| <span data-ttu-id="54540-506">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="54540-506">Default value</span></span> |<span data-ttu-id="54540-507">Ingen</span><span class="sxs-lookup"><span data-stu-id="54540-507">none</span></span> |
| <span data-ttu-id="54540-508">Acceptera indata från pipeline?</span><span class="sxs-lookup"><span data-stu-id="54540-508">Accept pipeline input?</span></span> |<span data-ttu-id="54540-509">true(ByPropertyName)</span><span class="sxs-lookup"><span data-stu-id="54540-509">true(ByPropertyName)</span></span> |
| <span data-ttu-id="54540-510">Acceptera jokertecken?</span><span class="sxs-lookup"><span data-stu-id="54540-510">Accept wildcard characters?</span></span> |<span data-ttu-id="54540-511">FALSKT</span><span class="sxs-lookup"><span data-stu-id="54540-511">false</span></span> |

<span data-ttu-id="54540-512">**&lt;CommandParameters&gt;**</span><span class="sxs-lookup"><span data-stu-id="54540-512">**&lt;CommandParameters&gt;**</span></span>

<span data-ttu-id="54540-513">Denna cmdlet stöder hello vanliga parametrar:-Debug, - ErrorAction, - ErrorVariable, - InformationAction, -InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - Verbose, - WarningAction och -WarningVariable.</span><span class="sxs-lookup"><span data-stu-id="54540-513">This cmdlet supports hello common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable.</span></span>

### <a name="inputs"></a><span data-ttu-id="54540-514">Indata</span><span class="sxs-lookup"><span data-stu-id="54540-514">Inputs</span></span>
<span data-ttu-id="54540-515">hello indatatypen är hello typ av hello objekt kan du skicka toohello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="54540-515">hello input type is hello type of hello objects that you can pipe toohello cmdlet.</span></span>

### <a name="outputs"></a><span data-ttu-id="54540-516">utdata</span><span class="sxs-lookup"><span data-stu-id="54540-516">Outputs</span></span>
<span data-ttu-id="54540-517">hello utdatatypen är hello typ av hello-objekt som hello cmdleten genererar.</span><span class="sxs-lookup"><span data-stu-id="54540-517">hello output type is hello type of hello objects that hello cmdlet emits.</span></span>

## <a name="next-step"></a><span data-ttu-id="54540-518">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="54540-518">Next step</span></span>
<span data-ttu-id="54540-519">Checka ut utbildningsvägar för Media Services.</span><span class="sxs-lookup"><span data-stu-id="54540-519">Check out Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="54540-520">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="54540-520">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

