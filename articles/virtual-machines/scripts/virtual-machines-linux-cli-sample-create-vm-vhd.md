---
title: "aaaAzure CLI skriptexempel - skapa en virtuell dator med en virtuell Hårddisk | Microsoft Docs"
description: "Azure CLI-skript Sample - skapa en virtuell dator med hjälp av en virtuell hårddisk."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/09/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: ce39092697a51e4e8a8e59ba8eb919955f616458
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-virtual-hard-disk"></a>Skapa en virtuell dator med en virtuell hårddisk

Det här exemplet skapar en virtuell dator med en virtuell Hårddisk.
En resursgrupp, ett lagringskonto och en behållare skapas och den skapar en virtuell dator genom att ladda upp toohello hello VHD-behållare.
Ersätter hello ssh offentlig nyckel med den offentliga nyckeln så att du har åtkomst toohello VM.

Du behöver en startbar virtuell Hårddisk.
Du kan hämta hello VHD som vi använde från https://azclisamples.blob.core.windows.net/vhds/sample.vhd eller använda din egen VHD. hello skript söker efter `~/sample.vhd`.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "Create VM using a VHD")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.

```azurecli-interactive 
az group delete -n az-cli-vhd
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon toocreate en resursgrupp, virtuella, tillgänglighetsuppsättning, belastningsutjämnare och alla relaterade resurser. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [AZ lagringskontolistan](https://docs.microsoft.com/cli/azure/storage/account#list) | Visar storage-konton |
| [AZ check-lagringskontonamnet](https://docs.microsoft.com/cli/azure/storage/account#check-name) | Kontrollerar att namnet på ett lagringskonto är giltig och att den inte redan finns |
| [AZ nycklar lagringskontolistan](https://docs.microsoft.com/cli/azure/storage/account/keys#list) | Visar en lista över nycklar för hello storage-konton |
| [AZ lagringsblob finns](https://docs.microsoft.com/cli/azure/storage/blob#exists) | Kontrollerar om hello blob finns |
| [Skapa AZ lagringsbehållare](https://docs.microsoft.com/cli/azure/storage/container#create) | Skapar en behållare i ett lagringskonto. |
| [ladda upp AZ storage-blob](https://docs.microsoft.com/cli/azure/storage/blob#upload) | Skapar en blob i hello-behållaren genom att ladda upp hello VHD. |
| [AZ vm lista](https://docs.microsoft.com/cli/azure/vm#list) | Används med `--query` kontrollera om hello VM namn används. | 
| [Skapa AZ vm](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | Skapar hello virtuella datorer. |
| [AZ vm åtkomst set linux-användare](https://docs.microsoft.com/cli/azure/vm/access#set-linux-user) | Återställer hello SSH key toogive hello aktuella användare åtkomst toohello VM. |
| [AZ vm lista-ip-adresser](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | Hämtar hello IP-adressen för hello VM som har skapats. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare virtuella CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
