---
title: Vidarebefordra Azure Automation jobbdata till OMS Log Analytics | Microsoft Docs
description: "Den här artikeln visar hur du skicka jobbstatus och runbook-jobbet strömmar till logganalys för Microsoft Operations Management Suite att ge ytterligare insikter och hantering."
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
ms.openlocfilehash: 2c0ca7fc332963e5a5db3c20c400ed877ae0cc54
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="forward-job-status-and-job-streams-from-automation-to-log-analytics-oms"></a><span data-ttu-id="cacce-103">Vidarebefordra jobbstatus och jobbet strömmar från Automation till logganalys (OMS)</span><span class="sxs-lookup"><span data-stu-id="cacce-103">Forward job status and job streams from Automation to Log Analytics (OMS)</span></span>
<span data-ttu-id="cacce-104">Automatisering kan skicka runbook jobbet status och jobbstatus strömmar till Microsoft Operations Management Suite (OMS) logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="cacce-104">Automation can send runbook job status and job streams to your Microsoft Operations Management Suite (OMS) Log Analytics workspace.</span></span>  <span data-ttu-id="cacce-105">Jobbet loggar och dataströmmar för jobbet är synliga i Azure-portalen eller med PowerShell, för enskilda jobb och detta kan du utföra enkla undersökningar.</span><span class="sxs-lookup"><span data-stu-id="cacce-105">Job logs and job streams are visible in the Azure portal, or with PowerShell, for individual jobs and this allows you to perform simple investigations.</span></span> <span data-ttu-id="cacce-106">Med Log Analytics kan du nu:</span><span class="sxs-lookup"><span data-stu-id="cacce-106">Now with Log Analytics you can:</span></span>

* <span data-ttu-id="cacce-107">Skaffa dig insikter om dina Automation-jobb</span><span class="sxs-lookup"><span data-stu-id="cacce-107">Get insight on your Automation jobs</span></span>
* <span data-ttu-id="cacce-108">Utlösare som en e-post eller en avisering baserat på din runbook jobbets status (till exempel misslyckades eller pausas)</span><span class="sxs-lookup"><span data-stu-id="cacce-108">Trigger an email or alert based on your runbook job status (for example, failed or suspended)</span></span>
* <span data-ttu-id="cacce-109">Skriva avancerade frågor över jobb-strömmar</span><span class="sxs-lookup"><span data-stu-id="cacce-109">Write advanced queries across your job streams</span></span>
* <span data-ttu-id="cacce-110">Korrelera jobb över Automation-konton</span><span class="sxs-lookup"><span data-stu-id="cacce-110">Correlate jobs across Automation accounts</span></span>
* <span data-ttu-id="cacce-111">Visualisera dina jobbhistorik över tid</span><span class="sxs-lookup"><span data-stu-id="cacce-111">Visualize your job history over time</span></span>     

## <a name="prerequisites-and-deployment-considerations"></a><span data-ttu-id="cacce-112">Krav och överväganden vid distribution</span><span class="sxs-lookup"><span data-stu-id="cacce-112">Prerequisites and deployment considerations</span></span>
<span data-ttu-id="cacce-113">Om du vill börja skicka Automation-loggar till logganalys, behöver du:</span><span class="sxs-lookup"><span data-stu-id="cacce-113">To start sending your Automation logs to Log Analytics, you need:</span></span>

