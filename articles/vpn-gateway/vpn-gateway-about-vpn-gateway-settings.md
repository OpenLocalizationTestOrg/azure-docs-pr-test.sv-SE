---
title: "aaaVPN gateway inställningar för Azure-anslutningar mellan lokala | Microsoft Docs"
description: "Lär dig mer om inställningar för VPN-Gateway för Azure virtuella nätverksgatewayer."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: ae665bc5-0089-45d0-a0d5-bc0ab4e79899
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/26/2017
ms.author: cherylmc
ms.openlocfilehash: 01229d99fa37e30e00aa00f939f488d631b5593c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="about-vpn-gateway-configuration-settings"></a><span data-ttu-id="023ca-103">Om konfigurationsinställningar för VPN-Gateway</span><span class="sxs-lookup"><span data-stu-id="023ca-103">About VPN Gateway configuration settings</span></span>

<span data-ttu-id="023ca-104">En VPN-gateway är en typ av virtuell nätverksgateway som skickar krypterad trafik mellan det virtuella nätverket och den lokala platsen i en offentlig anslutning.</span><span class="sxs-lookup"><span data-stu-id="023ca-104">A VPN gateway is a type of virtual network gateway that sends encrypted traffic between your virtual network and your on-premises location across a public connection.</span></span> <span data-ttu-id="023ca-105">Du kan också använda en VPN-gateway toosend trafik mellan virtuella nätverk över hello Azure stamnät.</span><span class="sxs-lookup"><span data-stu-id="023ca-105">You can also use a VPN gateway toosend traffic between virtual networks across hello Azure backbone.</span></span>

