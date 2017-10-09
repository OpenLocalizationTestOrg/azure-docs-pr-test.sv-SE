---
title: "aaaCustom skripttillägg på en virtuell Windows-dator | Microsoft Docs"
description: "Automatisera åtgärder för konfiguration av virtuell Azure-dator med hjälp av hello anpassat skript tillägget toorun PowerShell-skript på en fjärransluten Windows VM"
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: ebb7340a-8f61-4d3c-a290-d7bf8de2d0bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: nepeters
ms.openlocfilehash: cf7bb895dd211f07fd010dc03b68cd77df1127b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="custom-script-extension-for-windows-using-hello-classic-deployment-model"></a><span data-ttu-id="322f3-103">Anpassade skript tillägget för Windows med hello klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="322f3-103">Custom Script Extension for Windows using hello classic deployment model</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="322f3-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="322f3-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="322f3-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="322f3-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="322f3-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="322f3-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="322f3-107">Lär dig hur för[utföra dessa steg med hello Resource Manager-modellen](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="322f3-107">Learn how too[perform these steps using hello Resource Manager model](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="322f3-108">hello tillägget för anpassat skript hämtar och kör skript på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="322f3-108">hello Custom Script Extension downloads and executes scripts on Azure virtual machines.</span></span> <span data-ttu-id="322f3-109">Det här tillägget är användbart för post distributionskonfiguration, Programvaruinstallation eller någon annan konfiguration / hanteringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="322f3-109">This extension is useful for post deployment configuration, software installation, or any other configuration / management task.</span></span> <span data-ttu-id="322f3-110">Skript kan hämtas från Azure storage eller GitHub eller tillhandahålls toohello Azure-portalen på tillägget körtiden.</span><span class="sxs-lookup"><span data-stu-id="322f3-110">Scripts can be downloaded from Azure storage or GitHub, or provided toohello Azure portal at extension run time.</span></span> <span data-ttu-id="322f3-111">hello tillägget för anpassat skript kan integreras med Azure Resource Manager-mallar och kan också köras med hello Azure CLI, PowerShell, Azure-portalen eller hello Azure virtuella REST API.</span><span class="sxs-lookup"><span data-stu-id="322f3-111">hello Custom Script extension integrates with Azure Resource Manager templates, and can also be run using hello Azure CLI, PowerShell, Azure portal, or hello Azure Virtual Machine REST API.</span></span>

<span data-ttu-id="322f3-112">Det här dokumentet beskriver hur toouse hello tillägget för anpassat skript med hello Azure PowerShell-modulen, Azure Resource Manager-mallar och information om felsökning i Windows-System.</span><span class="sxs-lookup"><span data-stu-id="322f3-112">This document details how toouse hello Custom Script Extension using hello Azure PowerShell module, Azure Resource Manager templates, and details troubleshooting steps on Windows systems.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="322f3-113">Krav</span><span class="sxs-lookup"><span data-stu-id="322f3-113">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="322f3-114">Operativsystem</span><span class="sxs-lookup"><span data-stu-id="322f3-114">Operating System</span></span>

<span data-ttu-id="322f3-115">hello tillägget för anpassat skript för Windows kan köras mot Windows Server 2008 R2, 2012 och 2012 R2 2016 utgåvor.</span><span class="sxs-lookup"><span data-stu-id="322f3-115">hello Custom Script Extension for Windows can be run against Windows Server 2008 R2, 2012, 2012 R2, and 2016 releases.</span></span>

### <a name="script-location"></a><span data-ttu-id="322f3-116">Skriptets placering</span><span class="sxs-lookup"><span data-stu-id="322f3-116">Script Location</span></span>

<span data-ttu-id="322f3-117">hello skript måste lagras i Azure storage eller någon annan plats som är tillgängligt via en giltig URL toobe.</span><span class="sxs-lookup"><span data-stu-id="322f3-117">hello script needs toobe stored in Azure storage, or any other location accessible through a valid URL.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="322f3-118">Internet-anslutning</span><span class="sxs-lookup"><span data-stu-id="322f3-118">Internet Connectivity</span></span>

<span data-ttu-id="322f3-119">hello anpassade skript tillägget för Windows kräver att hello mål den virtuella datorn är ansluten toohello internet.</span><span class="sxs-lookup"><span data-stu-id="322f3-119">hello Custom Script Extension for Windows requires that hello target virtual machine is connected toohello internet.</span></span> 

## <a name="extension-schema"></a><span data-ttu-id="322f3-120">Tilläggsschema</span><span class="sxs-lookup"><span data-stu-id="322f3-120">Extension schema</span></span>

<span data-ttu-id="322f3-121">hello visar följande JSON hello schemat för hello tillägget för anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="322f3-121">hello following JSON shows hello schema for hello Custom Script Extension.</span></span> <span data-ttu-id="322f3-122">hello tillägget kräver en skript-plats (Azure Storage eller någon annan plats med en giltig URL) och en kommando-tooexecute.</span><span class="sxs-lookup"><span data-stu-id="322f3-122">hello extension requires a script location (Azure Storage or other location with valid URL), and a command tooexecute.</span></span> <span data-ttu-id="322f3-123">Om du använder Azure Storage som hello skriptkälla krävs ett Azure storage-konto namn och åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="322f3-123">If using Azure Storage as hello script source, an Azure storage account name and account key is required.</span></span> <span data-ttu-id="322f3-124">De här objekten ska behandlas som känsliga data och anges i hello tillägg skyddade inställningen konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="322f3-124">These items should be treated as sensitive data and specified in hello extensions protected setting configuration.</span></span> <span data-ttu-id="322f3-125">Azure VM tillägget skyddade inställningsdata krypteras och dekrypteras endast på hello virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="322f3-125">Azure VM extension protected setting data is encrypted, and only decrypted on hello target virtual machine.</span></span>

```json
{
    "name": "config-app",
    "type": "Microsoft.ClassicCompute/virtualMachines/extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-01",
    "properties": {
        "publisher": "Microsoft.Compute",
        "extension": "CustomScriptExtension",
        "version": "1.8",
        "parameters": {
            "public": {
                "fileUris": "[myScriptLocation]"
            },
            "private": {
                "commandToExecute": "[myExecutionString]"
            }
        }
    }
}
```

### <a name="property-values"></a><span data-ttu-id="322f3-126">Egenskapsvärden</span><span class="sxs-lookup"><span data-stu-id="322f3-126">Property values</span></span>

| <span data-ttu-id="322f3-127">Namn</span><span class="sxs-lookup"><span data-stu-id="322f3-127">Name</span></span> | <span data-ttu-id="322f3-128">Värdet / exempel</span><span class="sxs-lookup"><span data-stu-id="322f3-128">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="322f3-129">apiVersion</span><span class="sxs-lookup"><span data-stu-id="322f3-129">apiVersion</span></span> | <span data-ttu-id="322f3-130">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="322f3-130">2015-06-15</span></span> |
| <span data-ttu-id="322f3-131">Publisher</span><span class="sxs-lookup"><span data-stu-id="322f3-131">publisher</span></span> | <span data-ttu-id="322f3-132">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="322f3-132">Microsoft.Compute</span></span> |
| <span data-ttu-id="322f3-133">Tillägg</span><span class="sxs-lookup"><span data-stu-id="322f3-133">extension</span></span> | <span data-ttu-id="322f3-134">CustomScriptExtension</span><span class="sxs-lookup"><span data-stu-id="322f3-134">CustomScriptExtension</span></span> |
| <span data-ttu-id="322f3-135">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="322f3-135">typeHandlerVersion</span></span> | <span data-ttu-id="322f3-136">1.8</span><span class="sxs-lookup"><span data-stu-id="322f3-136">1.8</span></span> |
| <span data-ttu-id="322f3-137">fileUris (t.ex.)</span><span class="sxs-lookup"><span data-stu-id="322f3-137">fileUris (e.g)</span></span> | <span data-ttu-id="322f3-138">https://Raw.githubusercontent.com/Microsoft/DotNet-Core-Sample-Templates/Master/DotNet-Core-Music-Windows/Scripts/Configure-Music-App.ps1</span><span class="sxs-lookup"><span data-stu-id="322f3-138">https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1</span></span> |
| <span data-ttu-id="322f3-139">commandToExecute (t.ex.)</span><span class="sxs-lookup"><span data-stu-id="322f3-139">commandToExecute (e.g)</span></span> | <span data-ttu-id="322f3-140">PowerShell - ExecutionPolicy obegränsad - filen konfigurera-musik-app.ps1</span><span class="sxs-lookup"><span data-stu-id="322f3-140">powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1</span></span> |

## <a name="template-deployment"></a><span data-ttu-id="322f3-141">Malldistribution</span><span class="sxs-lookup"><span data-stu-id="322f3-141">Template deployment</span></span>

<span data-ttu-id="322f3-142">Azure VM-tillägg kan distribueras med Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="322f3-142">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="322f3-143">hello JSON-schema som beskrivs i föregående avsnitt i hello kan användas i en Azure Resource Manager mallen toorun hello tillägget för anpassat skript under en Azure Resource Manager för malldistribution.</span><span class="sxs-lookup"><span data-stu-id="322f3-143">hello JSON schema detailed in hello previous section can be used in an Azure Resource Manager template toorun hello Custom Script Extension during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="322f3-144">En exempelmall som innehåller hello tillägget för anpassat skript hittar du här, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="322f3-144">A sample template that includes hello Custom Script Extension can be found here, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="powershell-deployment"></a><span data-ttu-id="322f3-145">PowerShell-distribution</span><span class="sxs-lookup"><span data-stu-id="322f3-145">PowerShell deployment</span></span>

<span data-ttu-id="322f3-146">Hej `Set-AzureVMCustomScriptExtension` kommandot kan det vara används tooadd hello anpassat skript tillägget tooan befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="322f3-146">hello `Set-AzureVMCustomScriptExtension` command can be used tooadd hello Custom Script extension tooan existing virtual machine.</span></span> <span data-ttu-id="322f3-147">Mer information finns i [Set AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).</span><span class="sxs-lookup"><span data-stu-id="322f3-147">For more information, see [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).</span></span>

```powershell
# create vm object
$vm = Get-AzureVM -Name 2016clas -ServiceName 2016clas1313

# set extension
Set-AzureVMCustomScriptExtension -VM $vm -FileUri myFileUri -Run 'create-file.ps1'

# update vm
$vm | Update-AzureVM
```

## <a name="troubleshoot-and-support"></a><span data-ttu-id="322f3-148">Felsöka och stöd</span><span class="sxs-lookup"><span data-stu-id="322f3-148">Troubleshoot and support</span></span>

### <a name="troubleshoot"></a><span data-ttu-id="322f3-149">Felsöka</span><span class="sxs-lookup"><span data-stu-id="322f3-149">Troubleshoot</span></span>

<span data-ttu-id="322f3-150">Data om hello tillståndet för distributioner av tillägget kan hämtas från hello Azure-portalen och genom att använda hello Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="322f3-150">Data about hello state of extension deployments can be retrieved from hello Azure portal, and by using hello Azure PowerShell module.</span></span> <span data-ttu-id="322f3-151">toosee hello distributionstillstånd för en viss virtuell dator, kör följande kommando hello-tillägg.</span><span class="sxs-lookup"><span data-stu-id="322f3-151">toosee hello deployment state of extensions for a given VM, run hello following command.</span></span>

```powershell
Get-AzureVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="322f3-152">Tillägget körning utdata oss loggade toofiles hittades i hello följande katalog på hello virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="322f3-152">Extension execution output us logged toofiles found in hello following directory on hello target virtual machine.</span></span>

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

<span data-ttu-id="322f3-153">själva hello skriptet hämtas till hello följande katalog på hello virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="322f3-153">hello script itself is downloaded into hello following directory on hello target virtual machine.</span></span>

```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads
```

### <a name="support"></a><span data-ttu-id="322f3-154">Support</span><span class="sxs-lookup"><span data-stu-id="322f3-154">Support</span></span>

<span data-ttu-id="322f3-155">Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på hello [MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="322f3-155">If you need more help at any point in this article, you can contact hello Azure experts on hello [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="322f3-156">Alternativt kan du lagra en incident i Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="322f3-156">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="322f3-157">Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/en-us/support/options/) och välja Get support.</span><span class="sxs-lookup"><span data-stu-id="322f3-157">Go toohello [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="322f3-158">Information om hur du använder Azure stöder läsa hello [vanliga frågor om Microsoft Azure-supporten](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="322f3-158">For information about using Azure Support, read hello [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
