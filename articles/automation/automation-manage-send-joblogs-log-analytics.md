---
title: aaaForward Azure Automation-jobbet data tooOMS Log Analytics | Microsoft Docs
description: "Den här artikeln visar hur toosend status och runbook-jobbet Projekt strömmar tooMicrosoft Operations Management Suite logganalys toodeliver ytterligare insikter och hantering."
services: automation
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: tysonn
ms.assetid: c12724c6-01a9-4b55-80ae-d8b7b99bd436
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/02/2017
ms.author: magoedte
ms.openlocfilehash: e78b6c6677d6502711ce828e2d32b7a91922ae26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="forward-job-status-and-job-streams-from-automation-toolog-analytics-oms"></a><span data-ttu-id="aee7b-103">Vidarebefordra jobbstatus och jobbet strömmar från Automation tooLog Analytics (OMS)</span><span class="sxs-lookup"><span data-stu-id="aee7b-103">Forward job status and job streams from Automation tooLog Analytics (OMS)</span></span>
<span data-ttu-id="aee7b-104">Automatisering kan skicka runbook-jobbet status och jobbstatus dataströmmar tooyour Microsoft Operations Management Suite (OMS) logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="aee7b-104">Automation can send runbook job status and job streams tooyour Microsoft Operations Management Suite (OMS) Log Analytics workspace.</span></span>  <span data-ttu-id="aee7b-105">Loggar för jobbet och dataströmmar för jobbet är synliga i hello Azure-portalen eller PowerShell för enskilda jobb och detta kan du tooperform enkel undersökningar.</span><span class="sxs-lookup"><span data-stu-id="aee7b-105">Job logs and job streams are visible in hello Azure portal, or with PowerShell, for individual jobs and this allows you tooperform simple investigations.</span></span> <span data-ttu-id="aee7b-106">Med Log Analytics kan du nu:</span><span class="sxs-lookup"><span data-stu-id="aee7b-106">Now with Log Analytics you can:</span></span>

* <span data-ttu-id="aee7b-107">Skaffa dig insikter om dina Automation-jobb</span><span class="sxs-lookup"><span data-stu-id="aee7b-107">Get insight on your Automation jobs</span></span>
* <span data-ttu-id="aee7b-108">Utlösare som en e-post eller en avisering baserat på din runbook jobbets status (till exempel misslyckades eller pausas)</span><span class="sxs-lookup"><span data-stu-id="aee7b-108">Trigger an email or alert based on your runbook job status (for example, failed or suspended)</span></span>
* <span data-ttu-id="aee7b-109">Skriva avancerade frågor över jobb-strömmar</span><span class="sxs-lookup"><span data-stu-id="aee7b-109">Write advanced queries across your job streams</span></span>
* <span data-ttu-id="aee7b-110">Korrelera jobb över Automation-konton</span><span class="sxs-lookup"><span data-stu-id="aee7b-110">Correlate jobs across Automation accounts</span></span>
* <span data-ttu-id="aee7b-111">Visualisera dina jobbhistorik över tid</span><span class="sxs-lookup"><span data-stu-id="aee7b-111">Visualize your job history over time</span></span>     

## <a name="prerequisites-and-deployment-considerations"></a><span data-ttu-id="aee7b-112">Krav och överväganden vid distribution</span><span class="sxs-lookup"><span data-stu-id="aee7b-112">Prerequisites and deployment considerations</span></span>
<span data-ttu-id="aee7b-113">skicka din Automation toostart loggar tooLog Analytics, måste du:</span><span class="sxs-lookup"><span data-stu-id="aee7b-113">toostart sending your Automation logs tooLog Analytics, you need:</span></span>

