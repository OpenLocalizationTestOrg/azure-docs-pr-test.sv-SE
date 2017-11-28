---
title: "aaaAzure anpassade skript tillägget för Windows | Microsoft Docs"
description: "Automatisera åtgärder för Windows VM-konfigurationen med hjälp av hello tillägget för anpassat skript"
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
ms.openlocfilehash: 97e065242e9fed116ee20b074f4e302a0cd10585
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="custom-script-extension-for-windows"></a><span data-ttu-id="641ee-103">Tillägget för anpassat skript för Windows</span><span class="sxs-lookup"><span data-stu-id="641ee-103">Custom Script Extension for Windows</span></span>

<span data-ttu-id="641ee-104">hello tillägget för anpassat skript hämtar och kör skript på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="641ee-104">hello Custom Script Extension downloads and executes scripts on Azure virtual machines.</span></span> <span data-ttu-id="641ee-105">Det här tillägget är användbart för post distributionskonfiguration, Programvaruinstallation eller någon annan konfiguration / hanteringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="641ee-105">This extension is useful for post deployment configuration, software installation, or any other configuration / management task.</span></span> <span data-ttu-id="641ee-106">Skript kan hämtas från Azure storage eller GitHub eller tillhandahålls toohello Azure-portalen på tillägget körtiden.</span><span class="sxs-lookup"><span data-stu-id="641ee-106">Scripts can be downloaded from Azure storage or GitHub, or provided toohello Azure portal at extension run time.</span></span> <span data-ttu-id="641ee-107">hello tillägget för anpassat skript kan integreras med Azure Resource Manager-mallar och kan också köras med hello Azure CLI, PowerShell, Azure-portalen eller hello Azure virtuella REST API.</span><span class="sxs-lookup"><span data-stu-id="641ee-107">hello Custom Script extension integrates with Azure Resource Manager templates, and can also be run using hello Azure CLI, PowerShell, Azure portal, or hello Azure Virtual Machine REST API.</span></span>

<span data-ttu-id="641ee-108">Det här dokumentet beskriver hur toouse hello tillägget för anpassat skript med hello Azure PowerShell-modulen, Azure Resource Manager-mallar och information om felsökning i Windows-System.</span><span class="sxs-lookup"><span data-stu-id="641ee-108">This document details how toouse hello Custom Script Extension using hello Azure PowerShell module, Azure Resource Manager templates, and details troubleshooting steps on Windows systems.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="641ee-109">Krav</span><span class="sxs-lookup"><span data-stu-id="641ee-109">Prerequisites</span></span>

### <a name="operating-system"></a><span data-ttu-id="641ee-110">Operativsystem</span><span class="sxs-lookup"><span data-stu-id="641ee-110">Operating System</span></span>

<span data-ttu-id="641ee-111">hello tillägget för anpassat skript för Windows kan köras mot Windows Server 2008 R2, 2012 och 2012 R2 2016 utgåvor.</span><span class="sxs-lookup"><span data-stu-id="641ee-111">hello Custom Script Extension for Windows can be run against Windows Server 2008 R2, 2012, 2012 R2, and 2016 releases.</span></span>

### <a name="script-location"></a><span data-ttu-id="641ee-112">Skriptets placering</span><span class="sxs-lookup"><span data-stu-id="641ee-112">Script Location</span></span>

<span data-ttu-id="641ee-113">hello skript måste lagras i Azure Blob storage, eller någon annan plats som är tillgängligt via en giltig URL toobe.</span><span class="sxs-lookup"><span data-stu-id="641ee-113">hello script needs toobe stored in Azure Blob storage, or any other location accessible through a valid URL.</span></span>

### <a name="internet-connectivity"></a><span data-ttu-id="641ee-114">Internet-anslutning</span><span class="sxs-lookup"><span data-stu-id="641ee-114">Internet Connectivity</span></span>

<span data-ttu-id="641ee-115">hello anpassade skript tillägget för Windows kräver att hello mål den virtuella datorn är ansluten toohello internet.</span><span class="sxs-lookup"><span data-stu-id="641ee-115">hello Custom Script Extension for Windows requires that hello target virtual machine is connected toohello internet.</span></span> 

## <a name="extension-schema"></a><span data-ttu-id="641ee-116">Tilläggsschema</span><span class="sxs-lookup"><span data-stu-id="641ee-116">Extension schema</span></span>

