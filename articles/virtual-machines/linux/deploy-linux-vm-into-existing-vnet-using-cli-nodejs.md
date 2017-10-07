---
title: "aaaDeploy virtuella Linux-datorer i befintliga nätverk med Azure CLI 1.0 | Microsoft Docs"
description: "Hur toodeploy en Linux VM till ett befintligt virtuellt nätverk med hjälp av hello Azure CLI 1.0"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: e660f1563d386efc7788bd236f8b067145ea09bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-hello-azure-cli-10"></a>Hur toodeploy en Linux-dator i ett befintligt virtuellt nätverk med Azure med hello Azure CLI 1.0

Den här artikeln beskrivs hur du toouse Azure CLI 1.0 toodeploy en virtuell dator (VM) till ett befintligt virtuellt nätverk (VNet). hello kraven är:

- [ett Azure-konto](https://azure.microsoft.com/pricing/free-trial/)
- [offentliga och privata SSH-nyckelfiler](mac-create-ssh-keys.md)


## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet
Du kan göra hello med hjälp av något av följande versioner av CLI hello:

- [Azure CLI 1.0](#quick-commands) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)
- [Azure CLI 2.0](deploy-linux-vm-into-existing-vnet-using-cli.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering


## <a name="quick-commands"></a>Snabbkommandon

Om du behöver tooquickly utföra hello uppgiften, hello efter avsnittet information hello-kommandon som krävs. Mer detaljerad information och kontext för varje steg finns hello resten av dokumentet hello [startar här](deploy-linux-vm-into-existing-vnet-using-cli.md#detailed-walkthrough).

Krav: Resursgrupp, VNet, NSG med SSH inkommande, undernät. Ersätt alla exempel med dina egna inställningar.

### <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a>Distribuera hello VM till hello virtuell nätverksinfrastruktur

```azurecli
azure vm create myVM \
    -g myResourceGroup \
    -l eastus \
    -y linux \
    -Q Debian \
    -o mystorageaccount \
    -u myAdminUser \
    -M ~/.ssh/id_rsa.pub \
    -n myVM \
    -F myVNet \
    -j mySubnet \
    -N myVNic
```

## <a name="detailed-walkthrough"></a>Detaljerad genomgång

Azure tillgångar som hello Vnet och nätverkssäkerhetsgrupper måste vara statiska och resurser som distribueras sällan kvar längre. När ett virtuellt nätverk har distribuerats, kan den återanvändas av nya distributioner utan någon negativ påverkar toohello infrastruktur. Tänk på att ett VNet som en nätverksväxel traditionella maskinvara. Du behöver inte tooconfigure en helt ny maskinvara växel med varje distribution. Med en korrekt konfigurerad VNet, kan du fortsätta toodeploy nya servrar i det virtuella nätverket flera gånger med några eventuella ändringar som krävs under hello hello virtuella nätverk.

## <a name="create-hello-resource-group"></a>Skapa hello resursgrupp

Först skapar du en resurs grupp tooorganize allt som du skapar i den här genomgången. Mer information om resursgrupper finns [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)

```azurecli
azure group create myResourceGroup --location eastus
```

## <a name="create-hello-vnet"></a>Skapa hello VNet

hello första steget är toobuild ett VNet toolaunch hello virtuella datorer i. Hej VNet innehåller ett undernät för den här genomgången. Mer information om virtuella Azure-nätverk finns [skapa ett virtuellt nätverk med hjälp av hello Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md)

```azurecli
azure network vnet create myVNet \
    --resource-group myResourceGroup \
    --address-prefixes 10.10.0.0/24 \
    --location eastus
```

## <a name="create-hello-network-security-group"></a>Skapa hello nätverkssäkerhetsgrupp

hello undernät bygger bakom en befintlig nätverkssäkerhetsgrupp så skapa hello nätverkssäkerhetsgruppen innan hello undernät. Säkerhetsgrupper för Azure-nätverk är likvärdiga tooa brandväggen på hello nätverksnivån. Läs mer på Azure nätverkssäkerhetsgrupper [hur toocreate nätverkssäkerhet grupper i hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md)

```azurecli
azure network nsg create myNetworkSecurityGroup \
    --resource-group myResourceGroup \
    --location eastus
```

## <a name="add-an-inbound-ssh-allow-rule"></a>Lägg till regel för att tillåta en inkommande SSH

hello VM behöver åtkomst från hello internet så att en regel som tillåter inkommande port 22 trafik toobe har passerat hello nätverket tooport 22 på hello VM krävs.

```azurecli
azure network nsg rule create inboundSSH \
    --resource-group myResourceGroup \
    --nsg-name myNSG \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 100 \
    --source-address-prefix Internet \
    --source-port-range 22 \
    --destination-address-prefix 10.10.0.0/24 \
    --destination-port-range 22
```

## <a name="add-a-subnet-toohello-vnet"></a>Lägg till ett undernät toohello VNet

Virtuella datorer i hello VNet måste finnas i ett undernät. Varje virtuellt nätverk kan ha flera undernät. Skapa hello undernät och associera med hello nätverkssäkerhetsgruppen.

```azurecli
azure network vnet subnet create mySubNet \
    --resource-group myResourceGroup \
    --vnet-name myVNet \
    --address-prefix 10.10.0.0/26 \
    --network-security-group-name myNetworkSecurityGroup
```

hello undernät är nu läggs till i hello virtuella nätverk och som är associerade med hello nätverkssäkerhetsgruppen och regeln.


## <a name="add-a-vnic-toohello-subnet"></a>Lägg till ett virtuellt nätverkskort toohello undernät

Virtuella nätverkskort (VNics) är viktigt eftersom du kan återanvända dem genom att ansluta dem toodifferent virtuella datorer. Den här metoden används för att hålla hello VNic som en statisk resurs hello virtuella datorer kan vara tillfälliga. Skapa ett virtuellt nätverkskort och koppla den till hello undernät som skapats i hello föregående steg.

```azurecli
azure network nic create myVNic \
    --resource-group myResourceGroup \
    --location eastus \
    ---subnet-vnet-name myVNet \
    --subnet-name mySubNet
```

## <a name="deploy-hello-vm-into-hello-vnet-and-nsg"></a>Distribuera hello VM till hello VNet och NSG

Nu har du ett VNet och undernät i det virtuella nätverket och en nätverkssäkerhetsgrupp fungerar tooprotect hello undernät genom att blockera all inkommande trafik utom port 22 för SSH. hello VM kan nu distribueras i det här befintliga nätverksinfrastruktur.

Med hjälp av hello Azure CLI och hello `azure vm create` kommandot hello Linux VM är distribuerade toohello befintliga Azure-resursgrupp, virtuella nätverk, undernät och virtuellt nätverkskort. Mer information om hur du använder hello CLI toodeploy fullständig VM finns [skapa en fullständig Linux-miljö med hjälp av hello Azure CLI](create-cli-complete.md)

```azurecli
azure vm create myVM \
    --resource-group myResourceGroup \
    --location eastus \
    --os-type linux \
    --image-urn Debian \
    --storage-account-name mystorageaccount \
    --admin-username myAdminUser \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --vnet-name myVNet \
    --vnet-subnet-name mySubnet \
    --nic-name myVNic
```

Med hjälp av hello flaggor CLI toocall befintliga resurser du instruera Azure toodeploy hello VM i hello befintliga nätverk. När ett VNet och undernät har distribuerats, kan lämnas som statisk eller permanenta resurser i Azure-region.  

## <a name="next-steps"></a>Nästa steg

* [Använda en viss distribution för toocreate en Azure Resource Manager-mall](../windows/cli-deploy-templates.md)
* [Skapa en egen anpassad miljö för en virtuell Linux-dator med hjälp av Azure CLI-kommandon](create-cli-complete.md)
* [Skapa en Linux-VM på Azure med hjälp av mallar](create-ssh-secured-vm-from-template.md)
