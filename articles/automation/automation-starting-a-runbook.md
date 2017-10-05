---
title: Starta en runbook i Azure Automation | Microsoft Docs
description: "Sammanfattar olika metoder som kan användas för att starta en runbook i Azure Automation och innehåller information om hur du använder både Azure-portalen och Windows PowerShell."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 6ee756b4-9200-4eb2-9bda-ec156853803b
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/07/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 844831b63d5263987ed05370125fbe9f01913ab9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="starting-a-runbook-in-azure-automation"></a><span data-ttu-id="a318c-103">Starta en runbook i Azure Automation</span><span class="sxs-lookup"><span data-stu-id="a318c-103">Starting a runbook in Azure Automation</span></span>
<span data-ttu-id="a318c-104">Tabellen nedan hjälper dig att avgöra vilken metod för att starta en runbook i Azure Automation som är mest lämpliga för ditt specifika scenario.</span><span class="sxs-lookup"><span data-stu-id="a318c-104">The following table will help you determine the method to start a runbook in Azure Automation that is most suitable to your particular scenario.</span></span> <span data-ttu-id="a318c-105">Den här artikeln innehåller information om hur du startar en runbook med Azure-portalen och med Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a318c-105">This article includes details on starting a runbook with the Azure portal and with Windows PowerShell.</span></span> <span data-ttu-id="a318c-106">Information om de andra metoderna finns i övrig dokumentation som du kan komma åt från länkarna nedan.</span><span class="sxs-lookup"><span data-stu-id="a318c-106">Details on the other methods are provided in other documentation that you can access from the links below.</span></span>

