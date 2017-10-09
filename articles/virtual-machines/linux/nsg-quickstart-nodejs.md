---
title: aaaOpen portarna tooa Linux VM med Azure CLI 1.0 | Microsoft Docs
description: "Lär dig hur tooopen en port / skapa en slutpunkt tooyour Linux VM använder hello Azure resource manager distribution modell och hello Azure CLI 1.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 337c37d151f527b43d4852291159b2f70a00accc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="opening-ports-and-endpoints-tooa-linux-vm-in-azure-using-hello-azure-cli-10"></a>Öppna portar och slutpunkter tooa Linux VM i Azure med hjälp av hello Azure CLI 1.0
Du öppnar en port eller skapa en slutpunkt tooa virtuell dator (VM) i Azure genom att skapa ett filter för nätverk på ett undernät eller Virtuella datorns nätverksgränssnitt. Du placera dessa filter som styr både inkommande och utgående trafik på en Nätverkssäkerhetsgrupp kopplad toohello resurs som tar emot hello trafik. Nu ska vi använda ett vanligt exempel på Internet-trafik på port 80. Den här artikeln visar hur tooopen som använder en port tooa VM hello Azure CLI 1.0.


## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet
Du kan göra hello med hjälp av något av följande versioner av CLI hello:

- [Azure CLI 1.0](#quick-commands) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)
- [Azure CLI 2.0](nsg-quickstart.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering


## <a name="quick-commands"></a>Snabbkommandon
toocreate en Nätverkssäkerhetsgrupp och regler du behöver [hello Azure CLI 1.0](../../cli-install-nodejs.md) installerad och med hjälp av Resource Manager-läge:

```azurecli
azure config mode arm
```

Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn ingår *myResourceGroup*, *myNetworkSecurityGroup*, och *myVnet*.

Skapa din Nätverkssäkerhetsgruppen att ange egna namn och plats på lämpligt sätt. hello följande exempel skapar en Nätverkssäkerhetsgrupp med namnet *myNetworkSecurityGroup* i hello *eastus* plats:

```azurecli
azure network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

Lägg till en regel tooallow HTTP-trafik tooyour webbserver (eller justera för egna scenario, till exempel SSH åtkomst eller database connectivity). hello följande exempel skapas en regel med namnet *myNetworkSecurityGroupRule* tooallow TCP-trafik på port 80:

```azurecli
azure network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --direction inbound \
    --priority 1000 \
    --destination-port-range 80 \
    --access allow
```

Associera hello säkerhetsgrupp för nätverk med den Virtuella datorns nätverksgränssnitt (NIC). hello följande exempel associerar en befintlig NIC som heter *myNic* med hello säkerhetsgrupp för nätverk med namnet *myNetworkSecurityGroup*:

```azurecli
azure network nic set \
    --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --name myNic
```

Du kan också associera säkerhetsgrupp för nätverk med ett undernät för virtuellt nätverk i stället för bara toohello nätverksgränssnittet på en enda virtuell dator. hello följande exempel associerar ett befintligt undernät med namnet *mySubnet* i hello *myVnet* virtuellt nätverk med hello säkerhetsgrupp för nätverk med namnet *myNetworkSecurityGroup*:

```azurecli
azure network vnet subnet set \
    --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --vnet-name myVnet --name mySubnet
```

## <a name="more-information-on-network-security-groups"></a>Mer information om Nätverkssäkerhetsgrupper
hello här snabb kommandon kan du tooget upp och körs med trafiken flödar tooyour VM. Nätverkssäkerhetsgrupper ger många bra funktioner och granularitet för styra åtkomst tooyour resurser. Du kan läsa mer om [skapar en säkerhetsgrupp för nätverk och ACL-regler här](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).

Du kan definiera Nätverkssäkerhetsgrupper och ACL-regler som en del av Azure Resource Manager-mallar. Läs mer om [skapa Nätverkssäkerhetsgrupper med mallar](../../virtual-network/virtual-networks-create-nsg-arm-template.md).

Om du behöver toouse vidarebefordrade portar toomap en unik externa porten tooan Intern port på den virtuella datorn Använd belastningsutjämning och NAT (Network Address Translation)-regler. Du kan till exempel vill tooexpose TCP-port 8080 externt och har trafik dirigeras tooTCP port 80 på en virtuell dator. Du kan lära dig om [skapar en Internetriktade belastningsutjämnare](../../load-balancer/load-balancer-get-started-internet-arm-cli.md).

## <a name="next-steps"></a>Nästa steg
I det här exemplet skapas en enkel regel tooallow HTTP-trafik. Du kan hitta information om hur du skapar mer detaljerad miljöer i hello följande artiklar:

* [Översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
* [Vad är en nätverkssäkerhetsgrupp (NSG)?](../../virtual-network/virtual-networks-nsg.md)
* [Översikt av Azure Resource Manager för belastningsutjämnare](../../load-balancer/load-balancer-arm.md)

