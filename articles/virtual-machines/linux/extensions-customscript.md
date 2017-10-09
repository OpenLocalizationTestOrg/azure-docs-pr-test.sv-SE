---
title: "aaaRun anpassade skript på Linux virtuella datorer i Azure | Microsoft Docs"
description: "Automatisera åtgärder för Linux VM-konfigurationen med hjälp av hello tillägget för anpassat skript"
services: virtual-machines-linux
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cf17ab2b-8d7e-4078-b6df-955c6d5071c2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: nepeters
ms.openlocfilehash: f2c273a5fbd4cd1695aea48fa4bd08e691511e5f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-custom-script-extension-with-linux-virtual-machines"></a><span data-ttu-id="77af5-103">Med hjälp av hello Azure-tillägget för anpassat skript med Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="77af5-103">Using hello Azure Custom Script Extension with Linux Virtual Machines</span></span>
<span data-ttu-id="77af5-104">hello tillägget för anpassat skript hämtar och kör skript på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="77af5-104">hello Custom Script Extension downloads and executes scripts on Azure virtual machines.</span></span> <span data-ttu-id="77af5-105">Det här tillägget är användbart för post distributionskonfiguration, Programvaruinstallation eller någon annan konfiguration / hanteringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="77af5-105">This extension is useful for post deployment configuration, software installation, or any other configuration / management task.</span></span> <span data-ttu-id="77af5-106">Skript kan hämtas från Azure-lagring eller andra tillgängliga Internetplats eller tillhandahålls toohello tillägget körtiden.</span><span class="sxs-lookup"><span data-stu-id="77af5-106">Scripts can be downloaded from Azure storage or other accessible internet location, or provided toohello extension run time.</span></span> <span data-ttu-id="77af5-107">hello tillägget för anpassat skript kan integreras med Azure Resource Manager-mallar och kan också köras med hello Azure CLI, PowerShell, Azure-portalen eller hello Azure virtuella REST API.</span><span class="sxs-lookup"><span data-stu-id="77af5-107">hello Custom Script extension integrates with Azure Resource Manager templates, and can also be run using hello Azure CLI, PowerShell, Azure portal, or hello Azure Virtual Machine REST API.</span></span>

<span data-ttu-id="77af5-108">Det här dokumentet beskriver hur toouse hello tillägget för anpassat skript från hello Azure CLI och en Azure Resource Manager-mall och även information om felsökning i Linux-system.</span><span class="sxs-lookup"><span data-stu-id="77af5-108">This document details how toouse hello Custom Script Extension from hello Azure CLI, and an Azure Resource Manager template, and also details troubleshooting steps on Linux systems.</span></span>

## <a name="extension-configuration"></a><span data-ttu-id="77af5-109">Tilläggets konfiguration</span><span class="sxs-lookup"><span data-stu-id="77af5-109">Extension Configuration</span></span>
<span data-ttu-id="77af5-110">hello tillägget för anpassat skript konfigurationen anger sådant som skriptets placering och hello kommandot toobe kör.</span><span class="sxs-lookup"><span data-stu-id="77af5-110">hello Custom Script Extension configuration specifies things like script location and hello command toobe run.</span></span> <span data-ttu-id="77af5-111">Den här konfigurationen kan lagras i konfigurationsfiler, anges på kommandoraden för hello, eller i en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="77af5-111">This configuration can be stored in configuration files, specified on hello command line, or in an Azure Resource Manager template.</span></span> <span data-ttu-id="77af5-112">Känsliga data kan lagras i en skyddad konfiguration, som krypteras och dekrypteras endast inuti hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="77af5-112">Sensitive data can be stored in a protected configuration, which is encrypted and only decrypted inside hello virtual machine.</span></span> <span data-ttu-id="77af5-113">hello skyddade konfigurationen är användbar när hello körning av kommandot innehåller hemligheter, till exempel ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="77af5-113">hello protected configuration is useful when hello execution command includes secrets such as a password.</span></span>

### <a name="public-configuration"></a><span data-ttu-id="77af5-114">Offentliga konfiguration</span><span class="sxs-lookup"><span data-stu-id="77af5-114">Public Configuration</span></span>
<span data-ttu-id="77af5-115">Schema:</span><span class="sxs-lookup"><span data-stu-id="77af5-115">Schema:</span></span>

<span data-ttu-id="77af5-116">**Obs** -dessa egenskapsnamn är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="77af5-116">**Note** - these property names are case sensitive.</span></span> <span data-ttu-id="77af5-117">Använd hello namn som du ser nedan tooavoid distributionsproblem.</span><span class="sxs-lookup"><span data-stu-id="77af5-117">Use hello names as seen below tooavoid deployment issues.</span></span>

