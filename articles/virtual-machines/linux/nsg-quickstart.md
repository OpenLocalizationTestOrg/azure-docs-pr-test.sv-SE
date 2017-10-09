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
# <a name="open-ports-and-endpoints-tooa-linux-vm-with-hello-azure-cli"></a><span data-ttu-id="876d4-103">Öppna portar och slutpunkter tooa Linux VM med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="876d4-103">Open ports and endpoints tooa Linux VM with hello Azure CLI</span></span>
<span data-ttu-id="876d4-104">Du öppnar en port eller skapa en slutpunkt tooa virtuell dator (VM) i Azure genom att skapa ett filter för nätverk på ett undernät eller Virtuella datorns nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="876d4-104">You open a port, or create an endpoint, tooa virtual machine (VM) in Azure by creating a network filter on a subnet or VM network interface.</span></span> <span data-ttu-id="876d4-105">Du placera dessa filter som styr både inkommande och utgående trafik på en Nätverkssäkerhetsgrupp kopplad toohello resurs som tar emot hello trafik.</span><span class="sxs-lookup"><span data-stu-id="876d4-105">You place these filters, which control both inbound and outbound traffic, on a Network Security Group attached toohello resource that receives hello traffic.</span></span> <span data-ttu-id="876d4-106">Nu ska vi använda ett vanligt exempel på Internet-trafik på port 80.</span><span class="sxs-lookup"><span data-stu-id="876d4-106">Let's use a common example of web traffic on port 80.</span></span> <span data-ttu-id="876d4-107">Den här artikeln beskrivs hur du tooopen port tooa VM med hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="876d4-107">This article shows you how tooopen a port tooa VM with hello Azure CLI 2.0.</span></span> <span data-ttu-id="876d4-108">Du kan också utföra dessa steg med hello [Azure CLI 1.0](nsg-quickstart-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="876d4-108">You can also perform these steps with hello [Azure CLI 1.0](nsg-quickstart-nodejs.md).</span></span>


## <a name="quick-commands"></a><span data-ttu-id="876d4-109">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="876d4-109">Quick commands</span></span>
<span data-ttu-id="876d4-110">toocreate en nätverkssäkerhet och regler du behöver hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="876d4-110">toocreate a Network Security Group and rules you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="876d4-111">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="876d4-111">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="876d4-112">Exempel parameternamn inkluderar *myResourceGroup*, *myNetworkSecurityGroup*, och *myVnet*.</span><span class="sxs-lookup"><span data-stu-id="876d4-112">Example parameter names include *myResourceGroup*, *myNetworkSecurityGroup*, and *myVnet*.</span></span>

