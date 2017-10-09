---
title: "aaaCreate en Linux VM i Azure med flera nätverkskort | Microsoft Docs"
description: "Lär dig hur toocreate en Linux VM med flera nätverkskort anslutna tooit med hello Azure CLI eller Resource Manager-mallar."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 457dab734ceeeefd35cddaf1ebb9ea0a82f4e207
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-virtual-machine-with-multiple-nics-using-hello-azure-cli-10"></a>Skapa en virtuell Linux-dator med flera nätverkskort med hello Azure CLI 1.0
Du kan skapa en virtuell dator (VM) i Azure som har flera virtuella nätverk gränssnitt (NIC) som är anslutna tooit. Ett vanligt scenario är toohave olika undernät för frontend och backend-anslutning eller ett nätverk dedikerad tooa övervakning eller lösning för säkerhetskopiering. Den här artikeln innehåller snabb kommandon toocreate en virtuell dator med flera nätverkskort anslutna tooit. För detaljerad information, inklusive hur toocreate flera nätverkskort i en egen Bash-skript, Läs mer om [distribution av flera nätverkskort VMs](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Olika [VM-storlekar](sizes.md) stöder olika antal nätverkskort, så därför storlek den virtuella datorn.

> [!WARNING]
> När du skapar en virtuell dator – du kan inte lägga till nätverkskort tooan befintlig virtuell dator med hello Azure CLI 1.0 måste du koppla flera nätverkskort. Du kan [lägga till nätverkskort tooan befintlig virtuell dator med hello Azure CLI 2.0](multiple-nics.md). Du kan också [skapa en virtuell dator baserat på hello ursprungliga virtuella diskarna](copy-vm.md) och skapa flera nätverkskort som du distribuerar hello VM.


## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet
Du kan göra hello med hjälp av något av följande versioner av CLI hello:

- [Azure CLI 1.0](#create-supporting-resources) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)
- [Azure CLI 2.0](multiple-nics.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering


## <a name="create-supporting-resources"></a>Skapa stödresurser
Se till att du har hello [Azure CLI](../../cli-install-nodejs.md) loggas och med hjälp av Resource Manager-läge:

```azurecli
azure config mode arm
```

Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn ingår *myResourceGroup*, *mittlagringskonto*, och *myVM*.

Först skapa en resursgrupp. hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats:

```azurecli
azure group create myResourceGroup --location eastus
```

Skapa en storage-konto toohold dina virtuella datorer. hello följande exempel skapas ett lagringskonto med namnet *mittlagringskonto*:

```azurecli
azure storage account create mystorageaccount \
    --resource-group myResourceGroup \
    --location eastus \
    --kind Storage \
    --sku-name PLRS
```

Skapa ett virtuellt nätverk tooconnect dina virtuella datorer till. hello följande exempel skapas ett virtuellt nätverk med namnet *myVnet* med en adressprefixet *192.168.0.0/16*:

```azurecli
azure network vnet create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myVnet \
    --address-prefixes 192.168.0.0/16
```

Skapa två undernät för virtuellt nätverk – ett för frontend-trafik och en för backend-trafik. hello följande exempel skapar två undernät, med namnet *mySubnetFrontEnd* och *mySubnetBackEnd*:

```azurecli
azure network vnet subnet create \
    --resource-group myResourceGroup \
    --location myVnet \
    --name mySubnetFrontEnd \
    --address-prefix 192.168.1.0/24
azure network vnet subnet create \
    --resource-group myResourceGroup \
    --location myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

## <a name="create-and-configure-multiple-nics"></a>Skapa och konfigurera flera nätverkskort
Du kan läsa mer information om [distribution av flera nätverkskort med hello Azure CLI](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md), inklusive skript hello processen att loopa via toocreate alla hello-nätverkskort.

hello följande exempel skapar två nätverkskort som heter *myNic1* och *myNic2*, med ett nätverkskort som ansluter tooeach undernät:

```azurecli
azure network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic1 \
    --subnet-vnet-name myVnet \
    --subnet-name mySubnetFrontEnd
azure network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic2 \
    --subnet-vnet-name myVnet \
    --subnet-name mySubnetBackEnd
```

Vanligtvis du också skapa en [Nätverkssäkerhetsgruppen](../../virtual-network/virtual-networks-nsg.md) eller [belastningsutjämnare](../../load-balancer/load-balancer-overview.md) toohelp hantera och distribuera trafik mellan dina virtuella datorer. hello följande exempel skapar en Nätverkssäkerhetsgrupp med namnet *myNetworkSecurityGroup*:

```azurecli
azure network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

Binda med dina nätverkskort toohello Nätverkssäkerhetsgruppen `azure network nic set`. hello följande exempel Binder *myNic1* och *myNic2* med *myNetworkSecurityGroup*:

```azurecli
azure network nic set \
    --resource-group myResourceGroup \
    --name myNic1 \
    --network-security-group-name myNetworkSecurityGroup
azure network nic set \
    --resource-group myResourceGroup \
    --name myNic2 \
    --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-a-vm-and-attach-hello-nics"></a>Skapa en virtuell dator och koppla hello nätverkskort
När du skapar hello VM kan ange du nu flera nätverkskort. I stället använder `--nic-name` tooprovide ett enda nätverkskort i stället använder du `--nic-names` och ange en kommaavgränsad lista över nätverkskort. Du måste också tootake försiktig när du väljer hello VM-storlek. Det finns begränsningar för hello Totalt antal nätverkskort som du lägger till tooa VM. Läs mer om [Linux VM-storlekar](sizes.md). hello följande exempel visas hur toospecify flera nätverkskort och en virtuell dator storleken som stöds med hjälp av flera nätverkskort (*Standard_DS2_v2*):

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location eastus \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username azureuser \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
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
Se till att tooreview [Linux VM-storlekar](sizes.md) vid toocreating en virtuell dator med flera nätverkskort. Betala uppmärksamhet toohello maximalt antal nätverkskort som har stöd för varje VM-storlek. 

Kom ihåg att du kan inte lägga till ytterligare nätverkskort tooan befintlig virtuell dator måste du skapa alla hello nätverkskort när du distribuerar hello VM. Vara försiktig när du planerar din distributioner toomake till att du har alla hello krävs nätverksanslutning från hello början.