<span data-ttu-id="023ca-106">En VPN-gateway-anslutningen förlitar sig på hello konfiguration av flera resurser som innehåller konfigurerbara inställningar.</span><span class="sxs-lookup"><span data-stu-id="023ca-106">A VPN gateway connection relies on hello configuration of multiple resources, each of which contains configurable settings.</span></span> <span data-ttu-id="023ca-107">hello-avsnitt i den här artikeln beskrivs hello resurser och inställningar som gäller tooa VPN-gateway för ett virtuellt nätverk som skapats i Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="023ca-107">hello sections in this article discuss hello resources and settings that relate tooa VPN gateway for a virtual network created in Resource Manager deployment model.</span></span> <span data-ttu-id="023ca-108">Du kan hitta beskrivningar och Topologidiagram för varje anslutning lösning i hello [om VPN-Gateway](vpn-gateway-about-vpngateways.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="023ca-108">You can find descriptions and topology diagrams for each connection solution in hello [About VPN Gateway](vpn-gateway-about-vpngateways.md) article.</span></span>

## <span data-ttu-id="023ca-109"><a name="gwtype"></a>Gateway-typer</span><span class="sxs-lookup"><span data-stu-id="023ca-109"><a name="gwtype"></a>Gateway types</span></span>

<span data-ttu-id="023ca-110">Varje virtuellt nätverk kan bara ha en virtuell nätverksgateway för varje typ.</span><span class="sxs-lookup"><span data-stu-id="023ca-110">Each virtual network can only have one virtual network gateway of each type.</span></span> <span data-ttu-id="023ca-111">När du skapar en virtuell nätverksgateway, måste du se till att hello gateway-typen är korrekta för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="023ca-111">When you are creating a virtual network gateway, you must make sure that hello gateway type is correct for your configuration.</span></span>

<span data-ttu-id="023ca-112">hello tillgängliga värden för - GatewayType är:</span><span class="sxs-lookup"><span data-stu-id="023ca-112">hello available values for -GatewayType are:</span></span>

* <span data-ttu-id="023ca-113">Vpn</span><span class="sxs-lookup"><span data-stu-id="023ca-113">Vpn</span></span>
* <span data-ttu-id="023ca-114">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="023ca-114">ExpressRoute</span></span>

<span data-ttu-id="023ca-115">En VPN-gateway kräver hello `-GatewayType` *Vpn*.</span><span class="sxs-lookup"><span data-stu-id="023ca-115">A VPN gateway requires hello `-GatewayType` *Vpn*.</span></span>

<span data-ttu-id="023ca-116">Exempel:</span><span class="sxs-lookup"><span data-stu-id="023ca-116">Example:</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased
```

## <span data-ttu-id="023ca-117"><a name="gwsku"></a>Gateway-SKU:er</span><span class="sxs-lookup"><span data-stu-id="023ca-117"><a name="gwsku"></a>Gateway SKUs</span></span>

[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configure-hello-gateway-sku"></a><span data-ttu-id="023ca-118">Konfigurera hello gateway SKU</span><span class="sxs-lookup"><span data-stu-id="023ca-118">Configure hello gateway SKU</span></span>

#### <a name="azure-portal"></a><span data-ttu-id="023ca-119">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="023ca-119">Azure portal</span></span>

<span data-ttu-id="023ca-120">Om du använder hello Azure portal toocreate en virtuell nätverksgateway för hanteraren för filserverresurser, kan du välja hello gateway SKU med hjälp av hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="023ca-120">If you use hello Azure portal toocreate a Resource Manager virtual network gateway, you can select hello gateway SKU by using hello dropdown.</span></span> <span data-ttu-id="023ca-121">hello-alternativ som visas är motsvara toohello Gateway-typ och VPN-typ som du väljer.</span><span class="sxs-lookup"><span data-stu-id="023ca-121">hello options you are presented with correspond toohello Gateway type and VPN type that you select.</span></span>

#### <a name="powershell"></a><span data-ttu-id="023ca-122">PowerShell</span><span class="sxs-lookup"><span data-stu-id="023ca-122">PowerShell</span></span>

<span data-ttu-id="023ca-123">hello följande PowerShell-exempel anger hello `-GatewaySku` som VpnGw1.</span><span class="sxs-lookup"><span data-stu-id="023ca-123">hello following PowerShell example specifies hello `-GatewaySku` as VpnGw1.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku VpnGw1 `
-GatewayType Vpn -VpnType RouteBased
```

#### <span data-ttu-id="023ca-124"><a name="resize"></a>Ändra (storleksändringar) för en gateway-SKU</span><span class="sxs-lookup"><span data-stu-id="023ca-124"><a name="resize"></a>Change (resize) a gateway SKU</span></span>

<span data-ttu-id="023ca-125">Om du vill tooupgrade tooa din gateway-SKU kraftigare SKU som du kan använda hello `Resize-AzureRmVirtualNetworkGateway` PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="023ca-125">If you want tooupgrade your gateway SKU tooa more powerful SKU, you can use hello `Resize-AzureRmVirtualNetworkGateway` PowerShell cmdlet.</span></span> <span data-ttu-id="023ca-126">Du kan också nedgradera hello gateway SKU-storleken som använder denna cmdlet.</span><span class="sxs-lookup"><span data-stu-id="023ca-126">You can also downgrade hello gateway SKU size using this cmdlet.</span></span>

<span data-ttu-id="023ca-127">hello följande PowerShell-exempel visar en gateway-SKU som storleksändras tooVpnGw2.</span><span class="sxs-lookup"><span data-stu-id="023ca-127">hello following PowerShell example shows a gateway SKU being resized tooVpnGw2.</span></span>

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

## <span data-ttu-id="023ca-128"><a name="connectiontype"></a>Anslutningstyper</span><span class="sxs-lookup"><span data-stu-id="023ca-128"><a name="connectiontype"></a>Connection types</span></span>

<span data-ttu-id="023ca-129">I hello Resource Manager-distributionsmodellen kräver varje konfiguration av en specifik virtuell gatewaytyp av nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="023ca-129">In hello Resource Manager deployment model, each configuration requires a specific virtual network gateway connection type.</span></span> <span data-ttu-id="023ca-130">hello tillgängliga Resource Manager PowerShell värden för `-ConnectionType` är:</span><span class="sxs-lookup"><span data-stu-id="023ca-130">hello available Resource Manager PowerShell values for `-ConnectionType` are:</span></span>

* <span data-ttu-id="023ca-131">IPsec</span><span class="sxs-lookup"><span data-stu-id="023ca-131">IPsec</span></span>
* <span data-ttu-id="023ca-132">Vnet2Vnet</span><span class="sxs-lookup"><span data-stu-id="023ca-132">Vnet2Vnet</span></span>
* <span data-ttu-id="023ca-133">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="023ca-133">ExpressRoute</span></span>
* <span data-ttu-id="023ca-134">VPNClient</span><span class="sxs-lookup"><span data-stu-id="023ca-134">VPNClient</span></span>

<span data-ttu-id="023ca-135">I följande exempel PowerShell hello, skapar vi en S2S-anslutning som kräver hello anslutningstypen *IPsec*.</span><span class="sxs-lookup"><span data-stu-id="023ca-135">In hello following PowerShell example, we create a S2S connection that requires hello connection type *IPsec*.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

## <span data-ttu-id="023ca-136"><a name="vpntype"></a>VPN-typer</span><span class="sxs-lookup"><span data-stu-id="023ca-136"><a name="vpntype"></a>VPN types</span></span>

<span data-ttu-id="023ca-137">Du måste ange en VPN-typ när du skapar hello virtuell nätverksgateway för en konfiguration för VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="023ca-137">When you create hello virtual network gateway for a VPN gateway configuration, you must specify a VPN type.</span></span> <span data-ttu-id="023ca-138">hello VPN-typ som du väljer beror på topologin för hello-anslutning som du vill toocreate.</span><span class="sxs-lookup"><span data-stu-id="023ca-138">hello VPN type that you choose depends on hello connection topology that you want toocreate.</span></span> <span data-ttu-id="023ca-139">Till exempel kräver en P2S-anslutning en RouteBased VPN-typ.</span><span class="sxs-lookup"><span data-stu-id="023ca-139">For example, a P2S connection requires a RouteBased VPN type.</span></span> <span data-ttu-id="023ca-140">En VPN-typ kan också bero på hello maskinvara som du använder.</span><span class="sxs-lookup"><span data-stu-id="023ca-140">A VPN type can also depend on hello hardware that you are using.</span></span> <span data-ttu-id="023ca-141">S2S konfigurationer kräver en VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="023ca-141">S2S configurations require a VPN device.</span></span> <span data-ttu-id="023ca-142">Vissa VPN-enheter stöder bara en viss typ av VPN.</span><span class="sxs-lookup"><span data-stu-id="023ca-142">Some VPN devices only support a certain VPN type.</span></span>

<span data-ttu-id="023ca-143">hello VPN-typ du väljer måste uppfylla alla hello anslutning för hello lösning du vill toocreate.</span><span class="sxs-lookup"><span data-stu-id="023ca-143">hello VPN type you select must satisfy all hello connection requirements for hello solution you want toocreate.</span></span> <span data-ttu-id="023ca-144">Till exempel om du vill toocreate en S2S VPN-anslutning för gateway och en P2S VPN gateway-anslutning för hello samma virtuella nätverk, kan du använda VPN-typ *RouteBased* eftersom P2S kräver en RouteBased VPN-typ.</span><span class="sxs-lookup"><span data-stu-id="023ca-144">For example, if you want toocreate a S2S VPN gateway connection and a P2S VPN gateway connection for hello same virtual network, you would use VPN type *RouteBased* because P2S requires a RouteBased VPN type.</span></span> <span data-ttu-id="023ca-145">Du måste också tooverify att VPN-enheten stöds en RouteBased VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="023ca-145">You would also need tooverify that your VPN device supported a RouteBased VPN connection.</span></span> 

<span data-ttu-id="023ca-146">När en virtuell nätverksgateway har skapats kan ändra du inte hello VPN-typ.</span><span class="sxs-lookup"><span data-stu-id="023ca-146">Once a virtual network gateway has been created, you can't change hello VPN type.</span></span> <span data-ttu-id="023ca-147">Du har toodelete hello virtuell nätverksgateway och skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="023ca-147">You have toodelete hello virtual network gateway and create a new one.</span></span> <span data-ttu-id="023ca-148">Det finns två typer av VPN:</span><span class="sxs-lookup"><span data-stu-id="023ca-148">There are two VPN types:</span></span>

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

<span data-ttu-id="023ca-149">hello följande PowerShell-exempel anger hello `-VpnType` som *RouteBased*.</span><span class="sxs-lookup"><span data-stu-id="023ca-149">hello following PowerShell example specifies hello `-VpnType` as *RouteBased*.</span></span> <span data-ttu-id="023ca-150">När du skapar en gateway, måste du se till att hello - VpnType stämmer för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="023ca-150">When you are creating a gateway, you must make sure that hello -VpnType is correct for your configuration.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig `
-GatewayType Vpn -VpnType RouteBased
```

## <span data-ttu-id="023ca-151"><a name="requirements"></a>Krav för gateway</span><span class="sxs-lookup"><span data-stu-id="023ca-151"><a name="requirements"></a>Gateway requirements</span></span>

[!INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)]

## <span data-ttu-id="023ca-152"><a name="gwsub"></a>Gateway-undernät</span><span class="sxs-lookup"><span data-stu-id="023ca-152"><a name="gwsub"></a>Gateway subnet</span></span>

<span data-ttu-id="023ca-153">Innan du skapar en VPN-gateway, måste du skapa en gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="023ca-153">Before you create a VPN gateway, you must create a gateway subnet.</span></span> <span data-ttu-id="023ca-154">hello gateway-undernätet innehåller hello IP-adresser som hello virtuella nätverkets gateway virtuella datorer och tjänster används.</span><span class="sxs-lookup"><span data-stu-id="023ca-154">hello gateway subnet contains hello IP addresses that hello virtual network gateway VMs and services use.</span></span> <span data-ttu-id="023ca-155">När du skapar din virtuella nätverksgateway gateway VMs är distribuerade toohello gateway-undernät och konfigurerad med hello krävs VPN gateway-inställningar.</span><span class="sxs-lookup"><span data-stu-id="023ca-155">When you create your virtual network gateway, gateway VMs are deployed toohello gateway subnet and configured with hello required VPN gateway settings.</span></span> <span data-ttu-id="023ca-156">Aldrig måste du distribuera stor (till exempel ytterligare virtuella datorer) toohello gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="023ca-156">You must never deploy anything else (for example, additional VMs) toohello gateway subnet.</span></span> <span data-ttu-id="023ca-157">hello gateway-undernätet måste ha namnet ”GatewaySubnet' toowork korrekt.</span><span class="sxs-lookup"><span data-stu-id="023ca-157">hello gateway subnet must be named 'GatewaySubnet' toowork properly.</span></span> <span data-ttu-id="023ca-158">Namnge hello gatewayundernät 'GatewaySubnet' kan Azure vet att det är hello undernät toodeploy hello virtuell nätverksgateway virtuella datorer och tjänster.</span><span class="sxs-lookup"><span data-stu-id="023ca-158">Naming hello gateway subnet 'GatewaySubnet' lets Azure know that this is hello subnet toodeploy hello virtual network gateway VMs and services to.</span></span>

<span data-ttu-id="023ca-159">När du skapar hello gateway-undernät kan ange du hello antalet IP-adresser som hello undernät innehåller.</span><span class="sxs-lookup"><span data-stu-id="023ca-159">When you create hello gateway subnet, you specify hello number of IP addresses that hello subnet contains.</span></span> <span data-ttu-id="023ca-160">hello IP-adresser i hello gateway-undernät är allokerade toohello gateway VMs och gateway-tjänster.</span><span class="sxs-lookup"><span data-stu-id="023ca-160">hello IP addresses in hello gateway subnet are allocated toohello gateway VMs and gateway services.</span></span> <span data-ttu-id="023ca-161">Vissa konfigurationer kräver flera IP-adresser än andra.</span><span class="sxs-lookup"><span data-stu-id="023ca-161">Some configurations require more IP addresses than others.</span></span> <span data-ttu-id="023ca-162">Titta på hello instruktioner för konfiguration av hello som du vill använda toocreate och kontrollera att hello gateway-undernätet som du vill toocreate uppfyller dessa krav.</span><span class="sxs-lookup"><span data-stu-id="023ca-162">Look at hello instructions for hello configuration that you want toocreate and verify that hello gateway subnet you want toocreate meets those requirements.</span></span> <span data-ttu-id="023ca-163">Dessutom kanske du vill att din gateway-undernätet innehåller tillräckligt med IP-adresser tooaccommodate möjliga framtida ytterligare konfigurationer toomake.</span><span class="sxs-lookup"><span data-stu-id="023ca-163">Additionally, you may want toomake sure your gateway subnet contains enough IP addresses tooaccommodate possible future additional configurations.</span></span> <span data-ttu-id="023ca-164">Du kan skapa en gateway-undernätet så liten som /29, rekommenderar vi att du skapar ett gateway-undernät för /28 eller större (/ 28 minst/27, /26 osv.).</span><span class="sxs-lookup"><span data-stu-id="023ca-164">While you can create a gateway subnet as small as /29, we recommend that you create a gateway subnet of /28 or larger (/28, /27, /26 etc.).</span></span> <span data-ttu-id="023ca-165">På så sätt kan om du lägger till funktioner i hello framtida du inte har tootear din gateway och sedan ta bort och återskapa hello gateway-undernätet tooallow för flera IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="023ca-165">That way, if you add functionality in hello future, you won't have tootear your gateway, then delete and recreate hello gateway subnet tooallow for more IP addresses.</span></span>

<span data-ttu-id="023ca-166">hello visas följande Resource Manager PowerShell-exempel ett gateway-undernät med namnet GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="023ca-166">hello following Resource Manager PowerShell example shows a gateway subnet named GatewaySubnet.</span></span> <span data-ttu-id="023ca-167">Du kan se hello CIDR-notering anger en minst/27, vilket tillåter tillräckligt med IP-adresser för de flesta konfigurationer som finns för närvarande.</span><span class="sxs-lookup"><span data-stu-id="023ca-167">You can see hello CIDR notation specifies a /27, which allows for enough IP addresses for most configurations that currently exist.</span></span>

```powershell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <span data-ttu-id="023ca-168"><a name="lng"></a>Lokala nätverksgatewayer</span><span class="sxs-lookup"><span data-stu-id="023ca-168"><a name="lng"></a>Local network gateways</span></span>

<span data-ttu-id="023ca-169">När du skapar en VPN-gateway-konfiguration, representerar hello lokal nätverksgateway ofta den lokala platsen.</span><span class="sxs-lookup"><span data-stu-id="023ca-169">When creating a VPN gateway configuration, hello local network gateway often represents your on-premises location.</span></span> <span data-ttu-id="023ca-170">Hello klassiska distributionsmodellen var hello lokal nätverksgateway som anges tooas en lokal plats.</span><span class="sxs-lookup"><span data-stu-id="023ca-170">In hello classic deployment model, hello local network gateway was referred tooas a Local Site.</span></span> 

<span data-ttu-id="023ca-171">Namnge hello lokal nätverksgateway, hello offentliga IP-adressen för hello lokala VPN-enhet och ange hello adressprefix som finns på hello lokal plats.</span><span class="sxs-lookup"><span data-stu-id="023ca-171">You give hello local network gateway a name, hello public IP address of hello on-premises VPN device, and specify hello address prefixes that are located on hello on-premises location.</span></span> <span data-ttu-id="023ca-172">Azure tittar på hello mål-adressprefix för nätverkstrafik, tittar hello-konfiguration som du har angett för din lokala nätverksgateway och därefter dirigerar paket.</span><span class="sxs-lookup"><span data-stu-id="023ca-172">Azure looks at hello destination address prefixes for network traffic, consults hello configuration that you have specified for your local network gateway, and routes packets accordingly.</span></span> <span data-ttu-id="023ca-173">Du kan också ange lokala nätverksgatewayer för VNet-till-VNet-konfigurationer som använder en VPN-anslutning för gateway.</span><span class="sxs-lookup"><span data-stu-id="023ca-173">You also specify local network gateways for VNet-to-VNet configurations that use a VPN gateway connection.</span></span>

<span data-ttu-id="023ca-174">hello följande PowerShell-exempel skapar en ny lokal nätverksgateway:</span><span class="sxs-lookup"><span data-stu-id="023ca-174">hello following PowerShell example creates a new local network gateway:</span></span>

```powershell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```

<span data-ttu-id="023ca-175">Ibland behöver toomodify hello lokala gateway nätverksinställningar.</span><span class="sxs-lookup"><span data-stu-id="023ca-175">Sometimes you need toomodify hello local network gateway settings.</span></span> <span data-ttu-id="023ca-176">Till exempel när du lägger till eller ändra hello adressintervall, eller om hello IP-adress för hello VPN-enhet ändras.</span><span class="sxs-lookup"><span data-stu-id="023ca-176">For example, when you add or modify hello address range, or if hello IP address of hello VPN device changes.</span></span> <span data-ttu-id="023ca-177">Du kan ändra de här inställningarna i hello klassiska portalen på sidan för hello lokala nätverk för ett klassiska VNet.</span><span class="sxs-lookup"><span data-stu-id="023ca-177">For a classic VNet, you can change these settings in hello classic portal on hello Local Networks page.</span></span> <span data-ttu-id="023ca-178">För hanteraren för filserverresurser, se [ändra lokala gateway nätverksinställningar med hjälp av PowerShell](vpn-gateway-modify-local-network-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="023ca-178">For Resource Manager, see [Modify local network gateway settings using PowerShell](vpn-gateway-modify-local-network-gateway.md).</span></span>

## <span data-ttu-id="023ca-179"><a name="resources"></a>REST API: er och PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="023ca-179"><a name="resources"></a>REST APIs and PowerShell cmdlets</span></span>

<span data-ttu-id="023ca-180">Ytterligare tekniska resurser och särskild syntax krav för användning av REST API: er, PowerShell-cmdlets eller Azure CLI för VPN-Gateway-konfigurationer finns hello följande sidor:</span><span class="sxs-lookup"><span data-stu-id="023ca-180">For additional technical resources and specific syntax requirements when using REST APIs, PowerShell cmdlets, or Azure CLI for VPN Gateway configurations, see hello following pages:</span></span>

| <span data-ttu-id="023ca-181">**Klassisk**</span><span class="sxs-lookup"><span data-stu-id="023ca-181">**Classic**</span></span> | <span data-ttu-id="023ca-182">**Resource Manager**</span><span class="sxs-lookup"><span data-stu-id="023ca-182">**Resource Manager**</span></span> |
| --- | --- |
| [<span data-ttu-id="023ca-183">PowerShell</span><span class="sxs-lookup"><span data-stu-id="023ca-183">PowerShell</span></span>](/powershell/module/azure#networking) |[<span data-ttu-id="023ca-184">PowerShell</span><span class="sxs-lookup"><span data-stu-id="023ca-184">PowerShell</span></span>](/powershell/module/azurerm.network#vpn) |
| [<span data-ttu-id="023ca-185">REST-API</span><span class="sxs-lookup"><span data-stu-id="023ca-185">REST API</span></span>](https://msdn.microsoft.com/library/jj154113) |[<span data-ttu-id="023ca-186">REST-API</span><span class="sxs-lookup"><span data-stu-id="023ca-186">REST API</span></span>](/rest/api/network/virtualnetworkgateways) |
| <span data-ttu-id="023ca-187">Stöds inte</span><span class="sxs-lookup"><span data-stu-id="023ca-187">Not supported</span></span> | [<span data-ttu-id="023ca-188">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="023ca-188">Azure CLI</span></span>](/cli/azure/network/vnet-gateway)|

## <a name="next-steps"></a><span data-ttu-id="023ca-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="023ca-189">Next steps</span></span>

<span data-ttu-id="023ca-190">Mer information om tillgänglig anslutning konfigurationer finns [om VPN-Gateway](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="023ca-190">For more information about available connection configurations, see [About VPN Gateway](vpn-gateway-about-vpngateways.md).</span></span>