1. <span data-ttu-id="aee7b-114">Hej November 2016 eller senare versionen av [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) (v2.3.0).</span><span class="sxs-lookup"><span data-stu-id="aee7b-114">hello November 2016 or later release of [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) (v2.3.0).</span></span>
2. <span data-ttu-id="aee7b-115">Logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="aee7b-115">A Log Analytics workspace.</span></span> <span data-ttu-id="aee7b-116">Mer information finns i [Kom igång med logganalys](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="aee7b-116">For more information, see [Get started with Log Analytics](../log-analytics/log-analytics-get-started.md).</span></span> 
3. <span data-ttu-id="aee7b-117">hello ResourceId för ditt Azure Automation-konto</span><span class="sxs-lookup"><span data-stu-id="aee7b-117">hello ResourceId for your Azure Automation account</span></span>

<span data-ttu-id="aee7b-118">toofind hello ResourceId för Azure Automation-konto och logganalys-arbetsytan, kör följande PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="aee7b-118">toofind hello ResourceId for your Azure Automation account and Log Analytics workspace, run hello following PowerShell:</span></span>

```powershell
# Find hello ResourceId for hello Automation Account
Find-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"

# Find hello ResourceId for hello Log Analytics workspace
Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

<span data-ttu-id="aee7b-119">Om du har flera Automation-konton eller arbetsytor, i hello utdata från hello föregående kommandon, hitta hello *namn* du behöver tooconfigure och kopiera hello värde för *ResourceId*.</span><span class="sxs-lookup"><span data-stu-id="aee7b-119">If you have multiple Automation accounts, or workspaces, in hello output of hello preceding commands, find hello *Name* you need tooconfigure and copy hello value for *ResourceId*.</span></span>

<span data-ttu-id="aee7b-120">Om du behöver toofind hello *namn* för Automation-konto i hello Azure portal väljer ditt Automation-konto från hello **Automation-konto** och välj **alla inställningar**.</span><span class="sxs-lookup"><span data-stu-id="aee7b-120">If you need toofind hello *Name* of your Automation account, in hello Azure portal select your Automation account from hello **Automation account** blade and select **All settings**.</span></span>  <span data-ttu-id="aee7b-121">Från hello **alla inställningar** bladet under **kontoinställningar** Välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="aee7b-121">From hello **All settings** blade, under **Account Settings** select **Properties**.</span></span>  <span data-ttu-id="aee7b-122">I hello **egenskaper** bladet kan du Observera värdena.</span><span class="sxs-lookup"><span data-stu-id="aee7b-122">In hello **Properties** blade, you can note these values.</span></span><br> <span data-ttu-id="aee7b-123">![Egenskaper för Automation-konto](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).</span><span class="sxs-lookup"><span data-stu-id="aee7b-123">![Automation Account properties](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).</span></span>

## <a name="set-up-integration-with-log-analytics"></a><span data-ttu-id="aee7b-124">Ställa in integration med logganalys</span><span class="sxs-lookup"><span data-stu-id="aee7b-124">Set up integration with Log Analytics</span></span>
1. <span data-ttu-id="aee7b-125">På datorn, startar **Windows PowerShell** från hello **starta** skärmen.</span><span class="sxs-lookup"><span data-stu-id="aee7b-125">On your computer, start **Windows PowerShell** from hello **Start** screen.</span></span>  
2. <span data-ttu-id="aee7b-126">Kopiera och klistra in följande PowerShell hello och redigera hello värde för hello `$workspaceId` och `$automationAccountId`.</span><span class="sxs-lookup"><span data-stu-id="aee7b-126">Copy and paste hello following PowerShell, and edit hello value for hello `$workspaceId` and `$automationAccountId`.</span></span>  <span data-ttu-id="aee7b-127">För hello `-Environment` parameter, giltiga värden är *AzureCloud* eller *AzureUSGovernment* beroende på hello molnmiljö du arbetar med.</span><span class="sxs-lookup"><span data-stu-id="aee7b-127">For hello `-Environment` parameter, valid values are *AzureCloud* or *AzureUSGovernment* depending on hello cloud environment you are working in.</span></span>     

```powershell
[cmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$Environment="AzureCloud"
    )

#Check toosee which cloud environment toosign into.
Switch ($Environment)
   {
       "AzureCloud" {Login-AzureRmAccount}
       "AzureUSGovernment" {Login-AzureRmAccount -EnvironmentName AzureUSGovernment} 
   }

# if you have one Log Analytics workspace you can use hello following command tooget hello resource id of hello workspace
$workspaceId = (Get-AzureRmOperationalInsightsWorkspace).ResourceId

$automationAccountId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.AUTOMATION/ACCOUNTS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled $true

```

<span data-ttu-id="aee7b-128">När du har kört skriptet visas poster i logganalys inom 10 minuter efter nya JobLogs eller JobStreams skrivs.</span><span class="sxs-lookup"><span data-stu-id="aee7b-128">After running this script, you will see records in Log Analytics within 10 minutes of new JobLogs or JobStreams being written.</span></span>

<span data-ttu-id="aee7b-129">toosee hello loggar kör hello följande sökfrågor logganalys loggen:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`</span><span class="sxs-lookup"><span data-stu-id="aee7b-129">toosee hello logs, run hello following query in Log Analytics log search: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`</span></span>

### <a name="verify-configuration"></a><span data-ttu-id="aee7b-130">Verifiera konfigurationen</span><span class="sxs-lookup"><span data-stu-id="aee7b-130">Verify configuration</span></span>
<span data-ttu-id="aee7b-131">tooconfirm som skickar ditt Automation-konto loggar tooyour logganalys-arbetsytan, kontrollera att diagnostik är korrekt inställt på hello Automation-konto med hjälp av följande PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="aee7b-131">tooconfirm that your Automation account is sending logs tooyour Log Analytics workspace, check that diagnostics are set correctly on hello Automation account using hello following PowerShell:</span></span>

```powershell
[cmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$Environment="AzureCloud"
    )

#Check toosee which cloud environment toosign into.
Switch ($Environment)
   {
       "AzureCloud" {Login-AzureRmAccount}
       "AzureUSGovernment" {Login-AzureRmAccount -EnvironmentName AzureUSGovernment} 
   }
# if you have one Log Analytics workspace you can use hello following command tooget hello resource id of hello workspace
$workspaceId = (Get-AzureRmOperationalInsightsWorkspace).ResourceId

$automationAccountId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.AUTOMATION/ACCOUNTS/DEMO" 

Get-AzureRmDiagnosticSetting -ResourceId $automationAccountId
```

<span data-ttu-id="aee7b-132">Se till att i hello utdata:</span><span class="sxs-lookup"><span data-stu-id="aee7b-132">In hello output ensure that:</span></span>
+ <span data-ttu-id="aee7b-133">Under *loggar*, hello värde för *aktiverad* är *SANT*</span><span class="sxs-lookup"><span data-stu-id="aee7b-133">Under *Logs*, hello value for *Enabled* is *True*</span></span>
+ <span data-ttu-id="aee7b-134">Hej värdet för *WorkspaceId* anges toohello ResourceId för logganalys-arbetsytan</span><span class="sxs-lookup"><span data-stu-id="aee7b-134">hello value of *WorkspaceId* is set toohello ResourceId of your Log Analytics workspace</span></span>


## <a name="log-analytics-records"></a><span data-ttu-id="aee7b-135">Log Analytics-poster</span><span class="sxs-lookup"><span data-stu-id="aee7b-135">Log Analytics records</span></span>
<span data-ttu-id="aee7b-136">Diagnostik från Azure Automation skapar två typer av poster i logganalys och märks **typ = AzureDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="aee7b-136">Diagnostics from Azure Automation creates two types of records in Log Analytics and are tagged as **Type=AzureDiagnostics**.</span></span>

### <a name="job-logs"></a><span data-ttu-id="aee7b-137">Jobbloggar</span><span class="sxs-lookup"><span data-stu-id="aee7b-137">Job Logs</span></span>
| <span data-ttu-id="aee7b-138">Egenskap</span><span class="sxs-lookup"><span data-stu-id="aee7b-138">Property</span></span> | <span data-ttu-id="aee7b-139">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="aee7b-139">Description</span></span> |
| --- | --- |
| <span data-ttu-id="aee7b-140">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="aee7b-140">TimeGenerated</span></span> |<span data-ttu-id="aee7b-141">Datum och tid när hello runbook-jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="aee7b-141">Date and time when hello runbook job executed.</span></span> |
| <span data-ttu-id="aee7b-142">RunbookName_s</span><span class="sxs-lookup"><span data-stu-id="aee7b-142">RunbookName_s</span></span> |<span data-ttu-id="aee7b-143">hello namnet på hello runbook.</span><span class="sxs-lookup"><span data-stu-id="aee7b-143">hello name of hello runbook.</span></span> |
| <span data-ttu-id="aee7b-144">Caller_s</span><span class="sxs-lookup"><span data-stu-id="aee7b-144">Caller_s</span></span> |<span data-ttu-id="aee7b-145">Vem som initierat hello igen.</span><span class="sxs-lookup"><span data-stu-id="aee7b-145">Who initiated hello operation.</span></span>  <span data-ttu-id="aee7b-146">Möjliga värden är antingen en e-postadress eller ett system för schemalagda jobb.</span><span class="sxs-lookup"><span data-stu-id="aee7b-146">Possible values are either an email address or system for scheduled jobs.</span></span> |
| <span data-ttu-id="aee7b-147">Tenant_g</span><span class="sxs-lookup"><span data-stu-id="aee7b-147">Tenant_g</span></span> | <span data-ttu-id="aee7b-148">GUID som identifierar hello-klient för hello anroparen.</span><span class="sxs-lookup"><span data-stu-id="aee7b-148">GUID that identifies hello tenant for hello Caller.</span></span> |
| <span data-ttu-id="aee7b-149">JobId_g</span><span class="sxs-lookup"><span data-stu-id="aee7b-149">JobId_g</span></span> |<span data-ttu-id="aee7b-150">GUID som är hello-Id för hello runbook-jobbet.</span><span class="sxs-lookup"><span data-stu-id="aee7b-150">GUID that is hello Id of hello runbook job.</span></span> |
| <span data-ttu-id="aee7b-151">ResultType</span><span class="sxs-lookup"><span data-stu-id="aee7b-151">ResultType</span></span> |<span data-ttu-id="aee7b-152">hello status för hello runbook-jobbet.</span><span class="sxs-lookup"><span data-stu-id="aee7b-152">hello status of hello runbook job.</span></span>  <span data-ttu-id="aee7b-153">Möjliga värden:</span><span class="sxs-lookup"><span data-stu-id="aee7b-153">Possible values are:</span></span><br><span data-ttu-id="aee7b-154">- Startad</span><span class="sxs-lookup"><span data-stu-id="aee7b-154">- Started</span></span><br><span data-ttu-id="aee7b-155">- Stoppad</span><span class="sxs-lookup"><span data-stu-id="aee7b-155">- Stopped</span></span><br><span data-ttu-id="aee7b-156">-Pausad</span><span class="sxs-lookup"><span data-stu-id="aee7b-156">- Suspended</span></span><br><span data-ttu-id="aee7b-157">- Misslyckades</span><span class="sxs-lookup"><span data-stu-id="aee7b-157">- Failed</span></span><br><span data-ttu-id="aee7b-158">-Slutförd</span><span class="sxs-lookup"><span data-stu-id="aee7b-158">- Completed</span></span> |
| <span data-ttu-id="aee7b-159">Kategori</span><span class="sxs-lookup"><span data-stu-id="aee7b-159">Category</span></span> | <span data-ttu-id="aee7b-160">Klassificering av hello typ av data.</span><span class="sxs-lookup"><span data-stu-id="aee7b-160">Classification of hello type of data.</span></span>  <span data-ttu-id="aee7b-161">Hello-värdet är JobLogs för automatisering.</span><span class="sxs-lookup"><span data-stu-id="aee7b-161">For Automation, hello value is JobLogs.</span></span> |
| <span data-ttu-id="aee7b-162">OperationName</span><span class="sxs-lookup"><span data-stu-id="aee7b-162">OperationName</span></span> | <span data-ttu-id="aee7b-163">Anger hello åtgärd utförs i Azure.</span><span class="sxs-lookup"><span data-stu-id="aee7b-163">Specifies hello type of operation performed in Azure.</span></span>  <span data-ttu-id="aee7b-164">Hello-värdet är jobb för automatisering.</span><span class="sxs-lookup"><span data-stu-id="aee7b-164">For Automation, hello value is Job.</span></span> |
| <span data-ttu-id="aee7b-165">Resurs</span><span class="sxs-lookup"><span data-stu-id="aee7b-165">Resource</span></span> | <span data-ttu-id="aee7b-166">Namnet på hello Automation-konto</span><span class="sxs-lookup"><span data-stu-id="aee7b-166">Name of hello Automation account</span></span> |
| <span data-ttu-id="aee7b-167">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="aee7b-167">SourceSystem</span></span> | <span data-ttu-id="aee7b-168">Hur logganalys samlas in hello data.</span><span class="sxs-lookup"><span data-stu-id="aee7b-168">How Log Analytics collected hello data.</span></span> <span data-ttu-id="aee7b-169">Alltid *Azure* för Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="aee7b-169">Always *Azure* for Azure diagnostics.</span></span> |
| <span data-ttu-id="aee7b-170">ResultDescription</span><span class="sxs-lookup"><span data-stu-id="aee7b-170">ResultDescription</span></span> |<span data-ttu-id="aee7b-171">Beskriver hello runbook jobbstatus resultat.</span><span class="sxs-lookup"><span data-stu-id="aee7b-171">Describes hello runbook job result state.</span></span>  <span data-ttu-id="aee7b-172">Möjliga värden:</span><span class="sxs-lookup"><span data-stu-id="aee7b-172">Possible values are:</span></span><br><span data-ttu-id="aee7b-173">-Jobbet har startats</span><span class="sxs-lookup"><span data-stu-id="aee7b-173">- Job is started</span></span><br><span data-ttu-id="aee7b-174">-Jobbet misslyckades</span><span class="sxs-lookup"><span data-stu-id="aee7b-174">- Job Failed</span></span><br><span data-ttu-id="aee7b-175">-Jobbet slutfördes</span><span class="sxs-lookup"><span data-stu-id="aee7b-175">- Job Completed</span></span> |
| <span data-ttu-id="aee7b-176">CorrelationId</span><span class="sxs-lookup"><span data-stu-id="aee7b-176">CorrelationId</span></span> |<span data-ttu-id="aee7b-177">GUID som är hello Korrelations-Id för hello runbook-jobbet.</span><span class="sxs-lookup"><span data-stu-id="aee7b-177">GUID that is hello Correlation Id of hello runbook job.</span></span> |
| <span data-ttu-id="aee7b-178">Resurs-ID</span><span class="sxs-lookup"><span data-stu-id="aee7b-178">ResourceId</span></span> |<span data-ttu-id="aee7b-179">Anger hello Azure Automation-konto resurs-id för hello runbook.</span><span class="sxs-lookup"><span data-stu-id="aee7b-179">Specifies hello Azure Automation account resource id of hello runbook.</span></span> |
| <span data-ttu-id="aee7b-180">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="aee7b-180">SubscriptionId</span></span> | <span data-ttu-id="aee7b-181">hello Azure-prenumeration Id (GUID) för hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="aee7b-181">hello Azure subscription Id (GUID) for hello Automation account.</span></span> |
| <span data-ttu-id="aee7b-182">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="aee7b-182">ResourceGroup</span></span> | <span data-ttu-id="aee7b-183">Namnet på resursgruppen hello för hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="aee7b-183">Name of hello resource group for hello Automation account.</span></span> |
| <span data-ttu-id="aee7b-184">ResourceProvider</span><span class="sxs-lookup"><span data-stu-id="aee7b-184">ResourceProvider</span></span> | <span data-ttu-id="aee7b-185">MICROSOFT. AUTOMATION</span><span class="sxs-lookup"><span data-stu-id="aee7b-185">MICROSOFT.AUTOMATION</span></span> |
| <span data-ttu-id="aee7b-186">ResourceType</span><span class="sxs-lookup"><span data-stu-id="aee7b-186">ResourceType</span></span> | <span data-ttu-id="aee7b-187">AUTOMATIONACCOUNTS</span><span class="sxs-lookup"><span data-stu-id="aee7b-187">AUTOMATIONACCOUNTS</span></span> |


### <a name="job-streams"></a><span data-ttu-id="aee7b-188">Dataströmmar för jobbet</span><span class="sxs-lookup"><span data-stu-id="aee7b-188">Job Streams</span></span>
| <span data-ttu-id="aee7b-189">Egenskap</span><span class="sxs-lookup"><span data-stu-id="aee7b-189">Property</span></span> | <span data-ttu-id="aee7b-190">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="aee7b-190">Description</span></span> |
| --- | --- |
| <span data-ttu-id="aee7b-191">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="aee7b-191">TimeGenerated</span></span> |<span data-ttu-id="aee7b-192">Datum och tid när hello runbook-jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="aee7b-192">Date and time when hello runbook job executed.</span></span> |
| <span data-ttu-id="aee7b-193">RunbookName_s</span><span class="sxs-lookup"><span data-stu-id="aee7b-193">RunbookName_s</span></span> |<span data-ttu-id="aee7b-194">hello namnet på hello runbook.</span><span class="sxs-lookup"><span data-stu-id="aee7b-194">hello name of hello runbook.</span></span> |
| <span data-ttu-id="aee7b-195">Caller_s</span><span class="sxs-lookup"><span data-stu-id="aee7b-195">Caller_s</span></span> |<span data-ttu-id="aee7b-196">Vem som initierat hello igen.</span><span class="sxs-lookup"><span data-stu-id="aee7b-196">Who initiated hello operation.</span></span>  <span data-ttu-id="aee7b-197">Möjliga värden är antingen en e-postadress eller ett system för schemalagda jobb.</span><span class="sxs-lookup"><span data-stu-id="aee7b-197">Possible values are either an email address or system for scheduled jobs.</span></span> |
| <span data-ttu-id="aee7b-198">StreamType_s</span><span class="sxs-lookup"><span data-stu-id="aee7b-198">StreamType_s</span></span> |<span data-ttu-id="aee7b-199">hello typ av jobbström.</span><span class="sxs-lookup"><span data-stu-id="aee7b-199">hello type of job stream.</span></span> <span data-ttu-id="aee7b-200">Möjliga värden:</span><span class="sxs-lookup"><span data-stu-id="aee7b-200">Possible values are:</span></span><br><span data-ttu-id="aee7b-201">- Status</span><span class="sxs-lookup"><span data-stu-id="aee7b-201">-Progress</span></span><br><span data-ttu-id="aee7b-202">- Utdata</span><span class="sxs-lookup"><span data-stu-id="aee7b-202">- Output</span></span><br><span data-ttu-id="aee7b-203">- Varning</span><span class="sxs-lookup"><span data-stu-id="aee7b-203">- Warning</span></span><br><span data-ttu-id="aee7b-204">- Fel</span><span class="sxs-lookup"><span data-stu-id="aee7b-204">- Error</span></span><br><span data-ttu-id="aee7b-205">- Felsökning</span><span class="sxs-lookup"><span data-stu-id="aee7b-205">- Debug</span></span><br><span data-ttu-id="aee7b-206">- Verbose</span><span class="sxs-lookup"><span data-stu-id="aee7b-206">- Verbose</span></span> |
| <span data-ttu-id="aee7b-207">Tenant_g</span><span class="sxs-lookup"><span data-stu-id="aee7b-207">Tenant_g</span></span> | <span data-ttu-id="aee7b-208">GUID som identifierar hello-klient för hello anroparen.</span><span class="sxs-lookup"><span data-stu-id="aee7b-208">GUID that identifies hello tenant for hello Caller.</span></span> |
| <span data-ttu-id="aee7b-209">JobId_g</span><span class="sxs-lookup"><span data-stu-id="aee7b-209">JobId_g</span></span> |<span data-ttu-id="aee7b-210">GUID som är hello-Id för hello runbook-jobbet.</span><span class="sxs-lookup"><span data-stu-id="aee7b-210">GUID that is hello Id of hello runbook job.</span></span> |
| <span data-ttu-id="aee7b-211">ResultType</span><span class="sxs-lookup"><span data-stu-id="aee7b-211">ResultType</span></span> |<span data-ttu-id="aee7b-212">hello status för hello runbook-jobbet.</span><span class="sxs-lookup"><span data-stu-id="aee7b-212">hello status of hello runbook job.</span></span>  <span data-ttu-id="aee7b-213">Möjliga värden:</span><span class="sxs-lookup"><span data-stu-id="aee7b-213">Possible values are:</span></span><br><span data-ttu-id="aee7b-214">-Pågår</span><span class="sxs-lookup"><span data-stu-id="aee7b-214">- In Progress</span></span> |
| <span data-ttu-id="aee7b-215">Kategori</span><span class="sxs-lookup"><span data-stu-id="aee7b-215">Category</span></span> | <span data-ttu-id="aee7b-216">Klassificering av hello typ av data.</span><span class="sxs-lookup"><span data-stu-id="aee7b-216">Classification of hello type of data.</span></span>  <span data-ttu-id="aee7b-217">Hello-värdet är JobStreams för automatisering.</span><span class="sxs-lookup"><span data-stu-id="aee7b-217">For Automation, hello value is JobStreams.</span></span> |
| <span data-ttu-id="aee7b-218">OperationName</span><span class="sxs-lookup"><span data-stu-id="aee7b-218">OperationName</span></span> | <span data-ttu-id="aee7b-219">Anger hello åtgärd utförs i Azure.</span><span class="sxs-lookup"><span data-stu-id="aee7b-219">Specifies hello type of operation performed in Azure.</span></span>  <span data-ttu-id="aee7b-220">Hello-värdet är jobb för automatisering.</span><span class="sxs-lookup"><span data-stu-id="aee7b-220">For Automation, hello value is Job.</span></span> |
| <span data-ttu-id="aee7b-221">Resurs</span><span class="sxs-lookup"><span data-stu-id="aee7b-221">Resource</span></span> | <span data-ttu-id="aee7b-222">Namnet på hello Automation-konto</span><span class="sxs-lookup"><span data-stu-id="aee7b-222">Name of hello Automation account</span></span> |
| <span data-ttu-id="aee7b-223">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="aee7b-223">SourceSystem</span></span> | <span data-ttu-id="aee7b-224">Hur logganalys samlas in hello data.</span><span class="sxs-lookup"><span data-stu-id="aee7b-224">How Log Analytics collected hello data.</span></span> <span data-ttu-id="aee7b-225">Alltid *Azure* för Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="aee7b-225">Always *Azure* for Azure diagnostics.</span></span> |
| <span data-ttu-id="aee7b-226">ResultDescription</span><span class="sxs-lookup"><span data-stu-id="aee7b-226">ResultDescription</span></span> |<span data-ttu-id="aee7b-227">Innehåller hello utdataström från hello runbook.</span><span class="sxs-lookup"><span data-stu-id="aee7b-227">Includes hello output stream from hello runbook.</span></span> |
| <span data-ttu-id="aee7b-228">CorrelationId</span><span class="sxs-lookup"><span data-stu-id="aee7b-228">CorrelationId</span></span> |<span data-ttu-id="aee7b-229">GUID som är hello Korrelations-Id för hello runbook-jobbet.</span><span class="sxs-lookup"><span data-stu-id="aee7b-229">GUID that is hello Correlation Id of hello runbook job.</span></span> |
| <span data-ttu-id="aee7b-230">Resurs-ID</span><span class="sxs-lookup"><span data-stu-id="aee7b-230">ResourceId</span></span> |<span data-ttu-id="aee7b-231">Anger hello Azure Automation-konto resurs-id för hello runbook.</span><span class="sxs-lookup"><span data-stu-id="aee7b-231">Specifies hello Azure Automation account resource id of hello runbook.</span></span> |
| <span data-ttu-id="aee7b-232">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="aee7b-232">SubscriptionId</span></span> | <span data-ttu-id="aee7b-233">hello Azure-prenumeration Id (GUID) för hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="aee7b-233">hello Azure subscription Id (GUID) for hello Automation account.</span></span> |
| <span data-ttu-id="aee7b-234">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="aee7b-234">ResourceGroup</span></span> | <span data-ttu-id="aee7b-235">Namnet på resursgruppen hello för hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="aee7b-235">Name of hello resource group for hello Automation account.</span></span> |
| <span data-ttu-id="aee7b-236">ResourceProvider</span><span class="sxs-lookup"><span data-stu-id="aee7b-236">ResourceProvider</span></span> | <span data-ttu-id="aee7b-237">MICROSOFT. AUTOMATION</span><span class="sxs-lookup"><span data-stu-id="aee7b-237">MICROSOFT.AUTOMATION</span></span> |
| <span data-ttu-id="aee7b-238">ResourceType</span><span class="sxs-lookup"><span data-stu-id="aee7b-238">ResourceType</span></span> | <span data-ttu-id="aee7b-239">AUTOMATIONACCOUNTS</span><span class="sxs-lookup"><span data-stu-id="aee7b-239">AUTOMATIONACCOUNTS</span></span> |

## <a name="viewing-automation-logs-in-log-analytics"></a><span data-ttu-id="aee7b-240">Visa Automation loggar i logganalys</span><span class="sxs-lookup"><span data-stu-id="aee7b-240">Viewing Automation Logs in Log Analytics</span></span>
<span data-ttu-id="aee7b-241">Nu när du har startat skicka din Automation jobb loggar tooLog Analytics kan vi se vad du kan göra med dessa loggar i logganalys.</span><span class="sxs-lookup"><span data-stu-id="aee7b-241">Now that you have started sending your Automation job logs tooLog Analytics, let’s see what you can do with these logs inside Log Analytics.</span></span>

<span data-ttu-id="aee7b-242">toosee hello loggar, kör följande fråga hello:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`</span><span class="sxs-lookup"><span data-stu-id="aee7b-242">toosee hello logs, run hello following query: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`</span></span>

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a><span data-ttu-id="aee7b-243">Skicka ett e-postmeddelande när en runbook-jobb misslyckas eller pausar</span><span class="sxs-lookup"><span data-stu-id="aee7b-243">Send an email when a runbook job fails or suspends</span></span>
<span data-ttu-id="aee7b-244">En av våra främsta kunden frågar avser hello möjlighet toosend ett e-postmeddelande eller en text om något går fel med ett runbook-jobb.</span><span class="sxs-lookup"><span data-stu-id="aee7b-244">One of our top customer asks is for hello ability toosend an email or a text when something goes wrong with a runbook job.</span></span>   

<span data-ttu-id="aee7b-245">toocreate en avisering regeln du börja med att skapa en logg-sökning för hello runbook jobbposter som ska anropa hello avisering.</span><span class="sxs-lookup"><span data-stu-id="aee7b-245">toocreate an alert rule, you start by creating a log search for hello runbook job records that should invoke hello alert.</span></span>  <span data-ttu-id="aee7b-246">Klicka på hello **avisering** knappen toocreate och konfigurera hello varningsregel.</span><span class="sxs-lookup"><span data-stu-id="aee7b-246">Click hello **Alert** button toocreate and configure hello alert rule.</span></span>

1. <span data-ttu-id="aee7b-247">Klicka på sidan Översikt över Log Analytics hello **loggen Sök**.</span><span class="sxs-lookup"><span data-stu-id="aee7b-247">From hello Log Analytics Overview page, click **Log Search**.</span></span>
2. <span data-ttu-id="aee7b-248">Skapa en logg sökfråga för aviseringen genom att skriva hello efter sökning i hello frågefält: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended)` du kan också gruppera efter hello RunbookName med hjälp av:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended) | measure Count() by RunbookName_s`</span><span class="sxs-lookup"><span data-stu-id="aee7b-248">Create a log search query for your alert by typing hello following search into hello query field:  `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended)`  You can also group by hello RunbookName by using: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended) | measure Count() by RunbookName_s`</span></span>   

   <span data-ttu-id="aee7b-249">Om du har lagt upp loggar från mer än en Automation-konto eller prenumeration tooyour arbetsytan kan gruppera du aviseringarna genom prenumerationen och Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="aee7b-249">If you have set up logs from more than one Automation account or subscription tooyour workspace, you can group your alerts by subscription and Automation account.</span></span>  <span data-ttu-id="aee7b-250">Automation-kontonamnet kan härledas från hello fält i hello sökning av JobLogs.</span><span class="sxs-lookup"><span data-stu-id="aee7b-250">Automation account name can be derived from hello Resource field in hello search of JobLogs.</span></span>  