1. <span data-ttu-id="cacce-114">November 2016 eller senare versionen av [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) (v2.3.0).</span><span class="sxs-lookup"><span data-stu-id="cacce-114">The November 2016 or later release of [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) (v2.3.0).</span></span>
2. <span data-ttu-id="cacce-115">Logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="cacce-115">A Log Analytics workspace.</span></span> <span data-ttu-id="cacce-116">Mer information finns i [Kom igång med logganalys](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="cacce-116">For more information, see [Get started with Log Analytics](../log-analytics/log-analytics-get-started.md).</span></span> 
3. <span data-ttu-id="cacce-117">Resurs-ID för Azure Automation-konto</span><span class="sxs-lookup"><span data-stu-id="cacce-117">The ResourceId for your Azure Automation account</span></span>

<span data-ttu-id="cacce-118">Kör följande PowerShell för att hitta resurs-ID för Azure Automation-konto och logganalys-arbetsyta:</span><span class="sxs-lookup"><span data-stu-id="cacce-118">To find the ResourceId for your Azure Automation account and Log Analytics workspace, run the following PowerShell:</span></span>

```powershell
# Find the ResourceId for the Automation Account
Find-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"

# Find the ResourceId for the Log Analytics workspace
Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

<span data-ttu-id="cacce-119">Om du har flera Automation-konton eller arbetsytor i utdata från de föregående kommandona hitta den *namn* måste du konfigurera och kopiera värdet för *ResourceId*.</span><span class="sxs-lookup"><span data-stu-id="cacce-119">If you have multiple Automation accounts, or workspaces, in the output of the preceding commands, find the *Name* you need to configure and copy the value for *ResourceId*.</span></span>

<span data-ttu-id="cacce-120">Om du vill söka efter den *namn* ditt Automation-konto i Azure portal väljer du ditt Automation-konto från den **Automation-konto** och välj **alla inställningar** .</span><span class="sxs-lookup"><span data-stu-id="cacce-120">If you need to find the *Name* of your Automation account, in the Azure portal select your Automation account from the **Automation account** blade and select **All settings**.</span></span>  <span data-ttu-id="cacce-121">Från bladet **Alla inställningar** väljer du **Egenskaper** under **Kontoinställningar**.</span><span class="sxs-lookup"><span data-stu-id="cacce-121">From the **All settings** blade, under **Account Settings** select **Properties**.</span></span>  <span data-ttu-id="cacce-122">Notera dessa värden i bladet **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="cacce-122">In the **Properties** blade, you can note these values.</span></span><br> <span data-ttu-id="cacce-123">![Egenskaper för Automation-konto](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).</span><span class="sxs-lookup"><span data-stu-id="cacce-123">![Automation Account properties](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).</span></span>

## <a name="set-up-integration-with-log-analytics"></a><span data-ttu-id="cacce-124">Ställa in integration med logganalys</span><span class="sxs-lookup"><span data-stu-id="cacce-124">Set up integration with Log Analytics</span></span>
1. <span data-ttu-id="cacce-125">På datorn, startar **Windows PowerShell** från den **starta** skärmen.</span><span class="sxs-lookup"><span data-stu-id="cacce-125">On your computer, start **Windows PowerShell** from the **Start** screen.</span></span>  
2. <span data-ttu-id="cacce-126">Kopiera och klistra in följande PowerShell och redigera värdet för den `$workspaceId` och `$automationAccountId`.</span><span class="sxs-lookup"><span data-stu-id="cacce-126">Copy and paste the following PowerShell, and edit the value for the `$workspaceId` and `$automationAccountId`.</span></span>  <span data-ttu-id="cacce-127">För den `-Environment` parameter, giltiga värden är *AzureCloud* eller *AzureUSGovernment* beroende på hur du arbetar i molnmiljön.</span><span class="sxs-lookup"><span data-stu-id="cacce-127">For the `-Environment` parameter, valid values are *AzureCloud* or *AzureUSGovernment* depending on the cloud environment you are working in.</span></span>     

```powershell
[cmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$Environment="AzureCloud"
    )

#Check to see which cloud environment to sign into.
Switch ($Environment)
   {
       "AzureCloud" {Login-AzureRmAccount}
       "AzureUSGovernment" {Login-AzureRmAccount -EnvironmentName AzureUSGovernment} 
   }

# if you have one Log Analytics workspace you can use the following command to get the resource id of the workspace
$workspaceId = (Get-AzureRmOperationalInsightsWorkspace).ResourceId

$automationAccountId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.AUTOMATION/ACCOUNTS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled $true

```

<span data-ttu-id="cacce-128">När du har kört skriptet visas poster i logganalys inom 10 minuter efter nya JobLogs eller JobStreams skrivs.</span><span class="sxs-lookup"><span data-stu-id="cacce-128">After running this script, you will see records in Log Analytics within 10 minutes of new JobLogs or JobStreams being written.</span></span>

<span data-ttu-id="cacce-129">Kör följande fråga om du vill se loggarna i logganalys loggen sökning:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`</span><span class="sxs-lookup"><span data-stu-id="cacce-129">To see the logs, run the following query in Log Analytics log search: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`</span></span>

### <a name="verify-configuration"></a><span data-ttu-id="cacce-130">Verifiera konfigurationen</span><span class="sxs-lookup"><span data-stu-id="cacce-130">Verify configuration</span></span>
<span data-ttu-id="cacce-131">Kontrollera att diagnostik är korrekt inställt på Automation-kontot med följande PowerShell för att bekräfta att ditt Automation-konto är skicka loggar till logganalys-arbetsytan:</span><span class="sxs-lookup"><span data-stu-id="cacce-131">To confirm that your Automation account is sending logs to your Log Analytics workspace, check that diagnostics are set correctly on the Automation account using the following PowerShell:</span></span>

```powershell
[cmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$Environment="AzureCloud"
    )

#Check to see which cloud environment to sign into.
Switch ($Environment)
   {
       "AzureCloud" {Login-AzureRmAccount}
       "AzureUSGovernment" {Login-AzureRmAccount -EnvironmentName AzureUSGovernment} 
   }
# if you have one Log Analytics workspace you can use the following command to get the resource id of the workspace
$workspaceId = (Get-AzureRmOperationalInsightsWorkspace).ResourceId

$automationAccountId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.AUTOMATION/ACCOUNTS/DEMO" 

Get-AzureRmDiagnosticSetting -ResourceId $automationAccountId
```

<span data-ttu-id="cacce-132">Se till att i utdata:</span><span class="sxs-lookup"><span data-stu-id="cacce-132">In the output ensure that:</span></span>
+ <span data-ttu-id="cacce-133">Under *loggar*, värdet för *aktiverad* är *SANT*</span><span class="sxs-lookup"><span data-stu-id="cacce-133">Under *Logs*, the value for *Enabled* is *True*</span></span>
+ <span data-ttu-id="cacce-134">Värdet för *WorkspaceId* är inställd på ResourceId för logganalys-arbetsytan</span><span class="sxs-lookup"><span data-stu-id="cacce-134">The value of *WorkspaceId* is set to the ResourceId of your Log Analytics workspace</span></span>


## <a name="log-analytics-records"></a><span data-ttu-id="cacce-135">Log Analytics-poster</span><span class="sxs-lookup"><span data-stu-id="cacce-135">Log Analytics records</span></span>
<span data-ttu-id="cacce-136">Diagnostik från Azure Automation skapar två typer av poster i logganalys och märks **typ = AzureDiagnostics**.</span><span class="sxs-lookup"><span data-stu-id="cacce-136">Diagnostics from Azure Automation creates two types of records in Log Analytics and are tagged as **Type=AzureDiagnostics**.</span></span>

### <a name="job-logs"></a><span data-ttu-id="cacce-137">Jobbloggar</span><span class="sxs-lookup"><span data-stu-id="cacce-137">Job Logs</span></span>
| <span data-ttu-id="cacce-138">Egenskap</span><span class="sxs-lookup"><span data-stu-id="cacce-138">Property</span></span> | <span data-ttu-id="cacce-139">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cacce-139">Description</span></span> |
| --- | --- |
| <span data-ttu-id="cacce-140">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="cacce-140">TimeGenerated</span></span> |<span data-ttu-id="cacce-141">Datum och tid då runbook-jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="cacce-141">Date and time when the runbook job executed.</span></span> |
| <span data-ttu-id="cacce-142">RunbookName_s</span><span class="sxs-lookup"><span data-stu-id="cacce-142">RunbookName_s</span></span> |<span data-ttu-id="cacce-143">Anger namnet på runbooken.</span><span class="sxs-lookup"><span data-stu-id="cacce-143">The name of the runbook.</span></span> |
| <span data-ttu-id="cacce-144">Caller_s</span><span class="sxs-lookup"><span data-stu-id="cacce-144">Caller_s</span></span> |<span data-ttu-id="cacce-145">Den som initierade åtgärden.</span><span class="sxs-lookup"><span data-stu-id="cacce-145">Who initiated the operation.</span></span>  <span data-ttu-id="cacce-146">Möjliga värden är antingen en e-postadress eller ett system för schemalagda jobb.</span><span class="sxs-lookup"><span data-stu-id="cacce-146">Possible values are either an email address or system for scheduled jobs.</span></span> |
| <span data-ttu-id="cacce-147">Tenant_g</span><span class="sxs-lookup"><span data-stu-id="cacce-147">Tenant_g</span></span> | <span data-ttu-id="cacce-148">GUID som identifierar klienten för anroparen.</span><span class="sxs-lookup"><span data-stu-id="cacce-148">GUID that identifies the tenant for the Caller.</span></span> |
| <span data-ttu-id="cacce-149">JobId_g</span><span class="sxs-lookup"><span data-stu-id="cacce-149">JobId_g</span></span> |<span data-ttu-id="cacce-150">GUID som är Id för runbook-jobbet.</span><span class="sxs-lookup"><span data-stu-id="cacce-150">GUID that is the Id of the runbook job.</span></span> |
| <span data-ttu-id="cacce-151">ResultType</span><span class="sxs-lookup"><span data-stu-id="cacce-151">ResultType</span></span> |<span data-ttu-id="cacce-152">Status för runbookjobbet.</span><span class="sxs-lookup"><span data-stu-id="cacce-152">The status of the runbook job.</span></span>  <span data-ttu-id="cacce-153">Möjliga värden:</span><span class="sxs-lookup"><span data-stu-id="cacce-153">Possible values are:</span></span><br><span data-ttu-id="cacce-154">- Startad</span><span class="sxs-lookup"><span data-stu-id="cacce-154">- Started</span></span><br><span data-ttu-id="cacce-155">- Stoppad</span><span class="sxs-lookup"><span data-stu-id="cacce-155">- Stopped</span></span><br><span data-ttu-id="cacce-156">-Pausad</span><span class="sxs-lookup"><span data-stu-id="cacce-156">- Suspended</span></span><br><span data-ttu-id="cacce-157">- Misslyckades</span><span class="sxs-lookup"><span data-stu-id="cacce-157">- Failed</span></span><br><span data-ttu-id="cacce-158">-Slutförd</span><span class="sxs-lookup"><span data-stu-id="cacce-158">- Completed</span></span> |
| <span data-ttu-id="cacce-159">Kategori</span><span class="sxs-lookup"><span data-stu-id="cacce-159">Category</span></span> | <span data-ttu-id="cacce-160">Klassificering av typ av data.</span><span class="sxs-lookup"><span data-stu-id="cacce-160">Classification of the type of data.</span></span>  <span data-ttu-id="cacce-161">För Automation är värdet JobLogs.</span><span class="sxs-lookup"><span data-stu-id="cacce-161">For Automation, the value is JobLogs.</span></span> |
| <span data-ttu-id="cacce-162">OperationName</span><span class="sxs-lookup"><span data-stu-id="cacce-162">OperationName</span></span> | <span data-ttu-id="cacce-163">Anger åtgärdstypen i Azure.</span><span class="sxs-lookup"><span data-stu-id="cacce-163">Specifies the type of operation performed in Azure.</span></span>  <span data-ttu-id="cacce-164">Värdet är jobb för automatisering.</span><span class="sxs-lookup"><span data-stu-id="cacce-164">For Automation, the value is Job.</span></span> |
| <span data-ttu-id="cacce-165">Resurs</span><span class="sxs-lookup"><span data-stu-id="cacce-165">Resource</span></span> | <span data-ttu-id="cacce-166">Namnet på det Automation-kontot</span><span class="sxs-lookup"><span data-stu-id="cacce-166">Name of the Automation account</span></span> |
| <span data-ttu-id="cacce-167">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="cacce-167">SourceSystem</span></span> | <span data-ttu-id="cacce-168">Hur logganalys insamlade data.</span><span class="sxs-lookup"><span data-stu-id="cacce-168">How Log Analytics collected the data.</span></span> <span data-ttu-id="cacce-169">Alltid *Azure* för Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="cacce-169">Always *Azure* for Azure diagnostics.</span></span> |
| <span data-ttu-id="cacce-170">ResultDescription</span><span class="sxs-lookup"><span data-stu-id="cacce-170">ResultDescription</span></span> |<span data-ttu-id="cacce-171">Beskriver jobbstatusen för runbook.</span><span class="sxs-lookup"><span data-stu-id="cacce-171">Describes the runbook job result state.</span></span>  <span data-ttu-id="cacce-172">Möjliga värden:</span><span class="sxs-lookup"><span data-stu-id="cacce-172">Possible values are:</span></span><br><span data-ttu-id="cacce-173">-Jobbet har startats</span><span class="sxs-lookup"><span data-stu-id="cacce-173">- Job is started</span></span><br><span data-ttu-id="cacce-174">-Jobbet misslyckades</span><span class="sxs-lookup"><span data-stu-id="cacce-174">- Job Failed</span></span><br><span data-ttu-id="cacce-175">-Jobbet slutfördes</span><span class="sxs-lookup"><span data-stu-id="cacce-175">- Job Completed</span></span> |
| <span data-ttu-id="cacce-176">CorrelationId</span><span class="sxs-lookup"><span data-stu-id="cacce-176">CorrelationId</span></span> |<span data-ttu-id="cacce-177">GUID som är korrelations-Id för runbook-jobbet.</span><span class="sxs-lookup"><span data-stu-id="cacce-177">GUID that is the Correlation Id of the runbook job.</span></span> |
| <span data-ttu-id="cacce-178">Resurs-ID</span><span class="sxs-lookup"><span data-stu-id="cacce-178">ResourceId</span></span> |<span data-ttu-id="cacce-179">Anger Azure Automation-konto resurs-id för runbook.</span><span class="sxs-lookup"><span data-stu-id="cacce-179">Specifies the Azure Automation account resource id of the runbook.</span></span> |
| <span data-ttu-id="cacce-180">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="cacce-180">SubscriptionId</span></span> | <span data-ttu-id="cacce-181">Azure-prenumerationen Id (GUID) för Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="cacce-181">The Azure subscription Id (GUID) for the Automation account.</span></span> |
| <span data-ttu-id="cacce-182">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="cacce-182">ResourceGroup</span></span> | <span data-ttu-id="cacce-183">Namnet på resursgruppen för Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="cacce-183">Name of the resource group for the Automation account.</span></span> |
| <span data-ttu-id="cacce-184">ResourceProvider</span><span class="sxs-lookup"><span data-stu-id="cacce-184">ResourceProvider</span></span> | <span data-ttu-id="cacce-185">MICROSOFT. AUTOMATION</span><span class="sxs-lookup"><span data-stu-id="cacce-185">MICROSOFT.AUTOMATION</span></span> |
| <span data-ttu-id="cacce-186">ResourceType</span><span class="sxs-lookup"><span data-stu-id="cacce-186">ResourceType</span></span> | <span data-ttu-id="cacce-187">AUTOMATIONACCOUNTS</span><span class="sxs-lookup"><span data-stu-id="cacce-187">AUTOMATIONACCOUNTS</span></span> |


### <a name="job-streams"></a><span data-ttu-id="cacce-188">Dataströmmar för jobbet</span><span class="sxs-lookup"><span data-stu-id="cacce-188">Job Streams</span></span>
| <span data-ttu-id="cacce-189">Egenskap</span><span class="sxs-lookup"><span data-stu-id="cacce-189">Property</span></span> | <span data-ttu-id="cacce-190">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cacce-190">Description</span></span> |
| --- | --- |
| <span data-ttu-id="cacce-191">TimeGenerated</span><span class="sxs-lookup"><span data-stu-id="cacce-191">TimeGenerated</span></span> |<span data-ttu-id="cacce-192">Datum och tid då runbook-jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="cacce-192">Date and time when the runbook job executed.</span></span> |
| <span data-ttu-id="cacce-193">RunbookName_s</span><span class="sxs-lookup"><span data-stu-id="cacce-193">RunbookName_s</span></span> |<span data-ttu-id="cacce-194">Anger namnet på runbooken.</span><span class="sxs-lookup"><span data-stu-id="cacce-194">The name of the runbook.</span></span> |
| <span data-ttu-id="cacce-195">Caller_s</span><span class="sxs-lookup"><span data-stu-id="cacce-195">Caller_s</span></span> |<span data-ttu-id="cacce-196">Den som initierade åtgärden.</span><span class="sxs-lookup"><span data-stu-id="cacce-196">Who initiated the operation.</span></span>  <span data-ttu-id="cacce-197">Möjliga värden är antingen en e-postadress eller ett system för schemalagda jobb.</span><span class="sxs-lookup"><span data-stu-id="cacce-197">Possible values are either an email address or system for scheduled jobs.</span></span> |
| <span data-ttu-id="cacce-198">StreamType_s</span><span class="sxs-lookup"><span data-stu-id="cacce-198">StreamType_s</span></span> |<span data-ttu-id="cacce-199">Typ av jobbström.</span><span class="sxs-lookup"><span data-stu-id="cacce-199">The type of job stream.</span></span> <span data-ttu-id="cacce-200">Möjliga värden:</span><span class="sxs-lookup"><span data-stu-id="cacce-200">Possible values are:</span></span><br><span data-ttu-id="cacce-201">- Status</span><span class="sxs-lookup"><span data-stu-id="cacce-201">-Progress</span></span><br><span data-ttu-id="cacce-202">- Utdata</span><span class="sxs-lookup"><span data-stu-id="cacce-202">- Output</span></span><br><span data-ttu-id="cacce-203">- Varning</span><span class="sxs-lookup"><span data-stu-id="cacce-203">- Warning</span></span><br><span data-ttu-id="cacce-204">- Fel</span><span class="sxs-lookup"><span data-stu-id="cacce-204">- Error</span></span><br><span data-ttu-id="cacce-205">- Felsökning</span><span class="sxs-lookup"><span data-stu-id="cacce-205">- Debug</span></span><br><span data-ttu-id="cacce-206">- Verbose</span><span class="sxs-lookup"><span data-stu-id="cacce-206">- Verbose</span></span> |
| <span data-ttu-id="cacce-207">Tenant_g</span><span class="sxs-lookup"><span data-stu-id="cacce-207">Tenant_g</span></span> | <span data-ttu-id="cacce-208">GUID som identifierar klienten för anroparen.</span><span class="sxs-lookup"><span data-stu-id="cacce-208">GUID that identifies the tenant for the Caller.</span></span> |
| <span data-ttu-id="cacce-209">JobId_g</span><span class="sxs-lookup"><span data-stu-id="cacce-209">JobId_g</span></span> |<span data-ttu-id="cacce-210">GUID som är Id för runbook-jobbet.</span><span class="sxs-lookup"><span data-stu-id="cacce-210">GUID that is the Id of the runbook job.</span></span> |
| <span data-ttu-id="cacce-211">ResultType</span><span class="sxs-lookup"><span data-stu-id="cacce-211">ResultType</span></span> |<span data-ttu-id="cacce-212">Status för runbookjobbet.</span><span class="sxs-lookup"><span data-stu-id="cacce-212">The status of the runbook job.</span></span>  <span data-ttu-id="cacce-213">Möjliga värden:</span><span class="sxs-lookup"><span data-stu-id="cacce-213">Possible values are:</span></span><br><span data-ttu-id="cacce-214">-Pågår</span><span class="sxs-lookup"><span data-stu-id="cacce-214">- In Progress</span></span> |
| <span data-ttu-id="cacce-215">Kategori</span><span class="sxs-lookup"><span data-stu-id="cacce-215">Category</span></span> | <span data-ttu-id="cacce-216">Klassificering av typ av data.</span><span class="sxs-lookup"><span data-stu-id="cacce-216">Classification of the type of data.</span></span>  <span data-ttu-id="cacce-217">För Automation är värdet JobStreams.</span><span class="sxs-lookup"><span data-stu-id="cacce-217">For Automation, the value is JobStreams.</span></span> |
| <span data-ttu-id="cacce-218">OperationName</span><span class="sxs-lookup"><span data-stu-id="cacce-218">OperationName</span></span> | <span data-ttu-id="cacce-219">Anger åtgärdstypen i Azure.</span><span class="sxs-lookup"><span data-stu-id="cacce-219">Specifies the type of operation performed in Azure.</span></span>  <span data-ttu-id="cacce-220">Värdet är jobb för automatisering.</span><span class="sxs-lookup"><span data-stu-id="cacce-220">For Automation, the value is Job.</span></span> |
| <span data-ttu-id="cacce-221">Resurs</span><span class="sxs-lookup"><span data-stu-id="cacce-221">Resource</span></span> | <span data-ttu-id="cacce-222">Namnet på det Automation-kontot</span><span class="sxs-lookup"><span data-stu-id="cacce-222">Name of the Automation account</span></span> |
| <span data-ttu-id="cacce-223">SourceSystem</span><span class="sxs-lookup"><span data-stu-id="cacce-223">SourceSystem</span></span> | <span data-ttu-id="cacce-224">Hur logganalys insamlade data.</span><span class="sxs-lookup"><span data-stu-id="cacce-224">How Log Analytics collected the data.</span></span> <span data-ttu-id="cacce-225">Alltid *Azure* för Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="cacce-225">Always *Azure* for Azure diagnostics.</span></span> |
| <span data-ttu-id="cacce-226">ResultDescription</span><span class="sxs-lookup"><span data-stu-id="cacce-226">ResultDescription</span></span> |<span data-ttu-id="cacce-227">Innehåller utdataströmmen från runbook.</span><span class="sxs-lookup"><span data-stu-id="cacce-227">Includes the output stream from the runbook.</span></span> |
| <span data-ttu-id="cacce-228">CorrelationId</span><span class="sxs-lookup"><span data-stu-id="cacce-228">CorrelationId</span></span> |<span data-ttu-id="cacce-229">GUID som är korrelations-Id för runbook-jobbet.</span><span class="sxs-lookup"><span data-stu-id="cacce-229">GUID that is the Correlation Id of the runbook job.</span></span> |
| <span data-ttu-id="cacce-230">Resurs-ID</span><span class="sxs-lookup"><span data-stu-id="cacce-230">ResourceId</span></span> |<span data-ttu-id="cacce-231">Anger Azure Automation-konto resurs-id för runbook.</span><span class="sxs-lookup"><span data-stu-id="cacce-231">Specifies the Azure Automation account resource id of the runbook.</span></span> |
| <span data-ttu-id="cacce-232">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="cacce-232">SubscriptionId</span></span> | <span data-ttu-id="cacce-233">Azure-prenumerationen Id (GUID) för Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="cacce-233">The Azure subscription Id (GUID) for the Automation account.</span></span> |
| <span data-ttu-id="cacce-234">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="cacce-234">ResourceGroup</span></span> | <span data-ttu-id="cacce-235">Namnet på resursgruppen för Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="cacce-235">Name of the resource group for the Automation account.</span></span> |
| <span data-ttu-id="cacce-236">ResourceProvider</span><span class="sxs-lookup"><span data-stu-id="cacce-236">ResourceProvider</span></span> | <span data-ttu-id="cacce-237">MICROSOFT. AUTOMATION</span><span class="sxs-lookup"><span data-stu-id="cacce-237">MICROSOFT.AUTOMATION</span></span> |
| <span data-ttu-id="cacce-238">ResourceType</span><span class="sxs-lookup"><span data-stu-id="cacce-238">ResourceType</span></span> | <span data-ttu-id="cacce-239">AUTOMATIONACCOUNTS</span><span class="sxs-lookup"><span data-stu-id="cacce-239">AUTOMATIONACCOUNTS</span></span> |

## <a name="viewing-automation-logs-in-log-analytics"></a><span data-ttu-id="cacce-240">Visa Automation loggar i logganalys</span><span class="sxs-lookup"><span data-stu-id="cacce-240">Viewing Automation Logs in Log Analytics</span></span>
<span data-ttu-id="cacce-241">Nu när du har startat skicka dina Automation-jobbloggar till logganalys kan vi se vad du kan göra med dessa loggar i logganalys.</span><span class="sxs-lookup"><span data-stu-id="cacce-241">Now that you have started sending your Automation job logs to Log Analytics, let’s see what you can do with these logs inside Log Analytics.</span></span>

<span data-ttu-id="cacce-242">Kör följande fråga om du vill visa loggarna:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`</span><span class="sxs-lookup"><span data-stu-id="cacce-242">To see the logs, run the following query: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`</span></span>

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a><span data-ttu-id="cacce-243">Skicka ett e-postmeddelande när en runbook-jobb misslyckas eller pausar</span><span class="sxs-lookup"><span data-stu-id="cacce-243">Send an email when a runbook job fails or suspends</span></span>
<span data-ttu-id="cacce-244">En av våra främsta kunden frågar avser möjlighet att skicka ett e-postmeddelande eller en text om något går fel med ett runbook-jobb.</span><span class="sxs-lookup"><span data-stu-id="cacce-244">One of our top customer asks is for the ability to send an email or a text when something goes wrong with a runbook job.</span></span>   

<span data-ttu-id="cacce-245">Om du vill skapa en aviseringsregel, börja med att skapa en logg Sök efter poster för runbook-jobb som ska anropa aviseringen.</span><span class="sxs-lookup"><span data-stu-id="cacce-245">To create an alert rule, you start by creating a log search for the runbook job records that should invoke the alert.</span></span>  <span data-ttu-id="cacce-246">Klicka på den **avisering** för att skapa och konfigurera varningsregeln.</span><span class="sxs-lookup"><span data-stu-id="cacce-246">Click the **Alert** button to create and configure the alert rule.</span></span>

1. <span data-ttu-id="cacce-247">Översikt över Log Analytics-sidan klickar du på **loggen Sök**.</span><span class="sxs-lookup"><span data-stu-id="cacce-247">From the Log Analytics Overview page, click **Log Search**.</span></span>
2. <span data-ttu-id="cacce-248">Skapa en logg sökfråga för aviseringen genom att skriva följande sökningen i fältet fråga: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended)` du kan också gruppera efter RunbookName med hjälp av:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended) | measure Count() by RunbookName_s`</span><span class="sxs-lookup"><span data-stu-id="cacce-248">Create a log search query for your alert by typing the following search into the query field:  `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended)`  You can also group by the RunbookName by using: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended) | measure Count() by RunbookName_s`</span></span>   

   <span data-ttu-id="cacce-249">Om du har lagt upp loggar från mer än en Automation-konto eller prenumeration på din arbetsyta kan gruppera du aviseringar av prenumeration och Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="cacce-249">If you have set up logs from more than one Automation account or subscription to your workspace, you can group your alerts by subscription and Automation account.</span></span>  <span data-ttu-id="cacce-250">Automation-kontonamnet kan härledas från fältet resurs i sökningen i JobLogs.</span><span class="sxs-lookup"><span data-stu-id="cacce-250">Automation account name can be derived from the Resource field in the search of JobLogs.</span></span>  
