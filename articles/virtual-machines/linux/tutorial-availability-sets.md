---
title: "Tillgänglighetsuppsättningar självstudier för Linux virtuella datorer i Azure | Microsoft Docs"
description: "Läs mer om Tillgänglighetsuppsättningarna för Linux virtuella datorer i Azure."
documentationcenter: 
services: virtual-machines-linux
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 63fe3f165864f06228604cac56d06cc061ab25f5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-availability-sets"></a><span data-ttu-id="eff4e-103">Hur du använder tillgänglighetsuppsättningar</span><span class="sxs-lookup"><span data-stu-id="eff4e-103">How to use availability sets</span></span>


<span data-ttu-id="eff4e-104">I kursen får lära du dig att öka tillgängligheten och tillförlitligheten hos dina virtuella lösningar på Azure med hjälp av en funktion som kallas Tillgänglighetsuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="eff4e-104">In this tutorial, you will learn how to increase the availability and reliability of your Virtual Machine solutions on Azure using a capability called Availability Sets.</span></span> <span data-ttu-id="eff4e-105">Tillgänglighetsuppsättningar se till att de virtuella datorerna som du distribuerar i Azure är fördelade på flera isolerade maskinvara kluster.</span><span class="sxs-lookup"><span data-stu-id="eff4e-105">Availability sets ensure that the VMs you deploy on Azure are distributed across multiple isolated hardware clusters.</span></span> <span data-ttu-id="eff4e-106">Detta säkerställer att om ett maskinvaru- eller fel i Azure inträffar, underordnade uppsättning dina virtuella datorer som påverkas och som din lösning ska vara tillgängliga och fungerar för dina kunder som använder den.</span><span class="sxs-lookup"><span data-stu-id="eff4e-106">Doing this ensures that if a hardware or software failure within Azure happens, only a sub-set of your VMs will be impacted and that your overall solution will remain available and operational from the perspective of your customers using it.</span></span>

<span data-ttu-id="eff4e-107">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="eff4e-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="eff4e-108">Skapa en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="eff4e-108">Create an availability set</span></span>
> * <span data-ttu-id="eff4e-109">Skapa en virtuell dator i en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="eff4e-109">Create a VM in an availability set</span></span>
> * <span data-ttu-id="eff4e-110">Kontrollera tillgängliga storlekar på VM</span><span class="sxs-lookup"><span data-stu-id="eff4e-110">Check available VM sizes</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="eff4e-111">Om du väljer att installera och använda CLI lokalt kursen krävs att du använder Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="eff4e-111">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="eff4e-112">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="eff4e-112">Run `az --version` to find the version.</span></span> <span data-ttu-id="eff4e-113">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="eff4e-113">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="availability-set-overview"></a><span data-ttu-id="eff4e-114">Översikt över tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="eff4e-114">Availability set overview</span></span>

<span data-ttu-id="eff4e-115">En Tillgänglighetsuppsättning är en logisk gruppering funktion som du kan använda i Azure för att säkerställa att VM-resurser som du placerar i den isolerade från varandra när de distribueras i ett Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="eff4e-115">An Availability Set is a logical grouping capability that you can use in Azure to ensure that the VM resources you place within it are isolated from each other when they are deployed within an Azure datacenter.</span></span> <span data-ttu-id="eff4e-116">Azure säkerställer att de virtuella datorerna som du placerar i en Tillgänglighetsuppsättning körs över flera fysiska servrar, beräkna rack, lagringsenheter och nätverksväxlar.</span><span class="sxs-lookup"><span data-stu-id="eff4e-116">Azure ensures that the VMs you place within an Availability Set run across multiple physical servers, compute racks, storage units, and network switches.</span></span> <span data-ttu-id="eff4e-117">Detta säkerställer att om ett maskinvaru- eller Azure programvarufel påverkas endast en delmängd av dina virtuella datorer, och tillämpningsprogrammet övergripande förblir upp och fortsätter att vara tillgängliga för kunderna.</span><span class="sxs-lookup"><span data-stu-id="eff4e-117">This ensures that in the event of a hardware or Azure software failure, only a subset of your VMs will be impacted, and your overall application will stay up and continue to be available to your customers.</span></span> <span data-ttu-id="eff4e-118">Med hjälp av Tillgänglighetsuppsättningar är en viktig funktion att använda när du vill skapa tillförlitliga molnlösningar.</span><span class="sxs-lookup"><span data-stu-id="eff4e-118">Using Availability Sets is an essential capability to leverage when you want to build reliable cloud solutions.</span></span>

