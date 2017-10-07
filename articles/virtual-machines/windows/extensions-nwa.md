---
title: "aaaAzure Network Watcher Agent tillägg för virtuell dator för Windows | Microsoft Docs"
description: "Distribuera hello Network Watcher Agent på Windows virtuell dator med ett tillägg för virtuell dator."
services: virtual-machines-windows
documentationcenter: 
author: dennisg
manager: amku
editor: 
tags: azure-resource-manager
ms.assetid: 27e46af7-2150-45e8-b084-ba33de8c5e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: 21298706e462ff32c4d314f9a1ad127074ddf481
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="network-watcher-agent-virtual-machine-extension-for-windows"></a>Nätverk Watcher Agent tillägg för virtuell dator för Windows

## <a name="overview"></a>Översikt

[Azure Nätverksbevakaren](https://review.docs.microsoft.com/en-us/azure/network-watcher/) är en prestanda övervakning, diagnostik och analytics nätverkstjänst som tillåter övervakning för Azure-nätverk. hello Network Watcher Agent tillägg för virtuell dator är ett krav för några av hello Nätverksbevakaren funktioner på virtuella Azure-datorer. Detta omfattar att samla in nätverkstrafik på begäran och andra avancerade funktioner.

Det här dokumentet information hello stöds plattformar och distributionsalternativ för hello Network Watcher Agent tillägg för virtuell dator för Windows.

## <a name="prerequisites"></a>Krav

### <a name="operating-system"></a>Operativsystem

hello Network Watcher Agent tillägget utgåvor för Windows kan köras mot Windows Server 2008 R2, 2012 och 2012 R2 2016. Observera att hello Nano Server stöds inte just nu.

### <a name="internet-connectivity"></a>Internetanslutning

Vissa av hello Network Watcher Agent funktioner kräver att den virtuella måldatorn hello anslutna toohello Internet. Utan hello möjlighet tooestablish utgående anslutningar några av funktionerna för hello Network Watcher Agent kan fungera helt eller inte tillgänglig. Mer information finns i hello [Nätverksbevakaren dokumentationen](../../network-watcher/network-watcher-monitoring-overview.md).

## <a name="extension-schema"></a>Tilläggsschema

hello visar följande JSON hello schemat för hello Network Watcher Agent tillägg. hello tillägget varken kräver inte heller stöder alla inställningar som anges av användaren just nu och förlitar sig på en standardkonfiguration.

```json
{
    "type": "extensions",
    "name": "Microsoft.Azure.NetworkWatcher",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.NetworkWatcher",
        "type": "NetworkWatcherAgentWindows",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a>Egenskapsvärden

| Namn | Värdet / exempel |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Publisher | Microsoft.Azure.NetworkWatcher |
| typ | NetworkWatcherAgentWindows |
| typeHandlerVersion | 1.4 |


## <a name="template-deployment"></a>Malldistribution

Azure VM-tillägg kan distribueras med Azure Resource Manager-mallar. hello JSON-schema som beskrivs i föregående avsnitt i hello kan användas i en Azure Resource Manager mallen toorun hello Network Watcher Agent tillägg under en Azure Resource Manager för malldistribution.

## <a name="powershell-deployment"></a>PowerShell-distribution

Hej `Set-AzureRmVMExtension` kommandot kan det vara används toodeploy hello Network Watcher Agent virtuella tillägget tooan befintlig virtuell dator.

```powershell
Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup1" `
                       -Location "WestUS" `
                       -VMName "myVM1" `
                       -Name "networkWatcherAgent" `
                       -Publisher "Microsoft.Azure.NetworkWatcher" `
                       -Type "NetworkWatcherAgentWindows" `
                       -TypeHandlerVersion "1.4"
```

## <a name="troubleshooting-and-support"></a>Felsökning och support

### <a name="troubleshooting"></a>Felsökning

Data om hello tillståndet för distributioner av tillägget kan hämtas från hello Azure-portalen och genom att använda hello Azure PowerShell-modulen. toosee hello distributionsstatusen för tillägg för en viss virtuell kör hello följande kommando använder hello Azure PowerShell-modulen.

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup1 -VMName myVM1 -Name networkWatcherAgent
```

Tillägget körning utdata är loggade toofiles hittades i hello följande katalog:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentWindows\
```

### <a name="support"></a>Support

Om du behöver mer hjälp när som helst i den här artikeln kan du referera toohello användarhandboken för nätverket Watcher dokumentationen eller kontakta hello Azure experter på hello [MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/en-us/support/forums/). Alternativt kan du lagra en incident i Azure-supporten. Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/en-us/support/options/) och välja Get support. Information om hur du använder Azure stöder läsa hello [vanliga frågor om Microsoft Azure-supporten](https://azure.microsoft.com/en-us/support/faq/).
