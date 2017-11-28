---
title: "Anpassade skript körs på virtuella Linux-datorer i Azure | Microsoft Docs"
description: "Automatisera åtgärder för Linux VM-konfigurationen genom att använda tillägget för anpassat skript"
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
ms.openlocfilehash: 1dde64aac72c11ccfccf4fdb676279692befaadd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="using-the-azure-custom-script-extension-with-linux-virtual-machines"></a><span data-ttu-id="72967-103">Med hjälp av tillägget för anpassat skript för Azure med Linux virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="72967-103">Using the Azure Custom Script Extension with Linux Virtual Machines</span></span>
<span data-ttu-id="72967-104">Tillägget för anpassat skript hämtar och kör skript på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="72967-104">The Custom Script Extension downloads and executes scripts on Azure virtual machines.</span></span> <span data-ttu-id="72967-105">Det här tillägget är användbart för post distributionskonfiguration, Programvaruinstallation eller någon annan konfiguration / hanteringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="72967-105">This extension is useful for post deployment configuration, software installation, or any other configuration / management task.</span></span> <span data-ttu-id="72967-106">Skript kan hämtas från Azure-lagring eller andra tillgängliga Internetplats eller som tillägget körtiden.</span><span class="sxs-lookup"><span data-stu-id="72967-106">Scripts can be downloaded from Azure storage or other accessible internet location, or provided to the extension run time.</span></span> <span data-ttu-id="72967-107">Tillägget för anpassat skript kan integreras med Azure Resource Manager-mallar och kan också köras med hjälp av Azure CLI, PowerShell, Azure-portalen eller Azure Virtual Machine REST API.</span><span class="sxs-lookup"><span data-stu-id="72967-107">The Custom Script extension integrates with Azure Resource Manager templates, and can also be run using the Azure CLI, PowerShell, Azure portal, or the Azure Virtual Machine REST API.</span></span>

<span data-ttu-id="72967-108">Det här dokumentet beskrivs hur du använder tillägget för anpassat skript från Azure CLI och en Azure Resource Manager-mall och även detaljer felsökningssteg för Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="72967-108">This document details how to use the Custom Script Extension from the Azure CLI, and an Azure Resource Manager template, and also details troubleshooting steps on Linux systems.</span></span>

## <a name="extension-configuration"></a><span data-ttu-id="72967-109">Tilläggets konfiguration</span><span class="sxs-lookup"><span data-stu-id="72967-109">Extension Configuration</span></span>
<span data-ttu-id="72967-110">Tillägget för anpassat skript konfigurationen anger sådant som skriptets placering och kommandot ska köras.</span><span class="sxs-lookup"><span data-stu-id="72967-110">The Custom Script Extension configuration specifies things like script location and the command to be run.</span></span> <span data-ttu-id="72967-111">Den här konfigurationen kan lagras i konfigurationsfiler, anges på kommandoraden eller i en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="72967-111">This configuration can be stored in configuration files, specified on the command line, or in an Azure Resource Manager template.</span></span> <span data-ttu-id="72967-112">Känsliga data kan lagras i en skyddad konfiguration, som krypteras och dekrypteras endast inuti den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="72967-112">Sensitive data can be stored in a protected configuration, which is encrypted and only decrypted inside the virtual machine.</span></span> <span data-ttu-id="72967-113">Skyddade konfigurationen är användbar när kommandot körningen innehåller hemligheter, till exempel ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="72967-113">The protected configuration is useful when the execution command includes secrets such as a password.</span></span>

### <a name="public-configuration"></a><span data-ttu-id="72967-114">Offentliga konfiguration</span><span class="sxs-lookup"><span data-stu-id="72967-114">Public Configuration</span></span>
<span data-ttu-id="72967-115">Schema:</span><span class="sxs-lookup"><span data-stu-id="72967-115">Schema:</span></span>

<span data-ttu-id="72967-116">**Obs** -dessa egenskapsnamn är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="72967-116">**Note** - these property names are case sensitive.</span></span> <span data-ttu-id="72967-117">Använda namnen som visas nedan för att undvika distributionsproblem.</span><span class="sxs-lookup"><span data-stu-id="72967-117">Use the names as seen below to avoid deployment issues.</span></span>

