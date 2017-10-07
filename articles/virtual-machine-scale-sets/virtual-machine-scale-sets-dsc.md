---
title: "aaaUsing önskade tillstånd med Virtual Machine Scale konfigurationsuppsättningar | Microsoft Docs"
description: "Skaluppsättningar för den virtuella datorn med hello Azure DSC-tillägg"
services: virtual-machine-scale-sets
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: c8f047b5-0e6c-4ef3-8a47-f1b284d32942
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 04/05/2017
ms.author: zachal
ms.openlocfilehash: a35f1ca6700aa4889978032aa512882db50d6573
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-virtual-machine-scale-sets-with-hello-azure-dsc-extension"></a>Skaluppsättningar för den virtuella datorn med hello Azure DSC-tillägg
[Skaluppsättningar för den virtuella datorn](virtual-machine-scale-sets-overview.md) kan användas med hello [Azure önskad tillstånd Configuration (DSC)](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tillägget hanterare. Skaluppsättningar för den virtuella datorn ger ett sätt toodeploy och hantera ett stort antal virtuella datorer och skala Elastiskt in och ut i svaret tooload. DSC är används tooconfigure hello virtuella datorer när de kommer online så att de körs hello produktion programvara.

## <a name="differences-between-deploying-toovirtual-machines-and-virtual-machine-scale-sets"></a>Skillnader mellan distribuera tooVirtual datorer och virtuella datorer
hello underliggande mallstruktur för en skaluppsättning för virtuell dator är skiljer sig från en enda virtuell dator. En enda virtuell dator distribuerar specifikt tillägg under hello ”virtualMachines” noden. Det finns en post av typen ”tillägg” där DSC läggs toohello mall

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "DscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

En virtuell dator skala set-nod har ett ”egenskaper” avsnitt med hello ”VirtualMachineProfile”, ”extensionProfile”-attribut. DSC läggs till under ”tillägg”

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="behavior-for-a-virtual-machine-scale-set"></a>Beteende för en Skaluppsättning för virtuell dator
hello beteende för en skaluppsättning för virtuell dator är identiska toohello beteende för en enskild virtuell dator. När en ny virtuell dator skapas den automatiskt har etablerats med hello DSC-tillägg. Om en nyare version av hello WMF krävs av hello tillägget hello VM startas om innan kommer online. När den är online, hämtar hello DSC-konfiguration .zip och etablera på hello VM. Mer information finns i [hello översikt över Azure DSC-tillägg](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-steps"></a>Nästa steg
Granska hello [Azure Resource Manager-mall för hello DSC-tillägg](../virtual-machines/windows/extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Lär dig hur hello [DSC-tillägg på ett säkert sätt hanterar autentiseringsuppgifter](../virtual-machines/windows/extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Mer information om hello Azure DSC-tillägg-hanteraren finns [introduktion toohello Azure Desired State Configuration-tillägget hanteraren](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Mer information om PowerShell DSC [finns hello PowerShell Dokumentationscenter](https://msdn.microsoft.com/powershell/dsc/overview). 

