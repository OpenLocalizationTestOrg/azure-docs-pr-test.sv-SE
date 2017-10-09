---
title: "Ansluta din lokala nätverk tooan virtuella Azure-nätverket: plats-till-plats-VPN: Portal | Microsoft Docs"
description: "Steg toocreate en IPSec-anslutning från ditt lokala nätverk tooan virtuella Azure-nätverket via hello offentliga Internet. Dessa steg hjälper dig att skapa en anslutning för plats-till-plats VPN-Gateway av mellan platser med hjälp av hello portal."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 827a4db7-7fa5-4eaf-b7e1-e1518c51c815
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: 6f0acbaf1bf016026cefade048a116e94686103d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-site-to-site-connection-in-hello-azure-portal"></a><span data-ttu-id="73add-104">Skapa en plats-till-plats-anslutning i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="73add-104">Create a Site-to-Site connection in hello Azure portal</span></span>

<span data-ttu-id="73add-105">Den här artikeln visar hur hello toouse Azure portal toocreate plats-till-plats VPN-gateway-anslutningen från ditt lokala nätverk toohello VNet.</span><span class="sxs-lookup"><span data-stu-id="73add-105">This article shows you how toouse hello Azure portal toocreate a Site-to-Site VPN gateway connection from your on-premises network toohello VNet.</span></span> <span data-ttu-id="73add-106">hello stegen i den här artikeln gäller toohello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="73add-106">hello steps in this article apply toohello Resource Manager deployment model.</span></span> <span data-ttu-id="73add-107">Du kan också skapa den här konfigurationen med hjälp av en annan distributionsverktyget eller distributionsmodell genom att välja ett annat alternativ hello följande lista:</span><span class="sxs-lookup"><span data-stu-id="73add-107">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="73add-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="73add-108">Azure portal</span></span>](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="73add-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="73add-109">PowerShell</span></span>](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [<span data-ttu-id="73add-110">CLI</span><span class="sxs-lookup"><span data-stu-id="73add-110">CLI</span></span>](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [<span data-ttu-id="73add-111">Azure Portal (klassisk)</span><span class="sxs-lookup"><span data-stu-id="73add-111">Azure portal (classic)</span></span>](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>

<span data-ttu-id="73add-112">En plats-till-plats VPN-gateway-anslutningen har använt tooconnect ditt lokala nätverk tooan virtuella Azure-nätverket via en IPsec/IKE (IKEv1 eller IKEv2) VPN-tunnel.</span><span class="sxs-lookup"><span data-stu-id="73add-112">A Site-to-Site VPN gateway connection is used tooconnect your on-premises network tooan Azure virtual network over an IPsec/IKE (IKEv1 or IKEv2) VPN tunnel.</span></span> <span data-ttu-id="73add-113">Den här typen av anslutning kräver en VPN-enhet som finns lokalt som har ett externt Internetriktade offentliga IP-adress som tilldelats tooit.</span><span class="sxs-lookup"><span data-stu-id="73add-113">This type of connection requires a VPN device located on-premises that has an externally facing public IP address assigned tooit.</span></span> <span data-ttu-id="73add-114">Mer information om VPN-gatewayer finns i [Om VPN-gateway](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="73add-114">For more information about VPN gateways, see [About VPN gateway](vpn-gateway-about-vpngateways.md).</span></span>

![Diagram över plats-till-plats-anslutning med VPN-gateway](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/site-to-site-diagram.png)

## <a name="before-you-begin"></a><span data-ttu-id="73add-116">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="73add-116">Before you begin</span></span>

<span data-ttu-id="73add-117">Kontrollera att du har uppfyllt hello följande villkor innan du börjar din konfiguration:</span><span class="sxs-lookup"><span data-stu-id="73add-117">Verify that you have met hello following criteria before beginning your configuration:</span></span>

* <span data-ttu-id="73add-118">Kontrollera att du har en kompatibel VPN-enhet och någon som kan tooconfigure den.</span><span class="sxs-lookup"><span data-stu-id="73add-118">Make sure you have a compatible VPN device and someone who is able tooconfigure it.</span></span> <span data-ttu-id="73add-119">Se [Om VPN-enheter](vpn-gateway-about-vpn-devices.md) för mer information om kompatibla VPN-enheter och enhetskonfiguration.</span><span class="sxs-lookup"><span data-stu-id="73add-119">For more information about compatible VPN devices and device configuration, see [About VPN Devices](vpn-gateway-about-vpn-devices.md).</span></span>
* <span data-ttu-id="73add-120">Kontrollera att du har en extern offentlig IPv4-adress för VPN-enheten.</span><span class="sxs-lookup"><span data-stu-id="73add-120">Verify that you have an externally facing public IPv4 address for your VPN device.</span></span> <span data-ttu-id="73add-121">Den här IP-adressen får inte finnas bakom en NAT.</span><span class="sxs-lookup"><span data-stu-id="73add-121">This IP address cannot be located behind a NAT.</span></span>
* <span data-ttu-id="73add-122">Om du är bekant med hello IP-adressintervall som finns i ditt lokala nätverk konfiguration måste du toocoordinate med någon som kan ge de detaljer du.</span><span class="sxs-lookup"><span data-stu-id="73add-122">If you are unfamiliar with hello IP address ranges located in your on-premises network configuration, you need toocoordinate with someone who can provide those details for you.</span></span> <span data-ttu-id="73add-123">När du skapar den här konfigurationen måste du ange hello IP-adressintervall adressprefix Azure dirigerar tooyour lokal plats.</span><span class="sxs-lookup"><span data-stu-id="73add-123">When you create this configuration, you must specify hello IP address range prefixes that Azure will route tooyour on-premises location.</span></span> <span data-ttu-id="73add-124">Ingen av hello undernät i ditt lokala nätverk kan över lap med hello virtuella undernät som du vill tooconnect till.</span><span class="sxs-lookup"><span data-stu-id="73add-124">None of hello subnets of your on-premises network can over lap with hello virtual network subnets that you want tooconnect to.</span></span> 

### <span data-ttu-id="73add-125"><a name="values"></a>Exempelvärden</span><span class="sxs-lookup"><span data-stu-id="73add-125"><a name="values"></a>Example values</span></span>

<span data-ttu-id="73add-126">hello exemplen i den här artikeln används hello följande värden.</span><span class="sxs-lookup"><span data-stu-id="73add-126">hello examples in this article use hello following values.</span></span> <span data-ttu-id="73add-127">Du kan använda dessa värden toocreate en testmiljö eller referera toothem toobetter förstå hello exemplen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="73add-127">You can use these values toocreate a test environment, or refer toothem toobetter understand hello examples in this article.</span></span>

* <span data-ttu-id="73add-128">**VNet-namn:** TestVNet1</span><span class="sxs-lookup"><span data-stu-id="73add-128">**VNet Name:** TestVNet1</span></span>
* <span data-ttu-id="73add-129">**Adressutrymme:**</span><span class="sxs-lookup"><span data-stu-id="73add-129">**Address Space:**</span></span> 
  * <span data-ttu-id="73add-130">10.11.0.0/16</span><span class="sxs-lookup"><span data-stu-id="73add-130">10.11.0.0/16</span></span>
  * <span data-ttu-id="73add-131">10.12.0.0/16 (valfritt för den här övningen)</span><span class="sxs-lookup"><span data-stu-id="73add-131">10.12.0.0/16 (optional for this exercise)</span></span>
* <span data-ttu-id="73add-132">**Undernät:**</span><span class="sxs-lookup"><span data-stu-id="73add-132">**Subnets:**</span></span>
  * <span data-ttu-id="73add-133">FrontEnd: 10.11.0.0/24</span><span class="sxs-lookup"><span data-stu-id="73add-133">FrontEnd: 10.11.0.0/24</span></span>
  * <span data-ttu-id="73add-134">BackEnd: 10.12.0.0/24 (valfritt för den här övningen)</span><span class="sxs-lookup"><span data-stu-id="73add-134">BackEnd: 10.12.0.0/24 (optional for this exercise)</span></span>
* <span data-ttu-id="73add-135">**GatewaySubnet:** 10.11.255.0/27</span><span class="sxs-lookup"><span data-stu-id="73add-135">**GatewaySubnet:** 10.11.255.0/27</span></span>
* <span data-ttu-id="73add-136">**Resursgrupp:** TestRG1</span><span class="sxs-lookup"><span data-stu-id="73add-136">**Resource Group:** TestRG1</span></span>
* <span data-ttu-id="73add-137">**Plats:** Östra USA</span><span class="sxs-lookup"><span data-stu-id="73add-137">**Location:** East US</span></span>
* <span data-ttu-id="73add-138">**DNS Server:** Valfritt.</span><span class="sxs-lookup"><span data-stu-id="73add-138">**DNS Server:** Optional.</span></span> <span data-ttu-id="73add-139">hello IP-adressen för DNS-servern.</span><span class="sxs-lookup"><span data-stu-id="73add-139">hello IP address of your DNS server.</span></span>
* <span data-ttu-id="73add-140">**Namn på virtuell nätverksgateway:** VNet1GW</span><span class="sxs-lookup"><span data-stu-id="73add-140">**Virtual Network Gateway Name:** VNet1GW</span></span>
* <span data-ttu-id="73add-141">**Offentlig IP:** VNet1GWIP</span><span class="sxs-lookup"><span data-stu-id="73add-141">**Public IP:** VNet1GWIP</span></span>
* <span data-ttu-id="73add-142">**VPN-typ:** Routningsbaserad</span><span class="sxs-lookup"><span data-stu-id="73add-142">**VPN Type:** Route-based</span></span>
* <span data-ttu-id="73add-143">**Anslutningstyp:** Plats-till-plats (IPsec)</span><span class="sxs-lookup"><span data-stu-id="73add-143">**Connection Type:** Site-to-site (IPsec)</span></span>
* <span data-ttu-id="73add-144">**Gateway-typ:** VPN</span><span class="sxs-lookup"><span data-stu-id="73add-144">**Gateway Type:** VPN</span></span>
* <span data-ttu-id="73add-145">**Gateway-namn på lokalt nätverk:** Site2</span><span class="sxs-lookup"><span data-stu-id="73add-145">**Local Network Gateway Name:** Site2</span></span>
* <span data-ttu-id="73add-146">**Anslutningsnamn:** VNet1toSite2</span><span class="sxs-lookup"><span data-stu-id="73add-146">**Connection Name:** VNet1toSite2</span></span>

## <span data-ttu-id="73add-147"><a name="CreatVNet"></a>1. Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="73add-147"><a name="CreatVNet"></a>1. Create a virtual network</span></span>

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-s2s-rm-portal-include.md)]

## <span data-ttu-id="73add-148"><a name="dns"></a>2. Ange en DNS-server</span><span class="sxs-lookup"><span data-stu-id="73add-148"><a name="dns"></a>2. Specify a DNS server</span></span>

<span data-ttu-id="73add-149">DNS är inte obligatoriska toocreate plats-till-plats-anslutning.</span><span class="sxs-lookup"><span data-stu-id="73add-149">DNS is not required toocreate a Site-to-Site connection.</span></span> <span data-ttu-id="73add-150">Om du vill toohave namnmatchning för resurser som är distribuerade tooyour virtuella nätverk, ska du ange en DNS-server.</span><span class="sxs-lookup"><span data-stu-id="73add-150">However, if you want toohave name resolution for resources that are deployed tooyour virtual network, you should specify a DNS server.</span></span> <span data-ttu-id="73add-151">Den här inställningen kan du ange hello DNS-server som du vill toouse för namnmatchning för det här virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="73add-151">This setting lets you specify hello DNS server that you want toouse for name resolution for this virtual network.</span></span> <span data-ttu-id="73add-152">Den skapar inte någon DNS-server.</span><span class="sxs-lookup"><span data-stu-id="73add-152">It does not create a DNS server.</span></span> <span data-ttu-id="73add-153">Mer information om namnmatchning finns i [Namnmatchning för virtuella datorer](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span><span class="sxs-lookup"><span data-stu-id="73add-153">For more information about name resolution, see [Name Resolution for VMs and role instances](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).</span></span>

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <span data-ttu-id="73add-154"><a name="gatewaysubnet"></a>3. Skapa hello gateway-undernät</span><span class="sxs-lookup"><span data-stu-id="73add-154"><a name="gatewaysubnet"></a>3. Create hello gateway subnet</span></span>

[!INCLUDE [vpn-gateway-aboutgwsubnet](../../includes/vpn-gateway-about-gwsubnet-include.md)]

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-s2s-rm-portal-include.md)]

## <span data-ttu-id="73add-155"><a name="VNetGateway"></a>4. Skapa hello VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="73add-155"><a name="VNetGateway"></a>4. Create hello VPN gateway</span></span>

[!INCLUDE [vpn-gateway-add-gw-s2s-rm-portal](../../includes/vpn-gateway-add-gw-s2s-rm-portal-include.md)]

## <span data-ttu-id="73add-156"><a name="LocalNetworkGateway"></a>5. Skapa hello lokal nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="73add-156"><a name="LocalNetworkGateway"></a>5. Create hello local network gateway</span></span>

<span data-ttu-id="73add-157">hello lokal nätverksgateway refererar vanligtvis tooyour lokal plats.</span><span class="sxs-lookup"><span data-stu-id="73add-157">hello local network gateway typically refers tooyour on-premises location.</span></span> <span data-ttu-id="73add-158">Du namnge hello plats som Azure kan se tooit och sedan ange hello IP-adress för hello lokala VPN-enhet toowhich ska du skapa en anslutning.</span><span class="sxs-lookup"><span data-stu-id="73add-158">You give hello site a name by which Azure can refer tooit, then specify hello IP address of hello on-premises VPN device toowhich you will create a connection.</span></span> <span data-ttu-id="73add-159">Du kan också ange hello IP-adressprefix som vidarebefordras via hello VPN-gateway toohello VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="73add-159">You also specify hello IP address prefixes that will be routed through hello VPN gateway toohello VPN device.</span></span> <span data-ttu-id="73add-160">hello-adressprefix som du anger är hello prefix finns i ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="73add-160">hello address prefixes you specify are hello prefixes located on your on-premises network.</span></span> <span data-ttu-id="73add-161">Om ändringarna lokala nätverk eller om du behöver toochange hello offentliga IP-adressen för hello VPN-enhet kan uppdatera du enkelt hello värden senare.</span><span class="sxs-lookup"><span data-stu-id="73add-161">If your on-premises network changes or you need toochange hello public IP address for hello VPN device, you can easily update hello values later.</span></span>

[!INCLUDE [Add local network gateway](../../includes/vpn-gateway-add-lng-s2s-rm-portal-include.md)]

## <span data-ttu-id="73add-162"><a name="VPNDevice"></a>6. Konfigurera din VPN-enhet</span><span class="sxs-lookup"><span data-stu-id="73add-162"><a name="VPNDevice"></a>6. Configure your VPN device</span></span>

<span data-ttu-id="73add-163">Plats-till-platsanslutningar tooan lokalt nätverk kräver en VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="73add-163">Site-to-Site connections tooan on-premises network require a VPN device.</span></span> <span data-ttu-id="73add-164">I det här steget konfigurerar du VPN-enheten.</span><span class="sxs-lookup"><span data-stu-id="73add-164">In this step, you configure your VPN device.</span></span> <span data-ttu-id="73add-165">När du konfigurerar VPN-enhet behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="73add-165">When configuring your VPN device, you need hello following:</span></span>

- <span data-ttu-id="73add-166">En delad nyckel.</span><span class="sxs-lookup"><span data-stu-id="73add-166">A shared key.</span></span> <span data-ttu-id="73add-167">Detta är hello samma delade nyckel som du anger när du skapar din plats-till-plats VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="73add-167">This is hello same shared key that you specify when creating your Site-to-Site VPN connection.</span></span> <span data-ttu-id="73add-168">I vårt exempel använder vi en enkel delad nyckel.</span><span class="sxs-lookup"><span data-stu-id="73add-168">In our examples, we use a basic shared key.</span></span> <span data-ttu-id="73add-169">Vi rekommenderar att du generera en mer komplex viktiga toouse.</span><span class="sxs-lookup"><span data-stu-id="73add-169">We recommend that you generate a more complex key toouse.</span></span>
- <span data-ttu-id="73add-170">hello offentliga IP-adressen för din virtuella nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="73add-170">hello Public IP address of your virtual network gateway.</span></span> <span data-ttu-id="73add-171">Du kan visa hello offentliga IP-adressen med hjälp av hello Azure-portalen, PowerShell eller CLI.</span><span class="sxs-lookup"><span data-stu-id="73add-171">You can view hello public IP address by using hello Azure portal, PowerShell, or CLI.</span></span> <span data-ttu-id="73add-172">Navigera toofind hello offentliga IP-adressen för din VPN-gateway med hello Azure-portalen för**virtuella nätverksgatewayer**, klicka på hello namnet på din gateway.</span><span class="sxs-lookup"><span data-stu-id="73add-172">toofind hello Public IP address of your VPN gateway using hello Azure portal, navigate too**Virtual network gateways**, then click hello name of your gateway.</span></span>

[!INCLUDE [Configure a VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <span data-ttu-id="73add-173"><a name="CreateConnection"></a>7. Skapa hello VPN-anslutning</span><span class="sxs-lookup"><span data-stu-id="73add-173"><a name="CreateConnection"></a>7. Create hello VPN connection</span></span>

<span data-ttu-id="73add-174">Skapa hello plats-till-plats VPN-anslutning mellan din virtuella nätverksgateway och din lokala VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="73add-174">Create hello Site-to-Site VPN connection between your virtual network gateway and your on-premises VPN device.</span></span>

[!INCLUDE [Add connections](../../includes/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include.md)]

## <span data-ttu-id="73add-175"><a name="VerifyConnection"></a>8. Kontrollera hello VPN-anslutning</span><span class="sxs-lookup"><span data-stu-id="73add-175"><a name="VerifyConnection"></a>8. Verify hello VPN connection</span></span>

[!INCLUDE [Verify - Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <span data-ttu-id="73add-176"><a name="connectVM"></a>tooconnect tooa virtuell dator</span><span class="sxs-lookup"><span data-stu-id="73add-176"><a name="connectVM"></a>tooconnect tooa virtual machine</span></span>

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]

## <span data-ttu-id="73add-177"><a name="reset"></a>Hur tooreset en VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="73add-177"><a name="reset"></a>How tooreset a VPN gateway</span></span>

<span data-ttu-id="73add-178">Du kan behöva återställa en Azure VPN-gateway om VPN-anslutningen mellan flera platser i en eller flera VPN-tunnlar för plats-till-plats bryts.</span><span class="sxs-lookup"><span data-stu-id="73add-178">Resetting an Azure VPN gateway is helpful if you lose cross-premises VPN connectivity on one or more Site-to-Site VPN tunnels.</span></span> <span data-ttu-id="73add-179">I så fall kan din lokala VPN-enheter är alla fungerar, men är inte tooestablish IPsec-tunnlar med hello Azure VPN-gatewayer.</span><span class="sxs-lookup"><span data-stu-id="73add-179">In this situation, your on-premises VPN devices are all working correctly, but are not able tooestablish IPsec tunnels with hello Azure VPN gateways.</span></span> <span data-ttu-id="73add-180">Stegvisa anvisningar finns i [Återställa en VPN-gateway](vpn-gateway-resetgw-classic.md).</span><span class="sxs-lookup"><span data-stu-id="73add-180">For steps, see [Reset a VPN gateway](vpn-gateway-resetgw-classic.md).</span></span>

## <span data-ttu-id="73add-181"><a name="resize"></a>Hur toochange en gateway-SKU (ändra storlek på en gateway)</span><span class="sxs-lookup"><span data-stu-id="73add-181"><a name="resize"></a>How toochange a gateway SKU (resize a gateway)</span></span>

<span data-ttu-id="73add-182">Hello stegen toochange en gateway-SKU, finns [Gateway-SKU: er](vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span><span class="sxs-lookup"><span data-stu-id="73add-182">For hello steps toochange a gateway SKU, see [Gateway SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span></span>

## <a name="next-steps"></a><span data-ttu-id="73add-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="73add-183">Next steps</span></span>

* <span data-ttu-id="73add-184">Information om BGP finns hello [BGP översikt](vpn-gateway-bgp-overview.md) och [hur tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="73add-184">For information about BGP, see hello [BGP Overview](vpn-gateway-bgp-overview.md) and [How tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span></span>
* <span data-ttu-id="73add-185">Information om tvingad tunneltrafik finns i [Om forcerade tunnlar](vpn-gateway-forced-tunneling-rm.md).</span><span class="sxs-lookup"><span data-stu-id="73add-185">For information about Forced Tunneling, see [About Forced Tunneling](vpn-gateway-forced-tunneling-rm.md).</span></span>
* <span data-ttu-id="73add-186">Mer information om aktiv-aktiv-anslutningar med hög tillgänglighet finns i [Anslutning med hög tillgänglighet på flera platser och VNet-till-VNet-anslutning](vpn-gateway-highlyavailable.md).</span><span class="sxs-lookup"><span data-stu-id="73add-186">For information about Highly Available Active-Active connections, see [Highly Available cross-premises and VNet-to-VNet connectivity](vpn-gateway-highlyavailable.md).</span></span>
