---
title: "aaaAvailability anger självstudier för Linux virtuella datorer i Azure | Microsoft Docs"
description: "Läs mer om hello Tillgänglighetsuppsättningar för Linux virtuella datorer i Azure."
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
ms.openlocfilehash: 2a91e4a6057180035ec51410d9fffccaca343758
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-availability-sets"></a><span data-ttu-id="c3463-103">Hur toouse tillgänglighetsuppsättningar</span><span class="sxs-lookup"><span data-stu-id="c3463-103">How toouse availability sets</span></span>


<span data-ttu-id="c3463-104">I kursen får du lära dig hur tooincrease hello tillgänglighet och tillförlitlighet för dina lösningar för virtuell dator på Azure med hjälp av en funktion kallad Tillgänglighetsuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="c3463-104">In this tutorial, you will learn how tooincrease hello availability and reliability of your Virtual Machine solutions on Azure using a capability called Availability Sets.</span></span> <span data-ttu-id="c3463-105">Tillgänglighetsuppsättningar se till att hello virtuella datorer som du distribuerar i Azure är fördelade på flera isolerade maskinvara kluster.</span><span class="sxs-lookup"><span data-stu-id="c3463-105">Availability sets ensure that hello VMs you deploy on Azure are distributed across multiple isolated hardware clusters.</span></span> <span data-ttu-id="c3463-106">Detta säkerställer att om ett maskinvaru- eller fel i Azure inträffar, underordnade uppsättning dina virtuella datorer som påverkas och som din lösning ska vara tillgängliga och fungerar ur hello av dina kunder som använder den.</span><span class="sxs-lookup"><span data-stu-id="c3463-106">Doing this ensures that if a hardware or software failure within Azure happens, only a sub-set of your VMs will be impacted and that your overall solution will remain available and operational from hello perspective of your customers using it.</span></span>

<span data-ttu-id="c3463-107">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="c3463-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c3463-108">Skapa en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="c3463-108">Create an availability set</span></span>
> * <span data-ttu-id="c3463-109">Skapa en virtuell dator i en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="c3463-109">Create a VM in an availability set</span></span>
> * <span data-ttu-id="c3463-110">Kontrollera tillgängliga storlekar på VM</span><span class="sxs-lookup"><span data-stu-id="c3463-110">Check available VM sizes</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="c3463-111">Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="c3463-111">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="c3463-112">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="c3463-112">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="c3463-113">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c3463-113">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="availability-set-overview"></a><span data-ttu-id="c3463-114">Översikt över tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="c3463-114">Availability set overview</span></span>

<span data-ttu-id="c3463-115">En Tillgänglighetsuppsättning är en logisk gruppering funktion som du kan använda i Azure tooensure att hello VM resurser som du placerar i den isoleras från varandra när de distribueras i ett Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="c3463-115">An Availability Set is a logical grouping capability that you can use in Azure tooensure that hello VM resources you place within it are isolated from each other when they are deployed within an Azure datacenter.</span></span> <span data-ttu-id="c3463-116">Azure garanterar att hello virtuella datorer du placera inom en Tillgänglighetsuppsättning körs över flera fysiska servrar, beräkna rack, lagringsenheter och nätverksväxlar.</span><span class="sxs-lookup"><span data-stu-id="c3463-116">Azure ensures that hello VMs you place within an Availability Set run across multiple physical servers, compute racks, storage units, and network switches.</span></span> <span data-ttu-id="c3463-117">Detta säkerställer att hello för händelsen maskinvaru- eller programvarufel Azure, påverkas endast en delmängd av dina virtuella datorer, och tillämpningsprogrammet övergripande förblir upp och fortsätta toobe tillgängliga tooyour kunder.</span><span class="sxs-lookup"><span data-stu-id="c3463-117">This ensures that in hello event of a hardware or Azure software failure, only a subset of your VMs will be impacted, and your overall application will stay up and continue toobe available tooyour customers.</span></span> <span data-ttu-id="c3463-118">Med hjälp av Tillgänglighetsuppsättningar är en viktig funktion tooleverage om du vill toobuild tillförlitliga molnlösningar.</span><span class="sxs-lookup"><span data-stu-id="c3463-118">Using Availability Sets is an essential capability tooleverage when you want toobuild reliable cloud solutions.</span></span>

