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
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a><span data-ttu-id="8193e-103">Runbooks som körs på en Hybrid Runbook Worker</span><span class="sxs-lookup"><span data-stu-id="8193e-103">Running runbooks on a Hybrid Runbook Worker</span></span> 
<span data-ttu-id="8193e-104">Det finns ingen skillnad i runbooks som körs i Azure Automation och de som körs på en Hybrid Runbook Worker hello struktur.</span><span class="sxs-lookup"><span data-stu-id="8193e-104">There is no difference in hello structure of runbooks that run in Azure Automation and those that run on a Hybrid Runbook Worker.</span></span> <span data-ttu-id="8193e-105">Runbooks som du använder med varje troligen påtagligt skiljer sig dock eftersom runbooks inriktning på en Hybrid Runbook Worker vanligtvis hantera resurser på hello lokala datorn sig själv eller mot resurser i hello lokala miljö där det har distribuerats, när runbooks i Azure Automation kan vanligtvis hantera resurser i hello Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="8193e-105">Runbooks that you use with each most likely differ significantly though since runbooks targeting a Hybrid Runbook Worker typically manage resources on hello local computer itself or against resources in hello local environment where it is deployed, while runbooks in Azure Automation typically manage resources in hello Azure cloud.</span></span>

<span data-ttu-id="8193e-106">Du kan redigera en runbook för Hybrid Runbook Worker i Azure Automation, men om du försöker tootest hello runbooken i Redigeraren för hello kan det vara svårt.</span><span class="sxs-lookup"><span data-stu-id="8193e-106">You can edit a runbook for Hybrid Runbook Worker in Azure Automation, but you may have difficulties if you try tootest hello runbook in hello editor.</span></span>  <span data-ttu-id="8193e-107">hello PowerShell-moduler som har åtkomst till hello lokala resurser kan inte installeras i Azure Automation-miljö i så fall, hello testet misslyckas.</span><span class="sxs-lookup"><span data-stu-id="8193e-107">hello PowerShell modules that access hello local resources may not be installed in your Azure Automation environment in which case, hello test would fail.</span></span>  <span data-ttu-id="8193e-108">Om du installerar hello krävs moduler, och sedan hello runbook ska köras, men den kommer inte att kunna tooaccess lokala resurser för en testning.</span><span class="sxs-lookup"><span data-stu-id="8193e-108">If you do install hello required modules, then hello runbook will run, but it will not be able tooaccess local resources for a complete test.</span></span>

## <a name="starting-a-runbook-on-hybrid-runbook-worker"></a><span data-ttu-id="8193e-109">Starta en runbook på Hybrid Runbook Worker</span><span class="sxs-lookup"><span data-stu-id="8193e-109">Starting a runbook on Hybrid Runbook Worker</span></span>
<span data-ttu-id="8193e-110">[Starta en Runbook i Azure Automation](automation-starting-a-runbook.md) beskrivs olika metoder för att starta en runbook.</span><span class="sxs-lookup"><span data-stu-id="8193e-110">[Starting a Runbook in Azure Automation](automation-starting-a-runbook.md) describes different methods for starting a runbook.</span></span>  <span data-ttu-id="8193e-111">Runbook Worker-hybrid lägger till en **RunOn** alternativet där du kan ange hello namnet på en Hybrid Runbook Worker-gruppen.</span><span class="sxs-lookup"><span data-stu-id="8193e-111">Hybrid Runbook Worker adds a **RunOn** option where you can specify hello name of a Hybrid Runbook Worker Group.</span></span>  <span data-ttu-id="8193e-112">Om en grupp har angetts hämtas hello runbook och körning av av hello arbetare i gruppen.</span><span class="sxs-lookup"><span data-stu-id="8193e-112">If a group is specified, then hello runbook is retrieved and run by of hello workers in that group.</span></span>  <span data-ttu-id="8193e-113">Om det här alternativet inte anges, sedan körs i Azure Automation som vanligt.</span><span class="sxs-lookup"><span data-stu-id="8193e-113">If this option is not specified, then it is run in Azure Automation as normal.</span></span>

<span data-ttu-id="8193e-114">När du startar en runbook i hello Azure-portalen, visas en **körs på** där du kan välja alternativet **Azure** eller **Hybrid Worker**.</span><span class="sxs-lookup"><span data-stu-id="8193e-114">When you start a runbook in hello Azure portal, you are presented with a **Run on** option where you can select **Azure** or **Hybrid Worker**.</span></span>  <span data-ttu-id="8193e-115">Om du väljer **Hybrid Worker**, kan du markera hello grupp en listrutan.</span><span class="sxs-lookup"><span data-stu-id="8193e-115">If you select **Hybrid Worker**, then you can select hello group from a dropdown.</span></span>

