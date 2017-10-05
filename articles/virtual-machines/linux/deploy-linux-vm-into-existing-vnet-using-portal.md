---
title: "Distribuera virtuella Linux-datorer i befintliga nätverk med Azure-portalen | Microsoft Docs"
description: "Distribuera en Linux-VM till ett befintligt virtuellt Azure-nätverk som använder portalen."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 964c0fc41773b50a9fbe476df47460484c2ada66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-deploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-the-azure-portal"></a><span data-ttu-id="abe3a-103">Hur du distribuerar en virtuell Linux-dator i ett befintligt virtuellt nätverk med Azure med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="abe3a-103">How to deploy a Linux virtual machine into an existing Azure Virtual Network with the Azure portal</span></span>

<span data-ttu-id="abe3a-104">Den här artikeln visar hur du distribuerar en virtuell dator (VM) till ett befintligt virtuellt nätverk (VNet).</span><span class="sxs-lookup"><span data-stu-id="abe3a-104">This article shows you how to deploy a virtual machine (VM) into an existing virtual network (VNet).</span></span> <span data-ttu-id="abe3a-105">Azure tillgångar som Vnet och nätverket säkerhetsgrupper bör vara statiska och resurser som distribueras sällan kvar längre.</span><span class="sxs-lookup"><span data-stu-id="abe3a-105">Azure assets like VNets and network security groups should be static and long lived resources that are rarely deployed.</span></span> <span data-ttu-id="abe3a-106">När ett virtuellt nätverk har distribuerats, kan den återanvändas av konstant redeployments utan någon negativ påverkar i infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="abe3a-106">Once a VNet has been deployed, it can be reused by constant redeployments without any adverse affects to the infrastructure.</span></span> <span data-ttu-id="abe3a-107">Om du tänker ett VNet som en traditionell maskinvara nätverks-switch - du inte behöver konfigurera en helt ny maskinvara växel med varje distribution.</span><span class="sxs-lookup"><span data-stu-id="abe3a-107">Thinking about a VNet as being a traditional hardware network switch - you would not need to configure a brand new hardware switch with each deployment.</span></span>  

<span data-ttu-id="abe3a-108">Du kan fortsätta att distribuera nya servrar i det virtuella nätverket flera gånger med några eventuella ändringar som krävs under hela VNet med en korrekt konfigurerad VNet.</span><span class="sxs-lookup"><span data-stu-id="abe3a-108">With a correctly configured VNet, you can continue to deploy new servers into that VNet over and over with few, if any, changes required over the life of the VNet.</span></span>

## <a name="create-the-resource-group"></a><span data-ttu-id="abe3a-109">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="abe3a-109">Create the resource group</span></span>

<span data-ttu-id="abe3a-110">Först skapa en resursgrupp för att organisera allt som du skapar i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="abe3a-110">First, create a resource group to organize everything you create in this walkthrough.</span></span> <span data-ttu-id="abe3a-111">Läs mer om Azure-resursgrupper [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)</span><span class="sxs-lookup"><span data-stu-id="abe3a-111">For more information about Azure resource groups, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md)</span></span>

![createResourceGroup](./media/deploy-linux-vm-into-existing-vnet-using-portal/createResourceGroup.png)


## <a name="create-the-vnet"></a><span data-ttu-id="abe3a-113">Skapa VNet</span><span class="sxs-lookup"><span data-stu-id="abe3a-113">Create the VNet</span></span>

<span data-ttu-id="abe3a-114">Sedan skapa ett virtuellt nätverk för att starta de virtuella datorerna till.</span><span class="sxs-lookup"><span data-stu-id="abe3a-114">Next, build a VNet to launch the VMs into.</span></span> <span data-ttu-id="abe3a-115">Det virtuella nätverket innehåller ett undernät och är kopplat till nätverkssäkerhetsgruppen med undernätet i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="abe3a-115">The VNet contains one subnet and is associated with the network security group with this subnet in a later step.</span></span>

![createVNet](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVNet.png)

## <a name="add-a-vnic-to-the-subnet"></a><span data-ttu-id="abe3a-117">Lägg till ett virtuellt nätverkskort i undernätet</span><span class="sxs-lookup"><span data-stu-id="abe3a-117">Add a VNic to the subnet</span></span>

<span data-ttu-id="abe3a-118">Virtuella nätverkskort (VNics) är viktigt när du ansluter dem till olika virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="abe3a-118">Virtual network cards (VNics) are important as you can connect them to different VMs.</span></span> <span data-ttu-id="abe3a-119">Den här metoden används för att hålla VNic som en statisk resurs medan de virtuella datorerna kan vara tillfälligt.</span><span class="sxs-lookup"><span data-stu-id="abe3a-119">This approach keeps the VNic as a static resource while the VMs can be temporary.</span></span> <span data-ttu-id="abe3a-120">Skapa ett virtuellt nätverkskort och associera det med undernät som skapats i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="abe3a-120">Create a VNic and associate it with the subnet created in the previous step.</span></span>

![createVNic](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVNic.png)

## <a name="create-the-network-security-group"></a><span data-ttu-id="abe3a-122">Skapa nätverkssäkerhetsgruppen</span><span class="sxs-lookup"><span data-stu-id="abe3a-122">Create the network security group</span></span>

