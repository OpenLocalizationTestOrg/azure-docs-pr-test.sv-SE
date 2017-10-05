---
title: "Anpassat skript för Azure-tillägget för Windows | Microsoft Docs"
description: "Automatisera åtgärder för Windows VM-konfigurationen genom att använda tillägget för anpassat skript"
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f4181fee-7a9d-4a1c-b517-52956f5b7fa1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/16/2017
ms.author: nepeters
ms.openlocfilehash: a6f417ea6575b81258998ae3b31c10e9df59b603
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="custom-script-extension-for-windows"></a><span data-ttu-id="2b1a0-103">Tillägget för anpassat skript för Windows</span><span class="sxs-lookup"><span data-stu-id="2b1a0-103">Custom Script Extension for Windows</span></span>

<span data-ttu-id="2b1a0-104">Tillägget för anpassat skript hämtar och kör skript på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-104">The Custom Script Extension downloads and executes scripts on Azure virtual machines.</span></span> <span data-ttu-id="2b1a0-105">Det här tillägget är användbart för post distributionskonfiguration, Programvaruinstallation eller någon annan konfiguration / hanteringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-105">This extension is useful for post deployment configuration, software installation, or any other configuration / management task.</span></span> <span data-ttu-id="2b1a0-106">Skript kan hämtas från Azure storage eller GitHub eller tillhandahålls på Azure-portalen på tillägget körtiden.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-106">Scripts can be downloaded from Azure storage or GitHub, or provided to the Azure portal at extension run time.</span></span> <span data-ttu-id="2b1a0-107">Tillägget för anpassat skript kan integreras med Azure Resource Manager-mallar och kan också köras med hjälp av Azure CLI, PowerShell, Azure-portalen eller Azure Virtual Machine REST API.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-107">The Custom Script extension integrates with Azure Resource Manager templates, and can also be run using the Azure CLI, PowerShell, Azure portal, or the Azure Virtual Machine REST API.</span></span>

<span data-ttu-id="2b1a0-108">Det här dokumentet beskriver hur du använder den tillägget för anpassat skript med hjälp av Azure PowerShell-modulen, Azure Resource Manager-mallar och information om felsökning i Windows-System.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-108">This document details how to use the Custom Script Extension using the Azure PowerShell module, Azure Resource Manager templates, and details troubleshooting steps on Windows systems.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2b1a0-109">Krav</span><span class="sxs-lookup"><span data-stu-id="2b1a0-109">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="2b1a0-110">Operativsystem</span><span class="sxs-lookup"><span data-stu-id="2b1a0-110">Operating System</span></span>

<span data-ttu-id="2b1a0-111">Tillägget för anpassat skript utgåvor för Windows kan köras mot Windows Server 2008 R2, 2012 och 2012 R2 2016.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-111">The Custom Script Extension for Windows can be run against Windows Server 2008 R2, 2012, 2012 R2, and 2016 releases.</span></span>

### <a name="script-location"></a><span data-ttu-id="2b1a0-112">Skriptets placering</span><span class="sxs-lookup"><span data-stu-id="2b1a0-112">Script Location</span></span>

<span data-ttu-id="2b1a0-113">Skriptet måste lagras i Azure Blob storage eller någon annan plats som är tillgängligt via en giltig URL.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-113">The script needs to be stored in Azure Blob storage, or any other location accessible through a valid URL.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="2b1a0-114">Internet-anslutning</span><span class="sxs-lookup"><span data-stu-id="2b1a0-114">Internet Connectivity</span></span>

<span data-ttu-id="2b1a0-115">Anpassade skript tillägget för Windows kräver att den virtuella måldatorn är ansluten till internet.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-115">The Custom Script Extension for Windows requires that the target virtual machine is connected to the internet.</span></span> 

## <a name="extension-schema"></a><span data-ttu-id="2b1a0-116">Tilläggsschema</span><span class="sxs-lookup"><span data-stu-id="2b1a0-116">Extension schema</span></span>

