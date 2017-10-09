---
title: "aaaDeploy virtuella Linux-datorer i befintliga nätverk med Azure-portalen | Microsoft Docs"
description: "Distribuera en Linux-VM till ett befintligt virtuellt Azure-nätverk som använder hello-portalen."
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
ms.openlocfilehash: 51272b8cdb1dc7f3fe768d2ebbf4ebe0d754cf19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-hello-azure-portal"></a><span data-ttu-id="cf387-103">Hur toodeploy en Linux-dator i ett befintligt virtuellt nätverk med Azure med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="cf387-103">How toodeploy a Linux virtual machine into an existing Azure Virtual Network with hello Azure portal</span></span>

<span data-ttu-id="cf387-104">Den här artikeln beskrivs hur du toodeploy en virtuell dator (VM) till ett befintligt virtuellt nätverk (VNet).</span><span class="sxs-lookup"><span data-stu-id="cf387-104">This article shows you how toodeploy a virtual machine (VM) into an existing virtual network (VNet).</span></span> <span data-ttu-id="cf387-105">Azure tillgångar som Vnet och nätverket säkerhetsgrupper bör vara statiska och resurser som distribueras sällan kvar längre.</span><span class="sxs-lookup"><span data-stu-id="cf387-105">Azure assets like VNets and network security groups should be static and long lived resources that are rarely deployed.</span></span> <span data-ttu-id="cf387-106">När ett virtuellt nätverk har distribuerats, kan den återanvändas av konstant redeployments utan någon negativ påverkar toohello infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="cf387-106">Once a VNet has been deployed, it can be reused by constant redeployments without any adverse affects toohello infrastructure.</span></span> <span data-ttu-id="cf387-107">Om du tänker ett VNet som en traditionell maskinvara nätverks-switch - du behöver tooconfigure en helt ny maskinvara växel med varje distribution.</span><span class="sxs-lookup"><span data-stu-id="cf387-107">Thinking about a VNet as being a traditional hardware network switch - you would not need tooconfigure a brand new hardware switch with each deployment.</span></span>  

<span data-ttu-id="cf387-108">Med en korrekt konfigurerad VNet, kan du fortsätta toodeploy nya servrar i det virtuella nätverket flera gånger med några eventuella ändringar som krävs under hello hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="cf387-108">With a correctly configured VNet, you can continue toodeploy new servers into that VNet over and over with few, if any, changes required over hello life of hello VNet.</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="cf387-109">Skapa hello resursgrupp</span><span class="sxs-lookup"><span data-stu-id="cf387-109">Create hello resource group</span></span>

<span data-ttu-id="cf387-110">Först skapar du en resurs grupp tooorganize allt som du skapar i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="cf387-110">First, create a resource group tooorganize everything you create in this walkthrough.</span></span> <span data-ttu-id="cf387-111">Läs mer om Azure-resursgrupper [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)</span><span class="sxs-lookup"><span data-stu-id="cf387-111">For more information about Azure resource groups, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md)</span></span>

![createResourceGroup](./media/deploy-linux-vm-into-existing-vnet-using-portal/createResourceGroup.png)


## <a name="create-hello-vnet"></a><span data-ttu-id="cf387-113">Skapa hello VNet</span><span class="sxs-lookup"><span data-stu-id="cf387-113">Create hello VNet</span></span>

<span data-ttu-id="cf387-114">Skapa sedan en VNet toolaunch hello virtuella datorer i.</span><span class="sxs-lookup"><span data-stu-id="cf387-114">Next, build a VNet toolaunch hello VMs into.</span></span> <span data-ttu-id="cf387-115">Hej VNet innehåller ett undernät och associeras med hello nätverkssäkerhetsgrupp med det här undernätet i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="cf387-115">hello VNet contains one subnet and is associated with hello network security group with this subnet in a later step.</span></span>

![createVNet](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVNet.png)

## <a name="add-a-vnic-toohello-subnet"></a><span data-ttu-id="cf387-117">Lägg till ett virtuellt nätverkskort toohello undernät</span><span class="sxs-lookup"><span data-stu-id="cf387-117">Add a VNic toohello subnet</span></span>

<span data-ttu-id="cf387-118">Virtuella nätverkskort (VNics) är viktigt när du ansluter toodifferent virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="cf387-118">Virtual network cards (VNics) are important as you can connect them toodifferent VMs.</span></span> <span data-ttu-id="cf387-119">Den här metoden används för att hålla hello VNic som en statisk resurs hello virtuella datorer kan vara tillfälliga.</span><span class="sxs-lookup"><span data-stu-id="cf387-119">This approach keeps hello VNic as a static resource while hello VMs can be temporary.</span></span> <span data-ttu-id="cf387-120">Skapa ett virtuellt nätverkskort och koppla den till hello undernät som skapats i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="cf387-120">Create a VNic and associate it with hello subnet created in hello previous step.</span></span>

![createVNic](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVNic.png)

## <a name="create-hello-network-security-group"></a><span data-ttu-id="cf387-122">Skapa hello nätverkssäkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="cf387-122">Create hello network security group</span></span>