* <span data-ttu-id="72967-118">**commandToExecute**: (krävs, string) post punkt att köra skript</span><span class="sxs-lookup"><span data-stu-id="72967-118">**commandToExecute**: (required, string) the entry point script to execute</span></span>
* <span data-ttu-id="72967-119">**fileUris**: (valfritt, Strängmatrisen) URL: er för filer som ska hämtas.</span><span class="sxs-lookup"><span data-stu-id="72967-119">**fileUris**: (optional, string array) the URLs for files to be downloaded.</span></span>
* <span data-ttu-id="72967-120">**tidsstämpel** (valfritt, heltal) använda det här fältet bara för att utlösa en kör av skriptet genom att ändra värdet för det här fältet.</span><span class="sxs-lookup"><span data-stu-id="72967-120">**timestamp** (optional, integer) use this field only to trigger a rerun of the script by changing value of this field.</span></span>

```json
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a><span data-ttu-id="72967-121">Skyddade konfiguration</span><span class="sxs-lookup"><span data-stu-id="72967-121">Protected Configuration</span></span>
<span data-ttu-id="72967-122">Schema:</span><span class="sxs-lookup"><span data-stu-id="72967-122">Schema:</span></span>

<span data-ttu-id="72967-123">**Obs** -dessa egenskapsnamn är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="72967-123">**Note** - these property names are case sensitive.</span></span> <span data-ttu-id="72967-124">Använda namnen som visas nedan för att undvika distributionsproblem.</span><span class="sxs-lookup"><span data-stu-id="72967-124">Use the names as seen below to avoid deployment issues.</span></span>

* <span data-ttu-id="72967-125">**commandToExecute**: (valfritt, string) att posten punkt skriptet körs.</span><span class="sxs-lookup"><span data-stu-id="72967-125">**commandToExecute**: (optional, string) the entry point script to execute.</span></span> <span data-ttu-id="72967-126">Använd det här fältet i stället om kommandot innehåller hemligheter, till exempel lösenord.</span><span class="sxs-lookup"><span data-stu-id="72967-126">Use this field instead if your command contains secrets such as passwords.</span></span>
* <span data-ttu-id="72967-127">**storageAccountName**: (valfritt, string) namnet på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="72967-127">**storageAccountName**: (optional, string) the name of storage account.</span></span> <span data-ttu-id="72967-128">Om du anger autentiseringsuppgifter för lagring, måste alla fileUris vara URL: er för Azure-BLOB.</span><span class="sxs-lookup"><span data-stu-id="72967-128">If you specify storage credentials, all fileUris must be URLs for Azure Blobs.</span></span>
* <span data-ttu-id="72967-129">**storageAccountKey**: (valfritt, string) åtkomstnyckeln för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="72967-129">**storageAccountKey**: (optional, string) the access key of storage account.</span></span>

```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a><span data-ttu-id="72967-130">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="72967-130">Azure CLI</span></span>
<span data-ttu-id="72967-131">När du använder Azure CLI för att köra tillägget för anpassat skript, skapa en konfigurationsfil eller filer som innehåller åtminstone uri för filen och köra skriptkommandot.</span><span class="sxs-lookup"><span data-stu-id="72967-131">When using the Azure CLI to run the Custom Script Extension, create a configuration file or files containing at minimum the file uri, and the script execution command.</span></span>

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

<span data-ttu-id="72967-132">Inställningarna kan du kan också anges i kommandot som en JSON-formaterad sträng.</span><span class="sxs-lookup"><span data-stu-id="72967-132">Optionally the settings can be specified in the command as a JSON formatted string.</span></span> <span data-ttu-id="72967-133">På så sätt kan konfigurationen anges under körning och utan en separat fil.</span><span class="sxs-lookup"><span data-stu-id="72967-133">This allows the configuration to be specified during execution and without a separate configuration file.</span></span>

```azurecli
az vm extension set '
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://raw.githubusercontent.com/neilpeterson/test-extension/master/test.sh"],"commandToExecute": "./test.sh"}'
```

### <a name="azure-cli-examples"></a><span data-ttu-id="72967-134">Azure CLI-exempel</span><span class="sxs-lookup"><span data-stu-id="72967-134">Azure CLI Examples</span></span>

<span data-ttu-id="72967-135">**Exempel 1** -offentliga konfiguration med skriptfilen.</span><span class="sxs-lookup"><span data-stu-id="72967-135">**Example 1** - Public configuration with script file.</span></span>

```json
{
  "fileUris": ["https://raw.githubusercontent.com/neilpeterson/test-extension/master/test.sh"],
  "commandToExecute": "./test.sh"
}
```

<span data-ttu-id="72967-136">Azure CLI-kommando:</span><span class="sxs-lookup"><span data-stu-id="72967-136">Azure CLI command:</span></span>

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

<span data-ttu-id="72967-137">**Exempel 2** -offentliga konfiguration med ingen skriptfilen.</span><span class="sxs-lookup"><span data-stu-id="72967-137">**Example 2** - Public configuration with no script file.</span></span>

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