* <span data-ttu-id="77af5-118">**commandToExecute**: (krävs, string) hello post punkt skriptet tooexecute</span><span class="sxs-lookup"><span data-stu-id="77af5-118">**commandToExecute**: (required, string) hello entry point script tooexecute</span></span>
* <span data-ttu-id="77af5-119">**fileUris**: (valfritt, Strängmatrisen) hello URL: er för filer toobe hämtas.</span><span class="sxs-lookup"><span data-stu-id="77af5-119">**fileUris**: (optional, string array) hello URLs for files toobe downloaded.</span></span>
* <span data-ttu-id="77af5-120">**tidsstämpel** (valfritt, heltal) använder det här fältet endast tootrigger en kör hello skriptet genom att ändra värdet för det här fältet.</span><span class="sxs-lookup"><span data-stu-id="77af5-120">**timestamp** (optional, integer) use this field only tootrigger a rerun of hello script by changing value of this field.</span></span>

```json
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a><span data-ttu-id="77af5-121">Skyddade konfiguration</span><span class="sxs-lookup"><span data-stu-id="77af5-121">Protected Configuration</span></span>
<span data-ttu-id="77af5-122">Schema:</span><span class="sxs-lookup"><span data-stu-id="77af5-122">Schema:</span></span>

<span data-ttu-id="77af5-123">**Obs** -dessa egenskapsnamn är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="77af5-123">**Note** - these property names are case sensitive.</span></span> <span data-ttu-id="77af5-124">Använd hello namn som du ser nedan tooavoid distributionsproblem.</span><span class="sxs-lookup"><span data-stu-id="77af5-124">Use hello names as seen below tooavoid deployment issues.</span></span>

* <span data-ttu-id="77af5-125">**commandToExecute**: (valfritt, string) hello post punkt skriptet tooexecute.</span><span class="sxs-lookup"><span data-stu-id="77af5-125">**commandToExecute**: (optional, string) hello entry point script tooexecute.</span></span> <span data-ttu-id="77af5-126">Använd det här fältet i stället om kommandot innehåller hemligheter, till exempel lösenord.</span><span class="sxs-lookup"><span data-stu-id="77af5-126">Use this field instead if your command contains secrets such as passwords.</span></span>
* <span data-ttu-id="77af5-127">**storageAccountName**: (valfritt, string) hello namnet på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="77af5-127">**storageAccountName**: (optional, string) hello name of storage account.</span></span> <span data-ttu-id="77af5-128">Om du anger autentiseringsuppgifter för lagring, måste alla fileUris vara URL: er för Azure-BLOB.</span><span class="sxs-lookup"><span data-stu-id="77af5-128">If you specify storage credentials, all fileUris must be URLs for Azure Blobs.</span></span>
* <span data-ttu-id="77af5-129">**storageAccountKey**: (valfritt, string) hello åtkomstnyckeln för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="77af5-129">**storageAccountKey**: (optional, string) hello access key of storage account.</span></span>

```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a><span data-ttu-id="77af5-130">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="77af5-130">Azure CLI</span></span>
<span data-ttu-id="77af5-131">När du använder hello Azure CLI toorun hello tillägget för anpassat skript, skapa en konfigurationsfil eller filer som innehåller på minsta hello filen uri och hello skriptkommandot för körning.</span><span class="sxs-lookup"><span data-stu-id="77af5-131">When using hello Azure CLI toorun hello Custom Script Extension, create a configuration file or files containing at minimum hello file uri, and hello script execution command.</span></span>

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

<span data-ttu-id="77af5-132">Du kan också anges hello inställningar i hello kommando som en JSON-formaterad sträng.</span><span class="sxs-lookup"><span data-stu-id="77af5-132">Optionally hello settings can be specified in hello command as a JSON formatted string.</span></span> <span data-ttu-id="77af5-133">Detta gör att hello configuration toobe anges under körning och utan en separat fil.</span><span class="sxs-lookup"><span data-stu-id="77af5-133">This allows hello configuration toobe specified during execution and without a separate configuration file.</span></span>

```azurecli
az vm extension set '
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://raw.githubusercontent.com/neilpeterson/test-extension/master/test.sh"],"commandToExecute": "./test.sh"}'
```

### <a name="azure-cli-examples"></a><span data-ttu-id="77af5-134">Azure CLI-exempel</span><span class="sxs-lookup"><span data-stu-id="77af5-134">Azure CLI Examples</span></span>

<span data-ttu-id="77af5-135">**Exempel 1** -offentliga konfiguration med skriptfilen.</span><span class="sxs-lookup"><span data-stu-id="77af5-135">**Example 1** - Public configuration with script file.</span></span>

```json
{
  "fileUris": ["https://raw.githubusercontent.com/neilpeterson/test-extension/master/test.sh"],
  "commandToExecute": "./test.sh"
}
```

<span data-ttu-id="77af5-136">Azure CLI-kommando:</span><span class="sxs-lookup"><span data-stu-id="77af5-136">Azure CLI command:</span></span>

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

<span data-ttu-id="77af5-137">**Exempel 2** -offentliga konfiguration med ingen skriptfilen.</span><span class="sxs-lookup"><span data-stu-id="77af5-137">**Example 2** - Public configuration with no script file.</span></span>

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

