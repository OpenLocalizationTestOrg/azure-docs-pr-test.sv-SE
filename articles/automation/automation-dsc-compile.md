---
title: aaaCompiling konfigurationer i Azure Automation DSC | Microsoft Docs
description: "Den här artikeln beskriver hur toocompile önskade tillstånd Configuration (DSC) konfigurationer för Azure Automation."
services: automation
documentationcenter: na
author: eslesar
manager: carmonm
ms.assetid: 49f20b31-4fa5-4712-b1c7-8f4409f1aecc
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 02/07/2017
ms.author: magoedte; eslesar
ms.openlocfilehash: b195311318a2d7431c4d6b29f4b9a5f3a0a0a9a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="compiling-configurations-in-azure-automation-dsc"></a><span data-ttu-id="097b6-103">Kompilera konfigurationer i Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="097b6-103">Compiling configurations in Azure Automation DSC</span></span>

<span data-ttu-id="097b6-104">Du kan sammanställa önskad tillstånd Configuration (DSC) konfigurationer på två sätt med Azure Automation: hello Azure-portalen och med Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="097b6-104">You can compile Desired State Configuration (DSC) configurations in two ways with Azure Automation: in hello Azure portal, and with Windows PowerShell.</span></span> <span data-ttu-id="097b6-105">hello nedan hjälper dig att avgöra när toouse vilken metod baserat på hello egenskaper för varje:</span><span class="sxs-lookup"><span data-stu-id="097b6-105">hello following table will help you determine when toouse which method based on hello characteristics of each:</span></span>

### <a name="azure-portal"></a><span data-ttu-id="097b6-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="097b6-106">Azure portal</span></span>

* <span data-ttu-id="097b6-107">Enklaste metoden med interaktivt användargränssnitt</span><span class="sxs-lookup"><span data-stu-id="097b6-107">Simplest method with interactive user interface</span></span>
* <span data-ttu-id="097b6-108">Formuläret tooprovide enkel parametervärden</span><span class="sxs-lookup"><span data-stu-id="097b6-108">Form tooprovide simple parameter values</span></span>
* <span data-ttu-id="097b6-109">Spåra jobbets status</span><span class="sxs-lookup"><span data-stu-id="097b6-109">Easily track job state</span></span>
* <span data-ttu-id="097b6-110">Autentisera med Azure inloggning åtkomst</span><span class="sxs-lookup"><span data-stu-id="097b6-110">Access authenticated with Azure logon</span></span>

### <a name="windows-powershell"></a><span data-ttu-id="097b6-111">Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="097b6-111">Windows PowerShell</span></span>

* <span data-ttu-id="097b6-112">Anropa från kommandoraden med Windows PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="097b6-112">Call from command line with Windows PowerShell cmdlets</span></span>
* <span data-ttu-id="097b6-113">Kan ingå i automatisk lösning med flera steg</span><span class="sxs-lookup"><span data-stu-id="097b6-113">Can be included in automated solution with multiple steps</span></span>
* <span data-ttu-id="097b6-114">Ange enkla och komplexa parametervärden</span><span class="sxs-lookup"><span data-stu-id="097b6-114">Provide simple and complex parameter values</span></span>
* <span data-ttu-id="097b6-115">Spåra jobbets status</span><span class="sxs-lookup"><span data-stu-id="097b6-115">Track job state</span></span>
* <span data-ttu-id="097b6-116">Klienten krävs toosupport PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="097b6-116">Client required toosupport PowerShell cmdlets</span></span>
* <span data-ttu-id="097b6-117">Skicka ConfigurationData</span><span class="sxs-lookup"><span data-stu-id="097b6-117">Pass ConfigurationData</span></span>
* <span data-ttu-id="097b6-118">Kompilera konfigurationer som använder autentiseringsuppgifterna</span><span class="sxs-lookup"><span data-stu-id="097b6-118">Compile configurations that use credentials</span></span>

<span data-ttu-id="097b6-119">När du har valt en metod för kompilering, kan du följa hello respektive procedurerna nedan toostart kompilering.</span><span class="sxs-lookup"><span data-stu-id="097b6-119">Once you have decided on a compilation method, you can follow hello respective procedures below toostart compiling.</span></span>

