---
title: "aaaCreate en Linux VM i Azure med flera nätverkskort | Microsoft Docs"
description: "Lär dig hur toocreate en Linux VM med flera nätverkskort anslutna tooit med hello Azure CLI 2.0 eller Resource Manager-mallar."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 5d2d04d0-fc62-45fa-88b1-61808a2bc691
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 2723405914777a5dce4354d4f5d8413e357f58e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-linux-virtual-machine-in-azure-with-multiple-network-interface-cards"></a>Hur toocreate en Linux-dator i Azure med flera nätverkskort
Du kan skapa en virtuell dator (VM) i Azure som har flera virtuella nätverk gränssnitt (NIC) som är anslutna tooit. Ett vanligt scenario är toohave olika undernät för frontend och backend-anslutning eller ett nätverk dedikerad tooa övervakning eller lösning för säkerhetskopiering. Den här artikeln beskrivs hur toocreate en virtuell dator med flera nätverkskort anslutna tooit och tooadd eller ta bort nätverkskort från en befintlig virtuell dator. För detaljerad information, inklusive hur toocreate flera nätverkskort i en egen Bash-skript, Läs mer om [distribution av flera nätverkskort VMs](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Olika [VM-storlekar](sizes.md) stöder olika antal nätverkskort, så därför storlek den virtuella datorn.

Den här artikeln beskrivs hur toocreate en virtuell dator med flera nätverkskort med hello Azure CLI 2.0. Du kan också utföra dessa steg med hello [Azure CLI 1.0](multiple-nics-nodejs.md).


## <a name="create-supporting-resources"></a>Skapa stödresurser
Installera hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).

Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn ingår *myResourceGroup*, *mittlagringskonto*, och *myVM*.

Börja med att skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create). hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats:

```azurecli
az group create --name myResourceGroup --location eastus
```

Skapa hello virtuellt nätverk med [az network vnet skapa](/cli/azure/network/vnet#create). hello följande exempel skapas ett virtuellt nätverk med namnet *myVnet* och undernät med namnet *mySubnetFrontEnd*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnetFrontEnd \
    --subnet-prefix 192.168.1.0/24
```

Skapa ett undernät för hello backend-trafik med [az undernät för virtuellt nätverk skapa](/cli/azure/network/vnet/subnet#create). hello följande exempel skapas ett undernät med namnet *mySubnetBackEnd*:

```azurecli
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

Skapa en nätverkssäkerhetsgrupp med [az nätverket nsg skapa](/cli/azure/network/nsg#create). hello följande exempel skapar en nätverkssäkerhetsgrupp med namnet *myNetworkSecurityGroup*:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="create-and-configure-multiple-nics"></a>Skapa och konfigurera flera nätverkskort
Skapa två nätverkskort med [az nätverket nic skapa](/cli/azure/network/nic#create). hello följande exempel skapar två nätverkskort som heter *myNic1* och *myNic2*, ansluten hello säkerhetsgrupp för nätverk, med ett nätverkskort som ansluter tooeach undernät:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic1 \
    --vnet-name myVnet \
    --subnet mySubnetFrontEnd \
    --network-security-group myNetworkSecurityGroup
az network nic create \
    --resource-group myResourceGroup \
    --name myNic2 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

## <a name="create-a-vm-and-attach-hello-nics"></a>Skapa en virtuell dator och koppla hello nätverkskort
När du skapar hello VM, ange hello nätverkskort du skapade med `--nics`. Du måste också tootake försiktig när du väljer hello VM-storlek. Det finns begränsningar för hello Totalt antal nätverkskort som du lägger till tooa VM. Läs mer om [Linux VM-storlekar](sizes.md). 

Skapa en virtuell dator med [az vm create](/cli/azure/vm#create). hello följande exempel skapas en virtuell dator med namnet *myVM*:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --size Standard_DS3_v2 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic1 myNic2
```

## <a name="add-a-nic-tooa-vm"></a>Lägg till NIC-tooa VM
hello föregående steg skapas en virtuell dator med flera nätverkskort. Du kan också lägga till nätverkskort tooan befintlig virtuell dator med hello Azure CLI 2.0. 

Skapa ett annat nätverkskort med [az nätverket nic skapa](/cli/azure/network/nic#create). hello följande exempel skapas ett nätverkskort med namnet *myNic3* anslutna toohello backend-undernät och nätverkssäkerhetsgruppen skapade i föregående steg i hello:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic3 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

tooadd ett NIC-tooan befintliga VM först frigöra hello virtuell dator med [az vm frigöra](/cli/azure/vm#deallocate). hello följande exempel tar bort hello virtuella datorn med namnet *myVM*:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Lägg till hello nätverkskortet med [az vm nic lägga till](/cli/azure/vm/nic#add). hello följande exempel läggs *myNic3* för*myVM*:

```azurecli
az vm nic add \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

Starta virtuell dator med hello [az vm start](/cli/azure/vm#start):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```

## <a name="remove-a-nic-from-a-vm"></a>Ta bort ett nätverkskort från en virtuell dator
tooremove ett nätverkskort från en befintlig virtuell dator först frigöra hello VM med [az vm frigöra](/cli/azure/vm#deallocate). hello följande exempel tar bort hello virtuella datorn med namnet *myVM*:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Ta bort hello nätverkskortet med [az vm nic ta bort](/cli/azure/vm/nic#remove). hello följande exempel tar bort *myNic3* från *myVM*:

```azurecli
az vm nic remove \
    --resource-group myResourceGroup \
    --vm-name myVM 
    --nics myNic3
```

Starta virtuell dator med hello [az vm start](/cli/azure/vm#start):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```


## <a name="create-multiple-nics-using-resource-manager-templates"></a>Skapa flera nätverkskort med hjälp av Resource Manager-mallar
Azure Resource Manager-mallar använda deklarativa JSON filer toodefine din miljö. Du kan läsa ett [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md). Resource Manager-mallar ger ett sätt toocreate flera instanser av en resurs under distributionen, till exempel skapa flera nätverkskort. Du använder *kopiera* toospecify hello antal instanser toocreate:

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Läs mer om [skapar flera instanser med *kopiera*](../../resource-group-create-multiple.md). 

Du kan också använda en `copyIndex()` toothen Lägg till ett nummer tooa resursnamn som gör att du toocreate `myNic1`, `myNic2`, etc. hello följande visar ett exempel på att lägga till hello indexvärde:

```json
"name": "[concat('myNic', copyIndex())]", 
```

Du kan läsa en komplett exempel på [skapar flera nätverkskort med hjälp av Resource Manager-mallar](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Nästa steg
Granska [Linux VM-storlekar](sizes.md) vid toocreating en virtuell dator med flera nätverkskort. Betala uppmärksamhet toohello maximalt antal nätverkskort som har stöd för varje VM-storlek. 
