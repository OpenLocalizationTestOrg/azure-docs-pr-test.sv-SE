---
title: "aaaConfigure privata IP-adresser för virtuella datorer (klassisk) - Azure-portalen | Microsoft Docs"
description: "Lär dig hur tooconfigure privata IP-adresser för virtuella datorer (klassisk) med hello Azure-portalen."
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
ms.openlocfilehash: df4bfa6768fc9e66db89785b633ffdb0274dbc46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-hello-azure-portal"></a><span data-ttu-id="dc2ec-103">Konfigurera privat IP-adresser för en virtuell dator (klassisk) med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="dc2ec-103">Configure private IP addresses for a virtual machine (Classic) using hello Azure portal</span></span>

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="dc2ec-104">Den här artikeln beskriver hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="dc2ec-104">This article covers hello classic deployment model.</span></span> <span data-ttu-id="dc2ec-105">Du kan också [hantera en statisk privat IP-adress i hello Resource Manager-distributionsmodellen](virtual-networks-static-private-ip-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="dc2ec-105">You can also [manage a static private IP address in hello Resource Manager deployment model](virtual-networks-static-private-ip-arm-pportal.md).</span></span>

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

<span data-ttu-id="dc2ec-106">hello exempel stegen nedan förväntar sig en enkel miljö som redan har skapats.</span><span class="sxs-lookup"><span data-stu-id="dc2ec-106">hello sample steps below expect a simple environment already created.</span></span> <span data-ttu-id="dc2ec-107">Om du vill toorun hello steg så som de visas i det här dokumentet, först skapa hello testmiljö som beskrivs i [skapa ett vnet](virtual-networks-create-vnet-classic-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="dc2ec-107">If you want toorun hello steps as they are displayed in this document, first build hello test environment described in [create a vnet](virtual-networks-create-vnet-classic-pportal.md).</span></span>

## <a name="how-toospecify-a-static-private-ip-address-when-creating-a-vm"></a><span data-ttu-id="dc2ec-108">Hur toospecify en statisk privat IP-adress när du skapar en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="dc2ec-108">How toospecify a static private IP address when creating a VM</span></span>
<span data-ttu-id="dc2ec-109">toocreate en virtuell dator med namnet *DNS01* i hello *klientdel* undernätet i ett VNet med namnet *TestVNet* med en statisk privat IP-adress för *192.168.1.101*, Följ hello stegen nedan:</span><span class="sxs-lookup"><span data-stu-id="dc2ec-109">toocreate a VM named *DNS01* in hello *FrontEnd* subnet of a VNet named *TestVNet* with a static private IP of *192.168.1.101*, follow hello steps below:</span></span>

1. <span data-ttu-id="dc2ec-110">Från en webbläsare, navigerar du toohttp://portal.azure.com och, om det behövs, logga in med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="dc2ec-110">From a browser, navigate toohttp://portal.azure.com and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="dc2ec-111">Klicka på **ny** > **Compute** > **Windows Server 2012 R2 Datacenter**, Lägg märke till att hello **Välj en distributionsmodell** redan lista visas **klassiska**, och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="dc2ec-111">Click **NEW** > **Compute** > **Windows Server 2012 R2 Datacenter**, notice that hello **Select a deployment model** list already shows **Classic**, and then click **Create**.</span></span>
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-classic-pportal/figure01.png)
3. <span data-ttu-id="dc2ec-113">I hello **skapa VM** bladet anger hello namnet på hello VM toobe skapade (*DNS01* i vårt exempel), hello lokalt administratörskonto och lösenord.</span><span class="sxs-lookup"><span data-stu-id="dc2ec-113">In hello **Create VM** blade, enter hello name of hello VM toobe created (*DNS01* in our scenario), hello local administrator account, and password.</span></span>
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-classic-pportal/figure02.png)
4. <span data-ttu-id="dc2ec-115">Klicka på **valfri konfiguration** > **nätverk** > **virtuellt nätverk**, och klicka sedan på **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="dc2ec-115">Click **Optional Configuration** > **Network** > **Virtual Network**, and then click **TestVNet**.</span></span> <span data-ttu-id="dc2ec-116">Om **TestVNet** är inte tillgänglig, kontrollera att du använder hello *centrala USA* plats och har skapat hello testmiljön som beskrivs hello början av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="dc2ec-116">If **TestVNet** is not available, make sure you are using hello *Central US* location and have created hello test environment described at hello beginning of this article.</span></span>
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-classic-pportal/figure03.png)
5. <span data-ttu-id="dc2ec-118">I hello **nätverk** bladet, se till att hello undernät markerade är *klientdel*, klicka på **IP-adresser**under **tilldelningavIP-adress** klickar du på **statiska**, och ange sedan *192.168.1.101* för **IP-adress** som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="dc2ec-118">In hello **Network** blade, make sure hello subnet currently selected is *FrontEnd*, then click **IP addresses**, under **IP address assignment** click **Static**, and then enter *192.168.1.101* for **IP Address** as seen below.</span></span>
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-classic-pportal/figure04.png)    
6. <span data-ttu-id="dc2ec-120">Klicka på **OK** i hello **IP-adresser** bladet Klicka **OK** i hello **nätverk** bladet och klickar på **OK**i hello **valfria config** bladet.</span><span class="sxs-lookup"><span data-stu-id="dc2ec-120">Click **OK** in hello **IP addresses** blade, then click **OK** in hello **Network** blade, and click **OK** in hello **Optional config** blade.</span></span>
7. <span data-ttu-id="dc2ec-121">I hello **skapa VM** bladet, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="dc2ec-121">In hello **Create VM** blade, click **Create**.</span></span> <span data-ttu-id="dc2ec-122">Meddelande hello panelen nedan visas i instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="dc2ec-122">Notice hello tile below displayed in your dashboard.</span></span>
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a><span data-ttu-id="dc2ec-124">Hur tooretrieve statiska privata IP-information för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="dc2ec-124">How tooretrieve static private IP address information for a VM</span></span>
<span data-ttu-id="dc2ec-125">tooview hello statisk privat IP-adressinformation för hello skapas den virtuella datorn med hello ovanstående steg, köra hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="dc2ec-125">tooview hello static private IP address information for hello VM created with hello steps above, execute hello steps below.</span></span>

1. <span data-ttu-id="dc2ec-126">Hello Azure Azure-portalen klickar du på **Bläddra bland alla** > **virtuella datorer (klassisk)** > **DNS01**  >   **Alla inställningar** > **IP-adresser** och notera hello tilldelning av IP-adresser och IP-adress som du ser nedan.</span><span class="sxs-lookup"><span data-stu-id="dc2ec-126">From hello Azure Azure portal, click **BROWSE ALL** > **Virtual machines (classic)** > **DNS01** > **All settings** > **IP addresses** and notice hello IP address assignment and IP address as seen below.</span></span>
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a><span data-ttu-id="dc2ec-128">Hur tooremove en statisk privat IP-adress från en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="dc2ec-128">How tooremove a static private IP address from a VM</span></span>
<span data-ttu-id="dc2ec-129">tooremove hello statisk privat IP-adress från hello VM skapade ovan, följ hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="dc2ec-129">tooremove hello static private IP address from hello VM created above, follow hello steps below.</span></span>

1. <span data-ttu-id="dc2ec-130">Från hello **IP-adresser** bladet som visas ovan, klickar du på **dynamiska** toohello höger i **IP-adresstilldelning**, klicka på **spara**, och sedan Klicka på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="dc2ec-130">From hello **IP addresses** blade shown above, click **Dynamic** toohello right of **IP address assignment**, then click **Save**, and then click **Yes**.</span></span>
   
    ![Skapa virtuell dator i Azure-portalen](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-tooadd-a-static-private-ip-address-tooan-existing-vm"></a><span data-ttu-id="dc2ec-132">Hur tooadd en statisk privat IP-adress tooan befintliga VM</span><span class="sxs-lookup"><span data-stu-id="dc2ec-132">How tooadd a static private IP address tooan existing VM</span></span>
<span data-ttu-id="dc2ec-133">tooadd en statisk privat IP-adress toohello VM som skapats med hello anvisningarna ovan, så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="dc2ec-133">tooadd a static private IP address toohello VM created using hello steps above, follow hello steps below:</span></span>

1. <span data-ttu-id="dc2ec-134">Från hello **IP-adresser** bladet som visas ovan, klickar du på **statiska** toohello höger i **IP-adresstilldelning**.</span><span class="sxs-lookup"><span data-stu-id="dc2ec-134">From hello **IP addresses** blade shown above, click **Static** toohello right of **IP address assignment**.</span></span>
2. <span data-ttu-id="dc2ec-135">Typen *192.168.1.101* för **IP-adress**, klicka på **spara**, och klicka sedan på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="dc2ec-135">Type *192.168.1.101* for **IP address**, then click **Save**, and then click **Yes**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dc2ec-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dc2ec-136">Next steps</span></span>
* <span data-ttu-id="dc2ec-137">Lär dig mer om [reserverade offentliga IP-Adressen](virtual-networks-reserved-public-ip.md) adresser.</span><span class="sxs-lookup"><span data-stu-id="dc2ec-137">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="dc2ec-138">Lär dig mer om [instansnivå offentliga IP-går](virtual-networks-instance-level-public-ip.md) adresser.</span><span class="sxs-lookup"><span data-stu-id="dc2ec-138">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="dc2ec-139">Kontakta hello [reserverade IP-REST API: er](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="dc2ec-139">Consult hello [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>

