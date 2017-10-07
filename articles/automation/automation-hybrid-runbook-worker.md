---
title: aaaAzure Automation Hybrid Runbook Worker | Microsoft Docs
description: "Den här artikeln innehåller information om installation och användning av Hybrid Runbook Worker som är en funktion i Azure Automation som gör att du toorun runbooks på datorer i ditt lokala datacenter eller molnet providern."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 06227cda-f3d1-47fe-b3f8-436d2b9d81ee
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: magoedte;bwren
ms.openlocfilehash: ccee35e8324149a79ff692a867e5ce7801299bcb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automate-resources-in-your-data-center-or-cloud-with-hybrid-runbook-worker"></a>Automatisera resurser i ditt datacenter eller molnet med Hybrid Runbook Worker
Runbooks i Azure Automation kan inte komma åt resurser i andra moln eller lokala miljö eftersom de körs i hello Azure-molnet.  hello Hybrid Runbook Worker-funktionen i Azure Automation kan du toorun runbooks direkt på värddatorn för hello hello roll och mot resurser i hello miljö toomanage de lokala resurserna. Runbooks lagras och hanteras i Azure Automation och sedan levereras tooone eller fler avsedda datorer.  

Den här funktionen visas i följande bild hello:<br>  

![Hybrid Runbook Worker-översikt](media/automation-offering-get-started/automation-infradiagram-networkcomms.png)

