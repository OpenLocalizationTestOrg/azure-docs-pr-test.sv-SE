---
title: "aaaStarting och stoppa virtuella datorer med Azure Automation - PowerShell-arbetsflöde | Microsoft Docs"
description: Grafiska versionen av Azure Automation-scenariot inklusive runbooks toostart och stoppa klassiska virtuella datorer.
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: d380bd43-d45d-45af-a5b2-78e7f66263c3
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/06/2016
ms.author: magoedte;bwren
redirect_url: https://docs.microsoft.com/azure/automation/automation-solution-vm-management
redirect_document_id: False
ms.openlocfilehash: 273631c7fc5ddb989b3bbdc82b470ac3af6ee482
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

Detta är hello PowerShell-arbetsflöde runbook-versionen av det här scenariot. Det är också tillgängliga använder [grafiska runbook-flöden](automation-solution-startstopvm-graphical.md).

## <a name="getting-hello-scenario"></a>Hämta hello scenario
Det här scenariot består av två runbooks i PowerShell-arbetsflöde som du kan hämta från hello följande länkar.  Se hello [grafiska version](automation-solution-startstopvm-graphical.md) i det här scenariot för länkar toohello grafiska runbook-flöden.

| Runbook | Länk | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| Start-AzureVMs |[Starta klassiska virtuella Azure-datorer](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) |PowerShell-arbetsflöde |Startar alla klassiska virtuella datorer i en Azure subscriptionor alla virtuella datorer med ett visst namn. |
| Stoppa AzureVMs |[Stoppa klassiska virtuella Azure-datorer](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) |PowerShell-arbetsflöde |Stoppar alla virtuella datorer i ett automation-konto eller alla virtuella datorer med ett visst namn. |

## <a name="installing-and-configuring-hello-scenario"></a>Installera och konfigurera hello scenario
### <a name="1-install-hello-runbooks"></a>1. Installera hello runbooks
När du har hämtat hello runbooks, kan du importera dem med hjälp av proceduren hello i [importera en Runbook](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).

### <a name="2-review-hello-description-and-requirements"></a>2. Granska hello beskrivning och krav
Hej runbooks är kommenterade hjälptext som innehåller en beskrivning och nödvändiga tillgångar.  Du kan också få hello samma information från den här artikeln.

### <a name="3-configure-assets"></a>3. Konfigurera tillgångar
Hej runbooks kräver hello efter resurser som du måste skapa och fylla i med lämpliga värden.

| Tillgångstypen | Tillgångsnamnet | Beskrivning |
|:--- |:--- |:--- |:--- |
| Autentiseringsuppgift |AzureCredential |Innehåller autentiseringsuppgifter för ett konto som har behörighet toostart och stoppa virtuella datorer i hello Azure-prenumeration.  Du kan också ange en annan autentiseringsuppgiftstillgång i hello **autentiseringsuppgifter** parametern för hello **Add-AzureAccount** aktivitet. |
| Variabel |AzureSubscriptionId |Innehåller hello prenumerations-ID för din Azure-prenumeration. |

## <a name="using-hello-scenario"></a>Med hello scenariot
### <a name="parameters"></a>Parametrar
Hej runbooks har hello följande parametrar.  Du måste ange värden för alla obligatoriska parametrar och kan du ange värden för andra parametrar beroende på dina krav.

| Parameter | Typ | Obligatorisk | Beskrivning |
|:--- |:--- |:--- |:--- |
| Tjänstnamn |Sträng |Nej |Om ett värde anges, och sedan alla virtuella datorer med samma tjänstnamn startas eller stoppas.  Om inget värde anges, sedan alla klassiska virtuella datorer i hello Azure-prenumeration startas eller stoppas. |
| AzureSubscriptionIdAssetName |Sträng |Nej |Innehåller hello namnet på hello [variabeltillgång](#installing-and-configuring-the-scenario) som innehåller hello prenumerations-ID för din Azure-prenumeration.  Om du inte anger ett värde, *AzureSubscriptionId* används. |
| AzureCredentialAssetName |Sträng |Nej |Innehåller hello namnet på hello [autentiseringsuppgiftstillgång](#installing-and-configuring-the-scenario) som innehåller hello autentiseringsuppgifter för hello runbook toouse.  Om du inte anger ett värde, *AzureCredential* används. |

### <a name="starting-hello-runbooks"></a>Starta hello runbooks
Du kan använda någon av hello metoder i [starta en runbook i Azure Automation](automation-starting-a-runbook.md) toostart antingen hello runbooks i det här scenariot.

följande exempelkommandon hello använder Windows PowerShell toorun **StartAzureVMs** toostart alla virtuella datorer med hello tjänstnamnet *MyVMService*.

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a>Resultat
Hej runbooks kommer [utdata meddelandet](automation-runbook-output-and-messages.md) för varje virtuell dator som anger huruvida hello starta eller stoppa instruktion har skickats.  Du kan söka efter en specifik sträng i hello utgående toodetermine hello resultatet för varje runbook.  hello eventuella utdatasträngar visas i hello i den följande tabellen.

| Runbook | Villkor | Meddelande |
|:--- |:--- |:--- |
| Start-AzureVMs |Virtuell dator körs redan |MyVM körs redan |
| Start-AzureVMs |Start-begäran för den virtuella datorn har skickats |MyVM har startats |
| Start-AzureVMs |Det gick inte att startbegäran för den virtuella datorn |MyVM misslyckades toostart |
| Stoppa AzureVMs |Virtuell dator har redan stoppats. |MyVM har redan stoppats. |
| Stoppa AzureVMs |Stoppa begäran för den virtuella datorn har skickats |MyVM har stoppats |
| Stoppa AzureVMs |Stop-begäran för den virtuella datorn misslyckades |MyVM misslyckades toostop |

Till exempel försöker hello följande kodavsnitt från en runbook toostart alla virtuella datorer med hello tjänstnamnet *MyServiceName*.  Om någon av hello startar begäranden misslyckas, kan åtgärder vidtas.

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action tootake in case of success.
        }
        else {
            # Action tootake in case of error.
        }
    }