<span data-ttu-id="77af5-138">Azure CLI-kommando:</span><span class="sxs-lookup"><span data-stu-id="77af5-138">Azure CLI command:</span></span>

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

<span data-ttu-id="77af5-139">**Exempel 3** – en offentlig konfigurationsfil är används toospecify hello skriptfilen URI och en skyddad fil är används toospecify hello kommandot toobe utförs.</span><span class="sxs-lookup"><span data-stu-id="77af5-139">**Example 3** - A public configuration file is used toospecify hello script file URI, and a protected configuration file is used toospecify hello command toobe executed.</span></span>

<span data-ttu-id="77af5-140">Offentliga konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="77af5-140">Public configuration file:</span></span>

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"]
}
```

<span data-ttu-id="77af5-141">Skyddade konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="77af5-141">Protected configuration file:</span></span>  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

<span data-ttu-id="77af5-142">Azure CLI-kommando:</span><span class="sxs-lookup"><span data-stu-id="77af5-142">Azure CLI command:</span></span>

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json --protected-settings ./protected-config.json
```

## <a name="resource-manager-template"></a><span data-ttu-id="77af5-143">Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="77af5-143">Resource Manager Template</span></span>
<span data-ttu-id="77af5-144">hello Azure-tillägget för anpassat skript kan köras vid tidpunkten för distribution för virtuell dator med en Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="77af5-144">hello Azure Custom Script Extension can be run at Virtual Machine deployment time using a Resource Manager template.</span></span> <span data-ttu-id="77af5-145">toodo Lägg därför till korrekt formaterad JSON toohello Distributionsmall.</span><span class="sxs-lookup"><span data-stu-id="77af5-145">toodo so, add properly formatted JSON toohello deployment template.</span></span>

### <a name="resource-manager-examples"></a><span data-ttu-id="77af5-146">Resource Manager-exempel</span><span class="sxs-lookup"><span data-stu-id="77af5-146">Resource Manager Examples</span></span>
<span data-ttu-id="77af5-147">**Exempel 1** -offentliga konfiguration.</span><span class="sxs-lookup"><span data-stu-id="77af5-147">**Example 1** - public configuration.</span></span>

```json
{
    "name": "scriptextensiondemo",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-15",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('scriptextensiondemoName'))]"
    ],
    "tags": {
        "displayName": "scriptextensiondemo"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
      "settings": {
        "fileUris": [
          "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
        ],
        "commandToExecute": "sh hello.sh"
      }
    }
}
```

<span data-ttu-id="77af5-148">**Exempel 2** -körning av kommandot i skyddade konfiguration.</span><span class="sxs-lookup"><span data-stu-id="77af5-148">**Example 2** - execution command in protected configuration.</span></span>

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
      ]              
    },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <password>"
    }
  }
}
```

<span data-ttu-id="77af5-149">Se hello .net Core musik Store demonstration för en komplett exempel - [musik Store Demo](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).</span><span class="sxs-lookup"><span data-stu-id="77af5-149">See hello .Net Core Music Store Demo for a complete example - [Music Store Demo](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="77af5-150">Felsökning</span><span class="sxs-lookup"><span data-stu-id="77af5-150">Troubleshooting</span></span>
<span data-ttu-id="77af5-151">När hello tillägget för anpassat skript körs, skapas hello skript eller hämtas till en katalog och liknande toohello som följande exempel.</span><span class="sxs-lookup"><span data-stu-id="77af5-151">When hello Custom Script Extension runs, hello script is created or downloaded into a directory similar toohello following example.</span></span> <span data-ttu-id="77af5-152">hello kommandoutdata sparas i den här katalogen i `stdout` och `stderr` fil.</span><span class="sxs-lookup"><span data-stu-id="77af5-152">hello command output is also saved into this directory in `stdout` and `stderr` file.</span></span>

```bash
/var/lib/waagent/custom-script/download/0/
```

<span data-ttu-id="77af5-153">hello Azure skripttillägg genererar en logg som finns här.</span><span class="sxs-lookup"><span data-stu-id="77af5-153">hello Azure Script Extension produces a log, which can be found here.</span></span>

```bash
/var/log/azure/custom-script/handler.log
```

<span data-ttu-id="77af5-154">Hej Körningstillståndet för hello tillägget för anpassat skript kan också hämtas med hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="77af5-154">hello execution state of hello Custom Script Extension can also be retrieved with hello Azure CLI.</span></span>

```azurecli
az vm extension list -g myResourceGroup --vm-name myVM
```

<span data-ttu-id="77af5-155">hello utdata ser ut så hello följande text:</span><span class="sxs-lookup"><span data-stu-id="77af5-155">hello output looks like hello following text:</span></span>

```azurecli
info:    Executing command vm extension get
+ Looking up hello VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a><span data-ttu-id="77af5-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="77af5-156">Next Steps</span></span>
<span data-ttu-id="77af5-157">Information om andra skript VM-tillägg finns [översikt över Azure skript tillägget för Linux](extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="77af5-157">For information on other VM Script Extensions, see [Azure Script Extension overview for Linux](extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

