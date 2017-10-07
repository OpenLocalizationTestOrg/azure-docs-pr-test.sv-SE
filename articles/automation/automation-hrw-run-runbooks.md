---
title: "aaaRun runbooks på Azure Automation Hybrid Runbook Worker | Microsoft Docs"
description: "Den här artikeln innehåller information om runbooks som körs på datorer i ditt lokala datacenter eller molnprovider med hello Hybrid Runbook Worker-rollen."
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
ms.date: 07/22/2017
ms.author: magoedte
ms.openlocfilehash: 51961e02603e5690edd11e577594ad2ddea489a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a>Runbooks som körs på en Hybrid Runbook Worker 
Det finns ingen skillnad i runbooks som körs i Azure Automation och de som körs på en Hybrid Runbook Worker hello struktur. Runbooks som du använder med varje troligen påtagligt skiljer sig dock eftersom runbooks inriktning på en Hybrid Runbook Worker vanligtvis hantera resurser på hello lokala datorn sig själv eller mot resurser i hello lokala miljö där det har distribuerats, när runbooks i Azure Automation kan vanligtvis hantera resurser i hello Azure-molnet.

Du kan redigera en runbook för Hybrid Runbook Worker i Azure Automation, men om du försöker tootest hello runbooken i Redigeraren för hello kan det vara svårt.  hello PowerShell-moduler som har åtkomst till hello lokala resurser kan inte installeras i Azure Automation-miljö i så fall, hello testet misslyckas.  Om du installerar hello krävs moduler, och sedan hello runbook ska köras, men den kommer inte att kunna tooaccess lokala resurser för en testning.

## <a name="starting-a-runbook-on-hybrid-runbook-worker"></a>Starta en runbook på Hybrid Runbook Worker
[Starta en Runbook i Azure Automation](automation-starting-a-runbook.md) beskrivs olika metoder för att starta en runbook.  Runbook Worker-hybrid lägger till en **RunOn** alternativet där du kan ange hello namnet på en Hybrid Runbook Worker-gruppen.  Om en grupp har angetts hämtas hello runbook och körning av av hello arbetare i gruppen.  Om det här alternativet inte anges, sedan körs i Azure Automation som vanligt.

När du startar en runbook i hello Azure-portalen, visas en **körs på** där du kan välja alternativet **Azure** eller **Hybrid Worker**.  Om du väljer **Hybrid Worker**, kan du markera hello grupp en listrutan.

Använd hello **RunOn** parameter.  Du kan använda hello efter kommandot toostart en runbook med namnet Test-Runbook på en Hybrid Runbook Worker-grupp med namnet MyHybridGroup med Windows PowerShell.

    Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"