<span data-ttu-id="641ee-117">hello visar följande JSON hello schemat för hello tillägget för anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="641ee-117">hello following JSON shows hello schema for hello Custom Script Extension.</span></span> <span data-ttu-id="641ee-118">hello tillägget kräver en skript-plats (Azure Storage eller någon annan plats med en giltig URL) och en kommando-tooexecute.</span><span class="sxs-lookup"><span data-stu-id="641ee-118">hello extension requires a script location (Azure Storage or other location with valid URL), and a command tooexecute.</span></span> <span data-ttu-id="641ee-119">Om du använder Azure Storage som hello skriptkälla krävs ett Azure storage-konto namn och åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="641ee-119">If using Azure Storage as hello script source, an Azure storage account name and account key is required.</span></span> <span data-ttu-id="641ee-120">De här objekten ska behandlas som känsliga data och anges i hello tillägg skyddade inställningen konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="641ee-120">These items should be treated as sensitive data and specified in hello extensions protected setting configuration.</span></span> <span data-ttu-id="641ee-121">Azure VM tillägget skyddade inställningsdata krypteras och dekrypteras endast på hello virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="641ee-121">Azure VM extension protected setting data is encrypted, and only decrypted on hello target virtual machine.</span></span>

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

### <a name="property-values"></a><span data-ttu-id="641ee-122">Egenskapsvärden</span><span class="sxs-lookup"><span data-stu-id="641ee-122">Property values</span></span>

| <span data-ttu-id="641ee-123">Namn</span><span class="sxs-lookup"><span data-stu-id="641ee-123">Name</span></span> | <span data-ttu-id="641ee-124">Värdet / exempel</span><span class="sxs-lookup"><span data-stu-id="641ee-124">Value / Example</span></span> |
| ---- | ---- |
| <span data-ttu-id="641ee-125">apiVersion</span><span class="sxs-lookup"><span data-stu-id="641ee-125">apiVersion</span></span> | <span data-ttu-id="641ee-126">2015-06-15</span><span class="sxs-lookup"><span data-stu-id="641ee-126">2015-06-15</span></span> |
| <span data-ttu-id="641ee-127">Publisher</span><span class="sxs-lookup"><span data-stu-id="641ee-127">publisher</span></span> | <span data-ttu-id="641ee-128">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="641ee-128">Microsoft.Compute</span></span> |
| <span data-ttu-id="641ee-129">typ</span><span class="sxs-lookup"><span data-stu-id="641ee-129">type</span></span> | <span data-ttu-id="641ee-130">Tillägg</span><span class="sxs-lookup"><span data-stu-id="641ee-130">extensions</span></span> |
| <span data-ttu-id="641ee-131">typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="641ee-131">typeHandlerVersion</span></span> | <span data-ttu-id="641ee-132">1.9</span><span class="sxs-lookup"><span data-stu-id="641ee-132">1.9</span></span> |
| <span data-ttu-id="641ee-133">fileUris (t.ex.)</span><span class="sxs-lookup"><span data-stu-id="641ee-133">fileUris (e.g)</span></span> | <span data-ttu-id="641ee-134">https://Raw.githubusercontent.com/Microsoft/DotNet-Core-Sample-Templates/Master/DotNet-Core-Music-Windows/Scripts/Configure-Music-App.ps1</span><span class="sxs-lookup"><span data-stu-id="641ee-134">https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1</span></span> |
| <span data-ttu-id="641ee-135">commandToExecute (t.ex.)</span><span class="sxs-lookup"><span data-stu-id="641ee-135">commandToExecute (e.g)</span></span> | <span data-ttu-id="641ee-136">PowerShell - ExecutionPolicy obegränsad - filen konfigurera-musik-app.ps1</span><span class="sxs-lookup"><span data-stu-id="641ee-136">powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1</span></span> |
| <span data-ttu-id="641ee-137">storageAccountName (t.ex.)</span><span class="sxs-lookup"><span data-stu-id="641ee-137">storageAccountName (e.g)</span></span> | <span data-ttu-id="641ee-138">examplestorageacct</span><span class="sxs-lookup"><span data-stu-id="641ee-138">examplestorageacct</span></span> |
| <span data-ttu-id="641ee-139">storageAccountKey (t.ex.)</span><span class="sxs-lookup"><span data-stu-id="641ee-139">storageAccountKey (e.g)</span></span> | <span data-ttu-id="641ee-140">TmJK/1N3AbAZ3q / + hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg ==</span><span class="sxs-lookup"><span data-stu-id="641ee-140">TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg==</span></span> |

