---
title: "Starta och stoppa virtuella datorer med Azure Automation - PowerShell-arbetsflöde | Microsoft Docs"
description: "Grafiska versionen av Azure Automation-scenariot inklusive runbooks för att starta och stoppa klassiska virtuella datorer."
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
redirect_document_id: FALSE
ms.openlocfilehash: 95a7b02b0d11bf18c398daea48d16e0ead30b543
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a>Azure Automation-scenario – starta och stoppa virtuella datorer
Det här scenariot för Azure Automation innehåller runbooks för att starta och stoppa klassiska virtuella datorer.  Du kan använda det här scenariot för något av följande:  

* Använda runbooks utan ändringar i din egen miljö.
* Ändra runbooks för att utföra anpassade funktioner.  
* Anropa runbooks från en annan runbook som en del av en övergripande lösning.
* Använda runbooks som självstudier för att lära dig runbook authoring begrepp.

> [!div class="op_single_selector"]
> * [Grafisk](automation-solution-startstopvm-graphical.md)
> * [PowerShell-arbetsflöde](automation-solution-startstopvm-psworkflow.md)
> 
> 

Detta är PowerShell-arbetsflöde runbook-versionen av det här scenariot. Det är också tillgängliga använder [grafiska runbook-flöden](automation-solution-startstopvm-graphical.md).

## <a name="getting-the-scenario"></a>Hämta scenariot
Det här scenariot består av två PowerShell-arbetsflöde runbooks som kan hämtas från följande länkar.  Finns det [grafiska version](automation-solution-startstopvm-graphical.md) i det här scenariot länkar till grafiska runbook-flöden.

| Runbook | Länk | Typ | Beskrivning |
|:--- |:--- |:--- |:--- |
| Start-AzureVMs |[Starta klassiska virtuella Azure-datorer](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) |PowerShell-arbetsflöde |Startar alla klassiska virtuella datorer i en Azure subscriptionor alla virtuella datorer med ett visst namn. |
| Stoppa AzureVMs |[Stoppa klassiska virtuella Azure-datorer](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) |PowerShell-arbetsflöde |Stoppar alla virtuella datorer i ett automation-konto eller alla virtuella datorer med ett visst namn. |

## <a name="installing-and-configuring-the-scenario"></a>Installera och konfigurera scenariot
### <a name="1-install-the-runbooks"></a>1. Installera runbooks
När du har hämtat runbooks, kan du importera dem med hjälp av proceduren i [importera en Runbook](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).

### <a name="2-review-the-description-and-requirements"></a>2. Granska beskrivning och krav
Runbooks är kommenterade hjälptext som innehåller en beskrivning och nödvändiga tillgångar.  Du kan också få samma information från den här artikeln.

### <a name="3-configure-assets"></a>3. Konfigurera tillgångar
Runbooks kräver följande resurser som du måste skapa och fylla i med lämpliga värden.

| Tillgångstypen | Tillgångsnamnet | Beskrivning |
|:--- |:--- |:--- |:--- |
| Autentiseringsuppgift |AzureCredential |Innehåller autentiseringsuppgifter för ett konto som har behörighet att starta och stoppa virtuella datorer i Azure-prenumerationen.  Du kan också ange en annan autentiseringsuppgiftstillgång i den **autentiseringsuppgifter** parameter för den **Add-AzureAccount** aktivitet. |
| Variabel |AzureSubscriptionId |Innehåller prenumerations-ID för din Azure-prenumeration. |

## <a name="using-the-scenario"></a>Med scenario
### <a name="parameters"></a>Parametrar
Runbooks har följande parametrar.  Du måste ange värden för alla obligatoriska parametrar och kan du ange värden för andra parametrar beroende på dina krav.

