---
title: "Virtuella Azure-nätverk och virtuella Linux-datorer | Microsoft Docs"
description: "Självstudiekurs – hantera virtuella Azure-nätverk och virtuella Linux-datorer med Azure CLI"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 2366905b8160675f77cbc41ba97540af70be8c01
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="manage-azure-virtual-networks-and-linux-virtual-machines-with-the-azure-cli"></a><span data-ttu-id="7efc1-103">Hantera virtuella Azure-nätverk och virtuella Linux-datorer med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7efc1-103">Manage Azure Virtual Networks and Linux Virtual Machines with the Azure CLI</span></span>

<span data-ttu-id="7efc1-104">Azure virtual machines använder Azure-nätverk för interna och externa nätverkskommunikation.</span><span class="sxs-lookup"><span data-stu-id="7efc1-104">Azure virtual machines use Azure networking for internal and external network communication.</span></span> <span data-ttu-id="7efc1-105">Den här kursen går igenom hur du distribuerar två virtuella datorer och konfigurera Azure nätverk för dessa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7efc1-105">This tutorial walks through deploying two virtual machines and configuring Azure networking for these VMs.</span></span> <span data-ttu-id="7efc1-106">Exemplen i den här kursen förutsätter att de virtuella datorerna är värd för ett webbprogram med en-databas, men ett program inte har distribuerats i självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="7efc1-106">The examples in this tutorial assume that the VMs are hosting a web application with a database back-end, however an application is not deployed in the tutorial.</span></span> <span data-ttu-id="7efc1-107">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="7efc1-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7efc1-108">Distribuera ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="7efc1-108">Deploy a virtual network</span></span>
> * <span data-ttu-id="7efc1-109">Skapa ett undernät i ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="7efc1-109">Create a subnet within a virtual network</span></span>
> * <span data-ttu-id="7efc1-110">Anslut virtuella datorer till ett undernät</span><span class="sxs-lookup"><span data-stu-id="7efc1-110">Attach virtual machines to a subnet</span></span>
> * <span data-ttu-id="7efc1-111">Hantera virtuella offentliga IP-adresser</span><span class="sxs-lookup"><span data-stu-id="7efc1-111">Manage virtual machine public IP addresses</span></span>
> * <span data-ttu-id="7efc1-112">Skydda inkommande trafik för internet</span><span class="sxs-lookup"><span data-stu-id="7efc1-112">Secure incoming internet traffic</span></span>
> * <span data-ttu-id="7efc1-113">Säker VM VM-trafik</span><span class="sxs-lookup"><span data-stu-id="7efc1-113">Secure VM to VM traffic</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="7efc1-114">Om du väljer att installera och använda CLI lokalt kursen krävs att du använder Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="7efc1-114">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="7efc1-115">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="7efc1-115">Run `az --version` to find the version.</span></span> <span data-ttu-id="7efc1-116">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="7efc1-116">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="vm-networking-overview"></a><span data-ttu-id="7efc1-117">VM-nätverk – översikt</span><span class="sxs-lookup"><span data-stu-id="7efc1-117">VM networking overview</span></span>

<span data-ttu-id="7efc1-118">Virtuella Azure-nätverk kan säkra nätverksanslutningar mellan virtuella datorer, internet och andra Azure-tjänster, till exempel Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="7efc1-118">Azure virtual networks enable secure network connections between virtual machines, the internet, and other Azure services such as Azure SQL database.</span></span> <span data-ttu-id="7efc1-119">Virtuella nätverk är uppdelad i logiska segment som kallas subnät.</span><span class="sxs-lookup"><span data-stu-id="7efc1-119">Virtual networks are broken down into logical segments called subnets.</span></span> <span data-ttu-id="7efc1-120">Undernät används för flödeskontroll för nätverk och som en säkerhetsgräns.</span><span class="sxs-lookup"><span data-stu-id="7efc1-120">Subnets are used to control network flow, and as a security boundary.</span></span> <span data-ttu-id="7efc1-121">När du distribuerar en virtuell dator innehåller vanligtvis ett virtuellt nätverksgränssnitt som är ansluten till ett undernät.</span><span class="sxs-lookup"><span data-stu-id="7efc1-121">When deploying a VM, it generally includes a virtual network interface, which is attached to a subnet.</span></span>

## <a name="deploy-virtual-network"></a><span data-ttu-id="7efc1-122">Distribuera virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="7efc1-122">Deploy virtual network</span></span>

<span data-ttu-id="7efc1-123">Ett enda virtuellt nätverk skapas med två undernät för den här kursen.</span><span class="sxs-lookup"><span data-stu-id="7efc1-123">For this tutorial, a single virtual network is created with two subnets.</span></span> <span data-ttu-id="7efc1-124">Ett frontend undernät som värd för ett webbprogram och ett backend-undernät som värd för en databasserver.</span><span class="sxs-lookup"><span data-stu-id="7efc1-124">A front-end subnet for hosting a web application, and a back-end subnet for hosting a database server.</span></span>

