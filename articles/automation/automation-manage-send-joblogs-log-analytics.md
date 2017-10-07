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
# <a name="forward-job-status-and-job-streams-from-automation-toolog-analytics-oms"></a>Vidarebefordra jobbstatus och jobbet strömmar från Automation tooLog Analytics (OMS)
Automatisering kan skicka runbook-jobbet status och jobbstatus dataströmmar tooyour Microsoft Operations Management Suite (OMS) logganalys-arbetsytan.  Loggar för jobbet och dataströmmar för jobbet är synliga i hello Azure-portalen eller PowerShell för enskilda jobb och detta kan du tooperform enkel undersökningar. Med Log Analytics kan du nu:

* Skaffa dig insikter om dina Automation-jobb
* Utlösare som en e-post eller en avisering baserat på din runbook jobbets status (till exempel misslyckades eller pausas)
* Skriva avancerade frågor över jobb-strömmar
* Korrelera jobb över Automation-konton
* Visualisera dina jobbhistorik över tid     

## <a name="prerequisites-and-deployment-considerations"></a>Krav och överväganden vid distribution
skicka din Automation toostart loggar tooLog Analytics, måste du:

1. Hej November 2016 eller senare versionen av [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) (v2.3.0).
2. Logganalys-arbetsytan. Mer information finns i [Kom igång med logganalys](../log-analytics/log-analytics-get-started.md). 
3. hello ResourceId för ditt Azure Automation-konto

toofind hello ResourceId för Azure Automation-konto och logganalys-arbetsytan, kör följande PowerShell hello:

```powershell
# Find hello ResourceId for hello Automation Account
Find-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"

# Find hello ResourceId for hello Log Analytics workspace
Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

Om du har flera Automation-konton eller arbetsytor, i hello utdata från hello föregående kommandon, hitta hello *namn* du behöver tooconfigure och kopiera hello värde för *ResourceId*.

Om du behöver toofind hello *namn* för Automation-konto i hello Azure portal väljer ditt Automation-konto från hello **Automation-konto** och välj **alla inställningar**.  Från hello **alla inställningar** bladet under **kontoinställningar** Välj **egenskaper**.  I hello **egenskaper** bladet kan du Observera värdena.<br> ![Egenskaper för Automation-konto](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).

## <a name="set-up-integration-with-log-analytics"></a>Ställa in integration med logganalys
1. På datorn, startar **Windows PowerShell** från hello **starta** skärmen.  
2. Kopiera och klistra in följande PowerShell hello och redigera hello värde för hello `$workspaceId` och `$automationAccountId`.  För hello `-Environment` parameter, giltiga värden är *AzureCloud* eller *AzureUSGovernment* beroende på hello molnmiljö du arbetar med.     

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

När du har kört skriptet visas poster i logganalys inom 10 minuter efter nya JobLogs eller JobStreams skrivs.

toosee hello loggar kör hello följande sökfrågor logganalys loggen:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`

### <a name="verify-configuration"></a>Verifiera konfigurationen
tooconfirm som skickar ditt Automation-konto loggar tooyour logganalys-arbetsytan, kontrollera att diagnostik är korrekt inställt på hello Automation-konto med hjälp av följande PowerShell hello:

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

Se till att i hello utdata:
+ Under *loggar*, hello värde för *aktiverad* är *SANT*
+ Hej värdet för *WorkspaceId* anges toohello ResourceId för logganalys-arbetsytan


## <a name="log-analytics-records"></a>Log Analytics-poster
Diagnostik från Azure Automation skapar två typer av poster i logganalys och märks **typ = AzureDiagnostics**.

### <a name="job-logs"></a>Jobbloggar
| Egenskap | Beskrivning |
| --- | --- |
| TimeGenerated |Datum och tid när hello runbook-jobbet körs. |
| RunbookName_s |hello namnet på hello runbook. |
| Caller_s |Vem som initierat hello igen.  Möjliga värden är antingen en e-postadress eller ett system för schemalagda jobb. |
| Tenant_g | GUID som identifierar hello-klient för hello anroparen. |
| JobId_g |GUID som är hello-Id för hello runbook-jobbet. |
| ResultType |hello status för hello runbook-jobbet.  Möjliga värden:<br>- Startad<br>- Stoppad<br>-Pausad<br>- Misslyckades<br>-Slutförd |
| Kategori | Klassificering av hello typ av data.  Hello-värdet är JobLogs för automatisering. |
| OperationName | Anger hello åtgärd utförs i Azure.  Hello-värdet är jobb för automatisering. |
| Resurs | Namnet på hello Automation-konto |
| SourceSystem | Hur logganalys samlas in hello data. Alltid *Azure* för Azure-diagnostik. |
| ResultDescription |Beskriver hello runbook jobbstatus resultat.  Möjliga värden:<br>-Jobbet har startats<br>-Jobbet misslyckades<br>-Jobbet slutfördes |
| CorrelationId |GUID som är hello Korrelations-Id för hello runbook-jobbet. |
| Resurs-ID |Anger hello Azure Automation-konto resurs-id för hello runbook. |
| SubscriptionId | hello Azure-prenumeration Id (GUID) för hello Automation-konto. |
| ResourceGroup | Namnet på resursgruppen hello för hello Automation-konto. |
| ResourceProvider | MICROSOFT. AUTOMATION |
| ResourceType | AUTOMATIONACCOUNTS |


