---
title: "aaaAzure Network Watcher Agent tillägg för virtuell dator för Linux | Microsoft Docs"
description: "Distribuera hello Network Watcher Agent på Linux-dator som använder ett tillägg för virtuell dator."
services: virtual-machines-linux
documentationcenter: 
author: dennisg
manager: amku
editor: 
tags: azure-resource-manager
ms.assetid: 5c81e94c-e127-4dd2-ae83-a236c4512345
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: 84bed132cbda83d0917be490f9a50914578952a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="network-watcher-agent-virtual-machine-extension-for-linux"></a>Nätverk Watcher Agent tillägg för virtuell dator för Linux

## <a name="overview"></a>Översikt

[Azure Nätverksbevakaren](https://review.docs.microsoft.com/en-us/azure/network-watcher/) är en prestanda övervakning, diagnostik och analytics nätverkstjänst som tillåter övervakning för Azure-nätverk. hello Network Watcher Agent tillägg för virtuell dator är ett krav för några av hello Nätverksbevakaren funktioner på virtuella Azure-datorer. Detta omfattar att samla in nätverkstrafik på begäran och andra avancerade funktioner.

Det här dokumentet information hello stöds plattformar och distributionsalternativ för hello tillägg för virtuell dator i nätverket Watcher Agent för Linux.

## <a name="prerequisites"></a>Krav

### <a name="operating-system"></a>Operativsystem

hello Network Watcher Agent tillägget kan köras mot dessa Linux-distributioner:

| Distribution | Version |
|---|---|
| Ubuntu | 16.04 LTS, 14.04 LTS och 12.04 LTS |
| Debian | 7 och 8 |
| Redhat | 6.x och 7.x |
| Oracle Linux | 7 x |
| SUSE | 11 och 12 |
| OpenSuse | 7.0 |
| CentOS | 7.0 |

Observera att virtuell CoreOS inte stöds just nu.

### <a name="internet-connectivity"></a>Internetanslutning

Vissa av hello Network Watcher Agent funktioner kräver att den virtuella måldatorn hello anslutna toohello Internet. Utan hello möjlighet tooestablish utgående anslutningar några av funktionerna för hello Network Watcher Agent kan fungera helt eller inte tillgänglig. Mer information finns i hello [Nätverksbevakaren dokumentationen](https://review.docs.microsoft.com/en-us/azure/network-watcher/).

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
        "type": "NetworkWatcherAgentLinux",
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
| typ | NetworkWatcherAgentLinux |
| typeHandlerVersion | 1.4 |

## <a name="template-deployment"></a>Malldistribution

Azure VM-tillägg kan distribueras med Azure Resource Manager-mallar. hello JSON-schema som beskrivs i föregående avsnitt i hello kan användas i en Azure Resource Manager mallen toorun hello Network Watcher Agent tillägg under en Azure Resource Manager för malldistribution.

## <a name="azure-cli-deployment"></a>Azure CLI-distribution

hello Azure CLI kan vara används toodeploy hello nätverket Watcher Agent VM-tillägget tooan befintlig virtuell dator.

```azurecli
azure vm extension set myResourceGroup1 myVM1 NetworkWatcherAgentLinux Microsoft.Azure.NetworkWatcher 1.4
```

## <a name="troubleshooting-and-support"></a>Felsökning och support

### <a name="troubleshooting"></a>Felsökning

Data om hello tillståndet för distributioner av tillägget kan hämtas från hello Azure-portalen och genom att använda hello Azure CLI. toosee hello distributionsstatusen för tillägg för en viss virtuell dator, kör följande kommando använder hello hello Azure CLI.

```azurecli
azure vm extension get myResourceGroup1 myVM1
```

Tillägget körning utdata är loggade toofiles hittades i hello följande katalog:

`
/var/log/azure/Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentLinux/
`

### <a name="support"></a>Support

Om du behöver mer hjälp när som helst i den här artikeln kan du se toohello Nätverksbevakaren dokumentationen eller kontakta hello Azure experter på hello [MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/en-us/support/forums/). Alternativt kan du lagra en incident i Azure-supporten. Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/en-us/support/options/) och välja Get support. Information om hur du använder Azure stöder läsa hello [vanliga frågor om Microsoft Azure-supporten](https://azure.microsoft.com/en-us/support/faq/).
