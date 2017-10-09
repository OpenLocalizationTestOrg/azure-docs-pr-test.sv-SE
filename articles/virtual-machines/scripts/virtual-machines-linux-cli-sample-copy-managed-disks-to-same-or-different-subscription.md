---
title: aaaAzure CLI skriptexempel - kopia (flytta) hanteras diskar toosame eller annan prenumeration | Microsoft Docs
description: Azure CLI skriptexempel - kopia (flytta) hanteras diskar toosame eller en annan prenumeration
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
ms.openlocfilehash: b1fa207bd6e05d7094be08855e7823e3b7686013
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-managed-disks-toosame-or-different-subscription-with-cli"></a>Kopiera toosame för hanterade diskar eller annan prenumeration med CLI

Det här skriptet kopierar en toosame för hanterade diskar eller en annan prenumeration men hello i samma region. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "Copy managed disk")]


## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon toocreate en ny hanterade disk i hello mål en prenumeration med hjälp av hello-Id för hello källa hanterade disken. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [AZ disk visa](https://docs.microsoft.com/cli/azure/disk#show) | Hämtar alla hello egenskaper för en hanterad disk med hjälp av egenskaper för hello-namn och en resurs grupp av hello hanterade diskar. ID-egenskap är används toocopy hello hanteras disk toodifferent prenumeration.  |
| [Skapa AZ disk](https://docs.microsoft.com/cli/azure/disk#create) | Kopierar hanterade diskar genom att skapa en ny hanterad disk i en annan prenumeration med Id och namn hello överordnade hanterade disken.  |

## <a name="next-steps"></a>Nästa steg

[Skapa en virtuell dator från en hanterad disk](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare virtuella datorer och hanterade diskar CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