| Parameter | Typ | Obligatorisk | Beskrivning |
|:--- |:--- |:--- |:--- |
| Tjänstnamn |Sträng |Nej |Om ett värde anges, och sedan alla virtuella datorer med samma tjänstnamn startas eller stoppas.  Om inget värde anges, är sedan alla klassiska virtuella datorer i Azure-prenumerationen startas eller stoppas. |
| AzureSubscriptionIdAssetName |Sträng |Nej |Innehåller namnet på den [variabeltillgång](#installing-and-configuring-the-scenario) som innehåller prenumerations-ID för din Azure-prenumeration.  Om du inte anger ett värde, *AzureSubscriptionId* används. |
| AzureCredentialAssetName |Sträng |Nej |Innehåller namnet på den [autentiseringsuppgiftstillgång](#installing-and-configuring-the-scenario) som innehåller autentiseringsuppgifter för runbook ska användas.  Om du inte anger ett värde, *AzureCredential* används. |

### <a name="starting-the-runbooks"></a>Starta runbooks
Du kan använda någon av metoderna i [starta en runbook i Azure Automation](automation-starting-a-runbook.md) starta antingen runbooks i det här scenariot.

Följande exempelkommandon använder Windows PowerShell för att köra **StartAzureVMs** starta alla virtuella datorer med tjänstnamnet *MyVMService*.

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a>Resultat
Runbooks kommer [utdata meddelandet](automation-runbook-output-and-messages.md) för varje virtuell dator som anger huruvida starta eller stoppa instruktion skickades.  Du kan söka efter en specifik sträng i utdata att fastställa resultatet för varje runbook.  I följande tabell visas eventuella utdata-strängar.

| Runbook | Villkor | Meddelande |
|:--- |:--- |:--- |
| Start-AzureVMs |Virtuell dator körs redan |MyVM körs redan |
| Start-AzureVMs |Start-begäran för den virtuella datorn har skickats |MyVM har startats |
| Start-AzureVMs |Det gick inte att startbegäran för den virtuella datorn |Det gick inte att starta MyVM |
| Stoppa AzureVMs |Virtuell dator har redan stoppats. |MyVM har redan stoppats. |
| Stoppa AzureVMs |Stoppa begäran för den virtuella datorn har skickats |MyVM har stoppats |
| Stoppa AzureVMs |Stop-begäran för den virtuella datorn misslyckades |Det gick inte att stoppa MyVM |

Till exempel följande kodavsnitt från en runbook försöker starta alla virtuella datorer med tjänstnamnet *MyServiceName*.  Om någon av start-begäranden misslyckas, kan åtgärder vidtas.

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action to take in case of success.
        }
        else {
            # Action to take in case of error.
        }
    }


## <a name="detailed-breakdown"></a>Detaljerad analys
Nedan följer en detaljerad analys av runbooks i det här scenariot.  Du kan använda den här informationen för att anpassa runbooks eller bara om du vill veta från dem för att skapa egna automatiseringsscenarier.

### <a name="parameters"></a>Parametrar
    param (
        [Parameter(Mandatory=$false)]
        [String]  $AzureCredentialAssetName = 'AzureCredential',

        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)]
        [String] $ServiceName
    )

Arbetsflödet startar med att hämta värden för den [indataparametrar](#using-the-scenario).  Om de tillgången inte anges används standardnamn.

### <a name="output"></a>Resultat
    # Returns strings with status messages
    [OutputType([String])]

Den här raden deklarerar att utdata från runbooken ska vara en sträng.  Detta är inte obligatoriskt men det är bästa praxis för när runbook används som en [underordnad runbook](automation-child-runbooks.md) så att en överordnad runbook vet utdatatypen förväntar dig.

### <a name="authentication"></a>Autentisering
    # Connect to Azure and select the subscription to work against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

Nästa rader uppsättningen av [autentiseringsuppgifter](automation-credentials.md) och Azure-prenumeration som ska användas för resten av runbook.
Första vi använder **Get-AutomationPSCredential** att hämta den tillgång som innehåller autentiseringsuppgifter för åtkomst till starta och stoppa virtuella datorer i Azure-prenumerationen. **Lägg till AzureAccount** använder sedan tillgången för att ange autentiseringsuppgifter.  Utdata har tilldelats en dummy variabel så att den inte ingår i runbook-utdata.  

Variabeltillgång med ID: T hämtas med prenumerationen **Get-automationvariable,** och prenumerationen med **Välj AzureSubscription**.

### <a name="get-vms"></a>Hämta virtuella datorer
    # If there is a specific cloud service, then get all VMs in the service,
    # otherwise get all VMs in the subscription.
    if ($ServiceName)
    {
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else
    {
        $VMs = Get-AzureVM
    }

**Get-AzureVM** används för att hämta de virtuella datorerna runbook fungerar med.  Om ett värde har angetts i den **ServiceName** ange variabel, och sedan på virtuella datorer med att namn har hämtats.  Om **ServiceName** är tom, hämtas alla virtuella datorer.

### <a name="startstop-virtual-machines-and-send-output"></a>Starta/stoppa virtuella datorer och skicka utdata
    # Start each of the stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # The VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # The VM needs to be started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # The VM failed to start, so send notice
                Write-Output ($VM.InstanceName + " failed to start")
            }
            else
            {
                # The VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

På nästa rad gå igenom varje virtuell dator.  Första den **PowerState** för den virtuella datorn kontrolleras för att se om den redan är igång eller Stoppad, beroende på runbook.  Om det redan finns i måltillståndet skickas ett meddelande till utdata och runbook avslutas.  Om inte, sedan **Start AzureVM** eller **stoppa AzureVM** används för att försöka starta eller stoppa den virtuella datorn med resultatet av begäran lagras i en variabel.  Ett meddelande skickas sedan till utdata anger om begäran om att starta eller stoppa har skickats.

## <a name="next-steps"></a>Nästa steg
* Mer information om hur du arbetar med underordnade runbooks finns [underordnade runbooks i Azure Automation](automation-child-runbooks.md)
* Läs mer om utgående meddelanden under körning av runbook och loggning för att felsöka i [Runbook-utdata och meddelanden i Azure Automation](automation-runbook-output-and-messages.md)

