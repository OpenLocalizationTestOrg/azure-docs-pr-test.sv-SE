---
title: "ett virtuellt Azure-nätverk peering - aaaCreate Resource Manager - samma prenumeration | Microsoft Docs"
description: "Lär dig hur toocreate ett virtuellt nätverk peering mellan virtuella nätverk skapas via Resource Manager som finns i hello samma Azure-prenumeration."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 026bca75-2946-4c03-b4f6-9f3c5809c69a
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: jdial;narayan;annahar
ms.openlocfilehash: c2d24fdc8103c09c3bfb8e59be12e301d9e9a55a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-peering---resource-manager-same-subscription"></a><span data-ttu-id="f5176-103">Skapa ett virtuellt nätverk peering - hanteraren för filserverresurser, samma prenumeration</span><span class="sxs-lookup"><span data-stu-id="f5176-103">Create a virtual network peering - Resource Manager, same subscription</span></span>

<span data-ttu-id="f5176-104">I kursen får du lära dig toocreate ett virtuellt nätverk peering mellan virtuella nätverk som skapats via Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="f5176-104">In this tutorial, you learn toocreate a virtual network peering between virtual networks created through Resource Manager.</span></span> <span data-ttu-id="f5176-105">Båda virtuella nätverk finns i hello samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f5176-105">Both virtual networks exist in hello same subscription.</span></span> <span data-ttu-id="f5176-106">Peering två virtuella nätverk gör resurser i olika virtuella nätverk toocommunicate med varandra med hello samma bandbredd och svarstid som om hello resurserna var i hello samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="f5176-106">Peering two virtual networks enables resources in different virtual networks toocommunicate with each other with hello same bandwidth and latency as though hello resources were in hello same virtual network.</span></span> <span data-ttu-id="f5176-107">Lär dig mer om [virtuella nätverk peering](virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f5176-107">Learn more about [Virtual network peering](virtual-network-peering-overview.md).</span></span> 