<span data-ttu-id="eff4e-119">Nu ska vi titta en typisk VM-baserad lösning där du kan ha 4 frontend-webbservrar och använda 2 backend-VMs som värd för en databas.</span><span class="sxs-lookup"><span data-stu-id="eff4e-119">Let’s consider a typical VM-based solution where you might have 4 front-end web servers and use 2 back-end VMs that host a database.</span></span> <span data-ttu-id="eff4e-120">Med Azure, skulle du vill definiera två tillgänglighetsuppsättningar innan du distribuerar dina virtuella datorer: en tillgänglighetsuppsättning för skiktet ”web” och en tillgänglighetsuppsättning för skiktet ”databas”.</span><span class="sxs-lookup"><span data-stu-id="eff4e-120">With Azure, you’d want to define two availability sets before you deploy your VMs: one availability set for the “web” tier and one availability set for the “database” tier.</span></span> <span data-ttu-id="eff4e-121">När du skapar en ny virtuell dator kan du ange tillgänglighetsuppsättningen som en parameter till den virtuella datorn az skapar kommando och Azure automatiskt säkerställer att de virtuella datorerna som du skapar i uppsättningen med tillgängliga isoleras över flera fysiska maskinvaruresurser.</span><span class="sxs-lookup"><span data-stu-id="eff4e-121">When you create a new VM you can then specify the availability set as a parameter to the az vm create command, and Azure will automatically ensure that the VMs you create within the available set are isolated across multiple physical hardware resources.</span></span> <span data-ttu-id="eff4e-122">Det innebär att om den fysiska maskinvara som din webbserver eller databasen Server virtuella datorer körs på ett problem har uppstått, vet du att andra instanser av webbservern och databasen virtuella datorer fortsätter att köras bra eftersom de finns på olika typer av maskinvara.</span><span class="sxs-lookup"><span data-stu-id="eff4e-122">This means that if the physical hardware that one of your Web Server or Database Server VMs is running on has a problem, you know that the other instances of your Web Server and Database VMs will remain running fine because they are on different hardware.</span></span>

<span data-ttu-id="eff4e-123">Du bör alltid använda Tillgänglighetsuppsättningar när du vill distribuera tillförlitliga VM-baserade lösningar i Azure.</span><span class="sxs-lookup"><span data-stu-id="eff4e-123">You should always use Availability Sets when you want to deploy reliable VM-based solutions within Azure.</span></span>


## <a name="create-an-availability-set"></a><span data-ttu-id="eff4e-124">Skapa en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="eff4e-124">Create an availability set</span></span>

