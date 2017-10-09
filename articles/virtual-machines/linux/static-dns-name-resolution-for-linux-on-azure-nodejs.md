---
title: "aaaUsing interna DNS för VM-namnmatchning i Azure | Microsoft Docs"
description: "Med hjälp av interna DNS för namnmatchning för virtuell dator på Azure."
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
ms.devlang: na
ms.topic: article
ms.date: 12/05/2016
ms.author: v-livech
ms.openlocfilehash: 94fd6577aa51ce5db4dc26649b415ddeeb410eb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-internal-dns-for-vm-name-resolution-on-azure"></a>Med hjälp av interna DNS för namnmatchning för virtuell dator på Azure

Den här artikeln visar hur tooset statiska interna DNS-namn för Linux virtuella datorer med hjälp av virtuella nätverkskort (tillval) (VNic) och DNS-etikettnamn. Statisk DNS-namn används för permanenta infrastrukturtjänster som en Jenkins build-server, som används för det här dokumentet eller en Git-server.

hello kraven är:

* [ett Azure-konto](https://azure.microsoft.com/pricing/free-trial/)
* [offentliga och privata SSH-nyckelfiler](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet
Du kan göra hello med hjälp av något av följande versioner av CLI hello:

- [Azure CLI 1.0](#quick-commands) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)
- [Azure CLI 2.0](static-dns-name-resolution-for-linux-on-azure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering


## <a name="quick-commands"></a>Snabbkommandon

Om du behöver tooquickly utföra hello uppgiften, hello efter avsnittet information hello-kommandon som krävs. Mer detaljerad information och kontext för varje steg finns hello resten av dokumentet hello [startar här](#detailed-walkthrough).  

Krav: Resursgrupp, VNet, NSG med SSH inkommande, undernät.

### <a name="create-a-vnic-with-a-static-internal-dns-name"></a>Skapa ett virtuellt nätverkskort med ett statiskt interna DNS-namn

Hej `-r` cli-flaggan är för inställningen hello DNS-etikett, vilket ger hello statisk DNS-namn för hello VNic.

```azurecli
azure network nic create jenkinsVNic \
-g myResourceGroup \
-l westus \
-m myVNet \
-k mySubNet \
-r jenkins
```

### <a name="deploy-hello-vm-into-hello-vnet-nsg-and-connect-hello-vnic"></a>Distribuera hello VM till hello VNet, NSG och ansluta hello VNic

Hej `-N` ansluter hello VNic toohello ny virtuell dator under hello distribution tooAzure.

```azurecli
azure vm create jenkins \
-g myResourceGroup \
-l westus \
-y linux \
-Q Debian \
-o myStorageAcct \
-u myAdminUser \
-M ~/.ssh/id_rsa.pub \
-F myVNet \
-j mySubnet \
-N jenkinsVNic
```

## <a name="detailed-walkthrough"></a>Detaljerad genomgång

En fullständig kontinuerlig integrering och kontinuerlig distribution (CiCd) infrastrukturen i Azure kräver vissa toobe statisk eller långlivade servrar.  Du rekommenderas att Azure tillgångar som hello virtuella nätverk (Vnet) och Nätverkssäkerhetsgrupper (NSG: er) måste vara statiska och resurser som distribueras sällan kvar längre.  När ett virtuellt nätverk har distribuerats, kan den återanvändas av nya distributioner utan någon negativ påverkar toohello infrastruktur.  Lägga till toothis statiska nätverket en Git ger databasen server och en Jenkins automationsserver CiCd tooyour utvecklings- och testmiljöer.  

Interna DNS-namn är bara lösas i Azure-nätverk.  Eftersom hello DNS-namn är internt, men de är inte matchas toohello utanför internet, kan ge ytterligare säkerhet toohello infrastruktur.

_Ersätt alla exempel med egna namn._

## <a name="create-hello-resource-group"></a>Skapa hello resursgrupp

En resursgrupp är nödvändiga tooorganize allt vi skapa i den här genomgången.  Läs mer om Azure-resursgrupper [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```azurecli
azure group create myResourceGroup \
--location westus
```

## <a name="create-hello-vnet"></a>Skapa hello VNet

hello första steget är toobuild ett VNet toolaunch hello virtuella datorer i.  Hej VNet innehåller ett undernät för den här genomgången.  Mer information om virtuella Azure-nätverk finns [skapa ett virtuellt nätverk med hjälp av hello Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```azurecli
azure network vnet create myVNet \
--resource-group myResourceGroup \
--address-prefixes 10.10.0.0/24 \
--location westus
```

## <a name="create-hello-nsg"></a>Skapa hello NSG

hello undernät bygger bakom en befintlig säkerhetsgrupp för nätverk så att vi bygga hello NSG innan hello undernät.  Azure NSG: er är likvärdiga tooa brandväggen på hello nätverksnivån.  Mer information om Azure NSG: er finns [hur toocreate NSG: er i hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```azurecli
azure network nsg create myNSG \
--resource-group myResourceGroup \
--location westus
```

## <a name="add-an-inbound-ssh-allow-rule"></a>Lägg till regel för att tillåta en inkommande SSH

hello Linux VM behöver åtkomst från hello internet så att en regel som tillåter inkommande port 22 trafik toobe har passerat hello nätverket tooport 22 på hello Linux VM krävs.

```azurecli
azure network nsg rule create inboundSSH \
--resource-group myResourceGroup \
--nsg-name myNSG \
--access Allow \
--protocol Tcp \
--direction Inbound \
--priority 100 \
--source-address-prefix * \
--source-port-range * \
--destination-address-prefix 10.10.0.0/24 \
--destination-port-range 22
```

## <a name="add-a-subnet-toohello-vnet"></a>Lägg till ett undernät toohello VNet

Virtuella datorer i hello VNet måste finnas i ett undernät.  Varje virtuellt nätverk kan ha flera undernät.  Skapa hello undernät och associerar hello undernät med hello NSG tooadd ett brandväggen toohello undernät.

```azurecli
azure network vnet subnet create mySubNet \
--resource-group myResourceGroup \
--vnet-name myVNet \
--address-prefix 10.10.0.0/26 \
--network-security-group-name myNSG
```

hello undernät är nu läggs till i hello virtuella nätverk och som är associerade med hello NSG och hello NSG regel.

## <a name="creating-static-dns-names"></a>Skapa statiska DNS-namn

Azure är flexibla, men toouse DNS-namn för namnmatchning för virtuella datorer, måste toocreate dem som virtuella nätverkskort (VNics) med hjälp av DNS-etiketter.  VNics är viktigt eftersom du kan återanvända dem genom att ansluta dem toodifferent virtuella datorer, vilket håller hello VNic som en statisk resurs hello virtuella datorer kan vara tillfälliga.  Med hjälp av DNS-etiketter på hello VNic är vi kan tooenable enkel namnmatchning från andra virtuella datorer i hello virtuella nätverk.  Med matchas namn kan andra virtuella datorer tooaccess hello automation-server med DNS-namn för hello `Jenkins` eller hello Git-server som `gitrepo`.  Skapa ett virtuellt nätverkskort och koppla den till hello undernät som skapats i hello föregående steg.

```azurecli
azure network nic create jenkinsVNic \
-g myResourceGroup \
-l westus \
-m myVNet \
-k mySubNet \
-r jenkins
```

## <a name="deploy-hello-vm-into-hello-vnet-and-nsg"></a>Distribuera hello VM till hello VNet och NSG

Nu har vi ett VNet, ett undernät i det virtuella nätverket och en NSG som fungerar som en brandvägg tooprotect våra undernät genom att blockera all inkommande trafik utom port 22 för SSH.  hello VM kan nu distribueras i det här befintliga nätverksinfrastruktur.

Med hjälp av hello Azure CLI och hello `azure vm create` kommandot hello Linux VM är distribuerade toohello befintliga Azure-resursgrupp, virtuella nätverk, undernät och virtuellt nätverkskort.  Mer information om hur du använder hello CLI toodeploy fullständig VM finns [skapa en fullständig Linux-miljö med hjälp av hello Azure CLI](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```azurecli
azure vm create jenkins \
--resource-group myResourceGroup myVM \
--location westus \
--os-type linux \
--image-urn Debian \
--storage-account-name mystorageaccount \
--admin-username myAdminUser \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--vnet-name myVNet \
--vnet-subnet-name mySubnet \
--nic-name jenkinsVNic
```

Med hjälp av hello flaggor CLI toocall befintliga resurser vi ber Azure toodeploy hello VM i hello befintliga nätverk.  tooreiterate, när en VNet och undernät har distribuerats, de kan förbli statisk eller permanenta resurser i Azure-region.  

## <a name="next-steps"></a>Nästa steg
* [Skapa en egen anpassad miljö för en virtuell Linux-dator med hjälp av Azure CLI-kommandon](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Skapa en Linux-VM på Azure med hjälp av mallar](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
