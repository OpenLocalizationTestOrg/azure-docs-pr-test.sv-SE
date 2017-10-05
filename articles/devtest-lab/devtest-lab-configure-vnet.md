---
title: "Konfigurera ett virtuellt nätverk i Azure DevTest Labs | Microsoft Docs"
description: "Lär dig hur du konfigurerar ett befintligt virtuellt nätverk och undernät och använda dem i en virtuell dator med Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 6cda99c2-b87e-4047-90a0-5df10d8e9e14
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: tarcher
ms.openlocfilehash: 848752085729df7d98a3a4b7be36d894c12cd033
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-virtual-network-in-azure-devtest-labs"></a><span data-ttu-id="0ccf5-103">Konfigurera ett virtuellt nätverk i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="0ccf5-103">Configure a virtual network in Azure DevTest Labs</span></span>
<span data-ttu-id="0ccf5-104">Enligt beskrivningen i artikeln [lägga till en virtuell dator med artefakter i ett labb](devtest-lab-add-vm-with-artifacts.md)när du skapar en virtuell dator i labbet, du kan ange ett konfigurerat virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-104">As explained in the article, [Add a VM with artifacts to a lab](devtest-lab-add-vm-with-artifacts.md), when you create a VM in a lab, you can specify a configured virtual network.</span></span> <span data-ttu-id="0ccf5-105">Ett scenario för att göra detta är om du behöver åtkomst till corpnet resurser från dina virtuella datorer med hjälp av det virtuella nätverket som har konfigurerats med ExpressRoute eller plats-till-plats-VPN.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-105">One scenario for doing this is if you need to access your corpnet resources from your VMs using the virtual network that was configured with ExpressRoute or site-to-site VPN.</span></span> <span data-ttu-id="0ccf5-106">Följande avsnitt visar hur du lägger till ett befintligt virtuellt nätverk i inställningarna för virtuella nätverk i ett labb så att den ska kunna välja när du skapar virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-106">The following sections illustrate how to add your existing virtual network into a lab's Virtual Network settings so that it is available to choose when creating VMs.</span></span>

## <a name="configure-a-virtual-network-for-a-lab-using-the-azure-portal"></a><span data-ttu-id="0ccf5-107">Konfigurera ett virtuellt nätverk för ett labb i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0ccf5-107">Configure a virtual network for a lab using the Azure portal</span></span>
<span data-ttu-id="0ccf5-108">Följande steg beskriver hur du lägger till ett befintligt virtuellt nätverk (och undernät) i ett labb så att den kan användas när du skapar en virtuell dator i samma labbet.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-108">The following steps walk you through adding an existing virtual network (and subnet) to a lab so that it can be used when creating a VM in the same lab.</span></span> 

1. <span data-ttu-id="0ccf5-109">Logga in på [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="0ccf5-109">Sign in to the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="0ccf5-110">Välj **fler tjänster**, och välj sedan **DevTest Labs** från listan.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-110">Select **More Services**, and then select **DevTest Labs** from the list.</span></span>
3. <span data-ttu-id="0ccf5-111">Lista över labs, Välj önskade labbet.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-111">From the list of labs, select the desired lab.</span></span> 
4. <span data-ttu-id="0ccf5-112">På den testmiljön bladet välj **Configuration**.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-112">On the lab's blade, select **Configuration**.</span></span>
5. <span data-ttu-id="0ccf5-113">På testmiljön **Configuration** bladet väljer **virtuella nätverken**.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-113">On the lab's **Configuration** blade, select **Virtual networks**.</span></span>
6. <span data-ttu-id="0ccf5-114">På den **virtuella nätverken** bladet visas en lista över virtuella nätverk som konfigurerats för det aktuella labbet som standard virtuella nätverket som skapas för ditt labb.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-114">On the **Virtual networks** blade, you see a list of virtual networks configured for the current lab as well as the default virtual network that is created for your lab.</span></span> 
7. <span data-ttu-id="0ccf5-115">Välj **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-115">Select **+ Add**.</span></span>
   
    ![Lägg till ett befintligt virtuellt nätverk i ditt labb](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
8. <span data-ttu-id="0ccf5-117">På den **för virtuella nätverk** bladet väljer **[väljer virtuellt nätverk]**.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-117">On the **Virtual network** blade, select **[Select virtual network]**.</span></span>
   
    ![Välj ett befintligt virtuellt nätverk](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
9. <span data-ttu-id="0ccf5-119">På den **Välj virtuellt nätverk** bladet välj önskade virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-119">On the **Choose virtual network** blade, select the desired virtual network.</span></span> <span data-ttu-id="0ccf5-120">Bladet visar de virtuella nätverk som finns under samma region i prenumerationen som labbet.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-120">The blade shows all the virtual networks that are under the same region in the subscription as the lab.</span></span>  
10. <span data-ttu-id="0ccf5-121">När du har valt ett virtuellt nätverk, är du tillbaka till den **för virtuella nätverk** på undernät i listan längst ned på bladet.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-121">After selecting a virtual network, you are returned to the **Virtual network** Click the subnet in the list at the bottom of the blade.</span></span>

    ![Lista över undernät](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)
    
    <span data-ttu-id="0ccf5-123">Lab undernät-bladet visas.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-123">The Lab Subnet blade is displayed.</span></span>

    ![Blad för labbet undernät](./media/devtest-lab-configure-vnet/lab-subnet.png)

11. <span data-ttu-id="0ccf5-125">Ange en **labbundernätsnamn**.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-125">Specify a **Lab subnet name**.</span></span>
12. <span data-ttu-id="0ccf5-126">Om ett undernät som ska användas i testmiljön skapa Virtuella väljer **användning i virtuell dator skapas**.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-126">To allow a subnet to be used in lab VM creation, select **Use in virtual machine creation**.</span></span>
13. <span data-ttu-id="0ccf5-127">Så här aktiverar du en [delad offentlig IP-adress](devtest-lab-shared-ip.md)väljer **aktivera delad offentlig IP**.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-127">To enable a [shared public IP address](devtest-lab-shared-ip.md), select **Enable shared public IP**.</span></span>
14. <span data-ttu-id="0ccf5-128">Om du vill tillåta offentliga IP-adresser i ett undernät, Välj **kan skapa en offentlig IP-**.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-128">To allow public IP addresses in a subnet, select **Allow public IP creation**.</span></span>
15. <span data-ttu-id="0ccf5-129">I den **maximalt virtuella datorer per användare** anger de maximala virtuella datorerna per användare för varje undernät.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-129">In the **Maximum virtual machines per user** field, specify the maximum VMs per user for each subnet.</span></span> <span data-ttu-id="0ccf5-130">Lämna fältet tomt om du vill att ett obegränsat antal virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-130">If you want an unrestricted number of VMs, leave this field blank.</span></span>
16. <span data-ttu-id="0ccf5-131">Välj **OK** att stänga bladet Lab undernät.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-131">Select **OK** to close the Lab Subnet blade.</span></span>
17. <span data-ttu-id="0ccf5-132">Välj **spara** att stänga bladet virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-132">Select **Save** to close the Virtual network blade.</span></span>
18. <span data-ttu-id="0ccf5-133">Det virtuella nätverket är konfigurerat, kan du välja den när du skapar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0ccf5-133">Now that the virtual network is configured, it can be selected when creating a VM.</span></span> 
    <span data-ttu-id="0ccf5-134">Att se hur du skapar en virtuell dator och ange ett virtuellt nätverk finns i artikel [lägga till en virtuell dator med artefakter i ett labb](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="0ccf5-134">To see how to create a VM and specify a virtual network, refer to the article, [Add a VM with artifacts to a lab](devtest-lab-add-vm-with-artifacts.md).</span></span> 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="0ccf5-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0ccf5-135">Next steps</span></span>
<span data-ttu-id="0ccf5-136">När du har lagt till det önskade virtuella nätverket till ditt labb, nästa steg är att [lägga till en virtuell dator i labbet](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="0ccf5-136">Once you have added the desired virtual network to your lab, the next step is to [add a VM to your lab](devtest-lab-add-vm-with-artifacts.md).</span></span>

