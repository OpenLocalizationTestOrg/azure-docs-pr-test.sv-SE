---
title: "aaaAzure CLI skriptexempel - skapa en virtuell dator från en ögonblicksbild | Microsoft Docs"
description: "Azure CLI-skript Sample - skapa en virtuell dator från en ögonblicksbild"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: ddc95289dcb8a0ca7c7854d969983f96b8f4613f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-cli"></a>Skapa en virtuell dator från en ögonblicksbild med CLI

Det här skriptet skapar en virtuell dator från en ögonblicksbild av en OS-disk.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.sh "Create VM from snapshot")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon toocreate hanterade diskar, virtuell dator, och alla relaterade resurser. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [AZ ögonblicksbild visa](https://docs.microsoft.com/cli/azure/snapshot#show) | Hämtar ögonblicksbild med namnet på ögonblicksbilder och resursgruppens namn. ID-egenskapen för hello returnerade objekt är används toocreate hanterade diskar.  |
| [Skapa AZ disk](https://docs.microsoft.com/cli/azure/disk#create) | Skapar hanterade diskar från en ögonblicksbild med hjälp av ögonblicksbilder Id, namn på disk, lagringstyp och storlek  |
| [Skapa AZ vm](https://docs.microsoft.com/cli/azure/vm#create) | Skapar en virtuell dator med hjälp av en hanterad OS-disk |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare virtuella CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