<span data-ttu-id="eff4e-125">Du kan skapa en tillgänglighetsuppsättning med [az vm tillgänglighetsuppsättning skapa](/cli/azure/vm/availability-set#create).</span><span class="sxs-lookup"><span data-stu-id="eff4e-125">You can create an availability set using [az vm availability-set create](/cli/azure/vm/availability-set#create).</span></span> <span data-ttu-id="eff4e-126">I detta exempel vi in både antalet domäner för uppdatering och fel på *2* för tillgänglighetsuppsättning namngivna *myAvailabilitySet* i den *myResourceGroupAvailability* resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="eff4e-126">In this example, we set both the number of update and fault domains at *2* for the availability set named *myAvailabilitySet* in the *myResourceGroupAvailability* resource group.</span></span>

<span data-ttu-id="eff4e-127">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="eff4e-127">Create a resource group.</span></span>

```azurecli-interactive 
az group create --name myResourceGroupAvailability --location eastus
```


```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupAvailability \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

<span data-ttu-id="eff4e-128">Tillgänglighetsuppsättningar kan du isolera resurser över ”feldomäner” och ”uppdatera domäner”.</span><span class="sxs-lookup"><span data-stu-id="eff4e-128">Availability Sets allow you to isolate resources across "fault domains" and "update domains".</span></span> <span data-ttu-id="eff4e-129">En **feldomän** representerar isolerade mängd server + nätverk + lagring resurser.</span><span class="sxs-lookup"><span data-stu-id="eff4e-129">A **fault domain** represents an isolated collection of server + network + storage resources.</span></span> <span data-ttu-id="eff4e-130">I föregående exempel anger vi vill våra tillgänglighetsuppsättning ska distribueras under minst två feldomäner när våra virtuella datorer distribueras.</span><span class="sxs-lookup"><span data-stu-id="eff4e-130">In the preceding example, we indicate that we want our availability set to be distributed across at least two fault domains when our VMs are deployed.</span></span> <span data-ttu-id="eff4e-131">Vi också ange att vi vårt tillgänglighetsuppsättning distribuerade mellan två **uppdatera domäner**.</span><span class="sxs-lookup"><span data-stu-id="eff4e-131">We also indicate that we want our availability set distributed across two **update domains**.</span></span>  <span data-ttu-id="eff4e-132">Två update domäner Se till att när Azure utför programuppdateringar våra VM resurser isolerade hindrar alla program som körs under våra VM uppdateras samtidigt.</span><span class="sxs-lookup"><span data-stu-id="eff4e-132">Two update domains ensure that when Azure performs software updates our VM resources are isolated, preventing all the software running underneath our VM from being updated at the same time.</span></span>

## <a name="configure-virtual-network"></a><span data-ttu-id="eff4e-133">Konfigurera virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="eff4e-133">Configure virtual network</span></span>
<span data-ttu-id="eff4e-134">Innan du distribuerar vissa virtuella datorer och testa din belastningsutjämnare, skapa stödresurser för virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="eff4e-134">Before you deploy some VMs and can test your balancer, create the supporting virtual network resources.</span></span> <span data-ttu-id="eff4e-135">Mer information om virtuella nätverk finns i [hantera virtuella Azure-nätverk](tutorial-virtual-network.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="eff4e-135">For more information about virtual networks, see the [Manage Azure Virtual Networks](tutorial-virtual-network.md) tutorial.</span></span>

### <a name="create-network-resources"></a><span data-ttu-id="eff4e-136">Skapa nätverksresurser</span><span class="sxs-lookup"><span data-stu-id="eff4e-136">Create network resources</span></span>
<span data-ttu-id="eff4e-137">Skapa ett virtuellt nätverk med [az network vnet skapa](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="eff4e-137">Create a virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="eff4e-138">I följande exempel skapas ett virtuellt nätverk med namnet *myVnet* med ett undernät med namnet *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="eff4e-138">The following example creates a virtual network named *myVnet* with a subnet named *mySubnet*:</span></span>

```azurecli-interactive 
az network vnet create \
    --resource-group myResourceGroupAvailability \
    --name myVnet \
    --subnet-name mySubnet
```
<span data-ttu-id="eff4e-139">Virtuella nätverkskort skapas med [az nätverket nic skapa](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="eff4e-139">Virtual NICs are created with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="eff4e-140">I följande exempel skapas tre virtuella nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="eff4e-140">The following example creates three virtual NICs.</span></span> <span data-ttu-id="eff4e-141">(Ett virtuellt nätverkskort för varje virtuell dator skapar du en app i följande steg).</span><span class="sxs-lookup"><span data-stu-id="eff4e-141">(One virtual NIC for each VM you create for your app in the following steps).</span></span> <span data-ttu-id="eff4e-142">Du kan skapa ytterligare virtuella nätverkskort och virtuella datorer när som helst och lägga till dem i belastningsutjämnaren:</span><span class="sxs-lookup"><span data-stu-id="eff4e-142">You can create additional virtual NICs and VMs at any time and add them to the load balancer:</span></span>

```bash
for i in `seq 1 3`; do
    az network nic create \
        --resource-group myResourceGroupAvailability \
        --name myNic$i \
        --vnet-name myVnet \
        --subnet mySubnet \
        --lb-name myLoadBalancer \
        --lb-address-pools myBackEndPool
done
```

## <a name="create-vms-inside-an-availability-set"></a><span data-ttu-id="eff4e-143">Skapa virtuella datorer i en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="eff4e-143">Create VMs inside an availability set</span></span>

<span data-ttu-id="eff4e-144">Virtuella datorer måste skapas inom tillgänglighetsuppsättning för att kontrollera att de distribueras på rätt sätt i maskinvaran.</span><span class="sxs-lookup"><span data-stu-id="eff4e-144">VMs must be created within the availability set to make sure they are correctly distributed across the hardware.</span></span> <span data-ttu-id="eff4e-145">Du kan inte lägga till en befintlig virtuell dator till en tillgänglighetsuppsättning när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="eff4e-145">You can't add an existing VM to an availability set after it is created.</span></span> 

<span data-ttu-id="eff4e-146">När du skapar en virtuell dator med hjälp av [az vm skapa](/cli/azure/vm#create) du ange tillgänglighetsuppsättning med hjälp av den `--availability-set` parametern för att ange namnet på tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="eff4e-146">When you create a VM using [az vm create](/cli/azure/vm#create) you specify the availability set using the `--availability-set` parameter to specify the name of the availability set.</span></span>

```azurecli-interactive 
for i in `seq 1 2`; do
   az vm create \
     --resource-group myResourceGroupAvailability \
     --name myVM$i \
     --availability-set myAvailabilitySet \
     --nics myNic$i \
     --size Standard_DS1_v2  \
     --image Canonical:UbuntuServer:14.04.4-LTS:latest \
     --admin-username azureuser \
     --generate-ssh-keys \
     --no-wait
done 
```

<span data-ttu-id="eff4e-147">Nu har vi två virtuella datorer i vår nya tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="eff4e-147">We now have two virtual machines within our newly created availability set.</span></span> <span data-ttu-id="eff4e-148">Eftersom de finns i samma tillgänglighetsuppsättning säkerställer Azure att de virtuella datorerna och deras resurser (inklusive datadiskar) är fördelade på isolerade fysisk maskinvara.</span><span class="sxs-lookup"><span data-stu-id="eff4e-148">Because they are in the same availability set, Azure will ensure that the VMs and all their resources (including data disks) are distributed across isolated physical hardware.</span></span> <span data-ttu-id="eff4e-149">Den här distributionen garanterar mycket högre tillgänglighet av vår övergripande VM-lösningen.</span><span class="sxs-lookup"><span data-stu-id="eff4e-149">This distribution helps ensure much higher availability of our overall VM solution.</span></span>

<span data-ttu-id="eff4e-150">En sak som du kan stöta på när du lägger till virtuella datorer är en viss VM-storlek är inte längre tillgänglig för användning i din tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="eff4e-150">One thing you may encounter as you add VMs is that a particular VM size is no longer available to use within your availability set.</span></span> <span data-ttu-id="eff4e-151">Det här problemet kan inträffa om det finns inte längre tillräckligt med kapacitet för att lägga till den samtidigt som isolering reglerna tillgänglighetsuppsättningen tillämpar.</span><span class="sxs-lookup"><span data-stu-id="eff4e-151">This issue can happen if there is no longer enough capacity to add it while preserving the isolation rules the availability set enforces.</span></span> <span data-ttu-id="eff4e-152">Du kan kontrollera vilka VM-storlekar är tillgängliga för användning i en befintlig tillgänglighetsuppsättning med hjälp av den `--availability-set list-sizes` parameter.</span><span class="sxs-lookup"><span data-stu-id="eff4e-152">You can check to see what VM sizes are available to use within an existing availability set using the `--availability-set list-sizes` parameter.</span></span>

## <a name="check-for-available-vm-sizes"></a><span data-ttu-id="eff4e-153">Sök efter tillgängliga storlekar på VM</span><span class="sxs-lookup"><span data-stu-id="eff4e-153">Check for available VM sizes</span></span> 

<span data-ttu-id="eff4e-154">Du kan lägga till flera virtuella datorer tillgänglighetsuppsättning senare, men du behöver veta vilka VM-storlekar är tillgängliga på maskinvaran.</span><span class="sxs-lookup"><span data-stu-id="eff4e-154">You can add more VMs to the availability set later, but you need to know what VM sizes are available on the hardware.</span></span> <span data-ttu-id="eff4e-155">Använd [az vm-tillgänglighetsuppsättning lista över-storlekar](/cli/azure/availability-set#list-sizes) att lista alla tillgängliga storlekar på maskinvaran kluster för tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="eff4e-155">Use [az vm availability-set list-sizes](/cli/azure/availability-set#list-sizes) to list all the available sizes on the hardware cluster for the availability set.</span></span>

```azurecli-interactive 
az vm availability-set list-sizes \
     --resource-group myResourceGroupAvailability \
     --name myAvailabilitySet \
     --output table  
```

## <a name="next-steps"></a><span data-ttu-id="eff4e-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eff4e-156">Next steps</span></span>

<span data-ttu-id="eff4e-157">I den här självstudiekursen lärde du dig att:</span><span class="sxs-lookup"><span data-stu-id="eff4e-157">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="eff4e-158">Skapa en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="eff4e-158">Create an availability set</span></span>
> * <span data-ttu-id="eff4e-159">Skapa en virtuell dator i en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="eff4e-159">Create a VM in an availability set</span></span>
> * <span data-ttu-id="eff4e-160">Kontrollera tillgängliga storlekar på VM</span><span class="sxs-lookup"><span data-stu-id="eff4e-160">Check available VM sizes</span></span>

<span data-ttu-id="eff4e-161">Gå vidare till nästa kurs vill veta mer om virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="eff4e-161">Advance to the next tutorial to learn about virtual machine scale sets.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="eff4e-162">Skapa en VM-skalningsuppsättning</span><span class="sxs-lookup"><span data-stu-id="eff4e-162">Create a VM scale set</span></span>](tutorial-create-vmss.md)

