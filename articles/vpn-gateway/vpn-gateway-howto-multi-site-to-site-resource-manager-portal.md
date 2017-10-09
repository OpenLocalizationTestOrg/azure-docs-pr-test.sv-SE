---
title: "Lägga till flera VPN-gateway för plats-till-plats-anslutningar tooa VNet: Azure-portalen: Resource Manager | Microsoft Docs"
description: "Lägga till flera platser S2S-anslutningar tooa VPN-gateway som har en befintlig anslutning"
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
ms.openlocfilehash: b8c9ff454967f509dcef725f8bcec8564fad9b00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-site-to-site-connection-tooa-vnet-with-an-existing-vpn-gateway-connection"></a><span data-ttu-id="d5d3f-103">Lägg till en plats-till-plats-anslutning tooa VNet med en befintlig anslutning för VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="d5d3f-103">Add a Site-to-Site connection tooa VNet with an existing VPN gateway connection</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="d5d3f-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d5d3f-104">Azure portal</span></span>](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="d5d3f-105">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="d5d3f-105">PowerShell (classic)</span></span>](vpn-gateway-multi-site.md)
>
> 

<span data-ttu-id="d5d3f-106">Den här artikeln vägleder dig genom att använda hello Azure portal tooadd plats-till-plats (S2S) anslutningar tooa VPN-gateway som har en befintlig anslutning.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-106">This article walks you through using hello Azure portal tooadd Site-to-Site (S2S) connections tooa VPN gateway that has an existing connection.</span></span> <span data-ttu-id="d5d3f-107">Den här typen av anslutning är ofta hänvisas tooas ”flera” platskonfiguration.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-107">This type of connection is often referred tooas a "multi-site" configuration.</span></span> <span data-ttu-id="d5d3f-108">Du kan lägga till en S2S-anslutning tooa VNet som redan har en S2S-anslutning, punkt-till-plats-anslutning eller VNet-till-VNet-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-108">You can add a S2S connection tooa VNet that already has a S2S connection, Point-to-Site connection, or VNet-to-VNet connection.</span></span> <span data-ttu-id="d5d3f-109">Det finns vissa begränsningar när du lägger till anslutningar.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-109">There are some limitations when adding connections.</span></span> <span data-ttu-id="d5d3f-110">Kontrollera hello [innan du börjar](#before) avsnitt i den här artikeln tooverify innan du börjar din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-110">Check hello [Before you begin](#before) section in this article tooverify before you start your configuration.</span></span> 

<span data-ttu-id="d5d3f-111">Den här artikeln gäller tooVNets som skapats med hjälp av hello Resource Manager-modellen som har en RouteBased VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-111">This article applies tooVNets created using hello Resource Manager deployment model that have a RouteBased VPN gateway.</span></span> <span data-ttu-id="d5d3f-112">De här stegen gäller inte konfigurationer för samtidiga tooExpressRoute/plats-till-plats-anslutning.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-112">These steps do not apply tooExpressRoute/Site-to-Site coexisting connection configurations.</span></span> <span data-ttu-id="d5d3f-113">Se [ExpressRoute/S2S samtidiga anslutningar](../expressroute/expressroute-howto-coexist-resource-manager.md) information om samtidiga anslutningar.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-113">See [ExpressRoute/S2S coexisting connections](../expressroute/expressroute-howto-coexist-resource-manager.md) for information about coexisting connections.</span></span>

### <a name="deployment-models-and-methods"></a><span data-ttu-id="d5d3f-114">Distributionsmodeller och distributionsmetoder</span><span class="sxs-lookup"><span data-stu-id="d5d3f-114">Deployment models and methods</span></span>
[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

<span data-ttu-id="d5d3f-115">Vi uppdatera den här tabellen som nya artiklar och ytterligare verktyg blir tillgängliga för den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-115">We update this table as new articles and additional tools become available for this configuration.</span></span> <span data-ttu-id="d5d3f-116">När det finns en artikel länkar vi direkt tooit från den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-116">When an article is available, we link directly tooit from this table.</span></span>

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <span data-ttu-id="d5d3f-117"><a name="before"></a>Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="d5d3f-117"><a name="before"></a>Before you begin</span></span>
<span data-ttu-id="d5d3f-118">Kontrollera hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="d5d3f-118">Verify hello following items:</span></span>

* <span data-ttu-id="d5d3f-119">Du skapar inte en samtidiga ExpressRoute/S2S-anslutning.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-119">You are not creating an ExpressRoute/S2S coexisting connection.</span></span>
* <span data-ttu-id="d5d3f-120">Du har ett virtuellt nätverk som har skapats med en befintlig anslutning hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-120">You have a virtual network that was created using hello Resource Manager deployment model with an existing connection.</span></span>
* <span data-ttu-id="d5d3f-121">hello virtuell nätverksgateway för din VNet är RouteBased.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-121">hello virtual network gateway for your VNet is RouteBased.</span></span> <span data-ttu-id="d5d3f-122">Om du har en PolicyBased VPN-gateway, måste du ta bort hello virtuell nätverksgateway och skapa en ny VPN-gateway som RouteBased.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-122">If you have a PolicyBased VPN gateway, you must delete hello virtual network gateway and create a new VPN gateway as RouteBased.</span></span>
* <span data-ttu-id="d5d3f-123">Ingen av hello adressintervall överlappa för någon av hello Vnet som ansluter till detta virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-123">None of hello address ranges overlap for any of hello VNets that this VNet is connecting to.</span></span>
* <span data-ttu-id="d5d3f-124">Du har kompatibla VPN-enhet och någon som kan tooconfigure den.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-124">You have compatible VPN device and someone who is able tooconfigure it.</span></span> <span data-ttu-id="d5d3f-125">Se [Om VPN-enheter](vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="d5d3f-125">See [About VPN Devices](vpn-gateway-about-vpn-devices.md).</span></span> <span data-ttu-id="d5d3f-126">Om du inte är bekant med att konfigurera din VPN-enhet eller inte är bekant med hello IP-adressintervall som finns i din lokala nätverkskonfigurationen, måste toocoordinate med någon som kan ge de detaljer du.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-126">If you aren't familiar with configuring your VPN device, or are unfamiliar with hello IP address ranges located in your on-premises network configuration, you need toocoordinate with someone who can provide those details for you.</span></span>
* <span data-ttu-id="d5d3f-127">Du har ett externt Internetriktade offentliga IP-adressen för VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-127">You have an externally facing public IP address for your VPN device.</span></span> <span data-ttu-id="d5d3f-128">Den här IP-adressen får inte finnas bakom en NAT.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-128">This IP address cannot be located behind a NAT.</span></span>

## <span data-ttu-id="d5d3f-129"><a name="part1"></a>Del 1 – konfigurera en anslutning</span><span class="sxs-lookup"><span data-stu-id="d5d3f-129"><a name="part1"></a>Part 1 - Configure a connection</span></span>
1. <span data-ttu-id="d5d3f-130">Från en webbläsare, navigerar du toohello [Azure-portalen](http://portal.azure.com) och vid behov, logga in med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-130">From a browser, navigate toohello [Azure portal](http://portal.azure.com) and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="d5d3f-131">Klicka på **alla resurser** och leta upp din **virtuell nätverksgateway** hello listan över resurser och klicka på den.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-131">Click **All resources** and locate your **virtual network gateway** from hello list of resources and click it.</span></span>
3. <span data-ttu-id="d5d3f-132">På hello **virtuell nätverksgateway** bladet, klickar du på **anslutningar**.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-132">On hello **Virtual network gateway** blade, click **Connections**.</span></span>
   
    <span data-ttu-id="d5d3f-133">![Bladet Anslutningar](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Bladet Anslutningar")</span><span class="sxs-lookup"><span data-stu-id="d5d3f-133">![Connections blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Connections blade")</span></span><br>
4. <span data-ttu-id="d5d3f-134">På hello **anslutningar** bladet, klickar du på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-134">On hello **Connections** blade, click **+Add**.</span></span>
   
    <span data-ttu-id="d5d3f-135">![Lägg till anslutning för](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Lägg till anslutning för")</span><span class="sxs-lookup"><span data-stu-id="d5d3f-135">![Add connection button](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Add connection button")</span></span><br>
5. <span data-ttu-id="d5d3f-136">På hello **Lägg till anslutning** bladet registrerar hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="d5d3f-136">On hello **Add connection** blade, fill out hello following fields:</span></span>
   
   * <span data-ttu-id="d5d3f-137">**Namn:** hello-namn som du vill toogive toohello plats som du skapar hello anslutning till.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-137">**Name:** hello name you want toogive toohello site you are creating hello connection to.</span></span>
   * <span data-ttu-id="d5d3f-138">**Anslutningstyp:** Välj **plats-till-plats (IPsec)**.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-138">**Connection type:** Select **Site-to-site (IPsec)**.</span></span>
     
     <span data-ttu-id="d5d3f-139">![Lägg till anslutning bladet](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Lägg till anslutning bladet")</span><span class="sxs-lookup"><span data-stu-id="d5d3f-139">![Add connection blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Add connection blade")</span></span><br>

## <span data-ttu-id="d5d3f-140"><a name="part2"></a>Del 2 – Lägg till en lokal nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="d5d3f-140"><a name="part2"></a>Part 2 - Add a local network gateway</span></span>
1. <span data-ttu-id="d5d3f-141">Klicka på **lokal nätverksgateway** ***Välj en lokal nätverksgateway***.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-141">Click **Local network gateway** ***Choose a local network gateway***.</span></span> <span data-ttu-id="d5d3f-142">Då öppnas hello **Välj lokal nätverksgateway** bladet.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-142">This will open hello **Choose local network gateway** blade.</span></span>
   
    <span data-ttu-id="d5d3f-143">![Välj lokal nätverksgateway](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Välj lokal nätverksgateway")</span><span class="sxs-lookup"><span data-stu-id="d5d3f-143">![Choose local network gateway](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Choose local network gateway")</span></span><br>
2. <span data-ttu-id="d5d3f-144">Klicka på **Skapa nytt** tooopen hello **skapa lokal nätverksgateway** bladet.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-144">Click **Create new** tooopen hello **Create local network gateway** blade.</span></span>
   
    <span data-ttu-id="d5d3f-145">![Skapa lokal nätverksgateway-bladet](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "skapa lokal nätverksgateway")</span><span class="sxs-lookup"><span data-stu-id="d5d3f-145">![Create local network gateway blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "Create local network gateway")</span></span><br>
3. <span data-ttu-id="d5d3f-146">På hello **skapa lokal nätverksgateway** bladet registrerar hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="d5d3f-146">On hello **Create local network gateway** blade, fill out hello following fields:</span></span>
   
   * <span data-ttu-id="d5d3f-147">**Namn:** hello-namn som du vill toogive toohello lokala gateway nätverksresurs.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-147">**Name:** hello name you want toogive toohello local network gateway resource.</span></span>
   * <span data-ttu-id="d5d3f-148">**IP-adress:** hello hello VPN-enheten på hello-plats som du vill tooconnect till offentliga IP-adress.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-148">**IP address:** hello public IP address of hello VPN device on hello site that you want tooconnect to.</span></span>
   * <span data-ttu-id="d5d3f-149">**Adressutrymmet:** hello-adressutrymmet som du vill toobe dirigeras toohello ny lokal nätverksplats.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-149">**Address space:** hello address space that you want toobe routed toohello new local network site.</span></span>
4. <span data-ttu-id="d5d3f-150">Klicka på **OK** på hello **skapa lokal nätverksgateway** bladet toosave hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-150">Click **OK** on hello **Create local network gateway** blade toosave hello changes.</span></span>

## <span data-ttu-id="d5d3f-151"><a name="part3"></a>Del 3 – Lägg till hello delad nyckel och skapa hello-anslutning</span><span class="sxs-lookup"><span data-stu-id="d5d3f-151"><a name="part3"></a>Part 3 - Add hello shared key and create hello connection</span></span>
1. <span data-ttu-id="d5d3f-152">På hello **Lägg till anslutning** bladet Lägg till hello delad nyckel som du vill toouse toocreate anslutningen.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-152">On hello **Add connection** blade, add hello shared key that you want toouse toocreate your connection.</span></span> <span data-ttu-id="d5d3f-153">Du kan hämta hello delade nyckeln från din VPN-enhet eller skapa en här och konfigurera din VPN-enhet toouse hello samma delad nyckel.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-153">You can either get hello shared key from your VPN device, or make one up here and then configure your VPN device toouse hello same shared key.</span></span> <span data-ttu-id="d5d3f-154">hello viktig sak är att hello nycklar är exakt hello samma.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-154">hello important thing is that hello keys are exactly hello same.</span></span>
   
    <span data-ttu-id="d5d3f-155">![Delad nyckel](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Delad nyckel")</span><span class="sxs-lookup"><span data-stu-id="d5d3f-155">![Shared key](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Shared key")</span></span><br>
2. <span data-ttu-id="d5d3f-156">Hello längst ned på hello-bladet, klickar du på **OK** toocreate hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-156">At hello bottom of hello blade, click **OK** toocreate hello connection.</span></span>

## <span data-ttu-id="d5d3f-157"><a name="part4"></a>Del 4 – verifiera hello VPN-anslutning</span><span class="sxs-lookup"><span data-stu-id="d5d3f-157"><a name="part4"></a>Part 4 - Verify hello VPN connection</span></span>


[!INCLUDE [vpn-gateway-verify-connection-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="next-steps"></a><span data-ttu-id="d5d3f-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d5d3f-158">Next steps</span></span>

<span data-ttu-id="d5d3f-159">När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-159">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="d5d3f-160">Se hello virtuella datorer [Utbildningsväg](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) för mer information.</span><span class="sxs-lookup"><span data-stu-id="d5d3f-160">See hello virtual machines [learning path](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) for more information.</span></span>
