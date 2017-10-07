---
title: "Diagram över aaaStarting och stoppa virtuella datorer – | Microsoft Docs"
description: "PowerShell-arbetsflöde version av Azure Automation-scenariot inklusive runbooks toostart och stoppa klassiska virtuella datorer."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 62d93170-ca3d-42ef-ac1d-6d50b61ac87d
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/06/2016
ms.author: magoedte;bwren
redirect_url: https://docs.microsoft.com/azure/automation/automation-solution-vm-management
redirect_document_id: False
ms.openlocfilehash: 5add8d8cf35ea2e89a570744755ac7db0a6feb07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a>Azure Automation-scenario – starta och stoppa virtuella datorer
Det här scenariot för Azure Automation innehåller runbooks toostart och stoppa klassiska virtuella datorer.  Du kan använda det här scenariot för hello följande:  

* Använd hello runbooks utan ändringar i din egen miljö.
* Ändra hello runbooks tooperform anpassade funktioner.  
* Anropa hello runbooks från en annan runbook som en del av en övergripande lösning.
* Använd hello runbooks som självstudier toolearn runbook authoring begrepp.

> [!div class="op_single_selector"]
> * [Grafisk](automation-solution-startstopvm-graphical.md)
> * [PowerShell-arbetsflöde](automation-solution-startstopvm-psworkflow.md)
>
>

Detta är hello grafiska runbook-versionen av det här scenariot. Det är också tillgängliga använder [PowerShell-arbetsflöde runbooks](automation-solution-startstopvm-psworkflow.md).

## <a name="getting-hello-scenario"></a>Hämta hello scenario
Det här scenariot består av två två grafiska runbook-flöden som du kan hämta från hello följande länkar.  Se hello [PowerShell-arbetsflöde version](automation-solution-startstopvm-psworkflow.md) i det här scenariot för länkar toohello PowerShell-arbetsflöde runbooks.

