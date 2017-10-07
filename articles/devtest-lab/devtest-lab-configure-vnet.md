---
title: "aaaConfigure ett virtuellt nätverk i Azure DevTest Labs | Microsoft Docs"
description: "Lär dig hur tooconfigure ett befintligt virtuellt nätverk och undernät, och Använd dem i en virtuell dator med Azure DevTest Labs"
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
ms.openlocfilehash: a11ce8315e3c540e44aeacc9c5ee3dde014d4621
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-virtual-network-in-azure-devtest-labs"></a><span data-ttu-id="7e654-103">Konfigurera ett virtuellt nätverk i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="7e654-103">Configure a virtual network in Azure DevTest Labs</span></span>
<span data-ttu-id="7e654-104">Enligt beskrivningen i artikeln hello [lägga till en virtuell dator med artefakter tooa lab](devtest-lab-add-vm-with-artifacts.md)när du skapar en virtuell dator i ett labb du kan ange ett konfigurerat virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="7e654-104">As explained in hello article, [Add a VM with artifacts tooa lab](devtest-lab-add-vm-with-artifacts.md), when you create a VM in a lab, you can specify a configured virtual network.</span></span> <span data-ttu-id="7e654-105">Ett scenario för att göra detta är om du behöver tooaccess corpnet resurserna från dina virtuella datorer med hjälp av hello virtuella nätverk som har konfigurerats med ExpressRoute eller plats-till-plats-VPN.</span><span class="sxs-lookup"><span data-stu-id="7e654-105">One scenario for doing this is if you need tooaccess your corpnet resources from your VMs using hello virtual network that was configured with ExpressRoute or site-to-site VPN.</span></span> <span data-ttu-id="7e654-106">hello följande avsnitt visar hur tooadd ditt befintliga virtuella nätverk i ett labb inställningarna för virtuella nätverk så att den är tillgänglig toochoose när du skapar virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7e654-106">hello following sections illustrate how tooadd your existing virtual network into a lab's Virtual Network settings so that it is available toochoose when creating VMs.</span></span>

## <a name="configure-a-virtual-network-for-a-lab-using-hello-azure-portal"></a><span data-ttu-id="7e654-107">Konfigurera ett virtuellt nätverk för ett testlabb med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7e654-107">Configure a virtual network for a lab using hello Azure portal</span></span>
<span data-ttu-id="7e654-108">hello följande steg beskriver hur du lägger till ett befintligt virtuellt nätverk (och undernät) tooa labb så att den kan användas när du skapar en virtuell dator i hello samma labbet.</span><span class="sxs-lookup"><span data-stu-id="7e654-108">hello following steps walk you through adding an existing virtual network (and subnet) tooa lab so that it can be used when creating a VM in hello same lab.</span></span> 