<span data-ttu-id="2b1a0-117">Följande JSON visar schemat för tillägget för anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-117">The following JSON shows the schema for the Custom Script Extension.</span></span> <span data-ttu-id="2b1a0-118">Tillägget kräver en skript-plats (Azure Storage eller någon annan plats med en giltig URL) och ett kommando som ska köras.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-118">The extension requires a script location (Azure Storage or other location with valid URL), and a command to execute.</span></span> <span data-ttu-id="2b1a0-119">Om du använder Azure Storage som skriptkälla krävs ett Azure storage-konto namn och åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-119">If using Azure Storage as the script source, an Azure storage account name and account key is required.</span></span> <span data-ttu-id="2b1a0-120">De här objekten ska behandlas som känsliga data och angetts i konfigurationen för skyddade inställningen tillägg.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-120">These items should be treated as sensitive data and specified in the extensions protected setting configuration.</span></span> <span data-ttu-id="2b1a0-121">Azure för VM-tillägget skyddade inställningsdata krypteras och dekrypteras endast på den virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-121">Azure VM extension protected setting data is encrypted, and only decrypted on the target virtual machine.</span></span>

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
        "typeHandlerVersion": "1.9",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "fileUris": [
                "script location"
            ]
        },
        "protectedSettings": {
            "commandToExecute": "myExecutionCommand",
            "storageAccountName": "myStorageAccountName",
            "storageAccountKey": "myStorageAccountKey"
        }
    }
}
```

### <a name="property-values"></a><span data-ttu-id="2b1a0-122">Egenskapsvärden</span><span class="sxs-lookup"><span data-stu-id="2b1a0-122">Property values</span></span>

| <span data-ttu-id="2b1a0-123">Namn</span><span class="sxs-lookup"><span data-stu-id="2b1a0-123">Name</span></span> | <span data-ttu-id="2b1a0-124">Värdet / exempel</span><span class="sxs-lookup"><span data-stu-id="2b1a0-124">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="2b1a0-125">apiVersion</span><span class="sxs-lookup"><span data-stu-id="2b1a0-125">apiVersion</span></span> | <span data-ttu-id="2b1a0-126">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="2b1a0-126">2015-06-15</span></span> |
| <span data-ttu-id="2b1a0-127">Publisher</span><span class="sxs-lookup"><span data-stu-id="2b1a0-127">publisher</span></span> | <span data-ttu-id="2b1a0-128">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="2b1a0-128">Microsoft.Compute</span></span> |
| <span data-ttu-id="2b1a0-129">typ</span><span class="sxs-lookup"><span data-stu-id="2b1a0-129">type</span></span> | <span data-ttu-id="2b1a0-130">Tillägg</span><span class="sxs-lookup"><span data-stu-id="2b1a0-130">extensions</span></span> |
| <span data-ttu-id="2b1a0-131">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="2b1a0-131">typeHandlerVersion</span></span> | <span data-ttu-id="2b1a0-132">1.9</span><span class="sxs-lookup"><span data-stu-id="2b1a0-132">1.9</span></span> |
| <span data-ttu-id="2b1a0-133">fileUris (t.ex.)</span><span class="sxs-lookup"><span data-stu-id="2b1a0-133">fileUris (e.g)</span></span> | <span data-ttu-id="2b1a0-134">https://Raw.githubusercontent.com/Microsoft/DotNet-Core-Sample-Templates/Master/DotNet-Core-Music-Windows/Scripts/Configure-Music-App.ps1</span><span class="sxs-lookup"><span data-stu-id="2b1a0-134">https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1</span></span> |
| <span data-ttu-id="2b1a0-135">commandToExecute (t.ex.)</span><span class="sxs-lookup"><span data-stu-id="2b1a0-135">commandToExecute (e.g)</span></span> | <span data-ttu-id="2b1a0-136">PowerShell - ExecutionPolicy obegränsad - filen konfigurera-musik-app.ps1</span><span class="sxs-lookup"><span data-stu-id="2b1a0-136">powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1</span></span> |
| <span data-ttu-id="2b1a0-137">storageAccountName (t.ex.)</span><span class="sxs-lookup"><span data-stu-id="2b1a0-137">storageAccountName (e.g)</span></span> | <span data-ttu-id="2b1a0-138">examplestorageacct</span><span class="sxs-lookup"><span data-stu-id="2b1a0-138">examplestorageacct</span></span> |
| <span data-ttu-id="2b1a0-139">storageAccountKey (t.ex.)</span><span class="sxs-lookup"><span data-stu-id="2b1a0-139">storageAccountKey (e.g)</span></span> | <span data-ttu-id="2b1a0-140">TmJK/1N3AbAZ3q / + hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg ==</span><span class="sxs-lookup"><span data-stu-id="2b1a0-140">TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg==</span></span> |

<span data-ttu-id="2b1a0-141">**Obs** -dessa egenskapsnamn är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-141">**Note** - these property names are case sensitive.</span></span> <span data-ttu-id="2b1a0-142">Använda namnen som visas ovan för att undvika distributionsproblem.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-142">Use the names as seen above to avoid deployment issues.</span></span>

## <a name="template-deployment"></a><span data-ttu-id="2b1a0-143">Malldistribution</span><span class="sxs-lookup"><span data-stu-id="2b1a0-143">Template deployment</span></span>

<span data-ttu-id="2b1a0-144">Azure VM-tillägg kan distribueras med Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-144">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="2b1a0-145">JSON-schema som beskrivs i föregående avsnitt kan användas i en Azure Resource Manager-mall för att köra tillägget för anpassat skript under en Azure Resource Manager för malldistribution.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-145">The JSON schema detailed in the previous section can be used in an Azure Resource Manager template to run the Custom Script Extension during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="2b1a0-146">En exempelmall som innehåller tillägget för anpassat skript hittar du här, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="2b1a0-146">A sample template that includes the Custom Script Extension can be found here, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="powershell-deployment"></a><span data-ttu-id="2b1a0-147">PowerShell-distribution</span><span class="sxs-lookup"><span data-stu-id="2b1a0-147">PowerShell deployment</span></span>

<span data-ttu-id="2b1a0-148">Den `Set-AzureRmVMCustomScriptExtension` kommando kan användas för att lägga till tillägget för anpassat skript till en befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-148">The `Set-AzureRmVMCustomScriptExtension` command can be used to add the Custom Script extension to an existing virtual machine.</span></span> <span data-ttu-id="2b1a0-149">Mer information finns i [Set AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).</span><span class="sxs-lookup"><span data-stu-id="2b1a0-149">For more information, see [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).</span></span>
```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location myLocation `
    -FileUri myURL `
    -Run 'myScript.ps1' `
    -Name DemoScriptExtension
```