<span data-ttu-id="72967-138">Azure CLI-kommando:</span><span class="sxs-lookup"><span data-stu-id="72967-138">Azure CLI command:</span></span>

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

<span data-ttu-id="72967-139">**Exempel 3** – en offentlig konfigurationsfil används för att ange skriptfil URI och en skyddad fil används för att ange kommandot som ska köras.</span><span class="sxs-lookup"><span data-stu-id="72967-139">**Example 3** - A public configuration file is used to specify the script file URI, and a protected configuration file is used to specify the command to be executed.</span></span>

<span data-ttu-id="72967-140">Offentliga konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="72967-140">Public configuration file:</span></span>

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"]
}
```

<span data-ttu-id="72967-141">Skyddade konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="72967-141">Protected configuration file:</span></span>  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

<span data-ttu-id="72967-142">Azure CLI-kommando:</span><span class="sxs-lookup"><span data-stu-id="72967-142">Azure CLI command:</span></span>

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json --protected-settings ./protected-config.json
```

## <a name="resource-manager-template"></a><span data-ttu-id="72967-143">Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="72967-143">Resource Manager Template</span></span>
<span data-ttu-id="72967-144">Tillägget för Azure anpassat skript kan köras vid tidpunkten för distribution för virtuell dator med en Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="72967-144">The Azure Custom Script Extension can be run at Virtual Machine deployment time using a Resource Manager template.</span></span> <span data-ttu-id="72967-145">Gör du genom att lägga till korrekt formaterad JSON i mallen för distribution.</span><span class="sxs-lookup"><span data-stu-id="72967-145">To do so, add properly formatted JSON to the deployment template.</span></span>

### <a name="resource-manager-examples"></a><span data-ttu-id="72967-146">Resource Manager-exempel</span><span class="sxs-lookup"><span data-stu-id="72967-146">Resource Manager Examples</span></span>
<span data-ttu-id="72967-147">**Exempel 1** -offentliga konfiguration.</span><span class="sxs-lookup"><span data-stu-id="72967-147">**Example 1** - public configuration.</span></span>

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

<span data-ttu-id="72967-148">**Exempel 2** -körning av kommandot i skyddade konfiguration.</span><span class="sxs-lookup"><span data-stu-id="72967-148">**Example 2** - execution command in protected configuration.</span></span>

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

<span data-ttu-id="72967-149">Se .net Core musik Store demonstration för en komplett exempel - [musik Store Demo](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).</span><span class="sxs-lookup"><span data-stu-id="72967-149">See the .Net Core Music Store Demo for a complete example - [Music Store Demo](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="72967-150">Felsökning</span><span class="sxs-lookup"><span data-stu-id="72967-150">Troubleshooting</span></span>
<span data-ttu-id="72967-151">När tillägget för anpassat skript körs skriptet skapas eller hämtas i en katalog som liknar följande exempel.</span><span class="sxs-lookup"><span data-stu-id="72967-151">When the Custom Script Extension runs, the script is created or downloaded into a directory similar to the following example.</span></span> <span data-ttu-id="72967-152">Kommandoutdata sparas i den här katalogen i `stdout` och `stderr` fil.</span><span class="sxs-lookup"><span data-stu-id="72967-152">The command output is also saved into this directory in `stdout` and `stderr` file.</span></span>

```bash
/var/lib/waagent/custom-script/download/0/
```

<span data-ttu-id="72967-153">Tillägget för Azure-skript skapar en logg som finns här.</span><span class="sxs-lookup"><span data-stu-id="72967-153">The Azure Script Extension produces a log, which can be found here.</span></span>

```bash
/var/log/azure/custom-script/handler.log
```

<span data-ttu-id="72967-154">Körningstillståndet för tillägget för anpassat skript kan också hämtas med Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="72967-154">The execution state of the Custom Script Extension can also be retrieved with the Azure CLI.</span></span>

```azurecli
az vm extension list -g myResourceGroup --vm-name myVM
```

<span data-ttu-id="72967-155">Det ser ut som följande utdata:</span><span class="sxs-lookup"><span data-stu-id="72967-155">The output looks like the following text:</span></span>

```azurecli
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a><span data-ttu-id="72967-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="72967-156">Next Steps</span></span>
<span data-ttu-id="72967-157">Information om andra skript VM-tillägg finns [översikt över Azure skript tillägget för Linux](extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="72967-157">For information on other VM Script Extensions, see [Azure Script Extension overview for Linux](extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

