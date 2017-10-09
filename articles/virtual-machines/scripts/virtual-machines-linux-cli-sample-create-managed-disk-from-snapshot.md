---
title: "aaaAzure CLI skriptexempel - skapa en hanterad disk från en ögonblicksbild | Microsoft Docs"
description: "Azure CLI-skript Sample - skapa en hanterad disk från en ögonblicksbild"
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.openlocfilehash: 549692f5027b3f50b0dd89fe701ebbf0f51d6031
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-cli"></a>Skapa en hanterad disk från en ögonblicksbild med CLI

Det här skriptet skapar en hanterad disk från en ögonblicksbild. Använd den toorestore en virtuell dator från ögonblicksbilder av Operativsystemet och datadiskarna. Skapa OS och data från respektive ögonblicksbilder-hanterade diskar och sedan skapa en ny virtuell dator genom att koppla hanterade diskar. Du kan även återställa datadiskar på en befintlig virtuell dator genom att koppla datadiskar som skapas med hjälp av ögonblicksbilder.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-managed-disks-from-snapshot/create-managed-disks-from-snapshot.sh "Create managed disk from snapshot")]


## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon toocreate hanterade diskar från en ögonblicksbild. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [AZ ögonblicksbild visa](https://docs.microsoft.com/cli/azure/snapshot#show) | Hämtar alla hello egenskaperna för en ögonblicksbild med hello namn och egenskaper för resursgrupp hello ögonblicksbilder. ID-egenskap är används toocreate hanterade diskar.  |
| [Skapa AZ disk](https://docs.microsoft.com/cli/azure/disk#create) | Skapar en hanterad disk med en ögonblicksbild Id för en hanterad ögonblicksbild |

## <a name="next-steps"></a>Nästa steg

[Skapa en virtuell dator genom att koppla en hanterade diskar som OS-disk](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare virtuella datorer och hanterade diskar CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
