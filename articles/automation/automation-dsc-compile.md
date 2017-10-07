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
# <a name="compiling-configurations-in-azure-automation-dsc"></a>Kompilera konfigurationer i Azure Automation DSC

Du kan sammanställa önskad tillstånd Configuration (DSC) konfigurationer på två sätt med Azure Automation: hello Azure-portalen och med Windows PowerShell. hello nedan hjälper dig att avgöra när toouse vilken metod baserat på hello egenskaper för varje:

### <a name="azure-portal"></a>Azure Portal

* Enklaste metoden med interaktivt användargränssnitt
* Formuläret tooprovide enkel parametervärden
* Spåra jobbets status
* Autentisera med Azure inloggning åtkomst

### <a name="windows-powershell"></a>Windows PowerShell

* Anropa från kommandoraden med Windows PowerShell-cmdlets
* Kan ingå i automatisk lösning med flera steg
* Ange enkla och komplexa parametervärden
* Spåra jobbets status
* Klienten krävs toosupport PowerShell-cmdlets
* Skicka ConfigurationData
* Kompilera konfigurationer som använder autentiseringsuppgifterna

När du har valt en metod för kompilering, kan du följa hello respektive procedurerna nedan toostart kompilering.

## <a name="compiling-a-dsc-configuration-with-hello-azure-portal"></a>Kompilering av DSC-konfigurationen med hello Azure-portalen