<span data-ttu-id="c3463-119">Nu ska vi titta en typisk VM-baserad lösning där du kan ha 4 frontend-webbservrar och använda 2 backend-VMs som värd för en databas.</span><span class="sxs-lookup"><span data-stu-id="c3463-119">Let’s consider a typical VM-based solution where you might have 4 front-end web servers and use 2 back-end VMs that host a database.</span></span> <span data-ttu-id="c3463-120">Med Azure, du kan toodefine två tillgänglighetsuppsättningar innan du distribuerar dina virtuella datorer: en tillgänglighetsuppsättning för hello ”web”-nivå och en tillgänglighetsuppsättning för hello ”databas” nivå.</span><span class="sxs-lookup"><span data-stu-id="c3463-120">With Azure, you’d want toodefine two availability sets before you deploy your VMs: one availability set for hello “web” tier and one availability set for hello “database” tier.</span></span> <span data-ttu-id="c3463-121">När du skapar en ny virtuell dator kan du ange hello tillgänglighet som en parameter toohello az virtuell dator skapar kommando och Azure automatiskt garanterar att hello virtuella datorer som du skapar inom hello tillgänglig separat uppsättning över flera fysiska maskinvaruresurser.</span><span class="sxs-lookup"><span data-stu-id="c3463-121">When you create a new VM you can then specify hello availability set as a parameter toohello az vm create command, and Azure will automatically ensure that hello VMs you create within hello available set are isolated across multiple physical hardware resources.</span></span> <span data-ttu-id="c3463-122">Det innebär att du vet att hello om hello fysiska maskinvara som din webbserver eller databasen Server virtuella datorer körs på ett problem har uppstått, andra instanser av webbservern och databasen virtuella datorer fortsätter att köras bra eftersom de finns på olika typer av maskinvara.</span><span class="sxs-lookup"><span data-stu-id="c3463-122">This means that if hello physical hardware that one of your Web Server or Database Server VMs is running on has a problem, you know that hello other instances of your Web Server and Database VMs will remain running fine because they are on different hardware.</span></span>

<span data-ttu-id="c3463-123">Du bör alltid använda Tillgänglighetsuppsättningar när du vill toodeploy tillförlitliga VM-baserade lösningar i Azure.</span><span class="sxs-lookup"><span data-stu-id="c3463-123">You should always use Availability Sets when you want toodeploy reliable VM-based solutions within Azure.</span></span>


## <a name="create-an-availability-set"></a><span data-ttu-id="c3463-124">Skapa en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="c3463-124">Create an availability set</span></span>

