---
title: "Onboarding-datorer för hantering av Azure Automation DSC | Microsoft Docs"
description: "Hur du ställer in datorer för hantering med Azure Automation DSC"
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
ms.openlocfilehash: cc9b1ea19b4e17374d47e12f970cb333a8051559
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="onboarding-machines-for-management-by-azure-automation-dsc"></a><span data-ttu-id="b0320-103">Onboarding-datorer för hantering av Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="b0320-103">Onboarding machines for management by Azure Automation DSC</span></span>

## <a name="why-manage-machines-with-azure-automation-dsc"></a><span data-ttu-id="b0320-104">Varför hantera datorer med Azure Automation DSC?</span><span class="sxs-lookup"><span data-stu-id="b0320-104">Why manage machines with Azure Automation DSC?</span></span>

<span data-ttu-id="b0320-105">Som [PowerShell Desired State Configuration](https://technet.microsoft.com/library/dn249912.aspx), Azure Automation Desired State Configuration är en enkel men kraftfull, configuration management-tjänst för DSC-noder (fysiska och virtuella datorer) i molnet eller lokalt Datacenter.</span><span class="sxs-lookup"><span data-stu-id="b0320-105">Like [PowerShell Desired State Configuration](https://technet.microsoft.com/library/dn249912.aspx), Azure Automation Desired State Configuration is a simple, yet powerful, configuration management service for DSC nodes (physical and virtual machines) in any cloud or on-premises datacenter.</span></span> <span data-ttu-id="b0320-106">Den möjliggör skalbarhet över tusentals datorer snabbt och enkelt från en central, säker plats.</span><span class="sxs-lookup"><span data-stu-id="b0320-106">It enables scalability across thousands of machines quickly and easily from a central, secure location.</span></span> <span data-ttu-id="b0320-107">Du kan enkelt publicera datorer, tilldela dem deklarativ konfigurationer och visa rapporter som visar var datorn överensstämmelse till det tillstånd som du angav.</span><span class="sxs-lookup"><span data-stu-id="b0320-107">You can easily onboard machines, assign them declarative configurations, and view reports showing each machine's compliance to the desired state you specified.</span></span> <span data-ttu-id="b0320-108">Azure Automation DSC management lagret är att DSC vad Azure Automation management lagret är att PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="b0320-108">The Azure Automation DSC management layer is to DSC what the Azure Automation management layer is to PowerShell scripting.</span></span> <span data-ttu-id="b0320-109">Med andra ord på samma sätt som Azure Automation kan du hantera PowerShell-skript, får du också hantera DSC-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="b0320-109">In other words, in the same way that Azure Automation helps you manage PowerShell scripts, it also helps you manage DSC configurations.</span></span> <span data-ttu-id="b0320-110">Läs mer om fördelarna med att använda Azure Automation DSC i [översikt över Azure Automation DSC](automation-dsc-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b0320-110">To learn more about the benefits of using Azure Automation DSC, see [Azure Automation DSC overview](automation-dsc-overview.md).</span></span>

<span data-ttu-id="b0320-111">Azure Automation DSC kan användas för att hantera en mängd olika datorer:</span><span class="sxs-lookup"><span data-stu-id="b0320-111">Azure Automation DSC can be used to manage a variety of machines:</span></span>

* <span data-ttu-id="b0320-112">Virtuella Azure-datorer (klassisk)</span><span class="sxs-lookup"><span data-stu-id="b0320-112">Azure virtual machines (classic)</span></span>
* <span data-ttu-id="b0320-113">Virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="b0320-113">Azure virtual machines</span></span>
* <span data-ttu-id="b0320-114">Amazon Web Services (AWS) virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="b0320-114">Amazon Web Services (AWS) virtual machines</span></span>
* <span data-ttu-id="b0320-115">Fysisk virtuell Windows-datorer lokalt eller i ett moln än Azure/AWS</span><span class="sxs-lookup"><span data-stu-id="b0320-115">Physical/virtual Windows machines on-premises, or in a cloud other than Azure/AWS</span></span>
* <span data-ttu-id="b0320-116">Fysisk virtuell Linux-datorer lokalt, i Azure eller i ett moln än Azure</span><span class="sxs-lookup"><span data-stu-id="b0320-116">Physical/virtual Linux machines on-premises, in Azure, or in a cloud other than Azure</span></span>

<span data-ttu-id="b0320-117">Dessutom om du inte är redo att hantera datorkonfigurationen från molnet kan Azure Automation DSC också användas som en endast rapport-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="b0320-117">In addition, if you are not ready to manage machine configuration from the cloud, Azure Automation DSC can also be used as a report-only endpoint.</span></span> <span data-ttu-id="b0320-118">På så sätt kan du ange (push) önskad konfiguration via DSC lokala och visa omfattande rapportering på noden kompatibilitet med tillståndet i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="b0320-118">This allows you to set (push) desired configuration through DSC on-premises and view rich reporting details on node compliance with the desired state in Azure Automation.</span></span>

<span data-ttu-id="b0320-119">I följande avsnitt beskrivs hur du kan publicera varje typ av dator till Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="b0320-119">The following sections outline how you can onboard each type of machine to Azure Automation DSC.</span></span>

## <a name="azure-virtual-machines-classic"></a><span data-ttu-id="b0320-120">Virtuella Azure-datorer (klassisk)</span><span class="sxs-lookup"><span data-stu-id="b0320-120">Azure virtual machines (classic)</span></span>

<span data-ttu-id="b0320-121">Med Azure Automation DSC kan du enkelt publicera virtuella Azure-datorer (klassisk) för konfigurationshantering med antingen Azure-portalen eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b0320-121">With Azure Automation DSC, you can easily onboard Azure virtual machines (classic) for configuration management using either the Azure portal, or PowerShell.</span></span> <span data-ttu-id="b0320-122">Under huven och en administratör behöver fjärråtkomst till den virtuella datorn, registrerar Azure VM Desired State Configuration-tillägget den virtuella datorn med Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="b0320-122">Under the hood, and without an administrator having to remote into the VM, the Azure VM Desired State Configuration extension registers the VM with Azure Automation DSC.</span></span> <span data-ttu-id="b0320-123">Eftersom Azure VM Desired State Configuration-tillägg körs asynkront, steg att följa upp förloppet eller felsöka den finns i den [ **felsöka Azure virtuella onboarding** ](#troubleshooting-azure-virtual-machine-onboarding) avsnittet nedan.</span><span class="sxs-lookup"><span data-stu-id="b0320-123">Since the Azure VM Desired State Configuration extension runs asynchronously, steps to track its progress or troubleshoot it are provided in the [**Troubleshooting Azure virtual machine onboarding**](#troubleshooting-azure-virtual-machine-onboarding) section below.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="b0320-124">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b0320-124">Azure portal</span></span>

<span data-ttu-id="b0320-125">I den [Azure-portalen](http://portal.azure.com/), klickar du på **Bläddra** -> **virtuella datorer (klassisk)**.</span><span class="sxs-lookup"><span data-stu-id="b0320-125">In the [Azure portal](http://portal.azure.com/), click **Browse** -> **Virtual machines (classic)**.</span></span> <span data-ttu-id="b0320-126">Välj Windows virtuell dator som du vill publicera.</span><span class="sxs-lookup"><span data-stu-id="b0320-126">Select the Windows VM you want to onboard.</span></span> <span data-ttu-id="b0320-127">Klicka på den virtuella datorns instrumentpanelen bladet **alla inställningar** -> **tillägg** -> **Lägg till** -> **Azure Automation DSC** -> **skapa**.</span><span class="sxs-lookup"><span data-stu-id="b0320-127">On the virtual machine's dashboard blade, click **All settings** -> **Extensions** -> **Add** -> **Azure Automation DSC** -> **Create**.</span></span> <span data-ttu-id="b0320-128">Ange den [PowerShell DSC Local Configuration Manager värden](https://msdn.microsoft.com/powershell/dsc/metaconfig4) krävs för din användningsfall registreringsnyckel ditt Automation-konto och URL: en registrering och eventuellt en nodkonfiguration att tilldela den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b0320-128">Enter the [PowerShell DSC Local Configuration Manager values](https://msdn.microsoft.com/powershell/dsc/metaconfig4) required for your use case, your Automation account's registration key and registration URL, and optionally a node configuration to assign to the VM.</span></span>

![](./media/automation-dsc-onboarding/DSC_Onboarding_1.png)

<span data-ttu-id="b0320-129">Att hitta URL: en för registrering och nyckeln för Automation-konto för att publicera datorn för att se den [ **Secure registrering** ](#secure-registration) nedan.</span><span class="sxs-lookup"><span data-stu-id="b0320-129">To find the registration URL and key for the Automation account to onboard the machine to, see the [**Secure registration**](#secure-registration) section below.</span></span>

### <a name="powershell"></a><span data-ttu-id="b0320-130">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b0320-130">PowerShell</span></span>

```powershell
# log in to both Azure Service Management and Azure Resource Manager
Add-AzureAccount
Add-AzureRmAccount

# fill in correct values for your VM/Automation account here
$VMName = ""
$ServiceName = ""
$AutomationAccountName = ""
$AutomationAccountResourceGroup = ""

# fill in the name of a Node Configuration in Azure Automation DSC, for this VM to conform to
$NodeConfigName = ""

# get Azure Automation DSC registration info
$Account = Get-AzureRmAutomationAccount -ResourceGroupName $AutomationAccountResourceGroup -Name $AutomationAccountName
$RegistrationInfo = $Account | Get-AzureRmAutomationRegistrationInfo

# use the DSC extension to onboard the VM for management with Azure Automation DSC
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

## <a name="azure-virtual-machines"></a><span data-ttu-id="b0320-131">Virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="b0320-131">Azure virtual machines</span></span>

<span data-ttu-id="b0320-132">Azure Automation DSC kan du enkelt publicera virtuella Azure-datorer för konfigurationshantering, med hjälp av Azure-portalen, Azure Resource Manager-mallar eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b0320-132">Azure Automation DSC lets you easily onboard Azure virtual machines for configuration management, using either the Azure portal, Azure Resource Manager templates, or PowerShell.</span></span> <span data-ttu-id="b0320-133">Under huven och en administratör behöver fjärråtkomst till den virtuella datorn, registrerar Azure VM Desired State Configuration-tillägget den virtuella datorn med Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="b0320-133">Under the hood, and without an administrator having to remote into the VM, the Azure VM Desired State Configuration extension registers the VM with Azure Automation DSC.</span></span> <span data-ttu-id="b0320-134">Eftersom Azure VM Desired State Configuration-tillägg körs asynkront, steg att följa upp förloppet eller felsöka den finns i den [ **felsöka Azure virtuella onboarding** ](#troubleshooting-azure-virtual-machine-onboarding) avsnittet nedan.</span><span class="sxs-lookup"><span data-stu-id="b0320-134">Since the Azure VM Desired State Configuration extension runs asynchronously, steps to track its progress or troubleshoot it are provided in the [**Troubleshooting Azure virtual machine onboarding**](#troubleshooting-azure-virtual-machine-onboarding) section below.</span></span>

### <a name="azure-portal"></a><span data-ttu-id="b0320-135">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b0320-135">Azure portal</span></span>

<span data-ttu-id="b0320-136">I den [Azure-portalen](https://portal.azure.com/), gå till Azure Automation-konto där du vill publicera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b0320-136">In the [Azure portal](https://portal.azure.com/), navigate to the Azure Automation account where you want to onboard virtual machines.</span></span> <span data-ttu-id="b0320-137">Klicka på instrumentpanelen för automatisering konto, **DSC-noder** -> **lägga till Azure VM**.</span><span class="sxs-lookup"><span data-stu-id="b0320-137">On the Automation account dashboard, click **DSC Nodes** -> **Add Azure VM**.</span></span>

<span data-ttu-id="b0320-138">Under **Välj virtuella datorer som ska publiceras**, Välj en eller flera Azure virtuella datorer att publicera.</span><span class="sxs-lookup"><span data-stu-id="b0320-138">Under **Select virtual machines to onboard**, select one or more Azure virtual machines to onboard.</span></span>

![](./media/automation-dsc-onboarding/DSC_Onboarding_2.png)

<span data-ttu-id="b0320-139">Under **konfigurera registreringsdata**, ange den [PowerShell DSC Local Configuration Manager värden](https://msdn.microsoft.com/powershell/dsc/metaconfig4) krävs för din användningsfall och eventuellt en nodkonfiguration att tilldela den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b0320-139">Under **Configure registration data**, enter the [PowerShell DSC Local Configuration Manager values](https://msdn.microsoft.com/powershell/dsc/metaconfig4) required for your use case, and optionally a node configuration to assign to the VM.</span></span>

![](./media/automation-dsc-onboarding/DSC_Onboarding_3.png)

### <a name="azure-resource-manager-templates"></a><span data-ttu-id="b0320-140">Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="b0320-140">Azure Resource Manager templates</span></span>

<span data-ttu-id="b0320-141">Virtuella Azure-datorer kan distribueras och publicerats så att Azure Automation DSC via Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="b0320-141">Azure virtual machines can be deployed and onboarded to Azure Automation DSC via Azure Resource Manager templates.</span></span> <span data-ttu-id="b0320-142">Se [konfigurera en virtuell dator via DSC-tillägg och Azure Automation DSC](https://azure.microsoft.com/documentation/templates/dsc-extension-azure-automation-pullserver/) för en exempelmall som onboards en befintlig virtuell dator till Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="b0320-142">See [Configure a VM via DSC extension and Azure Automation DSC](https://azure.microsoft.com/documentation/templates/dsc-extension-azure-automation-pullserver/) for an example template that onboards an existing VM to Azure Automation DSC.</span></span> <span data-ttu-id="b0320-143">Att hitta Registreringsnyckeln och URL: en registrering vidtas som indata i den här mallen finns i [ **Secure registrering** ](#secure-registration) nedan.</span><span class="sxs-lookup"><span data-stu-id="b0320-143">To find the registration key and registration URL taken as input in this template, see the [**Secure registration**](#secure-registration) section below.</span></span>

### <a name="powershell"></a><span data-ttu-id="b0320-144">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b0320-144">PowerShell</span></span>

<span data-ttu-id="b0320-145">Den [registrera AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode) cmdlet kan användas för att publicera virtuella datorer i Azure-portalen via PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b0320-145">The [Register-AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode) cmdlet can be used to onboard virtual machines in the Azure portal via PowerShell.</span></span>

## <a name="amazon-web-services-aws-virtual-machines"></a><span data-ttu-id="b0320-146">Amazon Web Services (AWS) virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="b0320-146">Amazon Web Services (AWS) virtual machines</span></span>

<span data-ttu-id="b0320-147">Du kan enkelt publicera Amazon Web Services virtuella datorer för konfigurationshantering av Azure Automation DSC med AWS DSC Toolkit.</span><span class="sxs-lookup"><span data-stu-id="b0320-147">You can easily onboard Amazon Web Services virtual machines for configuration management by Azure Automation DSC using the AWS DSC Toolkit.</span></span> <span data-ttu-id="b0320-148">Du kan lära dig mer om toolkit [här](https://blogs.msdn.microsoft.com/powershell/2016/04/20/aws-dsc-toolkit/).</span><span class="sxs-lookup"><span data-stu-id="b0320-148">You can learn more about the toolkit [here](https://blogs.msdn.microsoft.com/powershell/2016/04/20/aws-dsc-toolkit/).</span></span>

## <a name="physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azureaws"></a><span data-ttu-id="b0320-149">Fysisk virtuell Windows-datorer lokalt eller i ett moln än Azure/AWS</span><span class="sxs-lookup"><span data-stu-id="b0320-149">Physical/virtual Windows machines on-premises, or in a cloud other than Azure/AWS</span></span>

<span data-ttu-id="b0320-150">Lokalt Windows-datorer och Windows-datorer i Azure-moln (till exempel Amazon Web Services) kan också vara publicerats så att Azure Automation DSC, så länge som de har utgående åtkomst till internet, via några enkla steg:</span><span class="sxs-lookup"><span data-stu-id="b0320-150">On-premises Windows machines and Windows machines in non-Azure clouds (such as Amazon Web Services) can also be onboarded to Azure Automation DSC, as long as they have outbound access to the internet, via a few simple steps:</span></span>

1. <span data-ttu-id="b0320-151">Kontrollera att den senaste versionen av [WMF 5](http://aka.ms/wmf5latest) installeras på datorer som du vill publicera till Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="b0320-151">Make sure the latest version of [WMF 5](http://aka.ms/wmf5latest) is installed on the machines you want to onboard to Azure Automation DSC.</span></span>
2. <span data-ttu-id="b0320-152">Följ anvisningarna i avsnittet [ **genererar DSC metaconfigurations** ](#generating-dsc-metaconfigurations) nedan för att skapa en mapp som innehåller nödvändiga DSC-metaconfigurations.</span><span class="sxs-lookup"><span data-stu-id="b0320-152">Follow the directions in section [**Generating DSC metaconfigurations**](#generating-dsc-metaconfigurations) below to generate a folder containing the needed DSC metaconfigurations.</span></span>
3. <span data-ttu-id="b0320-153">Via fjärranslutning tillämpa PowerShell DSC-metakonfigurationen för de datorer du vill publicera.</span><span class="sxs-lookup"><span data-stu-id="b0320-153">Remotely apply the PowerShell DSC metaconfiguration to the machines you want to onboard.</span></span> <span data-ttu-id="b0320-154">**Datorn som kommandot körs från måste ha den senaste versionen av [WMF 5](http://aka.ms/wmf5latest) installerat**:</span><span class="sxs-lookup"><span data-stu-id="b0320-154">**The machine this command is run from must have the latest version of [WMF 5](http://aka.ms/wmf5latest) installed**:</span></span>

    ```powershell
    Set-DscLocalConfigurationManager -Path C:\Users\joe\Desktop\DscMetaConfigs -ComputerName MyServer1, MyServer2
    ```

4. <span data-ttu-id="b0320-155">Om du inte använder PowerShell DSC-metaconfigurations via fjärranslutning, kopiera mappen metaconfigurations från steg 2 till varje dator för att publicera.</span><span class="sxs-lookup"><span data-stu-id="b0320-155">If you cannot apply the PowerShell DSC metaconfigurations remotely, copy the metaconfigurations folder from step 2 onto each machine to onboard.</span></span> <span data-ttu-id="b0320-156">Sedan anropar **Set DscLocalConfigurationManager** lokalt på varje dator som ska publiceras.</span><span class="sxs-lookup"><span data-stu-id="b0320-156">Then call **Set-DscLocalConfigurationManager** locally on each machine to onboard.</span></span>
5. <span data-ttu-id="b0320-157">Med hjälp av Azure-portalen eller cmdletar måste du kontrollera att datorerna som ska publiceras visas som DSC-noder som är registrerade i Azure Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="b0320-157">Using the Azure portal or cmdlets, check that the machines to onboard now show up as DSC nodes registered in your Azure Automation account.</span></span>

## <a name="physicalvirtual-linux-machines-on-premises-in-azure-or-in-a-cloud-other-than-azure"></a><span data-ttu-id="b0320-158">Fysisk virtuell Linux-datorer lokalt, i Azure eller i ett moln än Azure</span><span class="sxs-lookup"><span data-stu-id="b0320-158">Physical/virtual Linux machines on-premises, in Azure, or in a cloud other than Azure</span></span>

<span data-ttu-id="b0320-159">Lokal Linux-datorer, Linux-datorer i Azure och Linux-datorer i Azure-moln kan också vara publicerats så att Azure Automation DSC, så länge som de har utgående åtkomst till internet, via några enkla steg:</span><span class="sxs-lookup"><span data-stu-id="b0320-159">On-premises Linux machines, Linux machines in Azure, and Linux machines in non-Azure clouds can also be onboarded to Azure Automation DSC, as long as they have outbound access to the internet, via a few simple steps:</span></span>

1. <span data-ttu-id="b0320-160">Kontrollera att den senaste versionen av [PowerShell Desired State Configuration för Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) installeras på datorer som du vill publicera till Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="b0320-160">Make sure the latest version of [PowerShell Desired State Configuration for Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) is installed on the machines you want to onboard to Azure Automation DSC.</span></span>
2. <span data-ttu-id="b0320-161">Om den [PowerShell DSC Local Configuration Manager standardvärden](https://msdn.microsoft.com/powershell/dsc/metaconfig4) matchar din användningsfall och du vill publicera datorer så att de **både** hämtar från och rapportera till Azure Automation DSC:</span><span class="sxs-lookup"><span data-stu-id="b0320-161">If the [PowerShell DSC Local Configuration Manager defaults](https://msdn.microsoft.com/powershell/dsc/metaconfig4) match your use case, and you want to onboard machines such that they **both** pull from and report to Azure Automation DSC:</span></span>

   + <span data-ttu-id="b0320-162">Använd Register.py på varje Linux-dator för att publicera till Azure Automation DSC, ska publiceras Local Configuration Manager för PowerShell DSC-standardvärden:</span><span class="sxs-lookup"><span data-stu-id="b0320-162">On each Linux machine to onboard to Azure Automation DSC, use Register.py to onboard using the PowerShell DSC Local Configuration Manager defaults:</span></span>

     `/opt/microsoft/dsc/Scripts/Register.py <Automation account registration key> <Automation account registration URL>`

   + <span data-ttu-id="b0320-163">Registreringsnyckel och URL: en registrering för Automation-kontot finns i [ **Secure registrering** ](#secure-registration) nedan.</span><span class="sxs-lookup"><span data-stu-id="b0320-163">To find the registration key and registration URL for your Automation account, see the [**Secure registration**](#secure-registration) section below.</span></span>

     <span data-ttu-id="b0320-164">Om PowerShell DSC Local Configuration Manager som standard **gör** **inte** matchar din användningsfall, eller om du vill publicera datorer så att de endast rapportera till Azure Automation DSC, men inte hämtar konfigurationen eller PowerShell-moduler från den, Följ steg 3-6.</span><span class="sxs-lookup"><span data-stu-id="b0320-164">If the PowerShell DSC Local Configuration Manager defaults **do** **not** match your use case, or you want to onboard machines such that they only report to Azure Automation DSC, but do not pull configuration or PowerShell modules from it,  follow steps 3 - 6.</span></span> <span data-ttu-id="b0320-165">Annars Fortsätt direkt till steg 6.</span><span class="sxs-lookup"><span data-stu-id="b0320-165">Otherwise, proceed directly to step 6.</span></span>

3. <span data-ttu-id="b0320-166">Följ anvisningarna i den [ **genererar DSC metaconfigurations** ](#generating-dsc-metaconfigurations) avsnittet nedan för att skapa en mapp som innehåller nödvändiga DSC-metaconfigurations.</span><span class="sxs-lookup"><span data-stu-id="b0320-166">Follow the directions in the [**Generating DSC metaconfigurations**](#generating-dsc-metaconfigurations) section below to generate a folder containing the needed DSC metaconfigurations.</span></span>
4. <span data-ttu-id="b0320-167">Via fjärranslutning tillämpa PowerShell DSC-metakonfigurationen för de datorer du vill publicera:</span><span class="sxs-lookup"><span data-stu-id="b0320-167">Remotely apply the PowerShell DSC metaconfiguration to the machines you want to onboard:</span></span>

    ```powershell
    $SecurePass = ConvertTo-SecureString -String "<root password>" -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential "root", $SecurePass
    $Opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

    # need a CimSession for each Linux machine to onboard

    $Session = New-CimSession -Credential $Cred -ComputerName <your Linux machine> -Port 5986 -Authentication basic -SessionOption $Opt

    Set-DscLocalConfigurationManager -CimSession $Session -Path C:\Users\joe\Desktop\DscMetaConfigs
    ```

<span data-ttu-id="b0320-168">Datorn som kommandot körs från måste ha den senaste versionen av [WMF 5](http://aka.ms/wmf5latest) installerad.</span><span class="sxs-lookup"><span data-stu-id="b0320-168">The machine this command is run from must have the latest version of [WMF 5](http://aka.ms/wmf5latest) installed.</span></span>

1. <span data-ttu-id="b0320-169">Om du inte använda PowerShell DSC-metaconfigurations via fjärranslutning, för varje Linux-dator för att publicera, kopiera metakonfigurationen som motsvarar den datorn från mappen i steg 5 till Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="b0320-169">If you cannot apply the PowerShell DSC metaconfigurations remotely, for each Linux machine to onboard, copy the metaconfiguration corresponding to that machine from the folder in step 5 onto the Linux machine.</span></span> <span data-ttu-id="b0320-170">Sedan anropar `SetDscLocalConfigurationManager.py` lokalt på varje Linux-dator du vill publicera till Azure Automation DSC:</span><span class="sxs-lookup"><span data-stu-id="b0320-170">Then call `SetDscLocalConfigurationManager.py` locally on each Linux machine you want to onboard to Azure Automation DSC:</span></span>

   `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py -configurationmof <path to metaconfiguration file>`

2. <span data-ttu-id="b0320-171">Med hjälp av Azure-portalen eller cmdletar måste du kontrollera att datorerna som ska publiceras visas som DSC-noder som är registrerade i Azure Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="b0320-171">Using the Azure portal or cmdlets, check that the machines to onboard now show up as DSC nodes registered in your Azure Automation account.</span></span>

## <a name="generating-dsc-metaconfigurations"></a><span data-ttu-id="b0320-172">Generera DSC metaconfigurations</span><span class="sxs-lookup"><span data-stu-id="b0320-172">Generating DSC metaconfigurations</span></span>

<span data-ttu-id="b0320-173">Att publicera Allmänt någon dator till Azure Automation DSC en [DSC-metakonfigurationen](https://msdn.microsoft.com/en-us/powershell/dsc/metaconfig) kan genereras som, när tillämpas, talar om DSC-agenten på datorn för att hämta från och/eller rapportera till Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="b0320-173">To generically onboard any machine to Azure Automation DSC, a [DSC metaconfiguration](https://msdn.microsoft.com/en-us/powershell/dsc/metaconfig) can be generated that, when applied, tells the DSC agent on the machine to pull from and/or report to Azure Automation DSC.</span></span> <span data-ttu-id="b0320-174">DSC-metaconfigurations för Azure Automation DSC kan genereras med hjälp av en PowerShell DSC-konfiguration eller Azure Automation PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="b0320-174">DSC metaconfigurations for Azure Automation DSC can be generated using either a PowerShell DSC configuration, or the Azure Automation PowerShell cmdlets.</span></span>

> [!NOTE]
> <span data-ttu-id="b0320-175">DSC-metaconfigurations innehåller hemligheterna behövs till publicera en dator till ett Automation-konto för hantering.</span><span class="sxs-lookup"><span data-stu-id="b0320-175">DSC metaconfigurations contain the secrets needed to onboard a machine to an Automation account for management.</span></span> <span data-ttu-id="b0320-176">Se till att skydda alla DSC-metaconfigurations som du skapar eller tar bort dem efter användning.</span><span class="sxs-lookup"><span data-stu-id="b0320-176">Make sure to properly protect any DSC metaconfigurations you create, or delete them after use.</span></span>

### <a name="using-a-dsc-configuration"></a><span data-ttu-id="b0320-177">Med hjälp av DSC-konfiguration</span><span class="sxs-lookup"><span data-stu-id="b0320-177">Using a DSC Configuration</span></span>

1. <span data-ttu-id="b0320-178">Öppna PowerShell ISE som administratör på en dator i din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="b0320-178">Open the PowerShell ISE as an administrator in a machine in your local environment.</span></span> <span data-ttu-id="b0320-179">Datorn måste ha den senaste versionen av [WMF 5](http://aka.ms/wmf5latest) installerad.</span><span class="sxs-lookup"><span data-stu-id="b0320-179">The machine must have the latest version of [WMF 5](http://aka.ms/wmf5latest) installed.</span></span>
2. <span data-ttu-id="b0320-180">Kopiera följande skript lokalt.</span><span class="sxs-lookup"><span data-stu-id="b0320-180">Copy the following script locally.</span></span> <span data-ttu-id="b0320-181">Det här skriptet innehåller en PowerShell DSC-konfiguration för att skapa metaconfigurations och ett kommando för att skapa metakonfigurationen startar.</span><span class="sxs-lookup"><span data-stu-id="b0320-181">This script contains a PowerShell DSC configuration for creating metaconfigurations, and a command to kick off the metaconfiguration creation.</span></span>

    ```powershell
    # The DSC configuration that will generate metaconfigurations
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

    # Create the metaconfigurations
    # TODO: edit the below as needed for your use case
    $Params = @{
        RegistrationUrl = '<fill me in>';
        RegistrationKey = '<fill me in>';
        ComputerName = @('<some VM to onboard>', '<some other VM to onboard>');
        NodeConfigurationName = 'SimpleConfig.webserver';
        RefreshFrequencyMins = 30;
        ConfigurationModeFrequencyMins = 15;
        RebootNodeIfNeeded = $False;
        AllowModuleOverwrite = $False;
        ConfigurationMode = 'ApplyAndMonitor';
        ActionAfterReboot = 'ContinueConfiguration';
        ReportOnly = $False;  # Set to $True to have machines only report to AA DSC but not pull from it
    }

    # Use PowerShell splatting to pass parameters to the DSC configuration being invoked
    # For more info about splatting, run: Get-Help -Name about_Splatting
    DscMetaConfigs @Params
    ```

3. <span data-ttu-id="b0320-182">Fyll i registreringsnyckel och URL: en för ditt Automation-konto, samt namnen på datorerna som ska publiceras.</span><span class="sxs-lookup"><span data-stu-id="b0320-182">Fill in the registration key and URL for your Automation account, as well as the names of the machines to onboard.</span></span> <span data-ttu-id="b0320-183">Alla andra parametrar är valfria.</span><span class="sxs-lookup"><span data-stu-id="b0320-183">All other parameters are optional.</span></span> <span data-ttu-id="b0320-184">Registreringsnyckel och URL: en registrering för Automation-kontot finns i [ **Secure registrering** ](#secure-registration) nedan.</span><span class="sxs-lookup"><span data-stu-id="b0320-184">To find the registration key and registration URL for your Automation account, see the [**Secure registration**](#secure-registration) section below.</span></span>
4. <span data-ttu-id="b0320-185">Om du vill att datorerna rapportera DSC statusinformation till Azure Automation DSC, men inte pull-konfiguration eller PowerShell-moduler, ange den **ReportOnly** parameter till true.</span><span class="sxs-lookup"><span data-stu-id="b0320-185">If you want the machines to report DSC status information to Azure Automation DSC, but not pull configuration or PowerShell modules, set the **ReportOnly** parameter to true.</span></span>
5. <span data-ttu-id="b0320-186">Kör skriptet.</span><span class="sxs-lookup"><span data-stu-id="b0320-186">Run the script.</span></span> <span data-ttu-id="b0320-187">Du bör nu ha en mapp med namnet **DscMetaConfigs** som innehåller PowerShell DSC-metaconfigurations för datorer i arbetskatalogen publicera (som administratör):</span><span class="sxs-lookup"><span data-stu-id="b0320-187">You should now have a folder called **DscMetaConfigs** in your working directory, containing the PowerShell DSC metaconfigurations for the machines to onboard (as an administrator):</span></span>

    ```powershell
    Set-DscLocalConfigurationManager -Path ./DscMetaConfigs
    ```

### <a name="using-the-azure-automation-cmdlets"></a><span data-ttu-id="b0320-188">Med hjälp av Azure Automation-cmdlets</span><span class="sxs-lookup"><span data-stu-id="b0320-188">Using the Azure Automation cmdlets</span></span>

<span data-ttu-id="b0320-189">Om standardinställningarna PowerShell DSC Local Configuration Manager som matchar dina användningsfall och du vill publicera datorer så att de hämtar från såväl rapportera till Azure Automation DSC, ger Azure Automation-cmdlets en förenklad metoden för att skapa DSC metaconfigurations behövs:</span><span class="sxs-lookup"><span data-stu-id="b0320-189">If the PowerShell DSC Local Configuration Manager defaults match your use case, and you want to onboard machines such that they both pull from and report to Azure Automation DSC, the Azure Automation cmdlets provide a simplified method of generating the DSC metaconfigurations needed:</span></span>

1. <span data-ttu-id="b0320-190">Öppna PowerShell-konsolen eller PowerShell ISE som administratör på en dator i din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="b0320-190">Open the PowerShell console or PowerShell ISE as an administrator in a machine in your local environment.</span></span>
2. <span data-ttu-id="b0320-191">Anslut till Azure Resource Manager med **Add-AzureRmAccount**</span><span class="sxs-lookup"><span data-stu-id="b0320-191">Connect to Azure Resource Manager using **Add-AzureRmAccount**</span></span>
3. <span data-ttu-id="b0320-192">Ladda ned PowerShell DSC-metaconfigurations för datorer som du vill ska publiceras från Automation-kontot som du vill publicera noder:</span><span class="sxs-lookup"><span data-stu-id="b0320-192">Download the PowerShell DSC metaconfigurations for the machines you want to onboard from the Automation account to which you want to onboard nodes:</span></span>

    ```powershell
    # Define the parameters for Get-AzureRmAutomationDscOnboardingMetaconfig using PowerShell Splatting
    $Params = @{

        ResourceGroupName = 'ContosoResources'; # The name of the ARM Resource Group that contains your Azure Automation Account
        AutomationAccountName = 'ContosoAutomation'; # The name of the Azure Automation Account where you want a node on-boarded to
        ComputerName = @('web01', 'web02', 'sql01'); # The names of the computers that the meta configuration will be generated for
        OutputFolder = "$env:UserProfile\Desktop\";
    }
    # Use PowerShell splatting to pass parameters to the Azure Automation cmdlet being invoked
    # For more info about splatting, run: Get-Help -Name about_Splatting
    Get-AzureRmAutomationDscOnboardingMetaconfig @Params
    ```
    
4. <span data-ttu-id="b0320-193">Du bör nu ha en mapp med namnet ***DscMetaConfigs***, som innehåller PowerShell DSC-metaconfigurations för datorer publicera (som administratör):</span><span class="sxs-lookup"><span data-stu-id="b0320-193">You should now have a folder called ***DscMetaConfigs***, containing the PowerShell DSC metaconfigurations for the machines to onboard (as an administrator):</span></span>
    
    ```powershell
    Set-DscLocalConfigurationManager -Path $env:UserProfile\Desktop\DscMetaConfigs
    ```

## <a name="secure-registration"></a><span data-ttu-id="b0320-194">Säker registrering</span><span class="sxs-lookup"><span data-stu-id="b0320-194">Secure registration</span></span>

<span data-ttu-id="b0320-195">Datorer kan på ett säkert sätt publicera till en Azure Automation-konto via WMF 5 DSC registrering protokollet, vilket låter en DSC-nod att autentisera till en Pull-PowerShell DSC V2 eller Reporting server (inklusive Azure Automation DSC).</span><span class="sxs-lookup"><span data-stu-id="b0320-195">Machines can securely onboard to an Azure Automation account through the WMF 5 DSC registration protocol, which allows a DSC node to authenticate to a PowerShell DSC V2 Pull or Reporting server (including Azure Automation DSC).</span></span> <span data-ttu-id="b0320-196">Noden registrerar till servern på en **URL: en registrering**, autentiseras med hjälp av en **registreringsnyckel**.</span><span class="sxs-lookup"><span data-stu-id="b0320-196">The node registers to the server at a **Registration URL**, authenticating using a **Registration key**.</span></span> <span data-ttu-id="b0320-197">Under registreringen, DSC-nod och DSC Pull/rapportserver förhandla ett unikt certifikat för den här noden ska användas för autentisering till servern efter registreringen.</span><span class="sxs-lookup"><span data-stu-id="b0320-197">During registration, the DSC node and DSC Pull/Reporting server negotiate a unique certificate for this node to use for authentication to the server post-registration.</span></span> <span data-ttu-id="b0320-198">Den här processen förhindrar publicerats så noder från personifiera en annan, till exempel om en nod har komprometterats och uppför medvetet.</span><span class="sxs-lookup"><span data-stu-id="b0320-198">This process prevents onboarded nodes from impersonating one another, such as if a node is compromised and behaving maliciously.</span></span> <span data-ttu-id="b0320-199">Efter registreringen nyckel för tjänstregistrering används inte för autentiseringen igen och tas bort från noden.</span><span class="sxs-lookup"><span data-stu-id="b0320-199">After registration, the Registration key is not used for authentication again, and is deleted from the node.</span></span>

<span data-ttu-id="b0320-200">Du kan hämta den information som krävs för protokollet för DSC-registreringen från den **hantera nycklar** bladet i Azure preview portal.</span><span class="sxs-lookup"><span data-stu-id="b0320-200">You can get the information required for the DSC registration protocol from the **Manage Keys** blade in the Azure preview portal.</span></span> <span data-ttu-id="b0320-201">Öppna bladet genom att klicka på nyckelikonen på den **Essentials** panelen för Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="b0320-201">Open this blade by clicking the key icon on the **Essentials** panel for the Automation account.</span></span>

![](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

* <span data-ttu-id="b0320-202">URL: en för registrering är URL-fältet i bladet hantera nycklar.</span><span class="sxs-lookup"><span data-stu-id="b0320-202">Registration URL is the URL field in the Manage Keys blade.</span></span>
* <span data-ttu-id="b0320-203">Nyckel för tjänstregistrering är den primära åtkomstnyckeln eller sekundära åtkomstnyckeln i bladet hantera nycklar.</span><span class="sxs-lookup"><span data-stu-id="b0320-203">Registration key is the Primary Access Key or Secondary Access Key in the Manage Keys blade.</span></span> <span data-ttu-id="b0320-204">Antingen nyckeln kan användas.</span><span class="sxs-lookup"><span data-stu-id="b0320-204">Either key can be used.</span></span>

<span data-ttu-id="b0320-205">För extra säkerhet kan primära och sekundära åtkomstnycklarna för ett Automation-konto kan genereras när som helst (om den **hantera nycklar** bladet) för att förhindra framtida nod registreringar med tidigare nycklar.</span><span class="sxs-lookup"><span data-stu-id="b0320-205">For added security, the primary and secondary access keys of an Automation account can be regenerated at any time (on the **Manage Keys** blade) to prevent future node registrations using previous keys.</span></span>

## <a name="troubleshooting-azure-virtual-machine-onboarding"></a><span data-ttu-id="b0320-206">Felsökning av virtuell dator i Azure-onboarding</span><span class="sxs-lookup"><span data-stu-id="b0320-206">Troubleshooting Azure virtual machine onboarding</span></span>

<span data-ttu-id="b0320-207">Azure Automation DSC kan du enkelt inbyggda Windows Azure-datorer för konfigurationshantering.</span><span class="sxs-lookup"><span data-stu-id="b0320-207">Azure Automation DSC lets you easily onboard Azure Windows VMs for configuration management.</span></span> <span data-ttu-id="b0320-208">Under huven används Azure VM Desired State Configuration-tillägget för att registrera den virtuella datorn med Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="b0320-208">Under the hood, the Azure VM Desired State Configuration extension is used to register the VM with Azure Automation DSC.</span></span> <span data-ttu-id="b0320-209">Eftersom Azure VM Desired State Configuration-tillägg körs asynkront, spåra förloppet och felsökning av körningen kan det vara viktigt.</span><span class="sxs-lookup"><span data-stu-id="b0320-209">Since the Azure VM Desired State Configuration extension runs asynchronously, tracking its progress and troubleshooting its execution may be important.</span></span>

> [!NOTE]
> <span data-ttu-id="b0320-210">En metod för onboarding av en virtuell Azure Windows-dator till Azure Automation DSC som använder Azure VM Desired State Configuration-tillägget kan ta upp till en timme att visa upp som har registrerats i Azure Automation-nod.</span><span class="sxs-lookup"><span data-stu-id="b0320-210">Any method of onboarding an Azure Windows VM to Azure Automation DSC that uses the Azure VM Desired State Configuration extension could take up to an hour for the node to show up as registered in Azure Automation.</span></span> <span data-ttu-id="b0320-211">Detta beror på att installationen av Windows Management Framework 5.0 på den virtuella datorn via Azure VM DSC-tillägg som krävs för att publicera den virtuella datorn till Azure Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="b0320-211">This is due to the installation of Windows Management Framework 5.0 on the VM by the Azure VM DSC extension, which is required to onboard the VM to Azure Automation DSC.</span></span>

<span data-ttu-id="b0320-212">Tillägget i Azure-portalen går du till den virtuella datorn som publicerats så att felsöka eller visa status för Azure VM Desired State Configuration och sedan klicka -> **alla inställningar** -> **tillägg**  ->  **DSC**.</span><span class="sxs-lookup"><span data-stu-id="b0320-212">To troubleshoot or view the status of the Azure VM Desired State Configuration extension, in the Azure portal navigate to the VM being onboarded, then click -> **All settings** -> **Extensions** -> **DSC**.</span></span> <span data-ttu-id="b0320-213">Mer information kan du klicka på **visa detaljerad statusinformation om**.</span><span class="sxs-lookup"><span data-stu-id="b0320-213">For more details, you can click **View detailed status**.</span></span>

[![](./media/automation-dsc-onboarding/DSC_Onboarding_5.png)](https://technet.microsoft.com/library/dn249912.aspx)

## <a name="certificate-expiration-and-reregistration"></a><span data-ttu-id="b0320-214">Certifikatet upphör att gälla och omregistrering</span><span class="sxs-lookup"><span data-stu-id="b0320-214">Certificate expiration and reregistration</span></span>

<span data-ttu-id="b0320-215">När du registrerar en dator som en DSC-nod i Azure Automation DSC, finns det flera skäl till varför du kanske måste registrera om noden i framtiden:</span><span class="sxs-lookup"><span data-stu-id="b0320-215">After registering a machine as a DSC node in Azure Automation DSC, there are a number of reasons why you may need to reregister that node in the future:</span></span>

* <span data-ttu-id="b0320-216">Varje nod förhandlar automatiskt ett unikt certifikat för autentisering som upphör att gälla efter ett år efter registrering.</span><span class="sxs-lookup"><span data-stu-id="b0320-216">After registering, each node automatically negotiates a unique certificate for authentication that expires after one year.</span></span> <span data-ttu-id="b0320-217">Protokollet PowerShell DSC-registrering kan för närvarande automatiskt förnya certifikat när de närmar sig förfallodatum, så du behöver registrera om noderna efter ett år tid.</span><span class="sxs-lookup"><span data-stu-id="b0320-217">Currently, the PowerShell DSC registration protocol cannot automatically renew certificates when they are nearing expiration, so you need to reregister the nodes after a year's time.</span></span> <span data-ttu-id="b0320-218">Innan du registrerar om, kontrollerar du att varje nod körs Windows Management Framework 5.0 RTM.</span><span class="sxs-lookup"><span data-stu-id="b0320-218">Before reregistering, ensure that each node is running Windows Management Framework 5.0 RTM.</span></span> <span data-ttu-id="b0320-219">Om en nod Autentiseringscertifikatet upphör att gälla och noden inte återregistrerade, noden kan inte kommunicera med Azure Automation och kommer att markeras Unresponsive.</span><span class="sxs-lookup"><span data-stu-id="b0320-219">If a node's authentication certificate expires, and the node is not reregistered, the node will be unable to communicate with Azure Automation and will be marked 'Unresponsive.'</span></span> <span data-ttu-id="b0320-220">Omregistreringen utförs 90 dagar eller mindre från certifikatets förfallotid eller när som helst efter tidpunkt då certifikatet upphör att gälla, leder till ett nytt certifikat som genereras och används.</span><span class="sxs-lookup"><span data-stu-id="b0320-220">Reregistration performed 90 days or less from the certificate expiration time, or at any point after the certificate expiration time, will result in a new certificate being generated and used.</span></span>
* <span data-ttu-id="b0320-221">Att ändra [PowerShell DSC Local Configuration Manager värden](https://msdn.microsoft.com/powershell/dsc/metaconfig4) som angavs under registreringen av noden, till exempel ConfigurationMode.</span><span class="sxs-lookup"><span data-stu-id="b0320-221">To change any [PowerShell DSC Local Configuration Manager values](https://msdn.microsoft.com/powershell/dsc/metaconfig4) that were set during initial registration of the node, such as ConfigurationMode.</span></span> <span data-ttu-id="b0320-222">För närvarande kan värdena DSC-agenten endast ändras via omregistreringen.</span><span class="sxs-lookup"><span data-stu-id="b0320-222">Currently, these DSC agent values can only be changed through reregistration.</span></span> <span data-ttu-id="b0320-223">Det enda undantaget är nodkonfiguration tilldelad noden – här kan ändras i Azure Automation DSC direkt.</span><span class="sxs-lookup"><span data-stu-id="b0320-223">The one exception is the Node Configuration assigned to the node -- this can be changed in Azure Automation DSC directly.</span></span>

<span data-ttu-id="b0320-224">Omregistreringen kan utföras på samma sätt som du har registrerat noden till en början med hjälp av onboarding-metoderna som beskrivs i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="b0320-224">Reregistration can be performed in the same way you registered the node initially, using any of the onboarding methods described in this document.</span></span> <span data-ttu-id="b0320-225">Du behöver inte att avregistrera en nod från Azure Automation DSC innan du registrerar om den.</span><span class="sxs-lookup"><span data-stu-id="b0320-225">You do not need to unregister a node from Azure Automation DSC before reregistering it.</span></span>

## <a name="related-articles"></a><span data-ttu-id="b0320-226">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="b0320-226">Related Articles</span></span>

* [<span data-ttu-id="b0320-227">Översikt över Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="b0320-227">Azure Automation DSC overview</span></span>](automation-dsc-overview.md)
* [<span data-ttu-id="b0320-228">Azure Automation DSC-cmdlets</span><span class="sxs-lookup"><span data-stu-id="b0320-228">Azure Automation DSC cmdlets</span></span>](/powershell/module/azurerm.automation/#automation)
* [<span data-ttu-id="b0320-229">Priser för Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="b0320-229">Azure Automation DSC pricing</span></span>](https://azure.microsoft.com/pricing/details/automation/)
