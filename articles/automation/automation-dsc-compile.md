---
title: Kompilera konfigurationer i Azure Automation DSC | Microsoft Docs
description: "Den här artikeln beskriver hur du kompilera önskad tillstånd Configuration DSC ()-konfigurationer för Azure Automation."
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
ms.openlocfilehash: 1aadd604e676659475f00760af3b0bdfb13a4792
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="compiling-configurations-in-azure-automation-dsc"></a><span data-ttu-id="e4113-103">Kompilera konfigurationer i Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="e4113-103">Compiling configurations in Azure Automation DSC</span></span>

<span data-ttu-id="e4113-104">Du kan sammanställa önskad tillstånd Configuration (DSC) konfigurationer på två sätt med Azure Automation: i Azure-portalen och med Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e4113-104">You can compile Desired State Configuration (DSC) configurations in two ways with Azure Automation: in the Azure portal, and with Windows PowerShell.</span></span> <span data-ttu-id="e4113-105">Tabellen nedan hjälper dig att avgöra när du ska använda vilken metod på grund av egenskaper för varje:</span><span class="sxs-lookup"><span data-stu-id="e4113-105">The following table will help you determine when to use which method based on the characteristics of each:</span></span>

### <a name="azure-portal"></a><span data-ttu-id="e4113-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e4113-106">Azure portal</span></span>

* <span data-ttu-id="e4113-107">Enklaste metoden med interaktivt användargränssnitt</span><span class="sxs-lookup"><span data-stu-id="e4113-107">Simplest method with interactive user interface</span></span>
* <span data-ttu-id="e4113-108">Formulär med enkla parametervärden</span><span class="sxs-lookup"><span data-stu-id="e4113-108">Form to provide simple parameter values</span></span>
* <span data-ttu-id="e4113-109">Spåra jobbets status</span><span class="sxs-lookup"><span data-stu-id="e4113-109">Easily track job state</span></span>
* <span data-ttu-id="e4113-110">Autentisera med Azure inloggning åtkomst</span><span class="sxs-lookup"><span data-stu-id="e4113-110">Access authenticated with Azure logon</span></span>

### <a name="windows-powershell"></a><span data-ttu-id="e4113-111">Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="e4113-111">Windows PowerShell</span></span>

* <span data-ttu-id="e4113-112">Anropa från kommandoraden med Windows PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="e4113-112">Call from command line with Windows PowerShell cmdlets</span></span>
* <span data-ttu-id="e4113-113">Kan ingå i automatisk lösning med flera steg</span><span class="sxs-lookup"><span data-stu-id="e4113-113">Can be included in automated solution with multiple steps</span></span>
* <span data-ttu-id="e4113-114">Ange enkla och komplexa parametervärden</span><span class="sxs-lookup"><span data-stu-id="e4113-114">Provide simple and complex parameter values</span></span>
* <span data-ttu-id="e4113-115">Spåra jobbets status</span><span class="sxs-lookup"><span data-stu-id="e4113-115">Track job state</span></span>
* <span data-ttu-id="e4113-116">Klienten som krävs för stöd av PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="e4113-116">Client required to support PowerShell cmdlets</span></span>
* <span data-ttu-id="e4113-117">Skicka ConfigurationData</span><span class="sxs-lookup"><span data-stu-id="e4113-117">Pass ConfigurationData</span></span>
* <span data-ttu-id="e4113-118">Kompilera konfigurationer som använder autentiseringsuppgifterna</span><span class="sxs-lookup"><span data-stu-id="e4113-118">Compile configurations that use credentials</span></span>

<span data-ttu-id="e4113-119">När du har valt en metod för kompilering, kan du följa respektive procedurerna nedan för att starta kompilering.</span><span class="sxs-lookup"><span data-stu-id="e4113-119">Once you have decided on a compilation method, you can follow the respective procedures below to start compiling.</span></span>

## <a name="compiling-a-dsc-configuration-with-the-azure-portal"></a><span data-ttu-id="e4113-120">Kompilering av DSC-konfigurationen med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e4113-120">Compiling a DSC Configuration with the Azure portal</span></span>

