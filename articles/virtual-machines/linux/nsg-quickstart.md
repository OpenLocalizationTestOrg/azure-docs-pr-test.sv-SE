---
title: aaaOpen portarna tooa Linux VM med Azure CLI 2.0 | Microsoft Docs
description: "Lär dig hur tooopen en port / skapa en slutpunkt tooyour Linux VM använder hello Azure resource manager distribution modell och hello Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: eef9842b-495a-46cf-99a6-74e49807e74e
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: c79b31206e97558171609cf033bb3cb3370777c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="open-ports-and-endpoints-tooa-linux-vm-with-hello-azure-cli"></a>Öppna portar och slutpunkter tooa Linux VM med hello Azure CLI
Du öppnar en port eller skapa en slutpunkt tooa virtuell dator (VM) i Azure genom att skapa ett filter för nätverk på ett undernät eller Virtuella datorns nätverksgränssnitt. Du placera dessa filter som styr både inkommande och utgående trafik på en Nätverkssäkerhetsgrupp kopplad toohello resurs som tar emot hello trafik. Nu ska vi använda ett vanligt exempel på Internet-trafik på port 80. Den här artikeln beskrivs hur du tooopen port tooa VM med hello Azure CLI 2.0. Du kan också utföra dessa steg med hello [Azure CLI 1.0](nsg-quickstart-nodejs.md).


## <a name="quick-commands"></a>Snabbkommandon
toocreate en nätverkssäkerhet och regler du behöver hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).

Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn inkluderar *myResourceGroup*, *myNetworkSecurityGroup*, och *myVnet*.

Skapa hello nätverkssäkerhetsgrupp med [az nätverket nsg skapa](/cli/azure/network/nsg#create). hello följande exempel skapar en nätverkssäkerhetsgrupp med namnet *myNetworkSecurityGroup* i hello *eastus* plats:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

Lägga till en regel med [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create) tooallow HTTP trafik tooyour webbserver (eller justera för egna scenario, till exempel SSH åtkomst eller database connectivity). hello följande exempel skapas en regel med namnet *myNetworkSecurityGroupRule* tooallow TCP-trafik på port 80:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 80
```

Associera hello säkerhetsgrupp för nätverk med den Virtuella datorns nätverksgränssnitt (NIC) med [az nätverket nic uppdatering](/cli/azure/network/nic#update). hello följande exempel associerar en befintlig NIC som heter *myNic* med hello säkerhetsgrupp för nätverk med namnet *myNetworkSecurityGroup*:

```azurecli
az network nic update \
    --resource-group myResourceGroup \
    --name myNic \
    --network-security-group myNetworkSecurityGroup
```

Alternativt kan du associera säkerhetsgrupp för nätverk med ett undernät för virtuellt nätverk med [az network vnet undernät uppdatering](/cli/azure/network/vnet/subnet#update) i stället för bara toohello nätverksgränssnittet på en enda virtuell dator. hello följande exempel associerar ett befintligt undernät med namnet *mySubnet* i hello *myVnet* virtuellt nätverk med hello säkerhetsgrupp för nätverk med namnet *myNetworkSecurityGroup*:

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="more-information-on-network-security-groups"></a>Mer information om Nätverkssäkerhetsgrupper
hello här snabb kommandon kan du tooget upp och körs med trafiken flödar tooyour VM. Nätverkssäkerhetsgrupper ger många bra funktioner och granularitet för styra åtkomst tooyour resurser. Du kan läsa mer om [skapar en säkerhetsgrupp för nätverk och ACL-regler här](tutorial-virtual-network.md#secure-network-traffic).

Du bör placera dina virtuella datorer bakom en belastningsutjämnare i Azure för hög tillgänglighet webbprogram. hello belastningsutjämnare distribuerar trafik tooVMs med en Nätverkssäkerhetsgrupp som tillhandahåller trafikfiltrering. Mer information finns i [hur tooload saldo Linux virtuella datorer i Azure toocreate högtillgänglig programmet](tutorial-load-balancer.md).

## <a name="next-steps"></a>Nästa steg
I det här exemplet skapas en enkel regel tooallow HTTP-trafik. Du kan hitta information om hur du skapar mer detaljerad miljöer i hello följande artiklar:

* [Översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
* [Vad är en nätverkssäkerhetsgrupp (NSG)?](../../virtual-network/virtual-networks-nsg.md)
