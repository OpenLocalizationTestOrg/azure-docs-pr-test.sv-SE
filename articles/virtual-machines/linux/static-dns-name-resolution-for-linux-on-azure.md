---
title: "aaaUse interna DNS för VM-namnmatchning med hello Azure CLI 2.0 | Microsoft Docs"
description: "Hur toocreate virtuellt nätverkskort och använder interna DNS för namnmatchning för virtuell dator på Azure med hello Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 02/16/2017
ms.author: v-livech
ms.openlocfilehash: b3c4bfd3ab698f7b25d763ba9e60dd7984f6269d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-virtual-network-interface-cards-and-use-internal-dns-for-vm-name-resolution-on-azure"></a>Skapa virtuella nätverkskort och använda interna DNS för namnmatchning för virtuell dator på Azure
Den här artikeln visar hur tooset statiska interna DNS-namn för Linux virtuella datorer med virtuella nätverk nätverkskort (vNics) och DNS-etikettnamn med hello Azure CLI 2.0. Du kan också utföra dessa steg med hello [Azure CLI 1.0](static-dns-name-resolution-for-linux-on-azure-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Statisk DNS-namn används för permanenta infrastrukturtjänster som en Jenkins build-server, som används för det här dokumentet eller en Git-server.

hello kraven är:

* [ett Azure-konto](https://azure.microsoft.com/pricing/free-trial/)
* [offentliga och privata SSH-nyckelfiler](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a>Snabbkommandon
Om du behöver tooquickly utföra hello uppgiften, hello efter avsnittet information hello-kommandon som krävs. Mer detaljerad information och kontext för varje steg i hello resten av dokumentet hello [startar här](#detailed-walkthrough). tooperform dessa steg, behöver du hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).

Krav: Resursgrupp, virtuella nätverk och undernät, Nätverkssäkerhetsgrupp med SSH inkommande.

### <a name="create-a-virtual-network-interface-card-with-a-static-internal-dns-name"></a>Skapa ett virtuellt nätverkskort med ett statiskt interna DNS-namn
Skapa hello vNic med [az nätverket nic skapa](/cli/azure/network/nic#create). Hej `--internal-dns-name` CLI-flaggan är för inställningen hello DNS-etikett, vilket ger hello statisk DNS-namn för hello virtuella nätverksgränssnittskortet (vNic). hello följande exempel skapas ett virtuellt nätverkskort med namnet `myNic`, ansluter den toohello `myVnet` virtuella nätverk och skapar en intern DNS-namnpost kallas `jenkins`:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

### <a name="deploy-a-vm-and-connect-hello-vnic"></a>Distribuera en virtuell dator och Anslut hello vNic
Skapa en virtuell dator med [az vm create](/cli/azure/vm#create). Hej `--nics` flaggan ansluter hello vNic toohello VM under hello distribution tooAzure. hello följande exempel skapas en virtuell dator med namnet `myVM` med Azure hanterade diskar och bifogar hello vNic med namnet `myNic` från hello föregående steg:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="detailed-walkthrough"></a>Detaljerad genomgång

En fullständig kontinuerlig integrering och kontinuerlig distribution (CiCd) infrastrukturen i Azure kräver vissa toobe statisk eller långlivade servrar. Du rekommenderas att Azure tillgångar som hello virtuella nätverk och Nätverkssäkerhetsgrupper är statiska och resurser som distribueras sällan kvar längre. När ett virtuellt nätverk har distribuerats, kan den återanvändas av nya distributioner utan någon negativ påverkar toohello infrastruktur. Du kan senare lägga till en Git-lagringsplatsen server eller en Jenkins automation-server ger CiCd toothis virtuellt nätverk för utvecklings- och testmiljöer.  

Interna DNS-namn är bara lösas i Azure-nätverk. Eftersom hello DNS-namn är internt, men de är inte matchas toohello utanför internet, kan ge ytterligare säkerhet toohello infrastruktur.

Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn inkluderar `myResourceGroup`, `myNic`, och `myVM`.

## <a name="create-hello-resource-group"></a>Skapa hello resursgrupp
Först skapar hello resursgrupp med [az gruppen skapa](/cli/azure/group#create). hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `westus` plats:

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-hello-virtual-network"></a>Skapa hello virtuellt nätverk

hello nästa steg är toobuild ett virtuellt nätverk toolaunch hello virtuella datorer i. hello virtuellt nätverk innehåller ett undernät för den här genomgången. Mer information om virtuella Azure-nätverk finns [skapa ett virtuellt nätverk med hjälp av hello Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Skapa hello virtuellt nätverk med [az network vnet skapa](/cli/azure/network/vnet#create). hello följande exempel skapas ett virtuellt nätverk med namnet `myVnet` och undernät med namnet `mySubnet`:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

## <a name="create-hello-network-security-group"></a>Skapa hello Nätverkssäkerhetsgrupp
Säkerhetsgrupper för Azure nätverket är likvärdiga tooa brandväggen på nätverksnivå hello. Läs mer om Nätverkssäkerhetsgrupper [hur toocreate NSG: er i hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Skapa hello nätverkssäkerhetsgrupp med [az nätverket nsg skapa](/cli/azure/network/nsg#create). hello följande exempel skapar en nätverkssäkerhetsgrupp med namnet `myNetworkSecurityGroup`:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-rule-tooallow-ssh"></a>Lägg till en regel för inkommande trafik tooallow SSH
Lägg till en inkommande regel för hello nätverkssäkerhetsgrupp med [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create). hello följande exempel skapas en regel med namnet `myRuleAllowSSH`:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myRuleAllowSSH \
    --protocol tcp \
    --direction inbound \
    --priority 1000 \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 22 \
    --access allow
```

## <a name="associate-hello-subnet-with-hello-network-security-group"></a>Associera hello undernät med hello Nätverkssäkerhetsgrupp
tooassociate hello undernät med hello säkerhetsgrupp för nätverk använder [az network vnet undernät uppdatering](/cli/azure/network/vnet/subnet#update). hello följande exempel associerar hello undernätsnamn `mySubnet` med hello säkerhetsgrupp för nätverk med namnet `myNetworkSecurityGroup`:

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```


## <a name="create-hello-virtual-network-interface-card-and-static-dns-names"></a>Skapa hello virtuella nätverkskort och statisk DNS-namn
Azure är flexibla, men toouse DNS-namn för VM-namnmatchning, behöver du toocreate virtuella nätverkskort (vNics) som innehåller en DNS-etikett. vNics är viktigt eftersom du kan återanvända dem genom att ansluta dem toodifferent virtuella datorer över hello infrastruktur livscykel. Den här metoden används för att hålla hello vNic som en statisk resurs hello virtuella datorer kan vara tillfälliga. Vi kan kan tooenable enkel namnmatchning från andra virtuella datorer i hello VNet med hjälp av DNS-etiketter på hello virtuellt nätverkskort. Med matchas namn kan andra virtuella datorer tooaccess hello automation-server med DNS-namn för hello `Jenkins` eller hello Git-server som `gitrepo`.  

Skapa hello vNic med [az nätverket nic skapa](/cli/azure/network/nic#create). hello följande exempel skapas ett virtuellt nätverkskort med namnet `myNic`, ansluter den toohello `myVnet` virtuellt nätverk med namnet `myVnet`, och skapar en intern DNS-namnpost kallas `jenkins`:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

## <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a>Distribuera hello VM till hello virtuell nätverksinfrastruktur
Nu har vi ett virtuellt nätverk och undernät, en Nätverkssäkerhetsgrupp som fungerar som en brandvägg tooprotect våra undernät genom att blockera all inkommande trafik utom port 22 för SSH- och ett virtuellt nätverkskort. Du kan nu distribuera en virtuell dator i den här befintliga nätverksinfrastruktur.

Skapa en virtuell dator med [az vm create](/cli/azure/vm#create). hello följande exempel skapas en virtuell dator med namnet `myVM` med Azure hanterade diskar och bifogar hello vNic med namnet `myNic` från hello föregående steg:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

Med hjälp av hello flaggor CLI toocall befintliga resurser vi ber Azure toodeploy hello VM i hello befintliga nätverk. tooreiterate, när en VNet och undernät har distribuerats, de kan förbli statisk eller permanenta resurser i Azure-region.  

## <a name="next-steps"></a>Nästa steg
* [Skapa en egen anpassad miljö för en virtuell Linux-dator med hjälp av Azure CLI-kommandon](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Skapa en Linux-VM på Azure med hjälp av mallar](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
