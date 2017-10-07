---
title: aaaForward Azure Automation DSC reporting data tooOMS Log Analytics | Microsoft Docs
description: "Den här artikeln visar hur toosend önskade tillstånd Configuration (DSC) reporting data tooMicrosoft Operations Management Suite logganalys toodeliver ytterligare insikter och hantering."
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: tysonn
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: eslesar
ms.openlocfilehash: 21f78d5549d53ba3d7e237f55d9086f380cf3351
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="forward-azure-automation-dsc-reporting-data-toooms-log-analytics"></a>Vidarebefordra Azure Automation DSC reporting data tooOMS logganalys

Automatisering kan skicka DSC-noden status data tooyour Microsoft Operations Management Suite (OMS) logganalys-arbetsytan.  
Kompatibilitetsstatus är synlig i hello Azure-portalen eller med PowerShell för noderna och för enskilda DSC-resurser i nodkonfigurationer. Med Log Analytics kan du:

* Hämta information om kompatibilitet för hanterade noder och enskilda resurser
* Utlös en e-post eller en avisering baserat på status för efterlevnad
* Skriva avancerade frågor på dina hanterade noder
* Korrelera kompatibilitetsstatus över Automation-konton
* Visualisera dina nod kompatibilitetshistorik över tid

## <a name="prerequisites"></a>Krav

toostart skicka Automation DSC-rapporter tooLog Analytics, måste du:

* Hej November 2016 eller senare versionen av [Azure PowerShell](/powershell/azure/overview) (v2.3.0).
* Ett Azure Automation-konto. Mer information finns i [komma igång med Azure Automation](automation-offering-get-started.md)
* Logganalys-arbetsytan med en **Automation- och kontrollservern** tjänsterbjudande. Mer information finns i [Kom igång med logganalys](../log-analytics/log-analytics-get-started.md).
* Minst en Azure Automation DSC-nod. Mer information finns i [Onboarding datorer för hantering av Azure Automation DSC](automation-dsc-onboarding.md) 

## <a name="set-up-integration-with-log-analytics"></a>Ställa in integration med logganalys

toobegin importera data från Azure Automation DSC till logganalys fullständig hello följande steg:

1. Logga in tooyour Azure-konto i PowerShell. Se [logga in med Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/authenticate-azureps?view=azurermps-4.0.0)
1. Hämta hello _ResourceId_ för automation-konto genom att köra följande PowerShell-kommando hello: (om du har mer än en automation-konto, Välj hello _ResourceID_ för hello-konto som du vill tooconfigure).

  ```powershell
  # Find hello ResourceId for hello Automation Account
  Find-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"
  ```
1. Hämta hello _ResourceId_ för logganalys-arbetsytan genom att köra följande PowerShell-kommando hello: (om du har mer än en arbetsytan väljer du hello _ResourceID_ för hello arbetsytan som du vill tooconfigure).

  ```powershell
  # Find hello ResourceId for hello Log Analytics workspace
  Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
  ```
1. Kör hello följande PowerShell-kommandot, ersätter `<AutomationResourceId>` och `<WorkspaceResourceId>` med hello _ResourceId_ värden från varje hello föregående steg:

  ```powershell
  Set-AzureRmDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $true -Categories "DscNodeStatus"
  ```

Om du vill importera data från Azure Automation DSC till logganalys toostop kör hello följande PowerShell-kommando.

```powershell
Set-AzureRmDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $false -Categories "DscNodeStatus"
```

## <a name="view-hello-dsc-logs"></a>Visa hello DSC-loggfiler

När du har ställt in integration med Log Analytics för Automation DSC-data en **loggen Sök** knappen ska visas på hello **DSC-noder** bladet för ditt automation-konto. Klicka på hello **loggen Sök** knappen tooview hello loggar för DSC-noden data.

![Loggen sökknappen](media/automation-dsc-diagnostics/log-search-button.png)

