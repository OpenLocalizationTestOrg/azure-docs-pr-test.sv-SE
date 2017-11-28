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
# <a name="using-virtual-machine-scale-sets-with-hello-azure-dsc-extension"></a><span data-ttu-id="5930d-103">Skaluppsättningar för den virtuella datorn med hello Azure DSC-tillägg</span><span class="sxs-lookup"><span data-stu-id="5930d-103">Using Virtual Machine Scale Sets with hello Azure DSC Extension</span></span>
<span data-ttu-id="5930d-104">[Skaluppsättningar för den virtuella datorn](virtual-machine-scale-sets-overview.md) kan användas med hello [Azure önskad tillstånd Configuration (DSC)](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tillägget hanterare.</span><span class="sxs-lookup"><span data-stu-id="5930d-104">[Virtual Machine Scale Sets](virtual-machine-scale-sets-overview.md) can be used with hello [Azure Desired State Configuration (DSC)](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) extension handler.</span></span> <span data-ttu-id="5930d-105">Skaluppsättningar för den virtuella datorn ger ett sätt toodeploy och hantera ett stort antal virtuella datorer och skala Elastiskt in och ut i svaret tooload.</span><span class="sxs-lookup"><span data-stu-id="5930d-105">Virtual machine scale sets provide a way toodeploy and manage large numbers of virtual machines, and can elastically scale in and out in response tooload.</span></span> <span data-ttu-id="5930d-106">DSC är används tooconfigure hello virtuella datorer när de kommer online så att de körs hello produktion programvara.</span><span class="sxs-lookup"><span data-stu-id="5930d-106">DSC is used tooconfigure hello VMs as they come online so they are running hello production software.</span></span>

## <a name="differences-between-deploying-toovirtual-machines-and-virtual-machine-scale-sets"></a><span data-ttu-id="5930d-107">Skillnader mellan distribuera tooVirtual datorer och virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="5930d-107">Differences between deploying tooVirtual Machines and Virtual Machine Scale Sets</span></span>
<span data-ttu-id="5930d-108">hello underliggande mallstruktur för en skaluppsättning för virtuell dator är skiljer sig från en enda virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5930d-108">hello underlying template structure for a virtual machine scale set is slightly different from a single VM.</span></span> <span data-ttu-id="5930d-109">En enda virtuell dator distribuerar specifikt tillägg under hello ”virtualMachines” noden.</span><span class="sxs-lookup"><span data-stu-id="5930d-109">Specifically, a single VM deploys extensions under hello "virtualMachines" node.</span></span> <span data-ttu-id="5930d-110">Det finns en post av typen ”tillägg” där DSC läggs toohello mall</span><span class="sxs-lookup"><span data-stu-id="5930d-110">There is an entry of type "extensions" where DSC is added toohello template</span></span>

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

<span data-ttu-id="5930d-111">En virtuell dator skala set-nod har ett ”egenskaper” avsnitt med hello ”VirtualMachineProfile”, ”extensionProfile”-attribut.</span><span class="sxs-lookup"><span data-stu-id="5930d-111">A virtual machine scale set node has a "properties" section with hello "VirtualMachineProfile", "extensionProfile" attribute.</span></span> <span data-ttu-id="5930d-112">DSC läggs till under ”tillägg”</span><span class="sxs-lookup"><span data-stu-id="5930d-112">DSC is added under "extensions"</span></span>

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

## <a name="behavior-for-a-virtual-machine-scale-set"></a><span data-ttu-id="5930d-113">Beteende för en Skaluppsättning för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="5930d-113">Behavior for a Virtual Machine Scale Set</span></span>
<span data-ttu-id="5930d-114">hello beteende för en skaluppsättning för virtuell dator är identiska toohello beteende för en enskild virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5930d-114">hello behavior for a virtual machine scale set is identical toohello behavior for a single VM.</span></span> <span data-ttu-id="5930d-115">När en ny virtuell dator skapas den automatiskt har etablerats med hello DSC-tillägg.</span><span class="sxs-lookup"><span data-stu-id="5930d-115">When a new VM is created, it is automatically provisioned with hello DSC extension.</span></span> <span data-ttu-id="5930d-116">Om en nyare version av hello WMF krävs av hello tillägget hello VM startas om innan kommer online.</span><span class="sxs-lookup"><span data-stu-id="5930d-116">If a newer version of hello WMF is required by hello extension, hello VM reboots before coming online.</span></span> <span data-ttu-id="5930d-117">När den är online, hämtar hello DSC-konfiguration .zip och etablera på hello VM.</span><span class="sxs-lookup"><span data-stu-id="5930d-117">Once it is online, it downloads hello DSC configuration .zip and provision it on hello VM.</span></span> <span data-ttu-id="5930d-118">Mer information finns i [hello översikt över Azure DSC-tillägg](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5930d-118">More details can be found in [hello Azure DSC Extension Overview](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5930d-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5930d-119">Next steps</span></span>
<span data-ttu-id="5930d-120">Granska hello [Azure Resource Manager-mall för hello DSC-tillägg](../virtual-machines/windows/extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5930d-120">Examine hello [Azure Resource Manager template for hello DSC extension](../virtual-machines/windows/extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="5930d-121">Lär dig hur hello [DSC-tillägg på ett säkert sätt hanterar autentiseringsuppgifter](../virtual-machines/windows/extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5930d-121">Learn how hello [DSC extension securely handles credentials](../virtual-machines/windows/extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="5930d-122">Mer information om hello Azure DSC-tillägg-hanteraren finns [introduktion toohello Azure Desired State Configuration-tillägget hanteraren](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5930d-122">For more information on hello Azure DSC extension handler, see [Introduction toohello Azure Desired State Configuration extension handler](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="5930d-123">Mer information om PowerShell DSC [finns hello PowerShell Dokumentationscenter](https://msdn.microsoft.com/powershell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="5930d-123">For more information about PowerShell DSC, [visit hello PowerShell documentation center](https://msdn.microsoft.com/powershell/dsc/overview).</span></span> 

