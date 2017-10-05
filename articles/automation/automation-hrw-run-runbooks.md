---
title: "Köra runbooks på Azure Automation Hybrid Runbook Worker | Microsoft Docs"
description: "Den här artikeln innehåller information om runbooks som körs på datorer i ditt lokala datacenter eller molnprovider med Hybrid Runbook Worker-rollen."
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
ms.openlocfilehash: 993bc3ea480a329541ca4ae825189cdb5a2b4a8b
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a><span data-ttu-id="15383-103">Runbooks som körs på en Hybrid Runbook Worker</span><span class="sxs-lookup"><span data-stu-id="15383-103">Running runbooks on a Hybrid Runbook Worker</span></span> 
<span data-ttu-id="15383-104">Det finns ingen skillnad i runbooks som körs i Azure Automation och de som körs på en Hybrid Runbook Worker-strukturen.</span><span class="sxs-lookup"><span data-stu-id="15383-104">There is no difference in the structure of runbooks that run in Azure Automation and those that run on a Hybrid Runbook Worker.</span></span> <span data-ttu-id="15383-105">Runbooks som du använder med varje troligen påtagligt skiljer sig dock eftersom runbooks inriktning på en Hybrid Runbook Worker vanligtvis hantera resurser på den lokala datorn sig själv eller mot resurser i den lokala miljön där den distribueras, medan runbooks i Azure Automation kan vanligtvis hantera resurser i Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="15383-105">Runbooks that you use with each most likely differ significantly though since runbooks targeting a Hybrid Runbook Worker typically manage resources on the local computer itself or against resources in the local environment where it is deployed, while runbooks in Azure Automation typically manage resources in the Azure cloud.</span></span>

<span data-ttu-id="15383-106">Du kan redigera en runbook för Hybrid Runbook Worker i Azure Automation, men du kan det vara svårt att testa runbook i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="15383-106">You can edit a runbook for Hybrid Runbook Worker in Azure Automation, but you may have difficulties if you try to test the runbook in the editor.</span></span>  <span data-ttu-id="15383-107">PowerShell-moduler som har åtkomst till lokala resurser kan inte installeras i Azure Automation-miljö i vilket fall testet misslyckas.</span><span class="sxs-lookup"><span data-stu-id="15383-107">The PowerShell modules that access the local resources may not be installed in your Azure Automation environment in which case, the test would fail.</span></span>  <span data-ttu-id="15383-108">Om du installerar modulerna som krävs, körs runbook, men det kommer inte att få åtkomst till lokala resurser för en testning.</span><span class="sxs-lookup"><span data-stu-id="15383-108">If you do install the required modules, then the runbook will run, but it will not be able to access local resources for a complete test.</span></span>

## <a name="starting-a-runbook-on-hybrid-runbook-worker"></a><span data-ttu-id="15383-109">Starta en runbook på Hybrid Runbook Worker</span><span class="sxs-lookup"><span data-stu-id="15383-109">Starting a runbook on Hybrid Runbook Worker</span></span>
<span data-ttu-id="15383-110">[Starta en Runbook i Azure Automation](automation-starting-a-runbook.md) beskrivs olika metoder för att starta en runbook.</span><span class="sxs-lookup"><span data-stu-id="15383-110">[Starting a Runbook in Azure Automation](automation-starting-a-runbook.md) describes different methods for starting a runbook.</span></span>  <span data-ttu-id="15383-111">Runbook Worker-hybrid lägger till en **RunOn** alternativet där du kan ange namnet på en Hybrid Runbook Worker-gruppen.</span><span class="sxs-lookup"><span data-stu-id="15383-111">Hybrid Runbook Worker adds a **RunOn** option where you can specify the name of a Hybrid Runbook Worker Group.</span></span>  <span data-ttu-id="15383-112">Om en grupp anges hämtas runbook och kör av av de anställda i gruppen.</span><span class="sxs-lookup"><span data-stu-id="15383-112">If a group is specified, then the runbook is retrieved and run by of the workers in that group.</span></span>  <span data-ttu-id="15383-113">Om det här alternativet inte anges, sedan körs i Azure Automation som vanligt.</span><span class="sxs-lookup"><span data-stu-id="15383-113">If this option is not specified, then it is run in Azure Automation as normal.</span></span>

