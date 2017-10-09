---
title: aaaMonitor och hantera Stream Analytics-jobb med PowerShell | Microsoft Docs
description: "Lär dig hur toouse Azure PowerShell cmdlet: ar och toomonitor och hantera Stream Analytics-jobb."
keywords: Azure powershell, azure powershell-cmdlets, powershell-kommando, powershell-skript
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 514f454e-d18c-4081-8304-ab48577e15e8
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 44abc82f1c44a5ebc1701badd6547b84dac239b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a><span data-ttu-id="af9e4-104">Övervaka och hantera Stream Analytics-jobb med Azure PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="af9e4-104">Monitor and manage Stream Analytics jobs with Azure PowerShell cmdlets</span></span>
<span data-ttu-id="af9e4-105">Lär dig hur toomonitor och hantera Stream Analytics-resurser med Azure PowerShell-cmdlets och powershell-skript som kör grundläggande Stream Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="af9e4-105">Learn how toomonitor and manage Stream Analytics resources with Azure PowerShell cmdlets and powershell scripting that execute basic Stream Analytics tasks.</span></span>

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a><span data-ttu-id="af9e4-106">Förutsättningar för att köra Azure PowerShell-cmdlets för Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="af9e4-106">Prerequisites for running Azure PowerShell cmdlets for Stream Analytics</span></span>
* <span data-ttu-id="af9e4-107">Skapa en Azure-resursgrupp i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="af9e4-107">Create an Azure Resource Group in your subscription.</span></span> <span data-ttu-id="af9e4-108">hello följande är ett exempel Azure PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="af9e4-108">hello following is a sample Azure PowerShell script.</span></span> <span data-ttu-id="af9e4-109">Azure PowerShell information finns i [installera och konfigurera Azure PowerShell](/powershell/azure/overview);</span><span class="sxs-lookup"><span data-stu-id="af9e4-109">For Azure PowerShell information, see [Install and configure Azure PowerShell](/powershell/azure/overview);</span></span>  

<span data-ttu-id="af9e4-110">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-110">Azure PowerShell 0.9.8:</span></span>  

         # Log in tooyour Azure account
        Add-AzureAccount

        # Select hello Azure subscription you want toouse toocreate hello resource group if you have more than one subscription on your account.
        Select-AzureSubscription -SubscriptionName <subscription name>

        # If Stream Analytics has not been registered toohello subscription, remove remark symbol below (#) toorun hello Register-AzureProvider cmdlet tooregister hello provider namespace.
        #Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>

<span data-ttu-id="af9e4-111">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-111">Azure PowerShell 1.0:</span></span>  

         # Log in tooyour Azure account
        Login-AzureRmAccount

        # Select hello Azure subscription you want toouse toocreate hello resource group.
        Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

        # If Stream Analytics has not been registered toohello subscription, remove remark symbol below (#) toorun hello Register-AzureProvider cmdlet tooregister hello provider namespace.
        #Register-AzureRmResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureRMResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>



