---
title: "aaaVirtual datorn funktioner och tillägg för Windows i Azure | Microsoft Docs"
description: "Lär dig vilka tillägg som är tillgängliga för virtuella Azure-datorer, grupperade efter vad de tillhandahålla och förbättra."
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 999d63ee-890e-432e-9391-25b3fc6cde28
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/06/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 61ccfd696b38e9be1026d836d5796c2346fd650f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-machine-extensions-and-features-for-windows"></a><span data-ttu-id="ad314-103">Tillägg för virtuell dator och funktioner i Windows</span><span class="sxs-lookup"><span data-stu-id="ad314-103">Virtual machine extensions and features for Windows</span></span>

<span data-ttu-id="ad314-104">Tillägg för virtuell dator i Azure är små program som innehåller efter distributionen konfiguration och automatisering av uppgifter på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="ad314-104">Azure virtual machine extensions are small applications that provide post-deployment configuration and automation tasks on Azure virtual machines.</span></span> <span data-ttu-id="ad314-105">Om en virtuell dator kräver installation av programvara, anti-virusskydd eller Docker-konfiguration, VM-tillägget kan exempelvis vara används toocomplete dessa uppgifter.</span><span class="sxs-lookup"><span data-stu-id="ad314-105">For example, if a virtual machine requires software installation, anti-virus protection, or Docker configuration, a VM extension can be used toocomplete these tasks.</span></span> <span data-ttu-id="ad314-106">Azure VM-tillägg kan köras med hjälp av hello Azure CLI, PowerShell, Azure Resource Manager-mallar och hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ad314-106">Azure VM extensions can be run by using hello Azure CLI, PowerShell, Azure Resource Manager templates, and hello Azure portal.</span></span> <span data-ttu-id="ad314-107">Tillägg kan levereras tillsammans med en ny distribution av virtuella datorer eller köra mot alla befintliga system.</span><span class="sxs-lookup"><span data-stu-id="ad314-107">Extensions can be bundled with a new virtual machine deployment or run against any existing system.</span></span>

<span data-ttu-id="ad314-108">Det här dokumentet innehåller en översikt över virtuella tillägg, krav för att använda tillägg för virtuell dator och vägledning om hur toodetect, hantera och ta bort tillägg för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ad314-108">This document provides an overview of virtual machine extensions, prerequisites for using virtual machine extensions, and guidance on how toodetect, manage, and remove virtual machine extensions.</span></span> <span data-ttu-id="ad314-109">Det här dokumentet innehåller allmänna informationen eftersom många VM-tillägg är tillgängligt, var och en med en potentiellt unik konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ad314-109">This document provides generalized information because many VM extensions are available, each with a potentially unique configuration.</span></span> <span data-ttu-id="ad314-110">Tillägget-specifik information finns i varje dokument specifika toohello enskilda tillägg.</span><span class="sxs-lookup"><span data-stu-id="ad314-110">Extension-specific details can be found in each document specific toohello individual extension.</span></span>

## <a name="use-cases-and-samples"></a><span data-ttu-id="ad314-111">Användningsfall och exempel</span><span class="sxs-lookup"><span data-stu-id="ad314-111">Use cases and samples</span></span>

<span data-ttu-id="ad314-112">Det finns många olika Virtuella Azure-tillägg, var och en med en specifik användningsfall.</span><span class="sxs-lookup"><span data-stu-id="ad314-112">There are many different Azure VM extensions available, each with a specific use case.</span></span> <span data-ttu-id="ad314-113">Några exempel på användningsområden är:</span><span class="sxs-lookup"><span data-stu-id="ad314-113">Some example use cases are:</span></span>