<span data-ttu-id="f5176-108">hello steg toocreate ett virtuellt nätverk som peering är olika, beroende på om hello virtuella nätverk är i hello samma eller olika prenumerationer och som [Azure distributionsmodell](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello virtuella nätverk skapas via.</span><span class="sxs-lookup"><span data-stu-id="f5176-108">hello steps toocreate a virtual network peering are different, depending on whether hello virtual networks are in hello same, or different, subscriptions, and which [Azure deployment model](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello virtual networks are created through.</span></span> <span data-ttu-id="f5176-109">Lär dig hur toocreate ett virtuellt nätverk peering i andra scenarier genom att klicka på hello scenariot från hello följande tabell:</span><span class="sxs-lookup"><span data-stu-id="f5176-109">Learn how toocreate a virtual network peering in other scenarios by clicking hello scenario from hello following table:</span></span>

|<span data-ttu-id="f5176-110">Azure-distributionsmodell</span><span class="sxs-lookup"><span data-stu-id="f5176-110">Azure deployment model</span></span>  | <span data-ttu-id="f5176-111">Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="f5176-111">Azure subscription</span></span>  |
|--------- |---------|
|[<span data-ttu-id="f5176-112">Båda Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f5176-112">Both Resource Manager</span></span>](create-peering-different-subscriptions.md) |<span data-ttu-id="f5176-113">Olika</span><span class="sxs-lookup"><span data-stu-id="f5176-113">Different</span></span>|
|[<span data-ttu-id="f5176-114">En Resource Manager, en klassisk</span><span class="sxs-lookup"><span data-stu-id="f5176-114">One Resource Manager, one classic</span></span>](create-peering-different-deployment-models.md) |<span data-ttu-id="f5176-115">samma</span><span class="sxs-lookup"><span data-stu-id="f5176-115">Same</span></span>|
|[<span data-ttu-id="f5176-116">En Resource Manager, en klassisk</span><span class="sxs-lookup"><span data-stu-id="f5176-116">One Resource Manager, one classic</span></span>](create-peering-different-deployment-models-subscriptions.md) |<span data-ttu-id="f5176-117">Olika</span><span class="sxs-lookup"><span data-stu-id="f5176-117">Different</span></span>|

<span data-ttu-id="f5176-118">Att går inte skapa ett virtuellt nätverk som peering mellan två virtuella nätverk som distribuerats via hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="f5176-118">A virtual network peering cannot be created between two virtual networks deployed through hello classic deployment model.</span></span> <span data-ttu-id="f5176-119">Ett virtuellt nätverk som peering kan bara skapas mellan två virtuella nätverk som finns i hello samma Azure-region.</span><span class="sxs-lookup"><span data-stu-id="f5176-119">A virtual network peering can only be created between two virtual networks that exist in hello same Azure region.</span></span> <span data-ttu-id="f5176-120">Om du behöver tooconnect virtuella nätverk som skapats både via hello klassiska distributionsmodellen eller som finns i olika Azure-regioner kan du använda en Azure [VPN-Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tooconnect hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="f5176-120">If you need tooconnect virtual networks that were both created through hello classic deployment model, or that exist in different Azure regions, you can use an Azure [VPN Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tooconnect hello virtual networks.</span></span> 

<span data-ttu-id="f5176-121">Du kan använda hello [Azure-portalen](#portal), hello Azure [kommandoradsgränssnittet](#cli) (CLI) Azure [PowerShell](#powershell), eller en [Azure Resource Manager-mall](#template) toocreate peering ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f5176-121">You can use hello [Azure portal](#portal), hello Azure [command-line interface](#cli) (CLI), Azure [PowerShell](#powershell), or an [Azure Resource Manager template](#template) toocreate a virtual network peering.</span></span> <span data-ttu-id="f5176-122">Klicka på någon av hello tidigare verktyget länkar toogo direkt toohello steg för att skapa ett virtuellt nätverk peering verktyget dina val.</span><span class="sxs-lookup"><span data-stu-id="f5176-122">Click any of hello previous tool links toogo directly toohello steps for creating a virtual network peering using your tool of choice.</span></span>

## <span data-ttu-id="f5176-123"><a name="portal"></a>Skapa peering - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f5176-123"><a name="portal"></a>Create peering - Azure portal</span></span>

1. <span data-ttu-id="f5176-124">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f5176-124">Log in toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="f5176-125">hello-konto som du loggar in med måste ha hello behörighet toocreate peering ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f5176-125">hello account you log in with must have hello necessary permissions toocreate a virtual network peering.</span></span> <span data-ttu-id="f5176-126">Se hello [behörigheter](#permissions) i den här artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="f5176-126">See hello [Permissions](#permissions) section of this article for details.</span></span>
2. <span data-ttu-id="f5176-127">Klicka på **+ ny**, klickar du på **nätverk**, klicka på **för virtuella nätverk**.</span><span class="sxs-lookup"><span data-stu-id="f5176-127">Click **+ New**, click **Networking**, then click **Virtual network**.</span></span>
3. <span data-ttu-id="f5176-128">I hello **skapa virtuellt nätverk** bladet anger, eller Välj värden för hello följande inställningar och sedan klickar du på **skapa**:</span><span class="sxs-lookup"><span data-stu-id="f5176-128">In hello **Create virtual network** blade, enter, or select values for hello following settings, then click **Create**:</span></span>
    - <span data-ttu-id="f5176-129">**Namnet**: *myVnet1*</span><span class="sxs-lookup"><span data-stu-id="f5176-129">**Name**: *myVnet1*</span></span>
    - <span data-ttu-id="f5176-130">**Adressutrymmet**: *10.0.0.0/16*</span><span class="sxs-lookup"><span data-stu-id="f5176-130">**Address space**: *10.0.0.0/16*</span></span>
    - <span data-ttu-id="f5176-131">**Undernätnamnet**: *standard*</span><span class="sxs-lookup"><span data-stu-id="f5176-131">**Subnet name**: *default*</span></span>
    - <span data-ttu-id="f5176-132">**Adressintervall för gatewayundernät**: *10.0.0.0/24*</span><span class="sxs-lookup"><span data-stu-id="f5176-132">**Subnet address range**: *10.0.0.0/24*</span></span>
    - <span data-ttu-id="f5176-133">**Prenumerationen**: väljer din prenumeration</span><span class="sxs-lookup"><span data-stu-id="f5176-133">**Subscription**: Select your subscription</span></span>
    - <span data-ttu-id="f5176-134">**Resursgruppen**: Välj **Skapa nytt** och ange *myResourceGroup*</span><span class="sxs-lookup"><span data-stu-id="f5176-134">**Resource group**: Select **Create new** and enter *myResourceGroup*</span></span>
    - <span data-ttu-id="f5176-135">**Plats**: *östra USA*</span><span class="sxs-lookup"><span data-stu-id="f5176-135">**Location**: *East US*</span></span>
4. <span data-ttu-id="f5176-136">Slutför steg 2 – 3 igen att ange hello följande värden i steg 3:</span><span class="sxs-lookup"><span data-stu-id="f5176-136">Complete steps 2-3 again specifying hello following values in step 3:</span></span>
    - <span data-ttu-id="f5176-137">**Namnet**: *myVnet2*</span><span class="sxs-lookup"><span data-stu-id="f5176-137">**Name**: *myVnet2*</span></span>
    - <span data-ttu-id="f5176-138">**Adressutrymmet**: *10.1.0.0/16*</span><span class="sxs-lookup"><span data-stu-id="f5176-138">**Address space**: *10.1.0.0/16*</span></span>
    - <span data-ttu-id="f5176-139">**Undernätnamnet**: *standard*</span><span class="sxs-lookup"><span data-stu-id="f5176-139">**Subnet name**: *default*</span></span>
    - <span data-ttu-id="f5176-140">**Adressintervall för gatewayundernät**: *10.1.0.0/24*</span><span class="sxs-lookup"><span data-stu-id="f5176-140">**Subnet address range**: *10.1.0.0/24*</span></span>
    - <span data-ttu-id="f5176-141">**Prenumerationen**: väljer din prenumeration</span><span class="sxs-lookup"><span data-stu-id="f5176-141">**Subscription**: Select your subscription</span></span>
    - <span data-ttu-id="f5176-142">**Resursgruppen**: Välj **använda befintliga** och välj *myResourceGroup*</span><span class="sxs-lookup"><span data-stu-id="f5176-142">**Resource group**: Select **Use existing** and select *myResourceGroup*</span></span>
    - <span data-ttu-id="f5176-143">**Plats**: *östra USA*</span><span class="sxs-lookup"><span data-stu-id="f5176-143">**Location**: *East US*</span></span>
5. <span data-ttu-id="f5176-144">I hello **söka resurser** rutan hello överst i hello portal, typen *myResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="f5176-144">In hello **Search resources** box at hello top of hello portal, type *myResourceGroup*.</span></span> <span data-ttu-id="f5176-145">Klicka på **myResourceGroup** när den visas i sökresultaten hello.</span><span class="sxs-lookup"><span data-stu-id="f5176-145">Click **myResourceGroup** when it appears in hello search results.</span></span> <span data-ttu-id="f5176-146">Ett blad som visas för hello **myresourcegroup** resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="f5176-146">A blade appears for hello **myresourcegroup** resource group.</span></span> <span data-ttu-id="f5176-147">hello resursgruppen innehåller hello två virtuella nätverk skapas i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="f5176-147">hello resource group contains hello two virtual networks created in previous steps.</span></span>
6. <span data-ttu-id="f5176-148">Klicka på **myVNet1**.</span><span class="sxs-lookup"><span data-stu-id="f5176-148">Click **myVNet1**.</span></span>
7. <span data-ttu-id="f5176-149">I hello **myVnet1** bladet som visas, klickar du på **Peerkopplingar** hello lodräta listan med alternativ på hello vänster sida av hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="f5176-149">In hello **myVnet1** blade that appears, click **Peerings** from hello vertical list of options on hello left side of hello blade.</span></span>
8. <span data-ttu-id="f5176-150">I hello **myVnet1 - Peerkopplingar** bladet som visas, klickar du på **+ Lägg till**</span><span class="sxs-lookup"><span data-stu-id="f5176-150">In hello **myVnet1 - Peerings** blade that appeared, click **+ Add**</span></span>
9. <span data-ttu-id="f5176-151">I hello **Lägg till peering** bladet som visas anger, eller välj hello följande alternativ och klicka på **OK**:</span><span class="sxs-lookup"><span data-stu-id="f5176-151">In hello **Add peering** blade that appears, enter, or select hello following options, then click **OK**:</span></span>
     - <span data-ttu-id="f5176-152">**Namnet**: *myVnet1ToMyVnet2*</span><span class="sxs-lookup"><span data-stu-id="f5176-152">**Name**: *myVnet1ToMyVnet2*</span></span>
     - <span data-ttu-id="f5176-153">**Virtuellt nätverk distributionsmodell**: Välj **Resource Manager**.</span><span class="sxs-lookup"><span data-stu-id="f5176-153">**Virtual network deployment model**:  Select **Resource Manager**.</span></span> 
     - <span data-ttu-id="f5176-154">**Prenumerationen**: väljer din prenumeration</span><span class="sxs-lookup"><span data-stu-id="f5176-154">**Subscription**: Select your subscription</span></span>
     - <span data-ttu-id="f5176-155">**Virtuellt nätverk**: Klicka på **Välj ett virtuellt nätverk**, klicka på **myVnet2**.</span><span class="sxs-lookup"><span data-stu-id="f5176-155">**Virtual network**:  Click **Choose a virtual network**, then click **myVnet2**.</span></span>
     - <span data-ttu-id="f5176-156">**Tillåt åtkomst till virtuella nätverk:** se till att **aktiverad** är markerad.</span><span class="sxs-lookup"><span data-stu-id="f5176-156">**Allow virtual network access:** Ensure that **Enabled** is selected.</span></span>
    <span data-ttu-id="f5176-157">Inga andra inställningar används i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="f5176-157">No other settings are used in this tutorial.</span></span> <span data-ttu-id="f5176-158">Läs toolearn om inställningar för alla peering [hantera peerkopplingar mellan virtuella nätverk](virtual-network-manage-peering.md#create-a-peering).</span><span class="sxs-lookup"><span data-stu-id="f5176-158">toolearn about all peering settings, read [Manage virtual network peerings](virtual-network-manage-peering.md#create-a-peering).</span></span>
10. <span data-ttu-id="f5176-159">När du klickar på **OK** i föregående steg hello hello **Lägg till peering** blad stängs och du ser hello **myVnet1 - Peerkopplingar** bladet igen.</span><span class="sxs-lookup"><span data-stu-id="f5176-159">After clicking **OK** in hello previous step, hello **Add peering** blade closes and you see hello **myVnet1 - Peerings** blade again.</span></span> <span data-ttu-id="f5176-160">Efter några sekunder visas hello peering du skapade i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="f5176-160">After a few seconds, hello peering you created appears in hello blade.</span></span> <span data-ttu-id="f5176-161">**Initierade** visas i hello **PEERING STATUS** för hello **myVnet1ToMyVnet2** peering du skapat.</span><span class="sxs-lookup"><span data-stu-id="f5176-161">**Initiated** is listed in hello **PEERING STATUS** column for hello **myVnet1ToMyVnet2** peering you created.</span></span> <span data-ttu-id="f5176-162">Du har peerkoppla Vnet1 tooVnet2, men du måste nu peer-myVnet2 toomyVnet1.</span><span class="sxs-lookup"><span data-stu-id="f5176-162">You've peered Vnet1 tooVnet2, but now you must peer myVnet2 toomyVnet1.</span></span> <span data-ttu-id="f5176-163">Hej peering måste skapas i båda riktningarna tooenable resurser i hello virtuella nätverk toocommunicate med varandra.</span><span class="sxs-lookup"><span data-stu-id="f5176-163">hello peering must be created in both directions tooenable resources in hello virtual networks toocommunicate with each other.</span></span>
11. <span data-ttu-id="f5176-164">Slutför steg 5 – 10 igen för myVnet2.</span><span class="sxs-lookup"><span data-stu-id="f5176-164">Complete steps 5-10 again for myVnet2.</span></span>  <span data-ttu-id="f5176-165">Namnet hello peering *myVnet2ToMyVnet1*.</span><span class="sxs-lookup"><span data-stu-id="f5176-165">Name hello peering *myVnet2ToMyVnet1*.</span></span>
12. <span data-ttu-id="f5176-166">Några sekunder när du klickar på **OK** toocreate hello-peering för MyVnet2, hello **myVnet2ToMyVnet1** peering du precis har skapat visas med **ansluten** i hello  **PEERING STATUS** kolumn.</span><span class="sxs-lookup"><span data-stu-id="f5176-166">A few seconds after clicking **OK** toocreate hello peering for MyVnet2, hello **myVnet2ToMyVnet1** peering you just created is listed with **Connected** in hello **PEERING STATUS** column.</span></span>
13. <span data-ttu-id="f5176-167">Slutför steg 5 – 7 igen för MyVnet1.</span><span class="sxs-lookup"><span data-stu-id="f5176-167">Complete steps 5-7 again for MyVnet1.</span></span> <span data-ttu-id="f5176-168">Hej **PEERING STATUS** för hello **myVnet1ToVNet2** peering är nu också **ansluten**.</span><span class="sxs-lookup"><span data-stu-id="f5176-168">hello **PEERING STATUS** for hello **myVnet1ToVNet2** peering is now also **Connected**.</span></span> <span data-ttu-id="f5176-169">Hej peering etablerats efter att du ser **ansluten** i hello **PEERING STATUS** för både virtuella nätverk i hello peering.</span><span class="sxs-lookup"><span data-stu-id="f5176-169">hello peering is successfully established after you see **Connected** in hello **PEERING STATUS** column for both virtual networks in hello peering.</span></span>
14. <span data-ttu-id="f5176-170">**Valfria**: även om att skapa virtuella datorer inte ingår i den här självstudiekursen, du kan skapa en virtuell dator i varje virtuellt nätverk och ansluta från en virtuell dator toohello andra toovalidate anslutning.</span><span class="sxs-lookup"><span data-stu-id="f5176-170">**Optional**: Though creating virtual machines is not covered in this tutorial, you can create a virtual machine in each virtual network and connect from one virtual machine toohello other, toovalidate connectivity.</span></span>
15. <span data-ttu-id="f5176-171">**Valfria**: toodelete hello resurser som du skapar i den här självstudiekursen, fullständig hello stegen i hello [bort resurser](#delete-portal) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f5176-171">**Optional**: toodelete hello resources that you create in this tutorial, complete hello steps in hello [Delete resources](#delete-portal) section of this article.</span></span>

<span data-ttu-id="f5176-172">Azure-resurser som du skapar i antingen virtuellt nätverk är nu kan toocommunicate med varandra via sina IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="f5176-172">Any Azure resources you create in either virtual network are now able toocommunicate with each other through their IP addresses.</span></span> <span data-ttu-id="f5176-173">Om du använder standard-Azure namnmatchning för virtuella nätverk för hello hello resurser i hello virtuella nätverk är inte kan tooresolve namn över hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="f5176-173">If you're using default Azure name resolution for hello virtual networks, hello resources in hello virtual networks are not able tooresolve names across hello virtual networks.</span></span> <span data-ttu-id="f5176-174">Om du vill tooresolve namn över virtuella nätverk i en peering måste du skapa DNS-servern.</span><span class="sxs-lookup"><span data-stu-id="f5176-174">If you want tooresolve names across virtual networks in a peering, you must create your own DNS server.</span></span> <span data-ttu-id="f5176-175">Lär dig hur tooset in [namnmatchning med hjälp av DNS-servern](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span><span class="sxs-lookup"><span data-stu-id="f5176-175">Learn how tooset up [Name resolution using your own DNS server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span></span>

## <span data-ttu-id="f5176-176"><a name="cli"></a>Skapa peering - Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f5176-176"><a name="cli"></a>Create peering - Azure CLI</span></span>

<span data-ttu-id="f5176-177">Hej följande skript:</span><span class="sxs-lookup"><span data-stu-id="f5176-177">hello following script:</span></span>

- <span data-ttu-id="f5176-178">Kräver hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="f5176-178">Requires hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="f5176-179">toofind hello version, kör hello `az --version` kommando.</span><span class="sxs-lookup"><span data-stu-id="f5176-179">toofind hello version, run hello `az --version` command.</span></span> <span data-ttu-id="f5176-180">Om du behöver tooupgrade finns [installera Azure CLI 2.0]( /cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f5176-180">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
- <span data-ttu-id="f5176-181">Fungerar i ett Bash-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="f5176-181">Works in a Bash shell.</span></span> <span data-ttu-id="f5176-182">Alternativen på Azure CLI skriptkörning på Windows-klient kan se [kör Windows hello Azure CLI](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f5176-182">For options on running Azure CLI scripts on Windows client, see [Running hello Azure CLI in Windows](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span> 

<span data-ttu-id="f5176-183">Du kan använda hello Azure Cloud Shell istället för att installera hello CLI och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="f5176-183">Instead of installing hello CLI and its dependencies, you can use hello Azure Cloud Shell.</span></span> <span data-ttu-id="f5176-184">hello Azure Cloud-gränssnittet är ett kostnadsfritt Bash-gränssnitt som du kan köra direkt i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f5176-184">hello Azure Cloud Shell is a free Bash shell that you can run directly within hello Azure portal.</span></span> <span data-ttu-id="f5176-185">Den har hello Azure CLI förinstallerat och konfigurerats toouse med ditt konto.</span><span class="sxs-lookup"><span data-stu-id="f5176-185">It has hello Azure CLI preinstalled and configured toouse with your account.</span></span> <span data-ttu-id="f5176-186">Klicka på hello **prova** knappen i hello-skript som anropar ett moln-gränssnitt som loggar du kan logga in tooyour Azure-konto med.</span><span class="sxs-lookup"><span data-stu-id="f5176-186">Click hello **Try it** button in hello script that follows, which invokes a Cloud Shell that logs you can log in tooyour Azure account with.</span></span> <span data-ttu-id="f5176-187">tooexecute hello skript klickar du på hello **kopiera** knappen och klistra in hello innehållet i molnet-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="f5176-187">tooexecute hello script, click hello **Copy** button and paste hello contents into your Cloud Shell.</span></span>

1. <span data-ttu-id="f5176-188">Skapa en resursgrupp och två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="f5176-188">Create a resource group and two virtual networks.</span></span>

    ```azurecli-interactive
    #!/bin/bash

    # Variables for common values used throughout hello script.
    rgName="myResourceGroup"
    location="eastus"

    # Create a resource group.
    az group create \
      --name $rgName \
      --location $location

    # Create virtual network 1.
    az network vnet create \
      --name myVnet1 \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.0.0.0/16

    # Create virtual network 2.
    az network vnet create \
      --name myVnet2 \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.1.0.0/16
    ```

2. <span data-ttu-id="f5176-189">Skapa ett virtuellt nätverk peering mellan hello två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="f5176-189">Create a virtual network peering between hello two virtual networks.</span></span>
 
    ```azurecli-interactive
    # Get hello id for VNet1.
    vnet1Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet1 \
      --query id --out tsv)

    # Get hello id for VNet2.
    vnet2Id=$(az network vnet show \
      --resource-group $rgName \
      --name myVnet2 \
      --query id \
      --out tsv)

    # Peer VNet1 tooVNet2.
    az network vnet peering create \
      --name myVnet1ToMyVnet2 \
      --resource-group $rgName \
      --vnet-name myVnet1 \
      --remote-vnet-id $vnet2Id \
      --allow-vnet-access

    # Peer VNet2 tooVNet1.
    az network vnet peering create \
      --name myVnet2ToMyVnet1 \
      --resource-group $rgName \
      --vnet-name myVnet2 \
      --remote-vnet-id $vnet1Id \
      --allow-vnet-access
    ```

3. <span data-ttu-id="f5176-190">När hello skriptet körs, granska hello peerkopplingar för varje virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f5176-190">After hello script executes, review hello peerings for each virtual network.</span></span> 

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --output table
    ```
    
    <span data-ttu-id="f5176-191">Kör hello föregående kommando igen, ersätter *myVnet1* med *myVnet2*.</span><span class="sxs-lookup"><span data-stu-id="f5176-191">Run hello previous command again, replacing *myVnet1* with *myVnet2*.</span></span> <span data-ttu-id="f5176-192">hello utdata från både kommandon visar **ansluten** i hello **PeeringState** kolumn.</span><span class="sxs-lookup"><span data-stu-id="f5176-192">hello output of both commands shows **Connected** in hello **PeeringState** column.</span></span>

     <span data-ttu-id="f5176-193">Azure-resurser som du skapar i antingen virtuellt nätverk är nu kan toocommunicate med varandra via sina IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="f5176-193">Any Azure resources you create in either virtual network are now able toocommunicate with each other through their IP addresses.</span></span> <span data-ttu-id="f5176-194">Om du använder standard-Azure namnmatchning för virtuella nätverk för hello hello resurser i hello virtuella nätverk är inte kan tooresolve namn över hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="f5176-194">If you're using default Azure name resolution for hello virtual networks, hello resources in hello virtual networks are not able tooresolve names across hello virtual networks.</span></span> <span data-ttu-id="f5176-195">Om du vill tooresolve namn över virtuella nätverk i en peering måste du skapa DNS-servern.</span><span class="sxs-lookup"><span data-stu-id="f5176-195">If you want tooresolve names across virtual networks in a peering, you must create your own DNS server.</span></span> <span data-ttu-id="f5176-196">Lär dig hur tooset in [namnmatchning med hjälp av DNS-servern](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span><span class="sxs-lookup"><span data-stu-id="f5176-196">Learn how tooset up [Name resolution using your own DNS server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span></span>

4. <span data-ttu-id="f5176-197">**Valfria**: även om att skapa virtuella datorer inte ingår i den här självstudiekursen, du kan skapa en virtuell dator i varje virtuellt nätverk och ansluta från en virtuell dator toohello andra toovalidate anslutning.</span><span class="sxs-lookup"><span data-stu-id="f5176-197">**Optional**: Though creating virtual machines is not covered in this tutorial, you can create a virtual machine in each virtual network and connect from one virtual machine toohello other, toovalidate connectivity.</span></span>
5. <span data-ttu-id="f5176-198">**Valfria**: toodelete hello resurser som du skapar i den här självstudiekursen, fullständig hello stegen i [bort resurser](#delete-cli) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f5176-198">**Optional**: toodelete hello resources that you create in this tutorial, complete hello steps in [Delete resources](#delete-cli) in this article.</span></span>


## <span data-ttu-id="f5176-199"><a name="powershell"></a>Skapa peering - PowerShell</span><span class="sxs-lookup"><span data-stu-id="f5176-199"><a name="powershell"></a>Create peering - PowerShell</span></span>

1. <span data-ttu-id="f5176-200">Installera hello senaste versionen av hello PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modul.</span><span class="sxs-lookup"><span data-stu-id="f5176-200">Install hello latest version of hello PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) module.</span></span> <span data-ttu-id="f5176-201">Om du är ny tooAzure PowerShell Se [översikt över Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f5176-201">If you're new tooAzure PowerShell, see [Azure PowerShell overview](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
2. <span data-ttu-id="f5176-202">Gå toostart en PowerShell-session för**starta**, ange **powershell**, och klicka sedan på **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="f5176-202">toostart a PowerShell session, go too**Start**, enter **powershell**, and then click **PowerShell**.</span></span>
3. <span data-ttu-id="f5176-203">Logga in tooAzure i PowerShell genom att ange hello `login-azurermaccount` kommando.</span><span class="sxs-lookup"><span data-stu-id="f5176-203">In PowerShell, log in tooAzure by entering hello `login-azurermaccount` command.</span></span> <span data-ttu-id="f5176-204">hello-konto som du loggar in med måste ha hello behörighet toocreate peering ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f5176-204">hello account you log in with must have hello necessary permissions toocreate a virtual network peering.</span></span> <span data-ttu-id="f5176-205">Se hello [behörigheter](#permissions) i den här artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="f5176-205">See hello [Permissions](#permissions) section of this article for details.</span></span>
4. <span data-ttu-id="f5176-206">Skapa en resursgrupp och två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="f5176-206">Create a resource group and two virtual networks.</span></span> <span data-ttu-id="f5176-207">tooexecute hello skriptet kopiera hello följande skript, klistra in den i PowerShell och tryck sedan på `Enter` när hello sista raden visas på hello-skärmen:</span><span class="sxs-lookup"><span data-stu-id="f5176-207">tooexecute hello script, copy hello following script, paste it into PowerShell, and then press `Enter` after hello last line appears on hello screen:</span></span>

    ```powershell
    # Variables for common values used throughout hello script.
    $rgName='myResourceGroup'
    $location='eastus'

    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name $rgName `
      -Location $location

    # Create virtual network 1.
    $vnet1 = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnet1' `
      -AddressPrefix '10.0.0.0/16' `
      -Location $location

    # Create virtual network 2.
    $vnet2 = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnet2' `
      -AddressPrefix '10.1.0.0/16' `
      -Location $location
    ```

5. <span data-ttu-id="f5176-208">Skapa ett virtuellt nätverk peering mellan hello två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="f5176-208">Create a virtual network peering between hello two virtual networks.</span></span> <span data-ttu-id="f5176-209">Kopiera hello följande skript, klistra in i tooPowerShell och tryck sedan på `Enter` när hello sista raden visas på hello-skärmen:</span><span class="sxs-lookup"><span data-stu-id="f5176-209">Copy hello following script, paste in tooPowerShell, and then press `Enter` after hello last line appears on hello screen:</span></span>
    ```powershell
    # Peer VNet1 tooVNet2.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet1ToMyVnet2' `
      -VirtualNetwork $vnet1 `
      -RemoteVirtualNetworkId $vnet2.Id

    # Peer VNet2 tooVNet1.
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnet2ToMyVnet1' `
      -VirtualNetwork $vnet2 `
      -RemoteVirtualNetworkId $vnet1.Id
    ```
6. <span data-ttu-id="f5176-210">tooreview hello undernät för virtuellt nätverk för hello, kopiera hello följande kommando, klistra in i tooPowerShell och tryck sedan på `Enter`:</span><span class="sxs-lookup"><span data-stu-id="f5176-210">tooreview hello subnets for hello virtual network, copy hello following command, paste in tooPowerShell, and then press `Enter`:</span></span>

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    <span data-ttu-id="f5176-211">Kör hello föregående kommando igen, ersätter *myVnet1* med *myVnet2*.</span><span class="sxs-lookup"><span data-stu-id="f5176-211">Run hello previous command again, replacing *myVnet1* with *myVnet2*.</span></span> <span data-ttu-id="f5176-212">hello utdata från både kommandon visar **ansluten** i hello **PeeringState** kolumn.</span><span class="sxs-lookup"><span data-stu-id="f5176-212">hello output of both commands shows **Connected** in hello **PeeringState** column.</span></span>
7. <span data-ttu-id="f5176-213">**Valfria**: även om att skapa virtuella datorer inte ingår i den här självstudiekursen, du kan skapa en virtuell dator i varje virtuellt nätverk och ansluta från en virtuell dator toohello andra toovalidate anslutning.</span><span class="sxs-lookup"><span data-stu-id="f5176-213">**Optional**: Though creating virtual machines is not covered in this tutorial, you can create a virtual machine in each virtual network and connect from one virtual machine toohello other, toovalidate connectivity.</span></span>
8. <span data-ttu-id="f5176-214">**Valfria**: toodelete hello resurser som du skapar i den här självstudiekursen, fullständig hello stegen i [bort resurser](#delete-powershell) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f5176-214">**Optional**: toodelete hello resources that you create in this tutorial, complete hello steps in [Delete resources](#delete-powershell) in this article.</span></span>

<span data-ttu-id="f5176-215">Azure-resurser som du skapar i antingen virtuellt nätverk är nu kan toocommunicate med varandra via sina IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="f5176-215">Any Azure resources you create in either virtual network are now able toocommunicate with each other through their IP addresses.</span></span> <span data-ttu-id="f5176-216">Om du använder standard-Azure namnmatchning för virtuella nätverk för hello hello resurser i hello virtuella nätverk är inte kan tooresolve namn över hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="f5176-216">If you're using default Azure name resolution for hello virtual networks, hello resources in hello virtual networks are not able tooresolve names across hello virtual networks.</span></span> <span data-ttu-id="f5176-217">Om du vill tooresolve namn över virtuella nätverk i en peering måste du skapa DNS-servern.</span><span class="sxs-lookup"><span data-stu-id="f5176-217">If you want tooresolve names across virtual networks in a peering, you must create your own DNS server.</span></span> <span data-ttu-id="f5176-218">Lär dig hur tooset in [namnmatchning med hjälp av DNS-servern](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span><span class="sxs-lookup"><span data-stu-id="f5176-218">Learn how tooset up [Name resolution using your own DNS server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).</span></span>

## <span data-ttu-id="f5176-219"><a name="template"></a>Skapa peering - Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="f5176-219"><a name="template"></a>Create peering - Resource Manager template</span></span>

1. <span data-ttu-id="f5176-220">Referens [skapa ett virtuellt nätverk som peering](https://azure.microsoft.com/resources/templates/201-vnet-to-vnet-peering) Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="f5176-220">Reference [Create a virtual network peering](https://azure.microsoft.com/resources/templates/201-vnet-to-vnet-peering) Resource Manager template.</span></span> <span data-ttu-id="f5176-221">Instruktioner finns angivna med hello mallen för distribution av hello mallen med hjälp av hello Azure-portalen, PowerShell eller hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="f5176-221">Instructions are provided with hello template for deploying hello template using hello Azure portal, PowerShell, or hello Azure CLI.</span></span> <span data-ttu-id="f5176-222">Logga in toowhichever verktyg du väljer toodeploy hello mall med ett konto som har hello behörighet toocreate peering ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f5176-222">Log in toowhichever tool you choose toodeploy hello template with using an account that has hello necessary permissions toocreate a virtual network peering.</span></span> <span data-ttu-id="f5176-223">Se hello [behörigheter](#permissions) i den här artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="f5176-223">See hello [Permissions](#permissions) section of this article for details.</span></span>
2. <span data-ttu-id="f5176-224">**Valfria**: även om att skapa virtuella datorer inte ingår i den här självstudiekursen, du kan skapa en virtuell dator i varje virtuellt nätverk och ansluta från en virtuell dator toohello andra toovalidate anslutning.</span><span class="sxs-lookup"><span data-stu-id="f5176-224">**Optional**: Though creating virtual machines is not covered in this tutorial, you can create a virtual machine in each virtual network and connect from one virtual machine toohello other, toovalidate connectivity.</span></span>
3. <span data-ttu-id="f5176-225">**Valfria**: toodelete hello resurser som du skapar i den här självstudiekursen, fullständig hello stegen i hello [bort resurser](#delete) i den här artikeln använder antingen hello Azure-portalen, PowerShell eller hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="f5176-225">**Optional**: toodelete hello resources that you create in this tutorial, complete hello steps in hello [Delete resources](#delete) section of this article, using either hello Azure portal, PowerShell, or hello Azure CLI.</span></span>

## <span data-ttu-id="f5176-226"><a name="permissions"></a>Behörigheter</span><span class="sxs-lookup"><span data-stu-id="f5176-226"><a name="permissions"></a>Permissions</span></span>

<span data-ttu-id="f5176-227">hello-konton som du använder ett virtuellt nätverk som peering toocreate måste ha hello behövs eller behörigheter.</span><span class="sxs-lookup"><span data-stu-id="f5176-227">hello accounts you use toocreate a virtual network peering must have hello necessary role or permissions.</span></span> <span data-ttu-id="f5176-228">Till exempel om du peering två virtuella nätverk som heter VNet1 och VNet2, måste ditt konto tilldelas hello följande minsta eller behörigheter för varje virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="f5176-228">For example, if you were peering two virtual networks named VNet1 and VNet2, your account must be assigned hello following minimum role or permissions for each virtual network:</span></span>
    
|<span data-ttu-id="f5176-229">Virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="f5176-229">Virtual network</span></span>|<span data-ttu-id="f5176-230">Roll</span><span class="sxs-lookup"><span data-stu-id="f5176-230">Role</span></span>|<span data-ttu-id="f5176-231">Behörigheter</span><span class="sxs-lookup"><span data-stu-id="f5176-231">Permissions</span></span>|
|---|---|---|
|<span data-ttu-id="f5176-232">VNet1</span><span class="sxs-lookup"><span data-stu-id="f5176-232">VNet1</span></span>|[<span data-ttu-id="f5176-233">Nätverksdeltagare</span><span class="sxs-lookup"><span data-stu-id="f5176-233">Network Contributor</span></span>](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|<span data-ttu-id="f5176-234">Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write</span><span class="sxs-lookup"><span data-stu-id="f5176-234">Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write</span></span>|
|<span data-ttu-id="f5176-235">VNet2</span><span class="sxs-lookup"><span data-stu-id="f5176-235">VNet2</span></span>|[<span data-ttu-id="f5176-236">Nätverksdeltagare</span><span class="sxs-lookup"><span data-stu-id="f5176-236">Network Contributor</span></span>](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|<span data-ttu-id="f5176-237">Microsoft.Network/virtualNetworks/peer</span><span class="sxs-lookup"><span data-stu-id="f5176-237">Microsoft.Network/virtualNetworks/peer</span></span>|

<span data-ttu-id="f5176-238">Lär dig mer om [inbyggda roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) och tilldela särskilda behörigheter för[anpassade roller](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (enbart Resource Manager).</span><span class="sxs-lookup"><span data-stu-id="f5176-238">Learn more about [built-in roles](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) and assigning specific permissions too[custom roles](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Resource Manager only).</span></span>

## <span data-ttu-id="f5176-239"><a name="delete"></a>Ta bort resurser</span><span class="sxs-lookup"><span data-stu-id="f5176-239"><a name="delete"></a>Delete resources</span></span>
<span data-ttu-id="f5176-240">När du är klar med den här kursen kan kanske du vill toodelete hello resurser som du skapade i hello självstudier, så att du inte betalar användningsavgifter.</span><span class="sxs-lookup"><span data-stu-id="f5176-240">When you've finished this tutorial, you might want toodelete hello resources you created in hello tutorial, so you don't incur usage charges.</span></span> <span data-ttu-id="f5176-241">En resursgrupp också tar du bort alla resurser som finns i hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="f5176-241">Deleting a resource group also deletes all resources that are in hello resource group.</span></span>

### <span data-ttu-id="f5176-242"><a name="delete-portal"></a>Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f5176-242"><a name="delete-portal"></a>Azure portal</span></span>

1. <span data-ttu-id="f5176-243">Ange i hello portal sökrutan **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="f5176-243">In hello portal search box, enter **myResourceGroup**.</span></span> <span data-ttu-id="f5176-244">I hello sökresultaten klickar du på **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="f5176-244">In hello search results, click **myResourceGroup**.</span></span>
2. <span data-ttu-id="f5176-245">På hello **myResourceGroup** bladet, klickar du på hello **ta bort** ikon.</span><span class="sxs-lookup"><span data-stu-id="f5176-245">On hello **myResourceGroup** blade, click hello **Delete** icon.</span></span>
3. <span data-ttu-id="f5176-246">tooconfirm hello borttagning, i hello **typen hello RESURSGRUPPENS namn** ange **myResourceGroup**, och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="f5176-246">tooconfirm hello deletion, in hello **TYPE hello RESOURCE GROUP NAME** box, enter **myResourceGroup**, and then click **Delete**.</span></span>

### <span data-ttu-id="f5176-247"><a name="delete-cli"></a>Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f5176-247"><a name="delete-cli"></a>Azure CLI</span></span>

<span data-ttu-id="f5176-248">Ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f5176-248">Enter hello following command:</span></span>

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

### <span data-ttu-id="f5176-249"><a name="delete-powershell"></a>PowerShell</span><span class="sxs-lookup"><span data-stu-id="f5176-249"><a name="delete-powershell"></a>PowerShell</span></span>

<span data-ttu-id="f5176-250">Ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f5176-250">Enter hello following command:</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -force
```

## <a name="next-steps"></a><span data-ttu-id="f5176-251">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f5176-251">Next steps</span></span>

- <span data-ttu-id="f5176-252">Väl bekanta dig med viktiga [peering begränsningar för virtuellt nätverk och beteenden](virtual-network-manage-peering.md#requirements-and-constraints) innan du skapar ett virtuellt nätverk peering för produktion använder.</span><span class="sxs-lookup"><span data-stu-id="f5176-252">Thoroughly familiarize yourself with important [virtual network peering constraints and behaviors](virtual-network-manage-peering.md#requirements-and-constraints) before creating a virtual network peering for production use.</span></span>
- <span data-ttu-id="f5176-253">Lär dig mer om alla [peering inställningar för virtuella nätverk](virtual-network-manage-peering.md#create-a-peering).</span><span class="sxs-lookup"><span data-stu-id="f5176-253">Learn about all [virtual network peering settings](virtual-network-manage-peering.md#create-a-peering).</span></span>
- <span data-ttu-id="f5176-254">Lär dig hur för[skapa ett nav och eker nätverkstopologi](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) med virtuella nätverk peering.</span><span class="sxs-lookup"><span data-stu-id="f5176-254">Learn how too[create a hub and spoke network topology](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) with virtual network peering.</span></span>