<span data-ttu-id="8193e-116">Använd hello **RunOn** parameter.</span><span class="sxs-lookup"><span data-stu-id="8193e-116">Use hello **RunOn** parameter.</span></span>  <span data-ttu-id="8193e-117">Du kan använda hello efter kommandot toostart en runbook med namnet Test-Runbook på en Hybrid Runbook Worker-grupp med namnet MyHybridGroup med Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8193e-117">You can use hello following command toostart a runbook named Test-Runbook on a Hybrid Runbook Worker Group named MyHybridGroup using Windows PowerShell.</span></span>

    Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"

> [!NOTE]
> <span data-ttu-id="8193e-118">Hej **RunOn** parametern har lagts till toohello **Start AzureAutomationRunbook** cmdlet i version 0.9.1 av Microsoft Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8193e-118">hello **RunOn** parameter was added toohello **Start-AzureAutomationRunbook** cmdlet in version 0.9.1 of Microsoft Azure PowerShell.</span></span>  <span data-ttu-id="8193e-119">Du bör [Hämta senaste versionen av hello](https://azure.microsoft.com/downloads/) om du har en tidigare installerad.</span><span class="sxs-lookup"><span data-stu-id="8193e-119">You should [download hello latest version](https://azure.microsoft.com/downloads/) if you have an earlier one installed.</span></span>  <span data-ttu-id="8193e-120">Du behöver bara tooinstall den här versionen på en dator där du startar hello runbook från Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8193e-120">You only need tooinstall this version on a workstation where you are starting hello runbook from Windows PowerShell.</span></span>  <span data-ttu-id="8193e-121">Du behöver inte tooinstall den på hello worker dator om du avser toostart runbooks från datorn.</span><span class="sxs-lookup"><span data-stu-id="8193e-121">You do not need tooinstall it on hello worker computer unless you intend toostart runbooks from that computer.</span></span>  <span data-ttu-id="8193e-122">Du kan för närvarande startar en runbook på en Hybrid Runbook Worker från en annan runbook eftersom detta skulle kräva hello senaste versionen av Azure Powershell toobe installerad i ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="8193e-122">You cannot currently start a runbook on a Hybrid Runbook Worker from another runbook since this would require hello latest version of Azure Powershell toobe installed in your Automation account.</span></span>  <span data-ttu-id="8193e-123">senaste versionen av hello uppdateras automatiskt i Azure Automation och automatiskt flyttas fram toohello arbetare snart.</span><span class="sxs-lookup"><span data-stu-id="8193e-123">hello latest version is automatically updated in Azure Automation and automatically pushed down toohello workers soon.</span></span>
>
>

## <a name="runbook-permissions"></a><span data-ttu-id="8193e-124">Runbook-behörigheter</span><span class="sxs-lookup"><span data-stu-id="8193e-124">Runbook permissions</span></span>
<span data-ttu-id="8193e-125">Runbooks som körs på en Hybrid Runbook Worker kan inte använda hello samma metod som används oftast för runbooks som autentiserar tooAzure resurser, eftersom de har åtkomst till resurser utanför Azure.</span><span class="sxs-lookup"><span data-stu-id="8193e-125">Runbooks running on a Hybrid Runbook Worker cannot use hello same method that is typically used for runbooks authenticating tooAzure resources, since they are accessing resources outside of Azure.</span></span>  <span data-ttu-id="8193e-126">Hej runbook kan antingen ange sin egen autentisering toolocal resurser eller ange ett RunAs-konto tooprovide användarkontext för alla runbooks.</span><span class="sxs-lookup"><span data-stu-id="8193e-126">hello runbook can either provide its own authentication toolocal resources, or you can specify a RunAs account tooprovide a user context for all runbooks.</span></span>

### <a name="runbook-authentication"></a><span data-ttu-id="8193e-127">Runbook-autentisering</span><span class="sxs-lookup"><span data-stu-id="8193e-127">Runbook authentication</span></span>
<span data-ttu-id="8193e-128">Som standard körs runbooks hello gäller hello lokala systemkontot på hello lokala dator så att de måste ange sina egna autentisering tooresources som de ska ha åtkomst.</span><span class="sxs-lookup"><span data-stu-id="8193e-128">By default, runbooks will run in hello context of hello local System account on hello on-premises computer, so they must provide their own authentication tooresources that they will access.</span></span>  