<span data-ttu-id="7efc1-125">Innan du kan skapa ett virtuellt nätverk, skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="7efc1-125">Before you can create a virtual network, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="7efc1-126">I följande exempel skapas en resursgrupp med namnet *myRGNetwork* eastus plats.</span><span class="sxs-lookup"><span data-stu-id="7efc1-126">The following example creates a resource group named *myRGNetwork* in the eastus location.</span></span>

```azurecli-interactive 
az group create --name myRGNetwork --location eastus
```

### <a name="create-virtual-network"></a><span data-ttu-id="7efc1-127">Skapa det virtuella nätverket</span><span class="sxs-lookup"><span data-stu-id="7efc1-127">Create virtual network</span></span>

<span data-ttu-id="7efc1-128">Oss den [az network vnet skapa](/cli/azure/network/vnet#create) kommando för att skapa ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="7efc1-128">Us the [az network vnet create](/cli/azure/network/vnet#create) command to create a virtual network.</span></span> <span data-ttu-id="7efc1-129">I det här exemplet är nätverket heter *mvVnet* och ges en adressprefixet *10.0.0.0/16*.</span><span class="sxs-lookup"><span data-stu-id="7efc1-129">In this example, the network is named *mvVnet* and is given an address prefix of *10.0.0.0/16*.</span></span> <span data-ttu-id="7efc1-130">Dessutom skapas ett undernät med namnet *mySubnetFrontEnd* och prefixet *10.0.1.0/24*.</span><span class="sxs-lookup"><span data-stu-id="7efc1-130">A subnet is also created with a name of *mySubnetFrontEnd* and a prefix of *10.0.1.0/24*.</span></span> <span data-ttu-id="7efc1-131">Senare i den här självstudiekursen är frontend VM anslutet till det här undernätet.</span><span class="sxs-lookup"><span data-stu-id="7efc1-131">Later in this tutorial a front-end VM is connected to this subnet.</span></span> 

```azurecli-interactive 
az network vnet create \
  --resource-group myRGNetwork \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnetFrontEnd \
  --subnet-prefix 10.0.1.0/24
```

### <a name="create-subnet"></a><span data-ttu-id="7efc1-132">Skapa ett undernät</span><span class="sxs-lookup"><span data-stu-id="7efc1-132">Create subnet</span></span>

<span data-ttu-id="7efc1-133">Ett nytt undernät har lagts till i det virtuella nätverket med hjälp av den [az undernät för virtuellt nätverk skapa](/cli/azure/network/vnet/subnet#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="7efc1-133">A new subnet is added to the virtual network using the [az network vnet subnet create](/cli/azure/network/vnet/subnet#create) command.</span></span> <span data-ttu-id="7efc1-134">I det här exemplet undernätet med namnet *mySubnetBackEnd* och ges en adressprefixet *10.0.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="7efc1-134">In this example, the subnet is named *mySubnetBackEnd* and is given an address prefix of *10.0.2.0/24*.</span></span> <span data-ttu-id="7efc1-135">Det här undernätet används med alla backend-tjänster.</span><span class="sxs-lookup"><span data-stu-id="7efc1-135">This subnet is used with all back-end services.</span></span>

```azurecli-interactive 
az network vnet subnet create \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --address-prefix 10.0.2.0/24
```

<span data-ttu-id="7efc1-136">Ett nätverk har nu skapats och upp i två undernät, en för frontend-tjänster och en för backend-tjänster.</span><span class="sxs-lookup"><span data-stu-id="7efc1-136">At this point, a network has been created and segmented into two subnets, one for front-end services, and another for back-end services.</span></span> <span data-ttu-id="7efc1-137">I nästa avsnitt, virtuella datorer skapas och anslutna till dessa undernät.</span><span class="sxs-lookup"><span data-stu-id="7efc1-137">In the next section, virtual machines are created and connected to these subnets.</span></span>

## <a name="understand-public-ip-address"></a><span data-ttu-id="7efc1-138">Förstå offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="7efc1-138">Understand public IP address</span></span>

<span data-ttu-id="7efc1-139">En offentlig IP-adress kan Azure-resurser ska vara tillgänglig på internet.</span><span class="sxs-lookup"><span data-stu-id="7efc1-139">A public IP address allows Azure resources to be accessible on the internet.</span></span> <span data-ttu-id="7efc1-140">I det här avsnittet av kursen skapas en virtuell dator för att demonstrera hur du arbetar med offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="7efc1-140">In this section of the tutorial, a VM is created to demonstrate how to work with public IP addresses.</span></span>

### <a name="allocation-method"></a><span data-ttu-id="7efc1-141">Allokeringsmetod</span><span class="sxs-lookup"><span data-stu-id="7efc1-141">Allocation method</span></span>

<span data-ttu-id="7efc1-142">En offentlig IP-adress kan fördelas som dynamiska eller statiska.</span><span class="sxs-lookup"><span data-stu-id="7efc1-142">A public IP address can be allocated as either dynamic or static.</span></span> <span data-ttu-id="7efc1-143">Offentliga IP-adressen tilldelas dynamiskt som standard.</span><span class="sxs-lookup"><span data-stu-id="7efc1-143">By default, public IP address dynamically allocated.</span></span> <span data-ttu-id="7efc1-144">Dynamiska IP-adresser släpps när en virtuell dator har frigjorts.</span><span class="sxs-lookup"><span data-stu-id="7efc1-144">Dynamic IP addresses are released when a VM is deallocated.</span></span> <span data-ttu-id="7efc1-145">Detta medför IP-adressen ändras under åtgärder som innehåller en VM-flyttningen.</span><span class="sxs-lookup"><span data-stu-id="7efc1-145">This behavior causes the IP address to change during any operation that includes a VM deallocation.</span></span>

<span data-ttu-id="7efc1-146">Allokeringsmetoden kan anges till statisk som säkerställer att IP adress fortsätter vara tilldelade till en virtuell dator, även under en frigjord tillstånd.</span><span class="sxs-lookup"><span data-stu-id="7efc1-146">The allocation method can be set to static, which ensures that the IP address remain assigned to a VM, even during a deallocated state.</span></span> <span data-ttu-id="7efc1-147">Den IP-adressen kan inte anges när du använder en statiskt tilldelade IP-adress.</span><span class="sxs-lookup"><span data-stu-id="7efc1-147">When using a statically allocated IP address, the IP address itself cannot be specified.</span></span> <span data-ttu-id="7efc1-148">Istället tilldelas den från en pool med tillgängliga adresser.</span><span class="sxs-lookup"><span data-stu-id="7efc1-148">Instead, it is allocated from a pool of available addresses.</span></span>

### <a name="dynamic-allocation"></a><span data-ttu-id="7efc1-149">Dynamisk allokering</span><span class="sxs-lookup"><span data-stu-id="7efc1-149">Dynamic allocation</span></span>

<span data-ttu-id="7efc1-150">När du skapar en virtuell dator med den [az vm skapa](/cli/azure/vm#create) kommandot offentliga IP-adress allokering standardmetoden är dynamisk.</span><span class="sxs-lookup"><span data-stu-id="7efc1-150">When creating a VM with the [az vm create](/cli/azure/vm#create) command, the default public IP address allocation method is dynamic.</span></span> <span data-ttu-id="7efc1-151">I följande exempel skapas en virtuell dator med en dynamisk IP-adress.</span><span class="sxs-lookup"><span data-stu-id="7efc1-151">In the following example, a VM is created with a dynamic IP address.</span></span> 

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myFrontEndVM \
  --vnet-name myVnet \
  --subnet mySubnetFrontEnd \
  --nsg myNSGFrontEnd \
  --public-ip-address myFrontEndIP \
  --image UbuntuLTS \
  --generate-ssh-keys
```

### <a name="static-allocation"></a><span data-ttu-id="7efc1-152">Statisk tilldelning</span><span class="sxs-lookup"><span data-stu-id="7efc1-152">Static allocation</span></span>

<span data-ttu-id="7efc1-153">När du skapar en virtuell dator med hjälp av den [az vm skapa](/cli/azure/vm#create) kommandot, innehåller den `--public-ip-address-allocation static` argumentet att tilldela en statisk offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="7efc1-153">When creating a virtual machine using the [az vm create](/cli/azure/vm#create) command, include the `--public-ip-address-allocation static` argument to assign a static public IP address.</span></span> <span data-ttu-id="7efc1-154">Den här åtgärden visas inte i den här självstudiekursen, men i nästa avsnitt ändras dynamiskt allokerade IP-adress till ett statiskt allokerade adressen.</span><span class="sxs-lookup"><span data-stu-id="7efc1-154">This operation is not demonstrated in this tutorial, however in the next section a dynamically allocated IP address is changed to a statically allocated address.</span></span> 

### <a name="change-allocation-method"></a><span data-ttu-id="7efc1-155">Ändra allokeringsmetod</span><span class="sxs-lookup"><span data-stu-id="7efc1-155">Change allocation method</span></span>

<span data-ttu-id="7efc1-156">Allokeringsmetod för IP-adress kan ändras med den [az nätverket offentliga ip-uppdatering](/cli/azure/network/public-ip#update) kommando.</span><span class="sxs-lookup"><span data-stu-id="7efc1-156">The IP address allocation method can be changed using the [az network public-ip update](/cli/azure/network/public-ip#update) command.</span></span> <span data-ttu-id="7efc1-157">I det här exemplet ändras allokeringsmetoden av frontend VM IP-adress till statisk.</span><span class="sxs-lookup"><span data-stu-id="7efc1-157">In this example, the IP address allocation method of the front-end VM is changed to static.</span></span>

<span data-ttu-id="7efc1-158">Ta bort den virtuella datorn först.</span><span class="sxs-lookup"><span data-stu-id="7efc1-158">First, deallocate the VM.</span></span>

```azurecli-interactive 
az vm deallocate --resource-group myRGNetwork --name myFrontEndVM
```

<span data-ttu-id="7efc1-159">Använd den [az nätverket offentliga ip-uppdatering](/cli/azure/network/public-ip#update) kommando för att uppdatera allokeringsmetoden.</span><span class="sxs-lookup"><span data-stu-id="7efc1-159">Use the [az network public-ip update](/cli/azure/network/public-ip#update) command to update the allocation method.</span></span> <span data-ttu-id="7efc1-160">I det här fallet den `--allocation-method` anges till *statiska*.</span><span class="sxs-lookup"><span data-stu-id="7efc1-160">In this case, the `--allocation-method` is being set to *static*.</span></span>

```azurecli-interactive 
az network public-ip update --resource-group myRGNetwork --name myFrontEndIP --allocation-method static
```

<span data-ttu-id="7efc1-161">Starta den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="7efc1-161">Start the VM.</span></span>

```azurecli-interactive 
az vm start --resource-group myRGNetwork --name myFrontEndVM --no-wait
```

### <a name="no-public-ip-address"></a><span data-ttu-id="7efc1-162">Ingen offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="7efc1-162">No public IP address</span></span>

<span data-ttu-id="7efc1-163">Ofta behöver inte en virtuell dator som är tillgängliga via internet.</span><span class="sxs-lookup"><span data-stu-id="7efc1-163">Often, a VM does not need to be accessible over the internet.</span></span> <span data-ttu-id="7efc1-164">Om du vill skapa en virtuell dator utan en offentlig IP-adress, använder den `--public-ip-address ""` argument med en tom uppsättning med dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="7efc1-164">To create a VM without a public IP address, use the `--public-ip-address ""` argument with an empty set of double quotes.</span></span> <span data-ttu-id="7efc1-165">Den här konfigurationen visas senare i den här självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="7efc1-165">This configuration is demonstrated later in this tutorial</span></span>

## <a name="secure-network-traffic"></a><span data-ttu-id="7efc1-166">Skydda nätverkstrafik</span><span class="sxs-lookup"><span data-stu-id="7efc1-166">Secure network traffic</span></span>

<span data-ttu-id="7efc1-167">En nätverkssäkerhetsgrupp (NSG) innehåller en lista över säkerhetsregler som tillåter eller nekar nätverkstrafik till resurser som är anslutna till virtuella Azure-nätverk (VNet).</span><span class="sxs-lookup"><span data-stu-id="7efc1-167">A network security group (NSG) contains a list of security rules that allow or deny network traffic to resources connected to Azure Virtual Networks (VNet).</span></span> <span data-ttu-id="7efc1-168">NSG: er kan vara kopplad till undernät eller individuella nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="7efc1-168">NSGs can be associated to subnets or individual network interfaces.</span></span> <span data-ttu-id="7efc1-169">När en NSG är associerad med ett nätverksgränssnitt gäller bara den associera virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="7efc1-169">When an NSG is associated with a network interface, it applies only the associated VM.</span></span> <span data-ttu-id="7efc1-170">När en nätverkssäkerhetsgrupp är kopplad till ett undernät gäller reglerna för alla resurser som är anslutna till undernätet.</span><span class="sxs-lookup"><span data-stu-id="7efc1-170">When an NSG is associated to a subnet, the rules apply to all resources connected to the subnet.</span></span> 

### <a name="network-security-group-rules"></a><span data-ttu-id="7efc1-171">Regler för nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="7efc1-171">Network security group rules</span></span>

<span data-ttu-id="7efc1-172">NSG-regler definiera nätverk portar under vilken trafik tillåts eller nekas.</span><span class="sxs-lookup"><span data-stu-id="7efc1-172">NSG rules define networking ports over which traffic is allowed or denied.</span></span> <span data-ttu-id="7efc1-173">Regler kan innehålla käll- och IP-adressintervall så att trafik styrs mellan specifika system eller undernät.</span><span class="sxs-lookup"><span data-stu-id="7efc1-173">The rules can include source and destination IP address ranges so that traffic is controlled between specific systems or subnets.</span></span> <span data-ttu-id="7efc1-174">NSG-regler kan även innehålla en prioritet (mellan 1 – och 4096).</span><span class="sxs-lookup"><span data-stu-id="7efc1-174">NSG rules also include a priority (between 1—and 4096).</span></span> <span data-ttu-id="7efc1-175">Reglerna utvärderas prioritsordning.</span><span class="sxs-lookup"><span data-stu-id="7efc1-175">Rules are evaluated in the order of priority.</span></span> <span data-ttu-id="7efc1-176">En regel med en prioritet på 100 utvärderas innan en regel med prioritet 200.</span><span class="sxs-lookup"><span data-stu-id="7efc1-176">A rule with a priority of 100 is evaluated before a rule with priority 200.</span></span>

<span data-ttu-id="7efc1-177">Alla NSG:er har en uppsättning standardregler.</span><span class="sxs-lookup"><span data-stu-id="7efc1-177">All NSGs contain a set of default rules.</span></span> <span data-ttu-id="7efc1-178">Standardreglerna kan inte tas bort, men eftersom de tilldelas lägst prioritet så kan de överskridas av de reglerna du själv skapar.</span><span class="sxs-lookup"><span data-stu-id="7efc1-178">The default rules cannot be deleted, but because they are assigned the lowest priority, they can be overridden by the rules that you create.</span></span>

- <span data-ttu-id="7efc1-179">**Virtuellt nätverk** - trafik med ursprung och slutar med ett virtuellt nätverk tillåts både i inkommande och utgående riktningar.</span><span class="sxs-lookup"><span data-stu-id="7efc1-179">**Virtual network** - Traffic originating and ending in a virtual network is allowed both in inbound and outbound directions.</span></span>
- <span data-ttu-id="7efc1-180">**Internet** - utgående trafik tillåts, men inkommande trafik blockeras.</span><span class="sxs-lookup"><span data-stu-id="7efc1-180">**Internet** - Outbound traffic is allowed, but inbound traffic is blocked.</span></span>
- <span data-ttu-id="7efc1-181">**Belastningsutjämnaren** -Tillåt Azure belastningsutjämnare avsökning hälsotillståndet för dina virtuella datorer och rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="7efc1-181">**Load balancer** - Allow Azure’s load balancer to probe the health of your VMs and role instances.</span></span> <span data-ttu-id="7efc1-182">Om du inte använder en belastningsutjämnad uppsättning, kan du åsidosätta den här regeln.</span><span class="sxs-lookup"><span data-stu-id="7efc1-182">If you are not using a load balanced set, you can override this rule.</span></span>

### <a name="create-network-security-groups"></a><span data-ttu-id="7efc1-183">Skapa säkerhetsgrupper för nätverk</span><span class="sxs-lookup"><span data-stu-id="7efc1-183">Create network security groups</span></span>

<span data-ttu-id="7efc1-184">En nätverkssäkerhetsgrupp kan skapas samtidigt som en virtuell dator med hjälp av den [az vm skapa](/cli/azure/vm#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="7efc1-184">A network security group can be created at the same time as a VM using the [az vm create](/cli/azure/vm#create) command.</span></span> <span data-ttu-id="7efc1-185">När du gör det NSG: N är kopplad till nätverksgränssnitt för virtuella datorer och en NSG-regel är skapade för att tillåta trafik på port automatiskt *22* från andra källor.</span><span class="sxs-lookup"><span data-stu-id="7efc1-185">When doing so, the NSG is associated with the VMs network interface and an NSG rule is auto created to allow traffic on port *22* from any source.</span></span> <span data-ttu-id="7efc1-186">Tidigare i den här självstudien har frontend NSG: N skapats automatiskt med frontend VM.</span><span class="sxs-lookup"><span data-stu-id="7efc1-186">Earlier in this tutorial, the front-end NSG was auto-created with the front-end VM.</span></span> <span data-ttu-id="7efc1-187">En regel för NSG har också automatiskt skapat för port 22.</span><span class="sxs-lookup"><span data-stu-id="7efc1-187">An NSG rule was also auto created for port 22.</span></span> 

<span data-ttu-id="7efc1-188">I vissa fall kan vara det bra att skapa en NSG till exempel när standardreglerna för SSH inte skapas eller när NSG: N ska kopplas till ett undernät.</span><span class="sxs-lookup"><span data-stu-id="7efc1-188">In some cases, it may be helpful to pre-create an NSG, such as when default SSH rules should not be created, or when the NSG should be attached to a subnet.</span></span> 

<span data-ttu-id="7efc1-189">Använd den [az nätverket nsg skapa](/cli/azure/network/nsg#create) kommando för att skapa en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="7efc1-189">Use the [az network nsg create](/cli/azure/network/nsg#create) command to create a network security group.</span></span>

```azurecli-interactive 
az network nsg create --resource-group myRGNetwork --name myNSGBackEnd
```

<span data-ttu-id="7efc1-190">I stället för att koppla NSG till ett nätverksgränssnitt, är den associerad med ett undernät.</span><span class="sxs-lookup"><span data-stu-id="7efc1-190">Instead of associating the NSG to a network interface, it is associated with a subnet.</span></span> <span data-ttu-id="7efc1-191">I den här konfigurationen ärver någon virtuell dator som är kopplad till undernätet NSG-regler.</span><span class="sxs-lookup"><span data-stu-id="7efc1-191">In this configuration, any VM that is attached to the subnet inherits the NSG rules.</span></span>

<span data-ttu-id="7efc1-192">Uppdatera befintliga undernätet med namnet *mySubnetBackEnd* med nya NSG: N.</span><span class="sxs-lookup"><span data-stu-id="7efc1-192">Update the existing subnet named *mySubnetBackEnd* with the new NSG.</span></span>

```azurecli-interactive 
az network vnet subnet update \
  --resource-group myRGNetwork \
  --vnet-name myVnet \
  --name mySubnetBackEnd \
  --network-security-group myNSGBackEnd
```

<span data-ttu-id="7efc1-193">Nu skapa en virtuell dator som är ansluten till den *mySubnetBackEnd*.</span><span class="sxs-lookup"><span data-stu-id="7efc1-193">Now create a virtual machine, which is attached to the *mySubnetBackEnd*.</span></span> <span data-ttu-id="7efc1-194">Observera att den `--nsg` argumentet har värdet tomt dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="7efc1-194">Notice that the `--nsg` argument has a value of empty double quotes.</span></span> <span data-ttu-id="7efc1-195">En NSG behöver inte skapas med den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="7efc1-195">An NSG does not need to be created with the VM.</span></span> <span data-ttu-id="7efc1-196">Den virtuella datorn är ansluten till backend-undernät, som är skyddat med backend-förskapade NSG: N.</span><span class="sxs-lookup"><span data-stu-id="7efc1-196">The VM is attached to the back-end subnet, which is protected with the pre-created back-end NSG.</span></span> <span data-ttu-id="7efc1-197">Den här NSG gäller för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="7efc1-197">This NSG applies to the VM.</span></span> <span data-ttu-id="7efc1-198">Observera också här som den `--public-ip-address` argumentet har värdet tomt dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="7efc1-198">Also, notice here that the `--public-ip-address` argument has a value of empty double quotes.</span></span> <span data-ttu-id="7efc1-199">Den här konfigurationen skapar en virtuell dator utan en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="7efc1-199">This configuration creates a VM without a public IP address.</span></span> 

```azurecli-interactive 
az vm create \
  --resource-group myRGNetwork \
  --name myBackEndVM \
  --vnet-name myVnet \
  --subnet mySubnetBackEnd \
  --public-ip-address "" \
  --nsg "" \
  --image UbuntuLTS \
  --generate-ssh-keys
```

### <a name="secure-incoming-traffic"></a><span data-ttu-id="7efc1-200">Skydda inkommande trafik</span><span class="sxs-lookup"><span data-stu-id="7efc1-200">Secure incoming traffic</span></span>

<span data-ttu-id="7efc1-201">När frontend VM skapades, skapades en NSG-regel för att tillåta inkommande trafik på port 22.</span><span class="sxs-lookup"><span data-stu-id="7efc1-201">When the front-end VM was created, an NSG rule was created to allow incoming traffic on port 22.</span></span> <span data-ttu-id="7efc1-202">Den här regeln kan SSH-anslutningar till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="7efc1-202">This rule allows SSH connections to the VM.</span></span> <span data-ttu-id="7efc1-203">I det här exemplet trafik även ska tillåtas på port *80*.</span><span class="sxs-lookup"><span data-stu-id="7efc1-203">For this example, traffic should also be allowed on port *80*.</span></span> <span data-ttu-id="7efc1-204">Den här konfigurationen kan ett program som kan nås på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="7efc1-204">This configuration allows a web application to be accessed on the VM.</span></span>

<span data-ttu-id="7efc1-205">Använd den [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create) kommando för att skapa en regel för port *80*.</span><span class="sxs-lookup"><span data-stu-id="7efc1-205">Use the [az network nsg rule create](/cli/azure/network/nsg/rule#create) command to create a rule for port *80*.</span></span>

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGFrontEnd \
  --name http \
  --access allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range 80
```

<span data-ttu-id="7efc1-206">Frontend VM är nu endast tillgänglig på port *22* och port *80*.</span><span class="sxs-lookup"><span data-stu-id="7efc1-206">The front-end VM is now only accessible on port *22* and port *80*.</span></span> <span data-ttu-id="7efc1-207">All annan inkommande trafik blockeras på nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="7efc1-207">All other incoming traffic is blocked at the network security group.</span></span> <span data-ttu-id="7efc1-208">Det kan vara bra att visualisera regelkonfigurationer NSG.</span><span class="sxs-lookup"><span data-stu-id="7efc1-208">It may be helpful to visualize the NSG rule configurations.</span></span> <span data-ttu-id="7efc1-209">Returnera NSG regelkonfigurationen med den [az nätverket regellistan](/cli/azure/network/nsg/rule#list) kommando.</span><span class="sxs-lookup"><span data-stu-id="7efc1-209">Return the NSG rule configuration with the [az network rule list](/cli/azure/network/nsg/rule#list) command.</span></span> 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myNSGFrontEnd --output table
```

<span data-ttu-id="7efc1-210">Resultat:</span><span class="sxs-lookup"><span data-stu-id="7efc1-210">Output:</span></span>

```azurecli-interactive 
Access    DestinationAddressPrefix      DestinationPortRange  Direction    Name                 Priority  Protocol    ProvisioningState    ResourceGroup    SourceAddressPrefix    SourcePortRange
--------  --------------------------  ----------------------  -----------  -----------------  ----------  ----------  -------------------  ---------------  ---------------------  -----------------
Allow     *                                               22  Inbound      default-allow-ssh        1000  Tcp         Succeeded            myRGNetwork      *                      *
Allow     *                                               80  Inbound      http                      200  Tcp         Succeeded            myRGNetwork      *                      *
```

### <a name="secure-vm-to-vm-traffic"></a><span data-ttu-id="7efc1-211">Säker VM VM-trafik</span><span class="sxs-lookup"><span data-stu-id="7efc1-211">Secure VM to VM traffic</span></span>

<span data-ttu-id="7efc1-212">Regler för nätverkssäkerhetsgrupper kan också använda mellan virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7efc1-212">Network security group rules can also apply between VMs.</span></span> <span data-ttu-id="7efc1-213">I det här exemplet frontend VM behöver kommunicera med backend-VM på port *22* och *3306*.</span><span class="sxs-lookup"><span data-stu-id="7efc1-213">For this example, the front-end VM needs to communicate with the back-end VM on port *22* and *3306*.</span></span> <span data-ttu-id="7efc1-214">Den här konfigurationen tillåter SSH-anslutningar från frontend VM och även att ett program på den frontend virtuella datorn kan kommunicera med en backend-MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="7efc1-214">This configuration allows SSH connections from the front-end VM, and also allow an application on the front-end VM to communicate with a back-end MySQL database.</span></span> <span data-ttu-id="7efc1-215">All annan trafik ska blockeras mellan frontend- och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7efc1-215">All other traffic should be blocked between the front-end and back-end virtual machines.</span></span>

<span data-ttu-id="7efc1-216">Använd den [az nätverket nsg regeln skapa](/cli/azure/network/nsg/rule#create) kommando för att skapa en regel för port 22.</span><span class="sxs-lookup"><span data-stu-id="7efc1-216">Use the [az network nsg rule create](/cli/azure/network/nsg/rule#create) command to create a rule for port 22.</span></span> <span data-ttu-id="7efc1-217">Observera att den `--source-address-prefix` argumentet anger ett värde för *10.0.1.0/24*.</span><span class="sxs-lookup"><span data-stu-id="7efc1-217">Notice that the `--source-address-prefix` argument specifies a value of *10.0.1.0/24*.</span></span> <span data-ttu-id="7efc1-218">Den här konfigurationen säkerställer att endast trafik från undernätet som frontend tillåts via NSG: N.</span><span class="sxs-lookup"><span data-stu-id="7efc1-218">This configuration ensures that only traffic from the front-end subnet is allowed through the NSG.</span></span>

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name SSH \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "22"
```

<span data-ttu-id="7efc1-219">Lägg till regel för MySQL-trafik på port 3306.</span><span class="sxs-lookup"><span data-stu-id="7efc1-219">Now add a rule for MySQL traffic on port 3306.</span></span>

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name MySQL \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 200 \
  --source-address-prefix 10.0.1.0/24 \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "3306"
```

<span data-ttu-id="7efc1-220">Slutligen: eftersom NSG: er har en standardregel som tillåter all trafik mellan virtuella datorer i samma virtuella nätverk, en regel skapas för backend-NSG: er att blockera all trafik.</span><span class="sxs-lookup"><span data-stu-id="7efc1-220">Finally, because NSGs have a default rule allowing all traffic between VMs in the same VNet, a rule can be created for the back-end NSGs to block all traffic.</span></span> <span data-ttu-id="7efc1-221">Observera som den `--priority` ges värdet *300*, vilket är lägre som både MySQL och NSG-regler.</span><span class="sxs-lookup"><span data-stu-id="7efc1-221">Notice here that the `--priority` is given a value of *300*, which is lower that both the NSG and MySQL rules.</span></span> <span data-ttu-id="7efc1-222">Den här konfigurationen garanterar att SSH och MySQL trafiken fortfarande tillåts NSG: N.</span><span class="sxs-lookup"><span data-stu-id="7efc1-222">This configuration ensures that SSH and MySQL traffic is still allowed through the NSG.</span></span>

```azurecli-interactive 
az network nsg rule create \
  --resource-group myRGNetwork \
  --nsg-name myNSGBackEnd \
  --name denyAll \
  --access Deny \
  --protocol Tcp \
  --direction Inbound \
  --priority 300 \
  --source-address-prefix "*" \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range "*"
```

<span data-ttu-id="7efc1-223">Backend-VM är nu endast tillgänglig på port *22* och port *3306* från frontend undernät.</span><span class="sxs-lookup"><span data-stu-id="7efc1-223">The back-end VM is now only accessible on port *22* and port *3306* from the front-end subnet.</span></span> <span data-ttu-id="7efc1-224">All annan inkommande trafik blockeras på nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="7efc1-224">All other incoming traffic is blocked at the network security group.</span></span> <span data-ttu-id="7efc1-225">Det kan vara bra att visualisera regelkonfigurationer NSG.</span><span class="sxs-lookup"><span data-stu-id="7efc1-225">It may be helpful to visualize the NSG rule configurations.</span></span> <span data-ttu-id="7efc1-226">Returnera NSG regelkonfigurationen med den [az nätverket regellistan](/cli/azure/network/nsg/rule#list) kommando.</span><span class="sxs-lookup"><span data-stu-id="7efc1-226">Return the NSG rule configuration with the [az network rule list](/cli/azure/network/nsg/rule#list) command.</span></span> 

```azurecli-interactive 
az network nsg rule list --resource-group myRGNetwork --nsg-name myNSGBackEnd --output table
```

<span data-ttu-id="7efc1-227">Resultat:</span><span class="sxs-lookup"><span data-stu-id="7efc1-227">Output:</span></span>

```azurecli-interactive 
Access    DestinationAddressPrefix    DestinationPortRange    Direction    Name       Priority  Protocol    ProvisioningState    ResourceGroup    SourceAddressPrefix    SourcePortRange
--------  --------------------------  ----------------------  -----------  -------  ----------  ----------  -------------------  ---------------  ---------------------  -----------------
Allow     *                           22                      Inbound      SSH             100  Tcp         Succeeded            myRGNetwork      10.0.1.0/24            *
Allow     *                           3306                    Inbound      MySQL           200  Tcp         Succeeded            myRGNetwork      10.0.1.0/24            *
Deny      *                           *                       Inbound      denyAll         300  Tcp         Succeeded            myRGNetwork      *                      *
```

## <a name="next-steps"></a><span data-ttu-id="7efc1-228">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7efc1-228">Next steps</span></span>

<span data-ttu-id="7efc1-229">I den här självstudiekursen skapas och skyddas av Azure-nätverk som är relaterade till virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7efc1-229">In this tutorial, you created and secured Azure networks as related to virtual machines.</span></span> <span data-ttu-id="7efc1-230">Du har lärt dig hur till:</span><span class="sxs-lookup"><span data-stu-id="7efc1-230">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7efc1-231">Distribuera ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="7efc1-231">Deploy a virtual network</span></span>
> * <span data-ttu-id="7efc1-232">Skapa ett undernät i ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="7efc1-232">Create a subnet within a virtual network</span></span>
> * <span data-ttu-id="7efc1-233">Anslut virtuella datorer till ett undernät</span><span class="sxs-lookup"><span data-stu-id="7efc1-233">Attach virtual machines to a subnet</span></span>
> * <span data-ttu-id="7efc1-234">Hantera virtuella offentliga IP-adresser</span><span class="sxs-lookup"><span data-stu-id="7efc1-234">Manage virtual machine public IP addresses</span></span>
> * <span data-ttu-id="7efc1-235">Skydda inkommande trafik för internet</span><span class="sxs-lookup"><span data-stu-id="7efc1-235">Secure incoming internet traffic</span></span>
> * <span data-ttu-id="7efc1-236">Säker VM VM-trafik</span><span class="sxs-lookup"><span data-stu-id="7efc1-236">Secure VM to VM traffic</span></span>

<span data-ttu-id="7efc1-237">Gå vidare till nästa kurs att lära dig att skydda data på virtuella datorer med Azure backup.</span><span class="sxs-lookup"><span data-stu-id="7efc1-237">Advance to the next tutorial to learn about securing data on virtual machines using Azure backup.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="7efc1-238">Säkerhetskopiera virtuella Linux-datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="7efc1-238">Back up Linux virtual machines in Azure</span></span>](./tutorial-backup-vms.md)
