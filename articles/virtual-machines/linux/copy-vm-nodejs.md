---
title: aaaCreate en kopia av din Linux VM med hello Azure CLI 1.0 | Microsoft Docs
description: "Lär dig hur toocreate en kopia av den virtuella datorn i Azure Linux med hello Azure CLI 1.0 i hello Resource Manager-distributionsmodellen"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
tags: azure-resource-manager
ms.assetid: 770569d2-23c1-4a5b-801e-cddcd1375164
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 997a2c8109e7083ececd76fd1013e9ed4d3e6afd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-linux-virtual-machine-running-on-azure-with-hello-azure-cli-10"></a>Skapa en kopia av en Linux-dator som körs på Azure med hello Azure CLI 1.0
Den här artikeln visar hur toocreate en kopia av din Azure-dator (VM) kör Linux med hello Resource Manager-distributionsmodellen. Först du kopiera över hello operativsystemet och data diskar tooa ny behållare, och sedan konfigurera hello nätverksresurser och skapa hello ny virtuell dator.

Du kan också [överför och skapa en virtuell dator från anpassade diskavbildning](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet
Du kan göra hello med hjälp av något av följande versioner av CLI hello:

- Azure CLI 1.0 – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)
- [Azure CLI 2.0](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering

## <a name="before-you-begin"></a>Innan du börjar
Se till att du uppfyller följande krav innan du börjar hello åtgärderna hello:

* Du har hello [Azure CLI](../../cli-install-nodejs.md) hämtas och installeras på din dator. 
* Du måste också information om dina befintliga virtuella Azure Linux-datorn:

| Information om källdatorn VM | Där tooget den |
| --- | --- |
| VM-namn |`azure vm list` |
| Resursgruppens namn |`azure vm list` |
| Plats |`azure vm list` |
| Lagringskontonamnet |`azure storage account list -g <resourceGroup>` |
| Behållarens namn |`azure storage container list -a <sourcestorageaccountname>` |
| Namn på datakälla VM VHD-filen |`azure storage blob list --container <containerName>` |

* Du behöver toomake vissa alternativ om din nya VM:   <br> -Behållarnamn   <br> Namn på virtuell dator-   <br> VM - storlek   <br> vNet - namn   <br> -Undernätsnamn   <br> -IP-namn   <br> NIC - namn

## <a name="login-and-set-your-subscription"></a>Logga in och ange din prenumeration
1. Inloggningen toohello CLI.

    ```azurecli
    azure login
    ```
2. Kontrollera att du är i läget Resource Manager.

    ```azurecli
    azure config mode arm
    ```
3. Ange hello korrekt prenumeration. Du kan använda 'list azure-konto' toosee alla dina prenumerationer.

    ```azurecli
    azure account set mySubscriptionID
    ```

## <a name="stop-hello-vm"></a>Stoppa hello VM
Stoppa och frigöra Virtuella hello källdatorn. Du kan använda 'azure vm list' tooget en lista över alla hello virtuella datorer i din prenumeration och deras resursgruppnamn.

```azurecli
azure vm stop myResourceGroup myVM
azure vm deallocate myResourceGroup MyVM
```


## <a name="copy-hello-vhd"></a>Kopiera hello VHD
Du kan kopiera hello VHD från hello källa toohello lagringsplats med hello `azure storage blob copy start`. I det här exemplet ska vi toocopy hello VHD toohello samma lagringskonto, men en annan behållare.

toocopy hello VHD tooanother behållare i hello samma lagringskonto, typ:

```azurecli
azure storage blob copy start \
        https://mystorageaccountname.blob.core.windows.net:8080/mycontainername/myVHD.vhd \
        myNewContainerName
```

## <a name="set-up-hello-virtual-network-for-your-new-vm"></a>Konfigurera hello virtuellt nätverk för den nya virtuella datorn
Ange ett virtuellt nätverk och nätverkskort för den nya virtuella datorn. 

```azurecli
azure network vnet create myResourceGroup myVnet -l myLocation

azure network vnet subnet create -a <address.prefix.in.CIDR/format> myResourceGroup myVnet mySubnet

azure network public-ip create myResourceGroup myPublicIP -l myLocation

azure network nic create myResourceGroup myNic -k mySubnet -m myVnet -p myPublicIP -l myLocation
```


## <a name="create-hello-new-vm"></a>Skapa hello ny virtuell dator
Nu kan du skapa en virtuell dator från den överförda virtuella hårddisken [med hjälp av en resource manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) eller via hello kopieras CLI genom att ange hello URI tooyour disk genom att skriva:

```azurecli
azure vm create -n myVM -l myLocation -g myResourceGroup -f myNic \
        -z Standard_DS1_v2 -y Linux \
        https://mystorageaccountname.blob.core.windows.net:8080/mycontainername/myVHD.vhd 
```



## <a name="next-steps"></a>Nästa steg
toolearn hur toouse Azure CLI toomanage din nya virtuella datorn finns [Azure CLI-kommandona för hello Azure Resource Manager](../azure-cli-arm-commands.md).

