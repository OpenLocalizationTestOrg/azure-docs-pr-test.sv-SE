---
title: aaaRedeploy Linux virtuella datorer i Azure | Microsoft Docs
description: "Hur tooredeploy Linux-datorer i Azure toomitigate SSH-anslutningen utfärdar."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
tags: azure-resource-manager,top-support-issue
ms.assetid: e9530dd6-f5b0-4160-b36b-d75151d99eb7
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/23/2017
ms.author: iainfou
ms.openlocfilehash: 9adfd1b11f262d362133366b2bba5e69c70c9b82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="redeploy-linux-virtual-machine-toonew-azure-node"></a>Distribuera Linux virtuella toonew Azure nod
Om du står inför svårigheter felsökning SSH eller program använder tooa Linux virtuella dator (VM) i Azure kan kan omdistribuera hello VM hjälpa. När du distribuerar en virtuell dator flyttas hello VM tooa ny nod i hello Azure-infrastrukturen och sedan aktiveras den tillbaka. Alla konfigurationsalternativ och associerade resurser bevaras. Den här artikeln beskrivs hur du tooredeploy en virtuell dator med hjälp av Azure CLI eller hello Azure-portalen.

> [!NOTE]
> När du distribuerar en virtuell dator hello diskutrymme går förlorad och dynamiska IP-adresser som är kopplade till virtuella nätverksgränssnittet har uppdaterats. 

Du kan distribuera en virtuell dator med någon av följande alternativ för hello. Du behöver bara toochoose en alternativet tooredeploy den virtuella datorn:

- [Azure CLI 2.0](#azure-cli-20)
- [Azure CLI 1.0](#azure-cli-10)
- [Azure Portal](#using-azure-portal)

## <a name="use-hello-azure-cli-20"></a>Använd hello Azure CLI 2.0
Installera hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).

Distribuera den virtuella datorn med [az vm Omdistributionen](/cli/azure/vm#redeploy). följande exempel återdistribuerar hello hello virtuella datorn med namnet *myVM* i hello resursgrupp med namnet *myResourceGroup*:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM 
```

## <a name="use-hello-azure-cli-10"></a>Använd hello Azure CLI 1.0
Installera hello [senaste Azure CLI 1.0](../../cli-install-nodejs.md), logga in tooan Azure-konto och se till att du är i läget Resource Manager (`azure config mode arm`).

följande exempel återdistribuerar hello hello virtuella datorn med namnet *myVM* i hello resursgrupp med namnet *myResourceGroup*:

```azurecli
azure vm redeploy --resource-group myResourceGroup --vm-name myVM 
```

[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Nästa steg
Om du har problem med anslutning tooyour VM du hittar detaljerad hjälp på [felsökning av SSH-anslutningar](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [detaljerad SSH felsökningssteg](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Om du inte kommer åt ett program som körs på den virtuella datorn, du kan också läsa [felsökning av problem med programmet](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

