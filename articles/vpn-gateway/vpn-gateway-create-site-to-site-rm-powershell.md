---
title: "Ansluta din lokala nätverk tooan virtuella Azure-nätverket: plats-till-plats-VPN: PowerShell | Microsoft Docs"
description: "Steg toocreate en IPSec-anslutning från ditt lokala nätverk tooan virtuella Azure-nätverket via hello offentliga Internet. Dessa steg hjälper dig att skapa en plats-till-plats-anslutning med VPN-gateway med hjälp av PowerShell."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fcc2fda5-4493-4c15-9436-84d35adbda8e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: cb8db1dab3a5488816a7f7e8e63908a4c02f55db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vnet-with-a-site-to-site-vpn-connection-using-powershell"></a><span data-ttu-id="05258-104">Skapa ett VNet med en VPN-anslutning för plats-till-plats med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="05258-104">Create a VNet with a Site-to-Site VPN connection using PowerShell</span></span>

<span data-ttu-id="05258-105">Den här artikeln visar hur toouse PowerShell toocreate plats-till-plats VPN-gateway-anslutningen från ditt lokala nätverk toohello VNet.</span><span class="sxs-lookup"><span data-stu-id="05258-105">This article shows you how toouse PowerShell toocreate a Site-to-Site VPN gateway connection from your on-premises network toohello VNet.</span></span> <span data-ttu-id="05258-106">hello stegen i den här artikeln gäller toohello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="05258-106">hello steps in this article apply toohello Resource Manager deployment model.</span></span> <span data-ttu-id="05258-107">Du kan också skapa den här konfigurationen med hjälp av en annan distributionsverktyget eller distributionsmodell genom att välja ett annat alternativ hello följande lista:</span><span class="sxs-lookup"><span data-stu-id="05258-107">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="05258-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="05258-108">Azure portal</span></span>](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="05258-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="05258-109">PowerShell</span></span>](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [<span data-ttu-id="05258-110">CLI</span><span class="sxs-lookup"><span data-stu-id="05258-110">CLI</span></span>](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [<span data-ttu-id="05258-111">Azure Portal (klassisk)</span><span class="sxs-lookup"><span data-stu-id="05258-111">Azure portal (classic)</span></span>](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [<span data-ttu-id="05258-112">Klassisk portal (klassisk)</span><span class="sxs-lookup"><span data-stu-id="05258-112">Classic portal (classic)</span></span>](vpn-gateway-site-to-site-create.md)
> 
>


<span data-ttu-id="05258-113">En plats-till-plats VPN-gateway-anslutningen har använt tooconnect ditt lokala nätverk tooan virtuella Azure-nätverket via en IPsec/IKE (IKEv1 eller IKEv2) VPN-tunnel.</span><span class="sxs-lookup"><span data-stu-id="05258-113">A Site-to-Site VPN gateway connection is used tooconnect your on-premises network tooan Azure virtual network over an IPsec/IKE (IKEv1 or IKEv2) VPN tunnel.</span></span> <span data-ttu-id="05258-114">Den här typen av anslutning kräver en VPN-enhet som finns lokalt som har ett externt Internetriktade offentliga IP-adress som tilldelats tooit.</span><span class="sxs-lookup"><span data-stu-id="05258-114">This type of connection requires a VPN device located on-premises that has an externally facing public IP address assigned tooit.</span></span> <span data-ttu-id="05258-115">Mer information om VPN-gatewayer finns i [Om VPN-gateway](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="05258-115">For more information about VPN gateways, see [About VPN gateway](vpn-gateway-about-vpngateways.md).</span></span>

![Diagram över plats-till-plats-anslutning med VPN-gateway](./media/vpn-gateway-create-site-to-site-rm-powershell/site-to-site-diagram.png)

## <span data-ttu-id="05258-117"><a name="before"></a>Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="05258-117"><a name="before"></a>Before you begin</span></span>

<span data-ttu-id="05258-118">Kontrollera att du har uppfyllt hello följande villkor innan du börjar din konfiguration:</span><span class="sxs-lookup"><span data-stu-id="05258-118">Verify that you have met hello following criteria before beginning your configuration:</span></span>

* <span data-ttu-id="05258-119">Kontrollera att du har en kompatibel VPN-enhet och någon som kan tooconfigure den.</span><span class="sxs-lookup"><span data-stu-id="05258-119">Make sure you have a compatible VPN device and someone who is able tooconfigure it.</span></span> <span data-ttu-id="05258-120">Se [Om VPN-enheter](vpn-gateway-about-vpn-devices.md) för mer information om kompatibla VPN-enheter och enhetskonfiguration.</span><span class="sxs-lookup"><span data-stu-id="05258-120">For more information about compatible VPN devices and device configuration, see [About VPN Devices](vpn-gateway-about-vpn-devices.md).</span></span>
* <span data-ttu-id="05258-121">Kontrollera att du har en extern offentlig IPv4-adress för VPN-enheten.</span><span class="sxs-lookup"><span data-stu-id="05258-121">Verify that you have an externally facing public IPv4 address for your VPN device.</span></span> <span data-ttu-id="05258-122">Den här IP-adressen får inte finnas bakom en NAT.</span><span class="sxs-lookup"><span data-stu-id="05258-122">This IP address cannot be located behind a NAT.</span></span>
* <span data-ttu-id="05258-123">Om du är bekant med hello IP-adressintervall som finns i ditt lokala nätverk konfiguration måste du toocoordinate med någon som kan ge de detaljer du.</span><span class="sxs-lookup"><span data-stu-id="05258-123">If you are unfamiliar with hello IP address ranges located in your on-premises network configuration, you need toocoordinate with someone who can provide those details for you.</span></span> <span data-ttu-id="05258-124">När du skapar den här konfigurationen måste du ange hello IP-adressintervall adressprefix Azure dirigerar tooyour lokal plats.</span><span class="sxs-lookup"><span data-stu-id="05258-124">When you create this configuration, you must specify hello IP address range prefixes that Azure will route tooyour on-premises location.</span></span> <span data-ttu-id="05258-125">Ingen av hello undernät i ditt lokala nätverk kan över lap med hello virtuella undernät som du vill tooconnect till.</span><span class="sxs-lookup"><span data-stu-id="05258-125">None of hello subnets of your on-premises network can over lap with hello virtual network subnets that you want tooconnect to.</span></span>
* <span data-ttu-id="05258-126">Installera hello senaste versionen av hello Azure Resource Manager PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="05258-126">Install hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="05258-127">PowerShell-cmdlets uppdateras ofta och du behöver normalt tooupdate din PowerShell-cmdlets tooget hello senaste funktionen.</span><span class="sxs-lookup"><span data-stu-id="05258-127">PowerShell cmdlets are updated frequently and you will typically need tooupdate your PowerShell cmdlets tooget hello latest feature functionality.</span></span> <span data-ttu-id="05258-128">Om du inte uppdaterar din PowerShell-cmdlet misslyckas hello värden som har angetts.</span><span class="sxs-lookup"><span data-stu-id="05258-128">If you don't update your PowerShell cmdlets, hello values specified may fail.</span></span> <span data-ttu-id="05258-129">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för mer information om hämtning och installation av PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="05258-129">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information about downloading and installing PowerShell cmdlets.</span></span>

### <span data-ttu-id="05258-130"><a name="example"></a>Exempelvärden</span><span class="sxs-lookup"><span data-stu-id="05258-130"><a name="example"></a>Example values</span></span>

<span data-ttu-id="05258-131">hello exemplen i den här artikeln används hello följande värden.</span><span class="sxs-lookup"><span data-stu-id="05258-131">hello examples in this article use hello following values.</span></span> <span data-ttu-id="05258-132">Du kan använda dessa värden toocreate en testmiljö eller referera toothem toobetter förstå hello exemplen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="05258-132">You can use these values toocreate a test environment, or refer toothem toobetter understand hello examples in this article.</span></span>

```
#Example values

VnetName                = TestVNet1
ResourceGroup           = TestRG1
Location                = East US 
AddressSpace            = 10.11.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.11.1.0/28 
GatewaySubnet           = 10.11.0.0/27
LocalNetworkGatewayName = Site2
LNG Public IP           = <VPN device IP address> 
Local Address Prefixes  = 10.0.0.0/24, 20.0.0.0/24
Gateway Name            = VNet1GW
PublicIP                = VNet1GWIP
Gateway IP Config       = gwipconfig1 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = VNet1toSite2

```


## <span data-ttu-id="05258-133"><a name="Login"></a>1. Ansluta tooyour prenumeration</span><span class="sxs-lookup"><span data-stu-id="05258-133"><a name="Login"></a>1. Connect tooyour subscription</span></span>

[!INCLUDE [PowerShell login](../../includes/vpn-gateway-ps-login-include.md)]

## <span data-ttu-id="05258-134"><a name="VNet"></a>2. Skapa ett virtuellt nätverk och ett gateway-undernät.</span><span class="sxs-lookup"><span data-stu-id="05258-134"><a name="VNet"></a>2. Create a virtual network and a gateway subnet</span></span>

<span data-ttu-id="05258-135">Om du inte redan har ett virtuellt nätverk, skapa ett.</span><span class="sxs-lookup"><span data-stu-id="05258-135">If you don't already have a virtual network, create one.</span></span> <span data-ttu-id="05258-136">När du skapar ett virtuellt nätverk, se till att hello adressutrymmen som du anger inte överlappa hello-adressutrymmen som finns på ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="05258-136">When creating a virtual network, make sure that hello address spaces you specify don't overlap any of hello address spaces that you have on your on-premises network.</span></span>

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-include.md)]

[!INCLUDE [No NSG warning](../../includes/vpn-gateway-no-nsg-include.md)]

### <span data-ttu-id="05258-137"><a name="vnet"></a>toocreate ett virtuellt nätverk och ett gateway-undernät</span><span class="sxs-lookup"><span data-stu-id="05258-137"><a name="vnet"></a>toocreate a virtual network and a gateway subnet</span></span>

<span data-ttu-id="05258-138">I det här exemplet skapas ett virtuellt nätverk och ett gateway-undernät.</span><span class="sxs-lookup"><span data-stu-id="05258-138">This example creates a virtual network and a gateway subnet.</span></span> <span data-ttu-id="05258-139">Om du redan har ett virtuellt nätverk som du behöver tooadd ett gateway-undernät för att se [tooadd ett gateway-undernätet tooa virtuellt nätverk du redan har skapat](#gatewaysubnet).</span><span class="sxs-lookup"><span data-stu-id="05258-139">If you already have a virtual network that you need tooadd a gateway subnet to, see [tooadd a gateway subnet tooa virtual network you have already created](#gatewaysubnet).</span></span>

<span data-ttu-id="05258-140">Skapa en resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="05258-140">Create a resource group:</span></span>

```powershell
New-AzureRmResourceGroup -Name TestRG1 -Location 'East US'
```

<span data-ttu-id="05258-141">Skapa ditt virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="05258-141">Create your virtual network.</span></span>

1. <span data-ttu-id="05258-142">Ange hello variabler.</span><span class="sxs-lookup"><span data-stu-id="05258-142">Set hello variables.</span></span>

  ```powershell
  $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.11.0.0/27
  $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix 10.11.1.0/28
  ```
2. <span data-ttu-id="05258-143">Skapa hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="05258-143">Create hello VNet.</span></span>

  ```powershell
  New-AzureRmVirtualNetwork -Name TestVNet1 -ResourceGroupName TestRG1 `
  -Location 'East US' -AddressPrefix 10.11.0.0/16 -Subnet $subnet1, $subnet2
  ```

### <span data-ttu-id="05258-144"><a name="gatewaysubnet"></a>tooadd ett virtuellt nätverk för gateway-undernätet tooa du redan har skapat</span><span class="sxs-lookup"><span data-stu-id="05258-144"><a name="gatewaysubnet"></a>tooadd a gateway subnet tooa virtual network you have already created</span></span>

1. <span data-ttu-id="05258-145">Ange hello variabler.</span><span class="sxs-lookup"><span data-stu-id="05258-145">Set hello variables.</span></span>

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG1 -Name TestVet1
  ```
2. <span data-ttu-id="05258-146">Skapa hello gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="05258-146">Create hello gateway subnet.</span></span>

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.11.0.0/27 -VirtualNetwork $vnet
  ```
3. <span data-ttu-id="05258-147">Ange hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="05258-147">Set hello configuration.</span></span>

  ```powershell
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```

## <span data-ttu-id="05258-148">3. <a name="localnet"></a>Skapa hello lokal nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="05258-148">3. <a name="localnet"></a>Create hello local network gateway</span></span>

<span data-ttu-id="05258-149">hello lokal nätverksgateway refererar vanligtvis tooyour lokal plats.</span><span class="sxs-lookup"><span data-stu-id="05258-149">hello local network gateway typically refers tooyour on-premises location.</span></span> <span data-ttu-id="05258-150">Du namnge hello plats som Azure kan se tooit och sedan ange hello IP-adress för hello lokala VPN-enhet toowhich ska du skapa en anslutning.</span><span class="sxs-lookup"><span data-stu-id="05258-150">You give hello site a name by which Azure can refer tooit, then specify hello IP address of hello on-premises VPN device toowhich you will create a connection.</span></span> <span data-ttu-id="05258-151">Du kan också ange hello IP-adressprefix som vidarebefordras via hello VPN-gateway toohello VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="05258-151">You also specify hello IP address prefixes that will be routed through hello VPN gateway toohello VPN device.</span></span> <span data-ttu-id="05258-152">hello-adressprefix som du anger är hello prefix finns i ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="05258-152">hello address prefixes you specify are hello prefixes located on your on-premises network.</span></span> <span data-ttu-id="05258-153">Om ditt lokala nätverk ändras kan uppdatera du enkelt hello prefix.</span><span class="sxs-lookup"><span data-stu-id="05258-153">If your on-premises network changes, you can easily update hello prefixes.</span></span>

<span data-ttu-id="05258-154">Använd hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="05258-154">Use hello following values:</span></span>

* <span data-ttu-id="05258-155">Hej *GatewayIPAddress* är hello IP-adressen för din lokala VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="05258-155">hello *GatewayIPAddress* is hello IP address of your on-premises VPN device.</span></span> <span data-ttu-id="05258-156">VPN-enheten får inte finnas bakom en NAT.</span><span class="sxs-lookup"><span data-stu-id="05258-156">Your VPN device cannot be located behind a NAT.</span></span>
* <span data-ttu-id="05258-157">Hej *AddressPrefix* är din lokala adressutrymme.</span><span class="sxs-lookup"><span data-stu-id="05258-157">hello *AddressPrefix* is your on-premises address space.</span></span>

<span data-ttu-id="05258-158">tooadd en lokal nätverksgateway med ett enda adressprefix:</span><span class="sxs-lookup"><span data-stu-id="05258-158">tooadd a local network gateway with a single address prefix:</span></span>

  ```powershell
  New-AzureRmLocalNetworkGateway -Name Site2 -ResourceGroupName TestRG1 `
  -Location 'East US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.0.0.0/24'
  ```

<span data-ttu-id="05258-159">tooadd en lokal nätverksgateway med flera adressprefix:</span><span class="sxs-lookup"><span data-stu-id="05258-159">tooadd a local network gateway with multiple address prefixes:</span></span>

  ```powershell
  New-AzureRmLocalNetworkGateway -Name Site2 -ResourceGroupName TestRG1 `
  -Location 'East US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')
  ```

<span data-ttu-id="05258-160">toomodify IP-adressprefix för din lokala nätverksgateway:</span><span class="sxs-lookup"><span data-stu-id="05258-160">toomodify IP address prefixes for your local network gateway:</span></span><br>
<span data-ttu-id="05258-161">Ibland ändras prefixen för din lokala nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="05258-161">Sometimes your local network gateway prefixes change.</span></span> <span data-ttu-id="05258-162">hello steg du vidta toomodify din IP-adress som prefix beror på om du har skapat en VPN-anslutning för gateway.</span><span class="sxs-lookup"><span data-stu-id="05258-162">hello steps you take toomodify your IP address prefixes depend on whether you have created a VPN gateway connection.</span></span> <span data-ttu-id="05258-163">Se hello [ändra IP-adressprefix för en lokal nätverksgateway](#modify) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="05258-163">See hello [Modify IP address prefixes for a local network gateway](#modify) section of this article.</span></span>

## <span data-ttu-id="05258-164"><a name="PublicIP"></a>4. Begär en offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="05258-164"><a name="PublicIP"></a>4. Request a Public IP address</span></span>

<span data-ttu-id="05258-165">En VPN-gateway måste ha en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="05258-165">A VPN gateway must have a Public IP address.</span></span> <span data-ttu-id="05258-166">Du först begära hello IP-adressresurs och läser sedan jobbhistorikposten tooit när du skapar din virtuella nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="05258-166">You first request hello IP address resource, and then refer tooit when creating your virtual network gateway.</span></span> <span data-ttu-id="05258-167">hello IP-adressen tilldelas dynamiskt toohello resurs när hello VPN-gateway har skapats.</span><span class="sxs-lookup"><span data-stu-id="05258-167">hello IP address is dynamically assigned toohello resource when hello VPN gateway is created.</span></span> <span data-ttu-id="05258-168">VPN-gateway stöder för närvarande endast *dynamisk* offentlig IP-adressallokering.</span><span class="sxs-lookup"><span data-stu-id="05258-168">VPN Gateway currently only supports *Dynamic* Public IP address allocation.</span></span> <span data-ttu-id="05258-169">Du kan inte begära en statisk offentlig IP-adresstilldelning.</span><span class="sxs-lookup"><span data-stu-id="05258-169">You cannot request a Static Public IP address assignment.</span></span> <span data-ttu-id="05258-170">Detta innebär dock inte att hello IP-adressen ändras när den har tilldelats tooyour VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="05258-170">However, this does not mean that hello IP address changes after it has been assigned tooyour VPN gateway.</span></span> <span data-ttu-id="05258-171">hello endast tid hello offentliga IP-adressändringarna är när hello gateway bort och återskapas.</span><span class="sxs-lookup"><span data-stu-id="05258-171">hello only time hello Public IP address changes is when hello gateway is deleted and re-created.</span></span> <span data-ttu-id="05258-172">Den ändras inte vid storleksändring, återställning eller annat internt underhåll/uppgraderingar av din VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="05258-172">It doesn't change across resizing, resetting, or other internal maintenance/upgrades of your VPN gateway.</span></span>

<span data-ttu-id="05258-173">Begära en offentlig IP-adress som ska tilldelas tooyour virtuellt nätverk VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="05258-173">Request a Public IP address that will be assigned tooyour virtual network VPN gateway.</span></span>

```powershell
$gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName TestRG1 -Location 'East US' -AllocationMethod Dynamic
```

## <span data-ttu-id="05258-174"><a name="GatewayIPConfig"></a>5. Skapa hello gateway IP-adressering konfiguration</span><span class="sxs-lookup"><span data-stu-id="05258-174"><a name="GatewayIPConfig"></a>5. Create hello gateway IP addressing configuration</span></span>

<span data-ttu-id="05258-175">gateway-konfiguration för hello definierar hello undernät och hello offentliga IP-adressen toouse.</span><span class="sxs-lookup"><span data-stu-id="05258-175">hello gateway configuration defines hello subnet and hello public IP address toouse.</span></span> <span data-ttu-id="05258-176">Använd följande exempel toocreate hello gateway-konfiguration:</span><span class="sxs-lookup"><span data-stu-id="05258-176">Use hello following example toocreate your gateway configuration:</span></span>

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name TestVNet1 -ResourceGroupName TestRG1
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
```

## <span data-ttu-id="05258-177"><a name="CreateGateway"></a>6. Skapa hello VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="05258-177"><a name="CreateGateway"></a>6. Create hello VPN gateway</span></span>

<span data-ttu-id="05258-178">Skapa hello VPN-gateway för virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="05258-178">Create hello virtual network VPN gateway.</span></span> <span data-ttu-id="05258-179">Skapa en VPN-gateway kan ta upp too45 minuter eller mer toocomplete.</span><span class="sxs-lookup"><span data-stu-id="05258-179">Creating a VPN gateway can take up too45 minutes or more toocomplete.</span></span>

<span data-ttu-id="05258-180">Använd hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="05258-180">Use hello following values:</span></span>

* <span data-ttu-id="05258-181">Hej *- GatewayType* för plats-till-plats konfigurationen är *Vpn*.</span><span class="sxs-lookup"><span data-stu-id="05258-181">hello *-GatewayType* for a Site-to-Site configuration is *Vpn*.</span></span> <span data-ttu-id="05258-182">hello gateway-typ är alltid specifika toohello konfiguration som du implementerar.</span><span class="sxs-lookup"><span data-stu-id="05258-182">hello gateway type is always specific toohello configuration that you are implementing.</span></span> <span data-ttu-id="05258-183">Andra gateway-konfigurationer kan kräva -GatewayType ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="05258-183">For example, other gateway configurations may require -GatewayType ExpressRoute.</span></span>
* <span data-ttu-id="05258-184">Hej *- VpnType* kan vara *RouteBased* (kallas tooas en dynamisk Gateway i viss dokumentation) eller *PolicyBased* (kallas tooas en statisk Gateway i viss dokumentation ).</span><span class="sxs-lookup"><span data-stu-id="05258-184">hello *-VpnType* can be *RouteBased* (referred tooas a Dynamic Gateway in some documentation), or *PolicyBased* (referred tooas a Static Gateway in some documentation).</span></span> <span data-ttu-id="05258-185">Mer information om VPN-gatewaytyper finns i [Om VPN-gateway](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="05258-185">For more information about VPN gateway types, see [About VPN Gateway](vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="05258-186">Välj hello Gateway-SKU som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="05258-186">Select hello Gateway SKU that you want toouse.</span></span> <span data-ttu-id="05258-187">Det finns konfigurationsbegränsningar för vissa SKU:er.</span><span class="sxs-lookup"><span data-stu-id="05258-187">There are configuration limitations for certain SKUs.</span></span> <span data-ttu-id="05258-188">Se [Gateway SKU:er](vpn-gateway-about-vpn-gateway-settings.md#gwsku) för mer information.</span><span class="sxs-lookup"><span data-stu-id="05258-188">For more information, see [Gateway SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span></span> <span data-ttu-id="05258-189">Om du får ett felmeddelande när du skapar hello VPN-gateway om hello kontrollerar - GatewaySku, du att du har installerat hello senaste versionen av hello PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="05258-189">If you get an error when creating hello VPN gateway regarding hello -GatewaySku, verify that you have installed hello latest version of hello PowerShell cmdlets.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1 `
-Location 'East US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased -GatewaySku VpnGw1
```

## <span data-ttu-id="05258-190"><a name="ConfigureVPNDevice"></a>7. Konfigurera din VPN-enhet</span><span class="sxs-lookup"><span data-stu-id="05258-190"><a name="ConfigureVPNDevice"></a>7. Configure your VPN device</span></span>

<span data-ttu-id="05258-191">Plats-till-platsanslutningar tooan lokalt nätverk kräver en VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="05258-191">Site-to-Site connections tooan on-premises network require a VPN device.</span></span> <span data-ttu-id="05258-192">I det här steget konfigurerar du VPN-enheten.</span><span class="sxs-lookup"><span data-stu-id="05258-192">In this step, you configure your VPN device.</span></span> <span data-ttu-id="05258-193">När du konfigurerar VPN-enhet behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="05258-193">When configuring your VPN device, you need hello following:</span></span>

- <span data-ttu-id="05258-194">En delad nyckel.</span><span class="sxs-lookup"><span data-stu-id="05258-194">A shared key.</span></span> <span data-ttu-id="05258-195">Detta är hello samma delade nyckel som du anger när du skapar din plats-till-plats VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="05258-195">This is hello same shared key that you specify when creating your Site-to-Site VPN connection.</span></span> <span data-ttu-id="05258-196">I vårt exempel använder vi en enkel delad nyckel.</span><span class="sxs-lookup"><span data-stu-id="05258-196">In our examples, we use a basic shared key.</span></span> <span data-ttu-id="05258-197">Vi rekommenderar att du generera en mer komplex viktiga toouse.</span><span class="sxs-lookup"><span data-stu-id="05258-197">We recommend that you generate a more complex key toouse.</span></span>
- <span data-ttu-id="05258-198">hello offentliga IP-adressen för din virtuella nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="05258-198">hello Public IP address of your virtual network gateway.</span></span> <span data-ttu-id="05258-199">Du kan visa hello offentliga IP-adressen med hjälp av hello Azure-portalen, PowerShell eller CLI.</span><span class="sxs-lookup"><span data-stu-id="05258-199">You can view hello public IP address by using hello Azure portal, PowerShell, or CLI.</span></span> <span data-ttu-id="05258-200">toofind hello offentliga IP-adressen för din virtuella nätverksgateway med hjälp av PowerShell, Använd hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="05258-200">toofind hello Public IP address of your virtual network gateway using PowerShell, use hello following example:</span></span>

  ```powershell
  Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG1
  ```

[!INCLUDE [Configure VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]


## <span data-ttu-id="05258-201"><a name="CreateConnection"></a>8. Skapa hello VPN-anslutning</span><span class="sxs-lookup"><span data-stu-id="05258-201"><a name="CreateConnection"></a>8. Create hello VPN connection</span></span>

<span data-ttu-id="05258-202">Skapa sedan hello plats-till-plats VPN-anslutning mellan din virtuella nätverksgateway och VPN-enheten.</span><span class="sxs-lookup"><span data-stu-id="05258-202">Next, create hello Site-to-Site VPN connection between your virtual network gateway and your VPN device.</span></span> <span data-ttu-id="05258-203">Vara säker på att tooreplace hello värden med dina egna.</span><span class="sxs-lookup"><span data-stu-id="05258-203">Be sure tooreplace hello values with your own.</span></span> <span data-ttu-id="05258-204">hello delad nyckel måste matcha hello-värde som du använde för din konfiguration för VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="05258-204">hello shared key must match hello value you used for your VPN device configuration.</span></span> <span data-ttu-id="05258-205">Observera att hello '-ConnectionType' för plats-till-plats är *IPsec*.</span><span class="sxs-lookup"><span data-stu-id="05258-205">Notice that hello '-ConnectionType' for Site-to-Site is *IPsec*.</span></span>

1. <span data-ttu-id="05258-206">Ange hello variabler.</span><span class="sxs-lookup"><span data-stu-id="05258-206">Set hello variables.</span></span>
  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1
  $local = Get-AzureRmLocalNetworkGateway -Name Site2 -ResourceGroupName TestRG1
  ```

2. <span data-ttu-id="05258-207">Skapa hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="05258-207">Create hello connection.</span></span>
  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name VNet1toSite2 -ResourceGroupName TestRG1 `
  -Location 'East US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```

<span data-ttu-id="05258-208">Efter en kort när hello upprättas anslutning.</span><span class="sxs-lookup"><span data-stu-id="05258-208">After a short while, hello connection will be established.</span></span>

## <span data-ttu-id="05258-209"><a name="toverify"></a>9. Kontrollera hello VPN-anslutning</span><span class="sxs-lookup"><span data-stu-id="05258-209"><a name="toverify"></a>9. Verify hello VPN connection</span></span>

<span data-ttu-id="05258-210">Det finns några olika sätt tooverify VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="05258-210">There are a few different ways tooverify your VPN connection.</span></span>

[!INCLUDE [Verify connection](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <span data-ttu-id="05258-211"><a name="connectVM"></a>tooconnect tooa virtuell dator</span><span class="sxs-lookup"><span data-stu-id="05258-211"><a name="connectVM"></a>tooconnect tooa virtual machine</span></span>

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]


## <span data-ttu-id="05258-212"><a name="modify"></a>Ändra IP-adressprefixen för en lokal nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="05258-212"><a name="modify"></a>Modify IP address prefixes for a local network gateway</span></span>

<span data-ttu-id="05258-213">Ändrar hello IP-adressprefix som du vill att dirigeras tooyour lokal plats, kan du ändra hello lokal nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="05258-213">If hello IP address prefixes that you want routed tooyour on-premises location change, you can modify hello local network gateway.</span></span> <span data-ttu-id="05258-214">Det finns två uppsättningar med instruktioner.</span><span class="sxs-lookup"><span data-stu-id="05258-214">Two sets of instructions are provided.</span></span> <span data-ttu-id="05258-215">hello-instruktioner som du väljer beror på om du redan har skapat din gateway-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="05258-215">hello instructions you choose depend on whether you have already created your gateway connection.</span></span>

[!INCLUDE [Modify prefixes](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <span data-ttu-id="05258-216"><a name="modifygwipaddress"></a>Ändra hello gateway IP-adressen för en lokal nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="05258-216"><a name="modifygwipaddress"></a>Modify hello gateway IP address for a local network gateway</span></span>

[!INCLUDE [Modify gateway IP address](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a><span data-ttu-id="05258-217">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="05258-217">Next steps</span></span>

*  <span data-ttu-id="05258-218">När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="05258-218">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="05258-219">Mer information finns i [Virtuella datorer](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span><span class="sxs-lookup"><span data-stu-id="05258-219">For more information, see [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span></span>
* <span data-ttu-id="05258-220">Information om BGP finns hello [BGP översikt](vpn-gateway-bgp-overview.md) och [hur tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="05258-220">For information about BGP, see hello [BGP Overview](vpn-gateway-bgp-overview.md) and [How tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span></span>