<span data-ttu-id="641ee-141">**Obs** -dessa egenskapsnamn är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="641ee-141">**Note** - these property names are case sensitive.</span></span> <span data-ttu-id="641ee-142">Använd hello namn som visas ovanför tooavoid distributionsproblem.</span><span class="sxs-lookup"><span data-stu-id="641ee-142">Use hello names as seen above tooavoid deployment issues.</span></span>

## <a name="template-deployment"></a><span data-ttu-id="641ee-143">Malldistribution</span><span class="sxs-lookup"><span data-stu-id="641ee-143">Template deployment</span></span>

<span data-ttu-id="641ee-144">Azure VM-tillägg kan distribueras med Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="641ee-144">Azure VM extensions can be deployed with Azure Resource Manager templates.</span></span> <span data-ttu-id="641ee-145">hello JSON-schema som beskrivs i föregående avsnitt i hello kan användas i en Azure Resource Manager mallen toorun hello tillägget för anpassat skript under en Azure Resource Manager för malldistribution.</span><span class="sxs-lookup"><span data-stu-id="641ee-145">hello JSON schema detailed in hello previous section can be used in an Azure Resource Manager template toorun hello Custom Script Extension during an Azure Resource Manager template deployment.</span></span> <span data-ttu-id="641ee-146">En exempelmall som innehåller hello tillägget för anpassat skript hittar du här, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="641ee-146">A sample template that includes hello Custom Script Extension can be found here, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="powershell-deployment"></a><span data-ttu-id="641ee-147">PowerShell-distribution</span><span class="sxs-lookup"><span data-stu-id="641ee-147">PowerShell deployment</span></span>

<span data-ttu-id="641ee-148">Hej `Set-AzureRmVMCustomScriptExtension` kommandot kan det vara används tooadd hello anpassat skript tillägget tooan befintlig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="641ee-148">hello `Set-AzureRmVMCustomScriptExtension` command can be used tooadd hello Custom Script extension tooan existing virtual machine.</span></span> <span data-ttu-id="641ee-149">Mer information finns i [Set AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).</span><span class="sxs-lookup"><span data-stu-id="641ee-149">For more information, see [Set-AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).</span></span>
```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location myLocation `
    -FileUri myURL `
    -Run 'myScript.ps1' `
    -Name DemoScriptExtension
```

## <a name="troubleshoot-and-support"></a><span data-ttu-id="641ee-150">Felsöka och stöd</span><span class="sxs-lookup"><span data-stu-id="641ee-150">Troubleshoot and support</span></span>

### <a name="troubleshoot"></a><span data-ttu-id="641ee-151">Felsöka</span><span class="sxs-lookup"><span data-stu-id="641ee-151">Troubleshoot</span></span>

<span data-ttu-id="641ee-152">Data om hello tillståndet för distributioner av tillägget kan hämtas från hello Azure-portalen och genom att använda hello Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="641ee-152">Data about hello state of extension deployments can be retrieved from hello Azure portal, and by using hello Azure PowerShell module.</span></span> <span data-ttu-id="641ee-153">toosee hello distributionstillstånd för en viss virtuell dator, kör följande kommando hello-tillägg.</span><span class="sxs-lookup"><span data-stu-id="641ee-153">toosee hello deployment state of extensions for a given VM, run hello following command.</span></span>

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

<span data-ttu-id="641ee-154">Tillägget körning utdata är loggade toofiles hittades under hello följande katalog på hello virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="641ee-154">Extension execution output is logged toofiles found under hello following directory on hello target virtual machine.</span></span>
```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

<span data-ttu-id="641ee-155">hello angetts filerna laddas ned till hello följande katalog på hello virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="641ee-155">hello specified files are downloaded into hello following directory on hello target virtual machine.</span></span>
```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```
<span data-ttu-id="641ee-156">där `<n>` är ett heltal som kan ändras mellan exekveringar hello-tillägget.</span><span class="sxs-lookup"><span data-stu-id="641ee-156">where `<n>` is a decimal integer which may change between executions of hello extension.</span></span>  <span data-ttu-id="641ee-157">Hej `1.*` värdet matchar hello faktiska, aktuella `typeHandlerVersion` värdet hello-tillägget.</span><span class="sxs-lookup"><span data-stu-id="641ee-157">hello `1.*` value matches hello actual, current `typeHandlerVersion` value of hello extension.</span></span>  <span data-ttu-id="641ee-158">Hello faktiska directory kan till exempel vara `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.</span><span class="sxs-lookup"><span data-stu-id="641ee-158">For example, hello actual directory could be `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.</span></span>  