En teknisk översikt över hello Hybrid Runbook Worker-rollen och distribution överväganden finns [Automation arkitektur, översikt](automation-offering-get-started.md#automation-architecture-overview).    

## <a name="hybrid-runbook-worker-groups"></a>Hybrid Runbook Worker-grupper
Varje Runbook Worker-Hybrid är medlem i en Hybrid Runbook Worker-grupp som du anger när du installerar hello agent.  En grupp kan innehålla en enskild agent, men du kan installera flera agenter i en grupp för hög tillgänglighet.

När du startar en runbook på en Hybrid Runbook Worker kan ange du hello-grupp som den körs på.  hello medlemmar i gruppen för hello avgör vilken worker services hello-begäran.  Du kan inte ange en viss worker.

## <a name="relationship-tooservice-management-automation"></a>Relationen tooService Management Automation
[Service Management Automation (SMA)](https://technet.microsoft.com/library/dn469260.aspx) kan du toorun hello samma runbooks som stöds av Azure Automation i ditt lokala datacenter. SMA distribueras vanligen tillsammans med Windows Azure-paket, som Windows Azure-paket som innehåller ett grafiskt gränssnitt för hantering av SMA. Till skillnad från Azure Automation kräver SMA en lokal installation som innehåller web servrar toohost hello API, en databas toocontain runbooks och SMA konfiguration och Runbook Workers tooexecute runbook-jobb. Azure Automation tillhandahåller dessa tjänster i hello molnet och behöver du bara toomaintain hello Hybrid Runbook Worker-arbeten i din lokala miljö.

Om du är en befintlig SMA-användare, kan du flytta dina runbooks tooAzure Automation toobe som används med Hybrid Runbook Worker utan ändringar, förutsatt att de fungerar tooresources sin egen autentisering som beskrivs i [köra runbooks på en Hybrid Runbook Worker](automation-hrw-run-runbooks.md).  Runbooks i SMA körs i hello kontext för hello-tjänstkontot på hello worker-server som kan tillhandahålla att autentiseringen för hello runbooks.

Du kan använda hello följande kriterier toodetermine om Azure Automation Hybrid Runbook Worker eller Service Management Automation är mer lämpliga för dina behov.

* SMA kräver en lokal installation av dess underliggande komponenter som är anslutna tooWindows Azure Pack om grafiska hanteringsgränssnittet krävs. Flera lokala resurser behövs med högre underhållskostnader än Azure Automation som bara behöver en agent installeras på lokala runbook workers. hello agenter hanteras av Operations Management Suite ytterligare minska underhållskostnaderna.
* Azure Automation lagrar dess runbooks i hello molntjänster och ger dem tooon lokala Hybrid Runbook Worker. Om säkerhetsprinciperna inte tillåter detta problem bör du använda SMA.
* SMA ingår i System Center. och därför kräver en licens för System Center 2012 R2. Azure Automation baseras på en skiktad prenumeration modell.
* Azure Automation har avancerade funktioner, till exempel grafiska runbook-flöden som inte är tillgängliga i SMA.

## <a name="installing-hello-windows-hybrid-runbook-worker"></a>Installera hello Windows Hybrid Runbook Worker 

tooinstall och konfigurera en Windows-Hybrid Runbook Worker, det finns två metoder.  hello rekommenderas metoden använder en Automation runbook toocompletely automatisera hello krävs tooconfigure en Windows-dator.  hello andra metoden efter en stegvisa anvisningar toomanually installera och konfigurera hello roll.  

> [!NOTE]
> toomanage hello konfigurationen för dina servrar som stöder hello Hybrid Runbook Worker-rollen med önskad tillstånd Configuration DSC (), behöver du tooadd dem som DSC-noder.  Mer information om onboarding dem för hantering med DSC, se [Onboarding datorer för hantering av Azure Automation DSC](automation-dsc-onboarding.md).           
><br>
>Om du aktiverar hello [uppdatering hanteringslösning](../operations-management-suite/oms-solution-update-management.md), en Windows-dator ansluten tooyour OMS-arbetsytan konfigureras automatiskt som en Hybrid Runbook Worker toosupport runbooks som ingår i den här lösningen.  Det har inte registrerats med några Hybrid Worker-grupper som redan har definierats i ditt Automation-konto.  hello dator kan läggas tooa Hybrid Runbook Worker-gruppen i Automation-runbooks för toosupport ditt Automation-konto så länge som du använder samma konto för både hello lösningen och Hybrid Runbook Worker gruppmedlemskap hello.  Den här funktionen har lagts till tooversion 7.2.12024.0 av hello Hybrid Runbook Worker.  

Granska hello följande information om hello [maskin- och programvarukrav](automation-offering-get-started.md#hybrid-runbook-worker) och [information för att förbereda nätverket](automation-offering-get-started.md#network-planning) innan du börjar distribuera en Hybrid Runbook Worker.  När en runbook worker har distribuerats, granska [köra runbooks på en Hybrid Runbook Worker](automation-hrw-run-runbooks.md) toolearn hur tooconfigure runbooks-tooautomate processer i ditt lokala datacenter eller andra molnmiljö.  
 
### <a name="automated-deployment"></a>Automatisk distribution

Utföra hello följande steg tooautomate hello installationen och konfigurationen av hello Windows Hybrid Worker-rollen.  

1. Hämta hello *ny OnPremiseHybridWorker.ps1* skriptet från hello [PowerShell-galleriet](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker/1.0/DisplayScript) direkt från hello-dator som kör hello Hybrid Runbook Worker-rollen eller från en annan dator i din miljö och kopiera den toohello worker.  

    Hej *ny OnPremiseHybridWorker.ps1* skriptet kräver hello följande parametrar under körning:

  * *AutomationAccountName* (obligatoriskt) - hello namnet på ditt Automation-konto.  
  * *ResourceGroupName* (obligatoriskt) - hello namnet på hello resursgruppen som är associerade med ditt Automation-konto.  
  * *HybridGroupName* (obligatoriskt) - hello namnet på en Hybrid Runbook Worker-grupp som du anger som mål för hello runbooks som stöder det här scenariot. 
  *  *SubscriptionID* (obligatoriskt) - hello Azure prenumerations-Id som ditt Automation-konto.
  *  *WorkspaceName* (valfritt) – hello OMS arbetsytans namn.  Om du inte har en OMS-arbetsyta hello skriptet skapar och konfigurerar en.  

     > [!NOTE]
     > Det finns för närvarande hello endast Automation regioner som stöds för integrering med OMS - **Australien sydost**, **östra USA 2**, **Sydostasien**, och **Väst Europa**.  Om ditt Automation-konto inte är i någon av de regionerna, hello skriptet skapar en OMS-arbetsyta men den varnar dig om att det går inte att länka dem tillsammans.
     > 
2. På datorn, startar **Windows PowerShell** från hello **starta** skärm i administratörsläge.  
3. Navigera toohello mapp som innehåller hello skript du laddade ned och köra den ändrar hello värden för parametrar från hello PowerShell kommandoradsgränssnitt, *- AutomationAccountName*, *- ResourceGroupName* , *- HybridGroupName*, *- SubscriptionId*, och *- WorkspaceName*.

     > [!NOTE] 
     > Du kan ange tooauthenticate med Azure när du kör hello-skript.  Du **måste** logga in med ett konto som är medlem i rollen för hello Prenumerationsadministratörer och medadministratör för prenumerationen hello.  
     >  
    
        .\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> `
        -ResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
        -SubscriptionId <AzureSubscriptionId> -WorkspaceName <NameOfOMSWorkspace>

4. Du är ange tooagree tooinstall **NuGet** och du kan ange tooauthenticate med dina autentiseringsuppgifter för Azure.<br><br> ![Ny OnPremiseHybridWorker skript körs](media/automation-hybrid-runbook-worker/new-onpremisehybridworker-scriptoutput.png)

5. När hello skriptet har slutförts hello Hybrid Worker grupper bladet visar hello ny grupp och antal medlemmar eller om en befintlig grupp hello antal medlemmar ökas.  Du kan välja hello grupp hello listan hello **Hybrid Worker grupper** bladet och väljer hello **hybrider** panelen.  På hello **hybrider** bladet visas varje medlem i hello grupp i listan.  

### <a name="manual-deployment"></a>Manuell distribution 
Utföra hello två första stegen en gång för Automation-miljön och upprepar sedan hello återstående steg för varje worker-dator.

#### <a name="1-create-operations-management-suite-workspace"></a>1. Skapa Operations Management Suite-arbetsyta
Om du inte redan har en Operations Management Suite-arbetsyta, skapar du en med hjälp av anvisningarna i [hantera din arbetsyta](../log-analytics/log-analytics-manage-access.md). Du kan använda en befintlig arbetsyta om du redan har en.

#### <a name="2-add-automation-solution-toooperations-management-suite-workspace"></a>2. Lägg till Automation-lösningen tooOperations Management Suite-arbetsyta
Lösningar för att lägga till funktioner tooOperations Management Suite.  hello Automation-lösningen lägger till funktionalitet för Azure Automation, inklusive stöd för Runbook Worker-Hybrid.  När du lägger till hello lösning tooyour arbetsytan skickas det automatiskt ned worker komponenter toohello agentdatorn som du ska installera på hello nästa steg.

Följ instruktionerna för hello på [tooadd en lösning med hjälp av hello lösningar galleriet](../log-analytics/log-analytics-add-solutions.md) tooadd hello **Automation** lösning tooyour Operations Management Suite-arbetsyta.

#### <a name="3-install-hello-microsoft-monitoring-agent"></a>3. Installera hello Microsoft Monitoring Agent
hello Microsoft Monitoring Agent ansluter datorer tooOperations Management Suite.  När du installerar hello-agenten på den lokala datorn och ansluta den tooyour arbetsytan, hämtas automatiskt hello-komponenter som krävs för Runbook Worker-Hybrid.

Följ instruktionerna för hello på [ansluta Windows-datorer tooLog Analytics](../log-analytics/log-analytics-windows-agents.md) tooinstall hello-agenten på hello lokal dator.  Du kan upprepa processen för flera datorer tooadd tooyour miljö med flera personer.

När hello agent har lyckats ansluta tooOperations Management Suite, visas det på hello **anslutna källor** för hello Operations Management Suite **inställningar** fönstret.  Du kan verifiera den hello-agenten har korrekt hämtat hello Automation-lösningen när den har en mapp med namnet **AzureAutomationFiles** i C:\Program Files\Microsoft övervakning Agent\Agent.  tooconfirm hello versionen av hello Hybrid Runbook Worker kan du navigera tooC:\Program Files\Microsoft övervakning Agent\Agent\AzureAutomation\ och Observera hello \\ *version* undermappen.   

#### <a name="4-install-hello-runbook-environment-and-connect-tooazure-automation"></a>4. Installera hello runbook-miljön och ansluta tooAzure Automation
När du lägger till en agent tooOperations Management Suite hello Automation-lösningen push-meddelanden ned hello **HybridRegistration** PowerShell-modul som innehåller hello **Add-HybridRunbookWorker** cmdlet.  Du använder den här cmdlet tooinstall hello runbook-miljön på hello datorn och registrera den med Azure Automation.

Öppna ett PowerShell-session i administratörsläge och kör följande kommandon tooimport hello modulen hello.

    cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
    Import-Module HybridRegistration.psd1

Kör sedan hello **Add-HybridRunbookWorker** cmdlet med hello följande syntax:

    Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>

Du kan få hello information som krävs för denna cmdlet från hello **hantera nycklar** bladet i hello Azure-portalen.  Öppna bladet genom att välja hello **nycklar** alternativet från hello **inställningar** bladet i ditt Automation-konto.

![Hybrid Runbook Worker-översikt](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* **Gruppnamn** är hello namnet på hello Hybrid Runbook Worker-gruppen. Om den här gruppen finns redan i hello automation-kontot finns sedan hello aktuella dator läggs tooit.  Om den inte redan finns, sedan läggs den.
* **Slutpunkten** är hello **URL** i hello **hantera nycklar** bladet.
* **Token** är hello **primära åtkomstnyckeln** i hello **hantera nycklar** bladet.  

Använd hello **-Verbose** växel med **Add-HybridRunbookWorker** tooreceive detaljerad information om installation av hello.

#### <a name="5-install-powershell-modules"></a>5. Installera PowerShell-moduler
Runbooks kan använda någon av hello aktiviteter och cmdlets som definierats i hello-moduler som installerats i din Azure Automation-miljö.  Dessa moduler är inte automatiskt distribuerade tooon lokala datorer, så du måste installera dem manuellt.  hello undantaget är hello Azure-modulen är installerad som standard att tillhandahålla åtkomst toocmdlets för alla Azure-tjänster och aktiviteter för Azure Automation.

Eftersom hello Huvudsyftet med hello Hybrid Runbook Worker-funktionen är toomanage lokala resurser, måste du förmodligen tooinstall hello moduler som har stöd för dessa resurser.  Du kan se för[installerar moduler](http://msdn.microsoft.com/library/dd878350.aspx) information om hur du installerar Windows PowerShell-moduler.  Moduler som är installerade måste vara på en plats som refereras av PSModulePath miljövariabeln så att de importeras automatiskt av hello worker-Hybrid.  Mer information finns i [ändra hello PSModulePath installationssökväg](https://msdn.microsoft.com/library/dd878326%28v=vs.85%29.aspx). 

## <a name="removing-hybrid-runbook-worker"></a>Tar bort Hybrid Runbook Worker 
Du kan ta bort en eller flera Runbook Worker-hybrider från en grupp och du tar bort hello grupp, beroende på dina krav.  tooremove en Hybrid Runbook Worker från en lokal dator utföra hello följande steg.

1. Navigera tooyour Automation-konto i hello Azure-portalen.  
2. Från hello **inställningar** bladet väljer **nycklar** och notera hello värden för fältet **URL** och **primära åtkomstnyckeln**.  Du behöver den här informationen för hello nästa steg.
3. Öppna ett PowerShell-session i administratörsläge och kör hello följande kommando – `Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey>`.  Använd hello **-Verbose** växla för en detaljerad logg över hello borttagningen.

> [!NOTE]
> Detta tar inte bort hello Microsoft Monitoring Agent från hello dator kan endast hello funktioner och konfiguration av hello Hybrid Runbook Worker-rollen.  

## <a name="remove-hybrid-worker-groups"></a>Ta bort Hybrid Worker-grupper
tooremove en grupp, måste du först tooremove hello Hybrid Runbook Worker från varje dator som är medlem i gruppen för hello hello sätt som visades tidigare och utför sedan följande steg tooremove hello grupp hello.  

1. Öppna hello Automation-konto i hello Azure-portalen.
2. Välj hello **Hybrid Worker grupper** panelen och i hello **Hybrid Worker grupper** bladet, Välj hello-gruppen som du vill toodelete.  När du har valt hello specifik grupp hello **Hybrid worker-gruppen** egenskapsbladet visas.<br> ![Bladet hybrid Runbook Worker-grupp](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)   
3. Klicka på hello egenskapsbladet för hello markerad grupp **ta bort**.  Ett meddelande visas där du tillfrågas du tooconfirm åtgärden väljer **Ja** om du vill tooproceed.<br> ![Ta bort grupp bekräftelsedialogruta](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)<br> Den här processen kan ta flera sekunder toocomplete och du kan följa förloppet under **meddelanden** hello-menyn.  

## <a name="troubleshooting"></a>Felsökning 
hello Hybrid Runbook Worker beror på hello Microsoft Monitoring Agent toocommunicate med ditt Automation-konto tooregister hello worker, tar emot runbook-jobb och rapportera status. Om registreringen av hello worker misslyckas, är här några möjliga orsaker till hello-fel:  

1. hello hybrid worker finns bakom en proxyserver eller brandvägg.  
    Kontrollera hello datorn utgående åtkomst too*.azure-automation.net på port 443.  

2. hello datorn hello hybrid worker körs på har mindre än minimikraven för maskinvara hello [krav](automation-offering-get-started.md#hybrid-runbook-worker).  
    Datorer som kör hello Hybrid Runbook Worker måste uppfylla hello maskinvarukrav innan du utser den toohost den här funktionen. Annars kommer att bli överbelastad hello datorn beroende på hello resursutnyttjande andra bakgrundsprocesser och konkurrens på grund av runbooks under körning och orsaka fördröjningar för runbook-jobb eller timeout.
   Bekräfta hello dator toorun hello Hybrid Runbook Worker funktionen uppfyller hello maskinvarukrav.  Om den finns, övervaka CPU och minne användning toodetermine alla korrelation mellan hello prestanda för Hybrid Runbook Worker-processer och Windows.  Om det finns minne eller CPU-belastning, detta kan indikera hello måste tooupgrade eller lägga till fler processorer eller öka minne tooaddress hello resurs flaskhals och lösa hello-fel. Du kan också markera en annan beräkningsresurser som stöder hello minimikrav och skala när arbetsbelastning indikera att öka krävs.
    
3. hello tjänsten Microsoft Monitoring Agent körs inte.  
    Om hello Microsoft Monitoring Agent Windows-tjänsten inte körs, förhindrar detta hello Hybrid Runbook Worker från att kommunicera med Azure Automation.  Kontrollera hello-agenten körs genom att ange följande kommando i PowerShell hello: `get-service healthservice`.  Ange hello följande kommando i PowerShell toostart hello service om hello-tjänsten har stoppats: `start-service healthservice`.  

4. I hello **program och tjänster för Logs\Operations** händelseloggen händelse 4502 och EventMessage som innehåller **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**med hello följande beskrivning: *hello-certifikatet som presenterades av tjänsten hello <wsid>. oms.opinsights.azure.com har inte utfärdats av en certifikatutfärdare som används för Microsoft-tjänster. Kontakta din administratör toosee för nätverket om de kör en proxy som hindrar TLS/SSL-kommunikation. hello artikeln KB3126513 innehåller ytterligare felsökningsinformation för problem med nätverksanslutningen.*
    Detta kan bero på att din proxy- eller brandväggen blockking kommunikation tooMicrosoft Azure.  Kontrollera hello datorn utgående åtkomst too*.azure-automation.net på port 443.

Loggfilerna lagras lokalt på varje worker-hybrid på C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes.  Du kan kontrollera om det finns några varnings- eller felhändelser skrivs toohello **program och tjänster Logs\Microsoft-SMA\Operations** och **program och tjänster för Logs\Operations** händelseloggen som Visar att det finns en anslutning eller andra problem som påverkar onboarding av hello rollen tooAzure Automation eller problem under normal drift.  

## <a name="next-steps"></a>Nästa steg
Granska [köra runbooks på en Hybrid Runbook Worker](automation-hrw-run-runbooks.md) toolearn hur tooconfigure runbooks-tooautomate processer i ditt lokala datacenter eller andra molnmiljö.
