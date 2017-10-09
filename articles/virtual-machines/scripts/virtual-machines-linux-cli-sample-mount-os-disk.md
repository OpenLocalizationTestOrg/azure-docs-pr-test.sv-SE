---
title: aaaAzure skriptexempel CLI - montera operativsystemdisk | Microsoft Docs
description: Skriptexempel Azure CLI - montera operativsystemdisk
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 5c614d09a64780575b70424d29052f1a6affec59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-vms-operating-system-disk"></a>Felsöka en operativsystemdisk för virtuella datorer

Det här skriptet monterar hello operativsystemdisken för en misslyckad eller problematiska virtuell dator som data disk tooa andra virtuella datorer. Detta kan vara användbar vid felsökning av disk problem eller återställa data. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "Quick Create VM")]

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon toocreate en resursgrupp, en virtuell dator, och alla relaterade resurser. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [AZ vm visa](https://docs.microsoft.com/cli/azure/vm#show) | Returnera en lista över virtuella datorer. I det här fallet är hello frågealternativet används tooreturn hello virtuella operativsystemdisken. Det här värdet läggs sedan tooa variabelnamnet 'uri'. |
| [AZ virtuella ta bort](https://docs.microsoft.com/cli/azure/vm#delete) | Tar bort en virtuell dator. |
| [Skapa AZ vm](https://docs.microsoft.com/cli/azure/vm#create) | Skapar en virtuell dator.  |
| [Koppla AZ virtuell disk](https://docs.microsoft.com/cli/azure/vm/disk#attach) | Bifogar en disk tooa virtuell dator. |
| [AZ vm lista-ip-adresser](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | Returnerar hello IP-adresser för en virtuell dator. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare virtuella CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
