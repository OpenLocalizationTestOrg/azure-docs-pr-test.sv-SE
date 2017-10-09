---
title: "aaaAzure CLI skriptexempel - skapa en hanterad disk från en VHD-fil i ett lagringskonto i hello samma prenumeration | Microsoft Docs"
description: "Azure CLI-skript Sample - skapa en hanterad disk från en VHD-fil i ett lagringskonto i hello samma prenumeration"
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
ms.openlocfilehash: 6cfb3c54a7692b0f3999c585861340c1a6b4d348
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-hello-same-subscription-with-cli"></a>Skapa en hanterad disk från en VHD-fil i ett lagringskonto i hello samma prenumeration med CLI

Det här skriptet skapar en hanterad disk från en VHD-fil i ett lagringskonto i hello samma prenumeration. Använd det här skriptet tooimport en särskild (inte generaliserad/Sysprep) VHD toomanaged OS disk toocreate en virtuell dator. Eller använda den tooimport en VHD toomanaged data datadisk. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-managed-data-disks-from-vhd/create-managed-data-disks-from-vhd.sh "Create managed disk from VHD")]


## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon toocreate hanterade diskar från en virtuell Hårddisk. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ disk](https://docs.microsoft.com/cli/azure/disk#create) | Skapar en hanterad disk med hjälp av en VHD-URI i ett lagringskonto i hello samma prenumeration |

## <a name="next-steps"></a>Nästa steg

[Skapa en virtuell dator genom att koppla en hanterade diskar som OS-disk](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare virtuella datorer och hanterade diskar CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
