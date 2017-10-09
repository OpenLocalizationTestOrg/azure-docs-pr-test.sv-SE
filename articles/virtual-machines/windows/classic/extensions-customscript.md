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
# <a name="custom-script-extension-for-windows-using-hello-classic-deployment-model"></a>Anpassade skript tillägget för Windows med hello klassiska distributionsmodellen

> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Lär dig hur för[utföra dessa steg med hello Resource Manager-modellen](../extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

hello tillägget för anpassat skript hämtar och kör skript på virtuella Azure-datorer. Det här tillägget är användbart för post distributionskonfiguration, Programvaruinstallation eller någon annan konfiguration / hanteringsaktivitet. Skript kan hämtas från Azure storage eller GitHub eller tillhandahålls toohello Azure-portalen på tillägget körtiden. hello tillägget för anpassat skript kan integreras med Azure Resource Manager-mallar och kan också köras med hello Azure CLI, PowerShell, Azure-portalen eller hello Azure virtuella REST API.

Det här dokumentet beskriver hur toouse hello tillägget för anpassat skript med hello Azure PowerShell-modulen, Azure Resource Manager-mallar och information om felsökning i Windows-System.

## <a name="prerequisites"></a>Krav

### <a name="operating-system"></a>Operativsystem

hello tillägget för anpassat skript för Windows kan köras mot Windows Server 2008 R2, 2012 och 2012 R2 2016 utgåvor.

### <a name="script-location"></a>Skriptets placering

hello skript måste lagras i Azure storage eller någon annan plats som är tillgängligt via en giltig URL toobe.

### <a name="internet-connectivity"></a>Internet-anslutning

hello anpassade skript tillägget för Windows kräver att hello mål den virtuella datorn är ansluten toohello internet. 

## <a name="extension-schema"></a>Tilläggsschema

hello visar följande JSON hello schemat för hello tillägget för anpassat skript. hello tillägget kräver en skript-plats (Azure Storage eller någon annan plats med en giltig URL) och en kommando-tooexecute. Om du använder Azure Storage som hello skriptkälla krävs ett Azure storage-konto namn och åtkomstnyckel. De här objekten ska behandlas som känsliga data och anges i hello tillägg skyddade inställningen konfigurationen. Azure VM tillägget skyddade inställningsdata krypteras och dekrypteras endast på hello virtuella måldatorn.

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

### <a name="property-values"></a>Egenskapsvärden

| Namn | Värdet / exempel |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Publisher | Microsoft.Compute |
| Tillägg | CustomScriptExtension |
| typeHandlerVersion | 1.8 |
| fileUris (t.ex.) | https://Raw.githubusercontent.com/Microsoft/DotNet-Core-Sample-Templates/Master/DotNet-Core-Music-Windows/Scripts/Configure-Music-App.ps1 |
| commandToExecute (t.ex.) | PowerShell - ExecutionPolicy obegränsad - filen konfigurera-musik-app.ps1 |

## <a name="template-deployment"></a>Malldistribution

Azure VM-tillägg kan distribueras med Azure Resource Manager-mallar. hello JSON-schema som beskrivs i föregående avsnitt i hello kan användas i en Azure Resource Manager mallen toorun hello tillägget för anpassat skript under en Azure Resource Manager för malldistribution. En exempelmall som innehåller hello tillägget för anpassat skript hittar du här, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>PowerShell-distribution

Hej `Set-AzureVMCustomScriptExtension` kommandot kan det vara används tooadd hello anpassat skript tillägget tooan befintlig virtuell dator. Mer information finns i [Set AzureRmVMCustomScriptExtension ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).

```powershell
# create vm object
$vm = Get-AzureVM -Name 2016clas -ServiceName 2016clas1313

# set extension
Set-AzureVMCustomScriptExtension -VM $vm -FileUri myFileUri -Run 'create-file.ps1'

# update vm
$vm | Update-AzureVM
```

## <a name="troubleshoot-and-support"></a>Felsöka och stöd

### <a name="troubleshoot"></a>Felsöka

Data om hello tillståndet för distributioner av tillägget kan hämtas från hello Azure-portalen och genom att använda hello Azure PowerShell-modulen. toosee hello distributionstillstånd för en viss virtuell dator, kör följande kommando hello-tillägg.

```powershell
Get-AzureVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Tillägget körning utdata oss loggade toofiles hittades i hello följande katalog på hello virtuella måldatorn.

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

själva hello skriptet hämtas till hello följande katalog på hello virtuella måldatorn.

```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads
```

### <a name="support"></a>Support

Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på hello [MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/en-us/support/forums/). Alternativt kan du lagra en incident i Azure-supporten. Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/en-us/support/options/) och välja Get support. Information om hur du använder Azure stöder läsa hello [vanliga frågor om Microsoft Azure-supporten](https://azure.microsoft.com/en-us/support/faq/).
