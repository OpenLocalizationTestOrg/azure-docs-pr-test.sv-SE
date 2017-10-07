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
# <a name="onboarding-machines-for-management-by-azure-automation-dsc"></a><span data-ttu-id="4ec46-103">Onboarding-datorer för hantering av Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="4ec46-103">Onboarding machines for management by Azure Automation DSC</span></span>

## <a name="why-manage-machines-with-azure-automation-dsc"></a><span data-ttu-id="4ec46-104">Varför hantera datorer med Azure Automation DSC?</span><span class="sxs-lookup"><span data-stu-id="4ec46-104">Why manage machines with Azure Automation DSC?</span></span>

<span data-ttu-id="4ec46-105">Som [PowerShell Desired State Configuration](https://technet.microsoft.com/library/dn249912.aspx), Azure Automation Desired State Configuration är en enkel men kraftfull, configuration management-tjänst för DSC-noder (fysiska och virtuella datorer) i molnet eller lokalt Datacenter.</span><span class="sxs-lookup"><span data-stu-id="4ec46-105">Like [PowerShell Desired State Configuration](https://technet.microsoft.com/library/dn249912.aspx), Azure Automation Desired State Configuration is a simple, yet powerful, configuration management service for DSC nodes (physical and virtual machines) in any cloud or on-premises datacenter.</span></span> <span data-ttu-id="4ec46-106">Den möjliggör skalbarhet över tusentals datorer snabbt och enkelt från en central, säker plats.</span><span class="sxs-lookup"><span data-stu-id="4ec46-106">It enables scalability across thousands of machines quickly and easily from a central, secure location.</span></span> <span data-ttu-id="4ec46-107">Du kan enkelt publicera datorer, tilldela dem deklarativ konfigurationer och visa rapporter som visar var datorn toohello önskad kompatibilitetstillstånd du angett.</span><span class="sxs-lookup"><span data-stu-id="4ec46-107">You can easily onboard machines, assign them declarative configurations, and view reports showing each machine's compliance toohello desired state you specified.</span></span> <span data-ttu-id="4ec46-108">hello Azure Automation DSC management lagret är tooDSC vilka hello Azure Automation-hanteringslager är tooPowerShell skript.</span><span class="sxs-lookup"><span data-stu-id="4ec46-108">hello Azure Automation DSC management layer is tooDSC what hello Azure Automation management layer is tooPowerShell scripting.</span></span> <span data-ttu-id="4ec46-109">Med andra ord i hello samma sätt som med hjälp av Azure Automation kan du hantera PowerShell-skript, du kan också hantera DSC-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="4ec46-109">In other words, in hello same way that Azure Automation helps you manage PowerShell scripts, it also helps you manage DSC configurations.</span></span> <span data-ttu-id="4ec46-110">toolearn mer om hello fördelarna med att använda Azure Automation DSC finns [översikt över Azure Automation DSC](automation-dsc-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4ec46-110">toolearn more about hello benefits of using Azure Automation DSC, see [Azure Automation DSC overview](automation-dsc-overview.md).</span></span>

<span data-ttu-id="4ec46-111">Azure Automation DSC kan vara används toomanage olika datorer:</span><span class="sxs-lookup"><span data-stu-id="4ec46-111">Azure Automation DSC can be used toomanage a variety of machines:</span></span>

* <span data-ttu-id="4ec46-112">Virtuella Azure-datorer (klassisk)</span><span class="sxs-lookup"><span data-stu-id="4ec46-112">Azure virtual machines (classic)</span></span>
* <span data-ttu-id="4ec46-113">Virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="4ec46-113">Azure virtual machines</span></span>
* <span data-ttu-id="4ec46-114">Amazon Web Services (AWS) virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="4ec46-114">Amazon Web Services (AWS) virtual machines</span></span>
* <span data-ttu-id="4ec46-115">Fysisk virtuell Windows-datorer lokalt eller i ett moln än Azure/AWS</span><span class="sxs-lookup"><span data-stu-id="4ec46-115">Physical/virtual Windows machines on-premises, or in a cloud other than Azure/AWS</span></span>
* <span data-ttu-id="4ec46-116">Fysisk virtuell Linux-datorer lokalt, i Azure eller i ett moln än Azure</span><span class="sxs-lookup"><span data-stu-id="4ec46-116">Physical/virtual Linux machines on-premises, in Azure, or in a cloud other than Azure</span></span>

<span data-ttu-id="4ec46-117">Dessutom om du inte är redo toomanage datorkonfigurationen från molnet hello kan Azure Automation DSC också användas som en endast rapport-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="4ec46-117">In addition, if you are not ready toomanage machine configuration from hello cloud, Azure Automation DSC can also be used as a report-only endpoint.</span></span> <span data-ttu-id="4ec46-118">Detta kan du tooset (push) önskad konfiguration via DSC lokala och visa information om omfattande reporting på noden följer hello önskad status i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="4ec46-118">This allows you tooset (push) desired configuration through DSC on-premises and view rich reporting details on node compliance with hello desired state in Azure Automation.</span></span>

<span data-ttu-id="4ec46-119">hello följande avsnitt beskriver hur du kan publicera varje typ av dator tooAzure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="4ec46-119">hello following sections outline how you can onboard each type of machine tooAzure Automation DSC.</span></span>

## <a name="azure-virtual-machines-classic"></a><span data-ttu-id="4ec46-120">Virtuella Azure-datorer (klassisk)</span><span class="sxs-lookup"><span data-stu-id="4ec46-120">Azure virtual machines (classic)</span></span>

<span data-ttu-id="4ec46-121">Med Azure Automation DSC kan du enkelt publicera virtuella Azure-datorer (klassisk) för konfigurationshantering med hello Azure-portalen eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4ec46-121">With Azure Automation DSC, you can easily onboard Azure virtual machines (classic) for configuration management using either hello Azure portal, or PowerShell.</span></span> <span data-ttu-id="4ec46-122">Under huven hello och en administratör med tooremote i hello VM registrerar hello Azure VM Desired State Configuration-tillägget hello VM med Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="4ec46-122">Under hello hood, and without an administrator having tooremote into hello VM, hello Azure VM Desired State Configuration extension registers hello VM with Azure Automation DSC.</span></span> <span data-ttu-id="4ec46-123">Eftersom hello Azure VM Desired State Configuration-tillägg körs asynkront, steg tootrack förloppet eller felsöka den finns i hello [ **felsöka Azure virtuella onboarding** ](#troubleshooting-azure-virtual-machine-onboarding)nedan.</span><span class="sxs-lookup"><span data-stu-id="4ec46-123">Since hello Azure VM Desired State Configuration extension runs asynchronously, steps tootrack its progress or troubleshoot it are provided in hello [**Troubleshooting Azure virtual machine onboarding**](#troubleshooting-azure-virtual-machine-onboarding) section below.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="4ec46-124">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4ec46-124">Azure portal</span></span>

<span data-ttu-id="4ec46-125">I hello [Azure-portalen](http://portal.azure.com/), klickar du på **Bläddra** -> **virtuella datorer (klassisk)**.</span><span class="sxs-lookup"><span data-stu-id="4ec46-125">In hello [Azure portal](http://portal.azure.com/), click **Browse** -> **Virtual machines (classic)**.</span></span> <span data-ttu-id="4ec46-126">Välj hello Windows VM som du vill tooonboard.</span><span class="sxs-lookup"><span data-stu-id="4ec46-126">Select hello Windows VM you want tooonboard.</span></span> <span data-ttu-id="4ec46-127">På bladet instrumentpanelen hello virtuell dator klickar du på **alla inställningar** -> **tillägg** -> **Lägg till**  ->   **Azure Automation DSC** -> **skapa**.</span><span class="sxs-lookup"><span data-stu-id="4ec46-127">On hello virtual machine's dashboard blade, click **All settings** -> **Extensions** -> **Add** -> **Azure Automation DSC** -> **Create**.</span></span> <span data-ttu-id="4ec46-128">Ange hello [PowerShell DSC Local Configuration Manager värden](https://msdn.microsoft.com/powershell/dsc/metaconfig4) krävs för din användningsfall registreringsnyckel ditt Automation-konto och URL: en registrering och eventuellt en nod configuration tooassign toohello VM.</span><span class="sxs-lookup"><span data-stu-id="4ec46-128">Enter hello [PowerShell DSC Local Configuration Manager values](https://msdn.microsoft.com/powershell/dsc/metaconfig4) required for your use case, your Automation account's registration key and registration URL, and optionally a node configuration tooassign toohello VM.</span></span>

![](./media/automation-dsc-onboarding/DSC_Onboarding_1.png)

<span data-ttu-id="4ec46-129">URL: en för registrering av toofind hello och nyckeln för hello Automation-konto tooonboard hello dator, se hello [ **Secure registrering** ](#secure-registration) nedan.</span><span class="sxs-lookup"><span data-stu-id="4ec46-129">toofind hello registration URL and key for hello Automation account tooonboard hello machine to, see hello [**Secure registration**](#secure-registration) section below.</span></span>

### <a name="powershell"></a><span data-ttu-id="4ec46-130">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4ec46-130">PowerShell</span></span>

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

## <a name="azure-virtual-machines"></a><span data-ttu-id="4ec46-131">Virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="4ec46-131">Azure virtual machines</span></span>

<span data-ttu-id="4ec46-132">Azure Automation DSC kan du enkelt publicera virtuella Azure-datorer för konfigurationshantering, med hello Azure-portalen, Azure Resource Manager-mallar eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4ec46-132">Azure Automation DSC lets you easily onboard Azure virtual machines for configuration management, using either hello Azure portal, Azure Resource Manager templates, or PowerShell.</span></span> <span data-ttu-id="4ec46-133">Under huven hello och en administratör med tooremote i hello VM registrerar hello Azure VM Desired State Configuration-tillägget hello VM med Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="4ec46-133">Under hello hood, and without an administrator having tooremote into hello VM, hello Azure VM Desired State Configuration extension registers hello VM with Azure Automation DSC.</span></span> <span data-ttu-id="4ec46-134">Eftersom hello Azure VM Desired State Configuration-tillägg körs asynkront, steg tootrack förloppet eller felsöka den finns i hello [ **felsöka Azure virtuella onboarding** ](#troubleshooting-azure-virtual-machine-onboarding)nedan.</span><span class="sxs-lookup"><span data-stu-id="4ec46-134">Since hello Azure VM Desired State Configuration extension runs asynchronously, steps tootrack its progress or troubleshoot it are provided in hello [**Troubleshooting Azure virtual machine onboarding**](#troubleshooting-azure-virtual-machine-onboarding) section below.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="4ec46-135">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4ec46-135">Azure portal</span></span>

<span data-ttu-id="4ec46-136">I hello [Azure-portalen](https://portal.azure.com/), navigera toohello Azure Automation-konto där du vill tooonboard virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="4ec46-136">In hello [Azure portal](https://portal.azure.com/), navigate toohello Azure Automation account where you want tooonboard virtual machines.</span></span> <span data-ttu-id="4ec46-137">Klicka på instrumentpanelen för hello Automation-konto, **DSC-noder** -> **lägga till Azure VM**.</span><span class="sxs-lookup"><span data-stu-id="4ec46-137">On hello Automation account dashboard, click **DSC Nodes** -> **Add Azure VM**.</span></span>

<span data-ttu-id="4ec46-138">Under **Välj virtuella datorer tooonboard**, Välj en eller flera Azure virtuella datorer tooonboard.</span><span class="sxs-lookup"><span data-stu-id="4ec46-138">Under **Select virtual machines tooonboard**, select one or more Azure virtual machines tooonboard.</span></span>

![](./media/automation-dsc-onboarding/DSC_Onboarding_2.png)

<span data-ttu-id="4ec46-139">Under **konfigurera registreringsdata**, ange hello [PowerShell DSC Local Configuration Manager värden](https://msdn.microsoft.com/powershell/dsc/metaconfig4) krävs för din användningsfall och eventuellt en nod configuration tooassign toohello VM.</span><span class="sxs-lookup"><span data-stu-id="4ec46-139">Under **Configure registration data**, enter hello [PowerShell DSC Local Configuration Manager values](https://msdn.microsoft.com/powershell/dsc/metaconfig4) required for your use case, and optionally a node configuration tooassign toohello VM.</span></span>

![](./media/automation-dsc-onboarding/DSC_Onboarding_3.png)

### <a name="azure-resource-manager-templates"></a><span data-ttu-id="4ec46-140">Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="4ec46-140">Azure Resource Manager templates</span></span>

<span data-ttu-id="4ec46-141">Virtuella Azure-datorer kan distribueras och publicerats så tooAzure Automation DSC via Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="4ec46-141">Azure virtual machines can be deployed and onboarded tooAzure Automation DSC via Azure Resource Manager templates.</span></span> <span data-ttu-id="4ec46-142">Se [konfigurera en virtuell dator via DSC-tillägg och Azure Automation DSC](https://azure.microsoft.com/documentation/templates/dsc-extension-azure-automation-pullserver/) för en exempelmall som onboards en befintlig virtuell dator tooAzure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="4ec46-142">See [Configure a VM via DSC extension and Azure Automation DSC](https://azure.microsoft.com/documentation/templates/dsc-extension-azure-automation-pullserver/) for an example template that onboards an existing VM tooAzure Automation DSC.</span></span> <span data-ttu-id="4ec46-143">toofind hello nyckel och registrering URL: en registrering vidtas som indata i den här mallen finns hello [ **Secure registrering** ](#secure-registration) nedan.</span><span class="sxs-lookup"><span data-stu-id="4ec46-143">toofind hello registration key and registration URL taken as input in this template, see hello [**Secure registration**](#secure-registration) section below.</span></span>

### <a name="powershell"></a><span data-ttu-id="4ec46-144">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4ec46-144">PowerShell</span></span>

<span data-ttu-id="4ec46-145">Hej [registrera AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode) används tooonboard virtuella datorer i hello Azure-portalen via PowerShell kan du vara.</span><span class="sxs-lookup"><span data-stu-id="4ec46-145">hello [Register-AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode) cmdlet can be used tooonboard virtual machines in hello Azure portal via PowerShell.</span></span>

## <a name="amazon-web-services-aws-virtual-machines"></a><span data-ttu-id="4ec46-146">Amazon Web Services (AWS) virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="4ec46-146">Amazon Web Services (AWS) virtual machines</span></span>

<span data-ttu-id="4ec46-147">Du kan enkelt publicera Amazon Web Services virtuella datorer för konfigurationshantering av Azure Automation DSC med hello AWS DSC Toolkit.</span><span class="sxs-lookup"><span data-stu-id="4ec46-147">You can easily onboard Amazon Web Services virtual machines for configuration management by Azure Automation DSC using hello AWS DSC Toolkit.</span></span> <span data-ttu-id="4ec46-148">Du kan lära dig mer om hello toolkit [här](https://blogs.msdn.microsoft.com/powershell/2016/04/20/aws-dsc-toolkit/).</span><span class="sxs-lookup"><span data-stu-id="4ec46-148">You can learn more about hello toolkit [here](https://blogs.msdn.microsoft.com/powershell/2016/04/20/aws-dsc-toolkit/).</span></span>

## <a name="physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azureaws"></a><span data-ttu-id="4ec46-149">Fysisk virtuell Windows-datorer lokalt eller i ett moln än Azure/AWS</span><span class="sxs-lookup"><span data-stu-id="4ec46-149">Physical/virtual Windows machines on-premises, or in a cloud other than Azure/AWS</span></span>

<span data-ttu-id="4ec46-150">Lokalt Windows-datorer och Windows-datorer i Azure-moln (till exempel Amazon Web Services) kan också vara publicerats så tooAzure Automation DSC, så länge som de har utgående åtkomst toohello internet, via några enkla steg:</span><span class="sxs-lookup"><span data-stu-id="4ec46-150">On-premises Windows machines and Windows machines in non-Azure clouds (such as Amazon Web Services) can also be onboarded tooAzure Automation DSC, as long as they have outbound access toohello internet, via a few simple steps:</span></span>

1. <span data-ttu-id="4ec46-151">Se till att hello senaste versionen av [WMF 5](http://aka.ms/wmf5latest) är installerad på hello datorer du vill tooonboard tooAzure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="4ec46-151">Make sure hello latest version of [WMF 5](http://aka.ms/wmf5latest) is installed on hello machines you want tooonboard tooAzure Automation DSC.</span></span>
2. <span data-ttu-id="4ec46-152">Följ hello anvisningarna i avsnittet [ **genererar DSC metaconfigurations** ](#generating-dsc-metaconfigurations) nedan toogenerate behövs en mapp som innehåller hello DSC metaconfigurations.</span><span class="sxs-lookup"><span data-stu-id="4ec46-152">Follow hello directions in section [**Generating DSC metaconfigurations**](#generating-dsc-metaconfigurations) below toogenerate a folder containing hello needed DSC metaconfigurations.</span></span>
3. <span data-ttu-id="4ec46-153">Tillämpa hello PowerShell DSC metakonfigurationen toohello datorer du vill tooonboard via fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="4ec46-153">Remotely apply hello PowerShell DSC metaconfiguration toohello machines you want tooonboard.</span></span> <span data-ttu-id="4ec46-154">**hello-datorn det här kommandot körs från måste ha hello senaste versionen av [WMF 5](http://aka.ms/wmf5latest) installerat**:</span><span class="sxs-lookup"><span data-stu-id="4ec46-154">**hello machine this command is run from must have hello latest version of [WMF 5](http://aka.ms/wmf5latest) installed**:</span></span>

    ```powershell
    Set-DscLocalConfigurationManager -Path C:\Users\joe\Desktop\DscMetaConfigs -ComputerName MyServer1, MyServer2
    ```

4. <span data-ttu-id="4ec46-155">Om du inte använder hello PowerShell DSC metaconfigurations via fjärranslutning, kopiera hello metaconfigurations mappen från steg 2 till varje dator tooonboard.</span><span class="sxs-lookup"><span data-stu-id="4ec46-155">If you cannot apply hello PowerShell DSC metaconfigurations remotely, copy hello metaconfigurations folder from step 2 onto each machine tooonboard.</span></span> <span data-ttu-id="4ec46-156">Sedan anropar **Set DscLocalConfigurationManager** lokalt på varje dator tooonboard.</span><span class="sxs-lookup"><span data-stu-id="4ec46-156">Then call **Set-DscLocalConfigurationManager** locally on each machine tooonboard.</span></span>
5. <span data-ttu-id="4ec46-157">Med hello Azure-portalen eller cmdletar måste du kontrollera att hello datorer tooonboard nu visas som DSC-noder som är registrerade i Azure Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="4ec46-157">Using hello Azure portal or cmdlets, check that hello machines tooonboard now show up as DSC nodes registered in your Azure Automation account.</span></span>

## <a name="physicalvirtual-linux-machines-on-premises-in-azure-or-in-a-cloud-other-than-azure"></a><span data-ttu-id="4ec46-158">Fysisk virtuell Linux-datorer lokalt, i Azure eller i ett moln än Azure</span><span class="sxs-lookup"><span data-stu-id="4ec46-158">Physical/virtual Linux machines on-premises, in Azure, or in a cloud other than Azure</span></span>

<span data-ttu-id="4ec46-159">Lokal Linux-datorer, Linux-datorer i Azure, och Linux-datorer i Azure-moln kan också vara publicerats så tooAzure Automation DSC, så länge som de har utgående åtkomst toohello internet, via några enkla steg:</span><span class="sxs-lookup"><span data-stu-id="4ec46-159">On-premises Linux machines, Linux machines in Azure, and Linux machines in non-Azure clouds can also be onboarded tooAzure Automation DSC, as long as they have outbound access toohello internet, via a few simple steps:</span></span>

1. <span data-ttu-id="4ec46-160">Se till att hello senaste versionen av [PowerShell Desired State Configuration för Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) är installerad på hello datorer du vill tooonboard tooAzure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="4ec46-160">Make sure hello latest version of [PowerShell Desired State Configuration for Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) is installed on hello machines you want tooonboard tooAzure Automation DSC.</span></span>
2. <span data-ttu-id="4ec46-161">Om hello [PowerShell DSC Local Configuration Manager standardvärden](https://msdn.microsoft.com/powershell/dsc/metaconfig4) matchar din användningsfall och du vill tooonboard datorer så att de **både** hämtar från och rapportera tooAzure Automation DSC:</span><span class="sxs-lookup"><span data-stu-id="4ec46-161">If hello [PowerShell DSC Local Configuration Manager defaults](https://msdn.microsoft.com/powershell/dsc/metaconfig4) match your use case, and you want tooonboard machines such that they **both** pull from and report tooAzure Automation DSC:</span></span>

   + <span data-ttu-id="4ec46-162">Använd Register.py tooonboard med hello PowerShell DSC Local Configuration Manager som standard på varje Linux datorn tooonboard tooAzure Automation DSC:</span><span class="sxs-lookup"><span data-stu-id="4ec46-162">On each Linux machine tooonboard tooAzure Automation DSC, use Register.py tooonboard using hello PowerShell DSC Local Configuration Manager defaults:</span></span>

     `/opt/microsoft/dsc/Scripts/Register.py <Automation account registration key> <Automation account registration URL>`

   + <span data-ttu-id="4ec46-163">toofind hello nyckel och registrering URL: en registrering för Automation-kontot finns hello [ **Secure registrering** ](#secure-registration) nedan.</span><span class="sxs-lookup"><span data-stu-id="4ec46-163">toofind hello registration key and registration URL for your Automation account, see hello [**Secure registration**](#secure-registration) section below.</span></span>

     <span data-ttu-id="4ec46-164">Om hello PowerShell DSC Local Configuration Manager som standard **gör** **inte** matchar din användningsfall eller om du vill tooonboard datorer så att de endast rapportera tooAzure Automation DSC, men inte hämtar konfiguration eller PowerShell-moduler från den, följer du steg 3-6.</span><span class="sxs-lookup"><span data-stu-id="4ec46-164">If hello PowerShell DSC Local Configuration Manager defaults **do** **not** match your use case, or you want tooonboard machines such that they only report tooAzure Automation DSC, but do not pull configuration or PowerShell modules from it,  follow steps 3 - 6.</span></span> <span data-ttu-id="4ec46-165">Annars Fortsätt direkt toostep 6.</span><span class="sxs-lookup"><span data-stu-id="4ec46-165">Otherwise, proceed directly toostep 6.</span></span>

3. <span data-ttu-id="4ec46-166">Följ anvisningarna hello i hello [ **genererar DSC metaconfigurations** ](#generating-dsc-metaconfigurations) avsnittet nedan toogenerate en mapp som innehåller hello behövs DSC metaconfigurations.</span><span class="sxs-lookup"><span data-stu-id="4ec46-166">Follow hello directions in hello [**Generating DSC metaconfigurations**](#generating-dsc-metaconfigurations) section below toogenerate a folder containing hello needed DSC metaconfigurations.</span></span>
4. <span data-ttu-id="4ec46-167">Via fjärranslutning gäller hello PowerShell DSC metakonfigurationen toohello datorer du vill tooonboard:</span><span class="sxs-lookup"><span data-stu-id="4ec46-167">Remotely apply hello PowerShell DSC metaconfiguration toohello machines you want tooonboard:</span></span>

    ```powershell
    $SecurePass = ConvertTo-SecureString -String "<root password>" -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential "root", $SecurePass
    $Opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

    # need a CimSession for each Linux machine tooonboard

    $Session = New-CimSession -Credential $Cred -ComputerName <your Linux machine> -Port 5986 -Authentication basic -SessionOption $Opt

    Set-DscLocalConfigurationManager -CimSession $Session -Path C:\Users\joe\Desktop\DscMetaConfigs
    ```

<span data-ttu-id="4ec46-168">hello-datorn det här kommandot körs från måste ha hello senaste versionen av [WMF 5](http://aka.ms/wmf5latest) installerad.</span><span class="sxs-lookup"><span data-stu-id="4ec46-168">hello machine this command is run from must have hello latest version of [WMF 5](http://aka.ms/wmf5latest) installed.</span></span>

1. <span data-ttu-id="4ec46-169">Om du inte kan använda hello PowerShell DSC metaconfigurations via fjärranslutning, för varje dator tooonboard Linux kopiera hello metakonfigurationen motsvarande toothat dator från hello mapp i steg 5 till hello Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="4ec46-169">If you cannot apply hello PowerShell DSC metaconfigurations remotely, for each Linux machine tooonboard, copy hello metaconfiguration corresponding toothat machine from hello folder in step 5 onto hello Linux machine.</span></span> <span data-ttu-id="4ec46-170">Sedan anropar `SetDscLocalConfigurationManager.py` lokalt på varje Linux-dator du vill tooonboard tooAzure Automation DSC:</span><span class="sxs-lookup"><span data-stu-id="4ec46-170">Then call `SetDscLocalConfigurationManager.py` locally on each Linux machine you want tooonboard tooAzure Automation DSC:</span></span>

   `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py -configurationmof <path toometaconfiguration file>`

2. <span data-ttu-id="4ec46-171">Med hello Azure-portalen eller cmdletar måste du kontrollera att hello datorer tooonboard nu visas som DSC-noder som är registrerade i Azure Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="4ec46-171">Using hello Azure portal or cmdlets, check that hello machines tooonboard now show up as DSC nodes registered in your Azure Automation account.</span></span>

## <a name="generating-dsc-metaconfigurations"></a><span data-ttu-id="4ec46-172">Generera DSC metaconfigurations</span><span class="sxs-lookup"><span data-stu-id="4ec46-172">Generating DSC metaconfigurations</span></span>

<span data-ttu-id="4ec46-173">toogenerically publicera någon datorn tooAzure Automation DSC, en [DSC-metakonfigurationen](https://msdn.microsoft.com/en-us/powershell/dsc/metaconfig) kan genereras som, när tillämpas, visar hello DSC-agenten på hello datorn toopull från och/eller rapportera tooAzure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="4ec46-173">toogenerically onboard any machine tooAzure Automation DSC, a [DSC metaconfiguration](https://msdn.microsoft.com/en-us/powershell/dsc/metaconfig) can be generated that, when applied, tells hello DSC agent on hello machine toopull from and/or report tooAzure Automation DSC.</span></span> <span data-ttu-id="4ec46-174">DSC-metaconfigurations för Azure Automation DSC kan genereras med hjälp av antingen en PowerShell DSC-konfiguration eller hello Azure Automation PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="4ec46-174">DSC metaconfigurations for Azure Automation DSC can be generated using either a PowerShell DSC configuration, or hello Azure Automation PowerShell cmdlets.</span></span>

> [!NOTE]
> <span data-ttu-id="4ec46-175">DSC-metaconfigurations innehålla hello hemligheter behövs tooonboard dator-tooan Automation-konto för hantering.</span><span class="sxs-lookup"><span data-stu-id="4ec46-175">DSC metaconfigurations contain hello secrets needed tooonboard a machine tooan Automation account for management.</span></span> <span data-ttu-id="4ec46-176">Se till att tooproperly skydda alla DSC-metaconfigurations som du skapar eller tar bort dem när du har använt.</span><span class="sxs-lookup"><span data-stu-id="4ec46-176">Make sure tooproperly protect any DSC metaconfigurations you create, or delete them after use.</span></span>

### <a name="using-a-dsc-configuration"></a><span data-ttu-id="4ec46-177">Med hjälp av DSC-konfiguration</span><span class="sxs-lookup"><span data-stu-id="4ec46-177">Using a DSC Configuration</span></span>

1. <span data-ttu-id="4ec46-178">Öppna hello PowerShell ISE som administratör på en dator i din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="4ec46-178">Open hello PowerShell ISE as an administrator in a machine in your local environment.</span></span> <span data-ttu-id="4ec46-179">hello datorn måste ha hello senaste versionen av [WMF 5](http://aka.ms/wmf5latest) installerad.</span><span class="sxs-lookup"><span data-stu-id="4ec46-179">hello machine must have hello latest version of [WMF 5](http://aka.ms/wmf5latest) installed.</span></span>
2. <span data-ttu-id="4ec46-180">Kopiera hello följande skript lokalt.</span><span class="sxs-lookup"><span data-stu-id="4ec46-180">Copy hello following script locally.</span></span> <span data-ttu-id="4ec46-181">Det här skriptet innehåller en PowerShell DSC-konfiguration för att skapa metaconfigurations och ett kommando tookick hello metakonfigurationen skapa.</span><span class="sxs-lookup"><span data-stu-id="4ec46-181">This script contains a PowerShell DSC configuration for creating metaconfigurations, and a command tookick off hello metaconfiguration creation.</span></span>

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

3. <span data-ttu-id="4ec46-182">Fyll i hello registreringsnyckel och URL: en för Automation-konto, samt hello namnen på hello datorer tooonboard.</span><span class="sxs-lookup"><span data-stu-id="4ec46-182">Fill in hello registration key and URL for your Automation account, as well as hello names of hello machines tooonboard.</span></span> <span data-ttu-id="4ec46-183">Alla andra parametrar är valfria.</span><span class="sxs-lookup"><span data-stu-id="4ec46-183">All other parameters are optional.</span></span> <span data-ttu-id="4ec46-184">toofind hello nyckel och registrering URL: en registrering för Automation-kontot finns hello [ **Secure registrering** ](#secure-registration) nedan.</span><span class="sxs-lookup"><span data-stu-id="4ec46-184">toofind hello registration key and registration URL for your Automation account, see hello [**Secure registration**](#secure-registration) section below.</span></span>
4. <span data-ttu-id="4ec46-185">Om du vill hello datorer tooreport DSC status information tooAzure Automation DSC, men inte pull-konfiguration eller PowerShell-moduler, ange hello **ReportOnly** parametern tootrue.</span><span class="sxs-lookup"><span data-stu-id="4ec46-185">If you want hello machines tooreport DSC status information tooAzure Automation DSC, but not pull configuration or PowerShell modules, set hello **ReportOnly** parameter tootrue.</span></span>
5. <span data-ttu-id="4ec46-186">Kör skriptet hello.</span><span class="sxs-lookup"><span data-stu-id="4ec46-186">Run hello script.</span></span> <span data-ttu-id="4ec46-187">Du bör nu ha en mapp med namnet **DscMetaConfigs** arbetskatalogen med hello PowerShell DSC metaconfigurations för hello datorer tooonboard (som administratör):</span><span class="sxs-lookup"><span data-stu-id="4ec46-187">You should now have a folder called **DscMetaConfigs** in your working directory, containing hello PowerShell DSC metaconfigurations for hello machines tooonboard (as an administrator):</span></span>

    ```powershell
    Set-DscLocalConfigurationManager -Path ./DscMetaConfigs
    ```

### <a name="using-hello-azure-automation-cmdlets"></a><span data-ttu-id="4ec46-188">Med hjälp av hello Azure Automation-cmdlets</span><span class="sxs-lookup"><span data-stu-id="4ec46-188">Using hello Azure Automation cmdlets</span></span>

<span data-ttu-id="4ec46-189">Om hello PowerShell DSC Local Configuration Manager standardvärden matchar din användningsfall och du vill att tooonboard datorerna så att de hämtar från såväl rapportera tooAzure Automation DSC, ger hello Azure Automation cmdlets en förenklad metoden för att skapa hello DSC metaconfigurations behövs:</span><span class="sxs-lookup"><span data-stu-id="4ec46-189">If hello PowerShell DSC Local Configuration Manager defaults match your use case, and you want tooonboard machines such that they both pull from and report tooAzure Automation DSC, hello Azure Automation cmdlets provide a simplified method of generating hello DSC metaconfigurations needed:</span></span>

1. <span data-ttu-id="4ec46-190">Öppna hello PowerShell-konsolen eller PowerShell ISE som administratör på en dator i din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="4ec46-190">Open hello PowerShell console or PowerShell ISE as an administrator in a machine in your local environment.</span></span>
2. <span data-ttu-id="4ec46-191">Ansluta tooAzure Resource Manager med **Add-AzureRmAccount**</span><span class="sxs-lookup"><span data-stu-id="4ec46-191">Connect tooAzure Resource Manager using **Add-AzureRmAccount**</span></span>
3. <span data-ttu-id="4ec46-192">Hämta hello PowerShell DSC metaconfigurations för hello-datorer som du vill använda tooonboard från hello Automation-kontot toowhich som du vill tooonboard noder:</span><span class="sxs-lookup"><span data-stu-id="4ec46-192">Download hello PowerShell DSC metaconfigurations for hello machines you want tooonboard from hello Automation account toowhich you want tooonboard nodes:</span></span>

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
    
4. <span data-ttu-id="4ec46-193">Du bör nu ha en mapp med namnet ***DscMetaConfigs***, som innehåller hello PowerShell DSC metaconfigurations för hello datorer tooonboard (som administratör):</span><span class="sxs-lookup"><span data-stu-id="4ec46-193">You should now have a folder called ***DscMetaConfigs***, containing hello PowerShell DSC metaconfigurations for hello machines tooonboard (as an administrator):</span></span>
    
    ```powershell
    Set-DscLocalConfigurationManager -Path $env:UserProfile\Desktop\DscMetaConfigs
    ```

## <a name="secure-registration"></a><span data-ttu-id="4ec46-194">Säker registrering</span><span class="sxs-lookup"><span data-stu-id="4ec46-194">Secure registration</span></span>

<span data-ttu-id="4ec46-195">Datorer kan på ett säkert sätt publicera tooan Azure Automation-konto via hello WMF 5 DSC registrering protokollet, vilket låter en DSC-nod tooauthenticate tooa PowerShell DSC V2 Pull eller Reporting server (inklusive Azure Automation DSC).</span><span class="sxs-lookup"><span data-stu-id="4ec46-195">Machines can securely onboard tooan Azure Automation account through hello WMF 5 DSC registration protocol, which allows a DSC node tooauthenticate tooa PowerShell DSC V2 Pull or Reporting server (including Azure Automation DSC).</span></span> <span data-ttu-id="4ec46-196">hello nod registrerar toohello server på en **URL: en registrering**, autentiseras med hjälp av en **registreringsnyckel**.</span><span class="sxs-lookup"><span data-stu-id="4ec46-196">hello node registers toohello server at a **Registration URL**, authenticating using a **Registration key**.</span></span> <span data-ttu-id="4ec46-197">Under registreringen förhandla hello DSC-nod och DSC Pull/rapportserver ett unikt certifikat för den här noden toouse för autentisering toohello server efter registreringen.</span><span class="sxs-lookup"><span data-stu-id="4ec46-197">During registration, hello DSC node and DSC Pull/Reporting server negotiate a unique certificate for this node toouse for authentication toohello server post-registration.</span></span> <span data-ttu-id="4ec46-198">Den här processen förhindrar publicerats så noder från personifiera en annan, till exempel om en nod har komprometterats och uppför medvetet.</span><span class="sxs-lookup"><span data-stu-id="4ec46-198">This process prevents onboarded nodes from impersonating one another, such as if a node is compromised and behaving maliciously.</span></span> <span data-ttu-id="4ec46-199">Efter registreringen hello registreringsnyckel används inte för autentisering igen och tas bort från hello-nod.</span><span class="sxs-lookup"><span data-stu-id="4ec46-199">After registration, hello Registration key is not used for authentication again, and is deleted from hello node.</span></span>

<span data-ttu-id="4ec46-200">Du kan hämta hello information som krävs för protokollet för hello DSC-registreringen från hello **hantera nycklar** bladet i hello Azure preview portal.</span><span class="sxs-lookup"><span data-stu-id="4ec46-200">You can get hello information required for hello DSC registration protocol from hello **Manage Keys** blade in hello Azure preview portal.</span></span> <span data-ttu-id="4ec46-201">Öppna bladet genom att klicka på nyckelikonen för hello på hello **Essentials** för hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="4ec46-201">Open this blade by clicking hello key icon on hello **Essentials** panel for hello Automation account.</span></span>

![](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

* <span data-ttu-id="4ec46-202">URL: en registrering är hello URL-fältet i hello hantera nycklar bladet.</span><span class="sxs-lookup"><span data-stu-id="4ec46-202">Registration URL is hello URL field in hello Manage Keys blade.</span></span>
* <span data-ttu-id="4ec46-203">Registreringsnyckel är hello primärnyckel åtkomst eller sekundära åtkomstnyckeln i bladet för hello hantera nycklar.</span><span class="sxs-lookup"><span data-stu-id="4ec46-203">Registration key is hello Primary Access Key or Secondary Access Key in hello Manage Keys blade.</span></span> <span data-ttu-id="4ec46-204">Antingen nyckeln kan användas.</span><span class="sxs-lookup"><span data-stu-id="4ec46-204">Either key can be used.</span></span>

<span data-ttu-id="4ec46-205">För extra säkerhet kan hello primära och sekundära åtkomstnycklar för ett Automation-konto kan genereras när som helst (på hello **hantera nycklar** bladet) tooprevent framtida nod registreringar med tidigare nycklar.</span><span class="sxs-lookup"><span data-stu-id="4ec46-205">For added security, hello primary and secondary access keys of an Automation account can be regenerated at any time (on hello **Manage Keys** blade) tooprevent future node registrations using previous keys.</span></span>

## <a name="troubleshooting-azure-virtual-machine-onboarding"></a><span data-ttu-id="4ec46-206">Felsökning av virtuell dator i Azure-onboarding</span><span class="sxs-lookup"><span data-stu-id="4ec46-206">Troubleshooting Azure virtual machine onboarding</span></span>

<span data-ttu-id="4ec46-207">Azure Automation DSC kan du enkelt inbyggda Windows Azure-datorer för konfigurationshantering.</span><span class="sxs-lookup"><span data-stu-id="4ec46-207">Azure Automation DSC lets you easily onboard Azure Windows VMs for configuration management.</span></span> <span data-ttu-id="4ec46-208">Under hello tak är hello Azure VM Desired State Configuration-tillägget används tooregister hello virtuell dator med Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="4ec46-208">Under hello hood, hello Azure VM Desired State Configuration extension is used tooregister hello VM with Azure Automation DSC.</span></span> <span data-ttu-id="4ec46-209">Eftersom hello Azure VM Desired State Configuration-tillägg körs asynkront, kan spåra förloppet och felsökning av körningen vara viktigt.</span><span class="sxs-lookup"><span data-stu-id="4ec46-209">Since hello Azure VM Desired State Configuration extension runs asynchronously, tracking its progress and troubleshooting its execution may be important.</span></span>

> [!NOTE]
> <span data-ttu-id="4ec46-210">En metod för onboarding som en virtuell Azure Windows-dator tooAzure Automation DSC som använder hello Azure VM Desired State Configuration-tillägget kan ta upp tooan timme för hello nod tooshow upp som har registrerats i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="4ec46-210">Any method of onboarding an Azure Windows VM tooAzure Automation DSC that uses hello Azure VM Desired State Configuration extension could take up tooan hour for hello node tooshow up as registered in Azure Automation.</span></span> <span data-ttu-id="4ec46-211">Detta är på grund av toohello installationen av Windows Management Framework 5.0 på hello VM av hello Azure VM DSC-tillägg som är nödvändiga tooonboard hello VM tooAzure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="4ec46-211">This is due toohello installation of Windows Management Framework 5.0 on hello VM by hello Azure VM DSC extension, which is required tooonboard hello VM tooAzure Automation DSC.</span></span>

<span data-ttu-id="4ec46-212">tootroubleshoot eller visa hello status hello Azure VM Desired State Configuration-tillägget i hello Azure-portalen navigera toohello VM som publicerats så, sedan klickar du på -> **alla inställningar** -> **tillägg**   ->  **DSC**.</span><span class="sxs-lookup"><span data-stu-id="4ec46-212">tootroubleshoot or view hello status of hello Azure VM Desired State Configuration extension, in hello Azure portal navigate toohello VM being onboarded, then click -> **All settings** -> **Extensions** -> **DSC**.</span></span> <span data-ttu-id="4ec46-213">Mer information kan du klicka på **visa detaljerad statusinformation om**.</span><span class="sxs-lookup"><span data-stu-id="4ec46-213">For more details, you can click **View detailed status**.</span></span>

[![](./media/automation-dsc-onboarding/DSC_Onboarding_5.png)](https://technet.microsoft.com/library/dn249912.aspx)

## <a name="certificate-expiration-and-reregistration"></a><span data-ttu-id="4ec46-214">Certifikatet upphör att gälla och omregistrering</span><span class="sxs-lookup"><span data-stu-id="4ec46-214">Certificate expiration and reregistration</span></span>

<span data-ttu-id="4ec46-215">När du registrerar en dator som en DSC-nod i Azure Automation DSC, finns det flera skäl till varför du kanske behöver tooreregister noden i hello framtida:</span><span class="sxs-lookup"><span data-stu-id="4ec46-215">After registering a machine as a DSC node in Azure Automation DSC, there are a number of reasons why you may need tooreregister that node in hello future:</span></span>

* <span data-ttu-id="4ec46-216">Varje nod förhandlar automatiskt ett unikt certifikat för autentisering som upphör att gälla efter ett år efter registrering.</span><span class="sxs-lookup"><span data-stu-id="4ec46-216">After registering, each node automatically negotiates a unique certificate for authentication that expires after one year.</span></span> <span data-ttu-id="4ec46-217">För närvarande hello PowerShell DSC-registreringen protokollet kan inte automatiskt att förnya certifikat när de närmar sig förfallodatum, så du måste tooreregister hello noder efter ett år tid.</span><span class="sxs-lookup"><span data-stu-id="4ec46-217">Currently, hello PowerShell DSC registration protocol cannot automatically renew certificates when they are nearing expiration, so you need tooreregister hello nodes after a year's time.</span></span> <span data-ttu-id="4ec46-218">Innan du registrerar om, kontrollerar du att varje nod körs Windows Management Framework 5.0 RTM.</span><span class="sxs-lookup"><span data-stu-id="4ec46-218">Before reregistering, ensure that each node is running Windows Management Framework 5.0 RTM.</span></span> <span data-ttu-id="4ec46-219">Om en nod Autentiseringscertifikatet upphör att gälla och hello noden inte återregistrerade, hello noden toocommunicate med Azure Automation och markeras Unresponsive.</span><span class="sxs-lookup"><span data-stu-id="4ec46-219">If a node's authentication certificate expires, and hello node is not reregistered, hello node will be unable toocommunicate with Azure Automation and will be marked 'Unresponsive.'</span></span> <span data-ttu-id="4ec46-220">Omregistreringen utförs 90 dagar eller mindre från hello certifikatets förfallotid eller när som helst efter hello certifikatets förfallotid leder till ett nytt certifikat som genereras och används.</span><span class="sxs-lookup"><span data-stu-id="4ec46-220">Reregistration performed 90 days or less from hello certificate expiration time, or at any point after hello certificate expiration time, will result in a new certificate being generated and used.</span></span>
* <span data-ttu-id="4ec46-221">toochange alla [PowerShell DSC Local Configuration Manager värden](https://msdn.microsoft.com/powershell/dsc/metaconfig4) som angavs under registreringen av hello-nod, till exempel ConfigurationMode.</span><span class="sxs-lookup"><span data-stu-id="4ec46-221">toochange any [PowerShell DSC Local Configuration Manager values](https://msdn.microsoft.com/powershell/dsc/metaconfig4) that were set during initial registration of hello node, such as ConfigurationMode.</span></span> <span data-ttu-id="4ec46-222">För närvarande kan värdena DSC-agenten endast ändras via omregistreringen.</span><span class="sxs-lookup"><span data-stu-id="4ec46-222">Currently, these DSC agent values can only be changed through reregistration.</span></span> <span data-ttu-id="4ec46-223">hello ett undantag är hello nodkonfiguration tilldelade toohello nod – här kan ändras i Azure Automation DSC direkt.</span><span class="sxs-lookup"><span data-stu-id="4ec46-223">hello one exception is hello Node Configuration assigned toohello node -- this can be changed in Azure Automation DSC directly.</span></span>

<span data-ttu-id="4ec46-224">Omregistreringen kan utföras i hello samma sätt som du har registrerat hello nod först använda någon av hello onboarding metoder som beskrivs i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="4ec46-224">Reregistration can be performed in hello same way you registered hello node initially, using any of hello onboarding methods described in this document.</span></span> <span data-ttu-id="4ec46-225">Du behöver inte toounregister en nod från Azure Automation DSC innan du registrerar om den.</span><span class="sxs-lookup"><span data-stu-id="4ec46-225">You do not need toounregister a node from Azure Automation DSC before reregistering it.</span></span>

## <a name="related-articles"></a><span data-ttu-id="4ec46-226">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="4ec46-226">Related Articles</span></span>

* [<span data-ttu-id="4ec46-227">Översikt över Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="4ec46-227">Azure Automation DSC overview</span></span>](automation-dsc-overview.md)
* [<span data-ttu-id="4ec46-228">Azure Automation DSC-cmdlets</span><span class="sxs-lookup"><span data-stu-id="4ec46-228">Azure Automation DSC cmdlets</span></span>](/powershell/module/azurerm.automation/#automation)
* [<span data-ttu-id="4ec46-229">Priser för Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="4ec46-229">Azure Automation DSC pricing</span></span>](https://azure.microsoft.com/pricing/details/automation/)
