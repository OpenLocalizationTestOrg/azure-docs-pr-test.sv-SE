---
title: "Med hjälp av Desired State Configuration med Skalningsuppsättningar i virtuella | Microsoft Docs"
description: "Med virtuella skalan uppsättningar med Azure DSC-tillägg"
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
ms.openlocfilehash: b61b0acf3072569ab733a13defb465c921d26187
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a><span data-ttu-id="0f086-103">Med virtuella skalan uppsättningar med Azure DSC-tillägg</span><span class="sxs-lookup"><span data-stu-id="0f086-103">Using Virtual Machine Scale Sets with the Azure DSC Extension</span></span>
<span data-ttu-id="0f086-104">[Skaluppsättningar för den virtuella datorn](virtual-machine-scale-sets-overview.md) kan användas med den [Azure önskad tillstånd Configuration (DSC)](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tillägget hanterare.</span><span class="sxs-lookup"><span data-stu-id="0f086-104">[Virtual Machine Scale Sets](virtual-machine-scale-sets-overview.md) can be used with the [Azure Desired State Configuration (DSC)](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) extension handler.</span></span> <span data-ttu-id="0f086-105">Skaluppsättningar för den virtuella datorn är ett sätt att distribuera och hantera ett stort antal virtuella datorer och Elastiskt kan skala in eller ut för att läsa in.</span><span class="sxs-lookup"><span data-stu-id="0f086-105">Virtual machine scale sets provide a way to deploy and manage large numbers of virtual machines, and can elastically scale in and out in response to load.</span></span> <span data-ttu-id="0f086-106">DSC används för att konfigurera de virtuella datorerna som de är online så att de kör programmet för produktion.</span><span class="sxs-lookup"><span data-stu-id="0f086-106">DSC is used to configure the VMs as they come online so they are running the production software.</span></span>

## <a name="differences-between-deploying-to-virtual-machines-and-virtual-machine-scale-sets"></a><span data-ttu-id="0f086-107">Skillnader mellan distribution till virtuella datorer och virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="0f086-107">Differences between deploying to Virtual Machines and Virtual Machine Scale Sets</span></span>
<span data-ttu-id="0f086-108">Den underliggande strukturen i mallen för en skaluppsättning för virtuell dator är skiljer sig från en enda virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0f086-108">The underlying template structure for a virtual machine scale set is slightly different from a single VM.</span></span> <span data-ttu-id="0f086-109">En enda virtuell dator distribuerar specifikt tillägg under noden ”virtualMachines”.</span><span class="sxs-lookup"><span data-stu-id="0f086-109">Specifically, a single VM deploys extensions under the "virtualMachines" node.</span></span> <span data-ttu-id="0f086-110">Det finns en post av typen ”tillägg” där DSC har lagts till i mallen</span><span class="sxs-lookup"><span data-stu-id="0f086-110">There is an entry of type "extensions" where DSC is added to the template</span></span>

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

<span data-ttu-id="0f086-111">En virtuell dator skala set-nod har ett ”egenskaper” avsnitt med den ”VirtualMachineProfile”, ”extensionProfile”-attribut.</span><span class="sxs-lookup"><span data-stu-id="0f086-111">A virtual machine scale set node has a "properties" section with the "VirtualMachineProfile", "extensionProfile" attribute.</span></span> <span data-ttu-id="0f086-112">DSC läggs till under ”tillägg”</span><span class="sxs-lookup"><span data-stu-id="0f086-112">DSC is added under "extensions"</span></span>

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

## <a name="behavior-for-a-virtual-machine-scale-set"></a><span data-ttu-id="0f086-113">Beteende för en Skaluppsättning för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="0f086-113">Behavior for a Virtual Machine Scale Set</span></span>
<span data-ttu-id="0f086-114">Beteende för en skaluppsättning för virtuell dator är identiskt med beteendet för en enskild virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0f086-114">The behavior for a virtual machine scale set is identical to the behavior for a single VM.</span></span> <span data-ttu-id="0f086-115">När en ny virtuell dator skapas den automatiskt har etablerats med DSC-tillägg.</span><span class="sxs-lookup"><span data-stu-id="0f086-115">When a new VM is created, it is automatically provisioned with the DSC extension.</span></span> <span data-ttu-id="0f086-116">Om en nyare version av WMF krävs av tillägget för den virtuella datorn startas om innan kommer online.</span><span class="sxs-lookup"><span data-stu-id="0f086-116">If a newer version of the WMF is required by the extension, the VM reboots before coming online.</span></span> <span data-ttu-id="0f086-117">När den är online, hämtar .zip för DSC-konfigurationen och etablera på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0f086-117">Once it is online, it downloads the DSC configuration .zip and provision it on the VM.</span></span> <span data-ttu-id="0f086-118">Mer information finns i [Azure DSC-tillägg översikt](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0f086-118">More details can be found in [the Azure DSC Extension Overview](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0f086-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0f086-119">Next steps</span></span>
<span data-ttu-id="0f086-120">Granska de [Azure Resource Manager-mall för DSC-tillägg](../virtual-machines/windows/extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0f086-120">Examine the [Azure Resource Manager template for the DSC extension](../virtual-machines/windows/extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="0f086-121">Lär dig hur [DSC-tillägg på ett säkert sätt hanterar autentiseringsuppgifter](../virtual-machines/windows/extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0f086-121">Learn how the [DSC extension securely handles credentials](../virtual-machines/windows/extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="0f086-122">Mer information om Azure DSC-tillägg-hanteraren finns [introduktion till Azure Desired State Configuration-tillägget hanteraren](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0f086-122">For more information on the Azure DSC extension handler, see [Introduction to the Azure Desired State Configuration extension handler](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="0f086-123">Mer information om PowerShell DSC [finns på PowerShell documentation center](https://msdn.microsoft.com/powershell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="0f086-123">For more information about PowerShell DSC, [visit the PowerShell documentation center](https://msdn.microsoft.com/powershell/dsc/overview).</span></span> 