| Runbook | Länk | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| StartAzureClassicVM |[Starta klassiska virtuella Azure-datorns grafisk Runbook](https://gallery.technet.microsoft.com/scriptcenter/Start-Azure-Classic-VM-c6067b3d) |Grafisk |Startar alla klassiska virtuella datorer i en Azure-prenumeration eller alla virtuella datorer med ett visst namn. |
| StopAzureClassicVM |[Stoppa klassiska virtuella Azure-datorns grafisk Runbook](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-Classic-VM-397819bd) |Grafisk |Stoppar alla virtuella datorer i ett automation-konto eller alla virtuella datorer med ett visst namn. |

## <a name="installing-and-configuring-hello-scenario"></a>Installera och konfigurera hello scenario
### <a name="1-install-hello-runbooks"></a>1. Installera hello runbooks
När du har hämtat hello runbooks, kan du importera dem med hjälp av proceduren hello i [grafisk runbook procedurer](automation-graphical-authoring-intro.md#graphical-runbook-procedures).

### <a name="2-review-hello-description-and-requirements"></a>2. Granska hello beskrivning och krav
Hej runbooks innehåller en aktivitet som kallas **viktigt** som innehåller en beskrivning och nödvändiga tillgångar.  Du kan visa den här informationen genom att välja hello **viktigt** aktivitet och hello **Arbetsflödesskriptet** parameter.  Du kan också få hello samma information från den här artikeln.

### <a name="3-configure-assets"></a>3. Konfigurera tillgångar
Hej runbooks kräver hello efter resurser som du måste skapa och fylla i med lämpliga värden.  hello namn är standard.  Du kan använda tillgångar med olika namn om du anger namnen i hello [indataparametrar](#using-the-runbooks) när du startar hello runbook.

| Tillgångstypen | Standardnamnet | Beskrivning |
|:--- |:--- |:--- |:--- |
| [Autentiseringsuppgifter](automation-credentials.md) |AzureCredential |Innehåller autentiseringsuppgifter för ett konto som har behörighet toostart och stoppa virtuella datorer i hello Azure-prenumeration. |
| [Variabeln](automation-variables.md) |AzureSubscriptionId |Innehåller hello prenumerations-ID för din Azure-prenumeration. |

## <a name="using-hello-scenario"></a>Med hello scenariot
### <a name="parameters"></a>Parametrar
Hej runbooks varje har hello följande [indataparametrar](automation-starting-a-runbook.md#runbook-parameters).  Du måste ange värden för alla obligatoriska parametrar och kan du ange värden för andra parametrar beroende på dina krav.

| Parameter | Typ | Obligatorisk | Beskrivning |
|:--- |:--- |:--- |:--- |
| Tjänstnamn |Sträng |Nej |Om ett värde anges, och sedan alla virtuella datorer med samma tjänstnamn startas eller stoppas.  Om inget värde anges, sedan alla klassiska virtuella datorer i hello Azure-prenumeration startas eller stoppas. |
| AzureSubscriptionIdAssetName |Sträng |Nej |Innehåller hello namnet på hello [variabeltillgång](#installing-and-configuring-the-scenario) som innehåller hello prenumerations-ID för din Azure-prenumeration.  Om du inte anger ett värde, *AzureSubscriptionId* används. |
| AzureCredentialAssetName |Sträng |Nej |Innehåller hello namnet på hello [autentiseringsuppgiftstillgång](#installing-and-configuring-the-scenario) som innehåller hello autentiseringsuppgifter för hello runbook toouse.  Om du inte anger ett värde, *AzureCredential* används. |

### <a name="starting-hello-runbooks"></a>Starta hello runbooks
Du kan använda någon av hello metoder i [starta en runbook i Azure Automation](automation-starting-a-runbook.md) toostart antingen hello runbooks i den här artikeln.

följande exempelkommandon hello använder Windows PowerShell toorun **StartAzureClassicVM** toostart alla virtuella datorer med hello tjänstnamnet *MyVMService*.

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "StartAzureClassicVM" –Parameters $params

### <a name="output"></a>Resultat
Hej runbooks kommer [utdata meddelandet](automation-runbook-output-and-messages.md) för varje virtuell dator som anger huruvida hello starta eller stoppa instruktion har skickats.  Du kan söka efter en specifik sträng i hello utgående toodetermine hello resultatet för varje runbook.  hello eventuella utdatasträngar visas i hello i den följande tabellen.

| Runbook | Villkor | Meddelande |
|:--- |:--- |:--- |
| StartAzureClassicVM |Virtuell dator körs redan |MyVM körs redan |
| StartAzureClassicVM |Start-begäran för den virtuella datorn har skickats |MyVM har startats |
| StartAzureClassicVM |Det gick inte att startbegäran för den virtuella datorn |MyVM misslyckades toostart |
| StopAzureClassicVM |Virtuell dator körs redan |MyVM har redan stoppats. |
| StopAzureClassicVM |Start-begäran för den virtuella datorn har skickats |MyVM har startats |
| StopAzureClassicVM |Det gick inte att startbegäran för den virtuella datorn |MyVM misslyckades toostart |

Följande är en bild med hello **StartAzureClassicVM** som en [underordnad runbook](automation-child-runbooks.md) i en grafisk exempel-runbook.  Här används hello villkorliga länkar i hello i den följande tabellen.

| Länk | Kriterie |
|:--- |:--- |
| Lyckade länk |$ActivityOutput [StartAzureClassicVM]-som ”\* har startats” |
| Fel-länk |$ActivityOutput [StartAzureClassicVM]-notlike ”\* har startats” |

![Underordnad runbook-exempel](media/automation-solution-startstopvm/graphical-childrunbook-example.png)

## <a name="detailed-breakdown"></a>Detaljerad analys
Nedan följer en detaljerad analys av hello runbooks i det här scenariot.  Du kan använda den här informationen tooeither anpassa hello runbooks eller bara toolearn från dem för att skapa egna automatiseringsscenarier.

### <a name="authentication"></a>Autentisering
![Autentisering](media/automation-solution-startstopvm/graphical-authentication.png)

Hej runbook startas med aktiviteter tooset hello [autentiseringsuppgifter](automation-credentials.md) och Azure-prenumeration som ska användas för hello resten av hello runbook.

Hej först två aktiviteter **hämta prenumerations-Id** och **hämta autentiseringsuppgifter för Azure**, hämta hello [tillgångar](#installing-the-runbook) som används av hello två aktiviteter.  Dessa aktiviteter kan direkt ange hello tillgångar, men de måste hello resursnamn.  Eftersom vi tillåter att hello användaren toospecify namnen i hello [indataparametrar](#using-the-runbooks), måste dessa aktiviteter tooretrieve hello tillgångar med ett namn som anges av en indataparameter.

**Lägg till AzureAccount** anger hello autentiseringsuppgifter som ska användas för hello resten av hello runbook.  Hej autentiseringsuppgiftstillgång som hämtas från **hämta autentiseringsuppgifter för Azure** måste ha åtkomst toostart och stoppa virtuella datorer i hello Azure-prenumeration.  hello prenumeration som används är markerad som **Välj AzureSubscription** som använder hello prenumerations-Id från **hämta prenumerations-Id**.

### <a name="get-virtual-machines"></a>Hämta virtuella datorer
![Hämta virtuella datorer](media/automation-solution-startstopvm/graphical-getvms.png)

Hej runbook behöver toodetermine vilka virtuella datorer det kommer att arbeta med och om de redan igång eller Stoppad (beroende på hello runbook).   En av två aktiviteter hämtar hello virtuella datorer.  **Hämta virtuella datorer i tjänsten** körs om hello *ServiceName* Indataparametern för hello runbook innehåller ett värde.  **Hämta alla virtuella datorer** körs om hello *ServiceName* Indataparametern för hello runbook innehåller inte något värde.  Detta utförs genom hello villkorliga länkar före varje aktivitet.

Båda aktiviteter använder hello **Get-AzureVM** cmdlet.  **Hämta alla virtuella datorer** använder hello **ListAllVMs** parameterinställning tooreturn alla virtuella datorer.  **Hämta virtuella datorer i tjänsten** använder hello **GetVMByServiceAndVMName** parametern och som ger hello **ServiceName** indataparameter för hello **ServiceName**parameter.  

### <a name="merge-vms"></a>Koppla virtuella datorer
![Koppla virtuella datorer](media/automation-solution-startstopvm/graphical-mergevms.png)

hello **sammanfoga VMs** aktiviteten är obligatoriska tooprovide som indata för**Start AzureVM** som måste hello namn och tjänsten på hello toostart för virtuella datorer.  Att indata kan komma från antingen **hämta alla virtuella datorer** eller **hämta virtuella datorer i tjänsten**, men **Start AzureVM** kan bara ange en aktivitet för sina ingående.   

hello scenario är toocreate **sammanfoga VMs** som körs hello **Write-Output** cmdlet.  Hej **InputObject** -parametern för denna cmdlet är ett PowerShell-uttryck som kombinerar hello indata hello föregående två aktiviteter.  Endast en av dessa aktiviteter körs så att endast en uppsättning av utdata som förväntas.  **Start-AzureVM** kan använda dessa utdata för dess indataparametrar.

### <a name="startstop-virtual-machines"></a>Starta/stoppa virtuella datorer
![Starta virtuella datorer](media/automation-solution-startstopvm/graphical-startvm.png) ![Stoppa virtuella datorer](media/automation-solution-startstopvm/graphical-stopvm.png)

Beroende på hello runbook hello nästa aktiviteter försöker toostart eller stoppa hello runbook med hjälp av **Start AzureVM** eller **stoppa AzureVM**.  Eftersom hello aktivitet föregås av en pipelinelänk, körs en gång för varje objekt som returnerades från **sammanfoga VMs**.  hello länken är villkorlig så att hello aktiviteten körs bara om hello *RunningState* hello virtuella datorn är *stoppad* för **Start AzureVM** och * Starta* för **stoppa AzureVM**. Om detta inte är uppfyllt, sedan **meddela redan startats** eller **meddela har redan stoppats** körs toosend ett meddelande med hjälp av **Write-Output**.

### <a name="send-output"></a>Skicka utdata
![Meddela starta virtuella datorer](media/automation-solution-startstopvm/graphical-notifystart.png) ![Meddela stoppa virtuella datorer](media/automation-solution-startstopvm/graphical-notifystop.png)

hello sista steget i hello runbook är toosend utdata om hello start eller stop-begäran för varje virtuell dator har skickats. Det finns en separat **Write-Output** aktivitet skapas för varje, och vi ta reda på vilken en toorun med villkorliga länkar.  **Meddela VM igång** eller **meddela VM stoppats** körs om *OperationStatus* är *lyckades*.  Om *OperationStatus* har ett annat värde sedan **det gick inte att meddela tooStart** eller **det gick inte att meddela tooStop** körs.

## <a name="next-steps"></a>Nästa steg
* [Grafisk redigering i Azure Automation](automation-graphical-authoring-intro.md)
* [Underordnade runbooks i Azure Automation](automation-child-runbooks.md)
* [Runbook-utdata och meddelanden i Azure Automation](automation-runbook-output-and-messages.md)