| <span data-ttu-id="a318c-107">**METODEN**</span><span class="sxs-lookup"><span data-stu-id="a318c-107">**METHOD**</span></span> | <span data-ttu-id="a318c-108">**EGENSKAPER**</span><span class="sxs-lookup"><span data-stu-id="a318c-108">**CHARACTERISTICS**</span></span> |
| --- | --- |
| [<span data-ttu-id="a318c-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a318c-109">Azure Portal</span></span>](#starting-a-runbook-with-the-azure-portal) |<li><span data-ttu-id="a318c-110">Enklaste metoden med interaktivt användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="a318c-110">Simplest method with interactive user interface.</span></span><br> <li><span data-ttu-id="a318c-111">Formulär med enkla parametervärden.</span><span class="sxs-lookup"><span data-stu-id="a318c-111">Form to provide simple parameter values.</span></span><br> <li><span data-ttu-id="a318c-112">Spåra jobbets status.</span><span class="sxs-lookup"><span data-stu-id="a318c-112">Easily track job state.</span></span><br> <li><span data-ttu-id="a318c-113">Autentisera med Azure inloggning åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a318c-113">Access authenticated with Azure logon.</span></span> |
| [<span data-ttu-id="a318c-114">Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="a318c-114">Windows PowerShell</span></span>](https://msdn.microsoft.com/library/dn690259.aspx) |<li><span data-ttu-id="a318c-115">Anropa från kommandoraden med Windows PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="a318c-115">Call from command line with Windows PowerShell cmdlets.</span></span><br> <li><span data-ttu-id="a318c-116">Kan ingå i automatisk lösning med flera steg.</span><span class="sxs-lookup"><span data-stu-id="a318c-116">Can be included in automated solution with multiple steps.</span></span><br> <li><span data-ttu-id="a318c-117">Begäran har autentiserats med certifikat eller OAuth användarens huvudnamn / service principal.</span><span class="sxs-lookup"><span data-stu-id="a318c-117">Request is authenticated with certificate or OAuth user principal / service principal.</span></span><br> <li><span data-ttu-id="a318c-118">Ange enkla och komplexa parametervärden.</span><span class="sxs-lookup"><span data-stu-id="a318c-118">Provide simple and complex parameter values.</span></span><br> <li><span data-ttu-id="a318c-119">Spåra jobbets status.</span><span class="sxs-lookup"><span data-stu-id="a318c-119">Track job state.</span></span><br> <li><span data-ttu-id="a318c-120">Klienten behöver stöd för PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="a318c-120">Client required to support PowerShell cmdlets.</span></span> |
| [<span data-ttu-id="a318c-121">Azure Automation-API</span><span class="sxs-lookup"><span data-stu-id="a318c-121">Azure Automation API</span></span>](https://msdn.microsoft.com/library/azure/mt662285.aspx) |<li><span data-ttu-id="a318c-122">Mest flexibla metoden, men även de flesta komplexa.</span><span class="sxs-lookup"><span data-stu-id="a318c-122">Most flexible method but also most complex.</span></span><br> <li><span data-ttu-id="a318c-123">Anropa från valfri egen kod som kan göra HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="a318c-123">Call from any custom code that can make HTTP requests.</span></span><br> <li><span data-ttu-id="a318c-124">Begäran som autentiserats med certifikat eller Oauth användarens huvudnamn / service principal.</span><span class="sxs-lookup"><span data-stu-id="a318c-124">Request authenticated with certificate, or Oauth user principal / service principal.</span></span><br> <li><span data-ttu-id="a318c-125">Ange enkla och komplexa parametervärden.</span><span class="sxs-lookup"><span data-stu-id="a318c-125">Provide simple and complex parameter values.</span></span><br> <li><span data-ttu-id="a318c-126">Spåra jobbets status.</span><span class="sxs-lookup"><span data-stu-id="a318c-126">Track job state.</span></span> |
| [<span data-ttu-id="a318c-127">Webhooks</span><span class="sxs-lookup"><span data-stu-id="a318c-127">Webhooks</span></span>](automation-webhooks.md) |<li><span data-ttu-id="a318c-128">Starta runbook från http-begäran.</span><span class="sxs-lookup"><span data-stu-id="a318c-128">Start runbook from single HTTP request.</span></span><br> <li><span data-ttu-id="a318c-129">Autentisera med säkerhets-token i URL: en.</span><span class="sxs-lookup"><span data-stu-id="a318c-129">Authenticated with security token in URL.</span></span><br> <li><span data-ttu-id="a318c-130">Klienten kan inte åsidosätta parametervärden som anges när skapa webhooken.</span><span class="sxs-lookup"><span data-stu-id="a318c-130">Client cannot override parameter values specified when webhook created.</span></span> <span data-ttu-id="a318c-131">Runbook kan definiera en enda parameter som fylls i med information för HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="a318c-131">Runbook can define single parameter that is populated with the HTTP request details.</span></span><br> <li><span data-ttu-id="a318c-132">Ingen möjlighet att spåra jobbstatus via Webhooksadressen.</span><span class="sxs-lookup"><span data-stu-id="a318c-132">No ability to track job state through webhook URL.</span></span> |
| [<span data-ttu-id="a318c-133">Svara på Azure avisering</span><span class="sxs-lookup"><span data-stu-id="a318c-133">Respond to Azure Alert</span></span>](../log-analytics/log-analytics-alerts.md) |<li><span data-ttu-id="a318c-134">Starta en runbook som svar på Azure avisering.</span><span class="sxs-lookup"><span data-stu-id="a318c-134">Start a runbook in response to Azure alert.</span></span><br> <li><span data-ttu-id="a318c-135">Konfigurera webhook för runbook och länk till varning.</span><span class="sxs-lookup"><span data-stu-id="a318c-135">Configure webhook for runbook and link to alert.</span></span><br> <li><span data-ttu-id="a318c-136">Autentisera med säkerhets-token i URL: en.</span><span class="sxs-lookup"><span data-stu-id="a318c-136">Authenticated with security token in URL.</span></span> |
| [<span data-ttu-id="a318c-137">Schema</span><span class="sxs-lookup"><span data-stu-id="a318c-137">Schedule</span></span>](automation-schedules.md) |<li><span data-ttu-id="a318c-138">Runbook starta automatiskt på varje timme, dag, vecka eller månad schema.</span><span class="sxs-lookup"><span data-stu-id="a318c-138">Automatically start runbook on hourly, daily, weekly, or monthly schedule.</span></span><br> <li><span data-ttu-id="a318c-139">Ändra schema via Azure-portalen, PowerShell-cmdlets eller Azure API.</span><span class="sxs-lookup"><span data-stu-id="a318c-139">Manipulate schedule through Azure portal, PowerShell cmdlets, or Azure API.</span></span><br> <li><span data-ttu-id="a318c-140">Ange parametervärden som ska användas med schemat.</span><span class="sxs-lookup"><span data-stu-id="a318c-140">Provide parameter values to be used with schedule.</span></span> |
| [<span data-ttu-id="a318c-141">Från en annan Runbook</span><span class="sxs-lookup"><span data-stu-id="a318c-141">From Another Runbook</span></span>](automation-child-runbooks.md) |<li><span data-ttu-id="a318c-142">Använd en runbook som en aktivitet i en annan runbook.</span><span class="sxs-lookup"><span data-stu-id="a318c-142">Use a runbook as an activity in another runbook.</span></span><br> <li><span data-ttu-id="a318c-143">Användbart för funktioner som används av flera runbooks.</span><span class="sxs-lookup"><span data-stu-id="a318c-143">Useful for functionality used by multiple runbooks.</span></span><br> <li><span data-ttu-id="a318c-144">Ange parametervärden att underordnad runbook och använda utdata i överordnad runbook.</span><span class="sxs-lookup"><span data-stu-id="a318c-144">Provide parameter values to child runbook and use output in parent runbook.</span></span> |

<span data-ttu-id="a318c-145">Följande bild illustrerar detaljerade steg för steg i livscykeln för en runbook.</span><span class="sxs-lookup"><span data-stu-id="a318c-145">The following image illustrates detailed step-by-step process in the life cycle of a runbook.</span></span> <span data-ttu-id="a318c-146">Den innehåller olika sätt som en runbook startas i Azure Automation komponenter krävs för Hybrid Runbook Worker att köra Azure Automation-runbooks och samverkan mellan olika komponenter.</span><span class="sxs-lookup"><span data-stu-id="a318c-146">It includes different ways a runbook is started in Azure Automation, components required for Hybrid Runbook Worker to execute Azure Automation runbooks and interactions between different components.</span></span> <span data-ttu-id="a318c-147">Mer information om att köra Automation-runbooks i ditt datacenter, referera till [hybrid runbook Worker](automation-hybrid-runbook-worker.md)</span><span class="sxs-lookup"><span data-stu-id="a318c-147">To learn about executing Automation runbooks in your datacenter, refer to [hybrid runbook workers](automation-hybrid-runbook-worker.md)</span></span>

![Runbook-arkitektur](media/automation-starting-runbook/runbooks-architecture.png)

## <a name="starting-a-runbook-with-the-azure-portal"></a><span data-ttu-id="a318c-149">Starta en runbook med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a318c-149">Starting a runbook with the Azure portal</span></span>
1. <span data-ttu-id="a318c-150">Välj i Azure-portalen **Automation** och klicka sedan på namnet på ett automation-konto.</span><span class="sxs-lookup"><span data-stu-id="a318c-150">In the Azure portal, select **Automation** and then then click the name of an automation account.</span></span>
2. <span data-ttu-id="a318c-151">På navmenyn väljer **Runbooks**.</span><span class="sxs-lookup"><span data-stu-id="a318c-151">On the Hub menu, select **Runbooks**.</span></span>
3. <span data-ttu-id="a318c-152">På den **Runbooks** bladet Välj en runbook och klicka sedan på **starta**.</span><span class="sxs-lookup"><span data-stu-id="a318c-152">On the **Runbooks** blade, select a runbook, and then click **Start**.</span></span>
4. <span data-ttu-id="a318c-153">Om runbooken har parametrar uppmanas du att ange värden med en textruta för varje parameter.</span><span class="sxs-lookup"><span data-stu-id="a318c-153">If the runbook has parameters, you will be prompted to provide values with a text box for each parameter.</span></span> <span data-ttu-id="a318c-154">Se [Runbookparametrar](#Runbook-parameters) nedan för mer information om parametrar.</span><span class="sxs-lookup"><span data-stu-id="a318c-154">See [Runbook Parameters](#Runbook-parameters) below for further details on parameters.</span></span>
5. <span data-ttu-id="a318c-155">På den **jobbet** bladet kan du visa statusen för runbook-jobbet.</span><span class="sxs-lookup"><span data-stu-id="a318c-155">On the **Job** blade, you can view the status of the runbook job.</span></span>

## <a name="starting-a-runbook-with-windows-powershell"></a><span data-ttu-id="a318c-156">Starta en runbook med Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="a318c-156">Starting a runbook with Windows PowerShell</span></span>
<span data-ttu-id="a318c-157">Du kan använda den [Start AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) att starta en runbook med Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a318c-157">You can use the [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) to start a runbook with Windows PowerShell.</span></span> <span data-ttu-id="a318c-158">Följande exempelkod startar en runbook med namnet Test-Runbook.</span><span class="sxs-lookup"><span data-stu-id="a318c-158">The following sample code starts a runbook called Test-Runbook.</span></span>

```
Start-AzureRmAutomationRunbook -AutomationAccountName "MyAutomationAccount" -Name "Test-Runbook" -ResourceGroupName "ResourceGroup01"
```

<span data-ttu-id="a318c-159">Start-AzureRmAutomationRunbook returnerar ett jobbobjekt som du kan använda för att spåra dess status när runbook startas.</span><span class="sxs-lookup"><span data-stu-id="a318c-159">Start-AzureRmAutomationRunbook returns a job object that you can use to track its status once the runbook is started.</span></span> <span data-ttu-id="a318c-160">Du kan sedan använda detta jobbobjekt med [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) att avgöra status för jobbet och [Get-AzureRmAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) att få dess utdata.</span><span class="sxs-lookup"><span data-stu-id="a318c-160">You can then use this job object with [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) to determine the status of the job and [Get-AzureRmAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) to get its output.</span></span> <span data-ttu-id="a318c-161">Följande exempelkod startar en runbook med namnet Test-Runbook, väntar tills den har slutförts och visar sedan dess utdata.</span><span class="sxs-lookup"><span data-stu-id="a318c-161">The following sample code starts a runbook called Test-Runbook, waits until it has completed, and then displays its output.</span></span>

```
$runbookName = "Test-Runbook"
$ResourceGroup = "ResourceGroup01"
$AutomationAcct = "MyAutomationAccount"

$job = Start-AzureRmAutomationRunbook –AutomationAccountName $AutomationAcct -Name $runbookName -ResourceGroupName $ResourceGroup

$doLoop = $true
While ($doLoop) {
   $job = Get-AzureRmAutomationJob –AutomationAccountName $AutomationAcct -Id $job.JobId -ResourceGroupName $ResourceGroup
   $status = $job.Status
   $doLoop = (($status -ne "Completed") -and ($status -ne "Failed") -and ($status -ne "Suspended") -and ($status -ne "Stopped"))
}

Get-AzureRmAutomationJobOutput –AutomationAccountName $AutomationAcct -Id $job.JobId -ResourceGroupName $ResourceGroup –Stream Output
```

<span data-ttu-id="a318c-162">Om runbook kräver parametrar, så du måste ange dem som en [hashtable](http://technet.microsoft.com/library/hh847780.aspx) där nyckeln för hash-tabellen matchar parameternamnet och värdet är parametervärdet.</span><span class="sxs-lookup"><span data-stu-id="a318c-162">If the runbook requires parameters, then you must provide them as a [hashtable](http://technet.microsoft.com/library/hh847780.aspx) where the key of the hashtable matches the parameter name and the value is the parameter value.</span></span> <span data-ttu-id="a318c-163">I följande exempel visar hur du startar en runbook med två strängparametrar som heter FirstName och LastName, ett heltal som heter RepeatCount och en boolesk parameter som heter Show.</span><span class="sxs-lookup"><span data-stu-id="a318c-163">The following example shows how to start a runbook with two string parameters named FirstName and LastName, an integer named RepeatCount, and a boolean parameter named Show.</span></span> <span data-ttu-id="a318c-164">Mer information om parametrar finns [Runbookparametrar](#Runbook-parameters) nedan.</span><span class="sxs-lookup"><span data-stu-id="a318c-164">For additional information on parameters, see [Runbook Parameters](#Runbook-parameters) below.</span></span>

```
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -ResourceGroupName "ResourceGroup01" –Parameters $params
```

## <a name="runbook-parameters"></a><span data-ttu-id="a318c-165">Runbook-parametrar</span><span class="sxs-lookup"><span data-stu-id="a318c-165">Runbook parameters</span></span>
<span data-ttu-id="a318c-166">När du startar en runbook från Azure Portal eller Windows PowerShell skickas instruktionen via Azure Automation-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="a318c-166">When you start a runbook from the Azure Portal or Windows PowerShell, the instruction is sent through the Azure Automation web service.</span></span> <span data-ttu-id="a318c-167">Den här tjänsten stöder inte parametrar med komplexa datatyper.</span><span class="sxs-lookup"><span data-stu-id="a318c-167">This service does not support parameters with complex data types.</span></span> <span data-ttu-id="a318c-168">Om du måste ange ett värde för en komplex parameter, så måste du anropa den infogad från en annan runbook enligt beskrivningen i [underordnade runbooks i Azure Automation](automation-child-runbooks.md).</span><span class="sxs-lookup"><span data-stu-id="a318c-168">If you need to provide a value for a complex parameter, then you must call it inline from another runbook as described in [Child runbooks in Azure Automation](automation-child-runbooks.md).</span></span>

<span data-ttu-id="a318c-169">Azure Automation-webbtjänsten tillhandahåller särskilda funktioner för parametrar med vissa datatyper som beskrivs i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a318c-169">The Azure Automation web service will provide special functionality for parameters using certain data types as described in the following sections.</span></span>

### <a name="named-values"></a><span data-ttu-id="a318c-170">Namngivna värden</span><span class="sxs-lookup"><span data-stu-id="a318c-170">Named values</span></span>
<span data-ttu-id="a318c-171">Om parametern är datatypen [objekt] så att du kan använda följande JSON-format för att skicka en lista över namngivna värden: *{Name1: 'Värde1', Name2: 'Value2', Name3: 'Value3'}*.</span><span class="sxs-lookup"><span data-stu-id="a318c-171">If the parameter is data type [object], then you can use the following JSON format to send it a list of named values: *{Name1:'Value1', Name2:'Value2', Name3:'Value3'}*.</span></span> <span data-ttu-id="a318c-172">Dessa värden måste vara enkla typer.</span><span class="sxs-lookup"><span data-stu-id="a318c-172">These values must be simple types.</span></span> <span data-ttu-id="a318c-173">Runbooken får parametern som ett [PSCustomObject](https://msdn.microsoft.com/library/system.management.automation.pscustomobject%28v=vs.85%29.aspx) med egenskaper som motsvarar varje namngivet värde.</span><span class="sxs-lookup"><span data-stu-id="a318c-173">The runbook will receive the parameter as a [PSCustomObject](https://msdn.microsoft.com/library/system.management.automation.pscustomobject%28v=vs.85%29.aspx) with properties that correspond to each named value.</span></span>

<span data-ttu-id="a318c-174">Överväg följande test-runbook som accepterar en parameter med namnet användare.</span><span class="sxs-lookup"><span data-stu-id="a318c-174">Consider the following test runbook that accepts a parameter called user.</span></span>

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][object]$user
   )
    $userObject = $user | ConvertFrom-JSON
    if ($userObject.Show) {
        foreach ($i in 1..$userObject.RepeatCount) {
            $userObject.FirstName
            $userObject.LastName
        }
    }
}
```

<span data-ttu-id="a318c-175">Följande text kan användas för user-parameter.</span><span class="sxs-lookup"><span data-stu-id="a318c-175">The following text could be used for the user parameter.</span></span>

```
{FirstName:'Joe',LastName:'Smith',RepeatCount:'2',Show:'True'}
```

<span data-ttu-id="a318c-176">Detta resulterar i följande utdata.</span><span class="sxs-lookup"><span data-stu-id="a318c-176">This results in the following output.</span></span>

```
Joe
Smith
Joe
Smith
```

### <a name="arrays"></a><span data-ttu-id="a318c-177">matriser</span><span class="sxs-lookup"><span data-stu-id="a318c-177">Arrays</span></span>
<span data-ttu-id="a318c-178">Om parametern är en matris som t.ex. [array] eller [string []], kan du använda följande JSON-format för att skicka en lista med värden: *[Value1, Value2, Value3]*.</span><span class="sxs-lookup"><span data-stu-id="a318c-178">If the parameter is an array such as [array] or [string[]], then you can use the following JSON format to send it a list of values: *[Value1,Value2,Value3]*.</span></span> <span data-ttu-id="a318c-179">Dessa värden måste vara enkla typer.</span><span class="sxs-lookup"><span data-stu-id="a318c-179">These values must be simple types.</span></span>

<span data-ttu-id="a318c-180">Överväg följande test-runbook som accepterar en parameter med namnet *användaren*.</span><span class="sxs-lookup"><span data-stu-id="a318c-180">Consider the following test runbook that accepts a parameter called *user*.</span></span>

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][array]$user
   )
    if ($user[3]) {
        foreach ($i in 1..$user[2]) {
            $ user[0]
            $ user[1]
        }
    }
}
```

<span data-ttu-id="a318c-181">Följande text kan användas för user-parameter.</span><span class="sxs-lookup"><span data-stu-id="a318c-181">The following text could be used for the user parameter.</span></span>

```
["Joe","Smith",2,true]
```

<span data-ttu-id="a318c-182">Detta resulterar i följande utdata.</span><span class="sxs-lookup"><span data-stu-id="a318c-182">This results in the following output.</span></span>

```
Joe
Smith
Joe
Smith
```

### <a name="credentials"></a><span data-ttu-id="a318c-183">Autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="a318c-183">Credentials</span></span>
<span data-ttu-id="a318c-184">Om parametern är datatypen **PSCredential**, kan du ange namnet på en Azure Automation [autentiseringsuppgiftstillgång](automation-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="a318c-184">If the parameter is data type **PSCredential**, then you can provide the name of an Azure Automation [credential asset](automation-credentials.md).</span></span> <span data-ttu-id="a318c-185">Runbook hämtar autentiseringsuppgift med det namn som du anger.</span><span class="sxs-lookup"><span data-stu-id="a318c-185">The runbook will retrieve the credential with the name that you specify.</span></span>

<span data-ttu-id="a318c-186">Överväg följande test-runbook som accepterar en parameter med namnet autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a318c-186">Consider the following test runbook that accepts a parameter called credential.</span></span>

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][PSCredential]$credential
   )
   $credential.UserName
}
```

<span data-ttu-id="a318c-187">Följande text kan användas för den användaren parametern under förutsättning att det fanns en autentiseringstillgång kallas *mina autentiseringsuppgifter*.</span><span class="sxs-lookup"><span data-stu-id="a318c-187">The following text could be used for the user parameter assuming that there was a credential asset called *My Credential*.</span></span>

```
My Credential
```

<span data-ttu-id="a318c-188">Om användarnamnet i autentiseringsuppgiften var *jsmith*, detta resulterar i följande utdata.</span><span class="sxs-lookup"><span data-stu-id="a318c-188">Assuming the username in the credential was *jsmith*, this results in the following output.</span></span>

```
jsmith
```

## <a name="next-steps"></a><span data-ttu-id="a318c-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a318c-189">Next steps</span></span>
* <span data-ttu-id="a318c-190">Runbook-arkitekturen i aktuella artikeln ger en översikt över runbooks hantera resurserna i Azure och lokala med Hybrid Runbook Worker.</span><span class="sxs-lookup"><span data-stu-id="a318c-190">The runbook architecture in current article provides a high-level overview of runbooks managing resources in Azure and on-premises with the Hybrid Runbook Worker.</span></span>  <span data-ttu-id="a318c-191">Mer information om att köra Automation-runbooks i ditt datacenter, referera till [Runbook Worker-hybrider](automation-hybrid-runbook-worker.md).</span><span class="sxs-lookup"><span data-stu-id="a318c-191">To learn about executing Automation runbooks in your datacenter, refer to [Hybrid Runbook Workers](automation-hybrid-runbook-worker.md).</span></span>
* <span data-ttu-id="a318c-192">Mer information om att skapa modulbaserade runbooks som ska användas av andra runbooks för specifika eller vanliga funktioner avser [underordnade Runbooks](automation-child-runbooks.md).</span><span class="sxs-lookup"><span data-stu-id="a318c-192">To learn more about the creating modular runbooks to be used by other runbooks for specific or common functions, refer to [Child Runbooks](automation-child-runbooks.md).</span></span>

