---
title: "Lägga till flera VPN-gateway för plats-till-plats-anslutningar till ett virtuellt nätverk: Azure-portalen: Resource Manager | Microsoft Docs"
description: "Lägga till flera platser S2S-anslutningar till en VPN-gateway som har en befintlig anslutning"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f3e8b165-f20a-42ab-afbb-bf60974bb4b1
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/20/2017
ms.author: cherylmc
ms.openlocfilehash: 7ec57789ee76f4ec54e4f7b68ea75c19522f3d7c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a><span data-ttu-id="4f970-103">Lägga till en plats-till-plats-anslutning till ett virtuellt nätverk med en befintlig anslutning för VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="4f970-103">Add a Site-to-Site connection to a VNet with an existing VPN gateway connection</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="4f970-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4f970-104">Azure portal</span></span>](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="4f970-105">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="4f970-105">PowerShell (classic)</span></span>](vpn-gateway-multi-site.md)
>
> 

<span data-ttu-id="4f970-106">Den här artikeln vägleder dig genom att använda Azure portal för att lägga till plats-till-plats (S2S)-anslutningar till en VPN-gateway som har en befintlig anslutning.</span><span class="sxs-lookup"><span data-stu-id="4f970-106">This article walks you through using the Azure portal to add Site-to-Site (S2S) connections to a VPN gateway that has an existing connection.</span></span> <span data-ttu-id="4f970-107">Den här typen av anslutning kallas ofta ”flera” platskonfiguration.</span><span class="sxs-lookup"><span data-stu-id="4f970-107">This type of connection is often referred to as a "multi-site" configuration.</span></span> <span data-ttu-id="4f970-108">Du kan lägga till en S2S-anslutning till ett virtuellt nätverk som redan har en S2S-anslutning, punkt-till-plats-anslutning eller VNet-till-VNet-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="4f970-108">You can add a S2S connection to a VNet that already has a S2S connection, Point-to-Site connection, or VNet-to-VNet connection.</span></span> <span data-ttu-id="4f970-109">Det finns vissa begränsningar när du lägger till anslutningar.</span><span class="sxs-lookup"><span data-stu-id="4f970-109">There are some limitations when adding connections.</span></span> <span data-ttu-id="4f970-110">Kontrollera den [innan du börjar](#before) i den här artikeln för att verifiera innan du börjar din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="4f970-110">Check the [Before you begin](#before) section in this article to verify before you start your configuration.</span></span> 

<span data-ttu-id="4f970-111">Den här artikeln gäller för Vnet som skapats med hjälp av Resource Manager-distributionsmodellen som har en RouteBased VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="4f970-111">This article applies to VNets created using the Resource Manager deployment model that have a RouteBased VPN gateway.</span></span> <span data-ttu-id="4f970-112">Dessa steg gäller inte för konfigurationer för samtidiga ExpressRoute/plats-till-plats-anslutning.</span><span class="sxs-lookup"><span data-stu-id="4f970-112">These steps do not apply to ExpressRoute/Site-to-Site coexisting connection configurations.</span></span> <span data-ttu-id="4f970-113">Se [ExpressRoute/S2S samtidiga anslutningar](../expressroute/expressroute-howto-coexist-resource-manager.md) information om samtidiga anslutningar.</span><span class="sxs-lookup"><span data-stu-id="4f970-113">See [ExpressRoute/S2S coexisting connections](../expressroute/expressroute-howto-coexist-resource-manager.md) for information about coexisting connections.</span></span>

### <a name="deployment-models-and-methods"></a><span data-ttu-id="4f970-114">Distributionsmodeller och distributionsmetoder</span><span class="sxs-lookup"><span data-stu-id="4f970-114">Deployment models and methods</span></span>
[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

<span data-ttu-id="4f970-115">Vi uppdatera den här tabellen som nya artiklar och ytterligare verktyg blir tillgängliga för den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="4f970-115">We update this table as new articles and additional tools become available for this configuration.</span></span> <span data-ttu-id="4f970-116">När det finns en artikel länka det till den från den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="4f970-116">When an article is available, we link directly to it from this table.</span></span>

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <span data-ttu-id="4f970-117"><a name="before"></a>Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="4f970-117"><a name="before"></a>Before you begin</span></span>
<span data-ttu-id="4f970-118">Kontrollera följande:</span><span class="sxs-lookup"><span data-stu-id="4f970-118">Verify the following items:</span></span>

* <span data-ttu-id="4f970-119">Du skapar inte en samtidiga ExpressRoute/S2S-anslutning.</span><span class="sxs-lookup"><span data-stu-id="4f970-119">You are not creating an ExpressRoute/S2S coexisting connection.</span></span>
* <span data-ttu-id="4f970-120">Du har ett virtuellt nätverk som har skapats med hjälp av Resource Manager-distributionsmodellen med en befintlig anslutning.</span><span class="sxs-lookup"><span data-stu-id="4f970-120">You have a virtual network that was created using the Resource Manager deployment model with an existing connection.</span></span>
* <span data-ttu-id="4f970-121">Den virtuella nätverksgatewayen för din VNet är RouteBased.</span><span class="sxs-lookup"><span data-stu-id="4f970-121">The virtual network gateway for your VNet is RouteBased.</span></span> <span data-ttu-id="4f970-122">Om du har en PolicyBased VPN-gateway, måste du ta bort den virtuella nätverksgatewayen och skapa en ny VPN-gateway som RouteBased.</span><span class="sxs-lookup"><span data-stu-id="4f970-122">If you have a PolicyBased VPN gateway, you must delete the virtual network gateway and create a new VPN gateway as RouteBased.</span></span>
* <span data-ttu-id="4f970-123">Ingen av adressintervallen överlappa för någon av Vnet som ansluter till detta virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="4f970-123">None of the address ranges overlap for any of the VNets that this VNet is connecting to.</span></span>
* <span data-ttu-id="4f970-124">Du har kompatibla VPN-enhet och någon som går att konfigurera den.</span><span class="sxs-lookup"><span data-stu-id="4f970-124">You have compatible VPN device and someone who is able to configure it.</span></span> <span data-ttu-id="4f970-125">Se [Om VPN-enheter](vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="4f970-125">See [About VPN Devices](vpn-gateway-about-vpn-devices.md).</span></span> <span data-ttu-id="4f970-126">Om du inte vet hur man konfigurerar VPN-enheten eller inte känner till IP-adressintervallen i din lokala nätverkskonfiguration måste du vända dig till någon som kan ge den informationen till dig.</span><span class="sxs-lookup"><span data-stu-id="4f970-126">If you aren't familiar with configuring your VPN device, or are unfamiliar with the IP address ranges located in your on-premises network configuration, you need to coordinate with someone who can provide those details for you.</span></span>
* <span data-ttu-id="4f970-127">Du har ett externt Internetriktade offentliga IP-adressen för VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="4f970-127">You have an externally facing public IP address for your VPN device.</span></span> <span data-ttu-id="4f970-128">Den här IP-adressen får inte finnas bakom en NAT.</span><span class="sxs-lookup"><span data-stu-id="4f970-128">This IP address cannot be located behind a NAT.</span></span>

## <span data-ttu-id="4f970-129"><a name="part1"></a>Del 1 – konfigurera en anslutning</span><span class="sxs-lookup"><span data-stu-id="4f970-129"><a name="part1"></a>Part 1 - Configure a connection</span></span>
1. <span data-ttu-id="4f970-130">Navigera till [Azure-portalen](http://portal.azure.com) från en webbläsare och logga in med ditt Azure-konto vid behov.</span><span class="sxs-lookup"><span data-stu-id="4f970-130">From a browser, navigate to the [Azure portal](http://portal.azure.com) and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="4f970-131">Klicka på **alla resurser** och leta upp din **virtuell nätverksgateway** från listan över resurser och klicka på den.</span><span class="sxs-lookup"><span data-stu-id="4f970-131">Click **All resources** and locate your **virtual network gateway** from the list of resources and click it.</span></span>
3. <span data-ttu-id="4f970-132">På den **virtuell nätverksgateway** bladet, klickar du på **anslutningar**.</span><span class="sxs-lookup"><span data-stu-id="4f970-132">On the **Virtual network gateway** blade, click **Connections**.</span></span>
   
    <span data-ttu-id="4f970-133">![Anslutningar-bladet](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Connections blade")</span><span class="sxs-lookup"><span data-stu-id="4f970-133">![Connections blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Connections blade")</span></span><br>
4. <span data-ttu-id="4f970-134">På den **anslutningar** bladet, klickar du på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="4f970-134">On the **Connections** blade, click **+Add**.</span></span>
   
    <span data-ttu-id="4f970-135">![Lägg till anslutning](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Add connection button")</span><span class="sxs-lookup"><span data-stu-id="4f970-135">![Add connection button](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Add connection button")</span></span><br>
5. <span data-ttu-id="4f970-136">På den **Lägg till anslutning** bladet, Fyll i följande fält:</span><span class="sxs-lookup"><span data-stu-id="4f970-136">On the **Add connection** blade, fill out the following fields:</span></span>
   
   * <span data-ttu-id="4f970-137">**Namn:** namn som du vill ge till platsen du skapar en anslutning till.</span><span class="sxs-lookup"><span data-stu-id="4f970-137">**Name:** The name you want to give to the site you are creating the connection to.</span></span>
   * <span data-ttu-id="4f970-138">**Anslutningstyp:** Välj **plats-till-plats (IPsec)**.</span><span class="sxs-lookup"><span data-stu-id="4f970-138">**Connection type:** Select **Site-to-site (IPsec)**.</span></span>
     
     <span data-ttu-id="4f970-139">![Lägg till anslutning bladet](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Add connection blade")</span><span class="sxs-lookup"><span data-stu-id="4f970-139">![Add connection blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Add connection blade")</span></span><br>

## <span data-ttu-id="4f970-140"><a name="part2"></a>Del 2 – Lägg till en lokal nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="4f970-140"><a name="part2"></a>Part 2 - Add a local network gateway</span></span>
1. <span data-ttu-id="4f970-141">Klicka på **lokal nätverksgateway** ***Välj en lokal nätverksgateway***.</span><span class="sxs-lookup"><span data-stu-id="4f970-141">Click **Local network gateway** ***Choose a local network gateway***.</span></span> <span data-ttu-id="4f970-142">Då öppnas den **Välj lokal nätverksgateway** bladet.</span><span class="sxs-lookup"><span data-stu-id="4f970-142">This will open the **Choose local network gateway** blade.</span></span>
   
    <span data-ttu-id="4f970-143">![Välj lokal nätverksgateway](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Choose local network gateway")</span><span class="sxs-lookup"><span data-stu-id="4f970-143">![Choose local network gateway](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Choose local network gateway")</span></span><br>
2. <span data-ttu-id="4f970-144">Klicka på **Skapa nytt** att öppna den **skapa lokal nätverksgateway** bladet.</span><span class="sxs-lookup"><span data-stu-id="4f970-144">Click **Create new** to open the **Create local network gateway** blade.</span></span>
   
    <span data-ttu-id="4f970-145">![Skapa lokal nätverksgateway-bladet](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "Create local network gateway")</span><span class="sxs-lookup"><span data-stu-id="4f970-145">![Create local network gateway blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "Create local network gateway")</span></span><br>
3. <span data-ttu-id="4f970-146">På den **skapa lokal nätverksgateway** bladet, Fyll i följande fält:</span><span class="sxs-lookup"><span data-stu-id="4f970-146">On the **Create local network gateway** blade, fill out the following fields:</span></span>
   
   * <span data-ttu-id="4f970-147">**Namn:** namn som du vill ge till den lokala gateway-nätverksresursen.</span><span class="sxs-lookup"><span data-stu-id="4f970-147">**Name:** The name you want to give to the local network gateway resource.</span></span>
   * <span data-ttu-id="4f970-148">**IP-adress:** offentliga IP-adressen för VPN-enhet på den plats som du vill ansluta till.</span><span class="sxs-lookup"><span data-stu-id="4f970-148">**IP address:** The public IP address of the VPN device on the site that you want to connect to.</span></span>
   * <span data-ttu-id="4f970-149">**Adressutrymmet:** adressutrymmet som du vill att dirigeras till den nya lokala nätverksplatsen.</span><span class="sxs-lookup"><span data-stu-id="4f970-149">**Address space:** The address space that you want to be routed to the new local network site.</span></span>
4. <span data-ttu-id="4f970-150">Klicka på **OK** på den **skapa lokal nätverksgateway** bladet för att spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="4f970-150">Click **OK** on the **Create local network gateway** blade to save the changes.</span></span>

## <span data-ttu-id="4f970-151"><a name="part3"></a>Del 3 – Lägg till den delade nyckeln och skapa anslutningen</span><span class="sxs-lookup"><span data-stu-id="4f970-151"><a name="part3"></a>Part 3 - Add the shared key and create the connection</span></span>
1. <span data-ttu-id="4f970-152">På den **Lägg till anslutning** bladet Lägg till den delade nyckeln som du vill använda för att skapa anslutningen.</span><span class="sxs-lookup"><span data-stu-id="4f970-152">On the **Add connection** blade, add the shared key that you want to use to create your connection.</span></span> <span data-ttu-id="4f970-153">Du kan hämta den delade nyckeln från din VPN-enhet eller skapa en här och sedan konfigurera din VPN-enhet om du vill använda samma delade nyckel.</span><span class="sxs-lookup"><span data-stu-id="4f970-153">You can either get the shared key from your VPN device, or make one up here and then configure your VPN device to use the same shared key.</span></span> <span data-ttu-id="4f970-154">Viktigt är att de nycklarna är samma.</span><span class="sxs-lookup"><span data-stu-id="4f970-154">The important thing is that the keys are exactly the same.</span></span>
   
    <span data-ttu-id="4f970-155">![Delad nyckel](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Shared key")</span><span class="sxs-lookup"><span data-stu-id="4f970-155">![Shared key](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Shared key")</span></span><br>
2. <span data-ttu-id="4f970-156">Längst ned på bladet klickar du på **OK** att skapa anslutningen.</span><span class="sxs-lookup"><span data-stu-id="4f970-156">At the bottom of the blade, click **OK** to create the connection.</span></span>

## <span data-ttu-id="4f970-157"><a name="part4"></a>Del 4 – verifiera VPN-anslutningen</span><span class="sxs-lookup"><span data-stu-id="4f970-157"><a name="part4"></a>Part 4 - Verify the VPN connection</span></span>


[!INCLUDE [vpn-gateway-verify-connection-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="next-steps"></a><span data-ttu-id="4f970-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4f970-158">Next steps</span></span>

<span data-ttu-id="4f970-159">När anslutningen är klar kan du lägga till virtuella datorer till dina virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="4f970-159">Once your connection is complete, you can add virtual machines to your virtual networks.</span></span> <span data-ttu-id="4f970-160">Se de virtuella datorernas [utbildningsväg](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) för mer information.</span><span class="sxs-lookup"><span data-stu-id="4f970-160">See the virtual machines [learning path](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) for more information.</span></span>