<span data-ttu-id="c3463-125">Du kan skapa en tillgänglighetsuppsättning med [az vm tillgänglighetsuppsättning skapa](/cli/azure/vm/availability-set#create).</span><span class="sxs-lookup"><span data-stu-id="c3463-125">You can create an availability set using [az vm availability-set create](/cli/azure/vm/availability-set#create).</span></span> <span data-ttu-id="c3463-126">I det här exemplet vi ange båda hello antalet domäner för uppdatering och fel på *2* för hello tillgänglighetsuppsättning namngivna *myAvailabilitySet* i hello *myResourceGroupAvailability*resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="c3463-126">In this example, we set both hello number of update and fault domains at *2* for hello availability set named *myAvailabilitySet* in hello *myResourceGroupAvailability* resource group.</span></span>

<span data-ttu-id="c3463-127">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="c3463-127">Create a resource group.</span></span>

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

<span data-ttu-id="c3463-128">Tillgänglighetsuppsättningar tillåter tooisolate resurser över ”feldomäner” och ”uppdatera domäner”.</span><span class="sxs-lookup"><span data-stu-id="c3463-128">Availability Sets allow you tooisolate resources across "fault domains" and "update domains".</span></span> <span data-ttu-id="c3463-129">En **feldomän** representerar isolerade mängd server + nätverk + lagring resurser.</span><span class="sxs-lookup"><span data-stu-id="c3463-129">A **fault domain** represents an isolated collection of server + network + storage resources.</span></span> <span data-ttu-id="c3463-130">I föregående exempel hello, ange som vi vill vår tillgänglighetsuppsättning toobe fördelade på minst två feldomäner när våra virtuella datorer distribueras.</span><span class="sxs-lookup"><span data-stu-id="c3463-130">In hello preceding example, we indicate that we want our availability set toobe distributed across at least two fault domains when our VMs are deployed.</span></span> <span data-ttu-id="c3463-131">Vi också ange att vi vårt tillgänglighetsuppsättning distribuerade mellan två **uppdatera domäner**.</span><span class="sxs-lookup"><span data-stu-id="c3463-131">We also indicate that we want our availability set distributed across two **update domains**.</span></span>  <span data-ttu-id="c3463-132">Två update domäner för att se till att våra VM-resurser är isolerade hindrar alla hello-program som körs under våra VM uppdateras vid hello när Azure utför uppdateringar samtidigt.</span><span class="sxs-lookup"><span data-stu-id="c3463-132">Two update domains ensure that when Azure performs software updates our VM resources are isolated, preventing all hello software running underneath our VM from being updated at hello same time.</span></span>

## <a name="configure-virtual-network"></a><span data-ttu-id="c3463-133">Konfigurera ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="c3463-133">Configure virtual network</span></span>
<span data-ttu-id="c3463-134">Innan du distribuerar vissa virtuella datorer och testa din belastningsutjämnare, skapa hello stöder nätverksresurser på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c3463-134">Before you deploy some VMs and can test your balancer, create hello supporting virtual network resources.</span></span> <span data-ttu-id="c3463-135">Mer information om virtuella nätverk finns hello [hantera virtuella Azure-nätverk](tutorial-virtual-network.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="c3463-135">For more information about virtual networks, see hello [Manage Azure Virtual Networks](tutorial-virtual-network.md) tutorial.</span></span>

### <a name="create-network-resources"></a><span data-ttu-id="c3463-136">Skapa nätverksresurser</span><span class="sxs-lookup"><span data-stu-id="c3463-136">Create network resources</span></span>
<span data-ttu-id="c3463-137">Skapa ett virtuellt nätverk med [az network vnet skapa](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="c3463-137">Create a virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="c3463-138">hello följande exempel skapas ett virtuellt nätverk med namnet *myVnet* med ett undernät med namnet *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="c3463-138">hello following example creates a virtual network named *myVnet* with a subnet named *mySubnet*:</span></span>

```azurecli-interactive 
az network vnet create \
    --resource-group myResourceGroupAvailability \
    --name myVnet \
    --subnet-name mySubnet
```
<span data-ttu-id="c3463-139">Virtuella nätverkskort skapas med [az nätverket nic skapa](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="c3463-139">Virtual NICs are created with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="c3463-140">hello skapas följande exempel tre virtuella nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="c3463-140">hello following example creates three virtual NICs.</span></span> <span data-ttu-id="c3463-141">(Ett virtuellt nätverkskort för varje virtuell dator som du skapar för din app i hello följande steg).</span><span class="sxs-lookup"><span data-stu-id="c3463-141">(One virtual NIC for each VM you create for your app in hello following steps).</span></span> <span data-ttu-id="c3463-142">Du kan skapa ytterligare virtuella nätverkskort och virtuella datorer när som helst och lägga till dem toohello belastningsutjämnare:</span><span class="sxs-lookup"><span data-stu-id="c3463-142">You can create additional virtual NICs and VMs at any time and add them toohello load balancer:</span></span>

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

## <a name="create-vms-inside-an-availability-set"></a><span data-ttu-id="c3463-143">Skapa virtuella datorer i en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="c3463-143">Create VMs inside an availability set</span></span>

<span data-ttu-id="c3463-144">Virtuella datorer måste skapas inom hello tillgänglighet set toomake till korrekt distribueras de över hello maskinvara.</span><span class="sxs-lookup"><span data-stu-id="c3463-144">VMs must be created within hello availability set toomake sure they are correctly distributed across hello hardware.</span></span> <span data-ttu-id="c3463-145">Du kan inte lägga till en befintlig virtuell dator tooan tillgänglighetsuppsättning när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="c3463-145">You can't add an existing VM tooan availability set after it is created.</span></span> 

<span data-ttu-id="c3463-146">När du skapar en virtuell dator med hjälp av [az vm skapa](/cli/azure/vm#create) du ange hello tillgänglighet anges med hello `--availability-set` toospecify hello på parameternamn hello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="c3463-146">When you create a VM using [az vm create](/cli/azure/vm#create) you specify hello availability set using hello `--availability-set` parameter toospecify hello name of hello availability set.</span></span>

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

<span data-ttu-id="c3463-147">Nu har vi två virtuella datorer i vår nya tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="c3463-147">We now have two virtual machines within our newly created availability set.</span></span> <span data-ttu-id="c3463-148">Eftersom de hello samma tillgänglighetsuppsättning Azure säkerställer att hello virtuella datorer och alla sina resurser (inklusive datadiskar) är fördelade på isolerade fysisk maskinvara.</span><span class="sxs-lookup"><span data-stu-id="c3463-148">Because they are in hello same availability set, Azure will ensure that hello VMs and all their resources (including data disks) are distributed across isolated physical hardware.</span></span> <span data-ttu-id="c3463-149">Den här distributionen garanterar mycket högre tillgänglighet av vår övergripande VM-lösningen.</span><span class="sxs-lookup"><span data-stu-id="c3463-149">This distribution helps ensure much higher availability of our overall VM solution.</span></span>

<span data-ttu-id="c3463-150">En sak som du kan stöta på när du lägger till virtuella datorer är att en viss VM-storlek är inte längre tillgängliga toouse inom din tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="c3463-150">One thing you may encounter as you add VMs is that a particular VM size is no longer available toouse within your availability set.</span></span> <span data-ttu-id="c3463-151">Det här problemet kan inträffa om det finns inte längre tillräckligt med kapacitet tooadd det samtidigt som hello isolering regler hello tillgänglighetsuppsättning tvingar.</span><span class="sxs-lookup"><span data-stu-id="c3463-151">This issue can happen if there is no longer enough capacity tooadd it while preserving hello isolation rules hello availability set enforces.</span></span> <span data-ttu-id="c3463-152">Du kan kontrollera toosee vilka VM-storlekar är tillgängliga toouse i en befintlig tillgänglighetsuppsättning med hello `--availability-set list-sizes` parameter.</span><span class="sxs-lookup"><span data-stu-id="c3463-152">You can check toosee what VM sizes are available toouse within an existing availability set using hello `--availability-set list-sizes` parameter.</span></span>

## <a name="check-for-available-vm-sizes"></a><span data-ttu-id="c3463-153">Sök efter tillgängliga storlekar på VM</span><span class="sxs-lookup"><span data-stu-id="c3463-153">Check for available VM sizes</span></span> 

<span data-ttu-id="c3463-154">Du kan lägga till flera virtuella datorer toohello tillgänglighetsuppsättning senare, men du måste tooknow vilka VM-storlekar är tillgängliga på hello maskinvara.</span><span class="sxs-lookup"><span data-stu-id="c3463-154">You can add more VMs toohello availability set later, but you need tooknow what VM sizes are available on hello hardware.</span></span> <span data-ttu-id="c3463-155">Använd [az vm-tillgänglighetsuppsättning lista över-storlekar](/cli/azure/availability-set#list-sizes) toolist alla hello tillgängliga storlekar på hello maskinvara kluster för hello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="c3463-155">Use [az vm availability-set list-sizes](/cli/azure/availability-set#list-sizes) toolist all hello available sizes on hello hardware cluster for hello availability set.</span></span>

```azurecli-interactive 
az vm availability-set list-sizes \
     --resource-group myResourceGroupAvailability \
     --name myAvailabilitySet \
     --output table  
```

## <a name="next-steps"></a><span data-ttu-id="c3463-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c3463-156">Next steps</span></span>

<span data-ttu-id="c3463-157">I den här självstudiekursen lärde du dig att:</span><span class="sxs-lookup"><span data-stu-id="c3463-157">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c3463-158">Skapa en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="c3463-158">Create an availability set</span></span>
> * <span data-ttu-id="c3463-159">Skapa en virtuell dator i en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="c3463-159">Create a VM in an availability set</span></span>
> * <span data-ttu-id="c3463-160">Kontrollera tillgängliga storlekar på VM</span><span class="sxs-lookup"><span data-stu-id="c3463-160">Check available VM sizes</span></span>

<span data-ttu-id="c3463-161">Avancera toohello nästa självstudiekurs toolearn om skalningsuppsättningar i virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c3463-161">Advance toohello next tutorial toolearn about virtual machine scale sets.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c3463-162">Skapa en VM-skalningsuppsättning</span><span class="sxs-lookup"><span data-stu-id="c3463-162">Create a VM scale set</span></span>](tutorial-create-vmss.md)