## <a name="troubleshoot-and-support"></a><span data-ttu-id="2b1a0-150">Felsöka och stöd</span><span class="sxs-lookup"><span data-stu-id="2b1a0-150">Troubleshoot and support</span></span>

### <a name="troubleshoot"></a><span data-ttu-id="2b1a0-151">Felsöka</span><span class="sxs-lookup"><span data-stu-id="2b1a0-151">Troubleshoot</span></span>

<span data-ttu-id="2b1a0-152">Data om tillståndet för distributioner av tillägget kan hämtas från Azure-portalen och genom att använda Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-152">Data about the state of extension deployments can be retrieved from the Azure portal, and by using the Azure PowerShell module.</span></span> <span data-ttu-id="2b1a0-153">Kör följande kommando för att se distributionsstatusen för tillägg för en viss virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-153">To see the deployment state of extensions for a given VM, run the following command.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="2b1a0-154">Tillägget utförande-utdatan loggas till filer som finns i följande katalog på den virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-154">Extension execution output is logged to files found under the following directory on the target virtual machine.</span></span>
```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

<span data-ttu-id="2b1a0-155">De angivna filerna laddas ned till följande katalog på den virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-155">The specified files are downloaded into the following directory on the target virtual machine.</span></span>
```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```
<span data-ttu-id="2b1a0-156">där `<n>` är ett heltal som kan förändras mellan körningar av tillägget.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-156">where `<n>` is a decimal integer which may change between executions of the extension.</span></span>  <span data-ttu-id="2b1a0-157">Den `1.*` värdet matchar den faktiska, aktuellt `typeHandlerVersion` värdet för tillägget.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-157">The `1.*` value matches the actual, current `typeHandlerVersion` value of the extension.</span></span>  <span data-ttu-id="2b1a0-158">Den faktiska katalogen kan till exempel vara `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-158">For example, the actual directory could be `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.</span></span>  