### <a name="job-streams"></a>Dataströmmar för jobbet
| Egenskap | Beskrivning |
| --- | --- |
| TimeGenerated |Datum och tid när hello runbook-jobbet körs. |
| RunbookName_s |hello namnet på hello runbook. |
| Caller_s |Vem som initierat hello igen.  Möjliga värden är antingen en e-postadress eller ett system för schemalagda jobb. |
| StreamType_s |hello typ av jobbström. Möjliga värden:<br>- Status<br>- Utdata<br>- Varning<br>- Fel<br>- Felsökning<br>- Verbose |
| Tenant_g | GUID som identifierar hello-klient för hello anroparen. |
| JobId_g |GUID som är hello-Id för hello runbook-jobbet. |
| ResultType |hello status för hello runbook-jobbet.  Möjliga värden:<br>-Pågår |
| Kategori | Klassificering av hello typ av data.  Hello-värdet är JobStreams för automatisering. |
| OperationName | Anger hello åtgärd utförs i Azure.  Hello-värdet är jobb för automatisering. |
| Resurs | Namnet på hello Automation-konto |
| SourceSystem | Hur logganalys samlas in hello data. Alltid *Azure* för Azure-diagnostik. |
| ResultDescription |Innehåller hello utdataström från hello runbook. |
| CorrelationId |GUID som är hello Korrelations-Id för hello runbook-jobbet. |
| Resurs-ID |Anger hello Azure Automation-konto resurs-id för hello runbook. |
| SubscriptionId | hello Azure-prenumeration Id (GUID) för hello Automation-konto. |
| ResourceGroup | Namnet på resursgruppen hello för hello Automation-konto. |
| ResourceProvider | MICROSOFT. AUTOMATION |
| ResourceType | AUTOMATIONACCOUNTS |

## <a name="viewing-automation-logs-in-log-analytics"></a>Visa Automation loggar i logganalys
Nu när du har startat skicka din Automation jobb loggar tooLog Analytics kan vi se vad du kan göra med dessa loggar i logganalys.

toosee hello loggar, kör följande fråga hello:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a>Skicka ett e-postmeddelande när en runbook-jobb misslyckas eller pausar
En av våra främsta kunden frågar avser hello möjlighet toosend ett e-postmeddelande eller en text om något går fel med ett runbook-jobb.   

toocreate en avisering regeln du börja med att skapa en logg-sökning för hello runbook jobbposter som ska anropa hello avisering.  Klicka på hello **avisering** knappen toocreate och konfigurera hello varningsregel.

1. Klicka på sidan Översikt över Log Analytics hello **loggen Sök**.
2. Skapa en logg sökfråga för aviseringen genom att skriva hello efter sökning i hello frågefält: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended)` du kan också gruppera efter hello RunbookName med hjälp av:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended) | measure Count() by RunbookName_s`   

   Om du har lagt upp loggar från mer än en Automation-konto eller prenumeration tooyour arbetsytan kan gruppera du aviseringarna genom prenumerationen och Automation-konto.  Automation-kontonamnet kan härledas från hello fält i hello sökning av JobLogs.  
3. tooopen hello **lägga till Varningsregeln** klickar du på **avisering** hello överst på hello sidan. Mer information om hello alternativ tooconfigure hello avisering finns [aviseringar i logganalys](../log-analytics/log-analytics-alerts.md#alert-rules).

### <a name="find-all-jobs-that-have-completed-with-errors"></a>Hitta alla jobb som har slutförts med fel
Dessutom tooalerting på fel du hittar när en runbook-jobbet har en icke-avslutande fel. I dessa fall PowerShell genererar ett fel uppstod när strömmen, men hello-avslutande fel inte orsaka jobbet-toosuspend eller misslyckas.    

1. Klicka på i logganalys-arbetsytan **loggen Sök**.
2. Skriv i hello frågan `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams StreamType_s=Error | measure count() by JobId_g` och klicka sedan på **Sök**.

### <a name="view-job-streams-for-a-job"></a>Visa jobb dataströmmar för ett jobb
Du kanske också vill toolook till hello jobbet dataströmmar när du felsöker ett jobb.  hello visar följande fråga alla hello dataströmmar för ett enda projekt med GUID 2ebd22ea e05e-4eb9 - 9d 76-d73cbd4356e0:   

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams JobId_g="2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort TimeGenerated | select ResultDescription`

### <a name="view-historical-job-status"></a>Visa historiska jobbstatus
Slutligen kan du toovisualize din jobbhistorik över tid.  Du kan använda den här frågan toosearch hello status för jobb med tiden.

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs NOT(ResultType="started") | measure Count() by ResultType interval 1hour`  
<br> ![OMS historiska jobbet Status diagram](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)<br>

## <a name="summary"></a>Sammanfattning
Du kan få bättre inblick i hello status för ditt Automation-jobb efter genom att skicka ditt Automation jobbet status och dataströmmen data tooLog Analytics:
+ Du konfigurerar aviseringar toonotify du när det uppstår problem
+ Använda anpassade vyer och Sök frågor toovisualize relaterade din runbook-resultat, runbook jobbstatus och andra viktiga indikatorer eller mått.  

Log Analytics ger bättre operativa synlighet tooyour Automation-jobb och kan hjälpa adress incidenter snabbare.  

## <a name="next-steps"></a>Nästa steg
* toolearn mer information om hur tooconstruct olika sökfrågor och granska hello Automation jobb loggar med Log Analytics finns [logga sökningar i logganalys](../log-analytics/log-analytics-log-searches.md)
* hur toocreate och hämta utdata och felmeddelanden från runbooks, se toounderstand [Runbook utdata och meddelanden](automation-runbook-output-and-messages.md)
* Mer om runbook-körningen hur toomonitor runbook-jobb och annan teknisk information finns i toolearn [spåra ett runbook-jobb](automation-runbook-execution.md)
* toolearn mer om logganalys OMS och datakällor för samlingen, se [insamling av Azure storage-data i logganalys-översikt](../log-analytics/log-analytics-azure-storage.md)
