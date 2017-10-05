---
title: "Öppna portar till en Linux-VM med Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur du öppnar en port / skapa en slutpunkt för ditt Linux-VM med hjälp av Azure resource manager-distributionsmodellen och Azure CLI 1.0"
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
ms.openlocfilehash: 847bc76c37ed929851712ba1c12463a01032e267
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="opening-ports-and-endpoints-to-a-linux-vm-in-azure-using-the-azure-cli-10"></a><span data-ttu-id="755dc-103">Öppna portar och slutpunkter till en Linux-VM i Azure med hjälp av Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="755dc-103">Opening ports and endpoints to a Linux VM in Azure using the Azure CLI 1.0</span></span>
<span data-ttu-id="755dc-104">Du öppnar en port eller skapa en slutpunkt för en virtuell dator (VM) i Azure genom att skapa ett filter för nätverk på ett undernät eller Virtuella datorns nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="755dc-104">You open a port, or create an endpoint, to a virtual machine (VM) in Azure by creating a network filter on a subnet or VM network interface.</span></span> <span data-ttu-id="755dc-105">Du kan placera dessa filter som styr både inkommande och utgående trafik på en Nätverkssäkerhetsgrupp kopplad till den resurs som tar emot trafiken.</span><span class="sxs-lookup"><span data-stu-id="755dc-105">You place these filters, which control both inbound and outbound traffic, on a Network Security Group attached to the resource that receives the traffic.</span></span> <span data-ttu-id="755dc-106">Nu ska vi använda ett vanligt exempel på Internet-trafik på port 80.</span><span class="sxs-lookup"><span data-stu-id="755dc-106">Let's use a common example of web traffic on port 80.</span></span> <span data-ttu-id="755dc-107">Den här artikeln visar hur du öppnar en port till en virtuell dator med hjälp av Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="755dc-107">This article shows you how to open a port to a VM using the Azure CLI 1.0.</span></span>


## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="755dc-108">CLI-versioner för att slutföra uppgiften</span><span class="sxs-lookup"><span data-stu-id="755dc-108">CLI versions to complete the task</span></span>
<span data-ttu-id="755dc-109">Du kan slutföra uppgiften med någon av följande CLI-versioner:</span><span class="sxs-lookup"><span data-stu-id="755dc-109">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="755dc-110">[Azure CLI 1.0](#quick-commands) – våra CLI för klassisk och resurs management på distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="755dc-110">[Azure CLI 1.0](#quick-commands) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="755dc-111">[Azure CLI 2.0](nsg-quickstart.md) – vår nästa generations CLI för distributionsmodellen resurshantering</span><span class="sxs-lookup"><span data-stu-id="755dc-111">[Azure CLI 2.0](nsg-quickstart.md) - our next generation CLI for the resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="755dc-112">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="755dc-112">Quick commands</span></span>
<span data-ttu-id="755dc-113">Så här skapar du en säkerhetsgrupp för nätverk och regler som du behöver [Azure CLI 1.0](../../cli-install-nodejs.md) installerad och med hjälp av Resource Manager-läge:</span><span class="sxs-lookup"><span data-stu-id="755dc-113">To create a Network Security Group and rules you need [the Azure CLI 1.0](../../cli-install-nodejs.md) installed and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="755dc-114">Ersätt exempel parameternamn med egna värden i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="755dc-114">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="755dc-115">Exempel parameternamn ingår *myResourceGroup*, *myNetworkSecurityGroup*, och *myVnet*.</span><span class="sxs-lookup"><span data-stu-id="755dc-115">Example parameter names included *myResourceGroup*, *myNetworkSecurityGroup*, and *myVnet*.</span></span>

<span data-ttu-id="755dc-116">Skapa din Nätverkssäkerhetsgruppen att ange egna namn och plats på lämpligt sätt.</span><span class="sxs-lookup"><span data-stu-id="755dc-116">Create your Network Security Group, entering your own names and location appropriately.</span></span> <span data-ttu-id="755dc-117">I följande exempel skapas en Nätverkssäkerhetsgrupp med namnet *myNetworkSecurityGroup* i den *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="755dc-117">The following example creates a Network Security Group named *myNetworkSecurityGroup* in the *eastus* location:</span></span>

```azurecli
azure network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="755dc-118">Lägg till en regel för att tillåta HTTP-trafik till en webbserver (eller justera för egna scenario, till exempel SSH åtkomst eller database connectivity).</span><span class="sxs-lookup"><span data-stu-id="755dc-118">Add a rule to allow HTTP traffic to your webserver (or adjust for your own scenario, such as SSH access or database connectivity).</span></span> <span data-ttu-id="755dc-119">I följande exempel skapas en regel med namnet *myNetworkSecurityGroupRule* som tillåter TCP-trafik på port 80:</span><span class="sxs-lookup"><span data-stu-id="755dc-119">The following example creates a rule named *myNetworkSecurityGroupRule* to allow TCP traffic on port 80:</span></span>

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

<span data-ttu-id="755dc-120">Koppla Nätverkssäkerhetsgruppen till den Virtuella datorns nätverksgränssnitt (NIC).</span><span class="sxs-lookup"><span data-stu-id="755dc-120">Associate the Network Security Group with your VM's network interface (NIC).</span></span> <span data-ttu-id="755dc-121">I följande exempel associerar en befintlig NIC som heter *myNic* med Nätverkssäkerhetsgruppen med namnet *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="755dc-121">The following example associates an existing NIC named *myNic* with the Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
azure network nic set \
    --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --name myNic
```

<span data-ttu-id="755dc-122">Du kan också associera säkerhetsgrupp för nätverk med ett undernät för virtuellt nätverk i stället för bara till nätverksgränssnittet på en enda virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="755dc-122">Alternatively, you can associate your Network Security Group with a virtual network subnet rather than just to the network interface on a single VM.</span></span> <span data-ttu-id="755dc-123">I följande exempel associerar ett befintligt undernät med namnet *mySubnet* i den *myVnet* virtuellt nätverk med säkerhetsgruppen nätverk med namnet *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="755dc-123">The following example associates an existing subnet named *mySubnet* in the *myVnet* virtual network with the Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
azure network vnet subnet set \
    --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --vnet-name myVnet --name mySubnet
```

## <a name="more-information-on-network-security-groups"></a><span data-ttu-id="755dc-124">Mer information om Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="755dc-124">More information on Network Security Groups</span></span>
<span data-ttu-id="755dc-125">Snabb kommandon här kan du komma igång med trafik som flödar till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="755dc-125">The quick commands here allow you to get up and running with traffic flowing to your VM.</span></span> <span data-ttu-id="755dc-126">Nätverkssäkerhetsgrupper ger många bra funktioner och granularitet för att styra åtkomst till resurser.</span><span class="sxs-lookup"><span data-stu-id="755dc-126">Network Security Groups provide many great features and granularity for controlling access to your resources.</span></span> <span data-ttu-id="755dc-127">Du kan läsa mer om [skapar en säkerhetsgrupp för nätverk och ACL-regler här](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="755dc-127">You can read more about [creating a Network Security Group and ACL rules here](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).</span></span>

<span data-ttu-id="755dc-128">Du kan definiera Nätverkssäkerhetsgrupper och ACL-regler som en del av Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="755dc-128">You can define Network Security Groups and ACL rules as part of Azure Resource Manager templates.</span></span> <span data-ttu-id="755dc-129">Läs mer om [skapa Nätverkssäkerhetsgrupper med mallar](../../virtual-network/virtual-networks-create-nsg-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="755dc-129">Read more about [creating Network Security Groups with templates](../../virtual-network/virtual-networks-create-nsg-arm-template.md).</span></span>

<span data-ttu-id="755dc-130">Om du behöver använda port-vidarebefordran för att mappa en unik extern port till en intern port på den virtuella datorn Använd belastningsutjämning och NAT (Network Address Translation)-regler.</span><span class="sxs-lookup"><span data-stu-id="755dc-130">If you need to use port-forwarding to map a unique external port to an internal port on your VM, use a load balancer and Network Address Translation (NAT) rules.</span></span> <span data-ttu-id="755dc-131">Du kanske vill exponera TCP-port 8080 externt och har trafik dirigeras till TCP-port 80 på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="755dc-131">For example, you may want to expose TCP port 8080 externally and have traffic directed to TCP port 80 on a VM.</span></span> <span data-ttu-id="755dc-132">Du kan lära dig om [skapar en Internetriktade belastningsutjämnare](../../load-balancer/load-balancer-get-started-internet-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="755dc-132">You can learn about [creating an Internet-facing load balancer](../../load-balancer/load-balancer-get-started-internet-arm-cli.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="755dc-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="755dc-133">Next steps</span></span>
<span data-ttu-id="755dc-134">I det här exemplet skapas en enkel regel för att tillåta HTTP-trafik.</span><span class="sxs-lookup"><span data-stu-id="755dc-134">In this example, you created a simple rule to allow HTTP traffic.</span></span> <span data-ttu-id="755dc-135">Du kan hitta information om hur du skapar mer detaljerad miljöer i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="755dc-135">You can find information on creating more detailed environments in the following articles:</span></span>

* [<span data-ttu-id="755dc-136">Översikt över Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="755dc-136">Azure Resource Manager overview</span></span>](../../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="755dc-137">Vad är en nätverkssäkerhetsgrupp (NSG)?</span><span class="sxs-lookup"><span data-stu-id="755dc-137">What is a Network Security Group (NSG)?</span></span>](../../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="755dc-138">Översikt av Azure Resource Manager för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="755dc-138">Azure Resource Manager Overview for Load Balancers</span></span>](../../load-balancer/load-balancer-arm.md)

