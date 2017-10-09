---
title: "aaaAzure skriptexempel CLI - kopia (flytta) ögonblicksbild av en toosame för hanterade diskar eller en annan prenumeration med CLI | Microsoft Docs"
description: "Skriptexempel Azure CLI - kopia (flytta) ögonblicksbild av en toosame för hanterade diskar eller en annan prenumeration med CLI"
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
ms.openlocfilehash: f214ab1fc1cb2cb42479d82e455f20a8cc55c83d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-snapshot-of-a-managed-disk-toosame-or-different-subscription-with-cli"></a>Kopiera ögonblicksbild av en toosame för hanterade diskar eller en annan prenumeration med CLI

Det här skriptet kopierar en ögonblicksbild av en toosame för hanterade diskar eller en annan prenumeration. Använd det här skriptet toomove en ögonblicksbild toodifferent prenumeration i hello samma region som hello överordnade ögonblicksbild.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "Copy snapshot")]


## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon toocreate en ögonblicksbild i hello mål en prenumeration med hjälp av hello-Id för hello källa ögonblicksbild. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [AZ ögonblicksbild visa](https://docs.microsoft.com/cli/azure/snapshot#show) | Hämtar alla hello egenskaperna för en ögonblicksbild med hello namn och egenskaper för resursgrupp hello ögonblicksbilder. ID-egenskap är används toocopy hello ögonblicksbild toodifferent prenumeration.  |
| [Skapa AZ ögonblicksbild](https://docs.microsoft.com/cli/azure/snapshot#create) | Kopierar en ögonblicksbild genom att skapa en ögonblicksbild av en annan prenumeration med hello-Id och namnet på hello överordnade ögonblicksbild.  |

## <a name="next-steps"></a>Nästa steg

[Skapa en virtuell dator från en ögonblicksbild](./virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare virtuella datorer och hanterade diskar CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