<span data-ttu-id="876d4-113">Skapa hello nätverkssäkerhetsgrupp med [az nätverket nsg skapa](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="876d4-113">Create hello network security group with [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="876d4-114">hello följande exempel skapar en nätverkssäkerhetsgrupp med namnet *myNetworkSecurityGroup* i hello *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="876d4-114">hello following example creates a network security group named *myNetworkSecurityGroup* in hello *eastus* location:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="876d4-115">Lägga till en regel med [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create) tooallow HTTP trafik tooyour webbserver (eller justera för egna scenario, till exempel SSH åtkomst eller database connectivity).</span><span class="sxs-lookup"><span data-stu-id="876d4-115">Add a rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create) tooallow HTTP traffic tooyour webserver (or adjust for your own scenario, such as SSH access or database connectivity).</span></span> <span data-ttu-id="876d4-116">hello följande exempel skapas en regel med namnet *myNetworkSecurityGroupRule* tooallow TCP-trafik på port 80:</span><span class="sxs-lookup"><span data-stu-id="876d4-116">hello following example creates a rule named *myNetworkSecurityGroupRule* tooallow TCP traffic on port 80:</span></span>

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 80
```

<span data-ttu-id="876d4-117">Associera hello säkerhetsgrupp för nätverk med den Virtuella datorns nätverksgränssnitt (NIC) med [az nätverket nic uppdatering](/cli/azure/network/nic#update).</span><span class="sxs-lookup"><span data-stu-id="876d4-117">Associate hello Network Security Group with your VM's network interface (NIC) with [az network nic update](/cli/azure/network/nic#update).</span></span> <span data-ttu-id="876d4-118">hello följande exempel associerar en befintlig NIC som heter *myNic* med hello säkerhetsgrupp för nätverk med namnet *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="876d4-118">hello following example associates an existing NIC named *myNic* with hello Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network nic update \
    --resource-group myResourceGroup \
    --name myNic \
    --network-security-group myNetworkSecurityGroup
```

<span data-ttu-id="876d4-119">Alternativt kan du associera säkerhetsgrupp för nätverk med ett undernät för virtuellt nätverk med [az network vnet undernät uppdatering](/cli/azure/network/vnet/subnet#update) i stället för bara toohello nätverksgränssnittet på en enda virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="876d4-119">Alternatively, you can associate your Network Security Group with a virtual network subnet with [az network vnet subnet update](/cli/azure/network/vnet/subnet#update) rather than just toohello network interface on a single VM.</span></span> <span data-ttu-id="876d4-120">hello följande exempel associerar ett befintligt undernät med namnet *mySubnet* i hello *myVnet* virtuellt nätverk med hello säkerhetsgrupp för nätverk med namnet *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="876d4-120">hello following example associates an existing subnet named *mySubnet* in hello *myVnet* virtual network with hello Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="more-information-on-network-security-groups"></a><span data-ttu-id="876d4-121">Mer information om Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="876d4-121">More information on Network Security Groups</span></span>
<span data-ttu-id="876d4-122">hello här snabb kommandon kan du tooget upp och körs med trafiken flödar tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="876d4-122">hello quick commands here allow you tooget up and running with traffic flowing tooyour VM.</span></span> <span data-ttu-id="876d4-123">Nätverkssäkerhetsgrupper ger många bra funktioner och granularitet för styra åtkomst tooyour resurser.</span><span class="sxs-lookup"><span data-stu-id="876d4-123">Network Security Groups provide many great features and granularity for controlling access tooyour resources.</span></span> <span data-ttu-id="876d4-124">Du kan läsa mer om [skapar en säkerhetsgrupp för nätverk och ACL-regler här](tutorial-virtual-network.md#secure-network-traffic).</span><span class="sxs-lookup"><span data-stu-id="876d4-124">You can read more about [creating a Network Security Group and ACL rules here](tutorial-virtual-network.md#secure-network-traffic).</span></span>

<span data-ttu-id="876d4-125">Du bör placera dina virtuella datorer bakom en belastningsutjämnare i Azure för hög tillgänglighet webbprogram.</span><span class="sxs-lookup"><span data-stu-id="876d4-125">For highly available web applications, you should place your VMs behind an Azure Load Balancer.</span></span> <span data-ttu-id="876d4-126">hello belastningsutjämnare distribuerar trafik tooVMs med en Nätverkssäkerhetsgrupp som tillhandahåller trafikfiltrering.</span><span class="sxs-lookup"><span data-stu-id="876d4-126">hello load balancer distributes traffic tooVMs, with a Network Security Group that provides traffic filtering.</span></span> <span data-ttu-id="876d4-127">Mer information finns i [hur tooload saldo Linux virtuella datorer i Azure toocreate högtillgänglig programmet](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="876d4-127">For more information, see [How tooload balance Linux virtual machines in Azure toocreate a highly available application](tutorial-load-balancer.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="876d4-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="876d4-128">Next steps</span></span>
<span data-ttu-id="876d4-129">I det här exemplet skapas en enkel regel tooallow HTTP-trafik.</span><span class="sxs-lookup"><span data-stu-id="876d4-129">In this example, you created a simple rule tooallow HTTP traffic.</span></span> <span data-ttu-id="876d4-130">Du kan hitta information om hur du skapar mer detaljerad miljöer i hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="876d4-130">You can find information on creating more detailed environments in hello following articles:</span></span>

* [<span data-ttu-id="876d4-131">Översikt över Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="876d4-131">Azure Resource Manager overview</span></span>](../../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="876d4-132">Vad är en nätverkssäkerhetsgrupp (NSG)?</span><span class="sxs-lookup"><span data-stu-id="876d4-132">What is a Network Security Group (NSG)?</span></span>](../../virtual-network/virtual-networks-nsg.md)