- <span data-ttu-id="ad314-114">Använda PowerShell önskade konfigurationer tooa virtuella datorn med hjälp av hello DSC-tillägg för Windows.</span><span class="sxs-lookup"><span data-stu-id="ad314-114">Apply PowerShell Desired State configurations tooa virtual machine by using hello DSC extension for Windows.</span></span> <span data-ttu-id="ad314-115">Mer information finns i [tillägg för konfiguration av Azure önskade tillstånd](extensions-dsc-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ad314-115">For more information, see [Azure Desired State configuration extension](extensions-dsc-overview.md).</span></span>
- <span data-ttu-id="ad314-116">Konfigurera övervakning av virtuella datorer med hjälp av hello Microsoft Monitoring Agent VM-tillägget.</span><span class="sxs-lookup"><span data-stu-id="ad314-116">Configure virtual machine monitoring by using hello Microsoft Monitoring Agent VM extension.</span></span> <span data-ttu-id="ad314-117">Mer information finns i [ansluta Azure virtuella datorer tooLog Analytics](../../log-analytics/log-analytics-azure-vm-extension.md).</span><span class="sxs-lookup"><span data-stu-id="ad314-117">For more information, see [Connect Azure virtual machines tooLog Analytics](../../log-analytics/log-analytics-azure-vm-extension.md).</span></span>
- <span data-ttu-id="ad314-118">Konfigurera övervakning av Azure-infrastrukturen med hello Datadog tillägg.</span><span class="sxs-lookup"><span data-stu-id="ad314-118">Configure monitoring of your Azure infrastructure with hello Datadog extension.</span></span> <span data-ttu-id="ad314-119">Mer information finns i hello [Datadog blogg](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).</span><span class="sxs-lookup"><span data-stu-id="ad314-119">For more information, see hello [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).</span></span>
- <span data-ttu-id="ad314-120">Konfigurera en virtuell Azure-dator med hjälp av Chef.</span><span class="sxs-lookup"><span data-stu-id="ad314-120">Configure an Azure virtual machine by using Chef.</span></span> <span data-ttu-id="ad314-121">Mer information finns i [automatisera Azure distribution av virtuella datorer med Chef](chef-automation.md).</span><span class="sxs-lookup"><span data-stu-id="ad314-121">For more information, see [Automating Azure virtual machine deployment with Chef](chef-automation.md).</span></span>

<span data-ttu-id="ad314-122">Dessutom tooprocess-specifika tillägg, ett anpassat skript tillägg är tillgängligt för både Windows och Linux virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ad314-122">In addition tooprocess-specific extensions, a Custom Script extension is available for both Windows and Linux virtual machines.</span></span> <span data-ttu-id="ad314-123">hello tillägget för anpassat skript för Windows kan alla PowerShell-skriptet toobe som körs på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ad314-123">hello Custom Script extension for Windows allows any PowerShell script toobe run on a virtual machine.</span></span> <span data-ttu-id="ad314-124">Detta är användbart när du skapar Azure-distributioner som kräver konfiguration efter vilken interna Azure verktygsuppsättning kan ge.</span><span class="sxs-lookup"><span data-stu-id="ad314-124">This is useful when you're designing Azure deployments that require configuration beyond what native Azure tooling can provide.</span></span> <span data-ttu-id="ad314-125">Mer information finns i [tillägget Windows VM anpassade skript](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="ad314-125">For more information, see [Windows VM Custom Script extension](extensions-customscript.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="ad314-126">Krav</span><span class="sxs-lookup"><span data-stu-id="ad314-126">Prerequisites</span></span>

<span data-ttu-id="ad314-127">Varje tillägg för virtuell dator kan ha en egen uppsättning krav.</span><span class="sxs-lookup"><span data-stu-id="ad314-127">Each virtual machine extension may have its own set of prerequisites.</span></span> <span data-ttu-id="ad314-128">Hej Docker VM-tillägget har till exempel en förutsättning för en Linux-fördelning som stöds.</span><span class="sxs-lookup"><span data-stu-id="ad314-128">For instance, hello Docker VM extension has a prerequisite of a supported Linux distribution.</span></span> <span data-ttu-id="ad314-129">Krav av enskilda tillägg beskrivs i dokumentationen för hello-tillägg-specifika.</span><span class="sxs-lookup"><span data-stu-id="ad314-129">Requirements of individual extensions are detailed in hello extension-specific documentation.</span></span>

### <a name="azure-vm-agent"></a><span data-ttu-id="ad314-130">Virtuell Azure-datoragent</span><span class="sxs-lookup"><span data-stu-id="ad314-130">Azure VM agent</span></span>
<span data-ttu-id="ad314-131">hello Azure VM-agenten hanterar interaktionen mellan en virtuell Azure-dator och hello Azure-infrastrukturkontrollanten.</span><span class="sxs-lookup"><span data-stu-id="ad314-131">hello Azure VM agent manages interaction between an Azure virtual machine and hello Azure fabric controller.</span></span> <span data-ttu-id="ad314-132">hello VM-agenten är ansvarig för många funktioner för att distribuera och hantera virtuella Azure-datorer, inklusive kör VM-tillägg.</span><span class="sxs-lookup"><span data-stu-id="ad314-132">hello VM agent is responsible for many functional aspects of deploying and managing Azure virtual machines, including running VM extensions.</span></span> <span data-ttu-id="ad314-133">hello Azure VM-agenten är förinstallerat på Azure Marketplace-bilder och kan installeras på operativsystem som stöds.</span><span class="sxs-lookup"><span data-stu-id="ad314-133">hello Azure VM agent is preinstalled on Azure Marketplace images and can be installed on supported operating systems.</span></span>

<span data-ttu-id="ad314-134">Information om operativsystem som stöds och installationsanvisningar finns [virtuella Azure-datorn agent](agent-user-guide.md).</span><span class="sxs-lookup"><span data-stu-id="ad314-134">For information on supported operating systems and installation instructions, see [Azure virtual machine agent](agent-user-guide.md).</span></span>

## <a name="discover-vm-extensions"></a><span data-ttu-id="ad314-135">Identifiera VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="ad314-135">Discover VM extensions</span></span>
<span data-ttu-id="ad314-136">Det finns många olika VM-tillägg för användning med virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="ad314-136">Many different VM extensions are available for use with Azure virtual machines.</span></span> <span data-ttu-id="ad314-137">toosee en fullständig lista, kör följande kommando med hello Azure Resource Manager PowerShell-modulen hello.</span><span class="sxs-lookup"><span data-stu-id="ad314-137">toosee a complete list, run hello following command with hello Azure Resource Manager PowerShell module.</span></span> <span data-ttu-id="ad314-138">Kontrollera att toospecify hello önskad plats när du kör det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="ad314-138">Make sure toospecify hello desired location when you're running this command.</span></span>

```powershell
Get-AzureRmVmImagePublisher -Location WestUS | `
Get-AzureRmVMExtensionImageType | `
Get-AzureRmVMExtensionImage | Select Type, Version
```

## <a name="run-vm-extensions"></a><span data-ttu-id="ad314-139">Kör VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="ad314-139">Run VM extensions</span></span>

<span data-ttu-id="ad314-140">Tillägg för virtuell Azure-dator kan köras på den befintliga virtuella datorer, vilket är användbart när du behöver toomake konfigurationsändringar eller återställa anslutningen på en redan distribuerad virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ad314-140">Azure virtual machine extensions can be run on existing virtual machines, which is useful when you need toomake configuration changes or recover connectivity on an already deployed VM.</span></span> <span data-ttu-id="ad314-141">VM-tillägg kan också tillsammans med Azure Resource Manager mall-distributioner.</span><span class="sxs-lookup"><span data-stu-id="ad314-141">VM extensions can also be bundled with Azure Resource Manager template deployments.</span></span> <span data-ttu-id="ad314-142">Genom att använda tillägg med Resource Manager-mallar kan aktivera du virtuella Azure-datorer toobe distribuerats och konfigurerats utan hello behovet av att åtgärder efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="ad314-142">By using extensions with Resource Manager templates, you can enable Azure virtual machines toobe deployed and configured without hello need for post-deployment intervention.</span></span>

<span data-ttu-id="ad314-143">hello följande metoder kan vara används toorun filnamnstillägget mot en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ad314-143">hello following methods can be used toorun an extension against an existing virtual machine.</span></span>

### <a name="powershell"></a><span data-ttu-id="ad314-144">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ad314-144">PowerShell</span></span>

<span data-ttu-id="ad314-145">Det finns flera PowerShell-kommandon för att köra enskilda tillägg.</span><span class="sxs-lookup"><span data-stu-id="ad314-145">Several PowerShell commands exist for running individual extensions.</span></span> <span data-ttu-id="ad314-146">toosee kör hello följande PowerShell-kommandon för en lista.</span><span class="sxs-lookup"><span data-stu-id="ad314-146">toosee a list, run hello following PowerShell commands.</span></span>

```powershell
get-command Set-AzureRM*Extension* -Module AzureRM.Compute
```

<span data-ttu-id="ad314-147">Detta ger utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="ad314-147">This provides output similar toohello following:</span></span>

```powershell
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Set-AzureRmVMAccessExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMADDomainExtension                     2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMAEMExtension                          2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMBackupExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMBginfoExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMChefExtension                         2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMCustomScriptExtension                 2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDiagnosticsExtension                  2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDiskEncryptionExtension               2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDscExtension                          2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMExtension                             2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMSqlServerExtension                    2.2.0      AzureRM.Compute
```

<span data-ttu-id="ad314-148">hello följande exempel använder hello anpassat skript tillägget toodownload ett skript från en GitHub-databas till hello virtuella måldatorn och kör sedan hello skript.</span><span class="sxs-lookup"><span data-stu-id="ad314-148">hello following example uses hello Custom Script extension toodownload a script from a GitHub repository onto hello target virtual machine and then run hello script.</span></span> <span data-ttu-id="ad314-149">Mer information om hello tillägget för anpassat skript finns [översikt över tillägget för anpassat skript](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="ad314-149">For more information on hello Custom Script extension, see [Custom Script extension overview](extensions-customscript.md).</span></span>

```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" -Name "myCustomScript" `
    -FileUri "https://raw.githubusercontent.com/neilpeterson/nepeters-azure-templates/master/windows-custom-script-simple/support-scripts/Create-File.ps1" `
    -Run "Create-File.ps1" -Location "West US"
```

<span data-ttu-id="ad314-150">I det här exemplet är hello tillägget för virtuell dator åtkomst används tooreset hello administrativa lösenordet för en virtuell Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="ad314-150">In this example, hello VM Access extension is used tooreset hello administrative password of a Windows virtual machine.</span></span> <span data-ttu-id="ad314-151">Mer information om hello VM Access-tillägg finns [Återställ Fjärrskrivbordstjänsten i en Windows VM](reset-rdp.md).</span><span class="sxs-lookup"><span data-stu-id="ad314-151">For more information on hello VM Access extension, see [Reset Remote Desktop service in a Windows VM](reset-rdp.md).</span></span>

```powershell
$cred=Get-Credential

Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myVMAccess" `
    -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

<span data-ttu-id="ad314-152">Hej `Set-AzureRmVMExtension` kommando kan vara används toostart VM-tillägget.</span><span class="sxs-lookup"><span data-stu-id="ad314-152">hello `Set-AzureRmVMExtension` command can be used toostart any VM extension.</span></span> <span data-ttu-id="ad314-153">Mer information finns i hello [Set AzureRmVMExtension referens](https://msdn.microsoft.com/en-us/library/mt603745.aspx).</span><span class="sxs-lookup"><span data-stu-id="ad314-153">For more information, see hello [Set-AzureRmVMExtension reference](https://msdn.microsoft.com/en-us/library/mt603745.aspx).</span></span>


### <a name="azure-portal"></a><span data-ttu-id="ad314-154">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ad314-154">Azure portal</span></span>

<span data-ttu-id="ad314-155">VM-tillägget kan vara tillämpade tooan befintliga virtuella datorn via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ad314-155">A VM extension can be applied tooan existing virtual machine through hello Azure portal.</span></span> <span data-ttu-id="ad314-156">toodo så Välj hello virtuella dator du vill toouse, Välj **tillägg**, och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="ad314-156">toodo so, select hello virtual machine you want toouse, choose **Extensions**, and click **Add**.</span></span> <span data-ttu-id="ad314-157">Detta visar en lista över tillgängliga tillägg.</span><span class="sxs-lookup"><span data-stu-id="ad314-157">This provides a list of available extensions.</span></span> <span data-ttu-id="ad314-158">Välj hello en du vill använda och följ hello stegen i guiden hello.</span><span class="sxs-lookup"><span data-stu-id="ad314-158">Select hello one you want and follow hello steps in hello wizard.</span></span>

<span data-ttu-id="ad314-159">hello visar följande bild hello installation av hello Microsoft Antimalware tillägg från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ad314-159">hello following image shows hello installation of hello Microsoft Antimalware extension from hello Azure portal.</span></span>

![Installera tillägg för program mot skadlig kod](./media/extensions-features/installantimalwareextension.png)

### <a name="azure-resource-manager-templates"></a><span data-ttu-id="ad314-161">Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="ad314-161">Azure Resource Manager templates</span></span>

<span data-ttu-id="ad314-162">VM-tillägg kan vara tillagda tooan Azure Resource Manager-mall och utförs med hello distribution av hello mallen.</span><span class="sxs-lookup"><span data-stu-id="ad314-162">VM extensions can be added tooan Azure Resource Manager template and executed with hello deployment of hello template.</span></span> <span data-ttu-id="ad314-163">Distribuera tillägg med en mall är användbart för att skapa helt konfigurerade Azure-distributioner.</span><span class="sxs-lookup"><span data-stu-id="ad314-163">Deploying extensions with a template is useful for creating fully configured Azure deployments.</span></span> <span data-ttu-id="ad314-164">Till exempel hello följande JSON hämtas från en Resource Manager-mall som distribuerar en uppsättning belastningsutjämnade virtuella datorer och en Azure SQL database och installerar sedan en .NET Core-programmet på varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ad314-164">For example, hello following JSON is taken from a Resource Manager template that deploys a set of load-balanced virtual machines and an Azure SQL database, and then installs a .NET Core application on each VM.</span></span> <span data-ttu-id="ad314-165">hello VM-tillägget hand tar om hello Programvaruinstallation.</span><span class="sxs-lookup"><span data-stu-id="ad314-165">hello VM extension takes care of hello software installation.</span></span>

<span data-ttu-id="ad314-166">Mer information finns i hello [fullständig Resource Manager-mall](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="ad314-166">For more information, see hello [full Resource Manager template](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

<span data-ttu-id="ad314-167">Mer information finns i [redigera Azure Resource Manager-mallar med Windows VM-tillägg](template-description.md#extensions).</span><span class="sxs-lookup"><span data-stu-id="ad314-167">For more information, see [Authoring Azure Resource Manager templates with Windows VM extensions](template-description.md#extensions).</span></span>

## <a name="secure-vm-extension-data"></a><span data-ttu-id="ad314-168">Säkra data för VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="ad314-168">Secure VM extension data</span></span>

<span data-ttu-id="ad314-169">När du kör en VM-tillägget, kan det vara nödvändigt tooinclude känslig information, till exempel autentiseringsuppgifter, lagringskontonamn och åtkomst för lagringskontonycklar.</span><span class="sxs-lookup"><span data-stu-id="ad314-169">When you're running a VM extension, it may be necessary tooinclude sensitive information such as credentials, storage account names, and storage account access keys.</span></span> <span data-ttu-id="ad314-170">Många VM-tillägg innehåller en skyddad konfiguration som krypterar data och dekrypterar dem bara inuti hello virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="ad314-170">Many VM extensions include a protected configuration that encrypts data and only decrypts it inside hello target virtual machine.</span></span> <span data-ttu-id="ad314-171">Varje tillägg har en specifik skyddade konfigurationsschema som ska anges i tillägget specifika-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="ad314-171">Each extension has a specific protected configuration schema that will be detailed in extension-specific documentation.</span></span>

<span data-ttu-id="ad314-172">hello som följande exempel visar en instans av hello tillägget för anpassat skript för Windows.</span><span class="sxs-lookup"><span data-stu-id="ad314-172">hello following example shows an instance of hello Custom Script extension for Windows.</span></span> <span data-ttu-id="ad314-173">Observera att hello kommandot tooexecute innehåller en uppsättning autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="ad314-173">Notice that hello command tooexecute includes a set of credentials.</span></span> <span data-ttu-id="ad314-174">I det här exemplet kommer hello kommandot tooexecute inte krypteras.</span><span class="sxs-lookup"><span data-stu-id="ad314-174">In this example, hello command tooexecute will not be encrypted.</span></span>


```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ],
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

<span data-ttu-id="ad314-175">Skydda hello körning sträng genom att flytta hello **kommandot tooexecute** egenskapen toohello **skyddade** konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ad314-175">Secure hello execution string by moving hello **command tooexecute** property toohello **protected** configuration.</span></span>

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

## <a name="troubleshoot-vm-extensions"></a><span data-ttu-id="ad314-176">Felsökning av VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="ad314-176">Troubleshoot VM extensions</span></span>

<span data-ttu-id="ad314-177">Varje VM-tillägg kan ha specifika felsökningsanvisningar.</span><span class="sxs-lookup"><span data-stu-id="ad314-177">Each VM extension may have specific troubleshooting steps.</span></span> <span data-ttu-id="ad314-178">Exempelvis när du använder hello tillägget för anpassat skript finns körning skriptdetaljer lokalt på hello virtuell dator där hello tillägget kördes.</span><span class="sxs-lookup"><span data-stu-id="ad314-178">For instance, when you're using hello Custom Script extension, script execution details can be found locally on hello virtual machine on which hello extension was run.</span></span> <span data-ttu-id="ad314-179">Tillägget-specifika felsökning beskrivs i dokumentationen för tillägget-specifika.</span><span class="sxs-lookup"><span data-stu-id="ad314-179">Any extension-specific troubleshooting steps are detailed in extension-specific documentation.</span></span>

<span data-ttu-id="ad314-180">hello följande felsökningssteg gäller tooall tillägg för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ad314-180">hello following troubleshooting steps apply tooall virtual machine extensions.</span></span>

### <a name="view-extension-status"></a><span data-ttu-id="ad314-181">Visa status för tillägg</span><span class="sxs-lookup"><span data-stu-id="ad314-181">View extension status</span></span>

<span data-ttu-id="ad314-182">När ett tillägg för virtuell dator har körts mot en virtuell dator, kan du använda hello följande PowerShell-kommandot tooreturn tillståndets status.</span><span class="sxs-lookup"><span data-stu-id="ad314-182">After a virtual machine extension has been run against a virtual machine, use hello following PowerShell command tooreturn extension status.</span></span> <span data-ttu-id="ad314-183">Ersätt exempel parameternamn med egna värden.</span><span class="sxs-lookup"><span data-stu-id="ad314-183">Replace example parameter names with your own values.</span></span> <span data-ttu-id="ad314-184">Hej `Name` parameter tar hello namn toohello tillägget vid körning.</span><span class="sxs-lookup"><span data-stu-id="ad314-184">hello `Name` parameter takes hello name given toohello extension at execution time.</span></span>

```PowerShell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="ad314-185">Det ser ut så hello följande hello utdata:</span><span class="sxs-lookup"><span data-stu-id="ad314-185">hello output looks like hello following:</span></span>

```json
ResourceGroupName       : myResourceGroup
VMName                  : myVM
Name                    : myExtensionName
Location                : westus
Etag                    : null
Publisher               : Microsoft.Azure.Extensions
ExtensionType           : DockerExtension
TypeHandlerVersion      : 1.0
Id                      : /subscriptions/mySubscriptionIS/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM/extensions/myExtensionName
PublicSettings          :
ProtectedSettings       :
ProvisioningState       : Succeeded
Statuses                :
SubStatuses             :
AutoUpgradeMinorVersion : False
ForceUpdateTag          :
```

<span data-ttu-id="ad314-186">Tillståndets status är körningen kan också finnas i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ad314-186">Extension execution status can also be found in hello Azure portal.</span></span> <span data-ttu-id="ad314-187">tooview hello status för ett tillägg markerar hello virtuell dator, Välj **tillägg**, och välj hello önskade tillägg.</span><span class="sxs-lookup"><span data-stu-id="ad314-187">tooview hello status of an extension, select hello virtual machine, choose **Extensions**, and select hello desired extension.</span></span>

### <a name="rerun-vm-extensions"></a><span data-ttu-id="ad314-188">Kör VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="ad314-188">Rerun VM extensions</span></span>

<span data-ttu-id="ad314-189">Det kan finnas fall där tillägget för virtuell dator måste toobe kör igen.</span><span class="sxs-lookup"><span data-stu-id="ad314-189">There may be cases in which a virtual machine extension needs toobe rerun.</span></span> <span data-ttu-id="ad314-190">Du kan göra detta genom att ta bort hello tillägg och sedan köra hello-tillägget med en körning metod du föredrar.</span><span class="sxs-lookup"><span data-stu-id="ad314-190">You can do this by removing hello extension and then rerunning hello extension with an execution method of your choice.</span></span> <span data-ttu-id="ad314-191">tooremove ett tillägg, kör följande kommando med hello Azure PowerShell-modulen hello.</span><span class="sxs-lookup"><span data-stu-id="ad314-191">tooremove an extension, run hello following command with hello Azure PowerShell module.</span></span> <span data-ttu-id="ad314-192">Ersätt exempel parameternamn med egna värden.</span><span class="sxs-lookup"><span data-stu-id="ad314-192">Replace example parameter names with your own values.</span></span>

```powershell
Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="ad314-193">Ett tillägg kan också tas bort med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ad314-193">An extension can also be removed using hello Azure portal.</span></span> <span data-ttu-id="ad314-194">toodo så:</span><span class="sxs-lookup"><span data-stu-id="ad314-194">toodo so:</span></span>

1. <span data-ttu-id="ad314-195">Välj en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ad314-195">Select a virtual machine.</span></span>
2. <span data-ttu-id="ad314-196">Välj **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="ad314-196">Select **Extensions**.</span></span>
3. <span data-ttu-id="ad314-197">Välj önskad hello tillägg.</span><span class="sxs-lookup"><span data-stu-id="ad314-197">Choose hello desired extension.</span></span>
4. <span data-ttu-id="ad314-198">Välj **avinstallera**.</span><span class="sxs-lookup"><span data-stu-id="ad314-198">Select **Uninstall**.</span></span>

## <a name="common-vm-extensions-reference"></a><span data-ttu-id="ad314-199">Benämningen för VM-tillägg</span><span class="sxs-lookup"><span data-stu-id="ad314-199">Common VM extensions reference</span></span>
| <span data-ttu-id="ad314-200">Tilläggsnamn</span><span class="sxs-lookup"><span data-stu-id="ad314-200">Extension name</span></span> | <span data-ttu-id="ad314-201">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ad314-201">Description</span></span> | <span data-ttu-id="ad314-202">Mer information</span><span class="sxs-lookup"><span data-stu-id="ad314-202">More information</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ad314-203">Tillägget för anpassat skript för Windows</span><span class="sxs-lookup"><span data-stu-id="ad314-203">Custom Script Extension for Windows</span></span> |<span data-ttu-id="ad314-204">Kör skript mot en virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="ad314-204">Run scripts against an Azure virtual machine</span></span> |[<span data-ttu-id="ad314-205">Tillägget för anpassat skript för Windows</span><span class="sxs-lookup"><span data-stu-id="ad314-205">Custom Script Extension for Windows</span></span>](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| <span data-ttu-id="ad314-206">DSC-tillägg för Windows</span><span class="sxs-lookup"><span data-stu-id="ad314-206">DSC Extension for Windows</span></span> |<span data-ttu-id="ad314-207">PowerShell DSC (Desired State Configuration)-tillägg</span><span class="sxs-lookup"><span data-stu-id="ad314-207">PowerShell DSC (Desired State Configuration) Extension</span></span> |[<span data-ttu-id="ad314-208">DSC-tillägg för Windows</span><span class="sxs-lookup"><span data-stu-id="ad314-208">DSC Extension for Windows</span></span>](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| <span data-ttu-id="ad314-209">Azure Diagnostics-tillägg</span><span class="sxs-lookup"><span data-stu-id="ad314-209">Azure Diagnostics Extension</span></span> |<span data-ttu-id="ad314-210">Hantera Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="ad314-210">Manage Azure Diagnostics</span></span> |[<span data-ttu-id="ad314-211">Azure Diagnostics-tillägg</span><span class="sxs-lookup"><span data-stu-id="ad314-211">Azure Diagnostics Extension</span></span>](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| <span data-ttu-id="ad314-212">Tillägget för Azure VM-åtkomst</span><span class="sxs-lookup"><span data-stu-id="ad314-212">Azure VM Access Extension</span></span> |<span data-ttu-id="ad314-213">Hantera användare och autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="ad314-213">Manage users and credentials</span></span> |[<span data-ttu-id="ad314-214">Tillägg för virtuell dator åtkomst för Linux</span><span class="sxs-lookup"><span data-stu-id="ad314-214">VM Access Extension for Linux</span></span>](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |
