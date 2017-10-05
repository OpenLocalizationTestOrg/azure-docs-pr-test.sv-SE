---
title: "Konfigurera privata IP-adresser för virtuella datorer (klassisk) - Azure-portalen | Microsoft Docs"
description: "Lär dig hur du konfigurerar den privata IP-adresser för virtuella datorer (klassisk) med Azure-portalen."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: b8ef8367-58b2-42df-9f26-3269980950b8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bde6de3495c2909b63b1f85e420a4ff5e7ac2c1a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-the-azure-portal"></a><span data-ttu-id="d6f9c-103">Konfigurera privat IP-adresser för en virtuell dator (klassisk) som använder Azure portal</span><span class="sxs-lookup"><span data-stu-id="d6f9c-103">Configure private IP addresses for a virtual machine (Classic) using the Azure portal</span></span>

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="d6f9c-104">Den här artikeln beskriver hur du gör om du använder den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="d6f9c-104">This article covers the classic deployment model.</span></span> <span data-ttu-id="d6f9c-105">Du kan också [hantera en statisk privat IP-adress i Resource Manager-distributionsmodellen](virtual-networks-static-private-ip-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="d6f9c-105">You can also [manage a static private IP address in the Resource Manager deployment model](virtual-networks-static-private-ip-arm-pportal.md).</span></span>

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

<span data-ttu-id="d6f9c-106">Nedanstående exempel räknar en enkel miljö som redan har skapats.</span><span class="sxs-lookup"><span data-stu-id="d6f9c-106">The sample steps below expect a simple environment already created.</span></span> <span data-ttu-id="d6f9c-107">Om du vill köra stegen som de visas i det här dokumentet först skapa testmiljön som beskrivs i [skapa ett vnet](virtual-networks-create-vnet-classic-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="d6f9c-107">If you want to run the steps as they are displayed in this document, first build the test environment described in [create a vnet](virtual-networks-create-vnet-classic-pportal.md).</span></span>

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a><span data-ttu-id="d6f9c-108">Så här anger du en statisk privat IP-adress när du skapar en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d6f9c-108">How to specify a static private IP address when creating a VM</span></span>
<span data-ttu-id="d6f9c-109">Skapa en virtuell dator med namnet *DNS01* i den *klientdel* undernätet i ett VNet med namnet *TestVNet* med en statisk privat IP-adress för *192.168.1.101*, Följ stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="d6f9c-109">To create a VM named *DNS01* in the *FrontEnd* subnet of a VNet named *TestVNet* with a static private IP of *192.168.1.101*, follow the steps below:</span></span>

1. <span data-ttu-id="d6f9c-110">Från en webbläsare, navigerar du till http://portal.azure.com och loggar, vid behov, in med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="d6f9c-110">From a browser, navigate to http://portal.azure.com and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="d6f9c-111">Klicka på **ny** > **Compute** > **Windows Server 2012 R2 Datacenter**, Lägg märke till att den **Välj en distributionsmodell** redan lista visas **klassiska**, och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="d6f9c-111">Click **NEW** > **Compute** > **Windows Server 2012 R2 Datacenter**, notice that the **Select a deployment model** list already shows **Classic**, and then click **Create**.</span></span>
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-classic-pportal/figure01.png)
3. <span data-ttu-id="d6f9c-113">I den **skapa VM** bladet anger du namnet på den virtuella datorn skapas (*DNS01* i vårt exempel), lokalt administratörskonto och lösenord.</span><span class="sxs-lookup"><span data-stu-id="d6f9c-113">In the **Create VM** blade, enter the name of the VM to be created (*DNS01* in our scenario), the local administrator account, and password.</span></span>
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-classic-pportal/figure02.png)
4. <span data-ttu-id="d6f9c-115">Klicka på **valfri konfiguration** > **nätverk** > **virtuellt nätverk**, och klicka sedan på **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="d6f9c-115">Click **Optional Configuration** > **Network** > **Virtual Network**, and then click **TestVNet**.</span></span> <span data-ttu-id="d6f9c-116">Om **TestVNet** är inte tillgänglig, kontrollera att du använder den *centrala USA* plats och har skapat testmiljön som beskrivs i början av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="d6f9c-116">If **TestVNet** is not available, make sure you are using the *Central US* location and have created the test environment described at the beginning of this article.</span></span>
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-classic-pportal/figure03.png)
5. <span data-ttu-id="d6f9c-118">I den **nätverk** bladet Kontrollera undernätet markerade *klientdel*, klicka på **IP-adresser**under **IP-adresstilldelning** klickar du på **statiska**, och ange sedan *192.168.1.101* för **IP-adress** som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="d6f9c-118">In the **Network** blade, make sure the subnet currently selected is *FrontEnd*, then click **IP addresses**, under **IP address assignment** click **Static**, and then enter *192.168.1.101* for **IP Address** as seen below.</span></span>
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-classic-pportal/figure04.png)    
6. <span data-ttu-id="d6f9c-120">Klicka på **OK** i den **IP-adresser** bladet klicka sedan på **OK** i den **nätverk** bladet och klicka på **OK** i den **valfria config** bladet.</span><span class="sxs-lookup"><span data-stu-id="d6f9c-120">Click **OK** in the **IP addresses** blade, then click **OK** in the **Network** blade, and click **OK** in the **Optional config** blade.</span></span>
7. <span data-ttu-id="d6f9c-121">I den **skapa VM** bladet, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="d6f9c-121">In the **Create VM** blade, click **Create**.</span></span> <span data-ttu-id="d6f9c-122">Lägg märke till panelen nedan visas i instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="d6f9c-122">Notice the tile below displayed in your dashboard.</span></span>
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a><span data-ttu-id="d6f9c-124">Hur du hämtar statisk privat IP-adressinformation för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d6f9c-124">How to retrieve static private IP address information for a VM</span></span>
<span data-ttu-id="d6f9c-125">Utför stegen nedan om du vill visa statisk privat IP-adressinformation för den virtuella datorn skapas med stegen ovan.</span><span class="sxs-lookup"><span data-stu-id="d6f9c-125">To view the static private IP address information for the VM created with the steps above, execute the steps below.</span></span>

1. <span data-ttu-id="d6f9c-126">Azure-Azure-portalen klickar du på **Bläddra bland alla** > **virtuella datorer (klassisk)** > **DNS01** > **alla inställningar** > **IP-adresser** och Lägg märke till tilldelning av IP-adresser och IP-adress som du ser nedan.</span><span class="sxs-lookup"><span data-stu-id="d6f9c-126">From the Azure Azure portal, click **BROWSE ALL** > **Virtual machines (classic)** > **DNS01** > **All settings** > **IP addresses** and notice the IP address assignment and IP address as seen below.</span></span>
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a><span data-ttu-id="d6f9c-128">Ta bort en statisk privat IP-adress från en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d6f9c-128">How to remove a static private IP address from a VM</span></span>
<span data-ttu-id="d6f9c-129">Följ stegen nedan om du vill ta bort statisk privat IP-adress från den virtuella datorn skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="d6f9c-129">To remove the static private IP address from the VM created above, follow the steps below.</span></span>

1. <span data-ttu-id="d6f9c-130">Från den **IP-adresser** bladet som visas ovan, klickar du på **dynamiska** till höger om **IP-adresstilldelning**, klicka på **spara**, och klicka sedan på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="d6f9c-130">From the **IP addresses** blade shown above, click **Dynamic** to the right of **IP address assignment**, then click **Save**, and then click **Yes**.</span></span>
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a><span data-ttu-id="d6f9c-132">Hur du lägger till en statisk privat IP-adress till en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d6f9c-132">How to add a static private IP address to an existing VM</span></span>
<span data-ttu-id="d6f9c-133">Följ stegen nedan om du vill lägga till en statisk privat IP-adress till den virtuella datorn skapas med stegen ovan:</span><span class="sxs-lookup"><span data-stu-id="d6f9c-133">To add a static private IP address to the VM created using the steps above, follow the steps below:</span></span>

1. <span data-ttu-id="d6f9c-134">Från den **IP-adresser** bladet som visas ovan, klickar du på **statiska** till höger om **IP-adresstilldelning**.</span><span class="sxs-lookup"><span data-stu-id="d6f9c-134">From the **IP addresses** blade shown above, click **Static** to the right of **IP address assignment**.</span></span>
2. <span data-ttu-id="d6f9c-135">Typen *192.168.1.101* för **IP-adress**, klicka på **spara**, och klicka sedan på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="d6f9c-135">Type *192.168.1.101* for **IP address**, then click **Save**, and then click **Yes**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d6f9c-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d6f9c-136">Next steps</span></span>
* <span data-ttu-id="d6f9c-137">Lär dig mer om [reserverade offentliga IP-Adressen](virtual-networks-reserved-public-ip.md) adresser.</span><span class="sxs-lookup"><span data-stu-id="d6f9c-137">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="d6f9c-138">Lär dig mer om [instansnivå offentliga IP-går](virtual-networks-instance-level-public-ip.md) adresser.</span><span class="sxs-lookup"><span data-stu-id="d6f9c-138">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="d6f9c-139">Läs den [reserverade IP-REST API: er](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="d6f9c-139">Consult the [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>

