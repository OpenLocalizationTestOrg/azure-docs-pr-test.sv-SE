---
title: "aaaCopy en Linux VM med hjälp av Azure CLI 2.0 | Microsoft Docs"
description: "Lär dig hur toocreate en kopia av din virtuella Azure Linux-datorn med hjälp av Azure CLI 2.0 och hanterade diskar."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
tags: azure-resource-manager
ms.assetid: 770569d2-23c1-4a5b-801e-cddcd1375164
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 03/10/2017
ms.author: cynthn
ms.openlocfilehash: ee34a4259dd0c1e7bf49312fe3fe3ba809bf69e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-linux-vm-by-using-azure-cli-20-and-managed-disks"></a>Skapa en kopia av en Linux-VM med hjälp av Azure CLI 2.0 och hanterade diskar


Den här artikeln visar hur toocreate en kopia av din Azure virtuell dator (VM kör Linux med hjälp av) hello Azure CLI 2.0 och hello Azure Resource Manager-distributionsmodellen. Du kan också utföra dessa steg med hello [Azure CLI 1.0](copy-vm-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Du kan också [överför och skapa en virtuell dator från en virtuell Hårddisk](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="prerequisites"></a>Krav


-   Installera [Azure CLI 2.0](/cli/azure/install-az-cli2)

-   Logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).

-   Ha en Azure VM toouse som hello källa för din kopia.

## <a name="step-1-stop-hello-source-vm"></a>Steg 1: Stoppa hello källa VM


Frigöra Virtuella hello källdatorn med hjälp av [az vm frigöra](/cli/azure/vm#deallocate).
hello följande exempel tar bort hello virtuella datorn med namnet **myVM** i hello resursgruppen **myResourceGroup**:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

## <a name="step-2-copy-hello-source-vm"></a>Steg 2: Hello kopieringskälla VM


toocopy en virtuell dator, skapa en kopia av hello underliggande virtuell hårddisk. Den här processen skapar en särskild virtuell Hårddisk som en hanterad Disk som innehåller hello inställningar som hello datakällan VM.

Mer information om Azure Managed Disks finns i [Översikt över Azure Managed Disks](../windows/managed-disks-overview.md). 

1.  Visa en lista över varje virtuell dator och hello namnet på dess OS-disken med [az vm listan](/cli/azure/vm#list). hello följande exempel visar en lista över alla virtuella datorer i resursgruppen med namnet **myResourceGroup**:
    
    ```azurecli
    az vm list -g myTestRG --query '[].{Name:name,DiskName:storageProfile.osDisk.name}' --output table
    ```

    hello utdata är liknande toohello följande exempel:

    ```azurecli
    Name    DiskName
    ------  --------
    myVM    myDisk
    ```

1.  Kopiera hello disk genom att skapa en ny hanteras disk med hjälp av [az disk skapa](/cli/azure/disk#create). hello följande exempel skapas en disk med namnet **myCopiedDisk** från hello hanteras disk med namnet **myDisk**:

    ```azurecli
    az disk create --resource-group myResourceGroup --name myCopiedDisk --source myDisk
    ``` 

1.  Kontrollera diskar hello hanteras nu i resursgruppen med hjälp av [az Disklista](/cli/azure/disk#list). hello följande exempel visar hello hanterade diskar i hello resursgrupp med namnet **myResourceGroup**:

    ```azurecli
    az disk list --resource-group myResourceGroup --output table
    ```

1.  Hoppa över för[”steg 3: konfigurera ett virtuellt nätverk”](#step-3-set-up-a-virtual-network).


## <a name="step-3-set-up-a-virtual-network"></a>Steg 3: Konfigurera ett virtuellt nätverk


hello skapa följande valfria steg ett nytt virtuellt nätverk, undernät, offentliga IP-adressen och virtuella nätverksgränssnittskortet (NIC).

Om du kopierar en virtuell dator för att felsöka ändamål eller ytterligare distributioner kan du inte vill toouse en virtuell dator i ett befintligt virtuellt nätverk.

Om du vill toocreate infrastruktur för ett virtuellt nätverk för de kopierade virtuella datorerna, hello Följ nästa steg. Om du inte vill toocreate ett virtuellt nätverk kan du hoppa över för[steg 4: skapa en virtuell dator](#step-4-create-a-vm).

1.  Skapa hello virtuellt nätverk med hjälp av [az network vnet skapa](/cli/azure/network/vnet#create). hello följande exempel skapas ett virtuellt nätverk med namnet **myVnet** och ett undernät med namnet **mySubnet**:

    ```azurecli
    az network vnet create --resource-group myResourceGroup --location westus --name myVnet \
        --address-prefix 192.168.0.0/16 --subnet-name mySubnet --subnet-prefix 192.168.1.0/24
    ```

1.  Skapa en offentlig IP-adress med hjälp av [az nätverket offentliga IP-skapa](/cli/azure/network/public-ip#create). hello följande exempel skapas en offentlig IP-adress med namnet **myPublicIP** med hello DNS-namnet för **mypublicdns**. (hello DNS-namn måste vara unikt, så ange ett unikt namn)

    ```azurecli
    az network public-ip create --resource-group myResourceGroup --location westus \
        --name myPublicIP --dns-name mypublicdns --allocation-method static --idle-timeout 4
    ```

1.  Skapa hello nätverkskortet använder [az nätverket nic skapa](/cli/azure/network/nic#create).
    hello följande exempel skapas ett nätverkskort med namnet **myNic** som bifogade toothe **mySubnet** undernät:

    ```azurecli
    az network nic create --resource-group myResourceGroup --location westus --name myNic \
        --vnet-name myVnet --subnet mySubnet --public-ip-address myPublicIP
    ```

## <a name="step-4-create-a-vm"></a>Steg 4: Skapa en virtuell dator

Nu kan du skapa en virtuell dator med hjälp av [az vm skapa](/cli/azure/vm#create).

Ange hello kopieras hanterade diskar toouse som hello OS-disk (--bifoga-os-disk), enligt följande:

```azurecli
az vm create --resource-group myResourceGroup --name myCopiedVM \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics myNic --size Standard_DS1_v2 --os-type Linux \
    --attach-os-disk myCopiedDisk
```

## <a name="next-steps"></a>Nästa steg

toolearn hur toouse Azure CLI toomanage din nya VM finns [Azure CLI-kommandona för hello Azure Resource Manager](../azure-cli-arm-commands.md).
