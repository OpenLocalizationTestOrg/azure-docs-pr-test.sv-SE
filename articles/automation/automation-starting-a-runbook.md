---
title: aaaStarting en runbook i Azure Automation | Microsoft Docs
description: "Sammanfattar hello olika metoder som kan använda toostart en runbook i Azure Automation och ger information om hur du använder både hello Azure-portalen och Windows PowerShell."
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
ms.openlocfilehash: e44bce5b56b8e803f9247fbb4f3d4db7ab35c913
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="starting-a-runbook-in-azure-automation"></a><span data-ttu-id="657ac-103">Starta en runbook i Azure Automation</span><span class="sxs-lookup"><span data-stu-id="657ac-103">Starting a runbook in Azure Automation</span></span>
<span data-ttu-id="657ac-104">hello i den följande tabellen hjälper dig att avgöra hello metoden toostart en runbook i Azure Automation som är mest lämpliga tooyour visst scenario.</span><span class="sxs-lookup"><span data-stu-id="657ac-104">hello following table will help you determine hello method toostart a runbook in Azure Automation that is most suitable tooyour particular scenario.</span></span> <span data-ttu-id="657ac-105">Den här artikeln innehåller information om hur du startar en runbook med hello Azure-portalen och med Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="657ac-105">This article includes details on starting a runbook with hello Azure portal and with Windows PowerShell.</span></span> <span data-ttu-id="657ac-106">Information på hello andra metoder finns i övrig dokumentation som du kan komma åt från hello länkarna nedan.</span><span class="sxs-lookup"><span data-stu-id="657ac-106">Details on hello other methods are provided in other documentation that you can access from hello links below.</span></span>