> [!NOTE]
> <span data-ttu-id="af9e4-112">Stream Analytics-jobb som skapats inte övervaka aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="af9e4-112">Stream Analytics jobs created programmatically do not have monitoring enabled by default.</span></span>  <span data-ttu-id="af9e4-113">Du kan manuellt aktivera övervakning i hello Azure Portal genom att gå toohello jobbet övervakaren sidan och klicka på knappen för hello aktivera eller du kan göra detta via programmering genom att följa hello stegen finns i [Azure Stream Analytics - övervakaren dataström Analytics-jobb genom att programmera](stream-analytics-monitor-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="af9e4-113">You can manually enable monitoring in hello Azure Portal by navigating toohello job’s Monitor page and clicking hello Enable button or you can do this programmatically by following hello steps located at [Azure Stream Analytics - Monitor Stream Analytics Jobs Programatically](stream-analytics-monitor-jobs.md).</span></span>
> 
> 

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a><span data-ttu-id="af9e4-114">Azure PowerShell-cmdlets för Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="af9e4-114">Azure PowerShell cmdlets for Stream Analytics</span></span>
<span data-ttu-id="af9e4-115">hello följande Azure PowerShell-cmdlets kan använda toomonitor och hantera Azure Stream Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="af9e4-115">hello following Azure PowerShell cmdlets can be used toomonitor and manage Azure Stream Analytics jobs.</span></span> <span data-ttu-id="af9e4-116">Observera att Azure PowerShell har olika versioner.</span><span class="sxs-lookup"><span data-stu-id="af9e4-116">Note that Azure PowerShell has different versions.</span></span> 
<span data-ttu-id="af9e4-117">**I hello exempel listade hello första kommandot avser Azure PowerShell 0.9.8, är hello andra kommandot för Azure PowerShell 1.0.**</span><span class="sxs-lookup"><span data-stu-id="af9e4-117">**In hello examples listed hello first command is for Azure PowerShell 0.9.8, hello second command is for Azure PowerShell 1.0.**</span></span> <span data-ttu-id="af9e4-118">hello Azure PowerShell 1.0 kommandon har alltid ”AzureRM” i hello-kommandot.</span><span class="sxs-lookup"><span data-stu-id="af9e4-118">hello Azure PowerShell 1.0 commands will always have "AzureRM" in hello command.</span></span>

### <a name="get-azurestreamanalyticsjob--get-azurermstreamanalyticsjob"></a><span data-ttu-id="af9e4-119">Get-AzureStreamAnalyticsJob | Get-AzureRMStreamAnalyticsJob</span><span class="sxs-lookup"><span data-stu-id="af9e4-119">Get-AzureStreamAnalyticsJob | Get-AzureRMStreamAnalyticsJob</span></span>
<span data-ttu-id="af9e4-120">Visar en lista över alla Stream Analytics-jobb som definierats i hello Azure-prenumeration eller angivna resursgruppen eller hämtar jobbinformation om ett specifikt jobb inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="af9e4-120">Lists all Stream Analytics jobs defined in hello Azure subscription or specified resource group, or gets job information about a specific job within a resource group.</span></span>

<span data-ttu-id="af9e4-121">**Exempel 1**</span><span class="sxs-lookup"><span data-stu-id="af9e4-121">**Example 1**</span></span>

<span data-ttu-id="af9e4-122">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-122">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsJob

<span data-ttu-id="af9e4-123">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-123">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsJob

<span data-ttu-id="af9e4-124">Detta PowerShell-kommando returnerar information om alla hello Stream Analytics-jobb i hello Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="af9e4-124">This PowerShell command returns information about all hello Stream Analytics jobs in hello Azure subscription.</span></span>

<span data-ttu-id="af9e4-125">**Exempel 2**</span><span class="sxs-lookup"><span data-stu-id="af9e4-125">**Example 2**</span></span>

<span data-ttu-id="af9e4-126">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-126">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

<span data-ttu-id="af9e4-127">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-127">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

<span data-ttu-id="af9e4-128">Detta PowerShell-kommando returnerar information om alla hello Stream Analytics-jobb i hello resursgruppen StreamAnalytics-standard-Central-US.</span><span class="sxs-lookup"><span data-stu-id="af9e4-128">This PowerShell command returns information about all hello Stream Analytics jobs in hello resource group StreamAnalytics-Default-Central-US.</span></span>

<span data-ttu-id="af9e4-129">**Exempel 3**</span><span class="sxs-lookup"><span data-stu-id="af9e4-129">**Example 3**</span></span>

<span data-ttu-id="af9e4-130">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-130">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

<span data-ttu-id="af9e4-131">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-131">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

<span data-ttu-id="af9e4-132">Detta PowerShell-kommando returnerar information om hello Stream Analytics-jobbet StreamingJob i hello resursgruppen StreamAnalytics-standard-Central-US.</span><span class="sxs-lookup"><span data-stu-id="af9e4-132">This PowerShell command returns information about hello Stream Analytics job StreamingJob in hello resource group StreamAnalytics-Default-Central-US.</span></span>

### <a name="get-azurestreamanalyticsinput--get-azurermstreamanalyticsinput"></a><span data-ttu-id="af9e4-133">Get-AzureStreamAnalyticsInput | Get-AzureRMStreamAnalyticsInput</span><span class="sxs-lookup"><span data-stu-id="af9e4-133">Get-AzureStreamAnalyticsInput | Get-AzureRMStreamAnalyticsInput</span></span>
<span data-ttu-id="af9e4-134">Hämtar information om specifika indata eller en lista över alla hello indata som definieras i en angiven Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="af9e4-134">Lists all of hello inputs that are defined in a specified Stream Analytics job, or gets information about a specific input.</span></span>

<span data-ttu-id="af9e4-135">**Exempel 1**</span><span class="sxs-lookup"><span data-stu-id="af9e4-135">**Example 1**</span></span>

<span data-ttu-id="af9e4-136">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-136">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

<span data-ttu-id="af9e4-137">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-137">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

<span data-ttu-id="af9e4-138">Detta PowerShell-kommando returnerar information om alla hello indata som definierats i hello jobbet StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="af9e4-138">This PowerShell command returns information about all hello inputs defined in hello job StreamingJob.</span></span>

<span data-ttu-id="af9e4-139">**Exempel 2**</span><span class="sxs-lookup"><span data-stu-id="af9e4-139">**Example 2**</span></span>

<span data-ttu-id="af9e4-140">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-140">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

<span data-ttu-id="af9e4-141">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-141">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

<span data-ttu-id="af9e4-142">Detta PowerShell-kommando returnerar information om hello indata med namnet EntryStream som definierats i hello jobbet StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="af9e4-142">This PowerShell command returns information about hello input named EntryStream defined in hello job StreamingJob.</span></span>

### <a name="get-azurestreamanalyticsoutput--get-azurermstreamanalyticsoutput"></a><span data-ttu-id="af9e4-143">Get-AzureStreamAnalyticsOutput | Get-AzureRMStreamAnalyticsOutput</span><span class="sxs-lookup"><span data-stu-id="af9e4-143">Get-AzureStreamAnalyticsOutput | Get-AzureRMStreamAnalyticsOutput</span></span>
<span data-ttu-id="af9e4-144">Visar en lista över hello utdata som definieras i en angiven Stream Analytics-jobbet eller hämtar information om en specifik utdata.</span><span class="sxs-lookup"><span data-stu-id="af9e4-144">Lists all of hello outputs that are defined in a specified Stream Analytics job, or gets information about a specific output.</span></span>

<span data-ttu-id="af9e4-145">**Exempel 1**</span><span class="sxs-lookup"><span data-stu-id="af9e4-145">**Example 1**</span></span>

<span data-ttu-id="af9e4-146">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-146">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

<span data-ttu-id="af9e4-147">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-147">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

<span data-ttu-id="af9e4-148">Detta PowerShell-kommando returnerar information om hello utdata som definierats i hello jobbet StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="af9e4-148">This PowerShell command returns information about hello outputs defined in hello job StreamingJob.</span></span>

<span data-ttu-id="af9e4-149">**Exempel 2**</span><span class="sxs-lookup"><span data-stu-id="af9e4-149">**Example 2**</span></span>

<span data-ttu-id="af9e4-150">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-150">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

<span data-ttu-id="af9e4-151">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-151">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

<span data-ttu-id="af9e4-152">Detta PowerShell-kommando returnerar information om hello utdata med namnet utdata som definierats i hello jobbet StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="af9e4-152">This PowerShell command returns information about hello output named Output defined in hello job StreamingJob.</span></span>

### <a name="get-azurestreamanalyticsquota--get-azurermstreamanalyticsquota"></a><span data-ttu-id="af9e4-153">Get-AzureStreamAnalyticsQuota | Get-AzureRMStreamAnalyticsQuota</span><span class="sxs-lookup"><span data-stu-id="af9e4-153">Get-AzureStreamAnalyticsQuota | Get-AzureRMStreamAnalyticsQuota</span></span>
<span data-ttu-id="af9e4-154">Hämtar information om hello kvoten för strömmande enheter i ett visst område.</span><span class="sxs-lookup"><span data-stu-id="af9e4-154">Gets information about hello quota of streaming units in a specified region.</span></span>

<span data-ttu-id="af9e4-155">**Exempel 1**</span><span class="sxs-lookup"><span data-stu-id="af9e4-155">**Example 1**</span></span>

<span data-ttu-id="af9e4-156">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-156">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsQuota –Location "Central US" 

<span data-ttu-id="af9e4-157">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-157">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsQuota –Location "Central US" 

<span data-ttu-id="af9e4-158">Detta PowerShell-kommando returnerar information om hello kvoten och användning av enheter för strömning i hello centrala USA region.</span><span class="sxs-lookup"><span data-stu-id="af9e4-158">This PowerShell command returns information about hello quota and usage of streaming units in hello Central US region.</span></span>

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a><span data-ttu-id="af9e4-159">Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation</span><span class="sxs-lookup"><span data-stu-id="af9e4-159">Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation</span></span>
<span data-ttu-id="af9e4-160">Hämtar information om en omvandling definieras i ett Stream Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="af9e4-160">Gets information about a specific transformation defined in a Stream Analytics job.</span></span>

<span data-ttu-id="af9e4-161">**Exempel 1**</span><span class="sxs-lookup"><span data-stu-id="af9e4-161">**Example 1**</span></span>

<span data-ttu-id="af9e4-162">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-162">Azure PowerShell 0.9.8:</span></span>  

    Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

<span data-ttu-id="af9e4-163">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-163">Azure PowerShell 1.0:</span></span>  

    Get-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

<span data-ttu-id="af9e4-164">Detta PowerShell-kommando returnerar information om hello omvandling kallas StreamingJob hello jobb StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="af9e4-164">This PowerShell command returns information about hello transformation called StreamingJob in hello job StreamingJob.</span></span>

### <a name="new-azurestreamanalyticsinput--new-azurermstreamanalyticsinput"></a><span data-ttu-id="af9e4-165">Nya AzureStreamAnalyticsInput | Ny AzureRMStreamAnalyticsInput</span><span class="sxs-lookup"><span data-stu-id="af9e4-165">New-AzureStreamAnalyticsInput | New-AzureRMStreamAnalyticsInput</span></span>
<span data-ttu-id="af9e4-166">Skapar en ny inmatning inom ett Stream Analytics-jobb eller uppdaterar befintliga angivna indata.</span><span class="sxs-lookup"><span data-stu-id="af9e4-166">Creates a new input within a Stream Analytics job, or updates an existing specified input.</span></span>

<span data-ttu-id="af9e4-167">hello kan namnet på hello indata anges i hello JSON-fil eller på hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="af9e4-167">hello name of hello input can be specified in hello .json file or on hello command line.</span></span> <span data-ttu-id="af9e4-168">Om båda anges måste hello namn på kommandoraden för hello vara hello samma som hello något i hello-filen.</span><span class="sxs-lookup"><span data-stu-id="af9e4-168">If both are specified, hello name on hello command line must be hello same as hello one in hello file.</span></span>

<span data-ttu-id="af9e4-169">Om du anger indata som redan finns och inte anger hello – Force-parametern hello cmdlet ber oavsett tooreplace hello befintliga indata.</span><span class="sxs-lookup"><span data-stu-id="af9e4-169">If you specify an input that already exists and do not specify hello –Force parameter, hello cmdlet will ask whether or not tooreplace hello existing input.</span></span>

<span data-ttu-id="af9e4-170">Om du anger hello – Force-parametern och ange en befintlig ange namn, hello indata ersätts utan bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="af9e4-170">If you specify hello –Force parameter and specify an existing input name, hello input will be replaced without confirmation.</span></span>

<span data-ttu-id="af9e4-171">Detaljerad information om hello JSON-filstruktur och innehåll finns i toohello [skapa indata (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-input] avsnitt i hello [Stream Analytics Management REST API Referensbiblioteket][stream.analytics.rest.api.reference].</span><span class="sxs-lookup"><span data-stu-id="af9e4-171">For detailed information on hello JSON file structure and contents, refer toohello [Create Input (Azure Stream Analytics)][msdn-rest-api-create-stream-analytics-input] section of hello [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].</span></span>

<span data-ttu-id="af9e4-172">**Exempel 1**</span><span class="sxs-lookup"><span data-stu-id="af9e4-172">**Example 1**</span></span>

<span data-ttu-id="af9e4-173">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-173">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

<span data-ttu-id="af9e4-174">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-174">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

<span data-ttu-id="af9e4-175">Detta PowerShell-kommando skapar en ny indata från hello filen Input.json.</span><span class="sxs-lookup"><span data-stu-id="af9e4-175">This PowerShell command creates a new input from hello file Input.json.</span></span> <span data-ttu-id="af9e4-176">Om befintliga indata med hello-namnet som angetts i hello inkommande definitionsfilen har redan definierats hello cmdlet ber om huruvida tooreplace den.</span><span class="sxs-lookup"><span data-stu-id="af9e4-176">If an existing input with hello name specified in hello input definition file is already defined, hello cmdlet will ask whether or not tooreplace it.</span></span>

<span data-ttu-id="af9e4-177">**Exempel 2**</span><span class="sxs-lookup"><span data-stu-id="af9e4-177">**Example 2**</span></span>

<span data-ttu-id="af9e4-178">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-178">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

<span data-ttu-id="af9e4-179">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-179">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

<span data-ttu-id="af9e4-180">Detta PowerShell-kommando skapar en ny inmatning hello jobb kallas EntryStream.</span><span class="sxs-lookup"><span data-stu-id="af9e4-180">This PowerShell command creates a new input in hello job called EntryStream.</span></span> <span data-ttu-id="af9e4-181">Om en befintlig indata med det här namnet har redan definierats hello cmdlet ber om huruvida tooreplace den.</span><span class="sxs-lookup"><span data-stu-id="af9e4-181">If an existing input with this name is already defined, hello cmdlet will ask whether or not tooreplace it.</span></span>

<span data-ttu-id="af9e4-182">**Exempel 3**</span><span class="sxs-lookup"><span data-stu-id="af9e4-182">**Example 3**</span></span>

<span data-ttu-id="af9e4-183">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-183">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

<span data-ttu-id="af9e4-184">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-184">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

<span data-ttu-id="af9e4-185">Detta PowerShell-kommando ersätter hello definition av hello befintliga indatakälla kallas EntryStream hello definition från hello-fil.</span><span class="sxs-lookup"><span data-stu-id="af9e4-185">This PowerShell command replaces hello definition of hello existing input source called EntryStream with hello definition from hello file.</span></span>

### <a name="new-azurestreamanalyticsjob--new-azurermstreamanalyticsjob"></a><span data-ttu-id="af9e4-186">Nya AzureStreamAnalyticsJob | Ny AzureRMStreamAnalyticsJob</span><span class="sxs-lookup"><span data-stu-id="af9e4-186">New-AzureStreamAnalyticsJob | New-AzureRMStreamAnalyticsJob</span></span>
<span data-ttu-id="af9e4-187">Skapar ett nytt Stream Analytics-jobb i Microsoft Azure eller uppdaterar hello definitionen för en befintlig angivna jobbet.</span><span class="sxs-lookup"><span data-stu-id="af9e4-187">Creates a new Stream Analytics job in Microsoft Azure, or updates hello definition of an existing specified job.</span></span>

<span data-ttu-id="af9e4-188">hello namnet på hello jobb kan anges i hello JSON-fil eller på hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="af9e4-188">hello name of hello job can be specified in hello .json file or on hello command line.</span></span> <span data-ttu-id="af9e4-189">Om båda anges måste hello namn på kommandoraden för hello vara hello samma som hello något i hello-filen.</span><span class="sxs-lookup"><span data-stu-id="af9e4-189">If both are specified, hello name on hello command line must be hello same as hello one in hello file.</span></span>

<span data-ttu-id="af9e4-190">Om du anger ett namn som redan finns och inte anger hello – Force-parametern hello cmdlet ber oavsett tooreplace hello befintligt jobb.</span><span class="sxs-lookup"><span data-stu-id="af9e4-190">If you specify a job name that already exists and do not specify hello –Force parameter, hello cmdlet will ask whether or not tooreplace hello existing job.</span></span>

<span data-ttu-id="af9e4-191">Om du anger hello – Force-parametern och ange Jobbnamnet på en befintlig hello jobbdefinitionen ersätts utan bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="af9e4-191">If you specify hello –Force parameter and specify an existing job name, hello job definition will be replaced without confirmation.</span></span>

<span data-ttu-id="af9e4-192">Detaljerad information om hello JSON-filstruktur och innehåll finns i toohello [skapa Stream Analytics-jobbet] [ msdn-rest-api-create-stream-analytics-job] avsnitt i hello [Stream Analytics Management REST API-referens Biblioteket][stream.analytics.rest.api.reference].</span><span class="sxs-lookup"><span data-stu-id="af9e4-192">For detailed information on hello JSON file structure and contents, refer toohello [Create Stream Analytics Job][msdn-rest-api-create-stream-analytics-job] section of hello [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].</span></span>

<span data-ttu-id="af9e4-193">**Exempel 1**</span><span class="sxs-lookup"><span data-stu-id="af9e4-193">**Example 1**</span></span>

<span data-ttu-id="af9e4-194">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-194">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

<span data-ttu-id="af9e4-195">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-195">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

<span data-ttu-id="af9e4-196">Detta PowerShell-kommando skapar ett nytt jobb från hello definition i JobDefinition.json.</span><span class="sxs-lookup"><span data-stu-id="af9e4-196">This PowerShell command creates a new job from hello definition in JobDefinition.json.</span></span> <span data-ttu-id="af9e4-197">Om ett befintligt projekt med hello-namnet som angetts i definitionsfilen för hello jobbet har redan definierats hello cmdlet ber om huruvida tooreplace den.</span><span class="sxs-lookup"><span data-stu-id="af9e4-197">If an existing job with hello name specified in hello job definition file is already defined, hello cmdlet will ask whether or not tooreplace it.</span></span>

<span data-ttu-id="af9e4-198">**Exempel 2**</span><span class="sxs-lookup"><span data-stu-id="af9e4-198">**Example 2**</span></span>

<span data-ttu-id="af9e4-199">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-199">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

<span data-ttu-id="af9e4-200">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-200">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

<span data-ttu-id="af9e4-201">Detta PowerShell-kommando ersätter hello jobbdefinitionen för StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="af9e4-201">This PowerShell command replaces hello job definition for StreamingJob.</span></span>

### <a name="new-azurestreamanalyticsoutput--new-azurermstreamanalyticsoutput"></a><span data-ttu-id="af9e4-202">Nya AzureStreamAnalyticsOutput | Ny AzureRMStreamAnalyticsOutput</span><span class="sxs-lookup"><span data-stu-id="af9e4-202">New-AzureStreamAnalyticsOutput | New-AzureRMStreamAnalyticsOutput</span></span>
<span data-ttu-id="af9e4-203">Skapar en ny utdata inom ett Stream Analytics-jobb eller uppdaterar befintliga utdata.</span><span class="sxs-lookup"><span data-stu-id="af9e4-203">Creates a new output within a Stream Analytics job, or updates an existing output.</span></span>  

<span data-ttu-id="af9e4-204">hello namn för hello utdata kan anges i hello JSON-fil eller på hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="af9e4-204">hello name of hello output can be specified in hello .json file or on hello command line.</span></span> <span data-ttu-id="af9e4-205">Om båda anges måste hello namn på kommandoraden för hello vara hello samma som hello något i hello-filen.</span><span class="sxs-lookup"><span data-stu-id="af9e4-205">If both are specified, hello name on hello command line must be hello same as hello one in hello file.</span></span>

<span data-ttu-id="af9e4-206">Om du anger ett utgående som redan finns och inte anger hello – Force-parametern hello cmdlet ber oavsett tooreplace hello befintliga utdata.</span><span class="sxs-lookup"><span data-stu-id="af9e4-206">If you specify an output that already exists and do not specify hello –Force parameter, hello cmdlet will ask whether or not tooreplace hello existing output.</span></span>

<span data-ttu-id="af9e4-207">Om du anger hello – Force-parametern och ange en befintlig utdata namn, hello utdata ersätts utan bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="af9e4-207">If you specify hello –Force parameter and specify an existing output name, hello output will be replaced without confirmation.</span></span>

<span data-ttu-id="af9e4-208">Detaljerad information om hello JSON-filstruktur och innehåll finns i toohello [skapa utdata (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-output] avsnitt i hello [Stream Analytics Management REST API Referensbiblioteket][stream.analytics.rest.api.reference].</span><span class="sxs-lookup"><span data-stu-id="af9e4-208">For detailed information on hello JSON file structure and contents, refer toohello [Create Output (Azure Stream Analytics)][msdn-rest-api-create-stream-analytics-output] section of hello [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].</span></span>

<span data-ttu-id="af9e4-209">**Exempel 1**</span><span class="sxs-lookup"><span data-stu-id="af9e4-209">**Example 1**</span></span>

<span data-ttu-id="af9e4-210">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-210">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

<span data-ttu-id="af9e4-211">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-211">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

<span data-ttu-id="af9e4-212">Detta PowerShell-kommando skapar en ny utdata som kallas ”utdata” hello jobb StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="af9e4-212">This PowerShell command creates a new output called "output" in hello job StreamingJob.</span></span> <span data-ttu-id="af9e4-213">Om en befintlig utdata med det här namnet har redan definierats hello cmdlet ber om huruvida tooreplace den.</span><span class="sxs-lookup"><span data-stu-id="af9e4-213">If an existing output with this name is already defined, hello cmdlet will ask whether or not tooreplace it.</span></span>

<span data-ttu-id="af9e4-214">**Exempel 2**</span><span class="sxs-lookup"><span data-stu-id="af9e4-214">**Example 2**</span></span>

<span data-ttu-id="af9e4-215">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-215">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

<span data-ttu-id="af9e4-216">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-216">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

<span data-ttu-id="af9e4-217">Detta PowerShell-kommando ersätter hello definitionen för ”utdata” hello jobb StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="af9e4-217">This PowerShell command replaces hello definition for "output" in hello job StreamingJob.</span></span>

### <a name="new-azurestreamanalyticstransformation--new-azurermstreamanalyticstransformation"></a><span data-ttu-id="af9e4-218">Nya AzureStreamAnalyticsTransformation | Ny AzureRMStreamAnalyticsTransformation</span><span class="sxs-lookup"><span data-stu-id="af9e4-218">New-AzureStreamAnalyticsTransformation | New-AzureRMStreamAnalyticsTransformation</span></span>
<span data-ttu-id="af9e4-219">Skapar en ny omvandling inom ett Stream Analytics-jobb eller uppdaterar befintliga hello-omvandling.</span><span class="sxs-lookup"><span data-stu-id="af9e4-219">Creates a new transformation within a Stream Analytics job, or updates hello existing transformation.</span></span>

<span data-ttu-id="af9e4-220">hello namnet på hello omvandling kan anges i hello JSON-fil eller på hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="af9e4-220">hello name of hello transformation can be specified in hello .json file or on hello command line.</span></span> <span data-ttu-id="af9e4-221">Om båda anges måste hello namn på kommandoraden för hello vara hello samma som hello något i hello-filen.</span><span class="sxs-lookup"><span data-stu-id="af9e4-221">If both are specified, hello name on hello command line must be hello same as hello one in hello file.</span></span>

<span data-ttu-id="af9e4-222">Om du anger en omvandling som redan finns och inte anger hello – Force-parametern hello cmdlet ber oavsett tooreplace hello omvandling av befintliga.</span><span class="sxs-lookup"><span data-stu-id="af9e4-222">If you specify a transformation that already exists and do not specify hello –Force parameter, hello cmdlet will ask whether or not tooreplace hello existing transformation.</span></span>

<span data-ttu-id="af9e4-223">Om du anger hello – Force-parametern och ange namnet på en befintlig omvandling hello omvandling ersätts utan bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="af9e4-223">If you specify hello –Force parameter and specify an existing transformation name, hello transformation will be replaced without confirmation.</span></span>

<span data-ttu-id="af9e4-224">Detaljerad information om hello JSON-filstruktur och innehåll finns i toohello [skapa omvandling (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-transformation] avsnitt i hello [Stream Analytics Management REST API-referensbiblioteket][stream.analytics.rest.api.reference].</span><span class="sxs-lookup"><span data-stu-id="af9e4-224">For detailed information on hello JSON file structure and contents, refer toohello [Create Transformation (Azure Stream Analytics)][msdn-rest-api-create-stream-analytics-transformation] section of hello [Stream Analytics Management REST API Reference Library][stream.analytics.rest.api.reference].</span></span>

<span data-ttu-id="af9e4-225">**Exempel 1**</span><span class="sxs-lookup"><span data-stu-id="af9e4-225">**Example 1**</span></span>

<span data-ttu-id="af9e4-226">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-226">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

<span data-ttu-id="af9e4-227">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-227">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

<span data-ttu-id="af9e4-228">Detta PowerShell-kommando skapar en ny omvandling kallas StreamingJobTransform hello jobb StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="af9e4-228">This PowerShell command creates a new transformation called StreamingJobTransform in hello job StreamingJob.</span></span> <span data-ttu-id="af9e4-229">Om en befintlig transformation har redan definierats med samma namn, hello cmdlet ber om huruvida tooreplace den.</span><span class="sxs-lookup"><span data-stu-id="af9e4-229">If an existing transformation is already defined with this name, hello cmdlet will ask whether or not tooreplace it.</span></span>

<span data-ttu-id="af9e4-230">**Exempel 2**</span><span class="sxs-lookup"><span data-stu-id="af9e4-230">**Example 2**</span></span>

<span data-ttu-id="af9e4-231">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-231">Azure PowerShell 0.9.8:</span></span>  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

<span data-ttu-id="af9e4-232">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-232">Azure PowerShell 1.0:</span></span>  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

 <span data-ttu-id="af9e4-233">Detta PowerShell-kommando ersätter hello definitionen av StreamingJobTransform hello jobb StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="af9e4-233">This PowerShell command replaces hello definition of StreamingJobTransform in hello job StreamingJob.</span></span>

### <a name="remove-azurestreamanalyticsinput--remove-azurermstreamanalyticsinput"></a><span data-ttu-id="af9e4-234">Ta bort AzureStreamAnalyticsInput | Ta bort AzureRMStreamAnalyticsInput</span><span class="sxs-lookup"><span data-stu-id="af9e4-234">Remove-AzureStreamAnalyticsInput | Remove-AzureRMStreamAnalyticsInput</span></span>
<span data-ttu-id="af9e4-235">Tar bort specifika indata asynkront från en Stream Analytics-jobbet i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="af9e4-235">Asynchronously deletes a specific input from a Stream Analytics job in Microsoft Azure.</span></span>  
<span data-ttu-id="af9e4-236">Om du anger hello – Force-parametern, hello indata kommer att tas bort utan bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="af9e4-236">If you specify hello –Force parameter, hello input will be deleted without confirmation.</span></span>

<span data-ttu-id="af9e4-237">**Exempel 1**</span><span class="sxs-lookup"><span data-stu-id="af9e4-237">**Example 1**</span></span>

<span data-ttu-id="af9e4-238">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-238">Azure PowerShell 0.9.8:</span></span>  

    Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

<span data-ttu-id="af9e4-239">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-239">Azure PowerShell 1.0:</span></span>  

    Remove-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

<span data-ttu-id="af9e4-240">Det här PowerShell-kommandot tar bort hello inkommande EventStream hello jobb StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="af9e4-240">This PowerShell command removes hello input EventStream in hello job StreamingJob.</span></span>  

### <a name="remove-azurestreamanalyticsjob--remove-azurermstreamanalyticsjob"></a><span data-ttu-id="af9e4-241">Ta bort AzureStreamAnalyticsJob | Ta bort AzureRMStreamAnalyticsJob</span><span class="sxs-lookup"><span data-stu-id="af9e4-241">Remove-AzureStreamAnalyticsJob | Remove-AzureRMStreamAnalyticsJob</span></span>
<span data-ttu-id="af9e4-242">Tar bort ett specifikt Stream Analytics-jobb i Microsoft Azure asynkront.</span><span class="sxs-lookup"><span data-stu-id="af9e4-242">Asynchronously deletes a specific Stream Analytics job in Microsoft Azure.</span></span>  
<span data-ttu-id="af9e4-243">Om du anger hello – Force-parametern hello-jobbet kommer att tas bort utan bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="af9e4-243">If you specify hello –Force parameter, hello job will be deleted without confirmation.</span></span>

<span data-ttu-id="af9e4-244">**Exempel 1**</span><span class="sxs-lookup"><span data-stu-id="af9e4-244">**Example 1**</span></span>

<span data-ttu-id="af9e4-245">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-245">Azure PowerShell 0.9.8:</span></span>  

    Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

<span data-ttu-id="af9e4-246">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-246">Azure PowerShell 1.0:</span></span>  

    Remove-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

<span data-ttu-id="af9e4-247">Detta PowerShell-kommando bort hello jobbet StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="af9e4-247">This PowerShell command removes hello job StreamingJob.</span></span>  

### <a name="remove-azurestreamanalyticsoutput--remove-azurermstreamanalyticsoutput"></a><span data-ttu-id="af9e4-248">Ta bort AzureStreamAnalyticsOutput | Ta bort AzureRMStreamAnalyticsOutput</span><span class="sxs-lookup"><span data-stu-id="af9e4-248">Remove-AzureStreamAnalyticsOutput | Remove-AzureRMStreamAnalyticsOutput</span></span>
<span data-ttu-id="af9e4-249">Tar bort en specifik utdata från ett Stream Analytics-jobb i Microsoft Azure asynkront.</span><span class="sxs-lookup"><span data-stu-id="af9e4-249">Asynchronously deletes a specific output from a Stream Analytics job in Microsoft Azure.</span></span>  
<span data-ttu-id="af9e4-250">Om du anger hello – Force-parametern hello utdata kommer att tas bort utan bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="af9e4-250">If you specify hello –Force parameter, hello output will be deleted without confirmation.</span></span>

<span data-ttu-id="af9e4-251">**Exempel 1**</span><span class="sxs-lookup"><span data-stu-id="af9e4-251">**Example 1**</span></span>

<span data-ttu-id="af9e4-252">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-252">Azure PowerShell 0.9.8:</span></span>  

    Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

<span data-ttu-id="af9e4-253">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-253">Azure PowerShell 1.0:</span></span>  

    Remove-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

<span data-ttu-id="af9e4-254">Det här PowerShell-kommandot tar bort hello utdata utdata i hello jobbet StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="af9e4-254">This PowerShell command removes hello output Output in hello job StreamingJob.</span></span>  

### <a name="start-azurestreamanalyticsjob--start-azurermstreamanalyticsjob"></a><span data-ttu-id="af9e4-255">Start-AzureStreamAnalyticsJob | Start-AzureRMStreamAnalyticsJob</span><span class="sxs-lookup"><span data-stu-id="af9e4-255">Start-AzureStreamAnalyticsJob | Start-AzureRMStreamAnalyticsJob</span></span>
<span data-ttu-id="af9e4-256">Asynkront distribuerar och startar en Stream Analytics-jobb i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="af9e4-256">Asynchronously deploys and starts a Stream Analytics job in Microsoft Azure.</span></span>

<span data-ttu-id="af9e4-257">**Exempel 1**</span><span class="sxs-lookup"><span data-stu-id="af9e4-257">**Example 1**</span></span>

<span data-ttu-id="af9e4-258">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-258">Azure PowerShell 0.9.8:</span></span>  

    Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

<span data-ttu-id="af9e4-259">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-259">Azure PowerShell 1.0:</span></span>  

    Start-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

<span data-ttu-id="af9e4-260">Detta PowerShell-kommando startar hello jobbet StreamingJob med anpassade utdata starttid ange tooDecember 12, 2012, 12:12:12 UTC.</span><span class="sxs-lookup"><span data-stu-id="af9e4-260">This PowerShell command starts hello job StreamingJob with a custom output start time set tooDecember 12, 2012, 12:12:12 UTC.</span></span>

### <a name="stop-azurestreamanalyticsjob--stop-azurermstreamanalyticsjob"></a><span data-ttu-id="af9e4-261">Stoppa AzureStreamAnalyticsJob | Stoppa AzureRMStreamAnalyticsJob</span><span class="sxs-lookup"><span data-stu-id="af9e4-261">Stop-AzureStreamAnalyticsJob | Stop-AzureRMStreamAnalyticsJob</span></span>
<span data-ttu-id="af9e4-262">Asynkront stoppar ett Stream Analytics-jobb körs i Microsoft Azure och frigör resurser som var som användes.</span><span class="sxs-lookup"><span data-stu-id="af9e4-262">Asynchronously stops a Stream Analytics job from running in Microsoft Azure and de-allocates resources that were that were being used.</span></span> <span data-ttu-id="af9e4-263">hello förblir jobbdefinitionen och metadata tillgängliga i din prenumeration via både hello Azure-portalen och management API: er, så att hello jobb kan redigeras och startas om.</span><span class="sxs-lookup"><span data-stu-id="af9e4-263">hello job definition and metadata will remain available within your subscription through both hello Azure portal and management APIs, such that hello job can be edited and restarted.</span></span> <span data-ttu-id="af9e4-264">Du debiteras inte för ett jobb i hello stoppats tillstånd.</span><span class="sxs-lookup"><span data-stu-id="af9e4-264">You will not be charged for a job in hello stopped state.</span></span>

<span data-ttu-id="af9e4-265">**Exempel 1**</span><span class="sxs-lookup"><span data-stu-id="af9e4-265">**Example 1**</span></span>

<span data-ttu-id="af9e4-266">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-266">Azure PowerShell 0.9.8:</span></span>  

    Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

<span data-ttu-id="af9e4-267">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-267">Azure PowerShell 1.0:</span></span>  

    Stop-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

<span data-ttu-id="af9e4-268">Det här PowerShell-kommandot stoppar hello jobbet StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="af9e4-268">This PowerShell command stops hello job StreamingJob.</span></span>  

### <a name="test-azurestreamanalyticsinput--test-azurermstreamanalyticsinput"></a><span data-ttu-id="af9e4-269">Testa AzureStreamAnalyticsInput | Testa AzureRMStreamAnalyticsInput</span><span class="sxs-lookup"><span data-stu-id="af9e4-269">Test-AzureStreamAnalyticsInput | Test-AzureRMStreamAnalyticsInput</span></span>
<span data-ttu-id="af9e4-270">Testerna hello möjligheten för Stream Analytics tooconnect tooa angivna indata.</span><span class="sxs-lookup"><span data-stu-id="af9e4-270">Tests hello ability of Stream Analytics tooconnect tooa specified input.</span></span>

<span data-ttu-id="af9e4-271">**Exempel 1**</span><span class="sxs-lookup"><span data-stu-id="af9e4-271">**Example 1**</span></span>

<span data-ttu-id="af9e4-272">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-272">Azure PowerShell 0.9.8:</span></span>  

    Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

<span data-ttu-id="af9e4-273">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-273">Azure PowerShell 1.0:</span></span>  

    Test-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

<span data-ttu-id="af9e4-274">Det här PowerShell-kommandot tester hello anslutningsstatus hello indata EntryStream i StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="af9e4-274">This PowerShell command tests hello connection status of hello input EntryStream in StreamingJob.</span></span>  

### <a name="test-azurestreamanalyticsoutput--test-azurermstreamanalyticsoutput"></a><span data-ttu-id="af9e4-275">Testa AzureStreamAnalyticsOutput | Testa AzureRMStreamAnalyticsOutput</span><span class="sxs-lookup"><span data-stu-id="af9e4-275">Test-AzureStreamAnalyticsOutput | Test-AzureRMStreamAnalyticsOutput</span></span>
<span data-ttu-id="af9e4-276">Testerna hello möjligheten för Stream Analytics tooconnect tooa angetts utdata.</span><span class="sxs-lookup"><span data-stu-id="af9e4-276">Tests hello ability of Stream Analytics tooconnect tooa specified output.</span></span>

<span data-ttu-id="af9e4-277">**Exempel 1**</span><span class="sxs-lookup"><span data-stu-id="af9e4-277">**Example 1**</span></span>

<span data-ttu-id="af9e4-278">Azure PowerShell 0.9.8:</span><span class="sxs-lookup"><span data-stu-id="af9e4-278">Azure PowerShell 0.9.8:</span></span>  

    Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

<span data-ttu-id="af9e4-279">Azure PowerShell 1.0:</span><span class="sxs-lookup"><span data-stu-id="af9e4-279">Azure PowerShell 1.0:</span></span>  

    Test-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

<span data-ttu-id="af9e4-280">Det här PowerShell-kommandot tester hello anslutningsstatus hello utdata utdata i StreamingJob.</span><span class="sxs-lookup"><span data-stu-id="af9e4-280">This PowerShell command tests hello connection status of hello output Output in StreamingJob.</span></span>  

## <a name="get-support"></a><span data-ttu-id="af9e4-281">Få support</span><span class="sxs-lookup"><span data-stu-id="af9e4-281">Get support</span></span>
<span data-ttu-id="af9e4-282">För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="af9e4-282">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="af9e4-283">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="af9e4-283">Next steps</span></span>
* [<span data-ttu-id="af9e4-284">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="af9e4-284">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="af9e4-285">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="af9e4-285">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="af9e4-286">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="af9e4-286">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="af9e4-287">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="af9e4-287">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="af9e4-288">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="af9e4-288">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[msdn-switch-azuremode]: http://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

