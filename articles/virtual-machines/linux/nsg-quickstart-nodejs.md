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
# <a name="opening-ports-and-endpoints-tooa-linux-vm-in-azure-using-hello-azure-cli-10"></a><span data-ttu-id="2238d-103">Öppna portar och slutpunkter tooa Linux VM i Azure med hjälp av hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="2238d-103">Opening ports and endpoints tooa Linux VM in Azure using hello Azure CLI 1.0</span></span>
<span data-ttu-id="2238d-104">Du öppnar en port eller skapa en slutpunkt tooa virtuell dator (VM) i Azure genom att skapa ett filter för nätverk på ett undernät eller Virtuella datorns nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="2238d-104">You open a port, or create an endpoint, tooa virtual machine (VM) in Azure by creating a network filter on a subnet or VM network interface.</span></span> <span data-ttu-id="2238d-105">Du placera dessa filter som styr både inkommande och utgående trafik på en Nätverkssäkerhetsgrupp kopplad toohello resurs som tar emot hello trafik.</span><span class="sxs-lookup"><span data-stu-id="2238d-105">You place these filters, which control both inbound and outbound traffic, on a Network Security Group attached toohello resource that receives hello traffic.</span></span> <span data-ttu-id="2238d-106">Nu ska vi använda ett vanligt exempel på Internet-trafik på port 80.</span><span class="sxs-lookup"><span data-stu-id="2238d-106">Let's use a common example of web traffic on port 80.</span></span> <span data-ttu-id="2238d-107">Den här artikeln visar hur tooopen som använder en port tooa VM hello Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="2238d-107">This article shows you how tooopen a port tooa VM using hello Azure CLI 1.0.</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="2238d-108">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="2238d-108">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="2238d-109">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="2238d-109">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="2238d-110">[Azure CLI 1.0](#quick-commands) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="2238d-110">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="2238d-111">[Azure CLI 2.0](nsg-quickstart.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="2238d-111">[Azure CLI 2.0](nsg-quickstart.md) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="2238d-112">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="2238d-112">Quick commands</span></span>
<span data-ttu-id="2238d-113">toocreate en Nätverkssäkerhetsgrupp och regler du behöver [hello Azure CLI 1.0](../../cli-install-nodejs.md) installerad och med hjälp av Resource Manager-läge:</span><span class="sxs-lookup"><span data-stu-id="2238d-113">toocreate a Network Security Group and rules you need [hello Azure CLI 1.0](../../cli-install-nodejs.md) installed and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="2238d-114">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="2238d-114">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="2238d-115">Exempel parameternamn ingår *myResourceGroup*, *myNetworkSecurityGroup*, och *myVnet*.</span><span class="sxs-lookup"><span data-stu-id="2238d-115">Example parameter names included *myResourceGroup*, *myNetworkSecurityGroup*, and *myVnet*.</span></span>

<span data-ttu-id="2238d-116">Skapa din Nätverkssäkerhetsgruppen att ange egna namn och plats på lämpligt sätt.</span><span class="sxs-lookup"><span data-stu-id="2238d-116">Create your Network Security Group, entering your own names and location appropriately.</span></span> <span data-ttu-id="2238d-117">hello följande exempel skapar en Nätverkssäkerhetsgrupp med namnet *myNetworkSecurityGroup* i hello *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="2238d-117">hello following example creates a Network Security Group named *myNetworkSecurityGroup* in hello *eastus* location:</span></span>

```azurecli
azure network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="2238d-118">Lägg till en regel tooallow HTTP-trafik tooyour webbserver (eller justera för egna scenario, till exempel SSH åtkomst eller database connectivity).</span><span class="sxs-lookup"><span data-stu-id="2238d-118">Add a rule tooallow HTTP traffic tooyour webserver (or adjust for your own scenario, such as SSH access or database connectivity).</span></span> <span data-ttu-id="2238d-119">hello följande exempel skapas en regel med namnet *myNetworkSecurityGroupRule* tooallow TCP-trafik på port 80:</span><span class="sxs-lookup"><span data-stu-id="2238d-119">hello following example creates a rule named *myNetworkSecurityGroupRule* tooallow TCP traffic on port 80:</span></span>

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

<span data-ttu-id="2238d-120">Associera hello säkerhetsgrupp för nätverk med den Virtuella datorns nätverksgränssnitt (NIC).</span><span class="sxs-lookup"><span data-stu-id="2238d-120">Associate hello Network Security Group with your VM's network interface (NIC).</span></span> <span data-ttu-id="2238d-121">hello följande exempel associerar en befintlig NIC som heter *myNic* med hello säkerhetsgrupp för nätverk med namnet *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="2238d-121">hello following example associates an existing NIC named *myNic* with hello Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
azure network nic set \
    --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --name myNic
```