Hej **loggen Sök** blad öppnas och du ser en **DscNodeStatusData** åtgärden för varje DSC-nod och en **DscResourceStatusData** åtgärden för varje [DSC resursen](https://msdn.microsoft.com/powershell/dsc/resources) kallas i hello nod configuration tillämpade toothat nod.

Hej **DscResourceStatusData** åtgärden innehåller information om fel för DSC resurser som inte godkänts.

Klicka på varje åtgärd i hello listan toosee hello data för den åtgärden.

Du kan också visa hello-loggar [sökning i logganalys. Se [söka efter data med hjälp av loggen sökningar](../log-analytics/log-analytics-log-searches.md).
Typen hello följande fråga toofind din DSC loggar:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category = "DscNodeStatus"`

Du kan även begränsa hello frågan genom hello åtgärdsnamn. Till exempel: ' Type = AzureDiagnostics ResourceProvider = ”MICROSOFT. Kategori för AUTOMATION ”=” DscNodeStatus ”OperationName =” DscNodeStatusData ”

### <a name="send-an-email-when-a-dsc-compliance-check-fails"></a>Skicka ett e-postmeddelande när en DSC-kompatibilitetskontrollen misslyckas

En av våra främsta kundönskemål är hello möjlighet toosend ett e-postmeddelande eller en text om något går fel med en DSC-konfiguration.   

toocreate en avisering regeln du börja med att skapa en logg sökning efter hello DSC rapporten poster som ska anropa hello avisering.  Klicka på hello **avisering** knappen toocreate och konfigurera hello varningsregel.

1. Klicka på sidan Översikt över Log Analytics hello **loggen Sök**.
1. Skapa en logg sökfråga för aviseringen genom att skriva hello efter sökning i hello frågefält:`Type=AzureDiagnostics Category=DscNodeStatus NodeName_s=DSCTEST1 OperationName=DscNodeStatusData ResultType=Failed`

  Om du har lagt upp loggar från mer än en Automation-konto eller prenumeration tooyour arbetsytan kan gruppera du aviseringarna genom prenumerationen och Automation-konto.  
  Automation-kontonamnet kan härledas från hello fält i hello sökning av DscNodeStatusData.  
1. tooopen hello **lägga till Varningsregeln** klickar du på **avisering** hello överst på hello sidan. Läs mer på hello alternativ tooconfigure hello avisering [aviseringar i logganalys](../log-analytics/log-analytics-alerts.md#alert-rules).

### <a name="find-failed-dsc-resources-across-all-nodes"></a>Sök efter misslyckade DSC-resurser på alla noder

Fördelen med att använda Log Analytics är att du kan söka efter misslyckade kontroller mellan noder.
toofind alla instanser av DSC-resurser som har misslyckats.

1. Klicka på sidan Översikt över Log Analytics hello **loggen Sök**.
1. Skapa en logg sökfråga för aviseringen genom att skriva hello efter sökning i hello frågefält:`Type=AzureDiagnostics Category=DscNodeStatus OperationName=DscResourceStatusData ResultType=Failed`

### <a name="view-historical-dsc-node-status"></a>Visa historiska DSC-nod status

Slutligen kan du toovisualize historiken DSC-noden status över tid.  
Du kan använda den här frågan toosearch hello status för din DSC-noden status över tid.

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=DscNodeStatus NOT(ResultType="started") | measure Count() by ResultType interval 1hour`  

Då visas ett diagram över hello nod status över tid.

## <a name="log-analytics-records"></a>Log Analytics-poster

Diagnostik från Azure Automation skapar två typer av poster i logganalys.

### <a name="dscnodestatusdata"></a>DscNodeStatusData

| Egenskap | Beskrivning |
| --- | --- |
| TimeGenerated |Datum och tid när hello kompatibilitetskontrollen kördes. |
| OperationName |DscNodeStatusData |
| ResultType |Om hello nod är kompatibel. |
| NodeName_s |hello namnen på hello hanterade noder. |
| NodeComplianceStatus_s |Om hello nod är kompatibel. |
| DscReportStatus |Om kompatibilitetskontrollen hello har körts. |
| ConfigurationMode | Hello konfiguration är hur tillämpade toohello nod. Möjliga värden är __”ApplyOnly”__,__”ApplyandMonitior”__, och __”ApplyandAutoCorrect”__. <ul><li>__ApplyOnly__: DSC gäller hello konfigurationen och inget ytterligare såvida inte en ny konfiguration är nedtryckt toohello målnoden eller när en ny konfiguration hämtas från en server. DSC kontrollerar inte om inte ett tidigare konfigurerade tillstånd efter första gången för en ny konfiguration. DSC försöker tooapply hello configuration tills den lyckas innan __ApplyOnly__ träder i kraft. </li><li> __ApplyAndMonitor__: Detta är hello standardvärde. hello MGM gäller alla nya konfigurationer. Efter första gången för en ny konfiguration om hello målnoden drifts från hello önskad tillstånd rapporterar DSC hello diskrepans i loggarna. DSC försöker tooapply hello configuration tills den lyckas innan __ApplyAndMonitor__ träder i kraft.</li><li>__ApplyAndAutoCorrect__: DSC gäller alla nya konfigurationer. Efter första gången för en ny konfiguration om hello målnoden drifts från hello önskad tillstånd DSC rapporterar hello diskrepans i loggarna och tillämpa hello aktuella konfiguration.</li></ul> |
| HostName_s | hello namnen på hello hanterade noder. |
| IP-adress | hello IPv4-adressen för hello hanterade noder. |
| Kategori | DscNodeStatus |
| Resurs | hello namnet på hello Azure Automation-konto. |
| Tenant_g | GUID som identifierar hello-klient för hello anroparen. |
| NodeId_g |GUID som identifierar hello hanterad nod. |
| DscReportId_g |GUID som identifierar hello rapporten. |
| LastSeenTime_t |Datum och tid när hello rapporten senast visade. |
| ReportStartTime_t |Datum och tid då rapporten hello startades. |
| ReportEndTime_t |Datum och tid när hello rapporten slutförts. |
| NumberOfResources_d |hello antal DSC resurser anropas i hello tillämpas toohello konfigurationsnod. |
| SourceSystem | Hur logganalys samlas in hello data. Alltid *Azure* för Azure-diagnostik. |
| Resurs-ID |Anger hello Azure Automation-konto. |
| ResultDescription | hello beskrivning för den här åtgärden. |
| SubscriptionId | hello Azure-prenumeration Id (GUID) för hello Automation-konto. |
| ResourceGroup | Namnet på resursgruppen hello för hello Automation-konto. |
| ResourceProvider | MICROSOFT. AUTOMATION |
| ResourceType | AUTOMATIONACCOUNTS |
| CorrelationId |GUID som är hello Korrelations-Id för hello Kompatibilitetsrapport. |

### <a name="dscresourcestatusdata"></a>DscResourceStatusData

| Egenskap | Beskrivning |
| --- | --- |
| TimeGenerated |Datum och tid när hello kompatibilitetskontrollen kördes. |
| OperationName |DscResourceStatusData|
| ResultType |Om hello resursen är kompatibel. |
| NodeName_s |hello namnen på hello hanterade noder. |
| Kategori | DscNodeStatus |
| Resurs | hello namnet på hello Azure Automation-konto. |
| Tenant_g | GUID som identifierar hello-klient för hello anroparen. |
| NodeId_g |GUID som identifierar hello hanterad nod. |
| DscReportId_g |GUID som identifierar hello rapporten. |
| DscResourceId_s |hello namnet på instansen för hello DSC-resurs. |
| DscResourceName_s |hello namn på hello DSC-resurs. |
| DscResourceStatus_s |Om hello DSC-resurs är kompatibel. |
| DscModuleName_s |hello namnet på hello PowerShell-modulen som innehåller hello DSC-resurs. |
| DscModuleVersion_s |hello version av hello PowerShell-modulen som innehåller hello DSC-resurs. |
| DscConfigurationName_s |hello namn på hello konfiguration tillämpas toohello nod. |
| ErrorCode_s | hello felkod om hello resurs misslyckades. |
| ErrorMessage_s |hello felmeddelande om hello resurs misslyckades. |
| DscResourceDuration_d |hello tid i sekunder som körde hello DSC-resurs. |
| SourceSystem | Hur logganalys samlas in hello data. Alltid *Azure* för Azure-diagnostik. |
| Resurs-ID |Anger hello Azure Automation-konto. |
| ResultDescription | hello beskrivning för den här åtgärden. |
| SubscriptionId | hello Azure-prenumeration Id (GUID) för hello Automation-konto. |
| ResourceGroup | Namnet på resursgruppen hello för hello Automation-konto. |
| ResourceProvider | MICROSOFT. AUTOMATION |
| ResourceType | AUTOMATIONACCOUNTS |
| CorrelationId |GUID som är hello Korrelations-Id för hello Kompatibilitetsrapport. |

## <a name="summary"></a>Sammanfattning

Du kan få bättre inblick i hello status för Automation DSC-noder av genom att skicka ditt Automation DSC data tooLog Analytics:

* Du konfigurerar aviseringar toonotify du när det uppstår problem
* Använda anpassade vyer och Sök frågor toovisualize relaterade din runbook-resultat, runbook jobbstatus och andra viktiga indikatorer eller mått.  

Log Analytics ger större operativa synlighet tooyour Automation DSC data och kan hjälpa adress incidenter snabbare.  

## <a name="next-steps"></a>Nästa steg

* Mer om hur tooconstruct olika sökfrågor och granska hello Automation DSC loggar med Log Analytics toolearn finns [logga sökningar i logganalys](../log-analytics/log-analytics-log-searches.md)
* toolearn mer information om hur du använder Azure Automation DSC finns [komma igång med Azure Automation DSC](automation-dsc-getting-started.md)
* toolearn mer om logganalys OMS och datakällor för samlingen, se [insamling av Azure storage-data i logganalys-översikt](../log-analytics/log-analytics-azure-storage.md)

