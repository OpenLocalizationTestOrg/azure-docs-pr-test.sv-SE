---
title: "aaaAzure skriptexempel CLI - Export/kopiera ögonblicksbild som VHD tooa storage-konto i en annan region | Microsoft Docs"
description: "Skriptexempel Azure CLI - Export/kopiera ögonblicksbild som VHD tooa lagringskonto i samma eller olika prenumeration"
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
ms.openlocfilehash: 945f83d2ed715642156ca7b252af08559c652b14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-tooa-storage-account-in-different-region-with-cli"></a>Export/kopiera hanterad ögonblicksbilder som VHD tooa storage-konto i en annan region med CLI

Det här skriptet exporterar ett hanterade ögonblicksbild tooa storage-konto i annan region. Den första genererar hello hello ögonblicksbild SAS-URI och använder sedan toocopy den tooa storage-konto i en annan region. Använda skript toomaintain säkerhetskopian hanterade diskar i annan region för katastrofåterställning. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshots-to-storage-account/copy-snapshots-to-storage-account.sh "Copy snapshot")]


## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon toogenerate SAS-URI för en hanterad ögonblicksbilder och kopior och hello ögonblicksbild tooa lagringskonto med hjälp av SAS-URI. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [AZ ögonblicksbild bevilja åtkomst](https://docs.microsoft.com/cli/azure/snapshot#grant-access) | Genererar skrivskyddad SAS som används toocopy underliggande VHD-filen tooa lagringskonto eller ladda ned det lokala tooon  |
| [Starta AZ storage blob-kopia](https://docs.microsoft.com/en-us/cli/azure/storage/blob/copy#start) | Kopierar en blobb asynkront från en tooanother för storage-konto |

## <a name="next-steps"></a>Nästa steg

[Skapa en hanterad disk från en virtuell Hårddisk](virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

[Skapa en virtuell dator från en hanterad disk](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare virtuella datorer och hanterade diskar CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