3. <span data-ttu-id="cacce-251">Öppna den **lägga till Varningsregeln** klickar du på **avisering** överst på sidan.</span><span class="sxs-lookup"><span data-stu-id="cacce-251">To open the **Add Alert Rule** screen, click **Alert** at the top of the page.</span></span> <span data-ttu-id="cacce-252">Mer information om alternativen för att konfigurera aviseringen finns [aviseringar i logganalys](../log-analytics/log-analytics-alerts.md#alert-rules).</span><span class="sxs-lookup"><span data-stu-id="cacce-252">For further details on the options to configure the alert, see [Alerts in Log Analytics](../log-analytics/log-analytics-alerts.md#alert-rules).</span></span>

### <a name="find-all-jobs-that-have-completed-with-errors"></a><span data-ttu-id="cacce-253">Hitta alla jobb som har slutförts med fel</span><span class="sxs-lookup"><span data-stu-id="cacce-253">Find all jobs that have completed with errors</span></span>
<span data-ttu-id="cacce-254">Du hittar förutom Varna vid fel när en runbook-jobbet har en icke-avslutande fel.</span><span class="sxs-lookup"><span data-stu-id="cacce-254">In addition to alerting on failures, you can find when a runbook job has a non-terminating error.</span></span> <span data-ttu-id="cacce-255">I dessa fall PowerShell genererar ett fel uppstod när strömmen, men inte avslutar felen orsakar inte ditt jobb att pausa eller misslyckas.</span><span class="sxs-lookup"><span data-stu-id="cacce-255">In these cases PowerShell produces an error stream, but the non-terminating errors do not cause your job to suspend or fail.</span></span>    

1. <span data-ttu-id="cacce-256">Klicka på i logganalys-arbetsytan **loggen Sök**.</span><span class="sxs-lookup"><span data-stu-id="cacce-256">In your Log Analytics workspace, click **Log Search**.</span></span>
2. <span data-ttu-id="cacce-257">Ange i fältet fråga `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams StreamType_s=Error | measure count() by JobId_g` och klicka sedan på **Sök**.</span><span class="sxs-lookup"><span data-stu-id="cacce-257">In the query field, type `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams StreamType_s=Error | measure count() by JobId_g` and then click **Search**.</span></span>

### <a name="view-job-streams-for-a-job"></a><span data-ttu-id="cacce-258">Visa jobb dataströmmar för ett jobb</span><span class="sxs-lookup"><span data-stu-id="cacce-258">View job streams for a job</span></span>
<span data-ttu-id="cacce-259">När du felsöker ett jobb, kan du även vill söka i dataströmmar för jobbet.</span><span class="sxs-lookup"><span data-stu-id="cacce-259">When you are debugging a job, you may also want to look into the job streams.</span></span>  <span data-ttu-id="cacce-260">Följande fråga visar alla dataströmmar för ett enda projekt med GUID 2ebd22ea e05e-4eb9 - 9d 76-d73cbd4356e0:</span><span class="sxs-lookup"><span data-stu-id="cacce-260">The following query shows all the streams for a single job with GUID 2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0:</span></span>   

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams JobId_g="2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort TimeGenerated | select ResultDescription`

### <a name="view-historical-job-status"></a><span data-ttu-id="cacce-261">Visa historiska jobbstatus</span><span class="sxs-lookup"><span data-stu-id="cacce-261">View historical job status</span></span>
<span data-ttu-id="cacce-262">Slutligen kan du visualisera dina jobbhistorik över tid.</span><span class="sxs-lookup"><span data-stu-id="cacce-262">Finally, you may want to visualize your job history over time.</span></span>  <span data-ttu-id="cacce-263">Du kan använda den här frågan för att söka efter status för jobb med tiden.</span><span class="sxs-lookup"><span data-stu-id="cacce-263">You can use this query to search for the status of your jobs over time.</span></span>

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs NOT(ResultType="started") | measure Count() by ResultType interval 1hour`  
<br> <span data-ttu-id="cacce-264">![OMS historiska jobbet Status diagram](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)</span><span class="sxs-lookup"><span data-stu-id="cacce-264">![OMS Historical Job Status Chart](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)</span></span><br>

## <a name="summary"></a><span data-ttu-id="cacce-265">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="cacce-265">Summary</span></span>
<span data-ttu-id="cacce-266">Du kan få bättre inblick i ditt Automation-jobb efter status genom att skicka ditt Automation jobbdata status och dataströmmen till logganalys:</span><span class="sxs-lookup"><span data-stu-id="cacce-266">By sending your Automation job status and stream data to Log Analytics, you can get better insight into the status of your Automation jobs by:</span></span>
+ <span data-ttu-id="cacce-267">Ställa in aviseringar för att meddela dig när det uppstår problem</span><span class="sxs-lookup"><span data-stu-id="cacce-267">Setting up alerts to notify you when there is an issue</span></span>
+ <span data-ttu-id="cacce-268">Använda anpassade vyer och sökfrågor visualisera dina runbook-resultat, relaterade runbook jobbstatus och andra viktiga indikatorer eller mått.</span><span class="sxs-lookup"><span data-stu-id="cacce-268">Using custom views and search queries to visualize your runbook results, runbook job status, and other related key indicators or metrics.</span></span>  

<span data-ttu-id="cacce-269">Log Analytics ger bättre operativa synlighet till Automation-jobb och kan hjälpa adress incidenter snabbare.</span><span class="sxs-lookup"><span data-stu-id="cacce-269">Log Analytics provides greater operational visibility to your Automation jobs and can help address incidents quicker.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="cacce-270">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cacce-270">Next steps</span></span>
* <span data-ttu-id="cacce-271">Läs mer om hur du konstruerar olika sökfrågor och granskar jobbloggarna i Automation med Log Analytics i [Logga sökningar i Log Analytics](../log-analytics/log-analytics-log-searches.md)</span><span class="sxs-lookup"><span data-stu-id="cacce-271">To learn more about how to construct different search queries and review the Automation job logs with Log Analytics, see [Log searches in Log Analytics](../log-analytics/log-analytics-log-searches.md)</span></span>
* <span data-ttu-id="cacce-272">Information om hur du skapar och hämta utdata och felmeddelanden från runbooks finns [utdata och meddelanden](automation-runbook-output-and-messages.md)</span><span class="sxs-lookup"><span data-stu-id="cacce-272">To understand how to create and retrieve output and error messages from runbooks, see [Runbook output and messages](automation-runbook-output-and-messages.md)</span></span>
* <span data-ttu-id="cacce-273">Läs mer om att köra runbook, hur du övervakar runbook-jobb och andra tekniska detaljer i [Spåra runbook-jobb](automation-runbook-execution.md)</span><span class="sxs-lookup"><span data-stu-id="cacce-273">To learn more about runbook execution, how to monitor runbook jobs, and other technical details, see [Track a runbook job](automation-runbook-execution.md)</span></span>
* <span data-ttu-id="cacce-274">Läs mer om OMS Log Analytics och datakällsamling i [Samla in data om Azure-lagring i Log Analytics-översikten](../log-analytics/log-analytics-azure-storage.md)</span><span class="sxs-lookup"><span data-stu-id="cacce-274">To learn more about OMS Log Analytics and data collection sources, see [Collecting Azure storage data in Log Analytics overview](../log-analytics/log-analytics-azure-storage.md)</span></span>
