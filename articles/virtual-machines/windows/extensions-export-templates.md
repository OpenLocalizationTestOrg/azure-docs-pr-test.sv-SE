---
title: "aaaExporting Azure-resursgrupper som innehåller VM-tillägg | Microsoft Docs"
description: "Exportera Resource Manager-mallar som inkluderar tillägg för virtuell dator."
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7f4e2ca6-f1c7-4f59-a2cc-8f63132de279
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/05/2016
ms.author: nepeters
ms.openlocfilehash: cdbc2030988a19fe68429e8733dc60536c264abf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="exporting-resource-groups-that-contain-vm-extensions"></a>Exportera resursgrupper som innehåller VM-tillägg

Azure resursgrupper kan exporteras till en ny Resource Manager-mall som sedan kan distribueras på nytt. hello exportprocessen tolkar befintliga resurser och skapar en Resource Manager-mall som när den distribuerats resulterar i en liknande resursgrupp. När du använder hello resursgruppen exportalternativ mot en resursgrupp som innehåller tillägg för virtuell dator, flera objekt behöver toobe betraktas som tillägget kompatibilitet och skyddade inställningar.

Det här dokumentet beskriver hur hello resursgruppen exportprocessen fungerar om tillägg för virtuell dator, inklusive en lista över tillägg som stöds och information om hantering av skyddade data.

## <a name="supported-virtual-machine-extensions"></a>Tillägg för virtuella datorer

Det finns många tillägg för virtuell dator. Inte alla tillägg kan exporteras till en Resource Manager-mall med hjälp av funktionen för hello ”Automatiseringsskriptet”. Om ett tillägg för virtuell dator inte stöds, måste det toobe placeras manuellt i hello exporterad mall.

hello kan följande tillägg exporteras med hello automation skript-funktionen.

| Anknytning ||||
|---|---|---|---|
| Acronis säkerhetskopiering | Datadog Windows-agenten | OS-korrigering för Linux | VM-ögonblicksbild Linux
| Acronis säkerhetskopiering Linux | Docker-tillägg | Puppet-Agent |
| BG Info | DSC-tillägg | Plats 24 x 7 Apm Insight |
| BMC CTM Agent Linux | Dynatrace Linux | Linux platsservern 24 x 7 |
| BMC CTM Agent Windows | Dynatrace Windows | Windows platsservern 24 x 7 |
| Chef klienten | HPE säkerhet programmet Defender | Trend Micro DSA |
| Anpassat skript | IaaS program mot skadlig kod | Trend Micro DSA Linux |
| Anpassat skripttillägg | IaaS-diagnostik | VM-åtkomst för Linux |
| Anpassat skript för Linux | Klienten för Linux-Chef | VM-åtkomst för Linux |
| Datadog Linux-Agent | Linux-diagnostik | VM-ögonblicksbild |

## <a name="export-hello-resource-group"></a>Exportera hello resursgruppen.

tooexport en resursgrupp till en återanvändbara mall klar hello följande steg:

1. Logga in toohello Azure-portalen
2. Hello Hubbmenyn, klicka på resursgrupper
3. Välj hello målresursgruppen hello listan
4. I hello resursgrupp-bladet, klickar du på Automatiseringsskriptet

![Exportera mall](./media/extensions-export-templates/template-export.png)

hello Azure Resource Manager automatiseringar skriptet skapar en Resource Manager-mall, en fil med parametrar och flera exempel på distribution av skript, till exempel PowerShell och Azure CLI. Nu hello exporterad mall kan hämtas med hjälp av hello hämtningsknappen läggas till som en ny mall toohello mallbibliotek eller omdistribueras med hello knappen distribuera.

## <a name="configure-protected-settings"></a>Konfigurera inställningar för skyddade

Många tillägg för virtuella Azure-datorn innehåller en skyddad Inställningskonfiguration som krypterar känsliga data, till exempel autentiseringsuppgifter och konfigurationssträngar. Skyddade inställningarna exporteras inte med hello automation-skript. Om nödvändigt, skyddade inställningarna måste toobe gränsvärdet i hello exporteras utan mall.