<span data-ttu-id="abe3a-123">Säkerhetsgrupper för Azure-nätverket är likvärdiga med en brandvägg på nätverksnivå.</span><span class="sxs-lookup"><span data-stu-id="abe3a-123">Azure network security groups are equivalent to a firewall at the network layer.</span></span> <span data-ttu-id="abe3a-124">Läs mer på Azure nätverkssäkerhetsgrupper [vad är en Nätverkssäkerhetsgrupp](../../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="abe3a-124">For more information on Azure network security groups, see [What is a Network Security Group](../../virtual-network/virtual-networks-nsg.md).</span></span>

![createNSG](./media/deploy-linux-vm-into-existing-vnet-using-portal/createNSG.png)

## <a name="add-an-inbound-ssh-allow-rule"></a><span data-ttu-id="abe3a-126">Lägg till regel för att tillåta en inkommande SSH</span><span class="sxs-lookup"><span data-stu-id="abe3a-126">Add an inbound SSH allow rule</span></span>

<span data-ttu-id="abe3a-127">Den virtuella datorn behöver åtkomst från internet, så skapas en regel som tillåter inkommande port 22 trafik som skickas via nätverket till port 22 på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="abe3a-127">The VM needs access from the internet, so a rule allowing inbound port 22 traffic to be passed through the network to port 22 on the VM is created.</span></span>

![createInboundSSH](./media/deploy-linux-vm-into-existing-vnet-using-portal/createInboundSSH.png)

## <a name="associate-the-nsg-with-the-subnet"></a><span data-ttu-id="abe3a-129">Koppla NSG: N till undernätet</span><span class="sxs-lookup"><span data-stu-id="abe3a-129">Associate the NSG with the subnet</span></span>

<span data-ttu-id="abe3a-130">Associera nätverkssäkerhetsgruppen med VNet och undernät som skapats med undernätet.</span><span class="sxs-lookup"><span data-stu-id="abe3a-130">With the VNet and the subnet created, associate the network security group with the subnet.</span></span> <span data-ttu-id="abe3a-131">Nätverkssäkerhetsgrupper kan associeras med ett hela undernät eller ett enskilt virtuellt nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="abe3a-131">Network security groups can be associated with either an entire subnet or an individual VNic.</span></span> <span data-ttu-id="abe3a-132">Med brandväggen filtrerar trafik på undernätverksnivån, skyddas alla VNics och virtuella datorer i undernätet av nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="abe3a-132">With the firewall filtering traffic at the subnet level, all VNics and the VMs within the subnet are protected by the network security group.</span></span> <span data-ttu-id="abe3a-133">Den andra metoden är nätverkssäkerhetsgruppen som är associerat med bara ett enda virtuellt nätverkskort och skyddar en VM.</span><span class="sxs-lookup"><span data-stu-id="abe3a-133">The other approach is the network security group being associated with just a single VNic and protecting just one VM.</span></span>

![associateNSG](./media/deploy-linux-vm-into-existing-vnet-using-portal/associateNSG.png)


## <a name="deploy-the-vm-into-the-vnet-and-nsg"></a><span data-ttu-id="abe3a-135">Distribuera den virtuella datorn till VNet och NSG</span><span class="sxs-lookup"><span data-stu-id="abe3a-135">Deploy the VM into the VNet and NSG</span></span>

<span data-ttu-id="abe3a-136">Med hjälp av Azure portal distribueras Linux VM till den befintliga Azure-resursgrupp, virtuella nätverk, undernät och virtuellt nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="abe3a-136">Using the Azure portal, the Linux VM is deployed to the existing Azure Resource Group, VNet, Subnet, and VNic.</span></span>

![createVM](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVM.png)

<span data-ttu-id="abe3a-138">Genom att använda portalen för att välja befintliga resurser kan instruera du Azure för att distribuera den virtuella datorn i det befintliga nätverket.</span><span class="sxs-lookup"><span data-stu-id="abe3a-138">By using the portal to choose existing resources, you instruct Azure to deploy the VM inside the existing network.</span></span> <span data-ttu-id="abe3a-139">När ett VNet och undernät har distribuerats, kan lämnas som statisk eller permanenta resurser i Azure-region.</span><span class="sxs-lookup"><span data-stu-id="abe3a-139">Once a VNet and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="abe3a-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="abe3a-140">Next steps</span></span>

* [<span data-ttu-id="abe3a-141">Skapa en specifik distribution med hjälp av en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="abe3a-141">Use an Azure Resource Manager template to create a specific deployment</span></span>](../windows/cli-deploy-templates.md)
* [<span data-ttu-id="abe3a-142">Skapa en egen anpassad miljö för en virtuell Linux-dator med hjälp av Azure CLI-kommandon</span><span class="sxs-lookup"><span data-stu-id="abe3a-142">Create your own custom environment for a Linux VM using Azure CLI commands directly</span></span>](create-cli-complete.md)
* [<span data-ttu-id="abe3a-143">Skapa en Linux-VM på Azure med hjälp av mallar</span><span class="sxs-lookup"><span data-stu-id="abe3a-143">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md)
