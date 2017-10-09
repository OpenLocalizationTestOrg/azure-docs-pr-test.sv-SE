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
# <a name="how-toodeploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-hello-azure-cli"></a><span data-ttu-id="a4796-103">Hur toodeploy en Linux-dator i ett befintligt virtuellt nätverk med Azure med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a4796-103">How toodeploy a Linux virtual machine into an existing Azure Virtual Network with hello Azure CLI</span></span>

<span data-ttu-id="a4796-104">Den här artikeln visar hur hello toouse Azure CLI 2.0 toodeploy en virtuell dator (VM) till ett befintligt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="a4796-104">This article shows you how toouse hello Azure CLI 2.0 toodeploy a virtual machine (VM) into an existing virtual network.</span></span> <span data-ttu-id="a4796-105">hello kraven är:</span><span class="sxs-lookup"><span data-stu-id="a4796-105">hello requirements are:</span></span>

- [<span data-ttu-id="a4796-106">ett Azure-konto</span><span class="sxs-lookup"><span data-stu-id="a4796-106">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
- [<span data-ttu-id="a4796-107">offentliga och privata SSH-nyckelfiler</span><span class="sxs-lookup"><span data-stu-id="a4796-107">SSH public and private key files</span></span>](mac-create-ssh-keys.md)

<span data-ttu-id="a4796-108">Du kan också utföra dessa steg med hello [Azure CLI 1.0](deploy-linux-vm-into-existing-vnet-using-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="a4796-108">You can also perform these steps with hello [Azure CLI 1.0](deploy-linux-vm-into-existing-vnet-using-cli-nodejs.md).</span></span>


## <a name="quick-commands"></a><span data-ttu-id="a4796-109">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="a4796-109">Quick Commands</span></span>
<span data-ttu-id="a4796-110">Om du behöver tooquickly utföra hello uppgiften, hello efter avsnittet information hello-kommandon som krävs.</span><span class="sxs-lookup"><span data-stu-id="a4796-110">If you need tooquickly accomplish hello task, hello following section details hello  commands needed.</span></span> <span data-ttu-id="a4796-111">Mer detaljerad information och kontext för varje steg finns hello resten av dokumentet hello [startar här](#detailed-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="a4796-111">More detailed information and context for each step can be found hello rest of hello document, [starting here](#detailed-walkthrough).</span></span>

<span data-ttu-id="a4796-112">toocreate den här anpassade miljön, behöver du hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="a4796-112">toocreate this custom environment, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="a4796-113">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="a4796-113">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="a4796-114">Exempel parameternamn inkluderar *myResourceGroup*, *myVnet*, och *myVM*.</span><span class="sxs-lookup"><span data-stu-id="a4796-114">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>

<span data-ttu-id="a4796-115">**Krav:** Azure resursgrupp, virtuella nätverk och undernät, inkommande nätverkssäkerhetsgrupp med SSH, och ett virtuellt nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="a4796-115">**Pre-requirements:** Azure resource group, virtual network and subnet, network security group with SSH inbound, and a virtual network interface card.</span></span>

### <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a><span data-ttu-id="a4796-116">Distribuera hello VM till hello virtuell nätverksinfrastruktur</span><span class="sxs-lookup"><span data-stu-id="a4796-116">Deploy hello VM into hello virtual network infrastructure</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Debian \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="a4796-117">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="a4796-117">Detailed walkthrough</span></span>

<span data-ttu-id="a4796-118">Azure tillgångar som virtuella nätverk och nätverkssäkerhetsgrupper bör vara statiska och resurser som distribueras sällan kvar längre.</span><span class="sxs-lookup"><span data-stu-id="a4796-118">Azure assets like virtual networks and network security groups should be static and long lived resources that are rarely deployed.</span></span> <span data-ttu-id="a4796-119">När ett virtuellt nätverk har distribuerats, kan den återanvändas av nya distributioner utan någon negativ påverkar toohello infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="a4796-119">Once a virtual network has been deployed, it can be reused by new deployments without any adverse affects toohello infrastructure.</span></span> <span data-ttu-id="a4796-120">Tänka på ett virtuellt nätverk som en traditionell maskinvara nätverks-switch - behöver du inte tooconfigure en helt ny maskinvara växel med varje distribution.</span><span class="sxs-lookup"><span data-stu-id="a4796-120">Think about a virtual network as being a traditional hardware network switch - you would not need tooconfigure a brand new hardware switch with each deployment.</span></span> <span data-ttu-id="a4796-121">Med ett korrekt konfigurerat virtuellt nätverk kan du fortsätta toodeploy nya virtuella datorer i det virtuella nätverket flera gånger med få eventuella ändringar som krävs under hello hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="a4796-121">With a correctly configured virtual network, you can continue toodeploy new VMs into that virtual network over and over with few, if any, changes required over hello life of hello virtual network.</span></span>

<span data-ttu-id="a4796-122">toocreate den här anpassade miljön, behöver du hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="a4796-122">toocreate this custom environment, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="a4796-123">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="a4796-123">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="a4796-124">Exempel parameternamn inkluderar *myResourceGroup*, *myVnet*, och *myVM*.</span><span class="sxs-lookup"><span data-stu-id="a4796-124">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="a4796-125">Skapa hello resursgrupp</span><span class="sxs-lookup"><span data-stu-id="a4796-125">Create hello resource group</span></span>

<span data-ttu-id="a4796-126">Först skapa en Azure-resurs grupp tooorganize allt som du skapar i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="a4796-126">First, create an Azure resource group tooorganize everything you create in this walkthrough.</span></span> <span data-ttu-id="a4796-127">Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a4796-127">For more information on resource groups, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="a4796-128">Skapa hello resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="a4796-128">Create hello resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="a4796-129">hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats:</span><span class="sxs-lookup"><span data-stu-id="a4796-129">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

## <a name="create-hello-virtual-network"></a><span data-ttu-id="a4796-130">Skapa hello virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="a4796-130">Create hello virtual network</span></span>

<span data-ttu-id="a4796-131">Kan skapa ett virtuellt Azure-nätverk toolaunch hello virtuella datorer i.</span><span class="sxs-lookup"><span data-stu-id="a4796-131">Lets build an Azure virtual network toolaunch hello VMs into.</span></span> <span data-ttu-id="a4796-132">Mer information om virtuella nätverk finns [skapa ett virtuellt nätverk med hjälp av hello Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="a4796-132">For more information on virtual networks, see [Create a virtual network by using hello Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md).</span></span> <span data-ttu-id="a4796-133">Skapa hello virtuellt nätverk med [az network vnet skapa](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="a4796-133">Create hello virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="a4796-134">hello följande exempel skapas ett virtuellt nätverk med namnet *myVnet* och undernät med namnet *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="a4796-134">hello following example creates a virtual network named *myVnet* and subnet named *mySubnet*:</span></span>

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myVnet \
    --address-prefix 10.10.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 10.10.1.0/24
```

## <a name="create-hello-network-security-group"></a><span data-ttu-id="a4796-135">Skapa hello nätverkssäkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="a4796-135">Create hello network security group</span></span>

<span data-ttu-id="a4796-136">Säkerhetsgrupper för Azure-nätverk är likvärdiga tooa brandväggen på hello nätverksnivån.</span><span class="sxs-lookup"><span data-stu-id="a4796-136">Azure network security groups are equivalent tooa firewall at hello network layer.</span></span> <span data-ttu-id="a4796-137">Mer information om nätverkssäkerhetsgrupper finns [hur toocreate nätverkssäkerhet grupper i hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="a4796-137">For more information on network security groups, see [How toocreate network security groups in hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).</span></span> <span data-ttu-id="a4796-138">Skapa hello nätverkssäkerhetsgrupp med [az nätverket nsg skapa](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="a4796-138">Create hello network security group with [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="a4796-139">hello följande exempel skapar en nätverkssäkerhetsgrupp med namnet *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="a4796-139">hello following example creates a network security group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-ssh-allow-rule"></a><span data-ttu-id="a4796-140">Lägg till regel för att tillåta en inkommande SSH</span><span class="sxs-lookup"><span data-stu-id="a4796-140">Add an inbound SSH allow rule</span></span>

<span data-ttu-id="a4796-141">hello VM behöver åtkomst från hello internet, så att en regel som tillåter inkommande port 22 trafik toobe har passerat hello nätverket tooport 22 på hello VM krävs.</span><span class="sxs-lookup"><span data-stu-id="a4796-141">hello VM needs access from hello internet, so a rule allowing inbound port 22 traffic toobe passed through hello network tooport 22 on hello VM is needed.</span></span> <span data-ttu-id="a4796-142">Lägg till en inkommande regel för hello nätverkssäkerhetsgrupp med [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create).</span><span class="sxs-lookup"><span data-stu-id="a4796-142">Add an inbound rule for hello network security group with [az network nsg rule create](/cli/azure/network/nsg/rule#create).</span></span> <span data-ttu-id="a4796-143">hello följande exempel skapas en regel med namnet *myNetworkSecurityGroupRuleSSH*:</span><span class="sxs-lookup"><span data-stu-id="a4796-143">hello following example creates a rule named *myNetworkSecurityGroupRuleSSH*:</span></span>

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleSSH \
    --priority 1000 \
    --protocol tcp \
    --destination-port-range 22 \
```

## <a name="attach-hello-subnet-toohello-network-security-group"></a><span data-ttu-id="a4796-144">Koppla hello undernät toohello nätverkssäkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="a4796-144">Attach hello subnet toohello network security group</span></span>

<span data-ttu-id="a4796-145">hello regler för nätverkssäkerhetsgrupper kan vara tillämpade tooa undernät eller ett specifikt virtuellt nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="a4796-145">hello network security group rules can be applied tooa subnet or a specific virtual network interface.</span></span> <span data-ttu-id="a4796-146">Kan bifoga hello säkerhet grupp tooour undernät.</span><span class="sxs-lookup"><span data-stu-id="a4796-146">Lets attach hello network security group tooour subnet.</span></span> <span data-ttu-id="a4796-147">Koppla dina undernät toohello nätverkssäkerhetsgrupp med [az network vnet undernät uppdatering](/cli/azure/network/vnet/subnet#update):</span><span class="sxs-lookup"><span data-stu-id="a4796-147">Attach your subnet toohello network security group with [az network vnet subnet update](/cli/azure/network/vnet/subnet#update):</span></span>

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="add-a-virtual-network-interface-card-toohello-subnet"></a><span data-ttu-id="a4796-148">Lägg till ett virtuellt nätverk interface card toohello undernät</span><span class="sxs-lookup"><span data-stu-id="a4796-148">Add a virtual network interface card toohello subnet</span></span>

<span data-ttu-id="a4796-149">Virtuella nätverkskort (VNics) är viktigt eftersom du kan återanvända dem genom att ansluta dem toodifferent virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a4796-149">Virtual network interface cards (VNics) are important as you can reuse them by connecting them toodifferent VMs.</span></span> <span data-ttu-id="a4796-150">Den här återanvändning kan du tookeep hello VNic som en statisk resurs hello virtuella datorer kan vara tillfälliga.</span><span class="sxs-lookup"><span data-stu-id="a4796-150">This reuse allows you tookeep hello VNic as a static resource while hello VMs can be temporary.</span></span> <span data-ttu-id="a4796-151">Skapa ett virtuellt nätverkskort och koppla den till hello undernät med [az nätverket nic skapa](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="a4796-151">Create a VNic and associate it with hello subnet with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="a4796-152">hello följande exempel skapas ett virtuellt nätverkskort med namnet *myNic*:</span><span class="sxs-lookup"><span data-stu-id="a4796-152">hello following example creates a VNic named *myNic*:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet
```

## <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a><span data-ttu-id="a4796-153">Distribuera hello VM till hello virtuell nätverksinfrastruktur</span><span class="sxs-lookup"><span data-stu-id="a4796-153">Deploy hello VM into hello virtual network infrastructure</span></span>

<span data-ttu-id="a4796-154">Nu har du ett virtuellt nätverk och undernät och en säkerhet grupp tooprotect hello undernät i nätverket genom att blockera all inkommande trafik utom port 22 för SSH.</span><span class="sxs-lookup"><span data-stu-id="a4796-154">You now have a virtual network and subnet, and a network security group tooprotect hello subnet by blocking all inbound traffic except port 22 for SSH.</span></span> <span data-ttu-id="a4796-155">hello VM kan nu distribueras i det här befintliga nätverksinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="a4796-155">hello VM can now be deployed inside this existing network infrastructure.</span></span>

<span data-ttu-id="a4796-156">Skapa den virtuella datorn med [az vm skapa](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="a4796-156">Create your VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="a4796-157">Mer information om hello flaggar toouse med hello Azure CLI 2.0 toodeploy fullständig VM, finns [skapa en fullständig Linux-miljö med hjälp av hello Azure CLI](create-cli-complete.md).</span><span class="sxs-lookup"><span data-stu-id="a4796-157">For more information on hello flags toouse with hello Azure CLI 2.0 toodeploy a complete VM, see [Create a complete Linux environment by using hello Azure CLI](create-cli-complete.md).</span></span>

<span data-ttu-id="a4796-158">hello följande exempel skapar en virtuell dator med hjälp av Azure hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="a4796-158">hello following example creates a VM using Azure Managed Disks.</span></span> <span data-ttu-id="a4796-159">Diskarna som hanteras av hello Azure-plattformen och kräver inte någon förberedelse eller plats toostore dem.</span><span class="sxs-lookup"><span data-stu-id="a4796-159">These disks are handled by hello Azure platform and do not require any preparation or location toostore them.</span></span> <span data-ttu-id="a4796-160">Mer information om hanterade diskar finns i [Översikt över Azure Managed Disks](../../storage/storage-managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a4796-160">For more information about managed disks, see [Azure Managed Disks overview](../../storage/storage-managed-disks-overview.md).</span></span> <span data-ttu-id="a4796-161">Om du vill toouse ohanterad diskar finns i hello ytterligare kommentaren nedan.</span><span class="sxs-lookup"><span data-stu-id="a4796-161">If you wish toouse unmanaged disks, see hello additional note below.</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Debian \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic
```

<span data-ttu-id="a4796-162">Hoppa över det här steget om du använder hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="a4796-162">If you use managed disks, skip this step.</span></span> <span data-ttu-id="a4796-163">Om du vill toouse ohanterad diskar måste tooadd hello följande ytterligare parametrar toohello du fortsätter kommandot toocreate ohanterad diskar i hello lagringskontonamnet `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="a4796-163">If you wish toouse unmanaged disks, you need tooadd hello following additional parameters toohello proceeding command toocreate unmanaged disks in hello storage account named `mystorageaccount`:</span></span> 

```azurecli
    --use-unmanaged-disk \
    --storage-account mystorageaccount
```

<span data-ttu-id="a4796-164">Med hjälp av hello flaggor CLI toocall befintliga resurser du instruera Azure toodeploy hello VM i hello befintliga nätverk.</span><span class="sxs-lookup"><span data-stu-id="a4796-164">By using hello CLI flags toocall out existing resources, you instruct Azure toodeploy hello VM inside hello existing network.</span></span> <span data-ttu-id="a4796-165">När ett virtuellt nätverk och undernät har distribuerats, kan lämnas som statisk eller permanenta resurser i Azure-region.</span><span class="sxs-lookup"><span data-stu-id="a4796-165">Once a virtual network and subnet have been deployed, they can be left as static or permanent resources inside your Azure region.</span></span> <span data-ttu-id="a4796-166">I det här exemplet inte skapa och tilldela en offentlig IP-adress toohello virtuellt nätverkskort, så att den här virtuella datorn inte är offentligt tillgänglig över hello Internet.</span><span class="sxs-lookup"><span data-stu-id="a4796-166">In this example, you did not create and assign a public IP address toohello VNic, so this VM is not publicly accessible over hello Internet.</span></span> <span data-ttu-id="a4796-167">Mer information finns i [skapa en virtuell dator med en statisk offentlig IP-adress med hjälp av hello Azure CLI](../../virtual-network/virtual-network-deploy-static-pip-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="a4796-167">For more information, see [Create a VM with a static public IP using hello Azure CLI](../../virtual-network/virtual-network-deploy-static-pip-arm-cli.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a4796-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a4796-168">Next steps</span></span>
<span data-ttu-id="a4796-169">Mer information om sätt toocreate virtuella datorer i Azure finns i hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="a4796-169">For more information about ways toocreate virtual machines in Azure, see hello following resources:</span></span>

* [<span data-ttu-id="a4796-170">Använda en viss distribution för toocreate en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="a4796-170">Use an Azure Resource Manager template toocreate a specific deployment</span></span>](../windows/cli-deploy-templates.md)
* [<span data-ttu-id="a4796-171">Skapa en egen anpassad miljö för en virtuell Linux-dator med hjälp av Azure CLI-kommandon</span><span class="sxs-lookup"><span data-stu-id="a4796-171">Create your own custom environment for a Linux VM using Azure CLI commands directly</span></span>](create-cli-complete.md)
* [<span data-ttu-id="a4796-172">Skapa en Linux-VM på Azure med hjälp av mallar</span><span class="sxs-lookup"><span data-stu-id="a4796-172">Create a Linux VM on Azure using templates</span></span>](create-ssh-secured-vm-from-template.md)