### <a name="step-1---remove-template-parameter"></a>Steg 1 – ta bort mallparameter

När hello resursgruppen exporteras, en enda mallparameter skapas tooprovide exporteras en värdet toohello skyddade inställningar. Den här parametern kan tas bort. tooremove hello parametern titta igenom hello parameterlista och ta bort hello-parameter som ser ut ungefär toothis JSON-exemplet.

```json
"extensions_extensionname_protectedSettings": {
    "defaultValue": null,
    "type": "SecureObject"
}
```

### <a name="step-2---get-protected-settings-properties"></a>Steg 2 – Get skyddade egenskaper

En lista över de här egenskaperna måste toobe samlas in eftersom varje skyddad inställning har en uppsättning egenskaper som krävs. Varje parameter för hello skyddade inställningarna konfiguration finns i hello [Azure Resource Manager schemat på GitHub](https://raw.githubusercontent.com/Azure/azure-resource-manager-schemas/master/schemas/2015-08-01/Microsoft.Compute.json). Det här schemat innehåller endast hello parameteruppsättningar för hello-tillägg som nämns i hello översikt över i det här dokumentet. 

Från inom hello schema-lagringsplatsen, söka efter hello önskad tillägget i det här exemplet `IaaSDiagnostics`. En gång hello tillägg `protectedSettings` objekt har hittats, anteckna varje parameter. I exemplet hello av hello `IaasDiagnostic` tillägg, hello kräver parametrar är `storageAccountName`, `storageAccountKey`, och `storageAccountEndPoint`.

```json
"protectedSettings": {
    "type": "object",
    "properties": {
        "storageAccountName": {
            "type": "string"
        },
        "storageAccountKey": {
            "type": "string"
        },
        "storageAccountEndPoint": {
            "type": "string"
        }
    },
    "required": [
        "storageAccountName",
        "storageAccountKey",
        "storageAccountEndPoint"
    ]
}
```

### <a name="step-3---re-create-hello-protected-configuration"></a>Steg 3 – återskapa hello skyddade konfiguration

Hej exporterad mall, söka efter på `protectedSettings` och Ersätt hello exporterade skyddade inställningsobjekt med en ny som innehåller hello krävs Tilläggsparametrarna och ett värde för varje kriterium.

I exemplet hello av hello `IaasDiagnostic` tillägg, hello nya skyddade inställningen konfigurationen ser ut hello följande exempel:

```json
"protectedSettings": {
    "storageAccountName": "[parameters('storageAccountName')]",
    "storageAccountKey": "[parameters('storageAccountKey')]",
    "storageAccountEndPoint": "https://core.windows.net"
}
```

hello Final = extension resource ser ut ungefär toohello följande JSON-exempel:

```json
{
    "name": "Microsoft.Insights.VMDiagnosticsSettings",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "[variables('apiVersion')]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "tags": {
        "displayName": "AzureDiagnostics"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Diagnostics",
        "type": "IaaSDiagnostics",
        "typeHandlerVersion": "1.5",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
            "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
        },
        "protectedSettings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "storageAccountKey": "[parameters('storageAccountKey')]",
            "storageAccountEndPoint": "https://core.windows.net"
        }
    }
}
```

Om du använder mallen parametrar tooprovide egenskapsvärden, måste dessa toobe skapas. När du skapar mallparametrar för skyddade inställningsvärden måste du toouse hello `SecureString` parametertypen så att känslig värden är skyddade. Mer information om hur du använder parametrar finns [redigera Azure Resource Manager-mallar](../../resource-group-authoring-templates.md).

I exemplet hello av hello `IaasDiagnostic` tillägg, hello följande parametrar skulle skapas under hello parametrar i hello Resource Manager-mall.

```json
"storageAccountName": {
    "defaultValue": null,
    "type": "SecureString"
},
"storageAccountKey": {
    "defaultValue": null,
    "type": "SecureString"
}
```

Nu kan hello mallen distribueras med valfri metod för distribution av mallen.