3. <span data-ttu-id="aee7b-251">tooopen hello **lägga till Varningsregeln** klickar du på **avisering** hello överst på hello sidan.</span><span class="sxs-lookup"><span data-stu-id="aee7b-251">tooopen hello **Add Alert Rule** screen, click **Alert** at hello top of hello page.</span></span> <span data-ttu-id="aee7b-252">Mer information om hello alternativ tooconfigure hello avisering finns [aviseringar i logganalys](../log-analytics/log-analytics-alerts.md#alert-rules).</span><span class="sxs-lookup"><span data-stu-id="aee7b-252">For further details on hello options tooconfigure hello alert, see [Alerts in Log Analytics](../log-analytics/log-analytics-alerts.md#alert-rules).</span></span>

### <a name="find-all-jobs-that-have-completed-with-errors"></a><span data-ttu-id="aee7b-253">Hitta alla jobb som har slutförts med fel</span><span class="sxs-lookup"><span data-stu-id="aee7b-253">Find all jobs that have completed with errors</span></span>
<span data-ttu-id="aee7b-254">Dessutom tooalerting på fel du hittar när en runbook-jobbet har en icke-avslutande fel.</span><span class="sxs-lookup"><span data-stu-id="aee7b-254">In addition tooalerting on failures, you can find when a runbook job has a non-terminating error.</span></span> <span data-ttu-id="aee7b-255">I dessa fall PowerShell genererar ett fel uppstod när strömmen, men hello-avslutande fel inte orsaka jobbet-toosuspend eller misslyckas.</span><span class="sxs-lookup"><span data-stu-id="aee7b-255">In these cases PowerShell produces an error stream, but hello non-terminating errors do not cause your job toosuspend or fail.</span></span>    

1. <span data-ttu-id="aee7b-256">Klicka på i logganalys-arbetsytan **loggen Sök**.</span><span class="sxs-lookup"><span data-stu-id="aee7b-256">In your Log Analytics workspace, click **Log Search**.</span></span>
2. <span data-ttu-id="aee7b-257">Skriv i hello frågan `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams StreamType_s=Error | measure count() by JobId_g` och klicka sedan på **Sök**.</span><span class="sxs-lookup"><span data-stu-id="aee7b-257">In hello query field, type `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams StreamType_s=Error | measure count() by JobId_g` and then click **Search**.</span></span>

### <a name="view-job-streams-for-a-job"></a><span data-ttu-id="aee7b-258">Visa jobb dataströmmar för ett jobb</span><span class="sxs-lookup"><span data-stu-id="aee7b-258">View job streams for a job</span></span>
<span data-ttu-id="aee7b-259">Du kanske också vill toolook till hello jobbet dataströmmar när du felsöker ett jobb.</span><span class="sxs-lookup"><span data-stu-id="aee7b-259">When you are debugging a job, you may also want toolook into hello job streams.</span></span>  <span data-ttu-id="aee7b-260">hello visar följande fråga alla hello dataströmmar för ett enda projekt med GUID 2ebd22ea e05e-4eb9 - 9d 76-d73cbd4356e0:</span><span class="sxs-lookup"><span data-stu-id="aee7b-260">hello following query shows all hello streams for a single job with GUID 2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0:</span></span>   

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams JobId_g="2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort TimeGenerated | select ResultDescription`

### <a name="view-historical-job-status"></a><span data-ttu-id="aee7b-261">Visa historiska jobbstatus</span><span class="sxs-lookup"><span data-stu-id="aee7b-261">View historical job status</span></span>
<span data-ttu-id="aee7b-262">Slutligen kan du toovisualize din jobbhistorik över tid.</span><span class="sxs-lookup"><span data-stu-id="aee7b-262">Finally, you may want toovisualize your job history over time.</span></span>  <span data-ttu-id="aee7b-263">Du kan använda den här frågan toosearch hello status för jobb med tiden.</span><span class="sxs-lookup"><span data-stu-id="aee7b-263">You can use this query toosearch for hello status of your jobs over time.</span></span>

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs NOT(ResultType="started") | measure Count() by ResultType interval 1hour`  
<br> <span data-ttu-id="aee7b-264">![OMS historiska jobbet Status diagram](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)</span><span class="sxs-lookup"><span data-stu-id="aee7b-264">![OMS Historical Job Status Chart](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)</span></span><br>

## <a name="summary"></a><span data-ttu-id="aee7b-265">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="aee7b-265">Summary</span></span>
<span data-ttu-id="aee7b-266">Du kan få bättre inblick i hello status för ditt Automation-jobb efter genom att skicka ditt Automation jobbet status och dataströmmen data tooLog Analytics:</span><span class="sxs-lookup"><span data-stu-id="aee7b-266">By sending your Automation job status and stream data tooLog Analytics, you can get better insight into hello status of your Automation jobs by:</span></span>
+ <span data-ttu-id="aee7b-267">Du konfigurerar aviseringar toonotify du när det uppstår problem</span><span class="sxs-lookup"><span data-stu-id="aee7b-267">Setting up alerts toonotify you when there is an issue</span></span>
+ <span data-ttu-id="aee7b-268">Använda anpassade vyer och Sök frågor toovisualize relaterade din runbook-resultat, runbook jobbstatus och andra viktiga indikatorer eller mått.</span><span class="sxs-lookup"><span data-stu-id="aee7b-268">Using custom views and search queries toovisualize your runbook results, runbook job status, and other related key indicators or metrics.</span></span>  

<span data-ttu-id="aee7b-269">Log Analytics ger bättre operativa synlighet tooyour Automation-jobb och kan hjälpa adress incidenter snabbare.</span><span class="sxs-lookup"><span data-stu-id="aee7b-269">Log Analytics provides greater operational visibility tooyour Automation jobs and can help address incidents quicker.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="aee7b-270">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aee7b-270">Next steps</span></span>
* <span data-ttu-id="aee7b-271">toolearn mer information om hur tooconstruct olika sökfrågor och granska hello Automation jobb loggar med Log Analytics finns [logga sökningar i logganalys](../log-analytics/log-analytics-log-searches.md)</span><span class="sxs-lookup"><span data-stu-id="aee7b-271">toolearn more about how tooconstruct different search queries and review hello Automation job logs with Log Analytics, see [Log searches in Log Analytics](../log-analytics/log-analytics-log-searches.md)</span></span>
* <span data-ttu-id="aee7b-272">hur toocreate och hämta utdata och felmeddelanden från runbooks, se toounderstand [Runbook utdata och meddelanden](automation-runbook-output-and-messages.md)</span><span class="sxs-lookup"><span data-stu-id="aee7b-272">toounderstand how toocreate and retrieve output and error messages from runbooks, see [Runbook output and messages](automation-runbook-output-and-messages.md)</span></span>
* <span data-ttu-id="aee7b-273">Mer om runbook-körningen hur toomonitor runbook-jobb och annan teknisk information finns i toolearn [spåra ett runbook-jobb](automation-runbook-execution.md)</span><span class="sxs-lookup"><span data-stu-id="aee7b-273">toolearn more about runbook execution, how toomonitor runbook jobs, and other technical details, see [Track a runbook job](automation-runbook-execution.md)</span></span>
* <span data-ttu-id="aee7b-274">toolearn mer om logganalys OMS och datakällor för samlingen, se [insamling av Azure storage-data i logganalys-översikt](../log-analytics/log-analytics-azure-storage.md)</span><span class="sxs-lookup"><span data-stu-id="aee7b-274">toolearn more about OMS Log Analytics and data collection sources, see [Collecting Azure storage data in Log Analytics overview](../log-analytics/log-analytics-azure-storage.md)</span></span>
