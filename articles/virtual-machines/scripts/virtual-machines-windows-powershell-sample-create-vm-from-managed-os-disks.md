---
title: aaaAzure PowerShell skriptexempel - skapa en virtuell dator genom att koppla en hanterade diskar som OS-disk | Microsoft Docs
description: Azure PowerShell-skript Sample - skapa en virtuell dator genom att koppla en hanterade diskar som OS-disk
services: virtual-machines-windows
documentationcenter: virtual-machines
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 8ae5b14df3977a4af91b92692cb925199cfb8058
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-powershell"></a>Skapa en virtuell dator med en befintlig OS-disk hanterade med PowerShell

Det här skriptet skapar en virtuell dator genom att koppla en befintlig hanterade disk som OS-disken. Använd det här skriptet i föregående scenarier:
* Skapa en virtuell dator från en befintlig hanterade OS-disk som har kopierats från en hanterad disk i en annan prenumeration
* Skapa en virtuell dator från en befintlig hanterade disk som har skapats från en särskild VHD-fil 
* Skapa en virtuell dator från en befintlig hanterade OS-disk som har skapats från en ögonblicksbild 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from snapshot")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon tooget hanteras diskegenskaper, bifoga en hanterade diskar tooa ny virtuell dator och skapa en virtuell dator. Varje objekt i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Get-AzureRmDisk](/powershell/module/azurerm.compute/Get-AzureRmDisk) | Hämtar disk objekt baserat på hello namn och hello resursgruppen för en disk. ID-egenskapen för hello returnerade diskobjektet är används tooattach hello disk tooa ny virtuell dator |
| [Ny AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Skapar en VM-konfiguration. Den här konfigurationen omfattar information som VM-namn, operativsystem och administrativa autentiseringsuppgifter. hello konfiguration används under skapande av virtuell dator. |
| [Ange AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) | Bifogar en hanterade diskar med hello Id-egenskapen för hello disk som Operativsystemet disk tooa ny virtuell dator |
| [Ny AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Skapar en offentlig IP-adress. |
| [Ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Skapar ett nätverksgränssnitt. |
| [Ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Skapa en virtuell dator. |
|[Ta bort AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Tar bort en resursgrupp och alla resurser som ingår i. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).

Ytterligare virtuella PowerShell-skript-exempel finns i hello [Virtuella för Windows Azure-dokumentationen](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