## <a name="detailed-breakdown"></a>Detaljerad analys
Nedan följer en detaljerad analys av hello runbooks i det här scenariot.  Du kan använda den här informationen tooeither anpassa hello runbooks eller bara toolearn från dem för att skapa egna automatiseringsscenarier.

### <a name="parameters"></a>Parametrar
    param (
        [Parameter(Mandatory=$false)]
        [String]  $AzureCredentialAssetName = 'AzureCredential',

        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)]
        [String] $ServiceName
    )

hello-arbetsflöde inleds med att hämta hello värden för hello [indataparametrar](#using-the-scenario).  Standardnamn används om hello resursnamn inte har angetts.

### <a name="output"></a>Resultat
    # Returns strings with status messages
    [OutputType([String])]

Den här raden deklarerar att hello utdata från hello runbook ska vara en sträng.  Detta är inte obligatoriskt men det är bästa praxis för när hello runbook används som en [underordnad runbook](automation-child-runbooks.md) så att en överordnad runbook vet hello utdata skriver tooexpect.

### <a name="authentication"></a>Autentisering
    # Connect tooAzure and select hello subscription toowork against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

hello nästa rader ange hello [autentiseringsuppgifter](automation-credentials.md) och Azure-prenumeration som ska användas för hello resten av hello runbook.
Första vi använder **Get-AutomationPSCredential** tooget hello tillgång som innehåller hello behörighet med åtkomst toostart och stoppa virtuella datorer i hello Azure-prenumeration. **Lägg till AzureAccount** använder den här tillgången tooset hello autentiseringsuppgifter.  hello utdata tilldelas tooa dummy variabeln så att den inte ingår i hello runbook-utdata.  

Hej variabeltillgång med hello prenumerations-ID: T hämtas med **Get-automationvariable,** och hello prenumeration med **Välj AzureSubscription**.

### <a name="get-vms"></a>Hämta virtuella datorer
    # If there is a specific cloud service, then get all VMs in hello service,
    # otherwise get all VMs in hello subscription.
    if ($ServiceName)
    {
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else
    {
        $VMs = Get-AzureVM
    }

**Get-AzureVM** är används tooretrieve hello virtuella datorer hello runbook fungerar med.  Om ett värde har angetts i hello **ServiceName** ange variabel och sedan endast hello virtuella datorer med att namn har hämtats.  Om **ServiceName** är tom, hämtas alla virtuella datorer.

### <a name="startstop-virtual-machines-and-send-output"></a>Starta/stoppa virtuella datorer och skicka utdata
    # Start each of hello stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # hello VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # hello VM needs toobe started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # hello VM failed toostart, so send notice
                Write-Output ($VM.InstanceName + " failed toostart")
            }
            else
            {
                # hello VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

hello nästa rader gå igenom varje virtuell dator.  Först hello **PowerState** av hello virtuella datorn är markerad toosee om den redan är igång eller stoppad beroende hello runbook.  Om det redan finns i hello måltillståndet skickas ett meddelande toooutput och hello runbook slutar.  Om inte, sedan **Start AzureVM** eller **stoppa AzureVM** är används tooattempt toostart eller stoppa hello virtuell dator med hello resultatet av hello begäran lagrade tooa variabeln.  Ett meddelande skickas toooutput anger om hello begäran toostart eller stoppa har skickats.

## <a name="next-steps"></a>Nästa steg
* toolearn mer information om hur du arbetar med underordnade runbooks finns [underordnade runbooks i Azure Automation](automation-child-runbooks.md)
* Mer om toolearn utgående meddelanden under körning av runbook och loggning toohelp felsökning, se [Runbook-utdata och meddelanden i Azure Automation](automation-runbook-output-and-messages.md)