<span data-ttu-id="2238d-122">Du kan också associera säkerhetsgrupp för nätverk med ett undernät för virtuellt nätverk i stället för bara toohello nätverksgränssnittet på en enda virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2238d-122">Alternatively, you can associate your Network Security Group with a virtual network subnet rather than just toohello network interface on a single VM.</span></span> <span data-ttu-id="2238d-123">hello följande exempel associerar ett befintligt undernät med namnet *mySubnet* i hello *myVnet* virtuellt nätverk med hello säkerhetsgrupp för nätverk med namnet *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="2238d-123">hello following example associates an existing subnet named *mySubnet* in hello *myVnet* virtual network with hello Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
azure network vnet subnet set \
    --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --vnet-name myVnet --name mySubnet
```

## <a name="more-information-on-network-security-groups"></a><span data-ttu-id="2238d-124">Mer information om Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="2238d-124">More information on Network Security Groups</span></span>
<span data-ttu-id="2238d-125">hello här snabb kommandon kan du tooget upp och körs med trafiken flödar tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="2238d-125">hello quick commands here allow you tooget up and running with traffic flowing tooyour VM.</span></span> <span data-ttu-id="2238d-126">Nätverkssäkerhetsgrupper ger många bra funktioner och granularitet för styra åtkomst tooyour resurser.</span><span class="sxs-lookup"><span data-stu-id="2238d-126">Network Security Groups provide many great features and granularity for controlling access tooyour resources.</span></span> <span data-ttu-id="2238d-127">Du kan läsa mer om [skapar en säkerhetsgrupp för nätverk och ACL-regler här](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="2238d-127">You can read more about [creating a Network Security Group and ACL rules here](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).</span></span>

<span data-ttu-id="2238d-128">Du kan definiera Nätverkssäkerhetsgrupper och ACL-regler som en del av Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="2238d-128">You can define Network Security Groups and ACL rules as part of Azure Resource Manager templates.</span></span> <span data-ttu-id="2238d-129">Läs mer om [skapa Nätverkssäkerhetsgrupper med mallar](../../virtual-network/virtual-networks-create-nsg-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="2238d-129">Read more about [creating Network Security Groups with templates](../../virtual-network/virtual-networks-create-nsg-arm-template.md).</span></span>

<span data-ttu-id="2238d-130">Om du behöver toouse vidarebefordrade portar toomap en unik externa porten tooan Intern port på den virtuella datorn Använd belastningsutjämning och NAT (Network Address Translation)-regler.</span><span class="sxs-lookup"><span data-stu-id="2238d-130">If you need toouse port-forwarding toomap a unique external port tooan internal port on your VM, use a load balancer and Network Address Translation (NAT) rules.</span></span> <span data-ttu-id="2238d-131">Du kan till exempel vill tooexpose TCP-port 8080 externt och har trafik dirigeras tooTCP port 80 på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2238d-131">For example, you may want tooexpose TCP port 8080 externally and have traffic directed tooTCP port 80 on a VM.</span></span> <span data-ttu-id="2238d-132">Du kan lära dig om [skapar en Internetriktade belastningsutjämnare](../../load-balancer/load-balancer-get-started-internet-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="2238d-132">You can learn about [creating an Internet-facing load balancer](../../load-balancer/load-balancer-get-started-internet-arm-cli.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2238d-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2238d-133">Next steps</span></span>
<span data-ttu-id="2238d-134">I det här exemplet skapas en enkel regel tooallow HTTP-trafik.</span><span class="sxs-lookup"><span data-stu-id="2238d-134">In this example, you created a simple rule tooallow HTTP traffic.</span></span> <span data-ttu-id="2238d-135">Du kan hitta information om hur du skapar mer detaljerad miljöer i hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="2238d-135">You can find information on creating more detailed environments in hello following articles:</span></span>

* [<span data-ttu-id="2238d-136">Översikt över Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="2238d-136">Azure Resource Manager overview</span></span>](../../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="2238d-137">Vad är en nätverkssäkerhetsgrupp (NSG)?</span><span class="sxs-lookup"><span data-stu-id="2238d-137">What is a Network Security Group (NSG)?</span></span>](../../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="2238d-138">Översikt av Azure Resource Manager för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="2238d-138">Azure Resource Manager Overview for Load Balancers</span></span>](../../load-balancer/load-balancer-arm.md)

