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
# <a name="using-hello-azure-custom-script-extension-with-linux-virtual-machines"></a>Med hjälp av hello Azure-tillägget för anpassat skript med Linux-datorer
hello tillägget för anpassat skript hämtar och kör skript på virtuella Azure-datorer. Det här tillägget är användbart för post distributionskonfiguration, Programvaruinstallation eller någon annan konfiguration / hanteringsaktivitet. Skript kan hämtas från Azure-lagring eller andra tillgängliga Internetplats eller tillhandahålls toohello tillägget körtiden. hello tillägget för anpassat skript kan integreras med Azure Resource Manager-mallar och kan också köras med hello Azure CLI, PowerShell, Azure-portalen eller hello Azure virtuella REST API.

Det här dokumentet beskriver hur toouse hello tillägget för anpassat skript från hello Azure CLI och en Azure Resource Manager-mall och även information om felsökning i Linux-system.

## <a name="extension-configuration"></a>Tilläggets konfiguration
hello tillägget för anpassat skript konfigurationen anger sådant som skriptets placering och hello kommandot toobe kör. Den här konfigurationen kan lagras i konfigurationsfiler, anges på kommandoraden för hello, eller i en Azure Resource Manager-mall. Känsliga data kan lagras i en skyddad konfiguration, som krypteras och dekrypteras endast inuti hello virtuella datorn. hello skyddade konfigurationen är användbar när hello körning av kommandot innehåller hemligheter, till exempel ett lösenord.

### <a name="public-configuration"></a>Offentliga konfiguration
Schema:

**Obs** -dessa egenskapsnamn är skiftlägeskänsliga. Använd hello namn som du ser nedan tooavoid distributionsproblem.

* **commandToExecute**: (krävs, string) hello post punkt skriptet tooexecute
* **fileUris**: (valfritt, Strängmatrisen) hello URL: er för filer toobe hämtas.
* **tidsstämpel** (valfritt, heltal) använder det här fältet endast tootrigger en kör hello skriptet genom att ändra värdet för det här fältet.

```json
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a>Skyddade konfiguration
Schema:

**Obs** -dessa egenskapsnamn är skiftlägeskänsliga. Använd hello namn som du ser nedan tooavoid distributionsproblem.

* **commandToExecute**: (valfritt, string) hello post punkt skriptet tooexecute. Använd det här fältet i stället om kommandot innehåller hemligheter, till exempel lösenord.
* **storageAccountName**: (valfritt, string) hello namnet på lagringskontot. Om du anger autentiseringsuppgifter för lagring, måste alla fileUris vara URL: er för Azure-BLOB.
* **storageAccountKey**: (valfritt, string) hello åtkomstnyckeln för lagringskontot.

```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a>Azure CLI
När du använder hello Azure CLI toorun hello tillägget för anpassat skript, skapa en konfigurationsfil eller filer som innehåller på minsta hello filen uri och hello skriptkommandot för körning.

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

Du kan också anges hello inställningar i hello kommando som en JSON-formaterad sträng. Detta gör att hello configuration toobe anges under körning och utan en separat fil.

```azurecli
az vm extension set '
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://raw.githubusercontent.com/neilpeterson/test-extension/master/test.sh"],"commandToExecute": "./test.sh"}'
```

### <a name="azure-cli-examples"></a>Azure CLI-exempel

**Exempel 1** -offentliga konfiguration med skriptfilen.

```json
{
  "fileUris": ["https://raw.githubusercontent.com/neilpeterson/test-extension/master/test.sh"],
  "commandToExecute": "./test.sh"
}
```

Azure CLI-kommando:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

**Exempel 2** -offentliga konfiguration med ingen skriptfilen.

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Azure CLI-kommando:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

**Exempel 3** – en offentlig konfigurationsfil är används toospecify hello skriptfilen URI och en skyddad fil är används toospecify hello kommandot toobe utförs.

Offentliga konfigurationsfil:

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"]
}
```

Skyddade konfigurationsfil:  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

Azure CLI-kommando:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json --protected-settings ./protected-config.json
```

## <a name="resource-manager-template"></a>Resource Manager-mall
hello Azure-tillägget för anpassat skript kan köras vid tidpunkten för distribution för virtuell dator med en Resource Manager-mall. toodo Lägg därför till korrekt formaterad JSON toohello Distributionsmall.

### <a name="resource-manager-examples"></a>Resource Manager-exempel
**Exempel 1** -offentliga konfiguration.

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

**Exempel 2** -körning av kommandot i skyddade konfiguration.

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

Se hello .net Core musik Store demonstration för en komplett exempel - [musik Store Demo](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).

## <a name="troubleshooting"></a>Felsökning
När hello tillägget för anpassat skript körs, skapas hello skript eller hämtas till en katalog och liknande toohello som följande exempel. hello kommandoutdata sparas i den här katalogen i `stdout` och `stderr` fil.

```bash
/var/lib/waagent/custom-script/download/0/
```

hello Azure skripttillägg genererar en logg som finns här.

```bash
/var/log/azure/custom-script/handler.log
```

Hej Körningstillståndet för hello tillägget för anpassat skript kan också hämtas med hello Azure CLI.

```azurecli
az vm extension list -g myResourceGroup --vm-name myVM
```

hello utdata ser ut så hello följande text:

```azurecli
info:    Executing command vm extension get
+ Looking up hello VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Nästa steg
Information om andra skript VM-tillägg finns [översikt över Azure skript tillägget för Linux](extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

