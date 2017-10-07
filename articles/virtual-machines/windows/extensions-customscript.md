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
# <a name="custom-script-extension-for-windows"></a>Tillägget för anpassat skript för Windows

hello tillägget för anpassat skript hämtar och kör skript på virtuella Azure-datorer. Det här tillägget är användbart för post distributionskonfiguration, Programvaruinstallation eller någon annan konfiguration / hanteringsaktivitet. Skript kan hämtas från Azure storage eller GitHub eller tillhandahålls toohello Azure-portalen på tillägget körtiden. hello tillägget för anpassat skript kan integreras med Azure Resource Manager-mallar och kan också köras med hello Azure CLI, PowerShell, Azure-portalen eller hello Azure virtuella REST API.

Det här dokumentet beskriver hur toouse hello tillägget för anpassat skript med hello Azure PowerShell-modulen, Azure Resource Manager-mallar och information om felsökning i Windows-System.

## <a name="prerequisites"></a>Krav

### <a name="operating-system"></a>Operativsystem

hello tillägget för anpassat skript för Windows kan köras mot Windows Server 2008 R2, 2012 och 2012 R2 2016 utgåvor.

### <a name="script-location"></a>Skriptets placering

hello skript måste lagras i Azure Blob storage, eller någon annan plats som är tillgängligt via en giltig URL toobe.

### <a name="internet-connectivity"></a>Internet-anslutning

hello anpassade skript tillägget för Windows kräver att hello mål den virtuella datorn är ansluten toohello internet. 

## <a name="extension-schema"></a>Tilläggsschema

hello visar följande JSON hello schemat för hello tillägget för anpassat skript. hello tillägget kräver en skript-plats (Azure Storage eller någon annan plats med en giltig URL) och en kommando-tooexecute. Om du använder Azure Storage som hello skriptkälla krävs ett Azure storage-konto namn och åtkomstnyckel. De här objekten ska behandlas som känsliga data och anges i hello tillägg skyddade inställningen konfigurationen. Azure VM tillägget skyddade inställningsdata krypteras och dekrypteras endast på hello virtuella måldatorn.

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

### <a name="property-values"></a>Egenskapsvärden

| Namn | Värdet / exempel |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Publisher | Microsoft.Compute |
| typ | Tillägg |
| typeHandlerVersion | 1.9 |
| fileUris (t.ex.) | https://Raw.githubusercontent.com/Microsoft/DotNet-Core-Sample-Templates/Master/DotNet-Core-Music-Windows/Scripts/Configure-Music-App.ps1 |
| commandToExecute (t.ex.) | PowerShell - ExecutionPolicy obegränsad - filen konfigurera-musik-app.ps1 |
| storageAccountName (t.ex.) | examplestorageacct |
| storageAccountKey (t.ex.) | TmJK/1N3AbAZ3q / + hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg == |

**Obs** -dessa egenskapsnamn är skiftlägeskänsliga. Använd hello namn som visas ovanför tooavoid distributionsproblem.

## <a name="template-deployment"></a>Malldistribution

Azure VM-tillägg kan distribueras med Azure Resource Manager-mallar. hello JSON-schema som beskrivs i föregående avsnitt i hello kan användas i en Azure Resource Manager mallen toorun hello tillägget för anpassat skript under en Azure Resource Manager för malldistribution. En exempelmall som innehåller hello tillägget för anpassat skript hittar du här, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>PowerShell-distribution

Hej `Set-AzureRmVMCustomScriptExtension` kommandot kan det vara används tooadd hello anpassat skript tillägget tooan befintlig virtuell dator. Mer information finns i [Set AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).
```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location myLocation `
    -FileUri myURL `
    -Run 'myScript.ps1' `
    -Name DemoScriptExtension
```

## <a name="troubleshoot-and-support"></a>Felsöka och stöd

### <a name="troubleshoot"></a>Felsöka

Data om hello tillståndet för distributioner av tillägget kan hämtas från hello Azure-portalen och genom att använda hello Azure PowerShell-modulen. toosee hello distributionstillstånd för en viss virtuell dator, kör följande kommando hello-tillägg.

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Tillägget körning utdata är loggade toofiles hittades under hello följande katalog på hello virtuella måldatorn.
```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

hello angetts filerna laddas ned till hello följande katalog på hello virtuella måldatorn.
```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```
där `<n>` är ett heltal som kan ändras mellan exekveringar hello-tillägget.  Hej `1.*` värdet matchar hello faktiska, aktuella `typeHandlerVersion` värdet hello-tillägget.  Hello faktiska directory kan till exempel vara `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.  

När du kör hello `commandToExecute` kommandot hello tillägget har angett den här katalogen (t.ex. `...\Downloads\2`) som hello aktuella arbetskatalogen. Detta aktiverar hello användning av relativa sökvägar toolocate hello filer hämtas via hello `fileURIs` egenskapen. Finns hello tabellen nedan exempel.

Eftersom hello absolut sökväg kan variera över tid, är det bättre tooopt för relativa skriptfilen sökvägar i hello `commandToExecute` sträng, när det är möjligt. Exempel:
```json
    "commandToExecute": "powershell.exe . . . -File './scripts/myscript.ps1'"
```

Sökvägsinformation när hello första URI-segment som finns kvar för filer som hämtas via hello `fileUris` egenskapslistan.  I hello tabellen nedan visas nedladdade filer ska mappas till hämta underkataloger tooreflect hello struktur hello `fileUris` värden.  

#### <a name="examples-of-downloaded-files"></a>Exempel på filer som hämtas

| URI i fileUris | Relativa hämtade plats | Absolut ned * |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

\*Som ändras ovan, hello absoluta sökvägen under hello livslängd hello VM, men inte inom en enda körning av hello CustomScript-tillägg.

### <a name="support"></a>Support

Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på hello [MSDN Azure och Stack Overflow-forum] (https://azure.microsoft.com/en-us/support/forums/). Alternativt kan du lagra en incident i Azure-supporten. Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/en-us/support/options/) och välja Get support. Information om hur du använder Azure stöder läsa hello [vanliga frågor om Microsoft Azure-supporten](https://azure.microsoft.com/en-us/support/faq/).
