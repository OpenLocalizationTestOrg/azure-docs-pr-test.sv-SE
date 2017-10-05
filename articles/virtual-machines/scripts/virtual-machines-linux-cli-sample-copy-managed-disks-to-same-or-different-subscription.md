---
title: Azure CLI skriptexempel - kopia (flytta) hanterade diskar till samma eller olika prenumeration | Microsoft Docs
description: Azure CLI skriptexempel - kopia (flytta) hanterade diskar till samma eller en annan prenumeration
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
ms.openlocfilehash: dcf92babf84872ffbbba81127952f8422104c723
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="copy-managed-disks-to-same-or-different-subscription-with-cli"></a>Kopiera hanterade diskar till samma eller en annan prenumeration med CLI

Det här skriptet kopierar hanterade diskar till samma eller olika prenumeration men i samma region. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli[huvudsakliga](../../../cli_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "kopiera hanteras disk")]


## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon för att skapa en ny hanterade disk i målprenumerationen med ID: T för den hanterade källdisken. Varje kommando i tabellen länkar till kommandot viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [AZ disk visa](https://docs.microsoft.com/cli/azure/disk#show) | Hämtar alla egenskaper för en hanterad disk med hjälp av namn och egenskaper för resursgrupp för hanterade diskar. ID-egenskapen används för att kopiera hanterade diskar till en annan prenumeration.  |
| [Skapa AZ disk](https://docs.microsoft.com/cli/azure/disk#create) | Kopior av hanterade diskar genom att skapa en ny hanteras disk i en annan prenumeration med Id och namn överordnade hanterade disken.  |

## <a name="next-steps"></a>Nästa steg

[Skapa en virtuell dator från en hanterad disk](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare virtuella datorer och hanterade diskar CLI skriptexempel finns i den [virtuella Azure Linux-datorn dokumentationen](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