<span data-ttu-id="2b1a0-159">När du kör den `commandToExecute` kommandot tillägget har angett den här katalogen (t.ex. `...\Downloads\2`) som den aktuella arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-159">When executing the `commandToExecute` command, the extension will have set this directory (e.g., `...\Downloads\2`) as the current working directory.</span></span> <span data-ttu-id="2b1a0-160">Detta kan du använda relativa sökvägar för filer som hämtas den `fileURIs` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-160">This enables the use of relative paths to locate the files downloaded via the `fileURIs` property.</span></span> <span data-ttu-id="2b1a0-161">Se tabellen nedan som exempel.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-161">See the table below for examples.</span></span>

<span data-ttu-id="2b1a0-162">Eftersom absoluta sökvägen kan variera över tid, är det bättre att välja relativa skriptfilen sökvägarna i den `commandToExecute` sträng, när det är möjligt.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-162">Since the absolute download path may vary over time, it is better to opt for relative script/file paths in the `commandToExecute` string, whenever possible.</span></span> <span data-ttu-id="2b1a0-163">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2b1a0-163">For example:</span></span>
```json
    "commandToExecute": "powershell.exe . . . -File './scripts/myscript.ps1'"
```

<span data-ttu-id="2b1a0-164">Information om sökvägen efter det första segmentet i URI: N finns kvar för filer som hämtas den `fileUris` egenskapslistan.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-164">Path information after the first URI segment is retained for files downloaded via the `fileUris` property list.</span></span>  <span data-ttu-id="2b1a0-165">I tabellen nedan visas nedladdade filer ska mappas till hämta underkataloger återspeglar strukturen för de `fileUris` värden.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-165">As shown in the table below, downloaded files are mapped into download subdirectories to reflect the structure of the `fileUris` values.</span></span>  

#### <a name="examples-of-downloaded-files"></a><span data-ttu-id="2b1a0-166">Exempel på filer som hämtas</span><span class="sxs-lookup"><span data-stu-id="2b1a0-166">Examples of Downloaded Files</span></span>

| <span data-ttu-id="2b1a0-167">URI i fileUris</span><span class="sxs-lookup"><span data-stu-id="2b1a0-167">URI in fileUris</span></span> | <span data-ttu-id="2b1a0-168">Relativa hämtade plats</span><span class="sxs-lookup"><span data-stu-id="2b1a0-168">Relative downloaded location</span></span> | <span data-ttu-id="2b1a0-169">Absolut ned *</span><span class="sxs-lookup"><span data-stu-id="2b1a0-169">Absolute downloaded location *</span></span> |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

<span data-ttu-id="2b1a0-170">\*Som ändras ovan, de absoluta sökvägarna under livslängd för den virtuella datorn, men inte i en enda körning av CustomScript-tillägg.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-170">\* As above, the absolute directory paths will change over the lifetime of the VM, but not within a single execution of the CustomScript extension.</span></span>

### <a name="support"></a><span data-ttu-id="2b1a0-171">Support</span><span class="sxs-lookup"><span data-stu-id="2b1a0-171">Support</span></span>

<span data-ttu-id="2b1a0-172">Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta Azure experter på [MSDN Azure och Stack Overflow forumen] (https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="2b1a0-172">If you need more help at any point in this article, you can contact the Azure experts on the [MSDN Azure and Stack Overflow forums] (https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="2b1a0-173">Alternativt kan du lagra en incident i Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-173">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="2b1a0-174">Gå till den [Azure supportwebbplats](https://azure.microsoft.com/en-us/support/options/) och välja Get support.</span><span class="sxs-lookup"><span data-stu-id="2b1a0-174">Go to the [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="2b1a0-175">Information om hur du använder Azure stöder finns i [vanliga frågor om Microsoft Azure-supporten](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="2b1a0-175">For information about using Azure Support, read the [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
