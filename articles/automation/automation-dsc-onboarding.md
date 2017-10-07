---
title: "aaaOnboarding datorer för hantering av Azure Automation DSC | Microsoft Docs"
description: "Hur toosetup datorer för hantering med Azure Automation DSC"
services: automation
documentationcenter: dev-center-name
author: eslesar
manager: carmonm
ms.assetid: da13e1f5-2a1c-443b-8e3b-9f0d6f9e4810
ms.service: automation
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: TBD
ms.date: 12/13/2016
ms.author: eslesar
ms.openlocfilehash: ef15801fec2ffea4ba62dcba2fbe9af09268e424
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="onboarding-machines-for-management-by-azure-automation-dsc"></a>Onboarding-datorer för hantering av Azure Automation DSC

## <a name="why-manage-machines-with-azure-automation-dsc"></a>Varför hantera datorer med Azure Automation DSC?

Som [PowerShell Desired State Configuration](https://technet.microsoft.com/library/dn249912.aspx), Azure Automation Desired State Configuration är en enkel men kraftfull, configuration management-tjänst för DSC-noder (fysiska och virtuella datorer) i molnet eller lokalt Datacenter. Den möjliggör skalbarhet över tusentals datorer snabbt och enkelt från en central, säker plats. Du kan enkelt publicera datorer, tilldela dem deklarativ konfigurationer och visa rapporter som visar var datorn toohello önskad kompatibilitetstillstånd du angett. hello Azure Automation DSC management lagret är tooDSC vilka hello Azure Automation-hanteringslager är tooPowerShell skript. Med andra ord i hello samma sätt som med hjälp av Azure Automation kan du hantera PowerShell-skript, du kan också hantera DSC-konfigurationer. toolearn mer om hello fördelarna med att använda Azure Automation DSC finns [översikt över Azure Automation DSC](automation-dsc-overview.md).

Azure Automation DSC kan vara används toomanage olika datorer:

* Virtuella Azure-datorer (klassisk)
* Virtuella Azure-datorer
* Amazon Web Services (AWS) virtuella datorer
* Fysisk virtuell Windows-datorer lokalt eller i ett moln än Azure/AWS
* Fysisk virtuell Linux-datorer lokalt, i Azure eller i ett moln än Azure

Dessutom om du inte är redo toomanage datorkonfigurationen från molnet hello kan Azure Automation DSC också användas som en endast rapport-slutpunkt. Detta kan du tooset (push) önskad konfiguration via DSC lokala och visa information om omfattande reporting på noden följer hello önskad status i Azure Automation.

hello följande avsnitt beskriver hur du kan publicera varje typ av dator tooAzure Automation DSC.

## <a name="azure-virtual-machines-classic"></a>Virtuella Azure-datorer (klassisk)

Med Azure Automation DSC kan du enkelt publicera virtuella Azure-datorer (klassisk) för konfigurationshantering med hello Azure-portalen eller PowerShell. Under huven hello och en administratör med tooremote i hello VM registrerar hello Azure VM Desired State Configuration-tillägget hello VM med Azure Automation DSC. Eftersom hello Azure VM Desired State Configuration-tillägg körs asynkront, steg tootrack förloppet eller felsöka den finns i hello [ **felsöka Azure virtuella onboarding** ](#troubleshooting-azure-virtual-machine-onboarding)nedan.

### <a name="azure-portal"></a>Azure Portal

I hello [Azure-portalen](http://portal.azure.com/), klickar du på **Bläddra** -> **virtuella datorer (klassisk)**. Välj hello Windows VM som du vill tooonboard. På bladet instrumentpanelen hello virtuell dator klickar du på **alla inställningar** -> **tillägg** -> **Lägg till**  ->   **Azure Automation DSC** -> **skapa**. Ange hello [PowerShell DSC Local Configuration Manager värden](https://msdn.microsoft.com/powershell/dsc/metaconfig4) krävs för din användningsfall registreringsnyckel ditt Automation-konto och URL: en registrering och eventuellt en nod configuration tooassign toohello VM.

![](./media/automation-dsc-onboarding/DSC_Onboarding_1.png)

URL: en för registrering av toofind hello och nyckeln för hello Automation-konto tooonboard hello dator, se hello [ **Secure registrering** ](#secure-registration) nedan.

### <a name="powershell"></a>PowerShell

```powershell
# log in tooboth Azure Service Management and Azure Resource Manager
Add-AzureAccount
Add-AzureRmAccount

# fill in correct values for your VM/Automation account here
$VMName = ""
$ServiceName = ""
$AutomationAccountName = ""
$AutomationAccountResourceGroup = ""

# fill in hello name of a Node Configuration in Azure Automation DSC, for this VM tooconform to
$NodeConfigName = ""

# get Azure Automation DSC registration info
$Account = Get-AzureRmAutomationAccount -ResourceGroupName $AutomationAccountResourceGroup -Name $AutomationAccountName
$RegistrationInfo = $Account | Get-AzureRmAutomationRegistrationInfo

# use hello DSC extension tooonboard hello VM for management with Azure Automation DSC
$VM = Get-AzureVM -Name $VMName -ServiceName $ServiceName

$PublicConfiguration = ConvertTo-Json -Depth 8 @{
    SasToken = ""
    ModulesUrl = "https://eus2oaasibizamarketprod1.blob.core.windows.net/automationdscpreview/RegistrationMetaConfigV2.zip"
    ConfigurationFunction = "RegistrationMetaConfigV2.ps1\RegistrationMetaConfigV2"

# update these PowerShell DSC Local Configuration Manager defaults if they do not match your use case.
# See https://technet.microsoft.com/library/dn249922.aspx?f=255&MSPPError=-2147217396 for more details
    Properties = @{
    RegistrationKey = @{
        UserName = 'notused'
        Password = 'PrivateSettingsRef:RegistrationKey'
    }
    RegistrationUrl = $RegistrationInfo.Endpoint
    NodeConfigurationName = $NodeConfigName
    ConfigurationMode = "ApplyAndMonitor"
    ConfigurationModeFrequencyMins = 15
    RefreshFrequencyMins = 30
    RebootNodeIfNeeded = $False
    ActionAfterReboot = "ContinueConfiguration"
    AllowModuleOverwrite = $False
    }
}

$PrivateConfiguration = ConvertTo-Json -Depth 8 @{
    Items = @{
        RegistrationKey = $RegistrationInfo.PrimaryKey
    }
}

$VM = Set-AzureVMExtension `
    -VM $vm `
    -Publisher Microsoft.Powershell `
    -ExtensionName DSC `
    -Version 2.19 `
    -PublicConfiguration $PublicConfiguration `
    -PrivateConfiguration $PrivateConfiguration `
    -ForceUpdate

$VM | Update-AzureVM
```

## <a name="azure-virtual-machines"></a>Virtuella Azure-datorer

Azure Automation DSC kan du enkelt publicera virtuella Azure-datorer för konfigurationshantering, med hello Azure-portalen, Azure Resource Manager-mallar eller PowerShell. Under huven hello och en administratör med tooremote i hello VM registrerar hello Azure VM Desired State Configuration-tillägget hello VM med Azure Automation DSC. Eftersom hello Azure VM Desired State Configuration-tillägg körs asynkront, steg tootrack förloppet eller felsöka den finns i hello [ **felsöka Azure virtuella onboarding** ](#troubleshooting-azure-virtual-machine-onboarding)nedan.

### <a name="azure-portal"></a>Azure Portal

I hello [Azure-portalen](https://portal.azure.com/), navigera toohello Azure Automation-konto där du vill tooonboard virtuella datorer. Klicka på instrumentpanelen för hello Automation-konto, **DSC-noder** -> **lägga till Azure VM**.

Under **Välj virtuella datorer tooonboard**, Välj en eller flera Azure virtuella datorer tooonboard.

![](./media/automation-dsc-onboarding/DSC_Onboarding_2.png)

Under **konfigurera registreringsdata**, ange hello [PowerShell DSC Local Configuration Manager värden](https://msdn.microsoft.com/powershell/dsc/metaconfig4) krävs för din användningsfall och eventuellt en nod configuration tooassign toohello VM.

![](./media/automation-dsc-onboarding/DSC_Onboarding_3.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager-mallar

Virtuella Azure-datorer kan distribueras och publicerats så tooAzure Automation DSC via Azure Resource Manager-mallar. Se [konfigurera en virtuell dator via DSC-tillägg och Azure Automation DSC](https://azure.microsoft.com/documentation/templates/dsc-extension-azure-automation-pullserver/) för en exempelmall som onboards en befintlig virtuell dator tooAzure Automation DSC. toofind hello nyckel och registrering URL: en registrering vidtas som indata i den här mallen finns hello [ **Secure registrering** ](#secure-registration) nedan.

### <a name="powershell"></a>PowerShell

Hej [registrera AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode) används tooonboard virtuella datorer i hello Azure-portalen via PowerShell kan du vara.

## <a name="amazon-web-services-aws-virtual-machines"></a>Amazon Web Services (AWS) virtuella datorer

Du kan enkelt publicera Amazon Web Services virtuella datorer för konfigurationshantering av Azure Automation DSC med hello AWS DSC Toolkit. Du kan lära dig mer om hello toolkit [här](https://blogs.msdn.microsoft.com/powershell/2016/04/20/aws-dsc-toolkit/).

## <a name="physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azureaws"></a>Fysisk virtuell Windows-datorer lokalt eller i ett moln än Azure/AWS

Lokalt Windows-datorer och Windows-datorer i Azure-moln (till exempel Amazon Web Services) kan också vara publicerats så tooAzure Automation DSC, så länge som de har utgående åtkomst toohello internet, via några enkla steg:

1. Se till att hello senaste versionen av [WMF 5](http://aka.ms/wmf5latest) är installerad på hello datorer du vill tooonboard tooAzure Automation DSC.
2. Följ hello anvisningarna i avsnittet [ **genererar DSC metaconfigurations** ](#generating-dsc-metaconfigurations) nedan toogenerate behövs en mapp som innehåller hello DSC metaconfigurations.
3. Tillämpa hello PowerShell DSC metakonfigurationen toohello datorer du vill tooonboard via fjärranslutning. **hello-datorn det här kommandot körs från måste ha hello senaste versionen av [WMF 5](http://aka.ms/wmf5latest) installerat**:

    ```powershell
    Set-DscLocalConfigurationManager -Path C:\Users\joe\Desktop\DscMetaConfigs -ComputerName MyServer1, MyServer2
    ```

4. Om du inte använder hello PowerShell DSC metaconfigurations via fjärranslutning, kopiera hello metaconfigurations mappen från steg 2 till varje dator tooonboard. Sedan anropar **Set DscLocalConfigurationManager** lokalt på varje dator tooonboard.
5. Med hello Azure-portalen eller cmdletar måste du kontrollera att hello datorer tooonboard nu visas som DSC-noder som är registrerade i Azure Automation-konto.

## <a name="physicalvirtual-linux-machines-on-premises-in-azure-or-in-a-cloud-other-than-azure"></a>Fysisk virtuell Linux-datorer lokalt, i Azure eller i ett moln än Azure

Lokal Linux-datorer, Linux-datorer i Azure, och Linux-datorer i Azure-moln kan också vara publicerats så tooAzure Automation DSC, så länge som de har utgående åtkomst toohello internet, via några enkla steg:

1. Se till att hello senaste versionen av [PowerShell Desired State Configuration för Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) är installerad på hello datorer du vill tooonboard tooAzure Automation DSC.
2. Om hello [PowerShell DSC Local Configuration Manager standardvärden](https://msdn.microsoft.com/powershell/dsc/metaconfig4) matchar din användningsfall och du vill tooonboard datorer så att de **både** hämtar från och rapportera tooAzure Automation DSC:

   + Använd Register.py tooonboard med hello PowerShell DSC Local Configuration Manager som standard på varje Linux datorn tooonboard tooAzure Automation DSC:

     `/opt/microsoft/dsc/Scripts/Register.py <Automation account registration key> <Automation account registration URL>`

   + toofind hello nyckel och registrering URL: en registrering för Automation-kontot finns hello [ **Secure registrering** ](#secure-registration) nedan.

     Om hello PowerShell DSC Local Configuration Manager som standard **gör** **inte** matchar din användningsfall eller om du vill tooonboard datorer så att de endast rapportera tooAzure Automation DSC, men inte hämtar konfiguration eller PowerShell-moduler från den, följer du steg 3-6. Annars Fortsätt direkt toostep 6.

3. Följ anvisningarna hello i hello [ **genererar DSC metaconfigurations** ](#generating-dsc-metaconfigurations) avsnittet nedan toogenerate en mapp som innehåller hello behövs DSC metaconfigurations.
4. Via fjärranslutning gäller hello PowerShell DSC metakonfigurationen toohello datorer du vill tooonboard:

    ```powershell
    $SecurePass = ConvertTo-SecureString -String "<root password>" -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential "root", $SecurePass
    $Opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

    # need a CimSession for each Linux machine tooonboard

    $Session = New-CimSession -Credential $Cred -ComputerName <your Linux machine> -Port 5986 -Authentication basic -SessionOption $Opt

    Set-DscLocalConfigurationManager -CimSession $Session -Path C:\Users\joe\Desktop\DscMetaConfigs
    ```

hello-datorn det här kommandot körs från måste ha hello senaste versionen av [WMF 5](http://aka.ms/wmf5latest) installerad.

1. Om du inte kan använda hello PowerShell DSC metaconfigurations via fjärranslutning, för varje dator tooonboard Linux kopiera hello metakonfigurationen motsvarande toothat dator från hello mapp i steg 5 till hello Linux-dator. Sedan anropar `SetDscLocalConfigurationManager.py` lokalt på varje Linux-dator du vill tooonboard tooAzure Automation DSC:

   `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py -configurationmof <path toometaconfiguration file>`

2. Med hello Azure-portalen eller cmdletar måste du kontrollera att hello datorer tooonboard nu visas som DSC-noder som är registrerade i Azure Automation-konto.

## <a name="generating-dsc-metaconfigurations"></a>Generera DSC metaconfigurations

toogenerically publicera någon datorn tooAzure Automation DSC, en [DSC-metakonfigurationen](https://msdn.microsoft.com/en-us/powershell/dsc/metaconfig) kan genereras som, när tillämpas, visar hello DSC-agenten på hello datorn toopull från och/eller rapportera tooAzure Automation DSC. DSC-metaconfigurations för Azure Automation DSC kan genereras med hjälp av antingen en PowerShell DSC-konfiguration eller hello Azure Automation PowerShell-cmdlets.

> [!NOTE]
> DSC-metaconfigurations innehålla hello hemligheter behövs tooonboard dator-tooan Automation-konto för hantering. Se till att tooproperly skydda alla DSC-metaconfigurations som du skapar eller tar bort dem när du har använt.

### <a name="using-a-dsc-configuration"></a>Med hjälp av DSC-konfiguration

1. Öppna hello PowerShell ISE som administratör på en dator i din lokala miljö. hello datorn måste ha hello senaste versionen av [WMF 5](http://aka.ms/wmf5latest) installerad.
2. Kopiera hello följande skript lokalt. Det här skriptet innehåller en PowerShell DSC-konfiguration för att skapa metaconfigurations och ett kommando tookick hello metakonfigurationen skapa.

    ```powershell
    # hello DSC configuration that will generate metaconfigurations
    [DscLocalConfigurationManager()]
    Configuration DscMetaConfigs
    {

        param
        (
            [Parameter(Mandatory=$True)]
            [String]$RegistrationUrl,

            [Parameter(Mandatory=$True)]
            [String]$RegistrationKey,

            [Parameter(Mandatory=$True)]
            [String[]]$ComputerName,

            [Int]$RefreshFrequencyMins = 30,

            [Int]$ConfigurationModeFrequencyMins = 15,

            [String]$ConfigurationMode = "ApplyAndMonitor",

            [String]$NodeConfigurationName,

            [Boolean]$RebootNodeIfNeeded= $False,

            [String]$ActionAfterReboot = "ContinueConfiguration",

            [Boolean]$AllowModuleOverwrite = $False,

            [Boolean]$ReportOnly
        )

        if(!$NodeConfigurationName -or $NodeConfigurationName -eq "")
        {
            $ConfigurationNames = $null
        }
        else
        {
            $ConfigurationNames = @($NodeConfigurationName)
        }

        if($ReportOnly)
        {
        $RefreshMode = "PUSH"
        }
        else
        {
        $RefreshMode = "PULL"
        }

        Node $ComputerName
        {

            Settings
            {
                RefreshFrequencyMins = $RefreshFrequencyMins
                RefreshMode = $RefreshMode
                ConfigurationMode = $ConfigurationMode
                AllowModuleOverwrite = $AllowModuleOverwrite
                RebootNodeIfNeeded = $RebootNodeIfNeeded
                ActionAfterReboot = $ActionAfterReboot
                ConfigurationModeFrequencyMins = $ConfigurationModeFrequencyMins
            }

            if(!$ReportOnly)
            {
            ConfigurationRepositoryWeb AzureAutomationDSC
                {
                    ServerUrl = $RegistrationUrl
                    RegistrationKey = $RegistrationKey
                    ConfigurationNames = $ConfigurationNames
                }

                ResourceRepositoryWeb AzureAutomationDSC
                {
                ServerUrl = $RegistrationUrl
                RegistrationKey = $RegistrationKey
                }
            }

            ReportServerWeb AzureAutomationDSC
            {
                ServerUrl = $RegistrationUrl
                RegistrationKey = $RegistrationKey
            }
        }
    }

    # Create hello metaconfigurations
    # TODO: edit hello below as needed for your use case
    $Params = @{
        RegistrationUrl = '<fill me in>';
        RegistrationKey = '<fill me in>';
        ComputerName = @('<some VM tooonboard>', '<some other VM tooonboard>');
        NodeConfigurationName = 'SimpleConfig.webserver';
        RefreshFrequencyMins = 30;
        ConfigurationModeFrequencyMins = 15;
        RebootNodeIfNeeded = $False;
        AllowModuleOverwrite = $False;
        ConfigurationMode = 'ApplyAndMonitor';
        ActionAfterReboot = 'ContinueConfiguration';
        ReportOnly = $False;  # Set too$True toohave machines only report tooAA DSC but not pull from it
    }

    # Use PowerShell splatting toopass parameters toohello DSC configuration being invoked
    # For more info about splatting, run: Get-Help -Name about_Splatting
    DscMetaConfigs @Params
    ```

3. Fyll i hello registreringsnyckel och URL: en för Automation-konto, samt hello namnen på hello datorer tooonboard. Alla andra parametrar är valfria. toofind hello nyckel och registrering URL: en registrering för Automation-kontot finns hello [ **Secure registrering** ](#secure-registration) nedan.
4. Om du vill hello datorer tooreport DSC status information tooAzure Automation DSC, men inte pull-konfiguration eller PowerShell-moduler, ange hello **ReportOnly** parametern tootrue.
5. Kör skriptet hello. Du bör nu ha en mapp med namnet **DscMetaConfigs** arbetskatalogen med hello PowerShell DSC metaconfigurations för hello datorer tooonboard (som administratör):

    ```powershell
    Set-DscLocalConfigurationManager -Path ./DscMetaConfigs
    ```

### <a name="using-hello-azure-automation-cmdlets"></a>Med hjälp av hello Azure Automation-cmdlets

Om hello PowerShell DSC Local Configuration Manager standardvärden matchar din användningsfall och du vill att tooonboard datorerna så att de hämtar från såväl rapportera tooAzure Automation DSC, ger hello Azure Automation cmdlets en förenklad metoden för att skapa hello DSC metaconfigurations behövs:

1. Öppna hello PowerShell-konsolen eller PowerShell ISE som administratör på en dator i din lokala miljö.
2. Ansluta tooAzure Resource Manager med **Add-AzureRmAccount**
3. Hämta hello PowerShell DSC metaconfigurations för hello-datorer som du vill använda tooonboard från hello Automation-kontot toowhich som du vill tooonboard noder:

    ```powershell
    # Define hello parameters for Get-AzureRmAutomationDscOnboardingMetaconfig using PowerShell Splatting
    $Params = @{

        ResourceGroupName = 'ContosoResources'; # hello name of hello ARM Resource Group that contains your Azure Automation Account
        AutomationAccountName = 'ContosoAutomation'; # hello name of hello Azure Automation Account where you want a node on-boarded to
        ComputerName = @('web01', 'web02', 'sql01'); # hello names of hello computers that hello meta configuration will be generated for
        OutputFolder = "$env:UserProfile\Desktop\";
    }
    # Use PowerShell splatting toopass parameters toohello Azure Automation cmdlet being invoked
    # For more info about splatting, run: Get-Help -Name about_Splatting
    Get-AzureRmAutomationDscOnboardingMetaconfig @Params
    ```
    
4. Du bör nu ha en mapp med namnet ***DscMetaConfigs***, som innehåller hello PowerShell DSC metaconfigurations för hello datorer tooonboard (som administratör):
    
    ```powershell
    Set-DscLocalConfigurationManager -Path $env:UserProfile\Desktop\DscMetaConfigs
    ```

## <a name="secure-registration"></a>Säker registrering

Datorer kan på ett säkert sätt publicera tooan Azure Automation-konto via hello WMF 5 DSC registrering protokollet, vilket låter en DSC-nod tooauthenticate tooa PowerShell DSC V2 Pull eller Reporting server (inklusive Azure Automation DSC). hello nod registrerar toohello server på en **URL: en registrering**, autentiseras med hjälp av en **registreringsnyckel**. Under registreringen förhandla hello DSC-nod och DSC Pull/rapportserver ett unikt certifikat för den här noden toouse för autentisering toohello server efter registreringen. Den här processen förhindrar publicerats så noder från personifiera en annan, till exempel om en nod har komprometterats och uppför medvetet. Efter registreringen hello registreringsnyckel används inte för autentisering igen och tas bort från hello-nod.

Du kan hämta hello information som krävs för protokollet för hello DSC-registreringen från hello **hantera nycklar** bladet i hello Azure preview portal. Öppna bladet genom att klicka på nyckelikonen för hello på hello **Essentials** för hello Automation-konto.

![](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

* URL: en registrering är hello URL-fältet i hello hantera nycklar bladet.
* Registreringsnyckel är hello primärnyckel åtkomst eller sekundära åtkomstnyckeln i bladet för hello hantera nycklar. Antingen nyckeln kan användas.

För extra säkerhet kan hello primära och sekundära åtkomstnycklar för ett Automation-konto kan genereras när som helst (på hello **hantera nycklar** bladet) tooprevent framtida nod registreringar med tidigare nycklar.

## <a name="troubleshooting-azure-virtual-machine-onboarding"></a>Felsökning av virtuell dator i Azure-onboarding

Azure Automation DSC kan du enkelt inbyggda Windows Azure-datorer för konfigurationshantering. Under hello tak är hello Azure VM Desired State Configuration-tillägget används tooregister hello virtuell dator med Azure Automation DSC. Eftersom hello Azure VM Desired State Configuration-tillägg körs asynkront, kan spåra förloppet och felsökning av körningen vara viktigt.

> [!NOTE]
> En metod för onboarding som en virtuell Azure Windows-dator tooAzure Automation DSC som använder hello Azure VM Desired State Configuration-tillägget kan ta upp tooan timme för hello nod tooshow upp som har registrerats i Azure Automation. Detta är på grund av toohello installationen av Windows Management Framework 5.0 på hello VM av hello Azure VM DSC-tillägg som är nödvändiga tooonboard hello VM tooAzure Automation DSC.

tootroubleshoot eller visa hello status hello Azure VM Desired State Configuration-tillägget i hello Azure-portalen navigera toohello VM som publicerats så, sedan klickar du på -> **alla inställningar** -> **tillägg**   ->  **DSC**. Mer information kan du klicka på **visa detaljerad statusinformation om**.

[![](./media/automation-dsc-onboarding/DSC_Onboarding_5.png)](https://technet.microsoft.com/library/dn249912.aspx)

## <a name="certificate-expiration-and-reregistration"></a>Certifikatet upphör att gälla och omregistrering

När du registrerar en dator som en DSC-nod i Azure Automation DSC, finns det flera skäl till varför du kanske behöver tooreregister noden i hello framtida:

* Varje nod förhandlar automatiskt ett unikt certifikat för autentisering som upphör att gälla efter ett år efter registrering. För närvarande hello PowerShell DSC-registreringen protokollet kan inte automatiskt att förnya certifikat när de närmar sig förfallodatum, så du måste tooreregister hello noder efter ett år tid. Innan du registrerar om, kontrollerar du att varje nod körs Windows Management Framework 5.0 RTM. Om en nod Autentiseringscertifikatet upphör att gälla och hello noden inte återregistrerade, hello noden toocommunicate med Azure Automation och markeras Unresponsive. Omregistreringen utförs 90 dagar eller mindre från hello certifikatets förfallotid eller när som helst efter hello certifikatets förfallotid leder till ett nytt certifikat som genereras och används.
* toochange alla [PowerShell DSC Local Configuration Manager värden](https://msdn.microsoft.com/powershell/dsc/metaconfig4) som angavs under registreringen av hello-nod, till exempel ConfigurationMode. För närvarande kan värdena DSC-agenten endast ändras via omregistreringen. hello ett undantag är hello nodkonfiguration tilldelade toohello nod – här kan ändras i Azure Automation DSC direkt.

Omregistreringen kan utföras i hello samma sätt som du har registrerat hello nod först använda någon av hello onboarding metoder som beskrivs i det här dokumentet. Du behöver inte toounregister en nod från Azure Automation DSC innan du registrerar om den.

## <a name="related-articles"></a>Relaterade artiklar

* [Översikt över Azure Automation DSC](automation-dsc-overview.md)
* [Azure Automation DSC-cmdlets](/powershell/module/azurerm.automation/#automation)
* [Priser för Azure Automation DSC](https://azure.microsoft.com/pricing/details/automation/)