<span data-ttu-id="8193e-129">Du kan använda [autentiseringsuppgifter](http://msdn.microsoft.com/library/dn940015.aspx) och [certifikat](http://msdn.microsoft.com/library/dn940013.aspx) tillgångar i din runbook med cmdlet: ar som gör att du toospecify autentiseringsuppgifter så att du kan autentisera toodifferent resurser.</span><span class="sxs-lookup"><span data-stu-id="8193e-129">You can use [Credential](http://msdn.microsoft.com/library/dn940015.aspx) and [Certificate](http://msdn.microsoft.com/library/dn940013.aspx) assets in your runbook with cmdlets that allow you toospecify credentials so you can authenticate toodifferent resources.</span></span>  <span data-ttu-id="8193e-130">hello visas följande exempel en del av en runbook som startar om en dator.</span><span class="sxs-lookup"><span data-stu-id="8193e-130">hello following example shows a portion of a runbook that restarts a computer.</span></span>  <span data-ttu-id="8193e-131">Den hämtar autentiseringsuppgifter från ett tillgången och hello namn på autentiseringsuppgift för hello dator från en variabel tillgång och använder dessa värden med hello Restart-Computer cmdlet.</span><span class="sxs-lookup"><span data-stu-id="8193e-131">It retrieves credentials from a credential asset and hello name of hello computer from a variable asset and then uses these values with hello Restart-Computer cmdlet.</span></span>

    $Cred = Get-AzureRmAutomationCredential -ResourceGroupName "ResourceGroup01" -Name "MyCredential"
    $Computer = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" -Name  "ComputerName"

    Restart-Computer -ComputerName $Computer -Credential $Cred

<span data-ttu-id="8193e-132">Du kan också använda [InlineScript](automation-powershell-workflow.md#inlinescript), vilket gör att du toorun kodblock på en annan dator med autentiseringsuppgifter som anges av hello [PSCredential gemensamma parametern](http://technet.microsoft.com/library/jj129719.aspx).</span><span class="sxs-lookup"><span data-stu-id="8193e-132">You can also leverage [InlineScript](automation-powershell-workflow.md#inlinescript), which  allows you toorun blocks of code on another computer with credentials specified by hello [PSCredential common parameter](http://technet.microsoft.com/library/jj129719.aspx).</span></span>

### <a name="runas-account"></a><span data-ttu-id="8193e-133">RunAs-konto</span><span class="sxs-lookup"><span data-stu-id="8193e-133">RunAs account</span></span>
<span data-ttu-id="8193e-134">I stället för att autentisera sina egna resurser toolocal runbooks, kan du ange en **RunAs** för Hybrid worker-gruppen.</span><span class="sxs-lookup"><span data-stu-id="8193e-134">Instead of having runbooks provide their own authentication toolocal resources, you can specify a **RunAs** account for a Hybrid worker group.</span></span>  <span data-ttu-id="8193e-135">Du anger en [autentiseringsuppgiftstillgång](automation-credentials.md) som har åtkomst till toolocal resurser och alla runbooks som körs under autentiseringsuppgifterna vid körning på en Hybrid Runbook Worker i hello-gruppen.</span><span class="sxs-lookup"><span data-stu-id="8193e-135">You specify a [credential asset](automation-credentials.md) that has access toolocal resources, and all runbooks run under these credentials when running on a Hybrid Runbook Worker in hello group.</span></span>  

<span data-ttu-id="8193e-136">hello användarnamn för hello autentiseringsuppgifter måste vara i något av följande format hello:</span><span class="sxs-lookup"><span data-stu-id="8193e-136">hello user name for hello credential must be in one of hello following formats:</span></span>

* <span data-ttu-id="8193e-137">domän\användarnamn</span><span class="sxs-lookup"><span data-stu-id="8193e-137">domain\username</span></span>
* username@domain
* <span data-ttu-id="8193e-138">användarnamn (för konton lokala toohello lokal dator)</span><span class="sxs-lookup"><span data-stu-id="8193e-138">username (for accounts local toohello on-premises computer)</span></span>

<span data-ttu-id="8193e-139">Använd hello följa proceduren toospecify RunAs-konto för en Hybrid worker-gruppen:</span><span class="sxs-lookup"><span data-stu-id="8193e-139">Use hello following procedure toospecify a RunAs account for a Hybrid worker group:</span></span>

1. <span data-ttu-id="8193e-140">Skapa en [autentiseringsuppgiftstillgång](automation-credentials.md) med åtkomst toolocal resurser.</span><span class="sxs-lookup"><span data-stu-id="8193e-140">Create a [credential asset](automation-credentials.md) with access toolocal resources.</span></span>
2. <span data-ttu-id="8193e-141">Öppna hello Automation-konto i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8193e-141">Open hello Automation account in hello Azure portal.</span></span>
3. <span data-ttu-id="8193e-142">Välj hello **Hybrid Worker grupper** panelen och välj sedan hello-gruppen.</span><span class="sxs-lookup"><span data-stu-id="8193e-142">Select hello **Hybrid Worker Groups** tile, and then select hello group.</span></span>
4. <span data-ttu-id="8193e-143">Välj **alla inställningar** och sedan **Hybrid worker gruppinställningar**.</span><span class="sxs-lookup"><span data-stu-id="8193e-143">Select **All settings** and then **Hybrid worker group settings**.</span></span>
5. <span data-ttu-id="8193e-144">Ändra **kör som** från **standard** för**anpassad**.</span><span class="sxs-lookup"><span data-stu-id="8193e-144">Change **Run As** from **Default** too**Custom**.</span></span>
6. <span data-ttu-id="8193e-145">Välj hello autentiseringsuppgifter och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="8193e-145">Select hello credential and click **Save**.</span></span>

### <a name="automation-run-as-account"></a><span data-ttu-id="8193e-146">Automation-kör som-konto</span><span class="sxs-lookup"><span data-stu-id="8193e-146">Automation Run As account</span></span>
<span data-ttu-id="8193e-147">Som en del av din automatiserade build-processen för att distribuera resurser i Azure, kan du behöva åtkomst tooon lokalt system toosupport en aktivitet eller en uppsättning steg i sekvensen distribution.</span><span class="sxs-lookup"><span data-stu-id="8193e-147">As part of your automated build process for deploying resources in Azure, you may require access tooon-premise systems toosupport a task or set of steps in your deployment sequence.</span></span>  <span data-ttu-id="8193e-148">toosupport autentisering mot Azure med hjälp av hello-kör som-konto, behöver du tooinstall hello kör som-konto certifikat.</span><span class="sxs-lookup"><span data-stu-id="8193e-148">toosupport authentication against Azure using hello Run As account, you need tooinstall hello Run As account certificate.</span></span>  

<span data-ttu-id="8193e-149">Hej följande PowerShell-runbook *Export RunAsCertificateToHybridWorker*, exporterar hello-kör som-certifikat från Azure Automation-konto och laddar ned och importerar den till hello lokala datorns certifikatarkiv på en Worker-hybrid anslutna toohello samma konto.</span><span class="sxs-lookup"><span data-stu-id="8193e-149">hello following PowerShell runbook, *Export-RunAsCertificateToHybridWorker*, exports hello Run As certificate from your Azure Automation account and downloads and imports it into hello local machine certificate store on a Hybrid worker connected toohello same account.</span></span>  <span data-ttu-id="8193e-150">När steget har slutförts verifierar hello worker kan autentisera tooAzure med hello-kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="8193e-150">Once that step is completed, it verifies hello worker can successfully authenticate tooAzure using hello Run As account.</span></span>

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

<span data-ttu-id="8193e-151">Spara hello *Export RunAsCertificateToHybridWorker* runbook tooyour dator med en `.ps1` tillägg.</span><span class="sxs-lookup"><span data-stu-id="8193e-151">Save hello *Export-RunAsCertificateToHybridWorker* runbook tooyour computer with a `.ps1` extension.</span></span>  <span data-ttu-id="8193e-152">Importera den till ditt Automation-konto och redigera hello runbook, ändra hello-värdet för variabeln hello `$Password` med ditt eget lösenord.</span><span class="sxs-lookup"><span data-stu-id="8193e-152">Import it into your Automation account and edit hello runbook, changing hello value of hello variable `$Password` with your own password.</span></span>  <span data-ttu-id="8193e-153">Publicera och kör sedan hello runbook riktad hello Hybrid Worker-grupp som körs och autentisera runbooks med hello-kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="8193e-153">Publish and then run hello runbook targeting hello Hybrid Worker group that run and authenticate runbooks using hello Run As account.</span></span>  <span data-ttu-id="8193e-154">hello jobbet dataströmmen rapporter hello försök tooimport hello certifikatet till hello lokala datorns Arkiv och sätt med flera rader beroende på hur många Automation-konton har definierats i din prenumeration och om autentiseringen har lyckats.</span><span class="sxs-lookup"><span data-stu-id="8193e-154">hello job stream reports hello attempt tooimport hello certificate into hello local machine store, and follows with multiple lines depending on how many Automation accounts are defined in your subscription and if authentication is successful.</span></span>  

## <a name="troubleshooting-runbooks-on-hybrid-runbook-worker"></a><span data-ttu-id="8193e-155">Felsökning av runbooks på Hybrid Runbook Worker</span><span class="sxs-lookup"><span data-stu-id="8193e-155">Troubleshooting runbooks on Hybrid Runbook Worker</span></span>
<span data-ttu-id="8193e-156">Loggfilerna lagras lokalt på varje worker-hybrid på C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes.</span><span class="sxs-lookup"><span data-stu-id="8193e-156">Logs are stored locally on each hybrid worker at C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes.</span></span>  <span data-ttu-id="8193e-157">Worker-hybrid också registrerar fel och händelser i händelseloggen för hello under **program och tjänster Logs\Microsoft-SMA\Operational**.</span><span class="sxs-lookup"><span data-stu-id="8193e-157">Hybrid worker also records errors and events in hello Windows event log under **Application and Services Logs\Microsoft-SMA\Operational**.</span></span>  <span data-ttu-id="8193e-158">Händelser relaterade toorunbooks utfördes hello worker skrivs för**program och tjänster Logs\Microsoft-Automation\Operational**.</span><span class="sxs-lookup"><span data-stu-id="8193e-158">Events related toorunbooks executed on hello worker are written too**Application and Services Logs\Microsoft-Automation\Operational**.</span></span>  <span data-ttu-id="8193e-159">Hej **Microsoft SMA** loggen innehåller många fler händelser relaterade toohello runbook-jobbet intryckt toohello worker och hello behandling av hello runbook.</span><span class="sxs-lookup"><span data-stu-id="8193e-159">hello **Microsoft-SMA** log includes many more events related toohello runbook job pushed toohello worker and hello processing of hello runbook.</span></span>  <span data-ttu-id="8193e-160">När hello **Microsoft Automation** händelseloggen inte har många händelser med information som hjälper hello felsökning av runbook-körningen, hittar du minst hello resultatet av hello runbook-jobbet.</span><span class="sxs-lookup"><span data-stu-id="8193e-160">While hello **Microsoft-Automation** event log does not have many events with details assisting with hello troubleshooting of runbook execution, you will at least find hello results of hello runbook job.</span></span>  

<span data-ttu-id="8193e-161">[Runbook-utdata och meddelanden](automation-runbook-output-and-messages.md) tooAzure Automation från hybrider bara som runbook-jobb som körs i molnet hello skickas.</span><span class="sxs-lookup"><span data-stu-id="8193e-161">[Runbook output and messages](automation-runbook-output-and-messages.md) are sent tooAzure Automation from hybrid workers just like runbook jobs run in hello cloud.</span></span>  <span data-ttu-id="8193e-162">Du kan också aktivera hello utförlig och förlopp dataströmmar hello samma sätt som för andra runbooks.</span><span class="sxs-lookup"><span data-stu-id="8193e-162">You can also enable hello Verbose and Progress streams hello same way you would for other runbooks.</span></span>  

<span data-ttu-id="8193e-163">Om dina runbooks inte har slutförts korrekt och hello jobbet sammanfattning visar statusen **pausad**, granska hello artikeln Felsöka [Runbook Worker-Hybrid: runbook-jobbet avslutas med status Avbruten](automation-troubleshooting-hybrid-runbook-worker.md#a-runbook-job-terminates-with-a-status-of-suspended).</span><span class="sxs-lookup"><span data-stu-id="8193e-163">If your runbooks are not completing successfully and hello job summary shows a status of **Suspended**, please review hello troubleshooting article [Hybrid Runbook Worker: A runbook job terminates with a status of Suspended](automation-troubleshooting-hybrid-runbook-worker.md#a-runbook-job-terminates-with-a-status-of-suspended).</span></span>   

## <a name="next-steps"></a><span data-ttu-id="8193e-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8193e-164">Next steps</span></span>
* <span data-ttu-id="8193e-165">toolearn mer om hello olika metoder som kan använda toostart en runbook finns [starta en Runbook i Azure Automation](automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="8193e-165">toolearn more about hello different methods that can be used toostart a runbook, see [Starting a Runbook in Azure Automation](automation-starting-a-runbook.md).</span></span>  
* <span data-ttu-id="8193e-166">toounderstand hello olika procedurer för att arbeta med PowerShell och PowerShell-arbetsflöde runbooks i Azure Automation med hello textrepresentation editor finns [redigera en Runbook i Azure Automation](automation-edit-textual-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="8193e-166">toounderstand hello different procedures for working with PowerShell and PowerShell Workflow runbooks in Azure Automation using hello textual editor, see [Editing a Runbook in Azure Automation](automation-edit-textual-runbook.md)</span></span>