<span data-ttu-id="641ee-159">När du kör hello `commandToExecute` kommandot hello tillägget har angett den här katalogen (t.ex. `...\Downloads\2`) som hello aktuella arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="641ee-159">When executing hello `commandToExecute` command, hello extension will have set this directory (e.g., `...\Downloads\2`) as hello current working directory.</span></span> <span data-ttu-id="641ee-160">Detta aktiverar hello användning av relativa sökvägar toolocate hello filer hämtas via hello `fileURIs` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="641ee-160">This enables hello use of relative paths toolocate hello files downloaded via hello `fileURIs` property.</span></span> <span data-ttu-id="641ee-161">Finns hello tabellen nedan exempel.</span><span class="sxs-lookup"><span data-stu-id="641ee-161">See hello table below for examples.</span></span>

<span data-ttu-id="641ee-162">Eftersom hello absolut sökväg kan variera över tid, är det bättre tooopt för relativa skriptfilen sökvägar i hello `commandToExecute` sträng, när det är möjligt.</span><span class="sxs-lookup"><span data-stu-id="641ee-162">Since hello absolute download path may vary over time, it is better tooopt for relative script/file paths in hello `commandToExecute` string, whenever possible.</span></span> <span data-ttu-id="641ee-163">Exempel:</span><span class="sxs-lookup"><span data-stu-id="641ee-163">For example:</span></span>
```json
    "commandToExecute": "powershell.exe . . . -File './scripts/myscript.ps1'"
```

<span data-ttu-id="641ee-164">Sökvägsinformation när hello första URI-segment som finns kvar för filer som hämtas via hello `fileUris` egenskapslistan.</span><span class="sxs-lookup"><span data-stu-id="641ee-164">Path information after hello first URI segment is retained for files downloaded via hello `fileUris` property list.</span></span>  <span data-ttu-id="641ee-165">I hello tabellen nedan visas nedladdade filer ska mappas till hämta underkataloger tooreflect hello struktur hello `fileUris` värden.</span><span class="sxs-lookup"><span data-stu-id="641ee-165">As shown in hello table below, downloaded files are mapped into download subdirectories tooreflect hello structure of hello `fileUris` values.</span></span>  

#### <a name="examples-of-downloaded-files"></a><span data-ttu-id="641ee-166">Exempel på filer som hämtas</span><span class="sxs-lookup"><span data-stu-id="641ee-166">Examples of Downloaded Files</span></span>

| <span data-ttu-id="641ee-167">URI i fileUris</span><span class="sxs-lookup"><span data-stu-id="641ee-167">URI in fileUris</span></span> | <span data-ttu-id="641ee-168">Relativa hämtade plats</span><span class="sxs-lookup"><span data-stu-id="641ee-168">Relative downloaded location</span></span> | <span data-ttu-id="641ee-169">Absolut ned *</span><span class="sxs-lookup"><span data-stu-id="641ee-169">Absolute downloaded location *</span></span> |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

<span data-ttu-id="641ee-170">\*Som ändras ovan, hello absoluta sökvägen under hello livslängd hello VM, men inte inom en enda körning av hello CustomScript-tillägg.</span><span class="sxs-lookup"><span data-stu-id="641ee-170">\* As above, hello absolute directory paths will change over hello lifetime of hello VM, but not within a single execution of hello CustomScript extension.</span></span>

### <a name="support"></a><span data-ttu-id="641ee-171">Support</span><span class="sxs-lookup"><span data-stu-id="641ee-171">Support</span></span>

<span data-ttu-id="641ee-172">Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på hello [MSDN Azure och Stack Overflow-forum] (https://azure.microsoft.com/en-us/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="641ee-172">If you need more help at any point in this article, you can contact hello Azure experts on hello [MSDN Azure and Stack Overflow forums] (https://azure.microsoft.com/en-us/support/forums/).</span></span> <span data-ttu-id="641ee-173">Alternativt kan du lagra en incident i Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="641ee-173">Alternatively, you can file an Azure support incident.</span></span> <span data-ttu-id="641ee-174">Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/en-us/support/options/) och välja Get support.</span><span class="sxs-lookup"><span data-stu-id="641ee-174">Go toohello [Azure support site](https://azure.microsoft.com/en-us/support/options/) and select Get support.</span></span> <span data-ttu-id="641ee-175">Information om hur du använder Azure stöder läsa hello [vanliga frågor om Microsoft Azure-supporten](https://azure.microsoft.com/en-us/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="641ee-175">For information about using Azure Support, read hello [Microsoft Azure support FAQ](https://azure.microsoft.com/en-us/support/faq/).</span></span>