1. <span data-ttu-id="e4113-121">Från ditt Automation-konto klickar du på **DSC-konfigurationer**.</span><span class="sxs-lookup"><span data-stu-id="e4113-121">From your Automation account, click **DSC Configurations**.</span></span>
2. <span data-ttu-id="e4113-122">Klicka på en konfiguration för att öppna dess bladet.</span><span class="sxs-lookup"><span data-stu-id="e4113-122">Click a configuration to open its blade.</span></span>
3. <span data-ttu-id="e4113-123">Klicka på **Kompilera**.</span><span class="sxs-lookup"><span data-stu-id="e4113-123">Click **Compile**.</span></span>
4. <span data-ttu-id="e4113-124">Om konfigurationen har inga parametrar uppmanas du att bekräfta om du vill använda den.</span><span class="sxs-lookup"><span data-stu-id="e4113-124">If the configuration has no parameters, you will be prompted to confirm whether you want to compile it.</span></span> <span data-ttu-id="e4113-125">Om konfigurationen har parametrar av **kompilera konfiguration** öppnas bladet så att du kan ange parametervärden.</span><span class="sxs-lookup"><span data-stu-id="e4113-125">If the configuration has parameters, the **Compile Configuration** blade will open so you can provide parameter values.</span></span> <span data-ttu-id="e4113-126">Finns det [ **grundläggande parametrar** ](#basic-parameters) avsnittet nedan för mer information om parametrar.</span><span class="sxs-lookup"><span data-stu-id="e4113-126">See the [**Basic Parameters**](#basic-parameters) section below for further details on parameters.</span></span>
5. <span data-ttu-id="e4113-127">Den **Kompileringsjobbet** öppnas bladet så att du kan spåra kompilering jobbstatus och nodkonfigurationer (MOF configuration dokument) det orsakade placeras på Azure Automation DSC Pull-servern.</span><span class="sxs-lookup"><span data-stu-id="e4113-127">The **Compilation Job** blade is opened so that you can track the compilation job's status, and the node configurations (MOF configuration documents) it caused to be placed on the Azure Automation DSC Pull Server.</span></span>

## <a name="compiling-a-dsc-configuration-with-windows-powershell"></a><span data-ttu-id="e4113-128">Kompilering av DSC-konfigurationen med Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="e4113-128">Compiling a DSC Configuration with Windows PowerShell</span></span>

<span data-ttu-id="e4113-129">Du kan använda [ `Start-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) starta kompilerades med Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e4113-129">You can use [`Start-AzureRmAutomationDscCompilationJob`](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) to start compiling with Windows PowerShell.</span></span> <span data-ttu-id="e4113-130">Följande exempelkod startar kompilering av en DSC-konfiguration som kallas **SampleConfig**.</span><span class="sxs-lookup"><span data-stu-id="e4113-130">The following sample code starts compilation of a DSC configuration called **SampleConfig**.</span></span>

```powershell
Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"
```

<span data-ttu-id="e4113-131">`Start-AzureRmAutomationDscCompilationJob`Returnerar ett jobbobjekt som du kan använda för att spåra dess status för kompilering.</span><span class="sxs-lookup"><span data-stu-id="e4113-131">`Start-AzureRmAutomationDscCompilationJob` returns a compilation job object that you can use to track its status.</span></span> <span data-ttu-id="e4113-132">Du kan sedan använda detta jobbobjekt för kompilering med [ `Get-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob) att avgöra status för kompileringsjobbet och [ `Get-AzureRmAutomationDscCompilationJobOutput` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput) att visa dess dataströmmar (utdata).</span><span class="sxs-lookup"><span data-stu-id="e4113-132">You can then use this compilation job object with [`Get-AzureRmAutomationDscCompilationJob`](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob) to determine the status of the compilation job, and [`Get-AzureRmAutomationDscCompilationJobOutput`](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput) to view its streams (output).</span></span> <span data-ttu-id="e4113-133">Följande exempelkod startar sammanställning av den **SampleConfig** konfiguration, väntar tills den har slutförts och visar sedan dess dataströmmar.</span><span class="sxs-lookup"><span data-stu-id="e4113-133">The following sample code starts compilation of the **SampleConfig** configuration, waits until it has completed, and then displays its streams.</span></span>

```powershell
$CompilationJob = Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"

while($CompilationJob.EndTime –eq $null -and $CompilationJob.Exception –eq $null)
{
    $CompilationJob = $CompilationJob | Get-AzureRmAutomationDscCompilationJob
    Start-Sleep -Seconds 3
}

$CompilationJob | Get-AzureRmAutomationDscCompilationJobOutput –Stream Any
```

## <a name="basic-parameters"></a><span data-ttu-id="e4113-134">Grundläggande parametrar</span><span class="sxs-lookup"><span data-stu-id="e4113-134">Basic Parameters</span></span>
<span data-ttu-id="e4113-135">Parameterdeklaration i DSC-konfigurationer, inklusive parametertyper och egenskaper, fungerar på samma sätt som i Azure Automation-runbooks.</span><span class="sxs-lookup"><span data-stu-id="e4113-135">Parameter declaration in DSC configurations, including parameter types and properties, works the same as in Azure Automation runbooks.</span></span> <span data-ttu-id="e4113-136">Se [starta en runbook i Azure Automation](automation-starting-a-runbook.md) lära dig mer om runbook-parametrar.</span><span class="sxs-lookup"><span data-stu-id="e4113-136">See [Starting a runbook in Azure Automation](automation-starting-a-runbook.md) to learn more about runbook parameters.</span></span>

<span data-ttu-id="e4113-137">I följande exempel används två parametrar **FeatureName** och **IsPresent**, för att fastställa värden för egenskaper i den **ParametersExample.sample** nodkonfiguration genereras under kompilering.</span><span class="sxs-lookup"><span data-stu-id="e4113-137">The following example uses two parameters called **FeatureName** and **IsPresent**, to determine the values of properties in the **ParametersExample.sample** node configuration, generated during compilation.</span></span>

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

<span data-ttu-id="e4113-138">Du kan sammanställa DSC-konfigurationer som använder grundläggande parametrar i Azure Automation DSC-portalen eller med Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="e4113-138">You can compile DSC Configurations that use basic parameters in the Azure Automation DSC portal, or with Azure PowerShell:</span></span>

### <a name="portal"></a><span data-ttu-id="e4113-139">Portalen</span><span class="sxs-lookup"><span data-stu-id="e4113-139">Portal</span></span>

<span data-ttu-id="e4113-140">I portalen, kan du ange parametervärden när du klickar på **Kompilera**.</span><span class="sxs-lookup"><span data-stu-id="e4113-140">In the portal, you can enter parameter values after clicking **Compile**.</span></span>

![alternativ text](./media/automation-dsc-compile/DSC_compiling_1.png)

### <a name="powershell"></a><span data-ttu-id="e4113-142">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e4113-142">PowerShell</span></span>

<span data-ttu-id="e4113-143">PowerShell kräver parametrar i en [hashtable](http://technet.microsoft.com/library/hh847780.aspx) där nyckeln matchar parameternamnet och värdet är lika med parametervärdet.</span><span class="sxs-lookup"><span data-stu-id="e4113-143">PowerShell requires parameters in a [hashtable](http://technet.microsoft.com/library/hh847780.aspx) where the key matches the parameter name, and the value equals the parameter value.</span></span>

```powershell
$Parameters = @{
    "FeatureName" = "Web-Server"
    "IsPresent" = $False
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ParametersExample" -Parameters $Parameters
```

<span data-ttu-id="e4113-144">Information om skicka PSCredentials som parametrar finns <a href="#credential-assets"> **Inloggningstillgångar** </a> nedan.</span><span class="sxs-lookup"><span data-stu-id="e4113-144">For information about passing PSCredentials as parameters, see <a href="#credential-assets">**Credential Assets**</a> below.</span></span>

## <a name="configurationdata"></a><span data-ttu-id="e4113-145">ConfigurationData</span><span class="sxs-lookup"><span data-stu-id="e4113-145">ConfigurationData</span></span>
<span data-ttu-id="e4113-146">**ConfigurationData** kan du avgränsa strukturella konfigurationen från miljön specifik konfiguration när du använder PowerShell DSC.</span><span class="sxs-lookup"><span data-stu-id="e4113-146">**ConfigurationData** allows you to separate structural configuration from any environment specific configuration while using PowerShell DSC.</span></span> <span data-ttu-id="e4113-147">Se [avgränsa ”vad” från ”där” i PowerShell DSC](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) att lära dig mer om **ConfigurationData**.</span><span class="sxs-lookup"><span data-stu-id="e4113-147">See [Separating "What" from "Where" in PowerShell DSC](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) to learn more about **ConfigurationData**.</span></span>

> [!NOTE]
> <span data-ttu-id="e4113-148">Du kan använda **ConfigurationData** vid kompilering i Azure Automation DSC med hjälp av Azure PowerShell, men inte i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e4113-148">You can use **ConfigurationData** when compiling in Azure Automation DSC using Azure PowerShell, but not in the Azure portal.</span></span>

<span data-ttu-id="e4113-149">Följande DSC-exempelkonfiguration använder **ConfigurationData** via den **$ConfigurationData** och **$AllNodes** nyckelord.</span><span class="sxs-lookup"><span data-stu-id="e4113-149">The following example DSC configuration uses **ConfigurationData** via the **$ConfigurationData** and **$AllNodes** keywords.</span></span> <span data-ttu-id="e4113-150">Du måste också den [ **xWebAdministration** modulen](https://www.powershellgallery.com/packages/xWebAdministration/) för det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="e4113-150">You'll also need the [**xWebAdministration** module](https://www.powershellgallery.com/packages/xWebAdministration/) for this example:</span></span>

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

<span data-ttu-id="e4113-151">Du kan sammanställa DSC-konfigurationen ovan med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e4113-151">You can compile the DSC configuration above with PowerShell.</span></span> <span data-ttu-id="e4113-152">Den nedan PowerShell lägger till två nodkonfigurationer i Azure Automation DSC Pull-Server: **ConfigurationDataSample.MyVM1** och **ConfigurationDataSample.MyVM3**:</span><span class="sxs-lookup"><span data-stu-id="e4113-152">The below PowerShell adds two node configurations to the Azure Automation DSC Pull Server: **ConfigurationDataSample.MyVM1** and **ConfigurationDataSample.MyVM3**:</span></span>

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

## <a name="assets"></a><span data-ttu-id="e4113-153">Tillgångar</span><span class="sxs-lookup"><span data-stu-id="e4113-153">Assets</span></span>

<span data-ttu-id="e4113-154">Referenser för tillgången är samma i Azure Automation DSC-konfigurationer och runbooks.</span><span class="sxs-lookup"><span data-stu-id="e4113-154">Asset references are the same in Azure Automation DSC configurations and runbooks.</span></span> <span data-ttu-id="e4113-155">Läs följande avsnitt för mer information:</span><span class="sxs-lookup"><span data-stu-id="e4113-155">See the following for more information:</span></span>

* [<span data-ttu-id="e4113-156">Certifikat</span><span class="sxs-lookup"><span data-stu-id="e4113-156">Certificates</span></span>](automation-certificates.md)
* [<span data-ttu-id="e4113-157">Anslutningar</span><span class="sxs-lookup"><span data-stu-id="e4113-157">Connections</span></span>](automation-connections.md)
* [<span data-ttu-id="e4113-158">Autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="e4113-158">Credentials</span></span>](automation-credentials.md)
* [<span data-ttu-id="e4113-159">Variabler</span><span class="sxs-lookup"><span data-stu-id="e4113-159">Variables</span></span>](automation-variables.md)

### <a name="credential-assets"></a><span data-ttu-id="e4113-160">Inloggningstillgångar</span><span class="sxs-lookup"><span data-stu-id="e4113-160">Credential Assets</span></span>

<span data-ttu-id="e4113-161">När DSC-konfigurationer i Azure Automation kan referera till inloggningstillgångar med **Get-AzureRmAutomationCredential**, inloggningstillgångar kan också överföras via parametrar, om så önskas.</span><span class="sxs-lookup"><span data-stu-id="e4113-161">While DSC configurations in Azure Automation can reference credential assets using **Get-AzureRmAutomationCredential**, credential assets can also be passed in via parameters, if desired.</span></span> <span data-ttu-id="e4113-162">Om en konfiguration tar en parameter av **PSCredential** skriver, måste du skicka sträng namnet på en Azure Automation-autentiseringsuppgiftstillgång som att parametervärdet, i stället för ett PSCredential-objekt.</span><span class="sxs-lookup"><span data-stu-id="e4113-162">If a configuration takes a parameter of **PSCredential** type, then you need to pass the string name of an Azure Automation credential asset as that parameter’s value, rather than a PSCredential object.</span></span> <span data-ttu-id="e4113-163">Azure Automation-autentiseringsuppgiftstillgång med detta namn kommer hämtas och skickades till konfigurationen i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="e4113-163">Behind the scenes, the Azure Automation credential asset with that name will be retrieved and passed to the configuration.</span></span>

<span data-ttu-id="e4113-164">Att hålla autentiseringsuppgifter kräver säker nodkonfigurationer (MOF configuration dokument) kryptering av autentiseringsuppgifter i noden configuration MOF-filen.</span><span class="sxs-lookup"><span data-stu-id="e4113-164">Keeping credentials secure in node configurations (MOF configuration documents) requires encrypting the credentials in the node configuration MOF file.</span></span> <span data-ttu-id="e4113-165">Azure Automation tar ytterligare ett steg och krypterar hela MOF-filen.</span><span class="sxs-lookup"><span data-stu-id="e4113-165">Azure Automation takes this one step further and encrypts the entire MOF file.</span></span> <span data-ttu-id="e4113-166">Men måste för närvarande du ange PowerShell DSC går bra autentiseringsuppgifter mängden i klartext under noden configuration MOF generation, eftersom PowerShell DSC inte vet att Azure Automation kryptera hela MOF-filen efter dess generation via en kompileringsjobbet.</span><span class="sxs-lookup"><span data-stu-id="e4113-166">However, currently you must tell PowerShell DSC it is okay for credentials to be outputted in plain text during node configuration MOF generation, because PowerShell DSC doesn’t know that Azure Automation will be encrypting the entire MOF file after its generation via a compilation job.</span></span>

<span data-ttu-id="e4113-167">Du kan se PowerShell DSC som går bra autentiseringsuppgifter mängden i oformaterad text i den genererade nodkonfigurationen MOF-filer med hjälp av [ **ConfigurationData**](#configurationdata).</span><span class="sxs-lookup"><span data-stu-id="e4113-167">You can tell PowerShell DSC that it is okay for credentials to be outputted in plain text in the generated node configuration MOFs using [**ConfigurationData**](#configurationdata).</span></span> <span data-ttu-id="e4113-168">Överför du `PSDscAllowPlainTextPassword = $true` via **ConfigurationData** för varje nod block-namnet som visas i DSC-konfigurationen och använder autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e4113-168">You should pass `PSDscAllowPlainTextPassword = $true` via **ConfigurationData** for each node block’s name that appears in the DSC configuration and uses credentials.</span></span>

<span data-ttu-id="e4113-169">I följande exempel visas en DSC-konfiguration som använder en Automation-autentiseringsuppgiftstillgång.</span><span class="sxs-lookup"><span data-stu-id="e4113-169">The following example shows a DSC configuration that uses an Automation credential asset.</span></span>

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

<span data-ttu-id="e4113-170">Du kan sammanställa DSC-konfigurationen ovan med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e4113-170">You can compile the DSC configuration above with PowerShell.</span></span> <span data-ttu-id="e4113-171">Den nedan PowerShell lägger till två nodkonfigurationer i Azure Automation DSC Pull-Server: **CredentialSample.MyVM1** och **CredentialSample.MyVM2**.</span><span class="sxs-lookup"><span data-stu-id="e4113-171">The below PowerShell adds two node configurations to the Azure Automation DSC Pull Server:  **CredentialSample.MyVM1** and **CredentialSample.MyVM2**.</span></span>

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

## <a name="importing-node-configurations"></a><span data-ttu-id="e4113-172">Importera nodkonfigurationer</span><span class="sxs-lookup"><span data-stu-id="e4113-172">Importing node configurations</span></span>

<span data-ttu-id="e4113-173">Du kan också importera noden configuratons (MOF-filer) som du har kompilerats utanför Azure.</span><span class="sxs-lookup"><span data-stu-id="e4113-173">You can also import node configuratons (MOFs) that you have compiled outside of Azure.</span></span> <span data-ttu-id="e4113-174">En fördel med detta är att noden confiturations kan signeras.</span><span class="sxs-lookup"><span data-stu-id="e4113-174">One advantage of this is that node confiturations can be signed.</span></span>
<span data-ttu-id="e4113-175">En signerad nodkonfiguration verifieras lokalt på en hanterad nod av DSC-agenten säkerställer att konfigurationen tillämpas på noden kommer från en auktoriserad källa.</span><span class="sxs-lookup"><span data-stu-id="e4113-175">A signed node configuration is verified locally on a managed node by the DSC agent, ensuring that the configuration being applied to the node comes from an authorized source.</span></span>

> [!NOTE]
> <span data-ttu-id="e4113-176">Du kan använda import signerat konfigurationer i Azure Automation-konto, men Azure Automation stöder för närvarande inte kompilera signerade konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="e4113-176">You can use import signed configurations into your Azure Automation account, but Azure Automation does not currently support compiling signed configurations.</span></span>

> [!NOTE]
> <span data-ttu-id="e4113-177">En konfigurationsfil för noden får inte vara större än 1 MB så att det kan importeras till Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="e4113-177">A node configuration file must be no larger than 1 MB to allow it to be imported into Azure Automation.</span></span>

<span data-ttu-id="e4113-178">Du kan lära dig logga nodkonfigurationer på https://msdn.microsoft.com/en-us/powershell/wmf/5.1/dsc-improvements#how-to-sign-configuration-and-module.</span><span class="sxs-lookup"><span data-stu-id="e4113-178">You can learn how to sign node configurations at https://msdn.microsoft.com/en-us/powershell/wmf/5.1/dsc-improvements#how-to-sign-configuration-and-module.</span></span>

### <a name="importing-a-node-configuration-in-the-azure-portal"></a><span data-ttu-id="e4113-179">Importera en konfiguration av noden i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e4113-179">Importing a node configuration in the Azure portal</span></span>

1. <span data-ttu-id="e4113-180">Från ditt Automation-konto klickar du på **DSC-nodkonfigurationer**.</span><span class="sxs-lookup"><span data-stu-id="e4113-180">From your Automation account, click **DSC node configurations**.</span></span>

    ![DSC-nodkonfigurationer](./media/automation-dsc-compile/node-config.png)
2. <span data-ttu-id="e4113-182">I den **DSC-nodkonfigurationer** bladet, klickar du på **lägga till en NodeConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="e4113-182">In the **DSC node configurations** blade, click **Add a NodeConfiguration**.</span></span>
3. <span data-ttu-id="e4113-183">I den **importera** bladet klickar du på mappikonen bredvid den **nod konfigurationsfilen** textruta och bläddrar till en nod konfigurationsfil (MOF) på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="e4113-183">In the **Import** blade, click the folder icon next to the **Node Configuration File** textbox to browse for a node configuration file (MOF) on your local computer.</span></span>

    ![Sök efter lokal fil](./media/automation-dsc-compile/import-browse.png)
4. <span data-ttu-id="e4113-185">Ange ett namn i den **Konfigurationsnamnet** textruta.</span><span class="sxs-lookup"><span data-stu-id="e4113-185">Enter a name in the **Configuration Name** textbox.</span></span> <span data-ttu-id="e4113-186">Det här namnet måste matcha namnet på den konfiguration som nodkonfigurationen kompilerades.</span><span class="sxs-lookup"><span data-stu-id="e4113-186">This name must match the name of the configuration from which the node configuration was compiled.</span></span>
5. <span data-ttu-id="e4113-187">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e4113-187">Click **OK**.</span></span>

### <a name="importing-a-node-configuration-with-powershell"></a><span data-ttu-id="e4113-188">Importera en konfiguration av noden med PowerShell</span><span class="sxs-lookup"><span data-stu-id="e4113-188">Importing a node configuration with PowerShell</span></span>

<span data-ttu-id="e4113-189">Du kan använda den [importera AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration) för att importera en nodkonfiguration till ditt automation-konto.</span><span class="sxs-lookup"><span data-stu-id="e4113-189">You can use the [Import-AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration) cmdlet to import a node configuration into your automation account.</span></span>

```powershell
Import-AzureRmAutomationDscNodeConfiguration -AutomationAccountName "MyAutomationAccount" -ResourceGroupName "MyResourceGroup" -ConfigurationName "MyNodeConfiguration" -Path "C:\MyConfigurations\TestVM1.mof"
```