| <span data-ttu-id="657ac-107">**METODEN**</span><span class="sxs-lookup"><span data-stu-id="657ac-107">**METHOD**</span></span> | <span data-ttu-id="657ac-108">**EGENSKAPER**</span><span class="sxs-lookup"><span data-stu-id="657ac-108">**CHARACTERISTICS**</span></span> |
| --- | --- |
| [<span data-ttu-id="657ac-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="657ac-109">Azure Portal</span></span>](#starting-a-runbook-with-the-azure-portal) |<li><span data-ttu-id="657ac-110">Enklaste metoden med interaktivt användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="657ac-110">Simplest method with interactive user interface.</span></span><br> <li><span data-ttu-id="657ac-111">Formuläret tooprovide enkel parametervärden.</span><span class="sxs-lookup"><span data-stu-id="657ac-111">Form tooprovide simple parameter values.</span></span><br> <li><span data-ttu-id="657ac-112">Spåra jobbets status.</span><span class="sxs-lookup"><span data-stu-id="657ac-112">Easily track job state.</span></span><br> <li><span data-ttu-id="657ac-113">Autentisera med Azure inloggning åtkomst.</span><span class="sxs-lookup"><span data-stu-id="657ac-113">Access authenticated with Azure logon.</span></span> |
| [<span data-ttu-id="657ac-114">Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="657ac-114">Windows PowerShell</span></span>](https://msdn.microsoft.com/library/dn690259.aspx) |<li><span data-ttu-id="657ac-115">Anropa från kommandoraden med Windows PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="657ac-115">Call from command line with Windows PowerShell cmdlets.</span></span><br> <li><span data-ttu-id="657ac-116">Kan ingå i automatisk lösning med flera steg.</span><span class="sxs-lookup"><span data-stu-id="657ac-116">Can be included in automated solution with multiple steps.</span></span><br> <li><span data-ttu-id="657ac-117">Begäran har autentiserats med certifikat eller OAuth användarens huvudnamn / service principal.</span><span class="sxs-lookup"><span data-stu-id="657ac-117">Request is authenticated with certificate or OAuth user principal / service principal.</span></span><br> <li><span data-ttu-id="657ac-118">Ange enkla och komplexa parametervärden.</span><span class="sxs-lookup"><span data-stu-id="657ac-118">Provide simple and complex parameter values.</span></span><br> <li><span data-ttu-id="657ac-119">Spåra jobbets status.</span><span class="sxs-lookup"><span data-stu-id="657ac-119">Track job state.</span></span><br> <li><span data-ttu-id="657ac-120">Klienten måste toosupport PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="657ac-120">Client required toosupport PowerShell cmdlets.</span></span> |
| [<span data-ttu-id="657ac-121">Azure Automation-API</span><span class="sxs-lookup"><span data-stu-id="657ac-121">Azure Automation API</span></span>](https://msdn.microsoft.com/library/azure/mt662285.aspx) |<li><span data-ttu-id="657ac-122">Mest flexibla metoden, men även de flesta komplexa.</span><span class="sxs-lookup"><span data-stu-id="657ac-122">Most flexible method but also most complex.</span></span><br> <li><span data-ttu-id="657ac-123">Anropa från valfri egen kod som kan göra HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="657ac-123">Call from any custom code that can make HTTP requests.</span></span><br> <li><span data-ttu-id="657ac-124">Begäran som autentiserats med certifikat eller Oauth användarens huvudnamn / service principal.</span><span class="sxs-lookup"><span data-stu-id="657ac-124">Request authenticated with certificate, or Oauth user principal / service principal.</span></span><br> <li><span data-ttu-id="657ac-125">Ange enkla och komplexa parametervärden.</span><span class="sxs-lookup"><span data-stu-id="657ac-125">Provide simple and complex parameter values.</span></span><br> <li><span data-ttu-id="657ac-126">Spåra jobbets status.</span><span class="sxs-lookup"><span data-stu-id="657ac-126">Track job state.</span></span> |
| [<span data-ttu-id="657ac-127">Webhooks</span><span class="sxs-lookup"><span data-stu-id="657ac-127">Webhooks</span></span>](automation-webhooks.md) |<li><span data-ttu-id="657ac-128">Starta runbook från http-begäran.</span><span class="sxs-lookup"><span data-stu-id="657ac-128">Start runbook from single HTTP request.</span></span><br> <li><span data-ttu-id="657ac-129">Autentisera med säkerhets-token i URL: en.</span><span class="sxs-lookup"><span data-stu-id="657ac-129">Authenticated with security token in URL.</span></span><br> <li><span data-ttu-id="657ac-130">Klienten kan inte åsidosätta parametervärden som anges när skapa webhooken.</span><span class="sxs-lookup"><span data-stu-id="657ac-130">Client cannot override parameter values specified when webhook created.</span></span> <span data-ttu-id="657ac-131">Runbook kan definiera en enda parameter som fylls i med information om hello HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="657ac-131">Runbook can define single parameter that is populated with hello HTTP request details.</span></span><br> <li><span data-ttu-id="657ac-132">Ingen möjlighet tootrack jobbets status via Webhooksadressen.</span><span class="sxs-lookup"><span data-stu-id="657ac-132">No ability tootrack job state through webhook URL.</span></span> |
| [<span data-ttu-id="657ac-133">Svara tooAzure avisering</span><span class="sxs-lookup"><span data-stu-id="657ac-133">Respond tooAzure Alert</span></span>](../log-analytics/log-analytics-alerts.md) |<li><span data-ttu-id="657ac-134">Starta en runbook i svaret tooAzure avisering.</span><span class="sxs-lookup"><span data-stu-id="657ac-134">Start a runbook in response tooAzure alert.</span></span><br> <li><span data-ttu-id="657ac-135">Konfigurera webhook för runbook och länka tooalert.</span><span class="sxs-lookup"><span data-stu-id="657ac-135">Configure webhook for runbook and link tooalert.</span></span><br> <li><span data-ttu-id="657ac-136">Autentisera med säkerhets-token i URL: en.</span><span class="sxs-lookup"><span data-stu-id="657ac-136">Authenticated with security token in URL.</span></span> |
| [<span data-ttu-id="657ac-137">Schema</span><span class="sxs-lookup"><span data-stu-id="657ac-137">Schedule</span></span>](automation-schedules.md) |<li><span data-ttu-id="657ac-138">Runbook starta automatiskt på varje timme, dag, vecka eller månad schema.</span><span class="sxs-lookup"><span data-stu-id="657ac-138">Automatically start runbook on hourly, daily, weekly, or monthly schedule.</span></span><br> <li><span data-ttu-id="657ac-139">Ändra schema via Azure-portalen, PowerShell-cmdlets eller Azure API.</span><span class="sxs-lookup"><span data-stu-id="657ac-139">Manipulate schedule through Azure portal, PowerShell cmdlets, or Azure API.</span></span><br> <li><span data-ttu-id="657ac-140">Ange parametern värden toobe används med schemat.</span><span class="sxs-lookup"><span data-stu-id="657ac-140">Provide parameter values toobe used with schedule.</span></span> |
| [<span data-ttu-id="657ac-141">Från en annan Runbook</span><span class="sxs-lookup"><span data-stu-id="657ac-141">From Another Runbook</span></span>](automation-child-runbooks.md) |<li><span data-ttu-id="657ac-142">Använd en runbook som en aktivitet i en annan runbook.</span><span class="sxs-lookup"><span data-stu-id="657ac-142">Use a runbook as an activity in another runbook.</span></span><br> <li><span data-ttu-id="657ac-143">Användbart för funktioner som används av flera runbooks.</span><span class="sxs-lookup"><span data-stu-id="657ac-143">Useful for functionality used by multiple runbooks.</span></span><br> <li><span data-ttu-id="657ac-144">Ange parametern värden toochild runbook och använda utdata i överordnad runbook.</span><span class="sxs-lookup"><span data-stu-id="657ac-144">Provide parameter values toochild runbook and use output in parent runbook.</span></span> |

<span data-ttu-id="657ac-145">hello följande bild illustrerar detaljerade steg för steg i hello livscykel för en runbook.</span><span class="sxs-lookup"><span data-stu-id="657ac-145">hello following image illustrates detailed step-by-step process in hello life cycle of a runbook.</span></span> <span data-ttu-id="657ac-146">Den innehåller olika sätt som en runbook startas i Azure Automation komponenter som krävs för Hybrid Runbook Worker tooexecute Azure Automation-runbooks och samverkan mellan olika komponenter.</span><span class="sxs-lookup"><span data-stu-id="657ac-146">It includes different ways a runbook is started in Azure Automation, components required for Hybrid Runbook Worker tooexecute Azure Automation runbooks and interactions between different components.</span></span> <span data-ttu-id="657ac-147">toolearn om Automation-runbooks som körs i ditt datacenter finns för[hybrid runbook Worker](automation-hybrid-runbook-worker.md)</span><span class="sxs-lookup"><span data-stu-id="657ac-147">toolearn about executing Automation runbooks in your datacenter, refer too[hybrid runbook workers](automation-hybrid-runbook-worker.md)</span></span>

![Runbook-arkitektur](media/automation-starting-runbook/runbooks-architecture.png)

## <a name="starting-a-runbook-with-hello-azure-portal"></a><span data-ttu-id="657ac-149">Starta en runbook med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="657ac-149">Starting a runbook with hello Azure portal</span></span>
1. <span data-ttu-id="657ac-150">Välj i hello Azure-portalen, **Automation** och klicka sedan på hello namnet på ett automation-konto.</span><span class="sxs-lookup"><span data-stu-id="657ac-150">In hello Azure portal, select **Automation** and then then click hello name of an automation account.</span></span>
2. <span data-ttu-id="657ac-151">På navmenyn hello väljer **Runbooks**.</span><span class="sxs-lookup"><span data-stu-id="657ac-151">On hello Hub menu, select **Runbooks**.</span></span>
3. <span data-ttu-id="657ac-152">På hello **Runbooks** bladet Välj en runbook och klicka sedan på **starta**.</span><span class="sxs-lookup"><span data-stu-id="657ac-152">On hello **Runbooks** blade, select a runbook, and then click **Start**.</span></span>
4. <span data-ttu-id="657ac-153">Om hello runbook har parametrar, kommer du att ange tooprovide värden med en textruta för varje parameter.</span><span class="sxs-lookup"><span data-stu-id="657ac-153">If hello runbook has parameters, you will be prompted tooprovide values with a text box for each parameter.</span></span> <span data-ttu-id="657ac-154">Se [Runbookparametrar](#Runbook-parameters) nedan för mer information om parametrar.</span><span class="sxs-lookup"><span data-stu-id="657ac-154">See [Runbook Parameters](#Runbook-parameters) below for further details on parameters.</span></span>
5. <span data-ttu-id="657ac-155">På hello **jobbet** bladet kan du se statusen för hello av hello runbook-jobbet.</span><span class="sxs-lookup"><span data-stu-id="657ac-155">On hello **Job** blade, you can view hello status of hello runbook job.</span></span>

## <a name="starting-a-runbook-with-windows-powershell"></a><span data-ttu-id="657ac-156">Starta en runbook med Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="657ac-156">Starting a runbook with Windows PowerShell</span></span>
<span data-ttu-id="657ac-157">Du kan använda hello [Start AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) toostart en runbook med Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="657ac-157">You can use hello [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) toostart a runbook with Windows PowerShell.</span></span> <span data-ttu-id="657ac-158">hello startar följande exempelkod en runbook med namnet Test-Runbook.</span><span class="sxs-lookup"><span data-stu-id="657ac-158">hello following sample code starts a runbook called Test-Runbook.</span></span>

```
Start-AzureRmAutomationRunbook -AutomationAccountName "MyAutomationAccount" -Name "Test-Runbook" -ResourceGroupName "ResourceGroup01"
```

<span data-ttu-id="657ac-159">Start-AzureRmAutomationRunbook returnerar ett jobb objekt som du kan använda tootrack dess status när hello runbook startas.</span><span class="sxs-lookup"><span data-stu-id="657ac-159">Start-AzureRmAutomationRunbook returns a job object that you can use tootrack its status once hello runbook is started.</span></span> <span data-ttu-id="657ac-160">Du kan sedan använda detta jobbobjekt med [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) toodetermine hello status för hello jobb och [Get-AzureRmAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) tooget utdata.</span><span class="sxs-lookup"><span data-stu-id="657ac-160">You can then use this job object with [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) toodetermine hello status of hello job and [Get-AzureRmAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) tooget its output.</span></span> <span data-ttu-id="657ac-161">hello följande exempelkod startar en runbook med namnet Test-Runbook, väntar tills den har slutförts och visar sedan dess utdata.</span><span class="sxs-lookup"><span data-stu-id="657ac-161">hello following sample code starts a runbook called Test-Runbook, waits until it has completed, and then displays its output.</span></span>

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

<span data-ttu-id="657ac-162">Om hello runbook kräver parametrar, så du måste ange dem som en [hashtable](http://technet.microsoft.com/library/hh847780.aspx) där hello nyckeln för hello hashtable matchar hello parameternamnet och hello-värdet är hello parametervärdet.</span><span class="sxs-lookup"><span data-stu-id="657ac-162">If hello runbook requires parameters, then you must provide them as a [hashtable](http://technet.microsoft.com/library/hh847780.aspx) where hello key of hello hashtable matches hello parameter name and hello value is hello parameter value.</span></span> <span data-ttu-id="657ac-163">hello följande exempel visas hur toostart en runbook med två strängparametrar-heter FirstName och LastName, ett heltal som heter RepeatCount och en boolesk parameter som heter Show.</span><span class="sxs-lookup"><span data-stu-id="657ac-163">hello following example shows how toostart a runbook with two string parameters named FirstName and LastName, an integer named RepeatCount, and a boolean parameter named Show.</span></span> <span data-ttu-id="657ac-164">Mer information om parametrar finns [Runbookparametrar](#Runbook-parameters) nedan.</span><span class="sxs-lookup"><span data-stu-id="657ac-164">For additional information on parameters, see [Runbook Parameters](#Runbook-parameters) below.</span></span>

```
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -ResourceGroupName "ResourceGroup01" –Parameters $params
```

## <a name="runbook-parameters"></a><span data-ttu-id="657ac-165">Runbook-parametrar</span><span class="sxs-lookup"><span data-stu-id="657ac-165">Runbook parameters</span></span>
<span data-ttu-id="657ac-166">När du startar en runbook från hello Azure Portal eller Windows PowerShell skickas instruktionen hello via hello Azure Automation-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="657ac-166">When you start a runbook from hello Azure Portal or Windows PowerShell, hello instruction is sent through hello Azure Automation web service.</span></span> <span data-ttu-id="657ac-167">Den här tjänsten stöder inte parametrar med komplexa datatyper.</span><span class="sxs-lookup"><span data-stu-id="657ac-167">This service does not support parameters with complex data types.</span></span> <span data-ttu-id="657ac-168">Om du behöver tooprovide ett värde för en komplex parameter, så måste du anropa den infogad från en annan runbook enligt beskrivningen i [underordnade runbooks i Azure Automation](automation-child-runbooks.md).</span><span class="sxs-lookup"><span data-stu-id="657ac-168">If you need tooprovide a value for a complex parameter, then you must call it inline from another runbook as described in [Child runbooks in Azure Automation](automation-child-runbooks.md).</span></span>

<span data-ttu-id="657ac-169">hello Azure Automation-webbtjänsten tillhandahåller särskilda funktioner för parametrar med vissa datatyper som beskrivs i följande avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="657ac-169">hello Azure Automation web service will provide special functionality for parameters using certain data types as described in hello following sections.</span></span>

### <a name="named-values"></a><span data-ttu-id="657ac-170">Namngivna värden</span><span class="sxs-lookup"><span data-stu-id="657ac-170">Named values</span></span>
<span data-ttu-id="657ac-171">Om hello-parametern är datatypen [objekt] så att du kan använda följande JSON-format toosend den en lista över namngivna värden hello: *{Name1: 'Värde1', Name2: 'Value2', Name3: 'Value3'}*.</span><span class="sxs-lookup"><span data-stu-id="657ac-171">If hello parameter is data type [object], then you can use hello following JSON format toosend it a list of named values: *{Name1:'Value1', Name2:'Value2', Name3:'Value3'}*.</span></span> <span data-ttu-id="657ac-172">Dessa värden måste vara enkla typer.</span><span class="sxs-lookup"><span data-stu-id="657ac-172">These values must be simple types.</span></span> <span data-ttu-id="657ac-173">Hej runbook får hello-parametern som ett [PSCustomObject](https://msdn.microsoft.com/library/system.management.automation.pscustomobject%28v=vs.85%29.aspx) med egenskaper som motsvarar tooeach namngivet värde.</span><span class="sxs-lookup"><span data-stu-id="657ac-173">hello runbook will receive hello parameter as a [PSCustomObject](https://msdn.microsoft.com/library/system.management.automation.pscustomobject%28v=vs.85%29.aspx) with properties that correspond tooeach named value.</span></span>

<span data-ttu-id="657ac-174">Överväg följande test-runbook som accepterar en parameter med namnet användaren hello.</span><span class="sxs-lookup"><span data-stu-id="657ac-174">Consider hello following test runbook that accepts a parameter called user.</span></span>

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

<span data-ttu-id="657ac-175">hello kan följande text användas för hello user-parameter.</span><span class="sxs-lookup"><span data-stu-id="657ac-175">hello following text could be used for hello user parameter.</span></span>

```
{FirstName:'Joe',LastName:'Smith',RepeatCount:'2',Show:'True'}
```

<span data-ttu-id="657ac-176">Detta resulterar i följande utdata hello.</span><span class="sxs-lookup"><span data-stu-id="657ac-176">This results in hello following output.</span></span>

```
Joe
Smith
Joe
Smith
```

### <a name="arrays"></a><span data-ttu-id="657ac-177">matriser</span><span class="sxs-lookup"><span data-stu-id="657ac-177">Arrays</span></span>
<span data-ttu-id="657ac-178">Om hello-parametern är en matris som t.ex. [array] eller [string []], kan du använda hello följande JSON-format toosend den en lista med värden: *[Value1, Value2, Value3]*.</span><span class="sxs-lookup"><span data-stu-id="657ac-178">If hello parameter is an array such as [array] or [string[]], then you can use hello following JSON format toosend it a list of values: *[Value1,Value2,Value3]*.</span></span> <span data-ttu-id="657ac-179">Dessa värden måste vara enkla typer.</span><span class="sxs-lookup"><span data-stu-id="657ac-179">These values must be simple types.</span></span>

<span data-ttu-id="657ac-180">Överväg följande test-runbook som accepterar en parameter med namnet hello *användaren*.</span><span class="sxs-lookup"><span data-stu-id="657ac-180">Consider hello following test runbook that accepts a parameter called *user*.</span></span>

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

<span data-ttu-id="657ac-181">hello kan följande text användas för hello user-parameter.</span><span class="sxs-lookup"><span data-stu-id="657ac-181">hello following text could be used for hello user parameter.</span></span>

```
["Joe","Smith",2,true]
```

<span data-ttu-id="657ac-182">Detta resulterar i följande utdata hello.</span><span class="sxs-lookup"><span data-stu-id="657ac-182">This results in hello following output.</span></span>

```
Joe
Smith
Joe
Smith
```

### <a name="credentials"></a><span data-ttu-id="657ac-183">Autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="657ac-183">Credentials</span></span>
<span data-ttu-id="657ac-184">Om hello-parametern är datatypen **PSCredential**, kan du ange hello namnet på en Azure Automation [autentiseringsuppgiftstillgång](automation-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="657ac-184">If hello parameter is data type **PSCredential**, then you can provide hello name of an Azure Automation [credential asset](automation-credentials.md).</span></span> <span data-ttu-id="657ac-185">Hej runbook hämtar hello autentiseringsuppgiften hello som du anger.</span><span class="sxs-lookup"><span data-stu-id="657ac-185">hello runbook will retrieve hello credential with hello name that you specify.</span></span>

<span data-ttu-id="657ac-186">Överväg följande test-runbook som accepterar en parameter med namnet autentiseringsuppgifter hello.</span><span class="sxs-lookup"><span data-stu-id="657ac-186">Consider hello following test runbook that accepts a parameter called credential.</span></span>

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][PSCredential]$credential
   )
   $credential.UserName
}
```

<span data-ttu-id="657ac-187">hello följande text kan användas för hello user-parameter under förutsättning att det är en autentiseringstillgång kallas *mina autentiseringsuppgifter*.</span><span class="sxs-lookup"><span data-stu-id="657ac-187">hello following text could be used for hello user parameter assuming that there was a credential asset called *My Credential*.</span></span>

```
My Credential
```

<span data-ttu-id="657ac-188">Under förutsättning att hello användarnamnet i hello autentiseringsuppgifter är *jsmith*, detta resulterar i följande utdata hello.</span><span class="sxs-lookup"><span data-stu-id="657ac-188">Assuming hello username in hello credential was *jsmith*, this results in hello following output.</span></span>

```
jsmith
```

## <a name="next-steps"></a><span data-ttu-id="657ac-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="657ac-189">Next steps</span></span>
* <span data-ttu-id="657ac-190">hello runbook arkitektur i aktuella artikeln ger en översikt över runbooks hantera resurserna i Azure och lokala med hello Hybrid Runbook Worker.</span><span class="sxs-lookup"><span data-stu-id="657ac-190">hello runbook architecture in current article provides a high-level overview of runbooks managing resources in Azure and on-premises with hello Hybrid Runbook Worker.</span></span>  <span data-ttu-id="657ac-191">toolearn om Automation-runbooks som körs i ditt datacenter finns för[Runbook Worker-hybrider](automation-hybrid-runbook-worker.md).</span><span class="sxs-lookup"><span data-stu-id="657ac-191">toolearn about executing Automation runbooks in your datacenter, refer too[Hybrid Runbook Workers](automation-hybrid-runbook-worker.md).</span></span>
* <span data-ttu-id="657ac-192">toolearn mer om hello skapar modulbaserade runbooks toobe som används av andra runbooks för specifika eller vanliga funktioner finns för[underordnade Runbooks](automation-child-runbooks.md).</span><span class="sxs-lookup"><span data-stu-id="657ac-192">toolearn more about hello creating modular runbooks toobe used by other runbooks for specific or common functions, refer too[Child Runbooks](automation-child-runbooks.md).</span></span>