<span data-ttu-id="cf387-123">Säkerhetsgrupper för Azure-nätverk är likvärdiga tooa brandväggen på hello nätverksnivån.</span><span class="sxs-lookup"><span data-stu-id="cf387-123">Azure network security groups are equivalent tooa firewall at hello network layer.</span></span> <span data-ttu-id="cf387-124">Läs mer på Azure nätverkssäkerhetsgrupper [vad är en Nätverkssäkerhetsgrupp](../../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="cf387-124">For more information on Azure network security groups, see [What is a Network Security Group](../../virtual-network/virtual-networks-nsg.md).</span></span>

![createNSG](./media/deploy-linux-vm-into-existing-vnet-using-portal/createNSG.png)

## <a name="add-an-inbound-ssh-allow-rule"></a><span data-ttu-id="cf387-126">Lägg till regel för att tillåta en inkommande SSH</span><span class="sxs-lookup"><span data-stu-id="cf387-126">Add an inbound SSH allow rule</span></span>

<span data-ttu-id="cf387-127">hello VM behöver åtkomst från hello internet, så att en regel som tillåter inkommande port 22 trafik toobe har passerat hello nätverket tooport 22 på hello virtuell dator skapas.</span><span class="sxs-lookup"><span data-stu-id="cf387-127">hello VM needs access from hello internet, so a rule allowing inbound port 22 traffic toobe passed through hello network tooport 22 on hello VM is created.</span></span>

![createInboundSSH](./media/deploy-linux-vm-into-existing-vnet-using-portal/createInboundSSH.png)

## <a name="associate-hello-nsg-with-hello-subnet"></a><span data-ttu-id="cf387-129">Koppla hello NSG till hello undernät</span><span class="sxs-lookup"><span data-stu-id="cf387-129">Associate hello NSG with hello subnet</span></span>

<span data-ttu-id="cf387-130">Associera hello nätverkssäkerhetsgrupp med hello VNet och hello undernät som skapats med hello undernät.</span><span class="sxs-lookup"><span data-stu-id="cf387-130">With hello VNet and hello subnet created, associate hello network security group with hello subnet.</span></span> <span data-ttu-id="cf387-131">Nätverkssäkerhetsgrupper kan associeras med ett hela undernät eller ett enskilt virtuellt nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="cf387-131">Network security groups can be associated with either an entire subnet or an individual VNic.</span></span> <span data-ttu-id="cf387-132">Med hello brandvägg filtrera trafik på nivån för hello undernät, skyddas alla VNics och hello virtuella datorer i undernätet hello av hello nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="cf387-132">With hello firewall filtering traffic at hello subnet level, all VNics and hello VMs within hello subnet are protected by hello network security group.</span></span> <span data-ttu-id="cf387-133">hello är andra metoden hello network security group som är associerad med bara ett enda virtuellt nätverkskort och skyddar en VM.</span><span class="sxs-lookup"><span data-stu-id="cf387-133">hello other approach is hello network security group being associated with just a single VNic and protecting just one VM.</span></span>

![associateNSG](./media/deploy-linux-vm-into-existing-vnet-using-portal/associateNSG.png)


## <a name="deploy-hello-vm-into-hello-vnet-and-nsg"></a><span data-ttu-id="cf387-135">Distribuera hello VM till hello VNet och NSG</span><span class="sxs-lookup"><span data-stu-id="cf387-135">Deploy hello VM into hello VNet and NSG</span></span>

<span data-ttu-id="cf387-136">Hello Azure-portalen är hello Linux VM att använda, distribuerade toohello befintliga Azure-resursgrupp, virtuella nätverk, undernät och virtuellt nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="cf387-136">Using hello Azure portal, hello Linux VM is deployed toohello existing Azure Resource Group, VNet, Subnet, and VNic.</span></span>

![createVM](./media/deploy-linux-vm-into-existing-vnet-using-portal/createVM.png)

<span data-ttu-id="cf387-138">Genom att använda befintliga resurser för hello portal toochoose kan instruera Azure toodeploy hello VM i hello befintliga nätverk.</span><span class="sxs-lookup"><span data-stu-id="cf387-138">By using hello portal toochoose existing resources, you instruct Azure toodeploy hello VM inside hello existing network.</span></span> <span data-ttu-id="cf387-139">När ett VNet och undernät har distribuerats, kan lämnas som statisk eller permanenta resurser i Azure-region.</span><span class="sxs-lookup"><span data-stu-id="cf387-139">Once a VNet and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="cf387-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cf387-140">Next steps</span></span>

* [<span data-ttu-id="cf387-141">Använda en viss distribution för toocreate en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="cf387-141">Use an Azure Resource Manager template toocreate a specific deployment</span></span>](../windows/cli-deploy-templates.md)
* [<span data-ttu-id="cf387-142">Skapa en egen anpassad miljö för en virtuell Linux-dator med hjälp av Azure CLI-kommandon</span><span class="sxs-lookup"><span data-stu-id="cf387-142">Create your own custom environment for a Linux VM using Azure CLI commands directly</span></span>](create-cli-complete.md)
* [<span data-ttu-id="cf387-143">Skapa en Linux-VM på Azure med hjälp av mallar</span><span class="sxs-lookup"><span data-stu-id="cf387-143">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md)