1. Från ditt Automation-konto klickar du på **DSC-konfigurationer**.
2. Klicka på en konfiguration tooopen dess bladet.
3. Klicka på **Kompilera**.
4. Om hello konfiguration har inga parametrar, kommer du att ange tooconfirm om du vill toocompile den. Om hello konfiguration har parametrar hello **kompilera konfiguration** öppnas bladet så att du kan ange parametervärden. Se hello [ **grundläggande parametrar** ](#basic-parameters) avsnittet nedan för mer information om parametrar.
5. Hej **Kompileringsjobbet** öppnas bladet så att du kan spåra hello kompilering jobbets status och hello nodkonfigurationer (MOF configuration dokument) det orsakade toobe placerad hello hämtar Azure Automation DSC-Server.

## <a name="compiling-a-dsc-configuration-with-windows-powershell"></a>Kompilering av DSC-konfigurationen med Windows PowerShell

Du kan använda [ `Start-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) toostart kompilerades med Windows PowerShell. hello följande exempelkod startar kompilering av en DSC-konfiguration som kallas **SampleConfig**.

```powershell
Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"
```

`Start-AzureRmAutomationDscCompilationJob`Returnerar en sammanställning jobbet objekt som du kan använda tootrack dess status. Du kan sedan använda detta jobbobjekt för kompilering med [ `Get-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob) toodetermine hello status för hello kompileringsjobbet och [ `Get-AzureRmAutomationDscCompilationJobOutput` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput) tooview dess dataströmmar (utdata). hello följande exempelkod startar sammanställning av hello **SampleConfig** konfiguration, väntar tills den har slutförts och visar sedan dess dataströmmar.

```powershell
$CompilationJob = Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"

while($CompilationJob.EndTime –eq $null -and $CompilationJob.Exception –eq $null)
{
    $CompilationJob = $CompilationJob | Get-AzureRmAutomationDscCompilationJob
    Start-Sleep -Seconds 3
}

$CompilationJob | Get-AzureRmAutomationDscCompilationJobOutput –Stream Any
```

## <a name="basic-parameters"></a>Grundläggande parametrar
Parameterdeklaration i DSC-konfigurationer, inklusive parametertyper och egenskaper, fungerar hello samma som i Azure Automation-runbooks. Se [starta en runbook i Azure Automation](automation-starting-a-runbook.md) toolearn mer om runbook-parametrar.

hello följande exempel används två parametrar **FeatureName** och **IsPresent**, toodetermine hello värden för egenskaper i hello **ParametersExample.sample** nod konfiguration, genereras under kompilering.

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

Du kan sammanställa DSC-konfigurationer som använder grundläggande parametrar i hello Azure Automation DSC-portalen eller med Azure PowerShell:

### <a name="portal"></a>Portalen

I hello-portalen kan du ange parametervärden när du klickar på **Kompilera**.

![alternativ text](./media/automation-dsc-compile/DSC_compiling_1.png)

### <a name="powershell"></a>PowerShell

PowerShell kräver parametrar i en [hashtable](http://technet.microsoft.com/library/hh847780.aspx) där hello nyckel matchar hello parameternamnet och hello-värdet är lika med hello parametervärdet.

```powershell
$Parameters = @{
    "FeatureName" = "Web-Server"
    "IsPresent" = $False
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ParametersExample" -Parameters $Parameters
```

Information om skicka PSCredentials som parametrar finns <a href="#credential-assets"> **Inloggningstillgångar** </a> nedan.

## <a name="configurationdata"></a>ConfigurationData
**ConfigurationData** kan du tooseparate strukturella konfigurationen från miljön specifik konfiguration när du använder PowerShell DSC. Se [avgränsa ”vad” från ”där” i PowerShell DSC](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) toolearn mer om **ConfigurationData**.

> [!NOTE]
> Du kan använda **ConfigurationData** vid kompilering i Azure Automation DSC med hjälp av Azure PowerShell, men inte i hello Azure-portalen.

hello följande DSC-exempelkonfiguration använder **ConfigurationData** via hello **$ConfigurationData** och **$AllNodes** nyckelord. Du måste också hello [ **xWebAdministration** modulen](https://www.powershellgallery.com/packages/xWebAdministration/) för det här exemplet:

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

Du kan sammanställa hello DSC-konfigurationen ovan med PowerShell. hello nedan PowerShell lägger till två nod konfigurationer toohello hämtar Azure Automation DSC-Server: **ConfigurationDataSample.MyVM1** och **ConfigurationDataSample.MyVM3**:

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

## <a name="assets"></a>Tillgångar

Referenser för tillgången är hello samma i Azure Automation DSC-konfigurationer och runbooks. Se hello nedan för mer information:

* [Certifikat](automation-certificates.md)
* [Anslutningar](automation-connections.md)
* [Autentiseringsuppgifter](automation-credentials.md)
* [Variabler](automation-variables.md)

### <a name="credential-assets"></a>Inloggningstillgångar

När DSC-konfigurationer i Azure Automation kan referera till inloggningstillgångar med **Get-AzureRmAutomationCredential**, inloggningstillgångar kan också överföras via parametrar, om så önskas. Om en konfiguration tar en parameter av **PSCredential** skriver, måste du toopass hello sträng namnet på en Azure Automation-autentiseringsuppgiftstillgång som att parametervärdet i stället för ett PSCredential-objekt. Hello bakgrunden, kommer hello Azure Automation-autentiseringsuppgiftstillgång med det namnet hämtas och skickas toohello konfiguration.

Att hålla autentiseringsuppgifter kräver säker nodkonfigurationer (MOF configuration dokument) kryptering hello autentiseringsuppgifter i hello nod configuration MOF-filen. Azure Automation tar ytterligare ett steg och krypterar hello hela MOF-filen. Men för närvarande du tala PowerShell DSC går bra för autentiseringsuppgifter toobe utdata i klartext under noden configuration MOF generation, eftersom PowerShell DSC inte vet att Azure Automation ska kryptera hello hela MOF-filen efter dess generation via en kompileringsjobbet.

Du kan se PowerShell DSC går bra för autentiseringsuppgifter toobe för utdata i oformaterad text i hello genereras nodkonfiguration MOF-filer med hjälp av [ **ConfigurationData**](#configurationdata). Överför du `PSDscAllowPlainTextPassword = $true` via **ConfigurationData** för varje nod block namn som visas i hello DSC-konfigurationen och använder autentiseringsuppgifter.

hello visas följande exempel en DSC-konfiguration som använder en Automation-autentiseringsuppgiftstillgång.

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

Du kan sammanställa hello DSC-konfigurationen ovan med PowerShell. hello nedan PowerShell lägger till två nod konfigurationer toohello hämtar Azure Automation DSC-Server: **CredentialSample.MyVM1** och **CredentialSample.MyVM2**.

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

## <a name="importing-node-configurations"></a>Importera nodkonfigurationer

Du kan också importera noden configuratons (MOF-filer) som du har kompilerats utanför Azure. En fördel med detta är att noden confiturations kan signeras.
En signerad nodkonfiguration verifieras lokalt på en hanterad nod av hello DSC-agenten säkerställer att hello-konfigurationen som tillämpade toohello nod kommer från en auktoriserad källa.

> [!NOTE]
> Du kan använda import signerat konfigurationer i Azure Automation-konto, men Azure Automation stöder för närvarande inte kompilera signerade konfigurationer.

> [!NOTE]
> En konfigurationsfil för noden får inte vara större än 1 MB tooallow den toobe importeras till Azure Automation.

Du kan lära dig hur toosign nodkonfigurationer på https://msdn.microsoft.com/en-us/powershell/wmf/5.1/dsc-improvements#how-to-sign-configuration-and-module.

### <a name="importing-a-node-configuration-in-hello-azure-portal"></a>Importera en konfiguration av noden i hello Azure-portalen

1. Från ditt Automation-konto klickar du på **DSC-nodkonfigurationer**.

    ![DSC-nodkonfigurationer](./media/automation-dsc-compile/node-config.png)
2. I hello **DSC-nodkonfigurationer** bladet, klickar du på **lägga till en NodeConfiguration**.
3. I hello **importera** bladet, klickar du på hello mappen ikonen nästa toohello **nod konfigurationsfilen** textruta toobrowse för en nod konfigurationsfil (MOF) på den lokala datorn.

    ![Sök efter lokal fil](./media/automation-dsc-compile/import-browse.png)
4. Ange ett namn i hello **Konfigurationsnamnet** textruta. Det här namnet måste matcha hello namn på hello konfiguration från vilken hello nodkonfiguration kompilerades.
5. Klicka på **OK**.

### <a name="importing-a-node-configuration-with-powershell"></a>Importera en konfiguration av noden med PowerShell

Du kan använda hello [importera AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration) cmdlet tooimport en nodkonfiguration till ditt automation-konto.

```powershell
Import-AzureRmAutomationDscNodeConfiguration -AutomationAccountName "MyAutomationAccount" -ResourceGroupName "MyResourceGroup" -ConfigurationName "MyNodeConfiguration" -Path "C:\MyConfigurations\TestVM1.mof"
```



