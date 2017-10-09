---
title: "aaaOMS tillägg för Azure virtuell dator för Windows | Microsoft Docs"
description: "Distribuera hello OMS-agent på Windows virtuell dator med ett tillägg för virtuell dator."
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: feae6176-2373-4034-b5d9-a32c6b4e1f10
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: nepeters
ms.openlocfilehash: 3000f66c0acdec1d1fad2125b8c6b72a92b1ec92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="oms-virtual-machine-extension-for-windows"></a>OMS tillägg för virtuell dator för Windows

Operations Management Suite (OMS) ger funktioner för övervakning, aviseringar, och reparationen i molnet och lokala resurser. hello OMS-Agent tillägg för virtuell dator för Windows publiceras och stöds av Microsoft. hello-tillägget hello OMS-agent installeras på virtuella Azure-datorer och registrerar virtuella datorer i en befintlig OMS-arbetsyta. Det här dokumentet information hello stöds plattformar, konfigurationer och distributionsalternativ för hello OMS-tillägg för virtuell dator för Windows.

## <a name="prerequisites"></a>Krav

### <a name="operating-system"></a>Operativsystem
hello OMS-Agent tillägget utgåvor för Windows kan köras mot Windows Server 2008 R2, 2012 och 2012 R2 2016.

### <a name="internet-connectivity"></a>Internetanslutning
hello OMS-Agent tillägget för Windows kräver att hello mål den virtuella datorn är ansluten toohello internet. 

## <a name="extension-schema"></a>Tilläggsschema

hello visar följande JSON hello schemat för hello tillägget OMS-Agent. hello tillägget kräver hello arbetsytan Id och arbetsytenyckel från hello mål OMS-arbetsyta, finns dessa i hello OMS-portalen. Eftersom hello arbetsytenyckel ska behandlas som känsliga data, bör det lagras i en Inställningskonfiguration för skyddade. Azure VM tillägget skyddade inställningsdata krypteras och dekrypteras endast på hello virtuella måldatorn. Observera att **workspaceId** och **workspaceKey** är skiftlägeskänsliga.

```json
{
    "type": "extensions",
    "name": "OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```
### <a name="property-values"></a>Egenskapsvärden

| Namn | Värdet / exempel |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Publisher | Microsoft.EnterpriseCloud.Monitoring |
| typ | MicrosoftMonitoringAgent |
| typeHandlerVersion | 1.0 |
| workspaceId (t.ex.) | 6f680a37-00c6-41C7-a93f-1437e3462574 |
| workspaceKey (t.ex.) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI + rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ == |

## <a name="template-deployment"></a>Malldistribution

Azure VM-tillägg kan distribueras med Azure Resource Manager-mallar. hello JSON-schema som beskrivs i föregående avsnitt i hello kan användas i en Azure Resource Manager mallen toorun hello tillägget OMS-Agent under en Azure Resource Manager för malldistribution. En exempelmall som innehåller hello OMS-agenten VM-tillägget kan hittas på hello [Azure Quick Start-galleriet](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-windows-vm). 

hello JSON för ett tillägg för virtuell dator kan kapslas i hello virtuell datorresurs, eller placeras på hello rot- eller översta nivån i en Resource Manager JSON-mall. hello placeringen av hello JSON påverkar hello värdet för hello resursnamnet och typen. Mer information finns i [ange namn och typ för underordnade resurser](../../azure-resource-manager/resource-manager-template-child-resource.md). 

hello förutsätter följande exempel hello OMS-tillägget är kapslad i hello virtuella datorresurser. När för många kapslade hello tillägget resurs, hello JSON placeras i hello `"resources": []` objekt av hello virtuell dator.


```json
{
    "type": "extensions",
    "name": "OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```

När du monterar hello tillägget JSON hello roten i hello mallen hello resursnamnet innehåller en referens toohello överordnad virtuell dator och hello typen visar hello kapslade konfiguration. 

```json
{
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "<parentVmResource>/OMSExtension",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceId"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceKey"
        }
    }
}
```

## <a name="powershell-deployment"></a>PowerShell-distribution

Hej `Set-AzureRmVMExtension` kommandot kan det vara används toodeploy hello OMS-Agent virtuella tillägget tooan befintlig virtuell dator. Innan du kör kommandot hello måste hello offentliga och privata konfigurationer toobe som lagras i en PowerShell-hash-tabell. 

```powershell
$PublicSettings = @{"workspaceId" = "myWorkspaceId"}
$ProtectedSettings = @{"workspaceKey" = "myWorkspaceKey"}

Set-AzureRmVMExtension -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
    -ExtensionType "MicrosoftMonitoringAgent" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS ` 
```

## <a name="troubleshoot-and-support"></a>Felsöka och stöd

### <a name="troubleshoot"></a>Felsöka

Data om hello tillståndet för distributioner av tillägget kan hämtas från hello Azure-portalen och genom att använda hello Azure PowerShell-modulen. toosee hello distributionsstatusen för tillägg för en viss virtuell kör hello följande kommando använder hello Azure PowerShell-modulen.

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Tillägget körning utdata är loggade toofiles hittades i hello följande katalog:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\
```

### <a name="support"></a>Support

Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på hello [MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/en-us/support/forums/). Alternativt kan du lagra en incident i Azure-supporten. Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/en-us/support/options/) och välja Get support. Information om hur du använder Azure stöder läsa hello [vanliga frågor om Microsoft Azure-supporten](https://azure.microsoft.com/en-us/support/faq/).
