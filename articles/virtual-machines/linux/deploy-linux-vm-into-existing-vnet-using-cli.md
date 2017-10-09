---
title: "aaaDeploy virtuella Linux-datorer i befintliga nätverk med Azure CLI 2.0 | Microsoft Docs"
description: "Lär dig hur toodeploy en Linux-dator till ett befintligt virtuellt nätverk med hjälp av hello Azure CLI 2.0"
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
ms.devlang: azurecli
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 0df44b3437002df050db56f3b3899083fb49d803
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-hello-azure-cli"></a>Hur toodeploy en Linux-dator i ett befintligt virtuellt nätverk med Azure med hello Azure CLI

Den här artikeln visar hur hello toouse Azure CLI 2.0 toodeploy en virtuell dator (VM) till ett befintligt virtuellt nätverk. hello kraven är:

- [ett Azure-konto](https://azure.microsoft.com/pricing/free-trial/)
- [offentliga och privata SSH-nyckelfiler](mac-create-ssh-keys.md)

Du kan också utföra dessa steg med hello [Azure CLI 1.0](deploy-linux-vm-into-existing-vnet-using-cli-nodejs.md).


## <a name="quick-commands"></a>Snabbkommandon
Om du behöver tooquickly utföra hello uppgiften, hello efter avsnittet information hello-kommandon som krävs. Mer detaljerad information och kontext för varje steg finns hello resten av dokumentet hello [startar här](#detailed-walkthrough).

toocreate den här anpassade miljön, behöver du hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).

Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn inkluderar *myResourceGroup*, *myVnet*, och *myVM*.

**Krav:** Azure resursgrupp, virtuella nätverk och undernät, inkommande nätverkssäkerhetsgrupp med SSH, och ett virtuellt nätverkskort.

### <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a>Distribuera hello VM till hello virtuell nätverksinfrastruktur

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Debian \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic
```

## <a name="detailed-walkthrough"></a>Detaljerad genomgång

Azure tillgångar som virtuella nätverk och nätverkssäkerhetsgrupper bör vara statiska och resurser som distribueras sällan kvar längre. När ett virtuellt nätverk har distribuerats, kan den återanvändas av nya distributioner utan någon negativ påverkar toohello infrastruktur. Tänka på ett virtuellt nätverk som en traditionell maskinvara nätverks-switch - behöver du inte tooconfigure en helt ny maskinvara växel med varje distribution. Med ett korrekt konfigurerat virtuellt nätverk kan du fortsätta toodeploy nya virtuella datorer i det virtuella nätverket flera gånger med få eventuella ändringar som krävs under hello hello virtuellt nätverk.

toocreate den här anpassade miljön, behöver du hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).

Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn inkluderar *myResourceGroup*, *myVnet*, och *myVM*.

## <a name="create-hello-resource-group"></a>Skapa hello resursgrupp

Först skapa en Azure-resurs grupp tooorganize allt som du skapar i den här genomgången. Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md). Skapa hello resursgrupp med [az gruppen skapa](/cli/azure/group#create). hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats:

```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

## <a name="create-hello-virtual-network"></a>Skapa hello virtuellt nätverk

Kan skapa ett virtuellt Azure-nätverk toolaunch hello virtuella datorer i. Mer information om virtuella nätverk finns [skapa ett virtuellt nätverk med hjälp av hello Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md). Skapa hello virtuellt nätverk med [az network vnet skapa](/cli/azure/network/vnet#create). hello följande exempel skapas ett virtuellt nätverk med namnet *myVnet* och undernät med namnet *mySubnet*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myVnet \
    --address-prefix 10.10.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 10.10.1.0/24
```

## <a name="create-hello-network-security-group"></a>Skapa hello nätverkssäkerhetsgrupp

Säkerhetsgrupper för Azure-nätverk är likvärdiga tooa brandväggen på hello nätverksnivån. Mer information om nätverkssäkerhetsgrupper finns [hur toocreate nätverkssäkerhet grupper i hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md). Skapa hello nätverkssäkerhetsgrupp med [az nätverket nsg skapa](/cli/azure/network/nsg#create). hello följande exempel skapar en nätverkssäkerhetsgrupp med namnet *myNetworkSecurityGroup*:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-ssh-allow-rule"></a>Lägg till regel för att tillåta en inkommande SSH

hello VM behöver åtkomst från hello internet, så att en regel som tillåter inkommande port 22 trafik toobe har passerat hello nätverket tooport 22 på hello VM krävs. Lägg till en inkommande regel för hello nätverkssäkerhetsgrupp med [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create). hello följande exempel skapas en regel med namnet *myNetworkSecurityGroupRuleSSH*:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleSSH \
    --priority 1000 \
    --protocol tcp \
    --destination-port-range 22 \
```

## <a name="attach-hello-subnet-toohello-network-security-group"></a>Koppla hello undernät toohello nätverkssäkerhetsgrupp

hello regler för nätverkssäkerhetsgrupper kan vara tillämpade tooa undernät eller ett specifikt virtuellt nätverksgränssnitt. Kan bifoga hello säkerhet grupp tooour undernät. Koppla dina undernät toohello nätverkssäkerhetsgrupp med [az network vnet undernät uppdatering](/cli/azure/network/vnet/subnet#update):

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="add-a-virtual-network-interface-card-toohello-subnet"></a>Lägg till ett virtuellt nätverk interface card toohello undernät

Virtuella nätverkskort (VNics) är viktigt eftersom du kan återanvända dem genom att ansluta dem toodifferent virtuella datorer. Den här återanvändning kan du tookeep hello VNic som en statisk resurs hello virtuella datorer kan vara tillfälliga. Skapa ett virtuellt nätverkskort och koppla den till hello undernät med [az nätverket nic skapa](/cli/azure/network/nic#create). hello följande exempel skapas ett virtuellt nätverkskort med namnet *myNic*:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet
```

## <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a>Distribuera hello VM till hello virtuell nätverksinfrastruktur

Nu har du ett virtuellt nätverk och undernät och en säkerhet grupp tooprotect hello undernät i nätverket genom att blockera all inkommande trafik utom port 22 för SSH. hello VM kan nu distribueras i det här befintliga nätverksinfrastruktur.

Skapa den virtuella datorn med [az vm skapa](/cli/azure/vm#create). Mer information om hello flaggar toouse med hello Azure CLI 2.0 toodeploy fullständig VM, finns [skapa en fullständig Linux-miljö med hjälp av hello Azure CLI](create-cli-complete.md).

hello följande exempel skapar en virtuell dator med hjälp av Azure hanterade diskar. Diskarna som hanteras av hello Azure-plattformen och kräver inte någon förberedelse eller plats toostore dem. Mer information om hanterade diskar finns i [Översikt över Azure Managed Disks](../../storage/storage-managed-disks-overview.md). Om du vill toouse ohanterad diskar finns i hello ytterligare kommentaren nedan.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Debian \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic
```

Hoppa över det här steget om du använder hanterade diskar. Om du vill toouse ohanterad diskar måste tooadd hello följande ytterligare parametrar toohello du fortsätter kommandot toocreate ohanterad diskar i hello lagringskontonamnet `mystorageaccount`: 

```azurecli
    --use-unmanaged-disk \
    --storage-account mystorageaccount
```

Med hjälp av hello flaggor CLI toocall befintliga resurser du instruera Azure toodeploy hello VM i hello befintliga nätverk. När ett virtuellt nätverk och undernät har distribuerats, kan lämnas som statisk eller permanenta resurser i Azure-region. I det här exemplet inte skapa och tilldela en offentlig IP-adress toohello virtuellt nätverkskort, så att den här virtuella datorn inte är offentligt tillgänglig över hello Internet. Mer information finns i [skapa en virtuell dator med en statisk offentlig IP-adress med hjälp av hello Azure CLI](../../virtual-network/virtual-network-deploy-static-pip-arm-cli.md).

## <a name="next-steps"></a>Nästa steg
Mer information om sätt toocreate virtuella datorer i Azure finns i hello följande resurser:

* [Använda en viss distribution för toocreate en Azure Resource Manager-mall](../windows/cli-deploy-templates.md)
* [Skapa en egen anpassad miljö för en virtuell Linux-dator med hjälp av Azure CLI-kommandon](create-cli-complete.md)
* [Skapa en Linux-VM på Azure med hjälp av mallar](create-ssh-secured-vm-from-template.md)