## <a name="compiling-a-dsc-configuration-with-hello-azure-portal"></a><span data-ttu-id="097b6-120">Kompilering av DSC-konfigurationen med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="097b6-120">Compiling a DSC Configuration with hello Azure portal</span></span>

1. <span data-ttu-id="097b6-121">Från ditt Automation-konto klickar du på **DSC-konfigurationer**.</span><span class="sxs-lookup"><span data-stu-id="097b6-121">From your Automation account, click **DSC Configurations**.</span></span>
2. <span data-ttu-id="097b6-122">Klicka på en konfiguration tooopen dess bladet.</span><span class="sxs-lookup"><span data-stu-id="097b6-122">Click a configuration tooopen its blade.</span></span>
3. <span data-ttu-id="097b6-123">Klicka på **Kompilera**.</span><span class="sxs-lookup"><span data-stu-id="097b6-123">Click **Compile**.</span></span>
4. <span data-ttu-id="097b6-124">Om hello konfiguration har inga parametrar, kommer du att ange tooconfirm om du vill toocompile den.</span><span class="sxs-lookup"><span data-stu-id="097b6-124">If hello configuration has no parameters, you will be prompted tooconfirm whether you want toocompile it.</span></span> <span data-ttu-id="097b6-125">Om hello konfiguration har parametrar hello **kompilera konfiguration** öppnas bladet så att du kan ange parametervärden.</span><span class="sxs-lookup"><span data-stu-id="097b6-125">If hello configuration has parameters, hello **Compile Configuration** blade will open so you can provide parameter values.</span></span> <span data-ttu-id="097b6-126">Se hello [ **grundläggande parametrar** ](#basic-parameters) avsnittet nedan för mer information om parametrar.</span><span class="sxs-lookup"><span data-stu-id="097b6-126">See hello [**Basic Parameters**](#basic-parameters) section below for further details on parameters.</span></span>
5. <span data-ttu-id="097b6-127">Hej **Kompileringsjobbet** öppnas bladet så att du kan spåra hello kompilering jobbets status och hello nodkonfigurationer (MOF configuration dokument) det orsakade toobe placerad hello hämtar Azure Automation DSC-Server.</span><span class="sxs-lookup"><span data-stu-id="097b6-127">hello **Compilation Job** blade is opened so that you can track hello compilation job's status, and hello node configurations (MOF configuration documents) it caused toobe placed on hello Azure Automation DSC Pull Server.</span></span>

## <a name="compiling-a-dsc-configuration-with-windows-powershell"></a><span data-ttu-id="097b6-128">Kompilering av DSC-konfigurationen med Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="097b6-128">Compiling a DSC Configuration with Windows PowerShell</span></span>

<span data-ttu-id="097b6-129">Du kan använda [ `Start-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) toostart kompilerades med Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="097b6-129">You can use [`Start-AzureRmAutomationDscCompilationJob`](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) toostart compiling with Windows PowerShell.</span></span> <span data-ttu-id="097b6-130">hello följande exempelkod startar kompilering av en DSC-konfiguration som kallas **SampleConfig**.</span><span class="sxs-lookup"><span data-stu-id="097b6-130">hello following sample code starts compilation of a DSC configuration called **SampleConfig**.</span></span>

```powershell
Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"
```

<span data-ttu-id="097b6-131">`Start-AzureRmAutomationDscCompilationJob`Returnerar en sammanställning jobbet objekt som du kan använda tootrack dess status.</span><span class="sxs-lookup"><span data-stu-id="097b6-131">`Start-AzureRmAutomationDscCompilationJob` returns a compilation job object that you can use tootrack its status.</span></span> <span data-ttu-id="097b6-132">Du kan sedan använda detta jobbobjekt för kompilering med [ `Get-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob) toodetermine hello status för hello kompileringsjobbet och [ `Get-AzureRmAutomationDscCompilationJobOutput` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput) tooview dess dataströmmar (utdata).</span><span class="sxs-lookup"><span data-stu-id="097b6-132">You can then use this compilation job object with [`Get-AzureRmAutomationDscCompilationJob`](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob) toodetermine hello status of hello compilation job, and [`Get-AzureRmAutomationDscCompilationJobOutput`](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput) tooview its streams (output).</span></span> <span data-ttu-id="097b6-133">hello följande exempelkod startar sammanställning av hello **SampleConfig** konfiguration, väntar tills den har slutförts och visar sedan dess dataströmmar.</span><span class="sxs-lookup"><span data-stu-id="097b6-133">hello following sample code starts compilation of hello **SampleConfig** configuration, waits until it has completed, and then displays its streams.</span></span>

```powershell
$CompilationJob = Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"

while($CompilationJob.EndTime –eq $null -and $CompilationJob.Exception –eq $null)
{
    $CompilationJob = $CompilationJob | Get-AzureRmAutomationDscCompilationJob
    Start-Sleep -Seconds 3
}

$CompilationJob | Get-AzureRmAutomationDscCompilationJobOutput –Stream Any
```

## <a name="basic-parameters"></a><span data-ttu-id="097b6-134">Grundläggande parametrar</span><span class="sxs-lookup"><span data-stu-id="097b6-134">Basic Parameters</span></span>
<span data-ttu-id="097b6-135">Parameterdeklaration i DSC-konfigurationer, inklusive parametertyper och egenskaper, fungerar hello samma som i Azure Automation-runbooks.</span><span class="sxs-lookup"><span data-stu-id="097b6-135">Parameter declaration in DSC configurations, including parameter types and properties, works hello same as in Azure Automation runbooks.</span></span> <span data-ttu-id="097b6-136">Se [starta en runbook i Azure Automation](automation-starting-a-runbook.md) toolearn mer om runbook-parametrar.</span><span class="sxs-lookup"><span data-stu-id="097b6-136">See [Starting a runbook in Azure Automation](automation-starting-a-runbook.md) toolearn more about runbook parameters.</span></span>

<span data-ttu-id="097b6-137">hello följande exempel används två parametrar **FeatureName** och **IsPresent**, toodetermine hello värden för egenskaper i hello **ParametersExample.sample** nod konfiguration, genereras under kompilering.</span><span class="sxs-lookup"><span data-stu-id="097b6-137">hello following example uses two parameters called **FeatureName** and **IsPresent**, toodetermine hello values of properties in hello **ParametersExample.sample** node configuration, generated during compilation.</span></span>

```powershell
Configuration ParametersExample
{
    param(
        [Parameter(Mandatory=$true)]

        [string] $FeatureName,

        [Parameter(Mandatory=$true)]
        [boolean] $IsPresent
    )

    $EnsureString = "Present"
    if($IsPresent -eq $false)
    {
        $EnsureString = "Absent"
    }

    Node "sample"
    {
        WindowsFeature ($FeatureName + "Feature")
        {
            Ensure = $EnsureString
            Name = $FeatureName
        }
    }
}
```

<span data-ttu-id="097b6-138">Du kan sammanställa DSC-konfigurationer som använder grundläggande parametrar i hello Azure Automation DSC-portalen eller med Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="097b6-138">You can compile DSC Configurations that use basic parameters in hello Azure Automation DSC portal, or with Azure PowerShell:</span></span>

### <a name="portal"></a><span data-ttu-id="097b6-139">Portalen</span><span class="sxs-lookup"><span data-stu-id="097b6-139">Portal</span></span>

<span data-ttu-id="097b6-140">I hello-portalen kan du ange parametervärden när du klickar på **Kompilera**.</span><span class="sxs-lookup"><span data-stu-id="097b6-140">In hello portal, you can enter parameter values after clicking **Compile**.</span></span>

![alternativ text](./media/automation-dsc-compile/DSC_compiling_1.png)

### <a name="powershell"></a><span data-ttu-id="097b6-142">PowerShell</span><span class="sxs-lookup"><span data-stu-id="097b6-142">PowerShell</span></span>

<span data-ttu-id="097b6-143">PowerShell kräver parametrar i en [hashtable](http://technet.microsoft.com/library/hh847780.aspx) där hello nyckel matchar hello parameternamnet och hello-värdet är lika med hello parametervärdet.</span><span class="sxs-lookup"><span data-stu-id="097b6-143">PowerShell requires parameters in a [hashtable](http://technet.microsoft.com/library/hh847780.aspx) where hello key matches hello parameter name, and hello value equals hello parameter value.</span></span>

```powershell
$Parameters = @{
    "FeatureName" = "Web-Server"
    "IsPresent" = $False
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ParametersExample" -Parameters $Parameters
```

<span data-ttu-id="097b6-144">Information om skicka PSCredentials som parametrar finns <a href="#credential-assets"> **Inloggningstillgångar** </a> nedan.</span><span class="sxs-lookup"><span data-stu-id="097b6-144">For information about passing PSCredentials as parameters, see <a href="#credential-assets">**Credential Assets**</a> below.</span></span>

## <a name="configurationdata"></a><span data-ttu-id="097b6-145">ConfigurationData</span><span class="sxs-lookup"><span data-stu-id="097b6-145">ConfigurationData</span></span>
<span data-ttu-id="097b6-146">**ConfigurationData** kan du tooseparate strukturella konfigurationen från miljön specifik konfiguration när du använder PowerShell DSC.</span><span class="sxs-lookup"><span data-stu-id="097b6-146">**ConfigurationData** allows you tooseparate structural configuration from any environment specific configuration while using PowerShell DSC.</span></span> <span data-ttu-id="097b6-147">Se [avgränsa ”vad” från ”där” i PowerShell DSC](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) toolearn mer om **ConfigurationData**.</span><span class="sxs-lookup"><span data-stu-id="097b6-147">See [Separating "What" from "Where" in PowerShell DSC](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) toolearn more about **ConfigurationData**.</span></span>

> [!NOTE]
> <span data-ttu-id="097b6-148">Du kan använda **ConfigurationData** vid kompilering i Azure Automation DSC med hjälp av Azure PowerShell, men inte i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="097b6-148">You can use **ConfigurationData** when compiling in Azure Automation DSC using Azure PowerShell, but not in hello Azure portal.</span></span>

<span data-ttu-id="097b6-149">hello följande DSC-exempelkonfiguration använder **ConfigurationData** via hello **$ConfigurationData** och **$AllNodes** nyckelord.</span><span class="sxs-lookup"><span data-stu-id="097b6-149">hello following example DSC configuration uses **ConfigurationData** via hello **$ConfigurationData** and **$AllNodes** keywords.</span></span> <span data-ttu-id="097b6-150">Du måste också hello [ **xWebAdministration** modulen](https://www.powershellgallery.com/packages/xWebAdministration/) för det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="097b6-150">You'll also need hello [**xWebAdministration** module](https://www.powershellgallery.com/packages/xWebAdministration/) for this example:</span></span>

```powershell
Configuration ConfigurationDataSample
{
    Import-DscResource -ModuleName xWebAdministration -Name MSFT_xWebsite

    Write-Verbose $ConfigurationData.NonNodeData.SomeMessage

    Node $AllNodes.Where{$_.Role -eq "WebServer"}.NodeName
    {
        xWebsite Site
        {
            Name = $Node.SiteName
            PhysicalPath = $Node.SiteContents
            Ensure   = "Present"
        }
    }
}
```

<span data-ttu-id="097b6-151">Du kan sammanställa hello DSC-konfigurationen ovan med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="097b6-151">You can compile hello DSC configuration above with PowerShell.</span></span> <span data-ttu-id="097b6-152">hello nedan PowerShell lägger till två nod konfigurationer toohello hämtar Azure Automation DSC-Server: **ConfigurationDataSample.MyVM1** och **ConfigurationDataSample.MyVM3**:</span><span class="sxs-lookup"><span data-stu-id="097b6-152">hello below PowerShell adds two node configurations toohello Azure Automation DSC Pull Server: **ConfigurationDataSample.MyVM1** and **ConfigurationDataSample.MyVM3**:</span></span>

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = "MyVM1"
            Role = "WebServer"
        },
        @{
            NodeName = "MyVM2"
            Role = "SQLServer"
        },
        @{
            NodeName = "MyVM3"
            Role = "WebServer"
        }
    )

    NonNodeData = @{
        SomeMessage = "I love Azure Automation DSC!"
    }
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ConfigurationDataSample" -ConfigurationData $ConfigData
```

## <a name="assets"></a><span data-ttu-id="097b6-153">Tillgångar</span><span class="sxs-lookup"><span data-stu-id="097b6-153">Assets</span></span>

<span data-ttu-id="097b6-154">Referenser för tillgången är hello samma i Azure Automation DSC-konfigurationer och runbooks.</span><span class="sxs-lookup"><span data-stu-id="097b6-154">Asset references are hello same in Azure Automation DSC configurations and runbooks.</span></span> <span data-ttu-id="097b6-155">Se hello nedan för mer information:</span><span class="sxs-lookup"><span data-stu-id="097b6-155">See hello following for more information:</span></span>

* [<span data-ttu-id="097b6-156">Certifikat</span><span class="sxs-lookup"><span data-stu-id="097b6-156">Certificates</span></span>](automation-certificates.md)
* [<span data-ttu-id="097b6-157">Anslutningar</span><span class="sxs-lookup"><span data-stu-id="097b6-157">Connections</span></span>](automation-connections.md)
* [<span data-ttu-id="097b6-158">Autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="097b6-158">Credentials</span></span>](automation-credentials.md)
* [<span data-ttu-id="097b6-159">Variabler</span><span class="sxs-lookup"><span data-stu-id="097b6-159">Variables</span></span>](automation-variables.md)

### <a name="credential-assets"></a><span data-ttu-id="097b6-160">Inloggningstillgångar</span><span class="sxs-lookup"><span data-stu-id="097b6-160">Credential Assets</span></span>

<span data-ttu-id="097b6-161">När DSC-konfigurationer i Azure Automation kan referera till inloggningstillgångar med **Get-AzureRmAutomationCredential**, inloggningstillgångar kan också överföras via parametrar, om så önskas.</span><span class="sxs-lookup"><span data-stu-id="097b6-161">While DSC configurations in Azure Automation can reference credential assets using **Get-AzureRmAutomationCredential**, credential assets can also be passed in via parameters, if desired.</span></span> <span data-ttu-id="097b6-162">Om en konfiguration tar en parameter av **PSCredential** skriver, måste du toopass hello sträng namnet på en Azure Automation-autentiseringsuppgiftstillgång som att parametervärdet i stället för ett PSCredential-objekt.</span><span class="sxs-lookup"><span data-stu-id="097b6-162">If a configuration takes a parameter of **PSCredential** type, then you need toopass hello string name of an Azure Automation credential asset as that parameter’s value, rather than a PSCredential object.</span></span> <span data-ttu-id="097b6-163">Hello bakgrunden, kommer hello Azure Automation-autentiseringsuppgiftstillgång med det namnet hämtas och skickas toohello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="097b6-163">Behind hello scenes, hello Azure Automation credential asset with that name will be retrieved and passed toohello configuration.</span></span>

<span data-ttu-id="097b6-164">Att hålla autentiseringsuppgifter kräver säker nodkonfigurationer (MOF configuration dokument) kryptering hello autentiseringsuppgifter i hello nod configuration MOF-filen.</span><span class="sxs-lookup"><span data-stu-id="097b6-164">Keeping credentials secure in node configurations (MOF configuration documents) requires encrypting hello credentials in hello node configuration MOF file.</span></span> <span data-ttu-id="097b6-165">Azure Automation tar ytterligare ett steg och krypterar hello hela MOF-filen.</span><span class="sxs-lookup"><span data-stu-id="097b6-165">Azure Automation takes this one step further and encrypts hello entire MOF file.</span></span> <span data-ttu-id="097b6-166">Men för närvarande du tala PowerShell DSC går bra för autentiseringsuppgifter toobe utdata i klartext under noden configuration MOF generation, eftersom PowerShell DSC inte vet att Azure Automation ska kryptera hello hela MOF-filen efter dess generation via en kompileringsjobbet.</span><span class="sxs-lookup"><span data-stu-id="097b6-166">However, currently you must tell PowerShell DSC it is okay for credentials toobe outputted in plain text during node configuration MOF generation, because PowerShell DSC doesn’t know that Azure Automation will be encrypting hello entire MOF file after its generation via a compilation job.</span></span>

<span data-ttu-id="097b6-167">Du kan se PowerShell DSC går bra för autentiseringsuppgifter toobe för utdata i oformaterad text i hello genereras nodkonfiguration MOF-filer med hjälp av [ **ConfigurationData**](#configurationdata).</span><span class="sxs-lookup"><span data-stu-id="097b6-167">You can tell PowerShell DSC that it is okay for credentials toobe outputted in plain text in hello generated node configuration MOFs using [**ConfigurationData**](#configurationdata).</span></span> <span data-ttu-id="097b6-168">Överför du `PSDscAllowPlainTextPassword = $true` via **ConfigurationData** för varje nod block namn som visas i hello DSC-konfigurationen och använder autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="097b6-168">You should pass `PSDscAllowPlainTextPassword = $true` via **ConfigurationData** for each node block’s name that appears in hello DSC configuration and uses credentials.</span></span>

<span data-ttu-id="097b6-169">hello visas följande exempel en DSC-konfiguration som använder en Automation-autentiseringsuppgiftstillgång.</span><span class="sxs-lookup"><span data-stu-id="097b6-169">hello following example shows a DSC configuration that uses an Automation credential asset.</span></span>

```powershell
Configuration CredentialSample
{
    $Cred = Get-AzureRmAutomationCredential -ResourceGroupName "ResourceGroup01" -AutomationAccountName "AutomationAcct" -Name "SomeCredentialAsset"

    Node $AllNodes.NodeName
    {
        File ExampleFile
        {
            SourcePath = "\\Server\share\path\file.ext"
            DestinationPath = "C:\destinationPath"
            Credential = $Cred
        }
    }
}
```

<span data-ttu-id="097b6-170">Du kan sammanställa hello DSC-konfigurationen ovan med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="097b6-170">You can compile hello DSC configuration above with PowerShell.</span></span> <span data-ttu-id="097b6-171">hello nedan PowerShell lägger till två nod konfigurationer toohello hämtar Azure Automation DSC-Server: **CredentialSample.MyVM1** och **CredentialSample.MyVM2**.</span><span class="sxs-lookup"><span data-stu-id="097b6-171">hello below PowerShell adds two node configurations toohello Azure Automation DSC Pull Server:  **CredentialSample.MyVM1** and **CredentialSample.MyVM2**.</span></span>

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = "*"
            PSDscAllowPlainTextPassword = $True
        },
        @{
            NodeName = "MyVM1"
        },
        @{
            NodeName = "MyVM2"
        }
    )
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "CredentialSample" -ConfigurationData $ConfigData
```

## <a name="importing-node-configurations"></a><span data-ttu-id="097b6-172">Importera nodkonfigurationer</span><span class="sxs-lookup"><span data-stu-id="097b6-172">Importing node configurations</span></span>

<span data-ttu-id="097b6-173">Du kan också importera noden configuratons (MOF-filer) som du har kompilerats utanför Azure.</span><span class="sxs-lookup"><span data-stu-id="097b6-173">You can also import node configuratons (MOFs) that you have compiled outside of Azure.</span></span> <span data-ttu-id="097b6-174">En fördel med detta är att noden confiturations kan signeras.</span><span class="sxs-lookup"><span data-stu-id="097b6-174">One advantage of this is that node confiturations can be signed.</span></span>
<span data-ttu-id="097b6-175">En signerad nodkonfiguration verifieras lokalt på en hanterad nod av hello DSC-agenten säkerställer att hello-konfigurationen som tillämpade toohello nod kommer från en auktoriserad källa.</span><span class="sxs-lookup"><span data-stu-id="097b6-175">A signed node configuration is verified locally on a managed node by hello DSC agent, ensuring that hello configuration being applied toohello node comes from an authorized source.</span></span>

> [!NOTE]
> <span data-ttu-id="097b6-176">Du kan använda import signerat konfigurationer i Azure Automation-konto, men Azure Automation stöder för närvarande inte kompilera signerade konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="097b6-176">You can use import signed configurations into your Azure Automation account, but Azure Automation does not currently support compiling signed configurations.</span></span>

> [!NOTE]
> <span data-ttu-id="097b6-177">En konfigurationsfil för noden får inte vara större än 1 MB tooallow den toobe importeras till Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="097b6-177">A node configuration file must be no larger than 1 MB tooallow it toobe imported into Azure Automation.</span></span>

<span data-ttu-id="097b6-178">Du kan lära dig hur toosign nodkonfigurationer på https://msdn.microsoft.com/en-us/powershell/wmf/5.1/dsc-improvements#how-to-sign-configuration-and-module.</span><span class="sxs-lookup"><span data-stu-id="097b6-178">You can learn how toosign node configurations at https://msdn.microsoft.com/en-us/powershell/wmf/5.1/dsc-improvements#how-to-sign-configuration-and-module.</span></span>

### <a name="importing-a-node-configuration-in-hello-azure-portal"></a><span data-ttu-id="097b6-179">Importera en konfiguration av noden i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="097b6-179">Importing a node configuration in hello Azure portal</span></span>

1. <span data-ttu-id="097b6-180">Från ditt Automation-konto klickar du på **DSC-nodkonfigurationer**.</span><span class="sxs-lookup"><span data-stu-id="097b6-180">From your Automation account, click **DSC node configurations**.</span></span>

    ![DSC-nodkonfigurationer](./media/automation-dsc-compile/node-config.png)
2. <span data-ttu-id="097b6-182">I hello **DSC-nodkonfigurationer** bladet, klickar du på **lägga till en NodeConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="097b6-182">In hello **DSC node configurations** blade, click **Add a NodeConfiguration**.</span></span>
3. <span data-ttu-id="097b6-183">I hello **importera** bladet, klickar du på hello mappen ikonen nästa toohello **nod konfigurationsfilen** textruta toobrowse för en nod konfigurationsfil (MOF) på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="097b6-183">In hello **Import** blade, click hello folder icon next toohello **Node Configuration File** textbox toobrowse for a node configuration file (MOF) on your local computer.</span></span>

    ![Sök efter lokal fil](./media/automation-dsc-compile/import-browse.png)
4. <span data-ttu-id="097b6-185">Ange ett namn i hello **Konfigurationsnamnet** textruta.</span><span class="sxs-lookup"><span data-stu-id="097b6-185">Enter a name in hello **Configuration Name** textbox.</span></span> <span data-ttu-id="097b6-186">Det här namnet måste matcha hello namn på hello konfiguration från vilken hello nodkonfiguration kompilerades.</span><span class="sxs-lookup"><span data-stu-id="097b6-186">This name must match hello name of hello configuration from which hello node configuration was compiled.</span></span>
5. <span data-ttu-id="097b6-187">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="097b6-187">Click **OK**.</span></span>

### <a name="importing-a-node-configuration-with-powershell"></a><span data-ttu-id="097b6-188">Importera en konfiguration av noden med PowerShell</span><span class="sxs-lookup"><span data-stu-id="097b6-188">Importing a node configuration with PowerShell</span></span>

<span data-ttu-id="097b6-189">Du kan använda hello [importera AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration) cmdlet tooimport en nodkonfiguration till ditt automation-konto.</span><span class="sxs-lookup"><span data-stu-id="097b6-189">You can use hello [Import-AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration) cmdlet tooimport a node configuration into your automation account.</span></span>

```powershell
Import-AzureRmAutomationDscNodeConfiguration -AutomationAccountName "MyAutomationAccount" -ResourceGroupName "MyResourceGroup" -ConfigurationName "MyNodeConfiguration" -Path "C:\MyConfigurations\TestVM1.mof"
```