1. <span data-ttu-id="7e654-109">Logga in toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="7e654-109">Sign in toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
2. <span data-ttu-id="7e654-110">Välj **fler tjänster**, och välj sedan **DevTest Labs** hello-listan.</span><span class="sxs-lookup"><span data-stu-id="7e654-110">Select **More Services**, and then select **DevTest Labs** from hello list.</span></span>
3. <span data-ttu-id="7e654-111">Välj önskad hello-labb hello listan övningar.</span><span class="sxs-lookup"><span data-stu-id="7e654-111">From hello list of labs, select hello desired lab.</span></span> 
4. <span data-ttu-id="7e654-112">På bladet hello lab väljer **Configuration**.</span><span class="sxs-lookup"><span data-stu-id="7e654-112">On hello lab's blade, select **Configuration**.</span></span>
5. <span data-ttu-id="7e654-113">På hello lab **Configuration** bladet väljer **virtuella nätverken**.</span><span class="sxs-lookup"><span data-stu-id="7e654-113">On hello lab's **Configuration** blade, select **Virtual networks**.</span></span>
6. <span data-ttu-id="7e654-114">På hello **virtuella nätverken** bladet visas en lista över virtuella nätverk som konfigurerats för hello aktuella labbet samt hello standard virtuella nätverk som har skapats för ditt labb.</span><span class="sxs-lookup"><span data-stu-id="7e654-114">On hello **Virtual networks** blade, you see a list of virtual networks configured for hello current lab as well as hello default virtual network that is created for your lab.</span></span> 
7. <span data-ttu-id="7e654-115">Välj **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="7e654-115">Select **+ Add**.</span></span>
   
    ![Lägg till ett befintligt virtuellt nätverk tooyour labb](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
8. <span data-ttu-id="7e654-117">På hello **för virtuella nätverk** bladet väljer **[väljer virtuellt nätverk]**.</span><span class="sxs-lookup"><span data-stu-id="7e654-117">On hello **Virtual network** blade, select **[Select virtual network]**.</span></span>
   
    ![Välj ett befintligt virtuellt nätverk](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
9. <span data-ttu-id="7e654-119">På hello **Välj virtuellt nätverk** bladet, Välj hello önskade virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="7e654-119">On hello **Choose virtual network** blade, select hello desired virtual network.</span></span> <span data-ttu-id="7e654-120">hello bladet visar alla hello virtuella nätverk som är under hello samma region i hello prenumeration som hello labbet.</span><span class="sxs-lookup"><span data-stu-id="7e654-120">hello blade shows all hello virtual networks that are under hello same region in hello subscription as hello lab.</span></span>  
10. <span data-ttu-id="7e654-121">När du har valt ett virtuellt nätverk, returneras toohello **för virtuella nätverk** på hello undernät i hello lista längst hello hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="7e654-121">After selecting a virtual network, you are returned toohello **Virtual network** Click hello subnet in hello list at hello bottom of hello blade.</span></span>

    ![Lista över undernät](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)
    
    <span data-ttu-id="7e654-123">hello Lab undernät-bladet visas.</span><span class="sxs-lookup"><span data-stu-id="7e654-123">hello Lab Subnet blade is displayed.</span></span>

    ![Blad för labbet undernät](./media/devtest-lab-configure-vnet/lab-subnet.png)

11. <span data-ttu-id="7e654-125">Ange en **labbundernätsnamn**.</span><span class="sxs-lookup"><span data-stu-id="7e654-125">Specify a **Lab subnet name**.</span></span>
12. <span data-ttu-id="7e654-126">tooallow ett undernät toobe som använts i labbet skapa en virtuell dator, Välj **användning i virtuell dator skapas**.</span><span class="sxs-lookup"><span data-stu-id="7e654-126">tooallow a subnet toobe used in lab VM creation, select **Use in virtual machine creation**.</span></span>
13. <span data-ttu-id="7e654-127">tooenable en [delad offentlig IP-adress](devtest-lab-shared-ip.md)väljer **aktivera delad offentlig IP**.</span><span class="sxs-lookup"><span data-stu-id="7e654-127">tooenable a [shared public IP address](devtest-lab-shared-ip.md), select **Enable shared public IP**.</span></span>
14. <span data-ttu-id="7e654-128">tooallow offentliga IP-adresser i ett undernät, Välj **kan skapa en offentlig IP-**.</span><span class="sxs-lookup"><span data-stu-id="7e654-128">tooallow public IP addresses in a subnet, select **Allow public IP creation**.</span></span>
15. <span data-ttu-id="7e654-129">I hello **maximalt virtuella datorer per användare** anger hello maximala virtuella datorer per användare för varje undernät.</span><span class="sxs-lookup"><span data-stu-id="7e654-129">In hello **Maximum virtual machines per user** field, specify hello maximum VMs per user for each subnet.</span></span> <span data-ttu-id="7e654-130">Lämna fältet tomt om du vill att ett obegränsat antal virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7e654-130">If you want an unrestricted number of VMs, leave this field blank.</span></span>
16. <span data-ttu-id="7e654-131">Välj **OK** tooclose hello Lab undernät-bladet.</span><span class="sxs-lookup"><span data-stu-id="7e654-131">Select **OK** tooclose hello Lab Subnet blade.</span></span>
17. <span data-ttu-id="7e654-132">Välj **spara** tooclose hello virtuella nätverk-bladet.</span><span class="sxs-lookup"><span data-stu-id="7e654-132">Select **Save** tooclose hello Virtual network blade.</span></span>
18. <span data-ttu-id="7e654-133">Hello virtuellt nätverk är konfigurerat, kan du välja den när du skapar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="7e654-133">Now that hello virtual network is configured, it can be selected when creating a VM.</span></span> 
    <span data-ttu-id="7e654-134">toosee hur toocreate en virtuell dator och ange ett virtuellt nätverk finns i artikeln toohello [lägga till en virtuell dator med artefakter tooa lab](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="7e654-134">toosee how toocreate a VM and specify a virtual network, refer toohello article, [Add a VM with artifacts tooa lab](devtest-lab-add-vm-with-artifacts.md).</span></span> 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="7e654-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7e654-135">Next steps</span></span>
<span data-ttu-id="7e654-136">När du har lagt till hello önskade virtuella nätverket tooyour lab, hello nästa steg är för[lägga till ett labb för VM-tooyour](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="7e654-136">Once you have added hello desired virtual network tooyour lab, hello next step is too[add a VM tooyour lab](devtest-lab-add-vm-with-artifacts.md).</span></span>

