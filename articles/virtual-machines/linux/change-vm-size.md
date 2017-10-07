---
title: aaaHow tooresize en Linux VM med hello Azure CLI 2.0 | Microsoft Docs
description: "Hur tooscale upp eller skala ned en virtuell Linux-dator, genom att ändra hello VM-storlek."
services: virtual-machines-linux
documentationcenter: na
author: mikewasson
manager: timlt
editor: 
tags: 
ms.assetid: e163f878-b919-45c5-9f5a-75a64f3b14a0
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2017
ms.author: mwasson
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e8fba485b5bcc7824f546de5cf3df77624a28008
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="resize-a-linux-virtual-machine-using-cli-20"></a>Ändra storlek på en Linux-dator med hjälp av CLI 2.0

När du etablera en virtuell dator (VM) du kan skala hello VM uppåt eller nedåt genom att ändra hello [VM-storlek][vm-sizes]. I vissa fall måste du frigöra hello VM först. Du måste toodeallocate hello VM eventuellt hello storleken inte är tillgänglig på hello maskinvara kluster som är värd för hello VM. Den här artikeln beskrivs hur tooresize en Linux VM med hello Azure CLI 2.0. Du kan också utföra dessa steg med hello [Azure CLI 1.0](change-vm-size-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="resize-a-vm"></a>Ändra storlek på en virtuell dator
tooresize en virtuell dator måste hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).

1. Visa hello lista med tillgängliga Virtuella storlekar på hello maskinvara kluster där hello VM finns med [az vm-vm-storlek-alternativ för](/cli/azure/vm#list-vm-resize-options). hello följande exempel visar en lista över VM-storlekar för hello virtuella datorn med namnet `myVM` i hello resursgruppen `myResourceGroup` region:
   
    ```azurecli
    az vm list-vm-resize-options --resource-group myResourceGroup --name myVM --output table
    ```

2. Ändra storlek på eventuellt hello VM-storlek visas hello virtuell dator med [az vm ändra storlek på](/cli/azure/vm#resize). hello följande exempel ändrar storleken hello virtuella datorn med namnet `myVM` toohello `Standard_DS3_v2` storlek:
   
    ```azurecli
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    ```
   
    hello VM startas om under den här processen. Efter omstart hello mappas befintlig OS- och datadiskar om. Allt på hello diskutrymme går förlorad.

3. Eventuellt hello VM-storlek inte visas toofirst måste frigöra hello virtuell dator med [az vm frigöra](/cli/azure/vm#deallocate). Den här processen kan hello VM toothen att ändrade tooany storlek som hello stöder region och sedan startas. hello följande steg frigöra, ändra storlek på och sedan starta hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:
   
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    az vm start --resource-group myResourceGroup --name myVM
    ```
   
   > [!WARNING]
   > Det har frigjorts hello VM Frigör också alla dynamiska IP-adresser som tilldelats toohello VM. hello OS- och datadiskar påverkas inte.

## <a name="next-steps"></a>Nästa steg
För ytterligare utbyggbarhet köra flera VM-instanser och skala ut. Mer information finns i [skala automatiskt Linux-datorer i en virtuell dator Skaluppsättning][scale-set]. 

<!-- links -->
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
