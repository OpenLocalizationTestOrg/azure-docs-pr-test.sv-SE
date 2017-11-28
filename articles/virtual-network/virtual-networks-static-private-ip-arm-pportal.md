---
title: "aaaConfigure privata IP-adresser för virtuella datorer - Azure-portalen | Microsoft Docs"
description: "Lär dig hur tooconfigure privata IP-adresser för virtuella datorer med hello Azure-portalen."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 11245645-357d-4358-9a14-dd78e367b494
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 474161303cdf8cb98e16ffd7cef6b74debdbc49a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-hello-azure-portal"></a><span data-ttu-id="8be79-103">Konfigurera privat IP-adresser för en virtuell dator med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8be79-103">Configure private IP addresses for a virtual machine using hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="8be79-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8be79-104">Azure portal</span></span>](virtual-networks-static-private-ip-arm-pportal.md)
> * [<span data-ttu-id="8be79-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8be79-105">PowerShell</span></span>](virtual-networks-static-private-ip-arm-ps.md)
> * [<span data-ttu-id="8be79-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="8be79-106">Azure CLI</span></span>](virtual-networks-static-private-ip-arm-cli.md)
> * [<span data-ttu-id="8be79-107">Azure-portalen (klassisk)</span><span class="sxs-lookup"><span data-stu-id="8be79-107">Azure portal (Classic)</span></span>](virtual-networks-static-private-ip-classic-pportal.md)
> * [<span data-ttu-id="8be79-108">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="8be79-108">PowerShell (Classic)</span></span>](virtual-networks-static-private-ip-classic-ps.md)
> * [<span data-ttu-id="8be79-109">Azure CLI (klassisk)</span><span class="sxs-lookup"><span data-stu-id="8be79-109">Azure CLI (Classic)</span></span>](virtual-networks-static-private-ip-classic-cli.md)


[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="8be79-110">Den här artikeln beskriver hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="8be79-110">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="8be79-111">Du kan också [hantera statisk privat IP-adress i hello klassiska distributionsmodellen](virtual-networks-static-private-ip-classic-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="8be79-111">You can also [manage static private IP address in hello classic deployment model](virtual-networks-static-private-ip-classic-pportal.md).</span></span>

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

<span data-ttu-id="8be79-112">hello exempel stegen nedan förväntar sig en enkel miljö som redan har skapats.</span><span class="sxs-lookup"><span data-stu-id="8be79-112">hello sample steps below expect a simple environment already created.</span></span> <span data-ttu-id="8be79-113">Om du vill toorun hello steg så som de visas i det här dokumentet, först skapa hello testmiljö som beskrivs i [skapa ett vnet](virtual-networks-create-vnet-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="8be79-113">If you want toorun hello steps as they are displayed in this document, first build hello test environment described in [create a vnet](virtual-networks-create-vnet-arm-pportal.md).</span></span>

## <a name="how-toocreate-a-vm-for-testing-static-private-ip-addresses"></a><span data-ttu-id="8be79-114">Hur toocreate en virtuell dator för att testa statisk privat IP-adresser</span><span class="sxs-lookup"><span data-stu-id="8be79-114">How toocreate a VM for testing static private IP addresses</span></span>
<span data-ttu-id="8be79-115">Du kan inte ange en statisk privat IP-adress under hello skapandet av en virtuell dator i läget för hello Resource Manager distribution med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8be79-115">You cannot set a static private IP address during hello creation of a VM in hello Resource Manager deployment mode by using hello Azure portal.</span></span> <span data-ttu-id="8be79-116">Du måste först skapa hello VM är textmarkering dess privata IP-toobe statisk.</span><span class="sxs-lookup"><span data-stu-id="8be79-116">You must create hello VM first, tehn set its private IP toobe static.</span></span>

<span data-ttu-id="8be79-117">toocreate en virtuell dator med namnet *DNS01* i hello *klientdel* undernätet i ett VNet med namnet *TestVNet*, följ hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="8be79-117">toocreate a VM named *DNS01* in hello *FrontEnd* subnet of a VNet named *TestVNet*, follow hello steps below.</span></span>

1. <span data-ttu-id="8be79-118">Från en webbläsare, navigerar du toohttp://portal.azure.com och, om det behövs, logga in med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="8be79-118">From a browser, navigate toohttp://portal.azure.com and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="8be79-119">Klicka på **ny** > **Compute** > **Windows Server 2012 R2 Datacenter**, Lägg märke till att hello **Välj en distributionsmodell** redan lista visas **Resource Manager**, och klicka sedan på **skapa**enligt hello bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="8be79-119">Click **NEW** > **Compute** > **Windows Server 2012 R2 Datacenter**, notice that hello **Select a deployment model** list already shows **Resource Manager**, and then click **Create**, as seen in hello figure below.</span></span>
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-arm-pportal/figure01.png)
3. <span data-ttu-id="8be79-121">I hello **grunderna** bladet anger hello namnet på hello VM toobe skapade (*DNS01* i vårt exempel), hello lokalt administratörskonto och lösenord, som visas i hello bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="8be79-121">In hello **Basics** blade, enter hello name of hello VM toobe created (*DNS01* in our scenario), hello local administrator account, and password, as seen in hello figure below.</span></span>
   
    ![Bladet Grundläggande inställningar](./media/virtual-networks-static-ip-arm-pportal/figure02.png)
4. <span data-ttu-id="8be79-123">Se till att hello **plats** valts är *centrala USA*, klicka sedan på **Välj befintlig** under **resursgruppen**, klicka på **Resursgruppen** igen och klicka sedan på *TestRG*, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="8be79-123">Make sure hello **Location** selected is *Central US*, then click **Select existing** under **Resource group**, then click **Resource group** again, then click *TestRG*, and then click **OK**.</span></span>
   
    ![Bladet Grundläggande inställningar](./media/virtual-networks-static-ip-arm-pportal/figure03.png)
5. <span data-ttu-id="8be79-125">I hello **välja en storlek** bladet väljer **A1 Standard**, och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="8be79-125">In hello **Choose a size** blade, select **A1 Standard**, and then click **Select**.</span></span>
   
    ![Välj ett blad storlek](./media/virtual-networks-static-ip-arm-pportal/figure04.png)    
6. <span data-ttu-id="8be79-127">I den **inställningar** bladet, se till att hello följande egenskaper har angetts med hello värdena nedan och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="8be79-127">In teh **Settings** blade, make sure hello following properties are set are set with hello values below, and then click **OK**.</span></span>
   
    <span data-ttu-id="8be79-128">-**Lagringskontot**: *vnetstorage*</span><span class="sxs-lookup"><span data-stu-id="8be79-128">-**Storage account**: *vnetstorage*</span></span>
   
   * <span data-ttu-id="8be79-129">**Nätverket**: *TestVNet*</span><span class="sxs-lookup"><span data-stu-id="8be79-129">**Network**: *TestVNet*</span></span>
   * <span data-ttu-id="8be79-130">**Undernät**: *klientdel*</span><span class="sxs-lookup"><span data-stu-id="8be79-130">**Subnet**: *FrontEnd*</span></span>
     
     ![Välj ett blad storlek](./media/virtual-networks-static-ip-arm-pportal/figure05.png)     
7. <span data-ttu-id="8be79-132">I hello **sammanfattning** bladet, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="8be79-132">In hello **Summary** blade, click **OK**.</span></span> <span data-ttu-id="8be79-133">Meddelande hello panelen nedan visas i instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="8be79-133">Notice hello tile below displayed in your dashboard.</span></span>
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a><span data-ttu-id="8be79-135">Hur tooretrieve statiska privata IP-information för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="8be79-135">How tooretrieve static private IP address information for a VM</span></span>
<span data-ttu-id="8be79-136">tooview hello statisk privat IP-adressinformation för hello skapas den virtuella datorn med hello ovanstående steg, köra hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="8be79-136">tooview hello static private IP address information for hello VM created with hello steps above, execute hello steps below.</span></span>

1. <span data-ttu-id="8be79-137">Hello Azure Azure-portalen klickar du på **Bläddra bland alla** > **virtuella datorer** > **DNS01** > **alla inställningar för** > **nätverksgränssnitt** och klicka sedan på hello endast nätverksgränssnitt som anges.</span><span class="sxs-lookup"><span data-stu-id="8be79-137">From hello Azure Azure portal, click **BROWSE ALL** > **Virtual machines** > **DNS01** > **All settings** > **Network interfaces** and then click on hello only network interface listed.</span></span>
   
    ![Distribuera Virtuella sida vid sida](./media/virtual-networks-static-ip-arm-pportal/figure07.png)
2. <span data-ttu-id="8be79-139">I hello **nätverksgränssnittet** bladet, klickar du på **alla inställningar** > **IP-adresser** och meddelande hello **tilldelning** och **IP-adress** värden.</span><span class="sxs-lookup"><span data-stu-id="8be79-139">In hello **Network interface** blade, click **All settings** > **IP addresses** and notice hello **Assignment** and **IP address** values.</span></span>
   
    ![Distribuera Virtuella sida vid sida](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-tooadd-a-static-private-ip-address-tooan-existing-vm"></a><span data-ttu-id="8be79-141">Hur tooadd en statisk privat IP-adress tooan befintliga VM</span><span class="sxs-lookup"><span data-stu-id="8be79-141">How tooadd a static private IP address tooan existing VM</span></span>
<span data-ttu-id="8be79-142">tooadd en statisk privat IP-adress toohello VM som skapats med hello anvisningarna ovan, så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="8be79-142">tooadd a static private IP address toohello VM created using hello steps above, follow hello steps below:</span></span>

1. <span data-ttu-id="8be79-143">Från hello **IP-adresser** bladet som visas ovan, klickar du på **statiska** under **tilldelning**.</span><span class="sxs-lookup"><span data-stu-id="8be79-143">From hello **IP addresses** blade shown above, click **Static** under **Assignment**.</span></span>
2. <span data-ttu-id="8be79-144">Typen *192.168.1.101* för **IP-adress**, och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="8be79-144">Type *192.168.1.101* for **IP address**, and then click **Save**.</span></span>
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

> [!NOTE]
> <span data-ttu-id="8be79-146">Om när du klickar på **spara** du märker att hello tilldelning fortfarande är angiven för**dynamisk**, innebär det att hello IP-adress som du har angett används redan.</span><span class="sxs-lookup"><span data-stu-id="8be79-146">If after clicking **Save** you notice that hello assignment is still set too**Dynamic**, it means that hello IP address you typed is already in use.</span></span> <span data-ttu-id="8be79-147">Prova en annan IP-adress.</span><span class="sxs-lookup"><span data-stu-id="8be79-147">Try a different IP address.</span></span>
> 
> 

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a><span data-ttu-id="8be79-148">Hur tooremove en statisk privat IP-adress från en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="8be79-148">How tooremove a static private IP address from a VM</span></span>
<span data-ttu-id="8be79-149">tooremove hello statisk privat IP-adress från hello VM skapade ovan, Slutför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8be79-149">tooremove hello static private IP address from hello VM created above, complete hello following step:</span></span>

<span data-ttu-id="8be79-150">Från hello **IP-adresser** bladet som visas ovan, klickar du på **dynamiska** under **tilldelning**, och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="8be79-150">From hello **IP addresses** blade shown above, click **Dynamic** under **Assignment**, and then click **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8be79-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8be79-151">Next steps</span></span>
* <span data-ttu-id="8be79-152">Lär dig mer om [reserverade offentliga IP-Adressen](virtual-networks-reserved-public-ip.md) adresser.</span><span class="sxs-lookup"><span data-stu-id="8be79-152">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="8be79-153">Lär dig mer om [instansnivå offentliga IP-går](virtual-networks-instance-level-public-ip.md) adresser.</span><span class="sxs-lookup"><span data-stu-id="8be79-153">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="8be79-154">Kontakta hello [reserverade IP-REST API: er](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="8be79-154">Consult hello [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>