> [!NOTE]
> Hej **RunOn** parametern har lagts till toohello **Start AzureAutomationRunbook** cmdlet i version 0.9.1 av Microsoft Azure PowerShell.  Du bör [Hämta senaste versionen av hello](https://azure.microsoft.com/downloads/) om du har en tidigare installerad.  Du behöver bara tooinstall den här versionen på en dator där du startar hello runbook från Windows PowerShell.  Du behöver inte tooinstall den på hello worker dator om du avser toostart runbooks från datorn.  Du kan för närvarande startar en runbook på en Hybrid Runbook Worker från en annan runbook eftersom detta skulle kräva hello senaste versionen av Azure Powershell toobe installerad i ditt Automation-konto.  senaste versionen av hello uppdateras automatiskt i Azure Automation och automatiskt flyttas fram toohello arbetare snart.
>
>

## <a name="runbook-permissions"></a>Runbook-behörigheter
Runbooks som körs på en Hybrid Runbook Worker kan inte använda hello samma metod som används oftast för runbooks som autentiserar tooAzure resurser, eftersom de har åtkomst till resurser utanför Azure.  Hej runbook kan antingen ange sin egen autentisering toolocal resurser eller ange ett RunAs-konto tooprovide användarkontext för alla runbooks.

### <a name="runbook-authentication"></a>Runbook-autentisering
Som standard körs runbooks hello gäller hello lokala systemkontot på hello lokala dator så att de måste ange sina egna autentisering tooresources som de ska ha åtkomst.  

Du kan använda [autentiseringsuppgifter](http://msdn.microsoft.com/library/dn940015.aspx) och [certifikat](http://msdn.microsoft.com/library/dn940013.aspx) tillgångar i din runbook med cmdlet: ar som gör att du toospecify autentiseringsuppgifter så att du kan autentisera toodifferent resurser.  hello visas följande exempel en del av en runbook som startar om en dator.  Den hämtar autentiseringsuppgifter från ett tillgången och hello namn på autentiseringsuppgift för hello dator från en variabel tillgång och använder dessa värden med hello Restart-Computer cmdlet.

    $Cred = Get-AzureRmAutomationCredential -ResourceGroupName "ResourceGroup01" -Name "MyCredential"
    $Computer = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" -Name  "ComputerName"

    Restart-Computer -ComputerName $Computer -Credential $Cred

Du kan också använda [InlineScript](automation-powershell-workflow.md#inlinescript), vilket gör att du toorun kodblock på en annan dator med autentiseringsuppgifter som anges av hello [PSCredential gemensamma parametern](http://technet.microsoft.com/library/jj129719.aspx).

### <a name="runas-account"></a>RunAs-konto
I stället för att autentisera sina egna resurser toolocal runbooks, kan du ange en **RunAs** för Hybrid worker-gruppen.  Du anger en [autentiseringsuppgiftstillgång](automation-credentials.md) som har åtkomst till toolocal resurser och alla runbooks som körs under autentiseringsuppgifterna vid körning på en Hybrid Runbook Worker i hello-gruppen.  

hello användarnamn för hello autentiseringsuppgifter måste vara i något av följande format hello:

* domän\användarnamn
* username@domain
* användarnamn (för konton lokala toohello lokal dator)

Använd hello följa proceduren toospecify RunAs-konto för en Hybrid worker-gruppen:

1. Skapa en [autentiseringsuppgiftstillgång](automation-credentials.md) med åtkomst toolocal resurser.
2. Öppna hello Automation-konto i hello Azure-portalen.
3. Välj hello **Hybrid Worker grupper** panelen och välj sedan hello-gruppen.
4. Välj **alla inställningar** och sedan **Hybrid worker gruppinställningar**.
5. Ändra **kör som** från **standard** för**anpassad**.
6. Välj hello autentiseringsuppgifter och klicka på **spara**.

### <a name="automation-run-as-account"></a>Automation-kör som-konto
Som en del av din automatiserade build-processen för att distribuera resurser i Azure, kan du behöva åtkomst tooon lokalt system toosupport en aktivitet eller en uppsättning steg i sekvensen distribution.  toosupport autentisering mot Azure med hjälp av hello-kör som-konto, behöver du tooinstall hello kör som-konto certifikat.  

Hej följande PowerShell-runbook *Export RunAsCertificateToHybridWorker*, exporterar hello-kör som-certifikat från Azure Automation-konto och laddar ned och importerar den till hello lokala datorns certifikatarkiv på en Worker-hybrid anslutna toohello samma konto.  När steget har slutförts verifierar hello worker kan autentisera tooAzure med hello-kör som-konto.

    <#PSScriptInfo
    .VERSION 1.0
    .GUID 3a796b9a-623d-499d-86c8-c249f10a6986
    .AUTHOR Azure Automation Team
    .COMPANYNAME Microsoft
    .COPYRIGHT 
    .TAGS Azure Automation 
    .LICENSEURI 
    .PROJECTURI 
    .ICONURI 
    .EXTERNALMODULEDEPENDENCIES 
    .REQUIREDSCRIPTS 
    .EXTERNALSCRIPTDEPENDENCIES 
    .RELEASENOTES
    #>

    <#  
    .SYNOPSIS  
    Exports hello Run As certificate from an Azure Automation account tooa hybrid worker in that account. 
  
    .DESCRIPTION  
    This runbook exports hello Run As certificate from an Azure Automation account tooa hybrid worker in that account.
    Run this runbook in hello hybrid worker where you want hello certificate installed.
    This allows hello use of hello AzureRunAsConnection tooauthenticate tooAzure and manage Azure resources from runbooks running in hello hybrid worker.

    .EXAMPLE
    .\Export-RunAsCertificateToHybridWorker

    .NOTES
    AUTHOR: Azure Automation Team 
    LASTEDIT: 2016.10.13
    #>

    [OutputType([string])] 

    # Set hello password used for this certificate
    $Password = "YourStrongPasswordForTheCert"

    # Stop on errors
    $ErrorActionPreference = 'stop'

    # Get hello management certificate that will be used toomake calls into Azure Service Management resources
    $RunAsCert = Get-AutomationCertificate -Name "AzureRunAsCertificate"
       
    # location toostore temporary certificate in hello Automation service host
    $CertPath = Join-Path $env:temp  "AzureRunAsCertificate.pfx"
   
    # Save hello certificate
    $Cert = $RunAsCert.Export("pfx",$Password)
    Set-Content -Value $Cert -Path $CertPath -Force -Encoding Byte | Write-Verbose 

    Write-Output ("Importing certificate into $env:computername local machine root store from " + $CertPath)
    $SecurePassword = ConvertTo-SecureString $Password -AsPlainText -Force
    Import-PfxCertificate -FilePath $CertPath -CertStoreLocation Cert:\LocalMachine\My -Password $SecurePassword -Exportable | Write-Verbose

    # Test that authentication tooAzure Resource Manager is working
    $RunAsConnection = Get-AutomationConnection -Name "AzureRunAsConnection" 
    
    Add-AzureRmAccount `
      -ServicePrincipal `
      -TenantId $RunAsConnection.TenantId `
      -ApplicationId $RunAsConnection.ApplicationId `
      -CertificateThumbprint $RunAsConnection.CertificateThumbprint | Write-Verbose

    Set-AzureRmContext -SubscriptionId $RunAsConnection.SubscriptionID | Write-Verbose

    # List automation accounts tooconfirm Azure Resource Manager calls are working
    Get-AzureRmAutomationAccount | Select AutomationAccountName

Spara hello *Export RunAsCertificateToHybridWorker* runbook tooyour dator med en `.ps1` tillägg.  Importera den till ditt Automation-konto och redigera hello runbook, ändra hello-värdet för variabeln hello `$Password` med ditt eget lösenord.  Publicera och kör sedan hello runbook riktad hello Hybrid Worker-grupp som körs och autentisera runbooks med hello-kör som-konto.  hello jobbet dataströmmen rapporter hello försök tooimport hello certifikatet till hello lokala datorns Arkiv och sätt med flera rader beroende på hur många Automation-konton har definierats i din prenumeration och om autentiseringen har lyckats.  

## <a name="troubleshooting-runbooks-on-hybrid-runbook-worker"></a>Felsökning av runbooks på Hybrid Runbook Worker
Loggfilerna lagras lokalt på varje worker-hybrid på C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes.  Worker-hybrid också registrerar fel och händelser i händelseloggen för hello under **program och tjänster Logs\Microsoft-SMA\Operational**.  Händelser relaterade toorunbooks utfördes hello worker skrivs för**program och tjänster Logs\Microsoft-Automation\Operational**.  Hej **Microsoft SMA** loggen innehåller många fler händelser relaterade toohello runbook-jobbet intryckt toohello worker och hello behandling av hello runbook.  När hello **Microsoft Automation** händelseloggen inte har många händelser med information som hjälper hello felsökning av runbook-körningen, hittar du minst hello resultatet av hello runbook-jobbet.  

[Runbook-utdata och meddelanden](automation-runbook-output-and-messages.md) tooAzure Automation från hybrider bara som runbook-jobb som körs i molnet hello skickas.  Du kan också aktivera hello utförlig och förlopp dataströmmar hello samma sätt som för andra runbooks.  

Om dina runbooks inte har slutförts korrekt och hello jobbet sammanfattning visar statusen **pausad**, granska hello artikeln Felsöka [Runbook Worker-Hybrid: runbook-jobbet avslutas med status Avbruten](automation-troubleshooting-hybrid-runbook-worker.md#a-runbook-job-terminates-with-a-status-of-suspended).   

## <a name="next-steps"></a>Nästa steg
* toolearn mer om hello olika metoder som kan använda toostart en runbook finns [starta en Runbook i Azure Automation](automation-starting-a-runbook.md).  
* toounderstand hello olika procedurer för att arbeta med PowerShell och PowerShell-arbetsflöde runbooks i Azure Automation med hello textrepresentation editor finns [redigera en Runbook i Azure Automation](automation-edit-textual-runbook.md)
