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
# <a name="starting-a-runbook-in-azure-automation"></a>Starta en runbook i Azure Automation
hello i den följande tabellen hjälper dig att avgöra hello metoden toostart en runbook i Azure Automation som är mest lämpliga tooyour visst scenario. Den här artikeln innehåller information om hur du startar en runbook med hello Azure-portalen och med Windows PowerShell. Information på hello andra metoder finns i övrig dokumentation som du kan komma åt från hello länkarna nedan.

| **METODEN** | **EGENSKAPER** |
| --- | --- |
| [Azure Portal](#starting-a-runbook-with-the-azure-portal) |<li>Enklaste metoden med interaktivt användargränssnitt.<br> <li>Formuläret tooprovide enkel parametervärden.<br> <li>Spåra jobbets status.<br> <li>Autentisera med Azure inloggning åtkomst. |
| [Windows PowerShell](https://msdn.microsoft.com/library/dn690259.aspx) |<li>Anropa från kommandoraden med Windows PowerShell-cmdlets.<br> <li>Kan ingå i automatisk lösning med flera steg.<br> <li>Begäran har autentiserats med certifikat eller OAuth användarens huvudnamn / service principal.<br> <li>Ange enkla och komplexa parametervärden.<br> <li>Spåra jobbets status.<br> <li>Klienten måste toosupport PowerShell-cmdlets. |
| [Azure Automation-API](https://msdn.microsoft.com/library/azure/mt662285.aspx) |<li>Mest flexibla metoden, men även de flesta komplexa.<br> <li>Anropa från valfri egen kod som kan göra HTTP-begäranden.<br> <li>Begäran som autentiserats med certifikat eller Oauth användarens huvudnamn / service principal.<br> <li>Ange enkla och komplexa parametervärden.<br> <li>Spåra jobbets status. |
| [Webhooks](automation-webhooks.md) |<li>Starta runbook från http-begäran.<br> <li>Autentisera med säkerhets-token i URL: en.<br> <li>Klienten kan inte åsidosätta parametervärden som anges när skapa webhooken. Runbook kan definiera en enda parameter som fylls i med information om hello HTTP-begäran.<br> <li>Ingen möjlighet tootrack jobbets status via Webhooksadressen. |
| [Svara tooAzure avisering](../log-analytics/log-analytics-alerts.md) |<li>Starta en runbook i svaret tooAzure avisering.<br> <li>Konfigurera webhook för runbook och länka tooalert.<br> <li>Autentisera med säkerhets-token i URL: en. |
| [Schema](automation-schedules.md) |<li>Runbook starta automatiskt på varje timme, dag, vecka eller månad schema.<br> <li>Ändra schema via Azure-portalen, PowerShell-cmdlets eller Azure API.<br> <li>Ange parametern värden toobe används med schemat. |
| [Från en annan Runbook](automation-child-runbooks.md) |<li>Använd en runbook som en aktivitet i en annan runbook.<br> <li>Användbart för funktioner som används av flera runbooks.<br> <li>Ange parametern värden toochild runbook och använda utdata i överordnad runbook. |

hello följande bild illustrerar detaljerade steg för steg i hello livscykel för en runbook. Den innehåller olika sätt som en runbook startas i Azure Automation komponenter som krävs för Hybrid Runbook Worker tooexecute Azure Automation-runbooks och samverkan mellan olika komponenter. toolearn om Automation-runbooks som körs i ditt datacenter finns för[hybrid runbook Worker](automation-hybrid-runbook-worker.md)

![Runbook-arkitektur](media/automation-starting-runbook/runbooks-architecture.png)

## <a name="starting-a-runbook-with-hello-azure-portal"></a>Starta en runbook med hello Azure-portalen
1. Välj i hello Azure-portalen, **Automation** och klicka sedan på hello namnet på ett automation-konto.
2. På navmenyn hello väljer **Runbooks**.
3. På hello **Runbooks** bladet Välj en runbook och klicka sedan på **starta**.
4. Om hello runbook har parametrar, kommer du att ange tooprovide värden med en textruta för varje parameter. Se [Runbookparametrar](#Runbook-parameters) nedan för mer information om parametrar.
5. På hello **jobbet** bladet kan du se statusen för hello av hello runbook-jobbet.

## <a name="starting-a-runbook-with-windows-powershell"></a>Starta en runbook med Windows PowerShell
Du kan använda hello [Start AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) toostart en runbook med Windows PowerShell. hello startar följande exempelkod en runbook med namnet Test-Runbook.

```
Start-AzureRmAutomationRunbook -AutomationAccountName "MyAutomationAccount" -Name "Test-Runbook" -ResourceGroupName "ResourceGroup01"
```

Start-AzureRmAutomationRunbook returnerar ett jobb objekt som du kan använda tootrack dess status när hello runbook startas. Du kan sedan använda detta jobbobjekt med [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) toodetermine hello status för hello jobb och [Get-AzureRmAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) tooget utdata. hello följande exempelkod startar en runbook med namnet Test-Runbook, väntar tills den har slutförts och visar sedan dess utdata.

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

Om hello runbook kräver parametrar, så du måste ange dem som en [hashtable](http://technet.microsoft.com/library/hh847780.aspx) där hello nyckeln för hello hashtable matchar hello parameternamnet och hello-värdet är hello parametervärdet. hello följande exempel visas hur toostart en runbook med två strängparametrar-heter FirstName och LastName, ett heltal som heter RepeatCount och en boolesk parameter som heter Show. Mer information om parametrar finns [Runbookparametrar](#Runbook-parameters) nedan.

```
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -ResourceGroupName "ResourceGroup01" –Parameters $params
```

## <a name="runbook-parameters"></a>Runbook-parametrar
När du startar en runbook från hello Azure Portal eller Windows PowerShell skickas instruktionen hello via hello Azure Automation-webbtjänsten. Den här tjänsten stöder inte parametrar med komplexa datatyper. Om du behöver tooprovide ett värde för en komplex parameter, så måste du anropa den infogad från en annan runbook enligt beskrivningen i [underordnade runbooks i Azure Automation](automation-child-runbooks.md).

hello Azure Automation-webbtjänsten tillhandahåller särskilda funktioner för parametrar med vissa datatyper som beskrivs i följande avsnitt hello.

### <a name="named-values"></a>Namngivna värden
Om hello-parametern är datatypen [objekt] så att du kan använda följande JSON-format toosend den en lista över namngivna värden hello: *{Name1: 'Värde1', Name2: 'Value2', Name3: 'Value3'}*. Dessa värden måste vara enkla typer. Hej runbook får hello-parametern som ett [PSCustomObject](https://msdn.microsoft.com/library/system.management.automation.pscustomobject%28v=vs.85%29.aspx) med egenskaper som motsvarar tooeach namngivet värde.

Överväg följande test-runbook som accepterar en parameter med namnet användaren hello.

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

hello kan följande text användas för hello user-parameter.

```
{FirstName:'Joe',LastName:'Smith',RepeatCount:'2',Show:'True'}
```

Detta resulterar i följande utdata hello.

```
Joe
Smith
Joe
Smith
```

### <a name="arrays"></a>matriser
Om hello-parametern är en matris som t.ex. [array] eller [string []], kan du använda hello följande JSON-format toosend den en lista med värden: *[Value1, Value2, Value3]*. Dessa värden måste vara enkla typer.

Överväg följande test-runbook som accepterar en parameter med namnet hello *användaren*.

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

hello kan följande text användas för hello user-parameter.

```
["Joe","Smith",2,true]
```

Detta resulterar i följande utdata hello.

```
Joe
Smith
Joe
Smith
```

### <a name="credentials"></a>Autentiseringsuppgifter
Om hello-parametern är datatypen **PSCredential**, kan du ange hello namnet på en Azure Automation [autentiseringsuppgiftstillgång](automation-credentials.md). Hej runbook hämtar hello autentiseringsuppgiften hello som du anger.

Överväg följande test-runbook som accepterar en parameter med namnet autentiseringsuppgifter hello.

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][PSCredential]$credential
   )
   $credential.UserName
}
```

hello följande text kan användas för hello user-parameter under förutsättning att det är en autentiseringstillgång kallas *mina autentiseringsuppgifter*.

```
My Credential
```

Under förutsättning att hello användarnamnet i hello autentiseringsuppgifter är *jsmith*, detta resulterar i följande utdata hello.

```
jsmith
```

## <a name="next-steps"></a>Nästa steg
* hello runbook arkitektur i aktuella artikeln ger en översikt över runbooks hantera resurserna i Azure och lokala med hello Hybrid Runbook Worker.  toolearn om Automation-runbooks som körs i ditt datacenter finns för[Runbook Worker-hybrider](automation-hybrid-runbook-worker.md).
* toolearn mer om hello skapar modulbaserade runbooks toobe som används av andra runbooks för specifika eller vanliga funktioner finns för[underordnade Runbooks](automation-child-runbooks.md).