<span data-ttu-id="15383-114">När du startar en runbook i Azure portal visas med en **körs på** där du kan välja alternativet **Azure** eller **Hybrid Worker**.</span><span class="sxs-lookup"><span data-stu-id="15383-114">When you start a runbook in the Azure portal, you are presented with a **Run on** option where you can select **Azure** or **Hybrid Worker**.</span></span>  <span data-ttu-id="15383-115">Om du väljer **Hybrid Worker**, kan du markera gruppen en listrutan.</span><span class="sxs-lookup"><span data-stu-id="15383-115">If you select **Hybrid Worker**, then you can select the group from a dropdown.</span></span>

<span data-ttu-id="15383-116">Använd den **RunOn** parameter.</span><span class="sxs-lookup"><span data-stu-id="15383-116">Use the **RunOn** parameter.</span></span>  <span data-ttu-id="15383-117">Du kan använda följande kommando för att starta en runbook med namnet Test-Runbook på en Hybrid Runbook Worker-grupp med namnet MyHybridGroup med Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="15383-117">You can use the following command to start a runbook named Test-Runbook on a Hybrid Runbook Worker Group named MyHybridGroup using Windows PowerShell.</span></span>

    Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"

> [!NOTE]
> <span data-ttu-id="15383-118">Den **RunOn** parametern har lagts till i den **Start AzureAutomationRunbook** cmdlet i version 0.9.1 av Microsoft Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="15383-118">The **RunOn** parameter was added to the **Start-AzureAutomationRunbook** cmdlet in version 0.9.1 of Microsoft Azure PowerShell.</span></span>  <span data-ttu-id="15383-119">Du bör [hämta den senaste versionen](https://azure.microsoft.com/downloads/) om du har en tidigare installerad.</span><span class="sxs-lookup"><span data-stu-id="15383-119">You should [download the latest version](https://azure.microsoft.com/downloads/) if you have an earlier one installed.</span></span>  <span data-ttu-id="15383-120">Du behöver bara installera den här versionen på en arbetsstation där du startar runbook från Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="15383-120">You only need to install this version on a workstation where you are starting the runbook from Windows PowerShell.</span></span>  <span data-ttu-id="15383-121">Du behöver inte installera den på worker-datorn om du inte tänker starta runbooks från datorn.</span><span class="sxs-lookup"><span data-stu-id="15383-121">You do not need to install it on the worker computer unless you intend to start runbooks from that computer.</span></span>  <span data-ttu-id="15383-122">Du kan för närvarande startar en runbook på en Hybrid Runbook Worker från en annan runbook eftersom detta kräver den senaste versionen av Azure Powershell installeras i ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="15383-122">You cannot currently start a runbook on a Hybrid Runbook Worker from another runbook since this would require the latest version of Azure Powershell to be installed in your Automation account.</span></span>  <span data-ttu-id="15383-123">Den senaste versionen uppdateras automatiskt i Azure Automation och automatiskt flyttas fram till arbetare snart.</span><span class="sxs-lookup"><span data-stu-id="15383-123">The latest version is automatically updated in Azure Automation and automatically pushed down to the workers soon.</span></span>
>
>

## <a name="runbook-permissions"></a><span data-ttu-id="15383-124">Runbook-behörigheter</span><span class="sxs-lookup"><span data-stu-id="15383-124">Runbook permissions</span></span>
<span data-ttu-id="15383-125">Runbooks som körs på en Hybrid Runbook Worker kan inte använda samma metod som används vanligtvis för runbooks som autentiserar till Azure-resurser, eftersom de har åtkomst till resurser utanför Azure.</span><span class="sxs-lookup"><span data-stu-id="15383-125">Runbooks running on a Hybrid Runbook Worker cannot use the same method that is typically used for runbooks authenticating to Azure resources, since they are accessing resources outside of Azure.</span></span>  <span data-ttu-id="15383-126">Runbook kan antingen ange sin egen autentisering till lokala resurser eller ange ett RunAs-konto för att ge en användarkontext för alla runbooks.</span><span class="sxs-lookup"><span data-stu-id="15383-126">The runbook can either provide its own authentication to local resources, or you can specify a RunAs account to provide a user context for all runbooks.</span></span>

### <a name="runbook-authentication"></a><span data-ttu-id="15383-127">Runbook-autentisering</span><span class="sxs-lookup"><span data-stu-id="15383-127">Runbook authentication</span></span>
<span data-ttu-id="15383-128">Som standard körs runbooks i kontexten för det lokala systemkontot på den lokala datorn, så de måste ange sina egna autentisering till resurser som de kommer åt.</span><span class="sxs-lookup"><span data-stu-id="15383-128">By default, runbooks will run in the context of the local System account on the on-premises computer, so they must provide their own authentication to resources that they will access.</span></span>  

<span data-ttu-id="15383-129">Du kan använda [autentiseringsuppgifter](http://msdn.microsoft.com/library/dn940015.aspx) och [certifikat](http://msdn.microsoft.com/library/dn940013.aspx) tillgångar i din runbook med cmdlets som låter dig ange autentiseringsuppgifter så att du kan autentisera till olika resurser.</span><span class="sxs-lookup"><span data-stu-id="15383-129">You can use [Credential](http://msdn.microsoft.com/library/dn940015.aspx) and [Certificate](http://msdn.microsoft.com/library/dn940013.aspx) assets in your runbook with cmdlets that allow you to specify credentials so you can authenticate to different resources.</span></span>  <span data-ttu-id="15383-130">I följande exempel visas en del av en runbook som startar om en dator.</span><span class="sxs-lookup"><span data-stu-id="15383-130">The following example shows a portion of a runbook that restarts a computer.</span></span>  <span data-ttu-id="15383-131">Den hämtar autentiseringsuppgifter från en autentiseringstillgång och namnet på datorn från en variabel tillgång och använder dessa värden med cmdleten Restart-Computer.</span><span class="sxs-lookup"><span data-stu-id="15383-131">It retrieves credentials from a credential asset and the name of the computer from a variable asset and then uses these values with the Restart-Computer cmdlet.</span></span>

    $Cred = Get-AzureRmAutomationCredential -ResourceGroupName "ResourceGroup01" -Name "MyCredential"
    $Computer = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" -Name  "ComputerName"

    Restart-Computer -ComputerName $Computer -Credential $Cred

<span data-ttu-id="15383-132">Du kan också använda [InlineScript](automation-powershell-workflow.md#inlinescript), där du kan köra kodblock på en annan dator med autentiseringsuppgifter som anges av den [PSCredential gemensamma parametern](http://technet.microsoft.com/library/jj129719.aspx).</span><span class="sxs-lookup"><span data-stu-id="15383-132">You can also leverage [InlineScript](automation-powershell-workflow.md#inlinescript), which  allows you to run blocks of code on another computer with credentials specified by the [PSCredential common parameter](http://technet.microsoft.com/library/jj129719.aspx).</span></span>

### <a name="runas-account"></a><span data-ttu-id="15383-133">RunAs-konto</span><span class="sxs-lookup"><span data-stu-id="15383-133">RunAs account</span></span>
<span data-ttu-id="15383-134">I stället för med runbooks sin egen autentisering till lokala resurser, kan du ange en **RunAs** för Hybrid worker-gruppen.</span><span class="sxs-lookup"><span data-stu-id="15383-134">Instead of having runbooks provide their own authentication to local resources, you can specify a **RunAs** account for a Hybrid worker group.</span></span>  <span data-ttu-id="15383-135">Du anger en [autentiseringsuppgiftstillgång](automation-credentials.md) som har åtkomst till lokala resurser och alla runbooks som körs under autentiseringsuppgifterna vid körning på en Hybrid Runbook Worker i gruppen.</span><span class="sxs-lookup"><span data-stu-id="15383-135">You specify a [credential asset](automation-credentials.md) that has access to local resources, and all runbooks run under these credentials when running on a Hybrid Runbook Worker in the group.</span></span>  

<span data-ttu-id="15383-136">Användarnamn för autentiseringsuppgifter måste vara i något av följande format:</span><span class="sxs-lookup"><span data-stu-id="15383-136">The user name for the credential must be in one of the following formats:</span></span>

* <span data-ttu-id="15383-137">domän\användarnamn</span><span class="sxs-lookup"><span data-stu-id="15383-137">domain\username</span></span>
* username@domain
* <span data-ttu-id="15383-138">användarnamn (för konton som är lokala för lokal dator)</span><span class="sxs-lookup"><span data-stu-id="15383-138">username (for accounts local to the on-premises computer)</span></span>

<span data-ttu-id="15383-139">Använd följande procedur för att ange ett RunAs-konto för en Hybrid worker-gruppen:</span><span class="sxs-lookup"><span data-stu-id="15383-139">Use the following procedure to specify a RunAs account for a Hybrid worker group:</span></span>

1. <span data-ttu-id="15383-140">Skapa en [autentiseringsuppgiftstillgång](automation-credentials.md) åtkomst till lokala resurser.</span><span class="sxs-lookup"><span data-stu-id="15383-140">Create a [credential asset](automation-credentials.md) with access to local resources.</span></span>
2. <span data-ttu-id="15383-141">Öppna Automation-kontot i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="15383-141">Open the Automation account in the Azure portal.</span></span>
3. <span data-ttu-id="15383-142">Välj den **Hybrid Worker grupper** panelen och välj sedan gruppen.</span><span class="sxs-lookup"><span data-stu-id="15383-142">Select the **Hybrid Worker Groups** tile, and then select the group.</span></span>
4. <span data-ttu-id="15383-143">Välj **alla inställningar** och sedan **Hybrid worker gruppinställningar**.</span><span class="sxs-lookup"><span data-stu-id="15383-143">Select **All settings** and then **Hybrid worker group settings**.</span></span>
5. <span data-ttu-id="15383-144">Ändra **kör som-** från **standard** till **anpassad**.</span><span class="sxs-lookup"><span data-stu-id="15383-144">Change **Run As** from **Default** to **Custom**.</span></span>
6. <span data-ttu-id="15383-145">Välj autentiseringsuppgifter och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="15383-145">Select the credential and click **Save**.</span></span>

### <a name="automation-run-as-account"></a><span data-ttu-id="15383-146">Automation-kör som-konto</span><span class="sxs-lookup"><span data-stu-id="15383-146">Automation Run As account</span></span>
<span data-ttu-id="15383-147">Du kan behöva åtkomst till lokala system för att stödja en aktivitet eller en uppsättning steg i sekvensen distribution som del av din automatiserade build-processen för att distribuera resurser i Azure.</span><span class="sxs-lookup"><span data-stu-id="15383-147">As part of your automated build process for deploying resources in Azure, you may require access to on-premise systems to support a task or set of steps in your deployment sequence.</span></span>  <span data-ttu-id="15383-148">Du måste installera kör som-konto-certifikat för att stödja autentisering mot Azure med hjälp av kör som-kontot.</span><span class="sxs-lookup"><span data-stu-id="15383-148">To support authentication against Azure using the Run As account, you need to install the Run As account certificate.</span></span>  

<span data-ttu-id="15383-149">Följande PowerShell-runbook *Export RunAsCertificateToHybridWorker*, exporterar kör som-certifikatet från Azure Automation-konto och laddar ned och importerar den till lokala datorns certifikatarkiv på en Hybrid Worker anslutna till samma konto.</span><span class="sxs-lookup"><span data-stu-id="15383-149">The following PowerShell runbook, *Export-RunAsCertificateToHybridWorker*, exports the Run As certificate from your Azure Automation account and downloads and imports it into the local machine certificate store on a Hybrid worker connected to the same account.</span></span>  <span data-ttu-id="15383-150">När steget har slutförts verifierar arbetaren kan autentisera till Azure med Kör som-kontot.</span><span class="sxs-lookup"><span data-stu-id="15383-150">Once that step is completed, it verifies the worker can successfully authenticate to Azure using the Run As account.</span></span>

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
    Exports the Run As certificate from an Azure Automation account to a hybrid worker in that account. 
  
    .DESCRIPTION  
    This runbook exports the Run As certificate from an Azure Automation account to a hybrid worker in that account.
    Run this runbook in the hybrid worker where you want the certificate installed.
    This allows the use of the AzureRunAsConnection to authenticate to Azure and manage Azure resources from runbooks running in the hybrid worker.

    .EXAMPLE
    .\Export-RunAsCertificateToHybridWorker

    .NOTES
    AUTHOR: Azure Automation Team 
    LASTEDIT: 2016.10.13
    #>

    [OutputType([string])] 

    # Set the password used for this certificate
    $Password = "YourStrongPasswordForTheCert"

    # Stop on errors
    $ErrorActionPreference = 'stop'

    # Get the management certificate that will be used to make calls into Azure Service Management resources
    $RunAsCert = Get-AutomationCertificate -Name "AzureRunAsCertificate"
       
    # location to store temporary certificate in the Automation service host
    $CertPath = Join-Path $env:temp  "AzureRunAsCertificate.pfx"
   
    # Save the certificate
    $Cert = $RunAsCert.Export("pfx",$Password)
    Set-Content -Value $Cert -Path $CertPath -Force -Encoding Byte | Write-Verbose 

    Write-Output ("Importing certificate into $env:computername local machine root store from " + $CertPath)
    $SecurePassword = ConvertTo-SecureString $Password -AsPlainText -Force
    Import-PfxCertificate -FilePath $CertPath -CertStoreLocation Cert:\LocalMachine\My -Password $SecurePassword -Exportable | Write-Verbose

    # Test that authentication to Azure Resource Manager is working
    $RunAsConnection = Get-AutomationConnection -Name "AzureRunAsConnection" 
    
    Add-AzureRmAccount `
      -ServicePrincipal `
      -TenantId $RunAsConnection.TenantId `
      -ApplicationId $RunAsConnection.ApplicationId `
      -CertificateThumbprint $RunAsConnection.CertificateThumbprint | Write-Verbose

    Set-AzureRmContext -SubscriptionId $RunAsConnection.SubscriptionID | Write-Verbose

    # List automation accounts to confirm Azure Resource Manager calls are working
    Get-AzureRmAutomationAccount | Select AutomationAccountName

<span data-ttu-id="15383-151">Spara den *Export RunAsCertificateToHybridWorker* runbook på datorn med en `.ps1` tillägg.</span><span class="sxs-lookup"><span data-stu-id="15383-151">Save the *Export-RunAsCertificateToHybridWorker* runbook to your computer with a `.ps1` extension.</span></span>  <span data-ttu-id="15383-152">Importera den till ditt Automation-konto och redigera runbook, ändra värdet för variabeln `$Password` med ditt eget lösenord.</span><span class="sxs-lookup"><span data-stu-id="15383-152">Import it into your Automation account and edit the runbook, changing the value of the variable `$Password` with your own password.</span></span>  <span data-ttu-id="15383-153">Publicera och sedan köra runbook riktad Hybrid Worker-grupp som körs och autentisera runbooks med Kör som-kontot.</span><span class="sxs-lookup"><span data-stu-id="15383-153">Publish and then run the runbook targeting the Hybrid Worker group that run and authenticate runbooks using the Run As account.</span></span>  <span data-ttu-id="15383-154">Jobbet dataströmmen rapporterar försök att importera certifikatet till lokala datorns Arkiv och följer med flera rader beroende på hur många Automation-konton har definierats i din prenumeration och om autentiseringen har lyckats.</span><span class="sxs-lookup"><span data-stu-id="15383-154">The job stream reports the attempt to import the certificate into the local machine store, and follows with multiple lines depending on how many Automation accounts are defined in your subscription and if authentication is successful.</span></span>  

## <a name="troubleshooting-runbooks-on-hybrid-runbook-worker"></a><span data-ttu-id="15383-155">Felsökning av runbooks på Hybrid Runbook Worker</span><span class="sxs-lookup"><span data-stu-id="15383-155">Troubleshooting runbooks on Hybrid Runbook Worker</span></span>
<span data-ttu-id="15383-156">Loggfilerna lagras lokalt på varje worker-hybrid på C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes.</span><span class="sxs-lookup"><span data-stu-id="15383-156">Logs are stored locally on each hybrid worker at C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes.</span></span>  <span data-ttu-id="15383-157">Worker-hybrid också registrerar fel och händelser i händelseloggen i Windows under **program och tjänster Logs\Microsoft-SMA\Operational**.</span><span class="sxs-lookup"><span data-stu-id="15383-157">Hybrid worker also records errors and events in the Windows event log under **Application and Services Logs\Microsoft-SMA\Operational**.</span></span>  <span data-ttu-id="15383-158">Runbooks som körs på worker-relaterade händelser skrivs till **program och tjänster Logs\Microsoft-Automation\Operational**.</span><span class="sxs-lookup"><span data-stu-id="15383-158">Events related to runbooks executed on the worker are written to **Application and Services Logs\Microsoft-Automation\Operational**.</span></span>  <span data-ttu-id="15383-159">Den **Microsoft SMA** loggen innehåller många fler händelser relaterade till runbook-jobbet pushas till arbetaren och bearbetning av runbook.</span><span class="sxs-lookup"><span data-stu-id="15383-159">The **Microsoft-SMA** log includes many more events related to the runbook job pushed to the worker and the processing of the runbook.</span></span>  <span data-ttu-id="15383-160">När den **Microsoft Automation** händelseloggen inte har många händelser med information som hjälper med felsökning av runbook-körningen, hittar du minst resultaten av runbook-jobbet.</span><span class="sxs-lookup"><span data-stu-id="15383-160">While the **Microsoft-Automation** event log does not have many events with details assisting with the troubleshooting of runbook execution, you will at least find the results of the runbook job.</span></span>  

<span data-ttu-id="15383-161">[Runbook-utdata och meddelanden om](automation-runbook-output-and-messages.md) skickas till Azure Automation från hybrid Worker-arbeten precis som runbook-jobb körs i molnet.</span><span class="sxs-lookup"><span data-stu-id="15383-161">[Runbook output and messages](automation-runbook-output-and-messages.md) are sent to Azure Automation from hybrid workers just like runbook jobs run in the cloud.</span></span>  <span data-ttu-id="15383-162">Du kan också aktivera utförlig och förlopp strömmar på samma sätt som för andra runbooks.</span><span class="sxs-lookup"><span data-stu-id="15383-162">You can also enable the Verbose and Progress streams the same way you would for other runbooks.</span></span>  

<span data-ttu-id="15383-163">Om dina runbooks inte har slutförts korrekt och jobbet sammanfattning visar statusen **pausad**, granska felsökningsartikel [Hybrid Runbook Worker: runbook-jobbet avslutas med statusen Pausad ](automation-troubleshooting-hybrid-runbook-worker.md#a-runbook-job-terminates-with-a-status-of-suspended).</span><span class="sxs-lookup"><span data-stu-id="15383-163">If your runbooks are not completing successfully and the job summary shows a status of **Suspended**, please review the troubleshooting article [Hybrid Runbook Worker: A runbook job terminates with a status of Suspended](automation-troubleshooting-hybrid-runbook-worker.md#a-runbook-job-terminates-with-a-status-of-suspended).</span></span>   

## <a name="next-steps"></a><span data-ttu-id="15383-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="15383-164">Next steps</span></span>
* <span data-ttu-id="15383-165">Läs mer om olika metoder som kan användas för att starta en runbook i [starta en Runbook i Azure Automation](automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="15383-165">To learn more about the different methods that can be used to start a runbook, see [Starting a Runbook in Azure Automation](automation-starting-a-runbook.md).</span></span>  
* <span data-ttu-id="15383-166">Olika procedurer för att arbeta med PowerShell och PowerShell-arbetsflöde runbooks i Azure Automation med hjälp av textrepresentation editor finns [redigera en Runbook i Azure Automation](automation-edit-textual-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="15383-166">To understand the different procedures for working with PowerShell and PowerShell Workflow runbooks in Azure Automation using the textual editor, see [Editing a Runbook in Azure Automation](automation-edit-textual-runbook.md)</span></span>