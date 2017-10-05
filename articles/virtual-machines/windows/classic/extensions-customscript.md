---
title: "Tillägget för anpassat skript på en virtuell Windows-dator | Microsoft Docs"
description: "Automatisera åtgärder för konfiguration av virtuell Azure-dator med hjälp av tillägget för anpassat skript för att köra PowerShell-skript på en fjärransluten Windows VM"
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
ms.openlocfilehash: 986ab1025dc188cd7fa1cf8b131a9d4b859be8f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="custom-script-extension-for-windows-using-the-classic-deployment-model"></a><span data-ttu-id="01f9f-103">Anpassade skript tillägget för Windows med hjälp av den klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="01f9f-103">Custom Script Extension for Windows using the classic deployment model</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="01f9f-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="01f9f-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="01f9f-105">Den här artikeln täcker den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="01f9f-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="01f9f-106">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="01f9f-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="01f9f-107">Lär dig hur du [utför dessa steg med hjälp av Resource Manager-modellen](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="01f9f-107">Learn how to [perform these steps using the Resource Manager model](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="01f9f-108">Tillägget för anpassat skript hämtar och kör skript på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="01f9f-108">The Custom Script Extension downloads and executes scripts on Azure virtual machines.</span></span> <span data-ttu-id="01f9f-109">Det här tillägget är användbart för post distributionskonfiguration, Programvaruinstallation eller någon annan konfiguration / hanteringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="01f9f-109">This extension is useful for post deployment configuration, software installation, or any other configuration / management task.</span></span> <span data-ttu-id="01f9f-110">Skript kan hämtas från Azure storage eller GitHub eller tillhandahålls på Azure-portalen på tillägget körtiden.</span><span class="sxs-lookup"><span data-stu-id="01f9f-110">Scripts can be downloaded from Azure storage or GitHub, or provided to the Azure portal at extension run time.</span></span> <span data-ttu-id="01f9f-111">Tillägget för anpassat skript kan integreras med Azure Resource Manager-mallar och kan också köras med hjälp av Azure CLI, PowerShell, Azure-portalen eller Azure Virtual Machine REST API.</span><span class="sxs-lookup"><span data-stu-id="01f9f-111">The Custom Script extension integrates with Azure Resource Manager templates, and can also be run using the Azure CLI, PowerShell, Azure portal, or the Azure Virtual Machine REST API.</span></span>

<span data-ttu-id="01f9f-112">Det här dokumentet beskriver hur du använder den tillägget för anpassat skript med hjälp av Azure PowerShell-modulen, Azure Resource Manager-mallar och information om felsökning i Windows-System.</span><span class="sxs-lookup"><span data-stu-id="01f9f-112">This document details how to use the Custom Script Extension using the Azure PowerShell module, Azure Resource Manager templates, and details troubleshooting steps on Windows systems.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="01f9f-113">Krav</span><span class="sxs-lookup"><span data-stu-id="01f9f-113">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="01f9f-114">Operativsystem</span><span class="sxs-lookup"><span data-stu-id="01f9f-114">Operating System</span></span>

<span data-ttu-id="01f9f-115">Tillägget för anpassat skript utgåvor för Windows kan köras mot Windows Server 2008 R2, 2012 och 2012 R2 2016.</span><span class="sxs-lookup"><span data-stu-id="01f9f-115">The Custom Script Extension for Windows can be run against Windows Server 2008 R2, 2012, 2012 R2, and 2016 releases.</span></span>

### <a name="script-location"></a><span data-ttu-id="01f9f-116">Skriptets placering</span><span class="sxs-lookup"><span data-stu-id="01f9f-116">Script Location</span></span>

<span data-ttu-id="01f9f-117">Skriptet måste lagras i Azure storage eller någon annan plats som är tillgängligt via en giltig URL.</span><span class="sxs-lookup"><span data-stu-id="01f9f-117">The script needs to be stored in Azure storage, or any other location accessible through a valid URL.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="01f9f-118">Internet-anslutning</span><span class="sxs-lookup"><span data-stu-id="01f9f-118">Internet Connectivity</span></span>

<span data-ttu-id="01f9f-119">Anpassade skript tillägget för Windows kräver att den virtuella måldatorn är ansluten till internet.</span><span class="sxs-lookup"><span data-stu-id="01f9f-119">The Custom Script Extension for Windows requires that the target virtual machine is connected to the internet.</span></span> 

## <a name="extension-schema"></a><span data-ttu-id="01f9f-120">Tilläggsschema</span><span class="sxs-lookup"><span data-stu-id="01f9f-120">Extension schema</span></span>

<span data-ttu-id="01f9f-121">Följande JSON visar schemat för tillägget för anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="01f9f-121">The following JSON shows the schema for the Custom Script Extension.</span></span> <span data-ttu-id="01f9f-122">Tillägget kräver en skript-plats (Azure Storage eller någon annan plats med en giltig URL) och ett kommando som ska köras.</span><span class="sxs-lookup"><span data-stu-id="01f9f-122">The extension requires a script location (Azure Storage or other location with valid URL), and a command to execute.</span></span> <span data-ttu-id="01f9f-123">Om du använder Azure Storage som skriptkälla krävs ett Azure storage-konto namn och åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="01f9f-123">If using Azure Storage as the script source, an Azure storage account name and account key is required.</span></span> <span data-ttu-id="01f9f-124">De här objekten ska behandlas som känsliga data och angetts i konfigurationen för skyddade inställningen tillägg.</span><span class="sxs-lookup"><span data-stu-id="01f9f-124">These items should be treated as sensitive data and specified in the extensions protected setting configuration.</span></span> <span data-ttu-id="01f9f-125">Azure för VM-tillägget skyddade inställningsdata krypteras och dekrypteras endast på den virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="01f9f-125">Azure VM extension protected setting data is encrypted, and only decrypted on the target virtual machine.</span></span>

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

### <a name="property-values"></a><span data-ttu-id="01f9f-126">Egenskapsvärden</span><span class="sxs-lookup"><span data-stu-id="01f9f-126">Property values</span></span>

| <span data-ttu-id="01f9f-127">Namn</span><span class="sxs-lookup"><span data-stu-id="01f9f-127">Name</span></span> | <span data-ttu-id="01f9f-128">Värdet / exempel</span><span class="sxs-lookup"><span data-stu-id="01f9f-128">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="01f9f-129">apiVersion</span><span class="sxs-lookup"><span data-stu-id="01f9f-129">apiVersion</span></span> | <span data-ttu-id="01f9f-130">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="01f9f-130">2015-06-15</span></span> |
| <span data-ttu-id="01f9f-131">Publisher</span><span class="sxs-lookup"><span data-stu-id="01f9f-131">publisher</span></span> | <span data-ttu-id="01f9f-132">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="01f9f-132">Microsoft.Compute</span></span> |
| <span data-ttu-id="01f9f-133">Tillägg</span><span class="sxs-lookup"><span data-stu-id="01f9f-133">extension</span></span> | <span data-ttu-id="01f9f-134">CustomScriptExtension</span><span class="sxs-lookup"><span data-stu-id="01f9f-134">CustomScriptExtension</span></span> |
| <span data-ttu-id="01f9f-135">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="01f9f-135">typeHandlerVersion</span></span> | <span data-ttu-id="01f9f-136">1.8</span><span class="sxs-lookup"><span data-stu-id="01f9f-136">1.8</span></span> |
| <span data-ttu-id="01f9f-137">fileUris (t.ex.)</span><span class="sxs-lookup"><span data-stu-id="01f9f-137">fileUris (e.g)</span></span> | <span data-ttu-id="01f9f-138">https://Raw.githubusercontent.com/Microsoft/DotNet-Core-Sample-Templates/Master/DotNet-Core-Music-Windows/Scripts/Configure-Music-App.ps1</span><span class="sxs-lookup"><span data-stu-id="01f9f-138">https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1</span></span> |
| <span data-ttu-id="01f9f-139">commandToExecute (t.ex.)</span><span class="sxs-lookup"><span data-stu-id="01f9f-139">commandToExecute (e.g)</span></span> | <span data-ttu-id="01f9f-140">PowerShell - ExecutionPolicy obegränsad - filen konfigurera-musik-app.ps1</span><span class="sxs-lookup"><span data-stu-id="01f9f-140">powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1</span></span> |

## <a name="template-deployment"></a><span data-ttu-id="01f9f-141">Malldistribution</span><span class="sxs-lookup"><span data-stu-id="01f9f-141">Template deployment</span></span>

<span data-ttu-id="01f9f-142">Azure VM-tillägg kan distribueras med Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="01f9f-142">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="01f9f-143">JSON-schema som beskrivs i föregående avsnitt kan användas i en Azure Resource Manager-mall för att köra tillägget för anpassat skript under en Azure Resource Manager för malldistribution.</span><span class="sxs-lookup"><span data-stu-id="01f9f-143">The JSON schema detailed in the previous section can be used in an Azure Resource Manager template to run the Custom Script Extension during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="01f9f-144">En exempelmall som innehåller tillägget för anpassat skript hittar du här, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="01f9f-144">A sample template that includes the Custom Script Extension can be found here, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="powershell-deployment"></a><span data-ttu-id="01f9f-145">PowerShell-distribution</span><span class="sxs-lookup"><span data-stu-id="01f9f-145">PowerShell deployment</span></span>

<span data-ttu-id="01f9f-146">Den `Set-AzureVMCustomScriptExtension` kommando kan användas för att lägga till tillägget för anpassat skript till en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="01f9f-146">The `Set-AzureVMCustomScriptExtension` command can be used to add the Custom Script extension to an existing virtual machine.</span></span> <span data-ttu-id="01f9f-147">Mer information finns i [Set AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).</span><span class="sxs-lookup"><span data-stu-id="01f9f-147">For more information, see [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).</span></span>

```powershell
# create vm object
$vm = Get-AzureVM -Name 2016clas -ServiceName 2016clas1313

# set extension
Set-AzureVMCustomScriptExtension -VM $vm -FileUri myFileUri -Run 'create-file.ps1'

# update vm
$vm | Update-AzureVM
```

## <a name="troubleshoot-and-support"></a><span data-ttu-id="01f9f-148">Felsöka och stöd</span><span class="sxs-lookup"><span data-stu-id="01f9f-148">Troubleshoot and support</span></span>

### <a name="troubleshoot"></a><span data-ttu-id="01f9f-149">Felsöka</span><span class="sxs-lookup"><span data-stu-id="01f9f-149">Troubleshoot</span></span>

<span data-ttu-id="01f9f-150">Data om tillståndet för distributioner av tillägget kan hämtas från Azure-portalen och genom att använda Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="01f9f-150">Data about the state of extension deployments can be retrieved from the Azure portal, and by using the Azure PowerShell module.</span></span> <span data-ttu-id="01f9f-151">Kör följande kommando för att se distributionsstatusen för tillägg för en viss virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="01f9f-151">To see the deployment state of extensions for a given VM, run the following command.</span></span>

```powershell
Get-AzureVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="01f9f-152">Tillägget körning utdata oss inloggade filerna i följande katalog på den virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="01f9f-152">Extension execution output us logged to files found in the following directory on the target virtual machine.</span></span>

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

<span data-ttu-id="01f9f-153">Själva skriptet hämtas till följande katalog på den virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="01f9f-153">The script itself is downloaded into the following directory on the target virtual machine.</span></span>

```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads
```

### <a name="support"></a><span data-ttu-id="01f9f-154">Support</span><span class="sxs-lookup"><span data-stu-id="01f9f-154">Support</span></span>

<span data-ttu-id="01f9f-155">Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta Azure-experter på den [MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="01f9f-155">If you need more help at any point in this article, you can contact the Azure experts on the [MSDN Azure and Stack Overflow forums](https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="01f9f-156">Alternativt kan du lagra en incident i Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="01f9f-156">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="01f9f-157">Gå till den [Azure supportwebbplats](https://azure.microsoft.com/en-us/support/options/) och välja Get support.</span><span class="sxs-lookup"><span data-stu-id="01f9f-157">Go to the [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="01f9f-158">Information om hur du använder Azure stöder finns i [vanliga frågor om Microsoft Azure-supporten](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="01f9f-158">For information about using Azure Support, read the [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>